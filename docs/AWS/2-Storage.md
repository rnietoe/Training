# 2. Storage

## S3

**`AWS S3`** is **Object-based** for the safe storage of **flat** files such as text files, videos, pictures and any other flat file from 0 to 5 tb. The object has a key (filename), value (data), versionID, metadata, encryption, and security by ACL (**A**ccess **C**ontrol **L**ists), torrent and Bucket Policies 

!!!info "Objects stored in S3 are stored in multiple servers in multiple facilities across AWS"

**Buckets** are folders/containers for everything that you store in S3. S3 bucket names are **global**, and must be unique. Universal namespaces are like https://s3-region_name.amazonaws.com/bucket_name. The response code is http 200 when file upload succeeds.

!!!danger "Create new files are read inmediately. Updates and deletes takes a little bit of time to propagate."

S3 classes:

* **S3 Standard**: Frequently accessed data
* **S3 - IA** (**I**nfrequently **A**ccessed)
* **S3 One Zone - IA** (multiple AZ not required) aka **RRS**
* **S3 - Intelligent Tiering**, using machine learning to move files to the most cost-effective access tier
* **S3 Glacier** (low cost storage. retrieval times from 2 minutes to hours)
* **S3 Glacier Deep Archive** (lowest-cost. retrieval time of 12 hours is acceptable)

!!!danger "It does not make sense to use S3 standard. Instead, use S3 - Intelligent Tiering to save money"

More features:

* Unlimited storage
* We can use the bucket to host a **static website** using S3 with option `static website hosting` eanbled, with index.html and error.html. Dynamic website can not be hosted on S3.
* Encryption:
    * Encryption in Transit, using https (SSL/TLS)
    * Encryption at Rest:
        * In the server side (**S**erver **S**ide **E**ncryption):
            * **SSE-S3**: An encryption key that Amazon S3 creates, manages, and uses for you.
            * **SSE-KMS**: An encryption key protected by AWS **K**ey **M**anagament **S**ervice
            * **SSE-C** with customer provided keys
        * In the client side - encrypted before uploading
* Versioning (disabled by default):
    * usefull as backup tool
    * **MFA Delete** setting: an additional layer of security that requires MFA for changing Bucket Versioning settings and permanently deleting object versions. To modify MFA delete settings, use the AWS CLI, AWS SDK, or the Amazon S3 REST API.
    * Cannot be disabled once enabled
    * Uploaded new verions are **private** by default
    * Deleting a file create a new version (**deleted marker**), then restoring the file recovers all previous versions.
    * Integrated with lifecycle rules
