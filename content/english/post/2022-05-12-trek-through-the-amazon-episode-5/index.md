---
title: Trek through the Amazon, Episode 5
author: 'Jeonghoon Lee'
summary: The fifth episode to my journey through learning about Amazon Web Services
date: '2022-05-12'
slug: []
categories: []
tags: []
Description: ''
Tags: [AWS, Cloud, Programming]
Categories: [AWS, Cloud, Programming]
DisableComments: no
---

In the previous episode, we learned about creating a Virtual Private Cloud (VPC) through the New VPC Experience. Using the Management Console and the Launch Wizard can be super easy. But, we also want to be able to practice Infrastructure as Code (IaC) to move on to automation and reduce the possibility of human error.

First let's launch our Command (if you're on Windows). We have to configure the AWS profile based on your account settings:

> aws configure

After you've inputted your Access Key ID, Secret Access Key, and default region name, create the VPC:

> aws ec2 create-vpc \--cidr-block 10.0.0.0/16 \--query Vpc.VpcId \--output text

Make a note of the VPC ID as we will need it for later commands. This VPC should reside in your default region and come with default tenancy. Don't forget to enable DNS hostnames:

> aws ec2 modify-vpc-attribute \--vpc-id *vpc-identifier* \--enable-dns-hostnames "{\\"Value\\":true}"

Let's make four subnets within this VPC. Two should reside in one availability zone (AZ) and two should reside in another:

> aws ec2 create-subnet \--vpc-id *vpc-identifier* \--cidr-block 10.0.0.0/24 \--availability-zone us-east-1a \--query Subnet.SubnetId \--output text

> aws ec2 create-subnet \--vpc-id *vpc-identifier* \--cidr-block 10.0.1.0/24 \--availability-zone us-east-1a \--query Subnet.SubnetId \--output text

> aws ec2 create-subnet \--vpc-id *vpc-identifier* \--cidr-block 10.0.2.0/24 \--availability-zone us-east-1b \--query Subnet.SubnetId \--output text

> aws ec2 create-subnet \--vpc-id *vpc-identifier* \--cidr-block 10.0.3.0/24 \--availability-zone us-east-1b \--query Subnet.SubnetId \--output text

Enable auto-assign public IP for your two public subnets:

> aws ec2 modify-subnet-attribute \--subnet-id *subnet-identifier* \--map-public-ip-on-launch

Make an internet gateway:

> aws ec2 create-internet-gateway \--query InternetGateway.InternetGatewayId \--output text

Attach this internet gateway to your VPC:

> aws ec2 attach-internet-gateway \--vpc-id *vpc-identifier* \--internet-gateway-id *igw-identifier*

Create a custom route table that can be associated with the internet gateway:

> aws ec2 create-route-table \--vpc-id *vpc-identifier* \--query RouteTable.RouteTableId \--output text

Create a route to the internet gateway in your route table:

> aws ec2 create-route \--route-table-id *rtb-identifier* \--destination-cidr-block 0.0.0.0/0 \--gateway-id *igw-identifier*

Associate the two public subnets with the route table, making them publicly accessible:

> aws ec2 associate-route-table \--subnet-id *subnet-identifier* \--route-table-id *rtb-identifier*

Let's modify the default security group so that EC2 instances launched on public subnets can only be accessed through HTTP on Port 80. First, delete the pre-existing ingress rule:

> aws ec2 revoke-security-group-ingress \--group-id *sg-identifier* \--security-group-rule-ids *sgr-identifier*

Now, let's make our HTTP ingress rule:

> aws ec2 authorize-security-group-ingress \--group-id *sg-identifier* \--protocol tcp \--port 80 \--cidr 0.0.0.0/0

We have all the necessary components to our VPC as well as the same residencies and associations as the VPC created in the previous episode. Now, let's delete our newly-created VPC and its parts:

> aws ec2 delete-subnet \--subnet-id *subnet-identifier*

> aws ec2 delete-route-table \--route-table-id *rtb-identifier*

> aws ec2 detach-internet-gateway \--internet-gateway-id *igw-identifier* \--vpc-id *vpc-identifier*

> aws ec2 delete-internet-gateway \--internet-gateway-id *igw-identifier*

> aws ec2 delete-vpc \--vpc-id *vpc-identifier*

I hope this tutorial helped you in understanding the process behind creating a VPC using the command line.

In the next episode, I will be going through launching an EC2 instance in your VPC and installing an Apache Test Web Server. I will also discuss why I have neglected adding Port 22 (SSH access) to the VPCs and explain why AWS Systems Manager may be more beneficial than SSH going forward.