# 1. Introduction

* **IaaS**: Infrastructure as a service: you manage the entire infrastructure (**`EC2`**, **`VPC`**, **`RDS`**, **`S3`**) in the cloud, but platform and software run on others' infrastructure
* **PaaS**: Platform as a Service, you simply manage platform instead of the infrastructure, such as **`Elastic Beanstalk`** or **`Lightsail`** 
* **SaaS**: Software as a Service, you use the software developed by others from the cloud, such as Gmail
* **FaaS**: Function as a Service
* **DaaS**: Desktop as a Service such as Windows or Linux, provide by **`WorkSpaces`**.

 Three different ways to access AWS:

 1. Programmatic, using the `aws` CLI (**C**ommand **L**ine **I**nterface)
 2. Using the Console
 3. Using SDK 

The three types of cloud deployments are:

* Public
* Hybrid
* Private (also called 'on-prem')

Traditional Computing VS Cloud Computing

* IT Assets as provisioned resources
	* Bootstrapping scripts, Golden images, Containers, Hybrid
	* CloudFormation
	* Automation: serverless management and deployment
		* AWS Elastic Beanstalk
		* EC2 auto recovery and Auto Scaling
	* Alarms and Events - CloudWatch - Lambda scheduled events, WAF security automations
* Global, available and scalable capacity
	* Scale up / Vertical Scaling (increase RAM, CPU)
	* Scale Out - Horizontal Scaling (add multiple virtual machines behind a ELB)
		* Stateless applications (Lambda - Alexa)
		* Stateless componets (login credentials in a cookie)
		* Statefull components (store information in a db)
		* Distribute load to multiple nodes
		* Distributed Processing 
		* Implemente distributed processing
* Higher level managed services
* Built-in security
* Architecting for cost
* Operations on AWS
	
!!!danger "Build your systems to be scalable, use disposable resources, reduce infrastructure to code, and assume EVERYTHING will fail sooner or later."

Global services (for every region):

* IAM
* Route53
* CloudFront
* SNS (Simple Notify Service)
* SES

Regional Services with global view

* S3

Caching services:

* CloudFront
* API Gateway
* ElastiCache
* DynamoDB Accelerator (DAX)

AWS Service used on premise:

* AWS Snow Family:
    * **Snowball** - to upload 50TB (200$) or 80TB (250$) to AWS in a week instead of three months. This allow S3 imports/exports
    * **Snowball Edge** - Local compute and storage only till 100TB
    * **Snowmobile** - a track with 100PB
    * First 10 days are free. 15$ per day later
    * Data transfer to S3 is free. Data transfer out is charged
* **Storage gateway**: connect on-premise software with cloud-based storage (S3)
    * File Gateway. objects are flat files
    * Volume Gateway: objects are hard disk drives. It looks like EBS snapshots. there are storage volumes (entire dataset) and cached volumes
    * Tape gateway
* CodeDeploy - include applications
* Opsworks - include applications
* IoT greengrass

Overview: 

* **Availability Zone** (AZ) are data center
* **Region** is a distinct location within a geographical area with 2 or more AZ, designed to provide high availability to a specific geography. Choosen by law, latency and AWS Services

    !!!info "US East (N. Virginia) us-east-1 was the first region and all new services are deployed here first" 

* **Edge Locations** are Amazon's CDN (**C**ontent **D**elivery **N**etwork) end points. Objects are cached for 48 hours by default (TTL - **T**ime **T**o **L**ive). This not just read only. Types:
    * **Web Distribution** (websites)
    * **RTMD** (media streaming) - Discontinued support by CloudFront on December 31, 2020"

    !!!danger "Number of Edge Locations > Number of Availability Zones > Number of Regions"

## CloudFront

**`AWS CloudFront`** distribution is a collection of a CDN's Edge Locations. CloudFront content is **cached** in Edge Locations. This allows you to distribute content using a worldwide network of edge locations that provide low latency and high data transfer speeds. 

* From `AwS CloudFront` select `Create Distribution`
* The CloudFront origin can be an S3 bucket, an EC2 instance, an Elastic Load Balancer or Route53.
* We can restrict viewer access using signed URLs for individual files or signed cookies for multiple files. Netflix or CloudGuru samples. 
    * OAI - **O**rigin **A**ccess **I**dentity
    * If the origin is EC2 then use CloudFront Signed url 
    * If the origin is S3 then use S3 signed url 
* We can `create invalidations` to remove origin objets on the edge location
* To delete a CloudFront distribution, you have to disable it first. This process takes 15 minutes	
* Pricing depends on:
    * traffic distribution
    * requests
    * data transfer out

!!!danger "We will be charged when deleting cached data from an edge location"
    
## Resource Groups and Tag Editor

* **Tags** allow to find AWS resources in a selected region, but it can not directly managing those resources. This can make it easier to search for and filter resources by purpose, owner, environment, or other criteria.

* **Resources groups** allow to execute operations from **`AWS Systems Manager`** to different resources (such as a EC2 fleet) based on resources groups
