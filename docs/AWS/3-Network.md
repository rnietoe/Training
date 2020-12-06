# 3. Network

* **ENI** - **E**lastic **N**etwork **I**nterface - virtual network card for basic networking
* **ENA** - **E**nhanced **N**etworking **A**dapter - use SR-IOV (**S**ingle **R**oot **I/O V**irtualization) to allow speeds between 10 and 100 Gbps requirement
* **EFA** - **E**lastic **F**abric **A**dapter - machine learning or HPC (**H**igh **P**erformance **C**omputing) requirement

## VPC

**`AWS VPC`** (**V**irtual **P**rivate **C**loud) is a **virtual network** dedicated to a single AWS account. VPC lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resources. You have complete control over your virtual networking environment, including the selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways. You can use both IPv4 and IPv6 in your VPC for secure and easy access to resources and applications.

**VPC peering** creates a connection between two VPCs. 

A **VPG** (**V**irtual **P**rivate **G**ateway) is a logical, fully redundant distributed edge routing function that sits at the edge of your VPC. As it is capable of terminating VPN connections from your on-prem or customer environments, the VPG is the VPN concentrator on the Amazon side of the Site-to-Site VPN connection.

An **internet gateway** is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet. An internet gateway serves two purposes: to provide a target in your VPC route tables for internet-routable traffic, and to perform network address translation (NAT) for instances that have been assigned public IPv4 addresses

An **elastic network interface** is a logical networking component in a VPC that represents a virtual network card. It can include multiple attributes, such as security groups, IPv6 and IPv4 addresses, MAC addresses, and more.

## Route 53

**`AWS Route 53`** service name comes from port 53, where DNS work on 

we can register a DNS using `Route53` - `Register domain`. You can purchase and manage domain names such as example.com, and Route 53 will automatically configure DNS settings for your domains

!!!tip "Ensure there is a free bucket with the same domain name"

* **Failover Routing policy** routes data to a second resource if the first is unhealthy. Route 53 can be used for Disaster Recovery by simply shifting traffic to the new region.
* **Latency-based Routing policy** routes data to resources that have better performance

**Route 53 Traffic Flow** makes it easy for you to manage traffic globally through a variety of routing types, including Latency Based Routing, Geo DNS, Geoproximity, and Weighted Round Robin—all of which can be combined with DNS Failover to enable a variety of low-latency, fault-tolerant architectures. Using Route 53 Traffic Flow’s simple visual editor, you can easily manage how your end-users are routed to your application’s endpoints—whether in a single AWS region or distributed around the globe. 
