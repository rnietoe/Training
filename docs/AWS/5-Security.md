# 5. Security

## IAM 

**`AWS IAM`** (**I**dentity and **A**ccess **M**anagement) enables you to manage access to AWS services and resources securely. 

* Users, groups, roles and policies are managed **globally** and not for a specific a region.
* You may have a 3rd party device that uses **BioMetrics** to initiate and exchange of the password or secret key with AWS, but that is not an AWS IAM service
* You can use permissions to allow and deny users and groups access to AWS resources.
    * Managed policies. Attach readonly policies already defined in AWS
    * Inline policies: Select a policy template, generate a policy, or create a custom policy. 
    * Not explicitly allowed = implicitly denied
    * explicit deny > everything else 
* Groups are a collection of users with specific permissions/policies
* Roles are a secure way to grant permissions to entities that you trust.
    1. `Create role` from IAM.
    2. Select `EC2` as trusted entity to call AWS services on your behalf.
    3. Attach permission policy `AmazonS3FullAccess`
    4. Named as S3_Admin_Access

    ```shell
    aws iam create-role --role-name DEV_ROLE --assume-role-policy-document file://mypolicy.json
    ```

    * To attach an IAM role to an instance that has no role, the instance can be in the stopped or running state. 
    * To replace the IAM role on an instance that already has an attached IAM role, the instance must be in the running state.

* **Identities** include users, groups, and roles. These are the IAM resource objects that are used to identify and group. You can attach a policy to an IAM identity. 
* A **Principal** is a person or application that uses the AWS account root user, an IAM user, or an IAM role to sign in and make requests to AWS.

New users are assigned Access Key Id and secret when first created to access AWS via the APIs and CLI.  

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

With the **IAM Policy Simulator**, you can test and troubleshoot identity-based policies, IAM permissions boundaries (delegate permissions to other user), Organizations service control policies, and resource-based policies.

**Permissions boundary** define the maximum permissions an identity can have

ARN (**A**mazon **R**esource **N**ame)
    
    arn:partition:service:region:account_id:resouce_type:qualifier:resource:qualifier
* partition: aws|aws-cn
* service: s3|ec2|rds
* region: us-east-1|eu-central-1 (omitted when service is global such as iam)
* account_id: twelve digits (ommitted with service is globally unique, such as s3)
* resource_type: 
* resource: 
* qualifier:

## Security in the cloud 

**`AWS Artifact`**: [Compliance](https://aws.amazon.com/compliance/?nc1=h_ls) and security **reports** in the AWS Cloud

* A **PCI DSS Level 1** certification attests to the security of the AWS platform regarding **credit card transactions**.
* A **HIPAA** certification attests to the fact that the AWS Platform has met the standard required for the secure storage of **medical records** in the US

"AWS is responsible for the security **OF** the cloud. The customer is responsible for security **IN** the cloud ". Customer is responsible of patching EC2 instances. If you can do in the aws console, you are the responsible

Encryption is a shared responsability

[Shared responsability model](https://aws.amazon.com/compliance/shared-responsibility-model/) talks about who is responsible for waht in cloud

![](img/Shared_Responsibility_Model_V2.jpg)

The customer would be responsible for patching the Operating System for IaaS solutions

* **`Shield`** protect a lot of traffic (DDOS attacks). Only AWS Shield Advanced offers automated application layer monitoring. This costs $3000/month
* **`Inspector`** to anayze and report security issues on EC2, but it can not examine individual policies
* **`Trusted Advisor`** for recomendations and advices (not only EC2 instances). It helps you optimize cost, fault-tolerance, and more.
* **`CloudTrail`** track user **activity** and API usage
* **`CloudWatch`** monitoring **performance**
* **`Config`** monitor **configuration** settings
* **`Athena`** serverless service for **querying** data in S3 using SQL. Commonly used to analyse logs. It will work with a number of data formats including JSON, Apache Parquet, Apache ORC amongst others, but XML is not a format that is supported. 
* **`Macie`** uses Machine learning to protect **sensitive data** (**P**ersonally **I**dentifiable **I**nformation) stored in S3
* **`Kinesis`** work with Real-Time Streaming Data

* `Personal Health Dashboard` helps you to inspect account alerts and find remediation guidance for your account
* `Service Health Dashboard` displays the general status of AWS services

It's safer to use IAM roles than it is to use Access Keys.

## WAF 

**`AWS WAF`** (**W**eb **A**pplication **F**irewall) **block** request from specific IP address to stop hackers requests. It operates down to Layer 7.

## Directory Service

There are three options that help you migrate **Active Directory**-dependent applications to the AWS Cloud: 

* Managed Microsoft AD - **Directory Service** for Microsoft Active Diretory
* **AD Connector**
* **Simple AD**

These solutions also enable users to sign into AWS applications such as Amazon WorkSpaces and Amazon QuickSight with their AD credentials. Developers who don’t need AD can use Amazon **Cloud Directory** to create cloud-scale directories that organize and manage hierarchical information such as organizational charts, course catalogs, and device registries. Amazon **Cognito** user pools offer mobile and web application developers Internet-scale user directories with integrated sign-up and sign-in.

## RAM

**R**esource **A**ccess **M**anagement shares AWS resources with other AWS accounts.

1. Create a resource share
2. Choose the resources type to add to the resource share
3. Select the resource id
4. Add principals to the resource share. Principals can be AWS accounts, organizational units, or your organization.
5. Add tags to the resource share. 

from the other account, browse to `Resource Access Manager`: `Shared with me` : `Resource shares` and accept the resource share

## SSO

AWS **S**ingle **S**ign-**O**n is a cloud service that makes it easy to manage SSO access to multiple AWS accounts and business applications.

this require Active Directory and **SAML** (**S**ecurity **A**ssertion **M**arkup **L**anguage) integration