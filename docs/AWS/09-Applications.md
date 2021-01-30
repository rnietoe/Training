# 9. Applications

## SQS (Simple Queue Service)

**pull** based service to store messages in a queue to **decouple** the components of an application

* standard queues (default)
* FIFO queues (first in first out)

If you have an existing application that uses standard queues and you want to take advantage of the ordering or exactly-once processing features of FIFO queues, you need to configure the queue and your application correctly. You can't convert an existing standard queue into a FIFO queue. To make the move, you must either create a new FIFO queue for your application or delete your existing standard queue and recreate it as a FIFO queue. 

**retention period**: the message can remain in the queue for 14 days

**visibility timeout**: time while message is invisible during processing. maximum VisibilityTimeout is 12 hours. The default visibility timeout for a message is 30 seconds. The minimum is 0 seconds

**long polling**: retrieve messages from SQS queues, reducing the number of empty responses
    setting `ReceiveMessageWaitTimeSeconds` default value = 0 ?

attribute `DelaySeconds`: When a new message is added to the SQS queue, it will be hidden from consumer instances for a fixed period.

attribute `WaitTimeSeconds`: When the consumer instance polls for new work, the SQS service will allow it to wait a certain time for one or more messages to be available before closing the connection.

Duplicate messages occur when a consumer does not complete its message processing and the visibility timeout of the message expires, making it visible for another consumer to obtain. Increasing the visibility timeout to enable the consumer processing to complete, will prevent duplicate messages.

SQS service guarantees a message will be delivered at least once.

* automatically scales
* messages can be up to 256kb
* multiAZ

## SNS (Simple Notification Service)

**push** based web service to send messages / notifications from the cloud to mobile devices, by SMS or mail to SQS or any Http endpoint, for example CloudWatch or Cost Explorer

use the publish-subscribe mechanism based on topics, called pub-sub messaging

group multiple recipients using **topics** (a place to put some messages) 

An ARN (Amazon Resource Name) is created when you create a topic on Amazon SNS

messages are stored across multiple AZ to prevent lost

**DLQ** (Deed Letter Queue) is supported in SNS, SQS and Lambda

* messages can be up to 256kb
* multiAZ

send_message.py
```python
#!/usr/bin/env python3

import argparse
import logging
import sys
import uuid
from time import sleep

import boto3
from botocore.exceptions import ClientError

parser = argparse.ArgumentParser()
parser.add_argument("--queue-name", "-q", default="Messages", help="SQS queue name")
parser.add_argument("--interval", "-i", default=0.1, help="timer interval", type=float)
parser.add_argument("--message", "-m", help="message to send")
parser.add_argument("--log", "-l", default="INFO", help="logging level")
args = parser.parse_args()

if args.log:
    logging.basicConfig(format="[%(levelname)s] %(message)s", level=args.log)
else:
    parser.print_help(sys.stderr)

sqs = boto3.client("sqs")

try:
    logging.info(f"Getting queue URL for queue: {args.queue_name}")
    response = sqs.get_queue_url(QueueName=args.queue_name)
except ClientError as e:
    logging.error(e)
    exit(1)

queue_url = response["QueueUrl"]

logging.info(f"Queue URL: {queue_url}")

while True:
    try:
        message = str(uuid.uuid4())
        logging.info("Sending message: " + message)
        response = sqs.send_message(QueueUrl=queue_url, MessageBody=message)
        logging.info("MessageId: " + response["MessageId"])
        sleep(args.interval)
    except ClientError as e:
        logging.error(e)
        exit(1)
```

receive_messages.py
```python
#!/usr/bin/env python3

import logging
import time

import boto3
from botocore.exceptions import ClientError

QUEUE_NAME = "Messages"

logging.basicConfig(format="[%(levelname)s] %(message)s", level="INFO")
sqs = boto3.client("sqs")

try:
    logging.info(f"Getting queue URL for queue: {QUEUE_NAME}")
    response = sqs.get_queue_url(QueueName=QUEUE_NAME)
except ClientError as e:
    logging.error(e)
    exit(1)

queue_url = response["QueueUrl"]
logging.info(f"Queue URL: {queue_url}")

logging.info("Receiving messages from queue...")

while True:
    messages = sqs.receive_message(QueueUrl=queue_url, MaxNumberOfMessages=10)
    if "Messages" in messages:
        for message in messages["Messages"]:
            logging.info(f"Message body: {message['Body']}")
            time.sleep(1)  # simulate work
            sqs.delete_message(
                QueueUrl=queue_url, ReceiptHandle=message["ReceiptHandle"]
            )
        else:
            logging.info("Queue is now empty")
```

## SWF (Simple Workflow Service)

web service to coordinate synchronous and asynchronous work across distributed application components as executable code, web service calls, **human actions** or scripts, based on workflow tasks

it makes sure a task is assinged only onces and never duplicated till 1 year

SWF Actors:

* Workflow starters: start the workflow
* Deciders : decide what to do next if something fails or finish
* Activity workers

a **domain** refer to a collection of related workflows


## Step Functions

Step functions eventually replace SWF using state machines with: 

* deciders
* activity tasks
* worker tasks

support pararell processing

## Amazon Polly

Machine learning service to convert text to audio mp3 (Alexa)

## SAM (Serverless Application Model)

CloudFormation extension optimized for serverless functions, APIs and tables. it can run locally with docker

`Transform` tag

```shell
sam init

sam deploy --guided
```

## ECS (Elastic Container Service)

Amazon ECS is an orchestration service to deploy, manage, and scale Docker **containers** running applications, services, and batch processes. Amazon ECS places containers across your cluster based on your resource needs and is integrated with familiar features like ELB, EC2 security groups, EBS volumes and IAM roles.

no VM builds required, but Ec2 can be used for more control

**ECR** (Elastic Container Registry) is the managed docker container resigry to store, manage and deploy images. it work with on-premises deployment

Use **Fargate** to automatically build environments. Fargate is a serverless container engine (like docker). It works with both ECS and **EKS** (Elastic Kubernetes Service)

* container
* task
* service
* cluster