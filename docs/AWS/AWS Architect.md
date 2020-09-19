# AWS Architect

## Introduction to AWS

* IaaS: Infrastructure as a service, such as AWS - EC2
* PaaS: Platform as a Service, such as Go Daddy - Elastic Beanstalk or LightSail
* SaaS: Software as a Service, such as Gmail -  
* FaaS: Function as a Service

## Sign Up for an AWS Account

* rnietoe@gmail.com - AWS account (root account)
* rnietoe/training
* Abodroc@83
* Google Authenticator for Andorid

https://rnietoe.signin.aws.amazon.com/console

`Security/IAM` (Identity and Access Management). Users and groups are created globally and not for a specific a region
 MFA (multi-factor authentication)

 Three different ways to access AWS:

 1. Programmatic, using the command line
 2. Using the Console
 3. using SDK

## AWS Certification Preparation Guide

**ACloud.Guru Final Practice Exam**

https://aws.amazon.com/certification/certification-prep/

• Step 1: Take an AWS Training Class  
• Step 2: Review the Exam Guide and Sample Questions  
• Step 3: Practice with Self-Paced Labs and an Exam Prep Quest  
• Step 4: Study AWS Whitepapers  
• Step 5: Review AWS FAQs  
• Step 6: Take an Exam Prep Workshop  
• Step 7: Take a Practice Exam  
• Step 8: Schedule Your Exam and Get Certified  

https://www.qwiklabs.com/

AWS Documentation:  
https://docs.aws.amazon.com/index.html?nc2=h_ql_doc_do  
https://aws.amazon.com/faqs/  
https://aws.amazon.com/whitepapers/?whitepapers-main.sort-by=item.additionalFields.sortDate&whitepapers-main.sort-order=desc  
https://www.youtube.com/c/amazonwebservices/videos  
https://aws.amazon.com/architecture/  
https://aws.amazon.com/new/  
https://www.meetup.com/es/topics/amazon-web-services/  

AWS Exam
https://aws.amazon.com/certification/faqs/
https://aws.amazon.com/certification/certified-solutions-architect-associate/

AWS Cloud Practitioner Essentials (Second Edition) (Spanish)
https://www.aws.training/Details/Curriculum?id=46152

## AWS Certified Cloud Practitioner 2020

* Availability Zone = data center
* Region = geographical area with 2 or more AZ. Choosen by law, latency and AWS Services
* Edge Locations = CloudFront (cloud cache) - amazon's content delivery network (CDN) end points. Objects are cached for 48 hours by default (TTL - Time To Live). this not just read only. Types:
    * Web Distribution (websites)
    * RTMD (media streaming)

!!!warning "CloudFront is discontinuing support for RTMP distributions on December 31, 2020"

* `CloudFront` Distribution: an edge location collection. it allows you to distribute content using a worldwide network of edge locations that provide low latency and high data transfer speeds

    1. Create a CloudFront distribution
    2. Browse to the CloudFront distribution domain name (dzi85fss8k88j.cloudfront.net/malaga.jpg)
    3. To delete a CloudFront distribution, you have ti disable it first. this process takes 15 minutes

**US East (N. Virginia) us-east-1** was the first region and all new services are deployed here first 

Support Plans

* Basic - free
* Developer includes responses in 12 -24 hours - $29 a month
* Business includes 24x7 support - $100 a month
* Enterprise includes a TAM (Technical Account Manager) - $15000 a month

!!!important "Download putty and using putty gen, load private key file created at `EC2/Key Pairs` (rnietoe.ppk)" 

Setting up a billing alarm at `CouldWatch/Alarms/Billing` using SNS (Notify Service) topic


### Compute

#### EC2

EC2 is a virtual server in the cloud. Pricing models:

* On Demand: low cost for short term application
* Reserved: contract terms are 1 to 3 years
* Spot: based on start and end time.
* Dedicated Hosts: to reduce cost using your SW licenses

#### Lambda

### Databases

#### RDS (Relational Database Service)

#### DynamoDB (Non Relational Databases)

### Storage

#### S3 (Simple Storage Service)

Object-based storage such as text files, videos, pictures and any other flat file from 0 to 5 tb. 

`Buckets` are globally unique containers for everything that you store in Amazon S3

names are like https://s3-region_name.amazonaws.com/bucket_name

The response code is 200 when file upload succeeds

The object has a key (filename), value (data), versionID, Metadata, encryption, ans security by Access Control Lists and Bucket Policies 

