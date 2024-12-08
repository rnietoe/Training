# Compute

## Elastic Beanstalk

The source bundle you'll need to upload must meet the following requirements:
1. Consist of a single ZIP file or WAR file (you can include multiple WAR files inside your ZIP file)
2. Not exceed 500/512 MB
3. Not include a parent folder or top-level directory (subdirectories are fine)

**cron.yml** is used to deploy a worker application that processes periodic background tasks.

## Elastic Load Balancers

ALB target group supports EC2 instances, IP addresses or Lambda functions for load balancing

## Lambda

Different services can trigger your function,  such as  Api Gateway
* a lambda function can trigger other lambda functions
* ALB, Cognito, Lex, Alexa, API Gateway, CloudFront, DynamoDB, SQS, MQ, and Kinesis Data Firehose are all valid direct (synchronous) triggers for Lambda functions. using AWS CLI too. 
* **S3/SNS** are valid **asynchronous** triggers.

An event source mapping uses permissions in the function's **execution role** to read and manage items in the event source