* **S3 Transfer Acceleration** takes advantage of Amazon CloudFront (edge location - cache) using Amazon internal network (no internet). It enables fast, easy and secure transfers of files to and from your bucket.

    * [Speed Comparison Tool](https://s3-accelerate-speedtest.s3-accelerate.amazonaws.com/en/accelerate-speed-comparsion.html)

* **Cross Region Replication**: replicate a bucket from a region to another:

    * From S3 Management tab, select `Create replication rule`
    * Create a new role and select the source bucket and destination bucket. Replicated buckets can be in different AWS accounts
    * Replication requires versioning to be enable on the source and destionation buckets.
    * we can change the storage class for the replicated objetcs

    !!!danger "replication starts when files are added and updated later. Deletes are not replicated. Permissions and previous versions are not replicated"

* Use **S3 Lifecycle** rules to define actions you want AWS S3 to take during an object's lifetime such as **transitioning** objects to another storage class, archiving them, or deleting them after a specified period of time.
* **Multipart uploads** use multithreading to upload large files to S3 buckets **in parallel** (the parts of the file are uploaded in parallel). recommended for > 100mb and required for > 5Gb
* **AWS DataSync** is used to move large amounts of data from on-premise to AWS S3, EFS, FSx, etc.
* **`AWS DMS`** (**D**atabase **M**igrations **S**ervice) is the best choice for conventional database migrations to AWS.
    * homogenous: sample of Oracle to Oracle
    * heterogenous with SCT (**S**chema **C**onversion **T**ool): sample of SQL Server to Aurora
* [General S3 FAQs](https://aws.amazon.com/s3/faqs/)

[S3 Pricing](https://aws.amazon.com/s3/pricing/) depends on:

* Storage class
* Storage
* Requests
* Data transfer
* Transfer acceleration
* Cross region replication

To upload a file larger than 160 GB, use the AWS CLI, AWS SDK, or Amazon S3 REST API.

S3 Security:

* Uploaded files are **private** by default
* Bucket policies applies to the entire bucket (like one hosting a static S3 website - public) 
* Bucket ACLs. Object policies applies to an individual file.
* **Public** bucket policy sample:
```json
{
	"Version": "2020-09-08", // identify the document structure
	"Statement": [
		{
			"Sid": "PublicReadGetObject", // statement id
			"Effect": "Allow", // allow | deny
			"Principal": "*",
			"Action": [
				"s3:GetObject"
			],
			"Resource": [
				"arn:aws:s3:::BUCKET_NAME/*" 
			]
		}
	]
}
```

**S3 Objetc lock** are objects **W**ritten **O**nce and **R**ead **M**any. WORM model. Objects (the whole bucket or individual files) became unmodificable and undeletable:

* Governance mode: some users are grant with permissions to alter settings or delete the object version
* Compliance mode: objects version cannot be modified or deleted, even by the root user

S3 Performance:

* Use **prefixes** to improve performance: path between the bucket and file
* Use SSE-KMS have limitations
* Use **Multipart uploads** 
* Use downloads with **S3 Byte-Range Fetches**
* Use **S3 Select**/**Glacier Select** using SQL to download only the subset of data we need from ZIP or CSV files 

## Glacier

Glacier pricing:

* storage
* Data Retrieval times

**S3 Glacier Vault Lock Policy**: compliance controls for S3 Glacier with a Vault Lock policy. The policy can no longer be changed

## EFS

Based on **NFSv4**, Amazon EFS (**E**lastic **F**ile **S**ystem) is a mountable **file** storage service for EC2 (linux), but has no connection to S3 which is an object storage service.

Data is stored across multiple AZ's within a region

### How to create an EFS **shared** by two EC2 instances:

1. From AWS EFS, `Create file system`
2. Create EC2 instances with following user data: 

    ```shell
    #!/bin/bash
    yum update -y
    yum install httpd -y
    service httpd start
    chkconfig httpd on
    yum install amazon-efs-utils -y
    ```

3. default SG require inbound rule of type NFSv4 (**N**etwork **F**ile **S**ystem, port 2049) with source SG WebDMZ
4. connect to the EC2 instances, and then mount EFS:

    ```shell
    cd /var/www/html
    cd ..
    mount -t efs -o tls fs-9816b269:/ /var/www/html   # this command comes from `EFS - Amazon EC2 mount instructions from local VPC`
    cd html
    echo "<html><body><h1>using EFS</h1></body></html>" > index.html
    ```

## EBS

AWS EBS (**E**lastic **B**lock **S**torage) is like a virtual hard disk in the cloud. This is a **block** level storage service for use with AWS EC2 and again has no connection to S3.

EBS cannot be shared by two EC2 instances

!!!danger "an Amazon EBS volume attached as an additional disk (not the root volume) can be detached without stopping the instance"

EBS Types:

* SSD (**S**olid **S**tate **D**rive)
	* GP2 - General Purpose
	* IO1 - Provisioned IOPS - Input Output per second - high perfrmance
* HDD (**H**ard **D**isk **D**rive)
	* ST1 - Throughtput optimised - Low cost for frequently access
	* SC1 - Cold HDD - Lowest cost for less frequently access
	* Magnetic - Previous generation

![](img/ebs.PNG)

HDD based volumes will always be less expensive than SSD types. 

EBS Pricing depends on:

* Volumes
* Snapshots
* Data transfer

## FSx

* AWS FSx for windows file server - MS Windows file system
    * Based on SMB (Windows **S**erver **M**essage **B**lock)
* AWS FSx for Lustre
    * Optimised file system
    * can store data on S3