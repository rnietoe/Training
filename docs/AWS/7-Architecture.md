# Hight Availability Architecture

a web site requires a minimum of 6 instances and tolerate the failaure of 1 AZ. the most cost effective environment is 3 AZ with 3 instances in each AZ. if 1 AZ fails, there are still 6 instances...

## Elastic Load Balancers

[Elastic Load Balancing FAQs](https://aws.amazon.com/elasticloadbalancing/faqs/?nc1=h_ls)

!!!danger "ELB can spread load across AZs not regions."

* ALB (**A**pplication **L**oad **B**alancers) for intelligent routing - HTTP/HTTPS
* NLB (**N**etwork **L**oad **B**alancers) for extreme performance and static IPs - TCP/TLS
* CLB (**C**lassic **L**oad **B**alancers) for test and dev - low cost - HTTP/HTTPS/TCP

Error **504** means gateway timeout

**X-Forwarded-For** header is used to get the IPv4 address of end users

How to use classic load balancer

1. `Create load balancer` from EC2 : Load Balancers of type CLB
2. [Register EC2 instances](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-backend-instances.html)
3. Select our Security Group (virtual firewall)
7. Browse to the ELB (**E**lastic **L**oad **B**alance) DNS name and see the result, instead of browsing to the EC2 IP address

How to use application load balancer

1. `Create target group` to group instances, IP addresses or Lamda functions for load balancing
2. `Create load balancer` of type ALB
3. Configure Load Balancer with name and select every AZ

    !!!danger "At least two public subnets are required to enable the LB"

4. Configure Routing with a Target Troup name and the following health check settings:
	* healthy threshold: 3 times
	* unhealthy threshold: 3 times
	* timeout: 3 seconds
	* interval: 5 seconds
	* success code: 200
5. Register target adding our EC2 instances (to registered)
6. Review and create

**sticky sessions** bind a **user's session** to a specific EC2 instance. All user requests during the session are sent to the same intance (CLB) or target group (ALB)

**cross zone load balancing** allow ELB to send traffic to **another AZ**

**path patterns**  are listener with rules to foward requests based on the **URL path**

## Autoscaling 

* Groups
* Configuration Templates
* Scaling Options
    * To mantaint current instance levels at all times  (for example, 10 EC2 instances)
    * Scale manually
    * Scale based on a schedule
    * Scale based on demand using scaling polices
    * Use predictive scaling based on performance

1. `Create launch configuration` to create a saved instance configuration from EC2 : AUTO SCALING : Launch Configurations
2. `Create Auto Scaling group` to create a collection of Amazon EC2 instances that are treated as a logical unit

**EC2 Autoscaling** works in conjunction with the **AWS Autoscaling** service to provide a predictive ability to your autoscaling groups.