write new files are read inmediately. however, updates and deletes takes a little bit of time to propagate

* S3 Standard: Frequently accessed data
* S3 Standard - IA (Infrequently Accessed)
* S3 One Zone - IA
* S3 - Intelligent Tiering, using machine learning to move files to the most cost-effective access tier
* S3 Glacier (low cost storage. retrieval times fro minutes to hours)
* S3 Glacier Deep Archive (lowest-cost. retieval time of 12 hours is acceptable)
 
`Amazon S3 Transfer Acceleration` takes advantage of Amazon CloudFront (edge location - cache) using Amazon internal network (no internet) It enables fast, easy and secure transfers of files to and from your bucket.

`cross bucket replication`

To upload a file larger than 160 GB, use the AWS CLI, AWS SDK, or Amazon S3 REST API.

Uploaded files are not public by default:

* Bucket policies applies to the whole bucket
* Obkect policies applies to an individual file
* IAM policies applies to users and groups

We can use the bucket to host a static website using S3, with index.html and error.html. Dynamic website can not be hosted on S3

**Public** bucket policy sample:
```json
{
	"Version": "2020-09-08",
	"Statement": [
		{
			"Sid": "PublicReadGetObject",
			"Effect": "Allow",
			"Principal": "*",
			"Action": [
				"s3:GetObject"
			],
			"Resource": [
				"arn:aws:s3:::rnietoe/*"
			]
		}
	]
}
```

#### Glacier

### Network

#### VPC (Virtual Private Cloud)

#### Route53



## Rethinking Architecture in the Age of Truly Commoditized Compute | James Simmons

AWS Cost Control - Deprecated Aug 2020
4:30:00
Introduction to AWS CloudFormation
2:00:00
Introduction to Git
Supplementary
Git Branches
Supplementary
Git Log, Show and Diff
Supplementary
Introduction to Amazon RDS
3:00:00
S3 Masterclass
9:00:00
#101 - Securing S3
Supplementary
Mastering the AWS Well-Architected Framework
4:30:00
AWS Certified Solutions Architect Associate SAA-C02
12:30:00
Application Load Balancer - Deprecated Aug 2020
Supplementary
Introduction to Machine Learning
5:00:00
#107 - Does Twitter Hate Cats?
Supplementary
#101 - What is the DeepRacer?
Supplementary
Introduction to AWS AppSync
2:15:00
Docker Fundamentals - Deprecated Aug 2020
5:00:00
Professional
457 Total Lessons, 50.76 Hours of Video
AWS Security Best Practices - Deprecated Aug 2020
2:00:00
The Sky Is Falling, Run! | Mark Nunnikhoven
Supplementary
#109 - Your Very Own API
25:48
#108 - Let the Free Times Roll
19:48
The Complete Serverless Course - Deprecated Aug 2020
14:30:00
AS400/Mainframe to Serverless | David Wilson
Supplementary
Building Resilient Serverless Systems with "Non-Serverless" Components | Jeremy Daly
Supplementary
AWS Business Essentials
2:00:00
Demonstrating ROI: Convincing the VCs/Bosses to Invest in Serverless | MJ Ramachandran
Supplementary
AWS Certified Developer - Associate 2020
16:30:00
Git Merge
Supplementary
Git Merge Conflicts
Supplementary
Git Remotes
Supplementary
Hands on with AWS Redshift: Table Design - Deprecated Aug 2020
2:30:00
AWS Certified SysOps Administrator - Associate 2020
12:30:00
Essentials for Windows Administrators on AWS - Deprecated Aug 2020
Supplementary
Automating AWS with Python - Deprecated Aug 2020
Supplementary
Guru
496 Total Lessons, 76 Hours of Video
Advanced AWS CloudFormation - Deprecated Aug 2020
12:00:00
#111 - Soda-theft Detection Device
Supplementary
AWS DynamoDB - From Beginner to Pro - Deprecated Aug 2020
19:00:00
Kubernetes Deep Dive
4:00:00
#112 - How to Turn on a Lamp from Anywhere in the World
Supplementary
AWS CodeDeploy - Deprecated Aug 2020
4:00:00
Avoiding Work: Architecting for serverless efficiency | Ryan Scott Brown
Supplementary
AWS Certified Advanced Networking - Specialty 2020
25:00:00
AWS Certified Solutions Architect - Professional 2020
12:00:00