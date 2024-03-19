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

## Setting Up the VPC and Subnets
1. Log in to your AWS Management Console.
2. Navigate to the VPC dashboard.
3. Create a new VPC with a unique CIDR block.
4. Create three subnets within the VPC for the web, application, and database tiers, each in a different Availability Zone.
5. Ensure appropriate route tables and internet gateways for the subnets.

## Launching EC2 Instances
1. Go to the EC2 dashboard.
2. Launch an EC2 instance for each tier, ensuring they are in the respective subnets.
3. Use Amazon Machine Images (AMIs) for your instances, such as Amazon Linux or Ubuntu.
4. Configure security groups to allow necessary inbound and outbound traffic for each tier.

## Configuring the Web Tier
1. Connect to the web EC2 instance using SSH.
2. Install a web server like Apache or Nginx.
3. Configure the web server to serve your web application, e.g., a React.js website.
4. Set up the public-facing Application Load Balancer to forward client traffic to the web tier EC2 instances.
5. Configure health checks and enable autoscaling for the web tier.

## Configuring the Application Tier
1. Connect to the application EC2 instance using SSH.
2. Install Node.js and deploy the application code.
3. Create an internal-facing Application Load Balancer for the application tier and configure it to forward traffic to the Node.js servers.
4. Implement health checks and autoscaling for the application tier.

## Configuring the Database Tier
1. Set up an Aurora MySQL multi-AZ database in the database subnet.
2. Configure the necessary security groups and network settings to allow communication between the application tier and the database tier.

## Connecting the Tiers
1. Update the security groups to allow the web tier to communicate with the application tier, and the application tier to communicate with the database tier.
2. Configure the necessary firewall rules and network settings to enable communication between the tiers.

## Conclusion
This project provides a comprehensive guide to setting up a three-tier architecture on AWS for hosting web applications. It emphasizes the importance of monitoring and optimizing the architecture to meet changing needs while adhering to AWS best practices for security, scalability, and cost optimization.

For more detailed information and specific commands, please refer to the associated GitHub repository.
