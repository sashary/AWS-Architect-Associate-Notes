# AWS VPC - **V**irtual **P**rivate **N**etwork

## Sections
- [Anatomy of an Amazon VPC](anatomy-of-amazon-vpc.md)
- [VPC Network Architectures](vpc-network-architectures.md)


## Overview
- AWS VPC is a virtual network in a sense that the geographically distributed servers are connected to each other to form a single network.
    - Regional in scope, meaning your VPC can only exist within one AWS region.
    - Works like a traditional network - has a CIDR block, main route table, multiple subnets, and other external network connections.
        - Unlike the traditional network, AWS VPC can leverage a global network at a fraction of the cost.
- Not limited to a single data center : can span to multiple data centers and availability zones within an AWS region.
- A virtual network can be subdivided by two or more subnetworks (subnets).
    - In AWS, a subnet must reside entirely within one availability zone only (cannot span to two or more AZs)
        - You can have multiple subnets in the same availability zone however. This is because an availability zone is just composed of one or more datacenters.
- If one server needs access to another server that resides in another subnet, a main route table needs to be configured to connect them.
    - By default, all subnets in your Amazon VPC are interconnected.
    - Custom route tables can be created that you can associate with your subnet.
- A public or private VPC can be created for your VPC.
    - A VPC is private by default.
    - A public subnet is perfect for subnets that are meant to be publicly accessed over the internet.
    - A private subnet is best for backend systems like DBs or application servers with no internet access.
    - You can set up security groups and network access controls to manage the incoming traffic going in and out of your Amazon EC2 instances (hosted in the VPC).