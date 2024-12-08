# Applications

## SNS (Simple Notification Service)

**DLQ** (Deed Letter Queue) is supported in SNS, SQS and Lambda. A dead-letter queue saves discarded events for further processing - when an event fails all processing attempts or expires without being processed.

## SQS

to increase the number of messages the application receives:
1. Use the `ReceiveMessage` API to retrieve up to 10 messages at a time
2. Add additional Amazon SQS queues and have the application poll those queues to scale horizontally
