# AWS Solutions Architect Associate

[AWS Well-Architected Framework](https://wa.aws.amazon.com/index.en.html):

* [Reliability](https://wa.aws.amazon.com/wat.pillar.reliability.en.html). [Disaster Recovery Scenarios(https://jayendrapatil.com/aws-disaster-recovery-whitepaper/)]:
    * **Backup and restore** (RPO in hours, RTO in 24 hours or less): Back up your data and applications using point-in-time backups into the DR Region. Restore this data when necessary to recover from a disaster.
    * **Pilot light** (RPO in minutes, RTO in hours): Replicate your data from one region to another and provision a copy of your core workload infrastructure. Resources required to support data replication and backup such as databases and object storage are always on. Other elements such as application servers are loaded with application code and configurations, but are switched off and are only used during testing or when Disaster Recovery failover is invoked.
    * **Warm standby** (RPO in seconds, RTO in minutes): Maintain a scaled-down but fully functional version of your workload always running in the DR Region. Business-critical systems are fully duplicated and are always on, but with a scaled down fleet. When the time comes for recovery, the system is scaled up quickly to handle the production load. The more scaled-up the Warm Standby is, the lower RTO and control plane reliance will be. When scaled up to full scale this is known as a **Hot Standby**.
    * **Multi-region** (multi-site) active-active (RPO near zero, RTO potentially zero): Your workload is deployed to, and actively serving traffic from, multiple AWS Regions
* [Performance Efficiency](https://wa.aws.amazon.com/wat.pillar.performance.en.html)
* [Security](https://wa.aws.amazon.com/wat.pillar.security.en.html)
* [Cost Optimization](https://wa.aws.amazon.com/wat.pillar.costOptimization.en.html)
* [Operational Excellence](https://wa.aws.amazon.com/wat.pillar.operationalExcellence.en.html)

[https://rnietoe.signin.aws.amazon.com/console](https://rnietoe.signin.aws.amazon.com/console)

* rnietoe@gmail.com - AWS account (root account)
* rnietoe/training
* rnietoe/paying
* Google Authenticator for Android - **MFA** (Multi-Factor Authentication)

Book your exam [here](https://www.aws.training/certification)

1. [Introduction](01-Introduction.md)
2. [Storage](02-Storage.md)
3. [Network](03-Network.md)
4. [Compute](04-Compute.md)
5. [Security](05-Security.md)
6. [Management](06-Management.md)
    6. [CloudFormation](CloudFormation/06-CloudFormation.md)
7. [HA Architecture](07-Architecture.md)
8. [Databases](08-Databases.md)
9. [Applications](09-Applications.md)
10. [Media](10-Media.md)

## ACloud.Guru Final Practice Exam  
[https://aws.amazon.com/certification/certification-prep/](https://aws.amazon.com/certification/certification-prep/)

• Step 1: Take an AWS Training Class  
• Step 2: Review the Exam Guide and Sample Questions  
• Step 3: Practice with Self-Placed Labs and an Exam Prep Quest  
• Step 4: Study AWS Whitepapers  
• Step 5: Review AWS FAQs  
• Step 6: Take an Exam Prep Workshop  
• Step 7: Take a Practice Exam  
• Step 8: Schedule Your Exam and Get Certified  

[https://www.qwiklabs.com/](https://www.qwiklabs.com/)

## AWS Documentation
[https://docs.aws.amazon.com/index.html?nc2=h_ql_doc_do](https://docs.aws.amazon.com/index.html?nc2=h_ql_doc_do)  
[https://aws.amazon.com/faqs/](https://aws.amazon.com/faqs/)  
[https://aws.amazon.com/whitepapers/?whitepapers-main.sort-by=item.additionalFields.sortDate&whitepapers-main.sort-order=desc](https://aws.amazon.com/whitepapers/?whitepapers-main.sort-by=item.additionalFields.sortDate&whitepapers-main.sort-order=desc)    
[https://www.youtube.com/c/amazonwebservices/videos](https://www.youtube.com/c/amazonwebservices/videos)  
[https://aws.amazon.com/architecture/](https://aws.amazon.com/architecture/)  
[https://aws.amazon.com/new/](https://aws.amazon.com/new/)  
[https://www.meetup.com/es/topics/amazon-web-services/](https://www.meetup.com/es/topics/amazon-web-services/)  
[AWS Certified Solutions Architect Associate Practice Exams](https://www.udemy.com/course/aws-certified-solutions-architect-associate-practice-tests-k/)

## AWS Exam  
[https://aws.amazon.com/certification/faqs/](https://aws.amazon.com/certification/faqs/)  
[https://aws.amazon.com/certification/certified-solutions-architect-associate/](https://aws.amazon.com/certification/certified-solutions-architect-associate/)

AWS Cloud Practitioner Essentials (Second Edition) (Spanish)
[https://www.aws.training/Details/Curriculum?id=46152](https://www.aws.training/Details/Curriculum?id=46152)

https://jayendrapatil.com/aws-certified-solutions-architect-associate-saa-c02-exam-learning-path/

https://www.examtopics.com/exams/amazon/aws-certified-solutions-architect-associate-saa-c02/view/
