## Storage

# S3

You can protect data in transit using:
* Secure Socket Layer/Transport Layer Security (SSL/TLS)
* client-side encryption. 

You can protect data  at rest in Amazon S3:
* Server-Side Encryption
* Client-Side Encryption – Encrypt data client-side and upload the encrypted data to Amazon S3. In this case, you manage the encryption process, the encryption keys, and related tools.

When using a private subnet with no Internet connectivity there are only two options available for connecting to Amazon S3 (which remember, is a service with a public endpoint, it’s not in your VPC):
1. enable Internet connectivity through either a NAT Gateway or a NAT Instance. 
2. The other option is to enable a VPC endpoint (Gateway Endpoint) for S3.

You can then use an S3 bucket policy to indicate which VPCs and which VPC Endpoints have access to your S3 buckets.