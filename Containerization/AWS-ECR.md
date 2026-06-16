# AWS ECR Cheatsheet

![AWS ECR Logo](https://d1.awsstatic.com/product-marketing/ECR/Amazon-ECR-Product-Icon.6d8f0ef58f2d59f6d4d16f6f0d91982466e0e5c3.png)

---

## Table of Contents

* [1. What is AWS ECR?](#1-what-is-aws-ecr)
* [2. Why AWS ECR?](#2-why-aws-ecr)
* [3. ECR Architecture](#3-ecr-architecture)
* [4. Core Components](#4-core-components)
* [5. Public vs Private Repositories](#5-public-vs-private-repositories)
* [6. Creating Repositories](#6-creating-repositories)
* [7. Repository Management](#7-repository-management)
* [8. Authentication](#8-authentication)
* [9. Docker and ECR Integration](#9-docker-and-ecr-integration)
* [10. Image Tagging](#10-image-tagging)
* [11. Pushing Images](#11-pushing-images)
* [12. Pulling Images](#12-pulling-images)
* [13. Repository Policies](#13-repository-policies)
* [14. Image Scanning](#14-image-scanning)
* [15. Lifecycle Policies](#15-lifecycle-policies)

* [Additional Resources](#additional-resources)
* [Quick Reference](#quick-reference)
* [📎 Official Documentation](#-official-documentation)
* [📄 Copyright](#-copyright)
* [📎 Disclaimer & Attribution](#-disclaimer--attribution)
* [🎉 Conclusion](#-conclusion) 

---

# 1. What is AWS ECR?

AWS ECR (Amazon Elastic Container Registry) is a fully managed container image registry service provided by AWS.

It allows organizations to securely store, manage, scan, version, and distribute container images.

---

## Purpose

```text
Container Image Storage
Image Distribution
Version Management
Security Scanning
AWS Integration
```

---

## High-Level Architecture

```text
Developer
    │
    ▼
Docker Image
    │
    ▼
Amazon ECR
    │
    ▼
ECS / EKS / EC2
```

---

## Common Use Cases

```text
Kubernetes
ECS Deployments
CI/CD Pipelines
Microservices
Container Platforms
```

---

## Benefits

```text
Fully Managed
Highly Available
Secure
Scalable
AWS Native
```

---

# 2. Why AWS ECR?

Managing private container registries manually requires infrastructure, storage, security, and maintenance.

ECR eliminates this operational burden.

---

## Traditional Registry Approach

```text
Docker Registry
      │
      ▼
Servers
Storage
Security
Maintenance
```

---

## AWS ECR Approach

```text
Docker Image
      │
      ▼
Amazon ECR
      │
      ▼
AWS Managed Service
```

---

## Advantages

```text
No Infrastructure Management
Built-In Security
IAM Integration
Image Scanning
High Availability
```

---

## Popular Integrations

```text
ECS
EKS
Lambda
CodePipeline
GitHub Actions
```

---

# 3. ECR Architecture

Amazon ECR acts as a secure repository for container images.

---

## Architecture

```text
Developer
    │
    ▼
Docker CLI
    │
    ▼
Amazon ECR
    │
    ▼
Container Platform
```

---

## Workflow

```text
Build Image
    │
    ▼
Tag Image
    │
    ▼
Push to ECR
    │
    ▼
Deploy
```

---

## Components

```text
Repositories
Images
Policies
Scanning
Authentication
```

---

## Benefits

```text
Centralized Registry
Automation
Scalability
```

---

# 4. Core Components

ECR consists of several major components.

---

## Main Components

```text
Repositories
Images
Policies
Lifecycle Rules
Scanning
Replication
```

---

## Architecture

```text
Amazon ECR
     │
 ┌───┼───┐
 ▼   ▼   ▼
Images
Policies
Scanning
```

---

## Supporting Services

```text
IAM
CloudWatch
CloudTrail
EventBridge
KMS
```

---

## Benefits

```text
Security
Automation
Governance
```

---

# 5. Public vs Private Repositories

ECR supports both private and public repositories.

---

## Private Repository

```text
Organization Use
Restricted Access
IAM Controlled
```

---

## Public Repository

```text
Open Access
Public Images
Global Distribution
```

---

## Comparison

| Feature        | Private       | Public      |
| -------------- | ------------- | ----------- |
| Authentication | Required      | Optional    |
| Visibility     | Restricted    | Public      |
| Typical Use    | Internal Apps | Open Source |

---

## Benefits

```text
Flexibility
Controlled Access
Public Distribution
```

---

# 6. Creating Repositories

Repositories store container images.

---

## Create Repository

```bash
aws ecr create-repository \
--repository-name myapp
```

---

## Architecture

```text
Repository
     │
     ▼
Images
```

---

## Verify Repository

```bash
aws ecr describe-repositories
```

---

## Benefits

```text
Image Organization
Version Management
Access Control
```

---

# 7. Repository Management

Repositories can be listed, described, and deleted.

---

## List Repositories

```bash
aws ecr describe-repositories
```

---

## Describe Repository

```bash
aws ecr describe-repositories \
--repository-names myapp
```

---

## Delete Repository

```bash
aws ecr delete-repository \
--repository-name myapp
```

---

## Workflow

```text
Create
  │
  ▼
Store Images
  │
  ▼
Manage
```

---

## Benefits

```text
Organization
Control
Governance
```

---

# 8. Authentication

Authentication is required before pushing or pulling images.

---

## Login Workflow

```text
AWS CLI
    │
    ▼
Authentication Token
    │
    ▼
Docker Login
```

---

## Get Login Password

```bash
aws ecr get-login-password
```

---

## Login to ECR

```bash
aws ecr get-login-password \
| docker login \
--username AWS \
--password-stdin ACCOUNT.dkr.ecr.REGION.amazonaws.com
```

---

## Benefits

```text
Secure Access
IAM Integration
Token-Based Authentication
```

---

# 9. Docker and ECR Integration

Docker is used to build and push images.

---

## Workflow

```text
Docker Build
      │
      ▼
Docker Tag
      │
      ▼
Docker Push
      │
      ▼
Amazon ECR
```

---

## Common Commands

```bash
docker build
docker tag
docker push
docker pull
```

---

## Architecture

```text
Docker
   │
   ▼
Amazon ECR
```

---

## Benefits

```text
Native Integration
Simple Workflow
Automation
```

---

# 10. Image Tagging

Tags identify image versions.

---

## Example

```text
myapp:v1

myapp:v2

myapp:latest
```

---

## Tag Image

```bash
docker tag myapp:latest \
ACCOUNT.dkr.ecr.REGION.amazonaws.com/myapp:latest
```

---

## Architecture

```text
Repository
    │
    ▼
Image Tags
```

---

## Benefits

```text
Versioning
Tracking
Deployment Control
```

---

# 11. Pushing Images

Images are uploaded from Docker to ECR.

---

## Workflow

```text
Build
  │
  ▼
Tag
  │
  ▼
Push
```

---

## Push Image

```bash
docker push \
ACCOUNT.dkr.ecr.REGION.amazonaws.com/myapp:latest
```

---

## Architecture

```text
Docker Image
      │
      ▼
Amazon ECR
```

---

## Benefits

```text
Centralized Storage
Distribution
Version Control
```

---

# 12. Pulling Images

Images can be downloaded from ECR.

---

## Pull Image

```bash
docker pull \
ACCOUNT.dkr.ecr.REGION.amazonaws.com/myapp:latest
```

---

## Workflow

```text
Amazon ECR
     │
     ▼
Docker Host
```

---

## Common Use Cases

```text
EKS
ECS
EC2
CI/CD Pipelines
```

---

## Benefits

```text
Portability
Automation
Consistency
```

---

# 13. Repository Policies

Repository policies control access.

---

## Purpose

Manage permissions for repositories.

---

## Common Permissions

```text
Push Images
Pull Images
Delete Images
Manage Policies
```

---

## Architecture

```text
Repository
     │
     ▼
Policy
     │
     ▼
User Access
```

---

## Benefits

```text
Security
Access Control
Governance
```

---

# 14. Image Scanning

ECR can scan images for vulnerabilities.

---

## Workflow

```text
Image
  │
  ▼
Security Scan
  │
  ▼
Findings
```

---

## Vulnerability Categories

```text
Critical
High
Medium
Low
```

---

## Benefits

```text
Security
Compliance
Risk Reduction
```

---

## Use Cases

```text
Production Images
CI/CD Validation
Security Audits
```

---

# 15. Lifecycle Policies

Lifecycle policies automate image cleanup.

---

## Purpose

Remove unused images automatically.

---

## Workflow

```text
Old Images
     │
     ▼
Lifecycle Policy
     │
     ▼
Deletion
```

---

## Common Rules

```text
Keep Latest Images
Delete Untagged Images
Age-Based Cleanup
```

---

## Benefits

```text
Cost Optimization
Repository Cleanup
Automation
```

---

## Example Use Cases

```text
Development Repositories
CI/CD Pipelines
Large Image Collections
```

---

# 16. IAM Integration

AWS ECR integrates directly with AWS Identity and Access Management (IAM).

---

## Purpose

Control access to repositories and images.

---

## Architecture

```text id="p6m2wr"
IAM User/Role
      │
      ▼
IAM Policy
      │
      ▼
Amazon ECR
```

---

## Common Permissions

```text id="m8r4vn"
Push Images
Pull Images
Delete Images
Manage Repositories
```

---

## Benefits

```text id="k3n9wp"
Fine-Grained Access
Security
Centralized Management
```

---

# 17. ECR Security Best Practices

Security should be implemented throughout the image lifecycle.

---

## Recommendations

```text id="t7m1vk"
Enable Image Scanning
Use Least Privilege
Encrypt Images
Use Private Repositories
```

---

## Security Layers

```text id="v4p8wr"
IAM
Repository Policies
Encryption
Image Scanning
```

---

## Benefits

```text id="m2w7pn"
Compliance
Risk Reduction
Governance
```

---

# 18. Encryption

ECR supports encryption at rest.

---

## Architecture

```text id="r8n3vk"
Container Image
        │
        ▼
Encryption
        │
        ▼
Amazon ECR
```

---

## Encryption Options

```text id="k5m1wr"
AWS Managed Keys
Customer Managed KMS Keys
```

---

## Benefits

```text id="p9r4vn"
Data Protection
Compliance
Security
```

---

## Common Service

```text id="t1m8wp"
AWS KMS
```

---

# 19. Cross-Region Replication

ECR can automatically replicate images across AWS Regions.

---

## Architecture

```text id="v6n2pk"
ECR Region A
      │
      ▼
Replication
      │
      ▼
ECR Region B
```

---

## Benefits

```text id="m4r9wk"
Disaster Recovery
Global Availability
Reduced Latency
```

---

## Common Use Cases

```text id="k8p3vn"
Multi-Region Applications
Global Platforms
Backup Strategies
```

---

# 20. Cross-Account Access

Images can be shared across AWS accounts.

---

## Architecture

```text id="p2m7wr"
Account A
    │
    ▼
Repository Policy
    │
    ▼
Account B
```

---

## Benefits

```text id="v9r1pk"
Centralized Registries
Shared Services
Multi-Account Architecture
```

---

## Common Use Cases

```text id="m5n4wp"
Development Accounts
Production Accounts
Shared Platforms
```

---

# 21. ECR Public Gallery

ECR Public provides globally accessible repositories.

---

## Architecture

```text id="r3p8vn"
Public Repository
       │
       ▼
Internet Users
```

---

## Benefits

```text id="k1m6wr"
Open Source Distribution
Global Access
No AWS Account Required
```

---

## Common Use Cases

```text id="t8n2vk"
Open Source Images
Public Projects
Community Sharing
```

---

# 22. Monitoring with CloudWatch

CloudWatch monitors ECR activity and performance.

---

## Architecture

```text id="v4m9pr"
Amazon ECR
      │
      ▼
CloudWatch
      │
      ▼
Metrics
```

---

## Common Metrics

```text id="m7r3wk"
Pull Count
Push Count
Repository Activity
```

---

## Benefits

```text id="p5n1vr"
Visibility
Monitoring
Alerting
```

---

# 23. Logging with CloudTrail

CloudTrail records API activity.

---

## Architecture

```text id="k9m4wp"
AWS API Call
      │
      ▼
CloudTrail
      │
      ▼
Audit Logs
```

---

## Logged Activities

```text id="r2n8vk"
Repository Creation
Image Deletion
Policy Changes
Authentication Events
```

---

## Benefits

```text id="v1p7wr"
Auditing
Compliance
Security Investigations
```

---

# 24. EventBridge Integration

ECR events can trigger automated workflows.

---

## Architecture

```text id="m8r4pn"
Amazon ECR
      │
      ▼
EventBridge
      │
      ▼
Automation
```

---

## Common Events

```text id="k4n9wr"
Image Push
Image Scan Complete
Repository Updates
```

---

## Benefits

```text id="t6m2vk"
Automation
Notifications
Integration
```

---

# 25. ECS Integration

Amazon ECS can pull images directly from ECR.

---

## Architecture

```text id="v3p8wr"
Amazon ECR
      │
      ▼
ECS Task
      │
      ▼
Container
```

---

## Workflow

```text id="m1n7vk"
Push Image
     │
     ▼
ECR
     │
     ▼
ECS Deployment
```

---

## Benefits

```text id="r9m4wp"
Native Integration
Security
Automation
```

---

# 26. EKS Integration

Amazon EKS commonly uses ECR as its image registry.

---

## Architecture

```text id="k5p1vn"
Amazon ECR
      │
      ▼
Kubernetes Pod
      │
      ▼
Container
```

---

## Workflow

```text id="t2n8wr"
Build Image
     │
     ▼
Push ECR
     │
     ▼
Deploy Pod
```

---

## Benefits

```text id="v7m3pk"
Cloud Native
Scalable
Secure
```

---

# 27. Lambda Container Images

AWS Lambda supports container-based functions.

---

## Architecture

```text id="m4r9vn"
Container Image
       │
       ▼
Amazon ECR
       │
       ▼
AWS Lambda
```

---

## Benefits

```text id="k8n2wr"
Large Packages
Custom Runtimes
Simplified Deployment
```

---

## Common Use Cases

```text id="p1m7vk"
Serverless Applications
Custom Runtime Functions
ML Workloads
```

---

# 28. CI/CD Integration

ECR is commonly used in automated pipelines.

---

## Pipeline Architecture

```text id="v6p3wr"
Git Repository
       │
       ▼
CI Pipeline
       │
       ▼
Build Image
       │
       ▼
Push ECR
       │
       ▼
Deploy
```

---

## Common Tools

```text id="m9n4vk"
GitHub Actions
Jenkins
GitLab CI
AWS CodePipeline
```

---

## Benefits

```text id="r3m8wp"
Automation
Consistency
Rapid Delivery
```

---

# 29. Image Versioning Strategies

Proper tagging improves deployment control.

---

## Common Tags

```text id="k7n1vr"
latest
v1.0
v1.1
production
staging
```

---

## Architecture

```text id="t4m9wk"
Repository
    │
    ▼
Tags
    │
    ▼
Versions
```

---

## Best Practices

```text id="v2p8wr"
Avoid Latest in Production
Use Semantic Versioning
Maintain Release Tags
```

---

## Benefits

```text id="m5n3vk"
Traceability
Rollback Support
Deployment Safety
```

---

# 30. Troubleshooting

Troubleshooting helps resolve ECR issues.

---

## Verify Authentication

```bash id="r8m1wp"
aws ecr get-login-password
```

---

## List Repositories

```bash id="k2n7vr"
aws ecr describe-repositories
```

---

## List Images

```bash id="t9m4pk"
aws ecr list-images \
--repository-name myapp
```

---

## Common Issues

```text id="v1r8wn"
Authentication Failures
Access Denied Errors
Push Failures
Image Pull Failures
```

---

## Troubleshooting Workflow

```text id="m6p2vk"
Issue
 │
 ▼
Logs
 │
 ▼
Permissions
 │
 ▼
Resolution
```

---

# 31. Cost Optimization

Amazon ECR costs are based primarily on storage and data transfer.

---

## Cost Factors

```text id="m4p8wr"
Image Storage
Data Transfer
Cross-Region Replication
Image Scanning
```

---

## Architecture

```text id="k7n2vk"
Images
  │
  ▼
Storage Usage
  │
  ▼
Monthly Cost
```

---

## Optimization Techniques

```text id="v1m9pr"
Lifecycle Policies
Delete Unused Images
Reduce Duplicate Images
Use Image Compression
```

---

## Benefits

```text id="r5w3mn"
Lower Costs
Better Storage Management
Efficient Operations
```

---

# 32. High Availability

ECR is designed for high availability.

---

## Architecture

```text id="t8n4vk"
Amazon ECR
      │
      ▼
Multiple AWS AZs
```

---

## Features

```text id="m2r7wp"
Managed Infrastructure
Automatic Redundancy
Fault Tolerance
```

---

## Benefits

```text id="k9m1vn"
Reliability
Availability
Resilience
```

---

## Common Use Cases

```text id="v6p4wr"
Production Platforms
Enterprise Applications
Cloud Native Systems
```

---

# 33. Disaster Recovery

Disaster recovery ensures image availability during failures.

---

## Architecture

```text id="m8n3vk"
Primary Region
      │
      ▼
Replication
      │
      ▼
Secondary Region
```

---

## Recovery Workflow

```text id="r2w7pn"
Failure
   │
   ▼
Secondary Region
   │
   ▼
Restore Operations
```

---

## Best Practices

```text id="k4m9wr"
Enable Replication
Document Procedures
Test Recovery
```

---

## Benefits

```text id="t1n8vk"
Business Continuity
High Availability
Risk Reduction
```

---

# 34. Private Registry Best Practices

Private registries require governance and security.

---

## Recommendations

```text id="v3m7wp"
Enable Scanning
Use IAM Roles
Restrict Access
Encrypt Images
```

---

## Architecture

```text id="m7r2vk"
Private Repository
       │
       ▼
IAM Access Control
```

---

## Benefits

```text id="k8p1wn"
Security
Compliance
Governance
```

---

# 35. Production Best Practices

Production environments require strict controls.

---

## Recommendations

```text id="r4n9vp"
Enable Image Scanning
Use Lifecycle Policies
Implement Monitoring
Use Version Tags
```

---

## Production Workflow

```text id="t7m3wr"
Build
 │
 ▼
Scan
 │
 ▼
Push
 │
 ▼
Deploy
```

---

## Benefits

```text id="m5p8vk"
Reliability
Security
Operational Excellence
```

---

# 36. Real-World Architectures

---

## ECS Deployment

```text id="v9n1wp"
Developer
    │
    ▼
Amazon ECR
    │
    ▼
Amazon ECS
```

---

## EKS Deployment

```text id="k2m7vr"
Developer
    │
    ▼
Amazon ECR
    │
    ▼
Amazon EKS
```

---

## CI/CD Architecture

```text id="r8p4wn"
Git Repository
      │
      ▼
CI Pipeline
      │
      ▼
Amazon ECR
      │
      ▼
Deployment
```

---

## Multi-Region Architecture

```text id="m1w9pk"
ECR Region A
      │
      ▼
Replication
      │
      ▼
ECR Region B
```

---

# 37. ECR vs Docker Hub

| Feature                    | AWS ECR   | Docker Hub |
| -------------------------- | --------- | ---------- |
| AWS Native                 | Yes       | No         |
| IAM Integration            | Yes       | No         |
| Managed Service            | Yes       | Yes        |
| Public Images              | Limited   | Extensive  |
| Enterprise AWS Integration | Excellent | Limited    |

---

## Architecture

```text id="v6m2wr"
AWS Workloads
      │
      ▼
ECR

General Containers
      │
      ▼
Docker Hub
```

---

## Common Recommendation

```text id="k5r8vn"
ECR → AWS Environments

Docker Hub → General Usage
```

---

# 38. ECR vs Harbor

| Feature         | AWS ECR   | Harbor  |
| --------------- | --------- | ------- |
| Managed Service | Yes       | No      |
| Self-Hosted     | No        | Yes     |
| AWS Integration | Excellent | Limited |
| Image Scanning  | Yes       | Yes     |
| Replication     | Yes       | Yes     |

---

## Architecture

```text id="t2p7wk"
ECR
 │
 ▼
AWS Managed

Harbor
 │
 ▼
Self Managed
```

---

## Common Recommendation

```text id="m9n4vr"
ECR → AWS Native

Harbor → Multi-Cloud / On-Prem
```

---

# 39. ECR vs Artifact Registry

| Feature                | AWS ECR | Artifact Registry |
| ---------------------- | ------- | ----------------- |
| Cloud Provider         | AWS     | Google Cloud      |
| IAM Integration        | AWS IAM | Google IAM        |
| Kubernetes Integration | EKS     | GKE               |
| Managed Service        | Yes     | Yes               |

---

## Architecture

```text id="k1w8pr"
AWS → ECR

Google Cloud → Artifact Registry
```

---

## Benefits

```text id="v7m3pk"
Cloud Native Integration
Managed Operations
```

---

# 40. Common Use Cases

---

## Container Platforms

```text id="m4p9wr"
Amazon ECS
Amazon EKS
EC2 Containers
```

---

## CI/CD

```text id="r3n7vk"
Build
Push
Deploy
```

---

## Serverless

```text id="t8m2wp"
Lambda Container Images
```

---

## Enterprise Applications

```text id="k6r1vn"
Microservices
Cloud Native Platforms
Internal Registries
```

---

# 41. Interview Questions

---

## What is Amazon ECR?

Amazon Elastic Container Registry is a fully managed container image registry service.

---

## What is the difference between ECR and Docker Hub?

ECR is AWS-native and integrates with AWS services, while Docker Hub is a general-purpose container registry.

---

## What services commonly use ECR?

```text id="v2m8pk"
Amazon ECS
Amazon EKS
AWS Lambda
```

---

## How do you authenticate to ECR?

```bash id="p7r4wn"
aws ecr get-login-password
```

Used with Docker login.

---

## What is Image Scanning?

A feature that identifies vulnerabilities in container images.

---

## What are Lifecycle Policies?

Rules used to automatically delete old or unused images.

---

## What is Cross-Region Replication?

Automatic replication of container images between AWS Regions.

---

## Why use ECR with EKS?

Because EKS can securely pull container images directly from ECR using IAM integration.

---

# 42. Additional Resources

## Official Resources

* AWS ECR Documentation
* AWS CLI Documentation
* AWS IAM Documentation
* AWS ECS Documentation
* AWS EKS Documentation

---

## Learning Resources

* AWS Workshops
* AWS Skill Builder
* AWS Samples GitHub
* AWS Architecture Center

---

## Related Technologies

```text id="m8w3vr"
Docker
Harbor
Amazon ECS
Amazon EKS
AWS Lambda
```

---

# 43. Quick Reference

## Create Repository

```bash id="r5n1wp"
aws ecr create-repository \
--repository-name myapp
```

---

## List Repositories

```bash id="k9m4vn"
aws ecr describe-repositories
```

---

## Login

```bash id="t2p8wr"
aws ecr get-login-password
```

---

## Tag Image

```bash id="m6r3vk"
docker tag IMAGE TARGET
```

---

## Push Image

```bash id="v1n7wp"
docker push IMAGE
```

---

## Pull Image

```bash id="k4m2vr"
docker pull IMAGE
```

---

## List Images

```bash id="r8p9wn"
aws ecr list-images \
--repository-name myapp
```

---

## Delete Image

```bash id="t5m1vk"
aws ecr batch-delete-image
```

---

# 📎 Official Documentation

* [Amazon ECR Documentation](https://docs.aws.amazon.com/ecr/)
* [AWS CLI Documentation](https://docs.aws.amazon.com/cli/)
* [AWS IAM Documentation](https://docs.aws.amazon.com/iam/)
* [Amazon ECS Documentation](https://docs.aws.amazon.com/ecs/)
* [Amazon EKS Documentation](https://docs.aws.amazon.com/eks/)

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, DevOps engineering, cloud engineering, platform engineering, and container platform administration.

Primary references include:

* AWS ECR Documentation
* AWS IAM Documentation
* AWS ECS Documentation
* AWS EKS Documentation
* AWS CLI Documentation

Amazon ECR is a managed service provided by [Amazon Web Services](https://aws.amazon.com).

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

You now understand:

* Amazon ECR Fundamentals
* Repositories
* Authentication
* Docker Integration
* Image Tagging
* Image Scanning
* Lifecycle Policies
* IAM Integration
* Security
* Encryption
* Cross-Region Replication
* ECS Integration
* EKS Integration
* Lambda Integration
* CI/CD Pipelines
* Monitoring
* Troubleshooting

Amazon ECR provides:

```text id="q7m4vp"
Private Container Registry
+
AWS Native Integration
+
Image Security Scanning
+
Cross-Region Replication
+
High Availability
+
Managed Operations
```

and serves as the primary container registry service for modern AWS-based containerized and cloud-native workloads.

---

```md id="k3n8wr"
**Last Updated:** 2026
**Platform:** AWS ECR
**Version:** AWS Managed Service
**License:** Proprietary AWS Service

For latest information, visit https://docs.aws.amazon.com/ecr/
```