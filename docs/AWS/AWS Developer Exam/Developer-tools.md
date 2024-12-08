# Developer tools

https://digitalcloud.training/aws-developer-tools/

## AWS CodeDeploy 

AWS CodeDeploy can deploy software packages using an archive that has been uploaded to an Amazon S3 bucket. The archive file will typically be a .zip file containing the code and files required to deploy the software package.

The application specification file (AppSpec file) is a YAML-formatted or JSON-formatted file used by CodeDeploy to manage a deployment. The name of the AppSpec file for an EC2/On-Premises deployment must be **appspec.yml** or **appspec.yaml**

## AWS CodeBuild

**buildspec.yml** defines the build instructions

## X-Ray

Analyze and debug your application providing an end-to-end view of requests as they travel through your application and shows a map of your application’s underlying components.

You can record additional information about requests, the environment, or your application with 
* `Annotations`: key-value pairs with string, number, or Boolean values. Annotations are indexed for use with filter expressions.
* `Metadata`: key-value pairs that can have values of any type, including objects and lists, but are not indexed for use with filter expressions. Use metadata to record additional data that you want stored in the trace but don't need to use with search.
* `Subsegments` provide more granular timing information and details about downstream calls that your application made to fulfill the original request.
* `filter expressions` to find traces related to specific paths or users.