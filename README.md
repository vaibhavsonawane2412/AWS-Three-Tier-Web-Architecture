# AWS-Three-Tier-Web-Architecture
This repository provides a hands-on introduction to building scalable and available web applications on AWS using a three-tier architecture.
# Project Overview

This project outlines the setup of a three-tier architecture on AWS, comprising a presentation layer (frontend), an application layer (backend), and a data layer (database). The purpose of this architecture is to host and deploy a web application using various AWS services. The technology stack being utilized includes Amazon EC2 for virtual servers, Amazon RDS for database management, Amazon S3 for hosting static content, and other AWS services for networking and scalability.

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Setting Up the VPC and Subnets](#setting-up-the-vpc-and-subnets)
3. [Launching EC2 Instances](#launching-ec2-instances)
4. [Configuring the Web Tier](#configuring-the-web-tier)
5. [Configuring the Application Tier](#configuring-the-application-tier)
6. [Configuring the Database Tier](#configuring-the-database-tier)
7. [Connecting the Tiers](#connecting-the-tiers)
8. [Conclusion](#conclusion)

## Prerequisites
- An AWS account
- Basic understanding of AWS services
- AWS CLI installed on the local machine

## Architecture Overview
![Architecture Diagram](https://github.com/aws-samples/aws-three-tier-web-architecture-workshop/blob/main/application-code/web-tier/src/assets/3TierArch.png)

In this architecture, a public-facing Application Load Balancer forwards client traffic to our web tier EC2 instances. The web tier is running Nginx webservers that are configured to serve a React.js website and redirects our API calls to the application tier’s internal facing load balancer. The internal facing load balancer then forwards that traffic to the application tier, which is written in Node.js. The application tier manipulates data in an Aurora MySQL multi-AZ database and returns it to our web tier. Load balancing, health checks and autoscaling groups are created at each layer to maintain the availability of this architecture.


## Setting Up the VPC and Subnets
1. **Log in to your AWS Management Console.**
2. **Navigate to the VPC dashboard.**
3. **Create a new VPC with a unique CIDR block.**
   - Command:
     ```
     aws ec2 create-vpc --cidr-block 10.0.0.0/16
     ```
4. **Create three subnets within the VPC for the web, application, and database tiers, each in a different Availability Zone.**
   - Commands:
     ```
     aws ec2 create-subnet --vpc-id <VPC_ID> --cidr-block 10.0.1.0/24 --availability-zone us-east-1a
     aws ec2 create-subnet --vpc-id <VPC_ID> --cidr-block 10.0.2.0/24 --availability-zone us-east-1b
     aws ec2 create-subnet --vpc-id <VPC_ID> --cidr-block 10.0.3.0/24 --availability-zone us-east-1c
     ```
5. **Ensure appropriate route tables and internet gateways for the subnets.**

## Launching EC2 Instances
1. **Go to the EC2 dashboard.**
2. **Launch an EC2 instance for each tier, ensuring they are in the respective subnets.**
   - Command:
     ```
     aws ec2 run-instances --image-id <AMI_ID> --count 1 --instance-type t2.micro --subnet-id <SUBNET_ID>
     ```
3. **Configure security groups to allow necessary inbound and outbound traffic for each tier.**

## Configuring the Web Tier
1. **Connect to the web EC2 instance using SSH.**
   - Command:
     ```
     ssh -i <KEY_PAIR_FILE> ec2-user@<WEB_INSTANCE_PUBLIC_IP>
     ```
2. **Install a web server like Apache or Nginx.**
   - Command:
     ```
     sudo yum install -y httpd
     ```
3. **Configure the web server to serve your web application, e.g., a React.js website.**
4. **Set up the public-facing Application Load Balancer to forward client traffic to the web tier EC2 instances.**
   - Reference AWS Console for manual setup.
5. **Configure health checks and enable autoscaling for the web tier.**
   - Reference AWS Console for manual setup.

## Configuring the Application Tier
1. **Connect to the application EC2 instance using SSH.**
   - Command:
     ```
     ssh -i <KEY_PAIR_FILE> ec2-user@<APP_INSTANCE_PUBLIC_IP>
     ```
2. **Install Node.js and deploy the application code.**
   - Commands:
     ```
     sudo yum install -y nodejs npm
     git clone <REPO_URL>
     npm install
     npm start
     ```
3. **Create an internal-facing Application Load Balancer for the application tier and configure it to forward traffic to the Node.js servers.**
   - Reference AWS Console for manual setup.
4. **Implement health checks and autoscaling for the application tier.**
   - Reference AWS Console for manual setup.

## Configuring the Database Tier
1. **Set up an Aurora MySQL multi-AZ database in the database subnet.**
   - Reference AWS Console for manual setup.
2. **Configure the necessary security groups and network settings to allow communication between the application tier and the database tier.**

## Connecting the Tiers
1. **Update the security groups to allow the web tier to communicate with the application tier, and the application tier to communicate with the database tier.**
   - Commands:
     ```
     aws ec2 authorize-security-group-ingress --group-id <WEB_SECURITY_GROUP_ID> --protocol tcp --port 3000 --source-group <APP_SECURITY_GROUP_ID>
     aws ec2 authorize-security-group-ingress --group-id <APP_SECURITY_GROUP_ID> --protocol tcp --port 3306 --source-group <DB_SECURITY_GROUP_ID>
     ```
2. **Configure the necessary firewall rules and network settings to enable communication between the tiers.**

## Conclusion
This project provides a comprehensive guide to setting up a three-tier architecture on AWS for hosting web applications. It emphasizes the importance of monitoring and optimizing the architecture to meet changing needs while adhering to AWS best practices for security, scalability, and cost optimization.


