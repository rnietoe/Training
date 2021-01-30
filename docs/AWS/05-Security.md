# 5. Security

## IAM 

**`AWS IAM`** (Identity and Access Management) enables you to manage access to AWS services and resources securely. 

* Users, groups, roles and policies are managed **globally** (not for a specific a region).
* **Identities** include users, groups, and roles. These are the IAM resource objects that are used to identify and group. You can attach a policy to an IAM identity. 
* A **Principal** is a person or application that uses the AWS account root user, an IAM user or an IAM role to sign in and make requests to AWS.
* You may have a 3rd party device that uses **BioMetrics** to initiate and exchange of the password or secret key with AWS, but that is not an AWS IAM service
* You can use permissions to allow and deny users and groups access to AWS resources.
    * Managed policies. Attach readonly policies already defined in AWS
    * Inline policies: Select a policy template, generate a policy, or create a custom policy. 
    * Not explicitly allowed = implicitly denied
    * **explicit deny > everything else** 
* Groups are a collection of users with specific permissions/policies
* Roles are a secure way to grant permissions to entities that you trust.
    1. `Create role` from IAM.
    2. Select `EC2` as trusted entity to call AWS services on your behalf.
    3. Attach permission policy `AmazonS3FullAccess`
    4. Named as S3_Admin_Access

    ```shell
    aws iam create-role --role-name DEV_ROLE --assume-role-policy-document file://mypolicy.json
    ```

!!!danger "To attach an IAM role to an instance that has no role, the instance can be in the stopped or running state. To replace the IAM role on an instance that already has an attached IAM role, the instance must be in the running state."

**Permissions boundary** define the maximum permissions an identity can have, but it does apply any permission

New users are assigned with Access Key Id and secret when first created to access AWS via the APIs and CLI.  

It's safer to use IAM roles than it is to use Access Keys.

**Power User Access** allows access to all AWS services except the management of groups and users within IAM.

!!!danger "New users have NO permissions when first created. When you manage IAM policies, follow the standard security advice of granting **the least privilege**, or granting only the permissions required to perform a task. Determine what users (and roles) need to do, and then craft policies that allow them to perform only those tasks."

A Policy is the document used to grant permissions to users, groups, and roles, but it can not be attached directly to an application. Policies are written using JSON.

AdministratorAccess Policy json:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

* IAM policies applies to users and groups. 
* IAM roles by another AWS account provide the aws link to other account. Switch role for **cross account** with Console access 
* Changes to IAM Policies take effect almost immediately (with maybe a few seconds delay).
* policy conditions determine when a policy applies

With the **IAM Policy Simulator**, you can test and troubleshoot identity-based policies, IAM permissions boundaries, Organizations service control policies, and resource-based policies.

root user tasks:

* modify the root user
* change the AWS support plan
* close an AWS account
* Create a CloudFront key pair
* Enable MFA on an S3 bucket
* Restore permissions for other iAM users

!!!danger "use user and pwd in the AWS console. Use key pair in the AWS API and CLI"

```shell
aws iam create-access-key --user-name Alice
aws iam list-access-key --user-name Alice
```

## ARN

Amazon Resource Name
    
    arn:partition:service:region:account_id:resouce_type:qualifier:resource:qualifier
* partition: aws|aws-cn
* service: s3|ec2|rds
* region: us-east-1|eu-central-1 (omitted when service is global such as iam)
* account_id: twelve digits (ommitted with service is globally unique, such as s3)
* resource_type: 
* resource: 
* qualifier:

## Security in the cloud 

