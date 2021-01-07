# Applications

## SQS Simple Queue Service

**pull** based service to store messages in a queue to **decouple** the components of an application

* standard queues (default)
* FIFO queues (first in first out)

retention period: 14 days

**visibility timeout**: time while message is invisible during processing

**long polling**: retrieve messages from SQS queues when message arrives to the queue

## SWF Amazon Simple Workflow Service

web service to coordinate work across distributed application components as executable code, web service calls, **human actions** or scripts, based on workflow tasks

it makes sure a task is assinged only onces and never duplicated till 1 year

SWF Actors:

* Workflow starters: start the workflow
* Deciders : decide what to do next if something fails or finish
* Activity workers

## SNS Simple Notification Service

**push** based web service to send messages / notifications from the cloud to mobile devices, by SMS or mail to SQS or any Http endpoint

group multiple recipients using **topics**

messages are stored across multiple AZ to prevent lost