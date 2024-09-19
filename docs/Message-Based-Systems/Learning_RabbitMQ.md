# Learning RabbitMQ

## Introduction
[RabbitMQ](https://www.rabbitmq.com/) is a free and open source technology you can use in message-based systems as a message-broker solution to connect software systems, integrating multiple applications and services.  

Top three message brokers are: 

* ActiveMQ 
* AWS Simple Queue Service by Amazon
* Azure Service Bus

Advantages versus **RPC** (Remote Procedure Call)

* The sender no longer needs to know where the receiver is located. 
* There can even be multiple receivers without the sender needing to know. 
* The message broker can also duplicate the messages and you can easily add new receivers without changing the sender. 
* Queuing messages. If processing is slow the messages will just wait to be picked up. If the receiving service is down messages won't be lost. 
* Highly Available Queues will duplicate messages across nodes. When a node goes down, the messages will be delivered from another node.

Consequences:

* Asynchronous communication
* The message broker is now a crucial component of your application
* It can increase network traffic

**AMQP** (Advance Message Queue Protocol): A publisher sends a message to an exchange. The binding determines if a message is routed to a queue. A consumer who is subscribed to a queue will receive the message from the queue.

![](img/amqp.PNG)

## Exchange types

* **Direct**: route messages to the bound queue with the same routing key. Usefull for simple scenarios without too many exchanges bindings and queues
![](img/exchange_direct.PNG)

* **Fanout**: route messages to all bound queues and the routing key is just ignored. Usefull when messages have to be sent to all connected services.
![](img/exchange_fanout.PNG)

* **Topic**: route messages to bound queues based on patterns in the routing key. Usefull for complex scenarios when consumers want to filter out certain messages.
![](img/exchange_topic.PNG)

* **Header**: route messages to bound queues based on patterns in the headers (x-match:all/any). The routing key is just ignored. The headers don't have to be strings. They can be an integer or a dictionary for example. This exchange is only recommended if you really need the special filtering mechanisms 
![](img/exchange_headers.PNG)

!!!important " RabbitMQ will load balance messages between consumers, not queues. If the two applications are consuming the same queue, they will receive the messages one after the other"

## Some properties

* **Auto-delete** means that a queue will be deleted when the last client disconnects. 
* **Durability** means that a queue or exchange survives a broker restart. 

## RabbitMQ Installation
 RabbitMQ runs on any platform available:

 * On premise with Linux, Windows, or Mac servers
 * In the cloud with Amazon, Azure, Cloud Foundry, Google Cloud or [CloudAMQP](https://www.cloudamqp.com/)
 * Other optiosn are Docker Images, Helm Charts, Chef Cookbooks, and Puppet Modules

## Publish a message
```cs
using RabbitMQ.Client;

/*connection to the RabbitMQ instance*/
var factory = new ConnectionFactory();
factory.Uri = new Uri("amqp://guest:guest@localhost:5672");
var connection = factory.CreateConnection();
var channel = connection.CreateModel();

/*define the exchange to publish our messages to. This way I'm always sure that the exchange is present.*/
channel.ExchangeDeclare("webappExchange", ExchangeType.Fanout, true);
/*channel.ExchangeDeclare("webappExchange", ExchangeType.Direct, true);
  channel.ExchangeDeclare("webappExchange", ExchangeType.Topic, true);
  channel.ExchangeDeclare("webappExchange", ExchangeType.Headers, true);*/

/*AMQP is a binary protocol. We need to convert the inputs to raw bytes*/
var message = $"{tourname};{name};{email}";
var bytes = Encoding.UTF8.GetBytes(message);

/*publish the message. routing key is string.empty because exchange type is fanout*/
channel.BasicPublish("webappExchange", string.empty, null, bytes);
/*channel.BasicPublish("webappExchange", "tout.booking", null, bytes);  //Direct Exchange
  channel.BasicPublish("webappExchange", "tour.*", null, bytes);        //Topic Exchange

  var props = new BasicProperties();                                    //Heades Exchange
  props.Headers = new Dictionary<string, object> {"subject", "tour"}, {"action", "booked"} };
  channel.BasicPublish("webappExchange", "", props, bytes);*/

/*close connection*/
channel.Close();
connection.Close();
```

## Consume a message
```cs
using RabbitMQ.Client;
using RabbitMQ.Client.Events;

/*connection to the RabbitMQ instance*/
var factory = new ConnectionFactory();
factory.Uri = new Uri("amqp://guest:guest@localhost:5672");
var connection = factory.CreateConnection();
var channel = connection.CreateModel();

/*define the queue and bind the queue to the exchange. Note that routing key is string.empty because exchange type is fanout*/
channel.QueueDeclare("emailServiceQueue", true, false, false);
channel.QueueBind("emailServiceQueue", "webappExchange", string.empty);
/*channel.QueueBind("emailServiceQueue", "webappExchange", "tout.booking");     //Direct Exchange
  channel.QueueBind("emailServiceQueue", "webappExchange", "tout.*");           //Topic Exchange
                                                                                //Exchange Headers
  var headers = new Dictionary<string, object> {"subject", "tour"}, {"action", "booked"}, {"x-match", "all"}};
  channel.QueueBind("emailServiceQueue", "webappExchange", string.empty, headers);*/

/*define the consumer*/
var consumer = new EventingBasicConsumer(channel);
consumer.Received += (sender, eventArgs) =>
{
    /* AMQP is a binary protocol. We need to decode the raw bytes */
    var msg = Encoding.UTF8.GetString(eventArgs.Body);
    Console.WriteLine($"{eventArgs.RoutingKey}: {msg}");

    /*Exchange Headers data
    var subject = Encoding.UTF8.GetString(eventArgs.BasicProperties.Headers["subject"] as byte[]);
    var action = Encoding.UTF8.GetString(eventArgs.BasicProperties.Headers["action"] as byte[]);*/
};

//set the channel to start consuming from the queue
channel.BasicConsume("emailServiceQueue", true, consumer);

/*close connection*/
channel.Close();
connection.Close();
```

## Advance usage

* Remove guest user
* Create an user per application
* Create different users for management UI with different exchange and queue permissions ([read; write; configure](https://www.rabbitmq.com/access-control.html))
* Use **topic permissions** to secure the RabbitMQ instance with restrictions for publishers and consumers
* Validate UserId sample:
```cs
/*publish message with userId*/
var props = new BasicProperties();
props.UserId = "user1"

/*consume message with userId*/
Console.WriteLine(eventArgs.BasicProperties.UserID);
```

* Trace messaging with **amq.rabbitmq.trace** exchange and **rabbitmq_tracing** plugin. Extra messages are sent to a special exchange.
```shell
rabbitmq-plugins enable rabbitmq_tracing
rabbitmqctl trace_on
```

!!!note "RabbitMQ must be restarted"
!!!note "Tracing impact the performance"

## Dead Letters

A dead letter is a message on an exchange that has nowhere else to go. Reasons:

* The message is rejected or negatively acknowledged by the consumer 
* The message expires due to a time-to-live option specified on the queue or on the message. 
* The queue exceeded a length limit

Usefull to investigate issues: Sample:

```cs
var arguments = new Dictionary<string, object>
{
    {"x-dead-letter-exchange", "DLX"}
};
channel.QueueDeclare("backOfficeQueue", true, false, false, arguments);
/*reject the message by the consumer*/
channel.BasicReject(eventArgs.DeliveryTag, false);
/*set automatically acknowledge to false*/
channel.BasicConsume("backOfficeQueue", false, consumer);

/*consume the dead letter*/
channel.ExchangeDeclare("DLX", ExchangeType.Direct, true, false);
channel.QueueDeclare("deadLetters", true, false, false);
channel.QueueBind("deadLetters", "DLX", "");

var consumer = new EventingBasicConsumer(channel);
consumer.Received += (sender, eventArgs) =>
{
    var message = Encoding.UTF8.GetString(eventArgs.Body);
    var deathReasonBytes = eventArgs.BasicProperties.Headers["x-first-death-reason"] as byte[];
    var deathReason = Encoding.UTF8.GetString(deathReasonBytes);
    Console.WriteLine($"Deadletter: {message}. Reason: {deathReason}");
};

channel.BasicConsume("deadLetters", true, consumer);
```

## Next steps
https://www.rabbitmq.com/getstarted.html
https://www.rabbitmq.com/configure.html
https://www.rabbitmq.com/monitoring.html
https://www.rabbitmq.com/production-checklist.html