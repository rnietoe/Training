# 7. Hight Availability Architecture

When a web site requires a minimum of 6 EC2 and tolerate the failaure of 1 AZ, the most cost effective environment is 3 AZ with 3 EC2s in each AZ. if 1 AZ fails, there are still 6 EC2 instances.

## Elastic Load Balancers

[Elastic Load Balancer](https://aws.amazon.com/elasticloadbalancing/faqs/?nc1=h_ls) implements Dynamic Load Balancer

Load Balancing algorithms:

* Round Robin: first request to first server, second request to second server...
* Randomized
* Centrally managed, based on defined conditions
* threshold-based: all request to a server till a defined limit (threshold)

[ELB types](https://aws.amazon.com/elasticloadbalancing/features/?nc1=h_ls):

* ALB (**Application** Load Balancers) for intelligent routing - HTTP/HTTPS (layer 7)
* NLB (**Network** Load Balancers) for extreme performance and static IPs - TCP/TLS (layer 4)
	* You can passthrough encrypted traffic with an NLB and terminate the SSL on the EC2 instances as all data in transit encryption requirement
* CLB (**Classic** Load Balancers) for classic/**old/legacy** EC2 instantes or test/dev environments - low cost - HTTP/HTTPS/TCP (layers 4 and 7)
* GLB (**Gateway** Load Balancers) to deploy, scale, and manage your third-party **virtual appliances**

supported services:

* EC2
* ECS
* Auto Scaling
* CloudWatch
* Route53

some features:

* **X-Forwarded-For** header is used to get the IPv4 address of end users
* **sticky sessions** bind a **user's session** to a specific EC2 instance. All user requests during the session are sent to the same intance (CLB) or target group (ALB)
* **cross zone load balancing** allow ELB to send traffic to **another AZ**
    
    !!!danger "ELB can spread (propagar) load across AZs not regions."

* **path patterns**  are listener with rules to foward requests based on the **URL path**
* Error **504** means gateway timeout

How to use classic load balancer

1. `Create load balancer` from EC2 : Load Balancers of type CLB
2. [Register EC2 instances](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-backend-instances.html)
3. Select our Security Group (virtual firewall)
7. Browse to the **ELB** (Elastic Load Balance) DNS name and see the result, instead of browsing to the EC2 IP address

How to use application load balancer

1. `Create target group` to group EC2 instances, IP addresses or Lambda functions for load balancing
2. `Create load balancer` of type ALB
3. Configure Load Balancer with name and select every AZ

    !!!danger "At least two public subnets are required to enable the LB"

4. Configure Routing with a Target Group name and the following health check settings:
	* healthy threshold: 3 times
	* unhealthy threshold: 3 times
	* timeout: 3 seconds
	* interval: 5 seconds
	* success code: 200
5. Register target adding our EC2 instances (to registered)
6. Review and create

## Auto Scaling 

1. (required) `Create launch configuration` to create a saved instance configuration for the new EC2 instances
2. `Create Auto Scaling group` to create a collection of similar EC2 instances that are treated as a logical unit, based on its AMI
3. select all VPC subnets for multiAZ
4. metrics are:
	* CPU utilization
	* **DiskReadOps**
	* network-in 
	* network-out
5. new instances are launched

Creating an Auto Scaling group using: 

* a launch template: needed to use Dedicated Hosts and more features that can't be achieved with launch configurations.
* a launch configuration
* the Amazon EC2 launch wizard
* using an EC2 instance

Scaling Options

* To mantain current instance levels at all times  (for example, 10 EC2 instances)
* Scale manually
* scake based on healthchecks
* Scale based on a schedule: Scaling actions are performed automatically as a function of time and date
* Scale based on demand using scaling polices
* Use predictive scaling based on performance

**EC2 Autoscaling** works in conjunction with the **AWS Autoscaling** service to provide a predictive ability to your autoscaling groups.

[**Target tracking** scaling policies for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scaling-target-tracking.html) is more cost effective than using scheduled actions

A scaling **cooldown** helps you prevent your EC2 Auto Scaling group from launching or terminating additional instances before the effects of previous scaling activities are visible.

!!!error "the error "you must use a fully formed launch template" means the launch template is missing some information" 

!!!error "a second error can be not having enough permssions to launch EC2 instances..."
