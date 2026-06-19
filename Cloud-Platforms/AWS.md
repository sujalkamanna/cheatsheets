# AWS (Amazon Web Services) Cheatsheet

![AWS Logo](https://a0.awsstatic.com/libra-css/images/logos/aws_logo_smile_1200x630.png)

---

## Table of Contents

- [1. What is AWS?](#1-what-is-aws)
- [2. Why AWS?](#2-why-aws)
- [3. AWS Global Infrastructure](#3-aws-global-infrastructure)
- [4. Core Services Overview](#4-core-services-overview)
- [5. AWS Regions](#5-aws-regions)
- [6. Availability Zones](#6-availability-zones)
- [7. Edge Locations](#7-edge-locations)
- [8. AWS Account Structure](#8-aws-account-structure)
- [9. Shared Responsibility Model](#9-shared-responsibility-model)
- [10. Identity and Access Management (IAM)](#10-identity-and-access-management-iam)
- [11. Compute Services](#11-compute-services)
- [12. Storage Services](#12-storage-services)
- [13. Networking Services](#13-networking-services)
- [14. Database Services](#14-database-services)
- [15. Monitoring and Management](#15-monitoring-and-management)
- [16. Virtual Private Cloud (VPC)](#16-virtual-private-cloud-vpc)
- [17. Subnets](#17-subnets)
- [18. Route Tables](#18-route-tables)
- [19. Internet Gateway](#19-internet-gateway)
- [20. NAT Gateway](#20-nat-gateway)
- [21. Security Groups](#21-security-groups)
- [22. Network ACLs](#22-network-acls)
- [23. Elastic Load Balancer (ELB)](#23-elastic-load-balancer-elb)
- [24. Auto Scaling](#24-auto-scaling)
- [25. Amazon EC2 Deep Dive](#25-amazon-ec2-deep-dive)
- [26. Amazon S3 Deep Dive](#26-amazon-s3-deep-dive)
- [27. Amazon RDS](#27-amazon-rds)
- [28. DynamoDB](#28-dynamodb)
- [29. Amazon Route 53](#29-amazon-route-53)
- [30. AWS CloudFront](#30-aws-cloudfront)
- [31. AWS CloudWatch](#31-aws-cloudwatch)
- [32. AWS CloudTrail](#32-aws-cloudtrail)
- [33. AWS Systems Manager](#33-aws-systems-manager)
- [34. AWS Secrets Manager](#34-aws-secrets-manager)
- [35. AWS KMS](#35-aws-kms)
- [36. AWS Lambda](#36-aws-lambda)
- [37. API Gateway](#37-api-gateway)
- [38. EventBridge](#38-eventbridge)
- [39. SNS](#39-sns)
- [40. SQS](#40-sqs)
- [41. Step Functions](#41-step-functions)
- [42. Elastic Container Registry (ECR)](#42-elastic-container-registry-ecr)
- [43. Elastic Container Service (ECS)](#43-elastic-container-service-ecs)
- [44. Elastic Kubernetes Service (EKS)](#44-elastic-kubernetes-service-eks)
- [45. AWS Fargate](#45-aws-fargate)
- [46. Infrastructure as Code](#46-infrastructure-as-code)
- [47. CloudFormation](#47-cloudformation)
- [48. Terraform on AWS](#48-terraform-on-aws)
- [49. CI/CD on AWS](#49-cicd-on-aws)
- [50. AWS CodeCommit](#50-aws-codecommit)
- [51. AWS CodeBuild](#51-aws-codebuild)
- [52. AWS CodeDeploy](#52-aws-codedeploy)
- [53. AWS CodePipeline](#53-aws-codepipeline)
- [54. Security Best Practices](#54-security-best-practices)
- [55. Cost Optimization](#55-cost-optimization)
- [56. High Availability](#56-high-availability)
- [57. Disaster Recovery](#57-disaster-recovery)
- [58. AWS Well-Architected Framework](#58-aws-well-architected-framework)
- [59. Common Use Cases](#59-common-use-cases)
- [60. Interview Questions](#60-interview-questions)

- [Additional Resources](#additional-resources)
- [Quick Reference](#quick-reference)
- [📎 Official Documentation](#-official-documentation)
- [📄 Copyright](#-copyright)
- [📎 Disclaimer & Attribution](#-disclaimer--attribution)
- [🎉 Conclusion](#-conclusion)

---

# 1. What is AWS?

Amazon Web Services (AWS) is the world's most widely adopted cloud computing platform.

AWS provides on-demand cloud services including:

* Compute
* Storage
* Networking
* Databases
* Security
* Monitoring
* Analytics
* Machine Learning
* Containers
* Serverless Computing

---

## Why AWS?

Traditional Infrastructure:

```text
Application
    │
    ▼
Physical Server
    │
    ▼
Data Center
```

Cloud Infrastructure:

```text
Application
    │
    ▼
AWS Services
    │
    ▼
Global Infrastructure
```

---

## Key Benefits

```text
Pay-As-You-Go
Elastic Scaling
Global Availability
Managed Services
High Reliability
Strong Security
```

---

## Common Use Cases

### Web Application Hosting

```text
Users
  │
  ▼
AWS Load Balancer
  │
  ▼
EC2 Instances
```

---

### Kubernetes Platforms

```text
Users
  │
  ▼
Amazon EKS
  │
  ▼
Containers
```

---

### Serverless Applications

```text
API Gateway
      │
      ▼
AWS Lambda
      │
      ▼
DynamoDB
```

---

# 2. Core Concepts

---

## Cloud Computing

Cloud computing provides IT resources over the internet.

Examples:

```text
Virtual Machines
Storage
Databases
Networking
```

---

## Regions

A Region is a physical geographic location.

Examples:

```text
us-east-1
us-west-2
ap-south-1
eu-west-1
```

Example:

```text
Mumbai Region
ap-south-1
```

---

## Availability Zones (AZs)

Each Region contains multiple AZs.

```text
Mumbai Region
│
├── AZ-A
├── AZ-B
└── AZ-C
```

Purpose:

```text
High Availability
Fault Tolerance
```

---

## Edge Locations

Used by:

```text
CloudFront
Route53
Global Accelerator
```

Purpose:

```text
Low Latency
Content Delivery
```

---

## Elasticity

Resources automatically grow or shrink.

```text
Traffic ↑
   │
   ▼
More Servers

Traffic ↓
   │
   ▼
Fewer Servers
```

---

## Scalability

Ability to handle increasing workload.

### Vertical Scaling

```text
2 CPU
  ↓
8 CPU
```

---

### Horizontal Scaling

```text
Server 1
Server 2
Server 3
Server 4
```

---

# 3. AWS Global Infrastructure

AWS operates one of the largest cloud networks in the world.

---

## Infrastructure Components

```text
Region
   │
   ▼
Availability Zones
   │
   ▼
Data Centers
```

---

## Global Architecture

```text
Users
   │
   ▼
Edge Locations
   │
   ▼
AWS Region
   │
   ▼
Availability Zones
```

---

## Example Region

```text
Mumbai
ap-south-1
```

Contains multiple AZs.

---

## Why Multiple AZs?

If one AZ fails:

```text
AZ-A ✖
AZ-B ✔
AZ-C ✔
```

Applications remain available.

---

## Multi-Region Architecture

```text
Mumbai Region
      │
      ▼
Backup
      │
      ▼
Singapore Region
```

Benefits:

```text
Disaster Recovery
Business Continuity
```

---

# 4. AWS Shared Responsibility Model

One of the most important AWS concepts.

---

## AWS Responsibility

AWS manages:

```text
Physical Data Centers
Networking Hardware
Storage Hardware
Compute Hardware
Global Infrastructure
```

---

## Customer Responsibility

Customer manages:

```text
Operating Systems
Applications
Data
IAM Permissions
Network Configuration
```

---

## Model

```text
AWS
 │
 ├── Security OF the Cloud
 │
 ▼
Customer
 │
 ├── Security IN the Cloud
```

---

## Example

EC2 Instance:

AWS Secures:

```text
Physical Server
Network Hardware
Power Supply
```

Customer Secures:

```text
Linux OS
Users
Applications
SSH Access
```

---

# 5. Identity & Access Management (IAM)

IAM controls access to AWS resources.

---

## Core IAM Components

```text
Users
Groups
Roles
Policies
```

---

## IAM User

Represents an individual identity.

Example:

```text
john-admin
```

---

## IAM Group

Collection of users.

Example:

```text
Developers
DevOps
Admins
```

---

## IAM Role

Temporary permissions.

Commonly used by:

```text
EC2
Lambda
EKS
ECS
```

---

## IAM Policy

JSON document defining permissions.

Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "*"
    }
  ]
}
```

---

## IAM Best Practices

### Enable MFA

```text
Username
Password
+
MFA
```

---

### Least Privilege

Grant only required permissions.

Good:

```text
Read S3 Bucket
```

Bad:

```text
AdministratorAccess
```

---

### Use Roles Instead of Keys

Preferred:

```text
EC2 Role
```

Avoid:

```text
Access Keys
```

---

# 6. AWS CLI

AWS Command Line Interface allows managing AWS services from terminal.

---

## Install AWS CLI

Linux:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o awscliv2.zip
```

---

Verify:

```bash
aws --version
```

---

## Configure AWS CLI

```bash
aws configure
```

Provide:

```text
Access Key
Secret Key
Region
Output Format
```

---

Example:

```text
AWS Access Key ID:
AWS Secret Access Key:
Default region:
Default output format:
```

---

## Verify Identity

```bash
aws sts get-caller-identity
```

---

## List S3 Buckets

```bash
aws s3 ls
```

---

## List EC2 Instances

```bash
aws ec2 describe-instances
```

---

## Common AWS CLI Services

```text
aws s3
aws ec2
aws iam
aws eks
aws ecs
aws rds
aws lambda
```

---

# 7. Security Fundamentals

Security is AWS's highest priority.

---

## Security Layers

```text
IAM
Encryption
Security Groups
NACLs
MFA
CloudTrail
```

---

## Multi-Factor Authentication

Enable:

```text
Password
+
OTP
```

---

## Encryption

### At Rest

Examples:

```text
S3
EBS
RDS
```

---

### In Transit

```text
HTTPS
TLS
SSL
```

---

## Security Groups

Acts like a firewall.

```text
Internet
    │
    ▼
Security Group
    │
    ▼
EC2
```

---

Example:

Allow SSH:

```text
TCP 22
```

Allow HTTPS:

```text
TCP 443
```

---

## CloudTrail

Records AWS API activity.

```text
User
 │
 ▼
AWS API
 │
 ▼
CloudTrail Logs
```

---

# 8. AWS Account & Billing Basics

---

## AWS Account

Root account created during signup.

---

## Root User

Has full permissions.

Best Practice:

```text
Do NOT use daily
Enable MFA
```

---

## Billing Dashboard

Provides:

```text
Current Spend
Forecast
Cost Breakdown
Budgets
```

---

## AWS Free Tier

Includes:

```text
EC2
S3
Lambda
RDS
```

for limited usage.

---

## Cost Optimization Basics

Terminate unused:

```text
EC2
EBS
Elastic IPs
Snapshots
```

---

# 9. High Availability Concepts

High Availability (HA) means applications remain available during failures.

---

## Single AZ Architecture

```text
Users
  │
  ▼
EC2
  │
  ▼
AZ-A
```

Risk:

```text
AZ Failure
```

---

## Multi-AZ Architecture

```text
Users
   │
   ▼
Load Balancer
   │
   ├── EC2 AZ-A
   └── EC2 AZ-B
```

Benefits:

```text
Fault Tolerance
Resilience
```

---

# 10. Scalability Concepts

AWS enables automatic scaling.

---

## Vertical Scaling

Increase instance size.

```text
t3.micro
    ▼
t3.large
```

---

## Horizontal Scaling

Add more instances.

```text
EC2-1
EC2-2
EC2-3
EC2-4
```

---

## Auto Scaling

Automatically adjusts instance count.

```text
Traffic ↑
     │
     ▼
More EC2 Instances

Traffic ↓
     │
     ▼
Fewer EC2 Instances
```

---

## Scalability Workflow

```text
Users
  │
  ▼
Load Balancer
  │
  ▼
Auto Scaling Group
  │
  ▼
EC2 Instances
```

---

## Most Important Concepts

```text
Region
Availability Zone
IAM
Security Groups
AWS CLI
MFA
High Availability
Scalability
```

---

## Most Important Commands

```bash
aws configure

aws sts get-caller-identity

aws s3 ls

aws ec2 describe-instances
```

---

# 11. Compute Services

Compute services provide processing power for applications.

AWS offers:

```text
EC2
Lambda
ECS
EKS
Batch
Lightsail
```

---

## Compute Service Selection

```text
Need Virtual Machines?
        │
        ▼
       EC2

Need Containers?
        │
        ▼
     ECS/EKS

Need Serverless?
        │
        ▼
      Lambda
```

---

## Compute Architecture

```text
Users
  │
  ▼
Load Balancer
  │
  ▼
Compute Layer
  │
  ├── EC2
  ├── ECS
  ├── EKS
  └── Lambda
```

---

# 12. Amazon EC2

Amazon Elastic Compute Cloud (EC2) provides virtual servers.

---

## Common Use Cases

```text
Web Servers
Application Servers
Jenkins
GitLab
Databases
Monitoring Tools
```

---

## EC2 Architecture

```text
EC2 Instance
     │
     ├── CPU
     ├── Memory
     ├── Storage
     └── Network
```

---

## Instance Types

### General Purpose

```text
t3
t4g
m5
m6i
```

---

### Compute Optimized

```text
c5
c6i
```

---

### Memory Optimized

```text
r5
r6i
```

---

### Storage Optimized

```text
i3
i4i
```

---

## Launch Instance

AWS CLI:

```bash
aws ec2 run-instances \
--image-id ami-xxxx \
--instance-type t3.micro
```

---

## List Instances

```bash
aws ec2 describe-instances
```

---

## Stop Instance

```bash
aws ec2 stop-instances \
--instance-ids i-123456
```

---

## Start Instance

```bash
aws ec2 start-instances \
--instance-ids i-123456
```

---

## Terminate Instance

```bash
aws ec2 terminate-instances \
--instance-ids i-123456
```

---

## EC2 Pricing Models

### On-Demand

```text
Pay Per Use
```

---

### Reserved Instances

```text
1-Year
3-Year
```

---

### Spot Instances

```text
Low Cost
Interruptible
```

---

## EC2 Best Practices

```text
Use IAM Roles
Enable Monitoring
Use Auto Scaling
Use Security Groups
Avoid Root Login
```

---

# 13. Auto Scaling

Auto Scaling automatically adjusts EC2 capacity.

---

## Why Auto Scaling?

Traffic changes constantly.

```text
Traffic ↑
     │
     ▼
More Instances

Traffic ↓
     │
     ▼
Fewer Instances
```

---

## Auto Scaling Components

```text
Launch Template
        │
        ▼
Auto Scaling Group
        │
        ▼
EC2 Instances
```

---

## Auto Scaling Architecture

```text
Users
   │
   ▼
Load Balancer
   │
   ▼
Auto Scaling Group
   │
   ├── EC2-1
   ├── EC2-2
   └── EC2-3
```

---

## Benefits

```text
High Availability
Cost Optimization
Elastic Scaling
Fault Tolerance
```

---

# 14. Elastic Load Balancing (ELB)

Distributes traffic across targets.

---

## Types of Load Balancers

### Application Load Balancer (ALB)

```text
HTTP
HTTPS
Layer 7
```

---

### Network Load Balancer (NLB)

```text
TCP
UDP
Layer 4
```

---

### Gateway Load Balancer

```text
Security Appliances
Firewalls
```

---

## Load Balancer Architecture

```text
Users
   │
   ▼
Load Balancer
   │
   ├── EC2-1
   ├── EC2-2
   └── EC2-3
```

---

## Health Checks

Load Balancer verifies:

```text
Application Health
Port Availability
Response Status
```

---

# 15. Storage Services

AWS provides multiple storage solutions.

---

## Storage Categories

```text
Object Storage
Block Storage
File Storage
Archive Storage
```

---

## Service Mapping

```text
Object Storage
    └── S3

Block Storage
    └── EBS

File Storage
    └── EFS

Archive
    └── Glacier
```

---

# 16. Amazon S3

Amazon Simple Storage Service.

One of the most popular AWS services.

---

## S3 Features

```text
Highly Durable
Highly Available
Scalable
Secure
```

---

## Common Use Cases

```text
Backups
Static Websites
Logs
Data Lakes
Artifacts
```

---

## S3 Architecture

```text
Users
  │
  ▼
S3 Bucket
  │
  ├── file1
  ├── file2
  └── file3
```

---

## Create Bucket

```bash
aws s3 mb s3://my-bucket
```

---

## Upload File

```bash
aws s3 cp file.txt \
s3://my-bucket
```

---

## Download File

```bash
aws s3 cp \
s3://my-bucket/file.txt .
```

---

## List Buckets

```bash
aws s3 ls
```

---

## S3 Storage Classes

```text
Standard
Standard-IA
One Zone-IA
Glacier
Deep Archive
```

---

## S3 Best Practices

```text
Enable Versioning
Enable Encryption
Use Lifecycle Policies
Use Bucket Policies
```

---

# 17. Amazon EBS

Elastic Block Store.

Persistent storage for EC2.

---

## Architecture

```text
EC2 Instance
      │
      ▼
   EBS Volume
```

---

## Characteristics

```text
Persistent
High Performance
Block Storage
```

---

## Common Use Cases

```text
Databases
Application Data
OS Disks
```

---

## Snapshot

Create backup:

```text
EBS Volume
      │
      ▼
 Snapshot
```

---

## Best Practices

```text
Enable Encryption
Take Snapshots
Monitor IOPS
```

---

# 18. Amazon EFS

Elastic File System.

Managed NFS storage.

---

## Architecture

```text
EC2-1
   │
   ├── EFS
   │
EC2-2
```

---

## Features

```text
Shared Storage
Elastic Capacity
Multi-AZ
```

---

## Use Cases

```text
Containers
Shared Files
Web Applications
```

---

## Comparison

| Service | Type   |
| ------- | ------ |
| EBS     | Block  |
| EFS     | File   |
| S3      | Object |

---

# 19. Database Services

AWS provides multiple database solutions.

---

## Database Categories

```text
Relational
NoSQL
In-Memory
Data Warehouse
```

---

## Services

```text
RDS
Aurora
DynamoDB
ElastiCache
Redshift
```

---

# 20. Amazon RDS

Managed relational database service.

---

## Supported Engines

```text
MySQL
PostgreSQL
MariaDB
Oracle
SQL Server
```

---

## Architecture

```text
Application
      │
      ▼
      RDS
      │
      ▼
 Database
```

---

## Multi-AZ RDS

```text
Primary DB
      │
      ▼
 Standby DB
```

Benefits:

```text
High Availability
Automatic Failover
```

---

## RDS Benefits

```text
Automated Backups
Managed Patching
Monitoring
Scaling
```

---

# 21. Amazon DynamoDB

Managed NoSQL database.

---

## Characteristics

```text
Serverless
Highly Scalable
Fully Managed
```

---

## Architecture

```text
Application
      │
      ▼
  DynamoDB
```

---

## Common Use Cases

```text
Gaming
IoT
E-Commerce
Microservices
```

---

## Benefits

```text
Millisecond Latency
Auto Scaling
High Availability
```

---

# 22. Serverless Computing

Serverless removes server management.

---

## AWS Serverless Services

```text
Lambda
API Gateway
DynamoDB
EventBridge
```

---

## Benefits

```text
No Servers
Pay Per Use
Auto Scaling
```

---

# 23. AWS Lambda

Run code without managing servers.

---

## Workflow

```text
Event
  │
  ▼
Lambda
  │
  ▼
Response
```

---

## Event Sources

```text
API Gateway
S3
CloudWatch
SNS
SQS
```

---

## Example Use Cases

```text
Image Processing
Automation
API Backends
Scheduled Jobs
```

---

## Lambda Benefits

```text
Serverless
Automatic Scaling
Cost Effective
```

---

# 24. Container Services

AWS provides multiple container platforms.

---

## Services

```text
ECS
EKS
Fargate
```

---

## Selection

```text
AWS Native?
      │
      ▼
     ECS

Need Kubernetes?
      │
      ▼
     EKS
```

---

# 25. Amazon ECS

Elastic Container Service.

AWS-native container orchestration.

---

## Architecture

```text
ECS Cluster
      │
      ▼
 Tasks
      │
      ▼
 Containers
```

---

## Deployment Models

### EC2 Launch Type

```text
ECS
 │
 ▼
EC2
```

---

### Fargate

```text
ECS
 │
 ▼
Fargate
```

No server management.

---

## ECS Benefits

```text
Simple
Managed
Integrated with AWS
```

---

# 26. Amazon EKS

Elastic Kubernetes Service.

Managed Kubernetes platform.

---

## Architecture

```text
Users
  │
  ▼
EKS Control Plane
  │
  ▼
Worker Nodes
  │
  ▼
Pods
```

---

## Components

```text
Control Plane
Worker Nodes
Pods
Services
Ingress
```

---

## Benefits

```text
Managed Kubernetes
Highly Available
Integrated Security
```

---

## Common Use Cases

```text
Microservices
DevOps Platforms
Cloud Native Apps
```

---

## EKS Workflow

```text
Developer
     │
     ▼
kubectl
     │
     ▼
EKS Cluster
     │
     ▼
Pods
```

---

## Most Important Services

```text
EC2
S3
RDS
Lambda
EKS
IAM
```

---

## Most Important Commands

```bash
aws ec2 describe-instances

aws s3 ls

aws s3 cp

aws rds describe-db-instances
```

---

# 27. Networking Fundamentals

Networking is the foundation of AWS infrastructure.

---

## Network Architecture

```text
Internet
    │
    ▼
Route53
    │
    ▼
CloudFront
    │
    ▼
Load Balancer
    │
    ▼
VPC
    │
    ▼
Applications
```

---

## Core Networking Components

```text
VPC
Subnets
Route Tables
Internet Gateway
NAT Gateway
Security Groups
NACLs
```

---

## Typical Flow

```text
User
 │
 ▼
DNS
 │
 ▼
Load Balancer
 │
 ▼
Application
 │
 ▼
Database
```

---

# 28. Amazon VPC

VPC = Virtual Private Cloud.

Your private network inside AWS.

---

## VPC Architecture

```text
AWS Region
    │
    ▼
    VPC
    │
 ┌──┴──┐
 ▼     ▼
Public Private
Subnet Subnet
```

---

## Why VPC?

Provides:

```text
Isolation
Security
Network Control
Routing Control
```

---

## Example CIDR

```text
10.0.0.0/16
```

Provides:

```text
65,536 IP Addresses
```

---

## Create VPC

```bash
aws ec2 create-vpc \
--cidr-block 10.0.0.0/16
```

---

## View VPCs

```bash
aws ec2 describe-vpcs
```

---

## Best Practices

```text
Use Private Subnets
Use Multi-AZ Design
Avoid Default VPC
```

---

# 29. Subnets

Subnets divide a VPC into smaller networks.

---

## Types

### Public Subnet

Has:

```text
Internet Access
```

Examples:

```text
Load Balancers
Bastion Hosts
```

---

### Private Subnet

No direct internet access.

Examples:

```text
Applications
Databases
```

---

## Architecture

```text
VPC
 │
 ├── Public Subnet
 │       │
 │       ▼
 │      ALB
 │
 └── Private Subnet
         │
         ▼
       EC2
```

---

## Multi-AZ Example

```text
AZ-A
 ├─ Public
 └─ Private

AZ-B
 ├─ Public
 └─ Private
```

---

## Benefits

```text
High Availability
Security
Scalability
```

---

# 30. Route Tables

Route tables determine traffic flow.

---

## Example

```text
Destination      Target
0.0.0.0/0        IGW
10.0.0.0/16      Local
```

---

## Architecture

```text
Subnet
   │
   ▼
Route Table
   │
   ▼
Destination
```

---

## View Routes

```bash
aws ec2 describe-route-tables
```

---

## Public Route

```text
0.0.0.0/0
    │
    ▼
Internet Gateway
```

---

# 31. Internet Gateway (IGW)

Allows internet access.

---

## Architecture

```text
Internet
    │
    ▼
Internet Gateway
    │
    ▼
Public Subnet
```

---

## Use Cases

```text
Public Websites
Public APIs
Bastion Hosts
```

---

## Key Rule

Public subnet requires:

```text
Internet Gateway
+
Public Route
```

---

# 32. NAT Gateway

Allows private instances to access internet.

---

## Why NAT?

Private resources need updates.

Example:

```text
yum update
apt update
Docker Pull
```

---

## Architecture

```text
Private EC2
      │
      ▼
 NAT Gateway
      │
      ▼
 Internet
```

---

## Benefits

```text
Private Resources Stay Hidden
Outbound Internet Access
```

---

## Best Practice

Deploy NAT Gateway in each AZ.

---

# 33. Security Groups

Security Group = Instance Firewall.

---

## Characteristics

```text
Stateful
Allow Rules Only
Attached to Resources
```

---

## Architecture

```text
Internet
    │
    ▼
Security Group
    │
    ▼
EC2
```

---

## Example Rules

### SSH

```text
TCP 22
```

---

### HTTP

```text
TCP 80
```

---

### HTTPS

```text
TCP 443
```

---

## Example

```text
Allow:
22
80
443
```

---

## View Security Groups

```bash
aws ec2 describe-security-groups
```

---

## Best Practices

```text
Least Privilege
Restrict SSH
Use Separate Groups
```

---

# 34. Network ACLs (NACLs)

Network-level firewall.

---

## Characteristics

```text
Stateless
Allow Rules
Deny Rules
Subnet Level
```

---

## Comparison

| Feature     | Security Group | NACL   |
| ----------- | -------------- | ------ |
| Level       | Instance       | Subnet |
| Stateful    | Yes            | No     |
| Allow Rules | Yes            | Yes    |
| Deny Rules  | No             | Yes    |

---

## Architecture

```text
Subnet
   │
   ▼
NACL
   │
   ▼
Resources
```

---

## Use Cases

```text
Additional Security Layer
Subnet Protection
```

---

# 35. Route 53

AWS DNS Service.

---

## Purpose

Converts:

```text
example.com
```

into:

```text
IP Address
```

---

## Architecture

```text
User
 │
 ▼
Route53
 │
 ▼
Application
```

---

## Routing Policies

### Simple Routing

```text
One Resource
```

---

### Weighted Routing

```text
70% Traffic
30% Traffic
```

---

### Latency Routing

```text
Nearest Region
```

---

### Failover Routing

```text
Primary
   │
   ▼
Secondary
```

---

## Common Use Cases

```text
DNS Management
Health Checks
Traffic Routing
```

---

# 36. CloudFront

Content Delivery Network (CDN).

---

## Purpose

Deliver content globally.

---

## Architecture

```text
Users
   │
   ▼
CloudFront
   │
   ▼
Origin
```

---

## Origins

```text
S3
ALB
EC2
Custom Server
```

---

## Benefits

```text
Low Latency
Global Delivery
DDoS Protection
Caching
```

---

## Common Use Cases

```text
Static Websites
Media Streaming
APIs
```

---

# 37. CloudWatch

AWS Monitoring Service.

---

## Monitors

```text
EC2
RDS
Lambda
EKS
ECS
```

---

## Architecture

```text
AWS Services
      │
      ▼
CloudWatch
      │
      ▼
Metrics
```

---

## Metrics Examples

```text
CPU Utilization
Memory Usage
Disk Usage
Network Traffic
```

---

## Alarms

Example:

```text
CPU > 80%
```

Trigger:

```text
SNS Notification
```

---

## Logs

```text
Application Logs
System Logs
Lambda Logs
```

---

## View Metrics

```bash
aws cloudwatch list-metrics
```

---

# 38. CloudTrail

Records AWS API activity.

---

## Purpose

Audit AWS actions.

---

## Architecture

```text
User
 │
 ▼
AWS API
 │
 ▼
CloudTrail
```

---

## Tracks

```text
Who?
What?
When?
Where?
```

---

## Example

```text
User Deleted EC2
```

CloudTrail records:

```text
Username
Timestamp
IP Address
```

---

## Security Benefits

```text
Auditing
Compliance
Forensics
```

---

# 39. AWS Config

Tracks AWS configuration changes.

---

## Purpose

Configuration compliance.

---

## Architecture

```text
AWS Resource
      │
      ▼
AWS Config
      │
      ▼
History
```

---

## Examples

```text
Security Group Changes
S3 Policy Changes
IAM Changes
```

---

## Benefits

```text
Compliance
Governance
Auditing
```

---

# 40. DevOps Services

AWS provides managed CI/CD services.

---

## Core Services

```text
CodeCommit
CodeBuild
CodeDeploy
CodePipeline
```

---

## CI/CD Flow

```text
Developer
     │
     ▼
CodeCommit
     │
     ▼
CodeBuild
     │
     ▼
CodeDeploy
     │
     ▼
Production
```

---

# 41. CodeCommit

Git repository service.

---

## Features

```text
Private Git Repositories
IAM Integration
Encryption
```

---

## Architecture

```text
Developer
      │
      ▼
CodeCommit
```

---

# 42. CodeBuild

Build service.

---

## Purpose

Compile and test code.

---

## Workflow

```text
Source Code
      │
      ▼
CodeBuild
      │
      ▼
Artifact
```

---

## Common Tasks

```text
Build
Test
Package
```

---

# 43. CodeDeploy

Deployment automation service.

---

## Deployment Targets

```text
EC2
Lambda
ECS
```

---

## Architecture

```text
Artifact
    │
    ▼
CodeDeploy
    │
    ▼
Production
```

---

## Deployment Types

```text
In-Place
Blue-Green
```

---

# 44. CodePipeline

Orchestrates CI/CD pipelines.

---

## Pipeline Architecture

```text
Source
  │
  ▼
Build
  │
  ▼
Test
  │
  ▼
Deploy
```

---

## Example

```text
Git Push
   │
   ▼
CodePipeline
   │
   ▼
Production Deployment
```

---

## Benefits

```text
Automation
Consistency
Faster Releases
```

---

## Most Important Services

```text
VPC
Security Groups
Route53
CloudWatch
CloudTrail
CodePipeline
```

---

## Most Important Commands

```bash
aws ec2 describe-vpcs

aws ec2 describe-route-tables

aws ec2 describe-security-groups

aws cloudwatch list-metrics
```

---

# 45. High Availability (HA)

High Availability ensures applications remain available even during failures.

---

## Single AZ Architecture

```text
Users
  │
  ▼
EC2
  │
  ▼
AZ-A
```

Risk:

```text
AZ Failure
=
Application Down
```

---

## Multi-AZ Architecture

```text
Users
   │
   ▼
Load Balancer
   │
   ├── EC2 (AZ-A)
   └── EC2 (AZ-B)
```

Benefits:

```text
Fault Tolerance
High Availability
Automatic Recovery
```

---

## HA Best Practices

```text
Use Multiple AZs
Use Load Balancers
Use Auto Scaling
Enable Multi-AZ Databases
Avoid Single Points of Failure
```

---

# 46. Scalability Patterns

Scalability allows applications to handle growing workloads.

---

## Vertical Scaling

Increase resources.

```text
2 CPU
  ▼
8 CPU
```

Example:

```text
t3.micro
    ▼
m5.large
```

---

## Horizontal Scaling

Add more servers.

```text
EC2-1
EC2-2
EC2-3
EC2-4
```

---

## Elastic Architecture

```text
Users
  │
  ▼
Load Balancer
  │
  ▼
Auto Scaling Group
  │
  ├── EC2
  ├── EC2
  └── EC2
```

---

## AWS Services Supporting Scaling

```text
Auto Scaling
ALB
Lambda
DynamoDB
EKS
ECS
```

---

# 47. Disaster Recovery (DR)

Disaster Recovery focuses on restoring systems after major failures.

---

## DR Architecture

```text
Primary Region
      │
      ▼
Backup
      │
      ▼
Secondary Region
```

---

## Backup Strategy

```text
Application
      │
      ▼
Backup
      │
      ▼
S3
      │
      ▼
Recovery
```

---

## DR Models

### Backup & Restore

```text
Lowest Cost
Highest Recovery Time
```

---

### Pilot Light

```text
Minimal Infrastructure
Fast Recovery
```

---

### Warm Standby

```text
Partially Running Environment
```

---

### Multi-Site Active/Active

```text
Region A
   │
   ▼
Region B

Both Active
```

---

## AWS Backup Service

Centralized backup management.

Supported:

```text
EBS
RDS
EFS
DynamoDB
EC2
```

---

# 48. Cost Optimization

Cloud costs can grow rapidly without monitoring.

---

## Cost Optimization Pillars

```text
Right Sizing
Auto Scaling
Reserved Capacity
Monitoring
```

---

## Common Waste

```text
Unused EC2
Unused EBS
Idle Load Balancers
Unused Elastic IPs
Old Snapshots
```

---

## Right Sizing

Bad:

```text
m5.4xlarge
10% CPU Usage
```

Good:

```text
t3.medium
```

---

## Savings Plans

Commit usage:

```text
1 Year
3 Year
```

Benefits:

```text
Lower Costs
Predictable Billing
```

---

## Spot Instances

Use for:

```text
Batch Jobs
CI/CD
Testing
```

Benefits:

```text
Up To 90% Savings
```

---

## S3 Cost Optimization

Use:

```text
Lifecycle Policies
Intelligent Tiering
Glacier
Deep Archive
```

---

# 49. Real-World AWS Architectures

---

## Three-Tier Web Application

```text
Users
  │
  ▼
Load Balancer
  │
  ▼
Application Layer
(EC2/ECS/EKS)
  │
  ▼
Database Layer
(RDS)
```

---

## Serverless Architecture

```text
Users
  │
  ▼
API Gateway
  │
  ▼
Lambda
  │
  ▼
DynamoDB
```

---

## Kubernetes Architecture

```text
Users
  │
  ▼
ALB
  │
  ▼
EKS Cluster
  │
  ▼
Pods
```

---

## DevOps Platform

```text
Developer
    │
    ▼
CodePipeline
    │
    ▼
CodeBuild
    │
    ▼
ECR
    │
    ▼
EKS
```

---

## Monitoring Architecture

```text
Applications
      │
      ▼
CloudWatch
      │
      ▼
Alarms
      │
      ▼
SNS
```

---

# 50. Security Best Practices

Security should be implemented at every layer.

---

## IAM

Use:

```text
Roles
Groups
Least Privilege
```

Avoid:

```text
Administrator Access Everywhere
```

---

## Enable MFA

Protect:

```text
Root Account
IAM Users
```

---

## Encrypt Everything

Services:

```text
S3
EBS
RDS
EFS
```

---

## Logging

Enable:

```text
CloudTrail
CloudWatch
AWS Config
```

---

## Network Security

Use:

```text
Security Groups
NACLs
Private Subnets
```

---

## Secrets Management

Use:

```text
AWS Secrets Manager
Parameter Store
```

Never:

```text
Hardcode Passwords
```

---

# 51. Troubleshooting

---

## EC2 Not Reachable

Check:

```text
Security Groups
NACLs
Route Tables
Internet Gateway
```

---

## Website Not Loading

Verify:

```text
Load Balancer
Health Checks
Target Group
DNS
```

---

## S3 Access Denied

Check:

```text
IAM Policy
Bucket Policy
ACLs
```

---

## Lambda Failure

Check:

```text
CloudWatch Logs
IAM Role
Timeout
Memory
```

---

## RDS Connectivity Issues

Verify:

```text
Security Group
Subnet
Endpoint
Credentials
```

---

## Useful Commands

View identity:

```bash
aws sts get-caller-identity
```

---

List S3 buckets:

```bash
aws s3 ls
```

---

Describe EC2:

```bash
aws ec2 describe-instances
```

---

# 52. Interview Questions

---

## What is AWS?

AWS is a cloud platform providing compute, storage, networking, databases, security, and managed services.

---

## What is a Region?

A physical geographic location containing Availability Zones.

---

## What is an Availability Zone?

An isolated data center location within a Region.

---

## Difference Between Security Group and NACL?

| Security Group | NACL         |
| -------------- | ------------ |
| Stateful       | Stateless    |
| Instance Level | Subnet Level |
| Allow Only     | Allow & Deny |

---

## What is IAM?

Identity and Access Management service used to control permissions.

---

## Difference Between S3, EBS, and EFS?

| Service | Type           |
| ------- | -------------- |
| S3      | Object Storage |
| EBS     | Block Storage  |
| EFS     | File Storage   |

---

## What is Auto Scaling?

Automatically adjusts resources based on workload.

---

## What is a VPC?

Private network inside AWS.

---

## What is CloudWatch?

Monitoring and observability service.

---

## What is CloudTrail?

AWS API auditing service.

---

## Difference Between ECS and EKS?

```text
ECS
 └── AWS Native Containers

EKS
 └── Kubernetes
```

---

# 53. Additional Resources

## Official AWS Resources

* https://aws.amazon.com
* https://docs.aws.amazon.com
* https://aws.amazon.com/training

---

## Certification Resources

* AWS Cloud Practitioner
* AWS Solutions Architect Associate
* AWS Developer Associate
* AWS SysOps Administrator

---

## Learning Resources

* AWS Workshops
* AWS Skill Builder
* AWS Blogs
* AWS Whitepapers

---

# 54. Quick Reference

## Identity

```bash
aws configure
aws sts get-caller-identity
```

---

## S3

```bash
aws s3 ls

aws s3 cp file.txt s3://bucket

aws s3 sync . s3://bucket
```

---

## EC2

```bash
aws ec2 describe-instances

aws ec2 start-instances

aws ec2 stop-instances

aws ec2 terminate-instances
```

---

## IAM

```bash
aws iam list-users

aws iam list-roles
```

---

## VPC

```bash
aws ec2 describe-vpcs

aws ec2 describe-subnets

aws ec2 describe-route-tables
```

---

## Monitoring

```bash
aws cloudwatch list-metrics

aws logs describe-log-groups
```

---

# 📎 Official Documentation

Official Website:

https://aws.amazon.com

Documentation:

https://docs.aws.amazon.com

AWS CLI Documentation:

https://docs.aws.amazon.com/cli

AWS Training:

https://aws.amazon.com/training

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, cloud engineering, DevOps, SRE, and architecture studies.

Primary references include:

* AWS Official Documentation
* AWS Whitepapers
* AWS Training Resources
* AWS Certification Guides

Amazon Web Services (AWS) is a registered trademark of Amazon.com, Inc.

All trademarks, service names, and project names belong to their respective owners.

For authoritative information, always refer to official AWS documentation.

---

# 🎉 Conclusion

You now understand:

* AWS Fundamentals
* Global Infrastructure
* IAM & Security
* EC2
* Auto Scaling
* Load Balancers
* S3
* EBS
* EFS
* RDS
* DynamoDB
* Lambda
* ECS
* EKS
* VPC
* Networking
* Monitoring
* DevOps Services
* High Availability
* Scalability
* Disaster Recovery
* Cost Optimization
* Security Best Practices

AWS is the leading cloud platform because it combines:

```text
Compute
+
Storage
+
Networking
+
Security
+
Containers
+
Serverless
+
Databases
+
Global Infrastructure
```

into a highly scalable and reliable cloud ecosystem.

---

**Last Updated:** 2026
**Platform:** Amazon Web Services (AWS)
**License:** Proprietary Cloud Platform

For latest information, visit https://aws.amazon.com/documentation