**`AWS Artifact`**: [Compliance](https://aws.amazon.com/compliance/programs/) and security **reports** in the AWS Cloud

* A **PCI DSS Level 1** certification attests to the security of the AWS platform regarding **credit card transactions**.
* A **HIPAA** certification attests to the fact that the AWS Platform has met the standard required for the secure storage of **medical records** in the US
* A **ISO** certification for **quality**-controlled IT systems in the AWS cloud

[Shared responsability model](https://aws.amazon.com/compliance/shared-responsibility-model/) talks about who is responsible for what in cloud

![](img/Shared_Responsibility_Model_V2.jpg)

* AWS is responsible for the security **OF** the cloud. 
* Customer is responsible for security **IN** the cloud
* Customer is responsible of patching EC2 instances. 
* The customer would be responsible for patching the Operating System for IaaS solutions
* Encryption is a shared responsability

!!!tip "If you can do in the aws console, you are the responsible"

* **`Amazon Inspector`** to anayze and report security issues on EC2, but it can not examine individual policies
* **`Trusted Advisor`** for **recomendations** and advices (not only EC2 instances). It helps you optimize cost, fault-tolerance, and more.
* **`CloudTrail`** track user **activity** and API usage
* **`CloudWatch`** monitoring **performance**
* **`AWS Config`** monitor **configuration** settings
* **`Athena`** serverless service for **querying** data in S3 using SQL. Commonly used to analyse logs. It will work with a number of data formats including JSON, Apache Parquet, Apache ORC amongst others, but **XML** is not a format that is supported. 
* **`Macie`** uses Machine learning to protect **sensitive data** (Personally Identifiable Information) stored in S3

* `Personal Health Dashboard` helps you to inspect account alerts and find remediation guidance for your account
* `Service Health Dashboard` displays the general status of AWS services

## WAF (Web Application Firewall)

**`AWS WAF`** **block** requests to stop hackers requests from:

* specific IP address (CloudFront, ALB or API Gateway)
* origin country
* request size
* request header values
* SQL code injection
* Cross scripting

It works with CloudFront or ELB

It operates down to Layer 7 (http/https).

* allow all requests except specified
* block all requests except specified

Block traffic response is **forbbiden** (403)

## Shield

**`AWS Shield`** protect a lot of traffic (**DDOS** attacks).

* AWS Shield Standard: free protection in layers 3 and 4
* AWS Shield Advanced: advance protection costs $3000/month. It offers automated application layer monitoring. 

## Directory Service

There are three options that help you migrate **Active Directory**-dependent applications to the AWS Cloud: 

* Managed Microsoft AD - **Directory Service** for Microsoft Active Diretory
* **AD Connector**
* **Simple AD**

These solutions also enable users to sign into AWS applications such as Amazon WorkSpaces and Amazon QuickSight with their AD credentials. Developers who don’t need AD can use Amazon **Cloud Directory** to create cloud-scale directories that organize and manage hierarchical information such as organizational charts, course catalogs, and device registries. 

## RAM (Resource Access Management)

**`AWS RAM`** shares AWS resources with other AWS accounts.

1. Create a resource share
2. Choose the resources type to add to the resource share
3. Select the resource id
4. Add principals to the resource share. Principals can be AWS accounts, organizational units, or your organization.
5. Add tags to the resource share. 

from the other account, browse to `AWS RAM`: `Shared with me` : `Resource shares` and accept the shared resource

## SSO (Single Sign-On)

**`AWS SSO`** is a cloud service that makes it easy to manage SSO access to multiple AWS accounts and business applications.

this require Active Directory and **SAML** (Security Assertion Markup Language) integration

## Cognito

**`AWS Cognito`** is the SSO Solution in AWS providing Web Identity Federation (auth using **Facebook**, **Google** and Amazon) recomended for mobile apps log in

store user profiles

based on open standards as **OAuth2.0**, SAML2.0, OpenID Connect

1. users pool: **JWT** (Json Web Token) for registration, authentication and account recovery
2. identity pool: temporary AWS credentials to access AWS recources

Cognito uses Push synchronization to push updates and synchronize user data across multiple devices

## KMS (Key Management Service)

**`AWS KMS`** manages encryption keys to encrypt data

CMK (Customer Master Key) per region

* AWS Managed CMK: used by default; free
* AWS Owned CMK: used by AWS
* Customer Managed CMK: created by you. it allows **key rotation**: replace old access key

Symmetric CMK

* default option
* same key for encryption and decryption
* AES-256 algoritm
* data never leaves AWS unencrypted
* KMS must be called for using

Asymmetric CMK

* Public and Private key pair
* SSL
* RSA and ECC algoritm
* private key never leaves AWS unencrypted
* AWS services integrated with KMS does not support asymmetric CMKs

```shell
aws kms create-key --description "MyCMK"
# allow every action to the root user by default
aws kms create-alias --target-key-id KEYID --alias-name "alias/MyCMK"
echo "hello world" > test.txt
aws kms encrypt --key-id KEYID "alias/MyCMK" --plaintext file://test.txt --output text --query CiphertextBlob
aws kms encrypt --key-id KEYID "alias/MyCMK" --plaintext file://test.txt --output text --query CiphertextBlob | base64 --decode > test.txt.encrypted

aws kms decrypt --ciphertext-blob fileb://test.txt.encrypted --output text --query Plaintext 
aws kms decrypt --ciphertext-blob fileb://test.txt.encrypted --output text --query Plaintext | base64 --decode
```

 * FIPS 140-2 Level 2
 * KMS is multitenant

## CloudHSM (Hardware Security Model)

* FIPS 140-2 Level 3
* single tenant (dedicated hardware) 

manage your own keys

run within a VPC in your account 

## Secrets Manager

AWS Secrets Manager helps you protect access to your applications, services, and IT resources. You can easily **rotate**, manage, and retrieve database credentials, API keys, and other secrets throughout their lifecycle.

generate random secrets

## Security Hub

**`AWS Security Hub`** is a not free service that provides a consolidated view of your security status in AWS:

this runs automatic checks to scan for compliance with regulations and laws

* **`AWS GuardDuty`** is a IDS (intrusion detection system)
* **`AWS Inspector`** performs vulnerability analysis
* **`AWS Macie`** provides S3 bucket policy compliance scans for sensitive data