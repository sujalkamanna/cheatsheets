# Terraform on AWS Cheatsheet

![Terraform Logo](https://www.vectorlogo.zone/logos/terraformio/terraformio-icon.svg)

## Table of Contents

* [1. What is Terraform on AWS?](#1-what-is-terraform-on-aws)
* [2. Infrastructure as Code (IaC)](#2-infrastructure-as-code-iac)
* [3. Why Terraform for AWS?](#3-why-terraform-for-aws)
* [4. Terraform Architecture](#4-terraform-architecture)
* [5. Terraform Workflow](#5-terraform-workflow)
* [6. Installing Terraform](#6-installing-terraform)
* [7. AWS Authentication](#7-aws-authentication)
* [8. Terraform CLI Basics](#8-terraform-cli-basics)
* [9. Terraform Configuration Files](#9-terraform-configuration-files)
* [10. Terraform State Fundamentals](#10-terraform-state-fundamentals)
* [11. Terraform Lifecycle](#11-terraform-lifecycle)
* [12. Variables](#12-variables)
* [13. Outputs](#13-outputs)
* [14. Local Values](#14-local-values)
* [15. Data Sources](#15-data-sources)

---

# 1. What is Terraform on AWS?

Terraform is an Infrastructure as Code (IaC) tool used to provision and manage AWS resources using code.

Instead of manually creating resources through the AWS Console, Terraform automates infrastructure deployment.

---

## Traditional AWS Deployment

```text
Engineer
    │
    ▼
AWS Console
    │
    ▼
Manual Resources
```

---

## Terraform Deployment

```text
Terraform Code
       │
       ▼
Terraform Engine
       │
       ▼
AWS Resources
```

---

## Common AWS Resources Managed

```text
EC2
VPC
Subnets
IAM
S3
RDS
EKS
Load Balancers
```

---

## Benefits

```text
Automation
Version Control
Consistency
Scalability
Repeatability
```

---

# 2. Infrastructure as Code (IaC)

Infrastructure as Code means infrastructure is managed using code.

---

## Traditional Approach

```text
Login AWS Console
       │
       ▼
Create Resources
       │
       ▼
Manual Configuration
```

---

## IaC Approach

```text
Write Code
    │
    ▼
Deploy Infrastructure
    │
    ▼
Repeat Anywhere
```

---

## Advantages

```text
Automation
Consistency
Documentation
Collaboration
Disaster Recovery
```

---

## Example

Instead of:

```text
Create VPC Manually
Create EC2 Manually
Create S3 Manually
```

Use:

```text
terraform apply
```

---

# 3. Why Terraform for AWS?

Terraform is one of the most popular AWS automation tools.

---

## Key Reasons

```text
Multi-Cloud Support
Declarative Syntax
Large Community
Reusable Modules
State Management
```

---

## AWS Automation Example

```text
Terraform
    │
    ▼
AWS Provider
    │
    ▼
AWS APIs
```

---

## Real-World Uses

```text
Cloud Infrastructure
DevOps Pipelines
Kubernetes Clusters
Networking
Security Automation
```

---

## Benefits

```text
Faster Deployments
Reduced Errors
Infrastructure Consistency
```

---

# 4. Terraform Architecture

Terraform uses providers to communicate with cloud platforms.

---

## Architecture

```text
  Terraform Code
        │
        ▼
  Terraform Core
        │
        ▼
  AWS Provider
        │
        ▼
  AWS APIs
        │
        ▼
  AWS Resources
```

---

## Main Components

```text
Terraform Core
Providers
Resources
State File
Modules
```

---

## Workflow

```text
Code
 │
 ▼
Plan
 │
 ▼
Apply
 │
 ▼
Infrastructure
```

---

# 5. Terraform Workflow

Terraform follows a standard workflow.

---

## Workflow Diagram

```text
Write Code
     │
     ▼
terraform init
     │
     ▼
terraform validate
     │
     ▼
terraform plan
     │
     ▼
terraform apply
```

---

## Step 1

Initialize:

```bash
terraform init
```

---

## Step 2

Validate:

```bash
terraform validate
```

---

## Step 3

Preview:

```bash
terraform plan
```

---

## Step 4

Deploy:

```bash
terraform apply
```

---

## Step 5

Destroy (Optional)

```bash
terraform destroy
```

---

# 6. Installing Terraform

Terraform must be installed before use.

---

## Verify Installation

```bash
terraform version
```

---

## Common Installation Methods

```text
Package Manager
Zip Download
Official Repository
```

---

## Verify Binary

```bash
which terraform
```

---

## Expected Workflow

```text
Install
   │
   ▼
Verify
   │
   ▼
Configure AWS
```

---

# 7. AWS Authentication

Terraform must authenticate with AWS.

---

## Common Methods

```text
AWS CLI Credentials
Environment Variables
IAM Roles
AWS Profiles
```

---

## AWS CLI

Configure:

```bash
aws configure
```

---

## Verify Identity

```bash
aws sts get-caller-identity
```

---

## Environment Variables

```bash
export AWS_ACCESS_KEY_ID=xxxx

export AWS_SECRET_ACCESS_KEY=xxxx

export AWS_REGION=us-east-1
```

---

## Recommended Method

```text
IAM Roles
AWS Profiles
```

Avoid:

```text
Hardcoded Credentials
```

---

# 8. Terraform CLI Basics

Terraform CLI is used to manage infrastructure.

---

## Most Common Commands

Initialize:

```bash
terraform init
```

---

Validate:

```bash
terraform validate
```

---

Plan:

```bash
terraform plan
```

---

Apply:

```bash
terraform apply
```

---

Destroy:

```bash
terraform destroy
```

---

Format Code:

```bash
terraform fmt
```

---

Show State:

```bash
terraform show
```

---

# 9. Terraform Configuration Files

Terraform uses `.tf` files.

---

## Common Files

```text
main.tf
variables.tf
outputs.tf
providers.tf
terraform.tfvars
```

---

## Example Structure

```text
project/
│
├── main.tf
├── variables.tf
├── outputs.tf
├── providers.tf
└── terraform.tfvars
```

---

## Purpose

### main.tf

```text
Resources
```

---

### variables.tf

```text
Input Variables
```

---

### outputs.tf

```text
Output Values
```

---

### providers.tf

```text
Provider Configuration
```

---

# 10. Terraform State Fundamentals

Terraform stores infrastructure information in a state file.

---

## State File

```text
terraform.tfstate
```

---

## Purpose

Tracks:

```text
Resources
Attributes
Dependencies
```

---

## Architecture

```text
Terraform
     │
     ▼
State File
     │
     ▼
AWS Resources
```

---

## Why Important?

```text
Change Tracking
Dependency Resolution
Resource Mapping
```

---

## Warning

Never manually edit:

```text
terraform.tfstate
```

---

# 11. Terraform Lifecycle

Terraform resources follow a lifecycle.

---

## Lifecycle

```text
Create
  │
  ▼
Update
  │
  ▼
Destroy
```

---

## Terraform Process

```text
Plan
 │
 ▼
Create Resources
 │
 ▼
Manage Resources
 │
 ▼
Destroy Resources
```

---

## Lifecycle Benefits

```text
Predictability
Automation
Resource Tracking
```

---

# 12. Variables

Variables make configurations reusable.

---

## Purpose

Avoid hardcoding values.

---

## Example

```hcl
variable "region" {
  type = string
}
```

---

## Usage

```hcl
provider "aws" {
  region = var.region
}
```

---

## Benefits

```text
Flexibility
Reusability
Environment Separation
```

---

## Common Variable Types

```text
string
number
bool
list
map
object
```

---

# 13. Outputs

Outputs display information after deployment.

---

## Example

```hcl
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

---

## Usage

```text
Display Values
Pass Data
Share Information
```

---

## Architecture

```text
Terraform
    │
    ▼
Outputs
    │
    ▼
User
```

---

## Common Outputs

```text
Public IP
DNS Name
ARN
Resource IDs
```

---

# 14. Local Values

Locals simplify repeated expressions.

---

## Example

```hcl
locals {
  environment = "dev"
}
```

---

## Usage

```hcl
tags = {
  Environment = local.environment
}
```

---

## Benefits

```text
Cleaner Code
Reduced Repetition
Better Readability
```

---

## Architecture

```text
Locals
  │
  ▼
Resources
```

---

# 15. Data Sources

Data sources read existing AWS resources.

---

## Purpose

Use existing infrastructure.

---

## Example

```hcl
data "aws_vpc" "main" {
  default = true
}
```

---

## Architecture

```text
AWS Resource
      │
      ▼
Data Source
      │
      ▼
Terraform
```

---

## Common Data Sources

```text
VPC
AMI
Subnets
Availability Zones
IAM Roles
```

---

## Benefits

```text
Reusability
Dynamic Configuration
Existing Infrastructure Integration
```

---

# 16. Providers

Providers allow Terraform to interact with cloud platforms and services.

---

## Purpose

Terraform itself cannot create resources.

Providers act as plugins.

```text id="4w1c7p"
Terraform
    │
    ▼
Provider
    │
    ▼
Cloud Platform
```

---

## AWS Provider Example

```hcl id="7m3r8v"
provider "aws" {
  region = "us-east-1"
}
```

---

## Initialize Providers

```bash id="9k5x2n"
terraform init
```

---

## Common Providers

```text id="2q8t6j"
AWS
Azure
Google
Kubernetes
Docker
GitHub
```

---

## Benefits

```text id="5v1n9k"
Multi-Cloud Support
Extensibility
Automation
```

---

# 17. Resources

Resources are the building blocks of Terraform.

---

## Purpose

Resources represent infrastructure objects.

---

## Examples

```text id="8c4m7r"
EC2 Instance
S3 Bucket
VPC
IAM Role
RDS Database
```

---

## Resource Syntax

```hcl id="6j2w8p"
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

---

## Architecture

```text id="1r7v3m"
Terraform Resource
        │
        ▼
AWS Resource
```

---

## Resource Lifecycle

```text id="3k9x5n"
Create
Update
Destroy
```

---

# 18. EC2 with Terraform

EC2 instances can be provisioned automatically.

---

## Basic EC2 Example

```hcl id="7t4q9w"
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

---

## Deployment Flow

```text id="5n1j8r"
Terraform
    │
    ▼
EC2 Instance
```

---

## Common Attributes

```text id="8p6c4m"
AMI
Instance Type
Tags
Security Groups
Subnet
```

---

## Useful Outputs

```text id="4v2k7q"
Public IP
Private IP
Instance ID
```

---

# 19. VPC with Terraform

VPC provides network isolation.

---

## Architecture

```text id="6r8w3n"
AWS Account
     │
     ▼
VPC
     │
     ▼
Subnets
```

---

## Example

```hcl id="2m7v5p"
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}
```

---

## Components

```text id="9q4n8t"
VPC
Subnets
Route Tables
Internet Gateway
```

---

## Benefits

```text id="7w3k6m"
Isolation
Security
Scalability
```

---

# 20. Security Groups

Security Groups act as virtual firewalls.

---

## Purpose

Control inbound and outbound traffic.

---

## Architecture

```text id="5c8r1n"
Security Group
       │
       ▼
EC2 Instance
```

---

## Example

```hcl id="4m2v9k"
resource "aws_security_group" "web" {
  name = "web-sg"
}
```

---

## Common Rules

```text id="8j6p3w"
SSH 22
HTTP 80
HTTPS 443
```

---

## Best Practices

```text id="2x9k7m"
Least Privilege
Restrict SSH
Use Separate Groups
```

---

# 21. S3 Buckets

S3 provides object storage.

---

## Use Cases

```text id="6v4n2p"
Backups
Logs
Artifacts
Static Websites
Terraform State
```

---

## Example

```hcl id="1m8q5v"
resource "aws_s3_bucket" "logs" {
  bucket = "company-logs"
}
```

---

## Architecture

```text id="9k2r6n"
Terraform
    │
    ▼
S3 Bucket
```

---

## Benefits

```text id="4p7w8m"
Durability
Scalability
Low Cost
```

---

# 22. IAM Resources

IAM manages AWS permissions.

---

## Common Resources

```text id="7n4v2q"
Users
Roles
Policies
Groups
```

---

## Example IAM Role

```hcl id="3m8p5k"
resource "aws_iam_role" "app" {
  name = "app-role"
}
```

---

## Architecture

```text id="8w6n3r"
IAM Role
    │
    ▼
AWS Resources
```

---

## Best Practices

```text id="1q9v4m"
Least Privilege
Role-Based Access
Avoid Long-Term Keys
```

---

# 23. Modules

Modules are reusable Terraform components.

---

## Purpose

Avoid duplicating code.

---

## Architecture

```text id="7m2w8k"
Root Module
     │
     ▼
Child Modules
```

---

## Example

```hcl id="2p6n4v"
module "vpc" {
  source = "./modules/vpc"
}
```

---

## Benefits

```text id="9r4k7m"
Reusability
Standardization
Maintainability
```

---

# 24. Module Structure

A module follows a common layout.

---

## Example Structure

```text id="4x8m2q"
modules/
 └── vpc/
      ├── main.tf
      ├── variables.tf
      └── outputs.tf
```

---

## Common Files

```text id="6k1w9p"
main.tf
variables.tf
outputs.tf
README.md
```

---

## Architecture

```text id="3v7n5r"
Module
  │
  ├── Inputs
  ├── Resources
  └── Outputs
```

---

# 25. Module Best Practices

---

## Keep Modules Focused

Good:

```text id="8m4q2w"
VPC Module
IAM Module
EKS Module
```

---

Bad:

```text id="2n7k5v"
Everything Module
```

---

## Version Modules

```text id="6r3p9m"
Tag Releases
Track Changes
```

---

## Use Variables

Avoid:

```text id="1v8n4k"
Hardcoded Values
```

---

## Benefits

```text id="5p2m7w"
Reusable Infrastructure
Consistency
Scalability
```

---

# 26. Remote State

Remote State stores Terraform state centrally.

---

## Problem

Local State:

```text id="8k4r2m"
terraform.tfstate
```

exists only on one machine.

---

## Solution

```text id="4m9w6p"
Remote Backend
```

---

## Architecture

```text id="7v2n5k"
Terraform
    │
    ▼
Remote State
    │
    ▼
Team Access
```

---

## Benefits

```text id="2p8m4v"
Collaboration
Durability
Security
```

---

# 27. S3 Backend

The most common Terraform backend on AWS.

---

## Architecture

```text id="9n3k6r"
Terraform
    │
    ▼
S3 Bucket
    │
    ▼
State File
```

---

## Example

```hcl id="6m2v8p"
terraform {
  backend "s3" {
    bucket = "terraform-state"
    key    = "prod.tfstate"
    region = "us-east-1"
  }
}
```

---

## Benefits

```text id="3k7n5w"
Centralized State
Versioning
Reliability
```

---

# 28. DynamoDB Locking

Used to prevent concurrent state modifications.

---

## Why Needed?

Without locking:

```text id="8v4m2q"
User A Apply
User B Apply
```

can corrupt state.

---

## Architecture

```text id="5r9k3n"
Terraform
    │
    ▼
DynamoDB Lock
    │
    ▼
State File
```

---

## Benefits

```text id="1m7v4p"
Consistency
Protection
Team Collaboration
```

---

## Note

Historically used with:

```hcl id="7k2p9w"
dynamodb_table = "terraform-locks"
```

though newer Terraform workflows increasingly rely on backend-native locking where supported.

---

# 29. State Management

Managing state properly is critical.

---

## State Commands

List Resources:

```bash id="2v8m4n"
terraform state list
```

---

Show Resource:

```bash id="5k1p7r"
terraform state show RESOURCE
```

---

Move Resource:

```bash id="8n3v6m"
terraform state mv
```

---

Remove Resource:

```bash id="4p7k2w"
terraform state rm
```

---

## Best Practices

```text id="9r2m5v"
Use Remote State
Enable Versioning
Backup State
Protect Access
```

---

# 30. Terraform Workspaces

Workspaces allow multiple environments using the same code.

---

## Example Environments

```text id="3k8v4n"
dev
test
stage
prod
```

---

## Architecture

```text id="6m1p9w"
Terraform Code
       │
       ▼
Workspace
       │
 ┌─────┼─────┐
 ▼     ▼     ▼
Dev  Test  Prod
```

---

## Create Workspace

```bash id="8r4k2m"
terraform workspace new dev
```

---

## List Workspaces

```bash id="5n7p3v"
terraform workspace list
```

---

## Select Workspace

```bash id="2m9v6k"
terraform workspace select prod
```

---

## Benefits

```text id="7k3w1p"
Environment Isolation
Code Reuse
Simplified Management
```

---

# 31. Terraform Functions

Functions help transform and manipulate values.

---

## Purpose

Functions make configurations dynamic.

---

## Common Functions

```text id="f7m3kp"
length()
upper()
lower()
join()
split()
lookup()
format()
```

---

## Example

```hcl id="v2n8wr"
length(["a", "b", "c"])
```

Result:

```text id="m6q4pt"
3
```

---

## String Functions

```hcl id="c8r1mv"
upper("terraform")
```

Result:

```text id="t5n7pk"
TERRAFORM
```

---

## Benefits

```text id="x4w8qn"
Dynamic Values
Code Reusability
Reduced Hardcoding
```

---

# 32. Conditional Expressions

Terraform supports if-else style logic.

---

## Syntax

```hcl id="r9m2kv"
condition ? true_value : false_value
```

---

## Example

```hcl id="k6p4wt"
instance_type =
var.environment == "prod"
? "t3.large"
: "t3.micro"
```

---

## Architecture

```text id="u3n7rx"
Condition
    │
 ┌──┴──┐
 ▼     ▼
True False
```

---

## Use Cases

```text id="a5v8mk"
Environment Selection
Resource Sizing
Feature Toggles
```

---

# 33. Loops and for_each

Terraform can create multiple resources automatically.

---

## Purpose

Avoid repetitive code.

---

## Example

```hcl id="q8m4wp"
for_each = toset([
  "dev",
  "test",
  "prod"
])
```

---

## Architecture

```text id="b7n2kr"
Input List
    │
    ▼
for_each
    │
 ┌──┼──┐
 ▼  ▼  ▼
A  B  C
```

---

## Benefits

```text id="v5p9mx"
Automation
Scalability
Cleaner Code
```

---

# 34. count Meta Argument

Creates multiple copies of a resource.

---

## Example

```hcl id="g2r8wn"
resource "aws_instance" "web" {
  count = 3
}
```

---

## Result

```text id="p4m7kt"
web[0]
web[1]
web[2]
```

---

## Architecture

```text id="x8n3wv"
count = 3
    │
    ▼
3 Resources
```

---

## count vs for_each

| count   | for_each         |
| ------- | ---------------- |
| Numeric | Collection Based |
| Simpler | More Flexible    |

---

# 35. Dynamic Blocks

Generate nested configuration blocks dynamically.

---

## Purpose

Reduce repetitive configuration.

---

## Example

```hcl id="j6m2rp"
dynamic "ingress" {
}
```

---

## Architecture

```text id="k5v9wn"
Input Data
    │
    ▼
Dynamic Block
    │
    ▼
Generated Config
```

---

## Benefits

```text id="f3r8mx"
Reusable Code
Less Duplication
Flexibility
```

---

# 36. Provisioners

Provisioners execute commands during resource creation.

---

## Types

```text id="n7w2kp"
local-exec
remote-exec
```

---

## Example

```hcl id="p8m4rv"
provisioner "local-exec" {
  command = "echo deployed"
}
```

---

## Architecture

```text id="v1n6wt"
Terraform
    │
    ▼
Resource Created
    │
    ▼
Command Executes
```

---

## Warning

Provisioners should be used only when necessary.

Preferred:

```text id="r5k9mq"
Cloud-Init
User Data
Configuration Management
```

---

# 37. Null Resource

Used to execute actions without creating infrastructure.

---

## Example

```hcl id="m2w7pr"
resource "null_resource" "example" {
}
```

---

## Common Uses

```text id="y4n8kv"
Scripts
Automation Tasks
CI/CD Actions
```

---

## Architecture

```text id="q6m3wp"
Terraform
    │
    ▼
Null Resource
    │
    ▼
Action
```

---

## Benefits

```text id="x9r2kn"
Flexibility
Workflow Integration
```

---

# 38. Terraform Import

Import existing AWS resources into Terraform state.

---

## Problem

Resource exists but Terraform does not manage it.

---

## Solution

```bash id="h7m4wr"
terraform import
```

---

## Example

```bash id="u3n9kp"
terraform import aws_s3_bucket.logs my-bucket
```

---

## Architecture

```text id="d5v8mx"
Existing Resource
       │
       ▼
Import
       │
       ▼
Terraform State
```

---

## Benefits

```text id="k8r1wt"
Migration
Infrastructure Adoption
State Management
```

---

# 39. Terraform Taint & Replace

Force recreation of resources.

---

## Modern Approach

```bash id="w4m7kp"
terraform apply -replace=RESOURCE
```

---

## Example

```bash id="n6v2mr"
terraform apply \
-replace=aws_instance.web
```

---

## Architecture

```text id="p9k3wv"
Resource
   │
   ▼
Marked For Replace
   │
   ▼
Recreated
```

---

## Use Cases

```text id="f2r8mn"
Corrupted Resources
Configuration Issues
Infrastructure Repair
```

---

# 40. Terraform Drift Detection

Drift occurs when infrastructure changes outside Terraform.

---

## Example

```text id="y8n4wp"
Terraform Created EC2
         │
         ▼
Manual AWS Change
         │
         ▼
Drift
```

---

## Detect Drift

```bash id="v7m1kr"
terraform plan
```

---

## Workflow

```text id="m3w8qn"
Terraform State
       │
       ▼
AWS Resources
       │
       ▼
Comparison
```

---

## Benefits

```text id="k4p7vx"
Consistency
Compliance
Governance
```

---

# 41. Secrets Management

Secrets should never be stored in code.

---

## Examples

```text id="p6n2mw"
Passwords
API Keys
Tokens
Certificates
```

---

## Avoid

```text id="r8w5kp"
Hardcoded Credentials
Plain Text Secrets
```

---

## Recommended

```text id="x3m7vn"
Secrets Manager
Parameter Store
```

---

# 42. AWS Secrets Manager

Managed secret storage service.

---

## Architecture

```text id="w5r2mk"
Terraform
    │
    ▼
Secrets Manager
    │
    ▼
Applications
```

---

## Benefits

```text id="v9n4px"
Encryption
Rotation
Centralized Management
```

---

## Common Uses

```text id="q7m1wr"
Database Passwords
API Credentials
Tokens
```

---

# 43. AWS Systems Manager Parameter Store

Stores configuration and secrets.

---

## Architecture

```text id="t6p8mn"
Terraform
    │
    ▼
Parameter Store
    │
    ▼
Applications
```

---

## Types

```text id="m1v4wk"
String
StringList
SecureString
```

---

## Benefits

```text id="r5n7px"
Configuration Management
Encryption
Centralized Storage
```

---

# 44. Terraform Validation

Validation helps catch errors early.

---

## Validate Syntax

```bash id="y3m8kr"
terraform validate
```

---

## Format Code

```bash id="u7n2wp"
terraform fmt
```

---

## Plan Review

```bash id="v4p9mx"
terraform plan
```

---

## Workflow

```text id="k6r1wn"
Write Code
    │
    ▼
Validate
    │
    ▼
Plan
    │
    ▼
Apply
```

---

## Benefits

```text id="f8m5pv"
Reduced Errors
Improved Quality
Safer Deployments
```

---

# 45. CI/CD Integration

Terraform is commonly integrated into CI/CD pipelines.

---

## Pipeline Flow

```text id="p2w8mn"
Git Commit
     │
     ▼
CI Pipeline
     │
     ▼
Terraform Plan
     │
     ▼
Approval
     │
     ▼
Terraform Apply
```

---

## Popular Platforms

```text id="n5r4wk"
GitHub Actions
GitLab CI
Jenkins
Azure DevOps
CircleCI
```

---

## Typical Stages

```text id="u1m7px"
Lint
Validate
Plan
Review
Apply
```

---

## Benefits

```text id="w8n3mr"
Automation
Consistency
Faster Deployments
Auditability
```

---

## DevOps Architecture Example

```text id="k9p2wv"
Developer
    │
    ▼
Git Repository
    │
    ▼
CI/CD Pipeline
    │
    ▼
Terraform
    │
    ▼
AWS Infrastructure
```

---

# 46. Terraform Best Practices

Following best practices improves reliability, security, and maintainability.

---

## Organize Code Properly

Recommended Structure:

```text id="v7m3kp"
project/
│
├── modules/
├── environments/
├── main.tf
├── variables.tf
├── outputs.tf
└── providers.tf
```

---

## Use Modules

Avoid:

```text id="q2n8wr"
Large Monolithic Files
```

Use:

```text id="m5r1vx"
Reusable Modules
```

---

## Secure State Files

```text id="w8p4kn"
Remote State
Encryption
Access Controls
Versioning
```

---

## Avoid Hardcoded Values

Use:

```text id="r4m7wp"
Variables
Locals
Data Sources
```

---

## Best Practices Summary

```text id="k6n2mv"
Use Modules
Use Remote State
Version Control Everything
Protect Secrets
Use CI/CD
```

---

# 47. High Availability Architectures

Terraform can automate highly available AWS deployments.

---

## Multi-AZ Architecture

```text id="a8p3wr"
Users
  │
  ▼
Load Balancer
  │
 ┌┼┐
 ▼▼▼
AZ1 AZ2 AZ3
```

---

## Terraform Managed Resources

```text id="v5m8kn"
VPC
Subnets
Load Balancers
Auto Scaling Groups
Databases
```

---

## Benefits

```text id="x2r6wp"
Fault Tolerance
Redundancy
Business Continuity
```

---

## Common Services

```text id="t7n1mv"
ALB
ASG
RDS Multi-AZ
EKS
```

---

# 48. Multi-Environment Design

Organizations typically maintain multiple environments.

---

## Common Environments

```text id="g3p8wk"
Development
Testing
Staging
Production
```

---

## Architecture

```text id="m9r2vx"
Terraform Code
      │
      ▼
Workspaces
      │
 ┌────┼────┐
 ▼    ▼    ▼
Dev Stage Prod
```

---

## Benefits

```text id="k4n7wp"
Environment Isolation
Safer Deployments
Code Reuse
```

---

## Common Approaches

```text id="p8m3vr"
Workspaces
Separate State Files
Separate Accounts
```

---

# 49. Disaster Recovery

Terraform supports disaster recovery through Infrastructure as Code.

---

## Recovery Architecture

```text id="d7p2wk"
Terraform Code
       │
       ▼
Git Repository
       │
       ▼
AWS Infrastructure
```

---

## Recovery Workflow

```text id="f1m8vx"
Infrastructure Lost
         │
         ▼
terraform apply
         │
         ▼
Infrastructure Restored
```

---

## Components

```text id="n6r4wp"
Code Repository
Remote State
Backups
Version Control
```

---

## Best Practices

```text id="v3p7mk"
Backup State
Store Code in Git
Document Recovery Steps
Test Recovery Plans
```

---

# 50. Cost Optimization

Terraform helps control cloud spending.

---

## Common Cost Issues

```text id="m4w9pn"
Oversized Instances
Unused Resources
Idle Load Balancers
Unused Volumes
```

---

## Optimization Techniques

```text id="q8r2vx"
Auto Scaling
Resource Tagging
Scheduled Shutdowns
Right Sizing
```

---

## Terraform Benefits

```text id="p1n7wk"
Visibility
Automation
Consistency
```

---

## Cost Governance

```text id="t5m3vr"
Tags
Budgets
Policies
Monitoring
```

---

# 51. Real-World Terraform AWS Architectures

---

## Basic Web Application

```text id="k2p8wm"
Users
  │
  ▼
ALB
  │
  ▼
EC2
  │
  ▼
RDS
```

---

## EKS Platform

```text id="m7r4vx"
Terraform
    │
    ▼
EKS
    │
    ▼
Applications
```

---

## DevOps Platform

```text id="w4n9pk"
Git
 │
 ▼
CI/CD
 │
 ▼
Terraform
 │
 ▼
AWS
```

---

## Enterprise Architecture

```text id="r6p2mv"
AWS Organizations
        │
        ▼
Multiple Accounts
        │
        ▼
Terraform Modules
```

---

## GitOps Infrastructure

```text id="n3w8vr"
Git Repository
      │
      ▼
Pipeline
      │
      ▼
Terraform Apply
```

---

# 52. Troubleshooting

---

## Initialization Problems

Check:

```bash id="c8m4wp"
terraform init
```

---

## Validation Errors

```bash id="v2r7nk"
terraform validate
```

---

## State Issues

```bash id="m5p1vx"
terraform state list
```

---

## Drift Issues

```bash id="q7n3wr"
terraform plan
```

---

## Debug Logging

```bash id="t9m2vk"
export TF_LOG=DEBUG
```

---

## Common Problems

```text id="p4w8mn"
Authentication
State Corruption
Provider Versions
Resource Dependencies
```

---

# 53. Interview Questions

---

## What is Terraform?

Terraform is an Infrastructure as Code tool used to provision and manage infrastructure using declarative configuration files.

---

## What is a Provider?

A provider is a plugin that allows Terraform to communicate with external platforms such as AWS.

---

## What is Terraform State?

Terraform state is a mapping between configuration and real-world infrastructure resources.

---

## Why Use Remote State?

```text id="n8r4wp"
Collaboration
Durability
Security
Versioning
```

---

## Difference Between count and for_each?

| count       | for_each         |
| ----------- | ---------------- |
| Numeric     | Collection Based |
| Index Based | Key Based        |

---

## What is a Module?

A reusable collection of Terraform resources.

---

## What is Drift?

Changes made outside Terraform causing infrastructure to differ from the Terraform state.

---

## What is Terraform Plan?

A preview of changes Terraform will make before applying them.

---

## Why Use DynamoDB Locking?

Prevents simultaneous state modifications.

---

## What is Terraform Workspace?

A feature used to manage multiple environments from the same codebase.

---

# 54. Additional Resources

## Official Resources

* Terraform Documentation
* Terraform Registry
* AWS Provider Documentation
* HashiCorp Learn

---

## Certifications

* HashiCorp Terraform Associate
* AWS Certified Cloud Practitioner
* AWS Certified Solutions Architect
* AWS Certified DevOps Engineer

---

## Learning Platforms

* HashiCorp Learn
* AWS Workshops
* GitHub Terraform Examples
* Terraform Registry

---

# 55. Quick Reference

## Initialization

```bash id="v6n2wp"
terraform init
```

---

## Formatting

```bash id="r3m8vk"
terraform fmt
```

---

## Validation

```bash id="k7p4wn"
terraform validate
```

---

## Planning

```bash id="m1r9vx"
terraform plan
```

---

## Apply

```bash id="p8n3wk"
terraform apply
```

---

## Destroy

```bash id="q4m7vr"
terraform destroy
```

---

## State

```bash id="t2p8wm"
terraform state list
```

---

## Workspaces

```bash id="w5n1vx"
terraform workspace list

terraform workspace select prod
```

---

## Outputs

```bash id="n9r4wp"
terraform output
```

---

# 📎 Official Documentation

* Terraform Documentation: https://developer.hashicorp.com/terraform
* Terraform Registry: https://registry.terraform.io
* AWS Provider Documentation: https://registry.terraform.io/providers/hashicorp/aws
* HashiCorp Learn: https://developer.hashicorp.com/learn

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, DevOps engineering, cloud engineering, platform engineering, and Infrastructure as Code practices.

Primary references include:

* Terraform Documentation
* Terraform Registry
* AWS Provider Documentation
* HashiCorp Learn

Terraform is a product of HashiCorp. AWS, EC2, S3, IAM, VPC, RDS, EKS and related services are trademarks of Amazon Web Services, Inc.

All trademarks belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

You now understand:

* Infrastructure as Code (IaC)
* Terraform Fundamentals
* AWS Authentication
* Providers
* Resources
* EC2 Automation
* VPC Automation
* IAM Automation
* S3 Backends
* State Management
* DynamoDB Locking
* Modules
* Workspaces
* Functions
* Loops
* Dynamic Blocks
* Secrets Management
* CI/CD Integration
* High Availability
* Disaster Recovery
* Cost Optimization

Terraform on AWS enables:

```text id="f8m2vr"
Infrastructure as Code
+
Automation
+
Version Control
+
AWS Integration
+
Scalability
+
Repeatability
+
Cloud Governance
```

to build, manage, and scale modern cloud infrastructure efficiently.

---

```md
**Last Updated:** 2026
**Platform:** Terraform on AWS
**Version:** Terraform 1.13+ / AWS Provider 6.x+
**License:** Terraform (BUSL/MPL Components), AWS Services (Proprietary)

For latest information, visit https://developer.hashicorp.com/terraform
```