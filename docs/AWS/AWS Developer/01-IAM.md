# 1. IAM (Identity & Access Management)

Users, groups, roles and policies (json document which defines one or more permissions) are managed **globally** , instead for a specific a region).

Least privilege (pesimist) security: denies override allows

1. Create account with `pauniecas@gmail.com` and add MFA for root user

    * You can use password rotation
    * You may have a 3rd party device that uses **BioMetrics** to initiate and exchange of the password or secret key with AWS, but that is not an AWS IAM service

2. Create `developer` user (AKIAQKS24ZJGHHB23MVF)

    !!!Tip "Use username and password in the AWS console. Use key pair in the AWS API and CLI"

3. Create `sysadmin` group with AdminstratorAccessPolicy (full access)

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

4. Create account alias as pauniecas: [https://pauniecas.signin.aws.amazon.com/console](https://pauniecas.signin.aws.amazon.com/console)
5. Create a role with full access from EC2 to S3 using policy `AmazonS3FullAccess`

## IAM policy simulator
[IAM Policy Simulator](https://policysim.aws.amazon.com)

[Testing IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html)



