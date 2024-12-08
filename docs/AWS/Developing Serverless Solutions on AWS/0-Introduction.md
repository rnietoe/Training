# Introduction

* [Summary](https://www.aws.training/Details/InstructorLedTraining?id=109284)
* [PPT](https://www.aws.training/Details/eLearning?id=56189)
* [Labs](https://us-east-1.student.classrooms.aws.training/class/cfYz35an1niB5nu2ZAGW1K)
* [DirectPoll](https://directpoll.com/v?XDVhEtBQHdt40mvZmG3IRzBzX1iGk) - to use in the sequel api gateway trainning
* [checkip.amazonaws.com](https://checkip.amazonaws.com/)

deploying using different aws accounts instead of different VPCs, so no only network is different

Dev tools and frameworks:

* AWS Serverless Application Model (SAM)
* AWS Cloud Development Kit (CDK)

## Module 1 - Thinking Serverless

Amazon **API Gateway** is a synchronous event source and is well suited to be the front door to your application. You might need synchronous connections if: 
* A client requires a synchronous response.
* You need to direct how the response comes back.
* You need transactional boundaries.
* You need the absolutely lowest latency. 
* The message flows within a single service’s bounded context.

**Lambda** provides serverless compute and is where your business logic lives.
* Lambda functions run based on an event source and receive data through the event object.
* Lambda functions produce events that other services can consume. 
* Lambda has native support for events that AWS messaging and streaming services produce. These integrations can handle communication between services so you can connect things without knowledge of the services’ API. 

You can use Amazon **EventBridge** and Amazon Simple Notification Service (Amazon **SNS**) for messaging and fan-out:

* EventBridge is an event router that can abstract producers from consumers and filter events using rules. 
* Amazon SNS is a pub/sub service that can filter notifications to subscribers. 
* Both services are asynchronous event sources for Lambda and can be Lambda targets. Both provide built-in error handling. 

Amazon **SQS** can queue requests to prevent a service from becoming overwhelmed. It provides a durable store for events.

**Kinesis** Data Streams is a streaming event source for Lambda. 
A common use case is data processing where the stream contains data about upstream events. For example, send events that impact inventory to Kinesis Data Streams, and analyze inventory to dynamically offer discounts in the afternoons. 

* Configured authentication through an **Amazon Cognito** user pool
* Deployed your backend code using **AWS SAM**
* Viewed API documentation using the **Swagger Editor**
* Updated your front-end configuration file and ran the build through **AWS Cloud9** to test it prior to deployment
* Deployed your front-end application using **Amplify**

**Vue JavaScript** framework is a progressive framework for building user interfaces. Unlike other monolithic frameworks, Vue is designed to be incrementally adoptable. The core library focuses on the view layer only and is easy to pick up and integrate with other libraries or existing projects. Vue is also perfectly capable of powering sophisticated single-page applications when used in combination with modern tooling and supporting libraries.

Swagger API is an open-source software framework backed by a large ecosystem of tools that help developers design, build, document, and consume RESTful web services. Swagger also allows you to understand and test your backend API specifically. Swagger would be displayed using https://editor.swagger.io/ in case of moving sequel apigw to aws api gateway. 

Setting up authentication with **Amazon Cognito** lets you add user sign-up, sign-in, and access control to your web and mobile apps quickly and easily. Amazon Cognito scales to millions of users and supports sign-in with social identity providers, such as Facebook, Google, and Amazon, and enterprise identity providers via SAML 2.0.

Deploying the backend application using **AWS SAM** is an open-source framework for building serverless applications. It provides shorthand syntax to express functions, APIs, databases, and event source mappings. With just a few lines per resource, you can define the application you want and model it using YAML. During deployment, AWS SAM transforms and expands the AWS SAM syntax into AWS CloudFormation syntax, enabling you to build serverless applications faster.

Deploying the bookmark application using **AWS Amplify** is a set of tools and services that enables mobile and front-end web developers to build secure, scalable full stack applications powered by AWS. Amplify includes an open-source framework with use-case-centric libraries and a powerful toolchain to create and add cloud-based features to your application, and a web-hosting service to deploy static web applications.

Testing the bookmark application. **Amazon DynamoDB** is a key-value and document database that delivers single-digit millisecond performance at any scale. It’s a fully managed, multi-Region, durable database with built-in security, backup and restore, and in-memory caching for internet-scale applications. DynamoDB can handle more than 10 trillion requests per day and can support peaks of more than 20 million requests per second.

## Module 2 

1. *build* apigateway http api
2. create /snackpick get *route*
3. create *integration* with a lambda function
4. create dev *stage* (endpoint) with automatic deployment

api gateway is set as the lambda trigger

1. *build*  apigateway rest api
2. example API (or import from swagger or open API 3)
3. create prod *stage*

How AWS AppSync works for GraphQL APIs??

## Module 3 - Auth

* JWT-based authorization
* AWS Iam permissions
* Lambda authorizers

Amazon *Cognito* - Vue framework

## Module 4 - Deployments

* AWS CloudFormation
* AWS CDK
* AWS SAM (like tfs release)
* Amplify Framework

transform CF to AWS SAM template

## Module 5 - EventBridge and SNS

SNS act as a dead-letter queue for lambda functions. S3 required if payload is bigger thant sns max size


## Module 6: Event-Driven Development Using Queues and Streams

## Module 7: Writing Good Lambda Functions

execution role policies (permissions from lambda) and resource-based policies (permissions to lambda)

vpc and subnets of lambda equals to dynamodb network configuration?

**Amazon RDS proxy endpoint** to allow connection pooling

* only include runtime necessities
* prefer simpler frameworks
* for java, put your dependency .jar file in a separate /lib directory
* use AWS EFS for large files - that never change -, external libraries and file sharing, instead of s3 to use athena?

lambda versioning. once version created, code, environment, layer, etc. can no be changed

create version alias, for example TEST,PROD, with weighted (%) base on memory updates or whatever change.

function start running from the handler method

use layer to share code across functions, external dependencies

use unique identifiers for the messageid and the payload

invocation errors VS function errors. set apigw or sns (with a queue for dead-letter) between the client and the lambda

retry invoke up to 6 hours

step functions (https://aws.amazon.com/step-functions/getting-started/): orchestrtion from a lambda to other instead of using sequel workflow
 
 use callbacks to wait for an external action
 use .sync suffix to pause the process until external action complete

## Module 9: Observability and Monitoring



## Module 10: Serverless Application Security 

mTLS(mutual TLS):

* https://aws.amazon.com/es/blogs/aws-spanish/configuracion-automatica-de-tls-mutuo-mtls-en-amazon-api-gateway/
* https://docs.aws.amazon.com/apigateway/latest/developerguide/rest-api-mutual-tls.html

VPC link ?

## Module 11. Scaling

[AWS Lambda Power Tuning](https://docs.aws.amazon.com/lambda/latest/operatorguide/profile-functions.html)

Just to complete, the Kinesis client libary is available in Java, Phyton, .NET, Node.js and Ruby

## Module 12. Automating the Deployment Pipeline

* Source - AWS CodeCommit
* Build - AWS CodeBuild, AWS CodePipeline, AWS SAM (like tfs release)
* Test - AWS CDK
* Deploy - AWS CloudFormation

[Safe Lambda deployments](https://github.com/aws/serverless-application-model/blob/develop/docs/safe_lambda_deployments.rst) using cloudwatch alarms (and hooks)