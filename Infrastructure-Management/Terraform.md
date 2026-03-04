# Terraform Cheatsheet

![Terraform Logo](https://www.terraform.io/images/og-image.png)

---

## Table of Contents
1. [What is Terraform?](#1-what-is-terraform)
2. [Installation & Setup](#2-installation--setup)
3. [Terraform Basics](#3-terraform-basics)
4. [Configuration Files](#4-configuration-files)
5. [Providers](#5-providers)
6. [Resources & Data Sources](#6-resources--data-sources)
7. [Variables & Outputs](#7-variables--outputs)
8. [State Management](#8-state-management)
9. [Modules](#9-modules)
10. [Functions & Expressions](#10-functions--expressions)
11. [Provisioners & Meta-Arguments](#11-provisioners--meta-arguments)
12. [Workspaces](#12-workspaces)
13. [Deployment](#13-deployment)
14. [Best Practices](#14-best-practices)
15. [Troubleshooting](#15-troubleshooting)

---

## 1. What is Terraform?

Terraform is an open-source Infrastructure as Code (IaC) tool that allows you to define, provision, and manage cloud infrastructure using declarative configuration files. It supports all major cloud providers and on-premises infrastructure.

**Key Features:**
- Declarative configuration language (HCL)
- Multi-cloud support (AWS, Azure, GCP, etc.)
- State management
- Execution plans
- Resource dependency tracking
- Modular architecture
- Workspace management
- Remote state storage
- Drift detection
- Provider ecosystem

---

## 2. Installation & Setup

### Install Terraform

```bash
# macOS with Homebrew
brew install terraform

# Ubuntu/Debian
curl https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform

# CentOS/RHEL
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum -y install terraform

# Windows (Chocolatey)
choco install terraform

# Windows (direct)
# Download from https://www.terraform.io/downloads.html
# Add to PATH

# Verify installation
terraform version
terraform -version
```

### Cloud Provider Setup

```bash
# AWS - Configure credentials
aws configure

# Or set environment variables
export AWS_ACCESS_KEY_ID="your-key"
export AWS_SECRET_ACCESS_KEY="your-secret"
export AWS_REGION="us-east-1"

# Azure - Login
az login

# GCP - Authenticate
gcloud auth application-default login

# Kubernetes
export KUBECONFIG=~/.kube/config
```

### Project Initialization

```bash
# Create project directory
mkdir terraform-project && cd terraform-project

# Initialize Terraform
terraform init

# Creates .terraform directory and downloads providers

# Check Terraform version
terraform version

# Validate configuration
terraform validate

# Format configuration files
terraform fmt

# Recursively format files
terraform fmt -recursive
```

---

## 3. Terraform Basics

### Directory Structure

```
terraform-project/
├── main.tf                     # Main configuration
├── variables.tf                # Variable definitions
├── outputs.tf                  # Output definitions
├── terraform.tfvars            # Variable values
├── providers.tf                # Provider configuration
├── locals.tf                   # Local values
├── versions.tf                 # Version constraints
├── .terraform/                 # Downloaded providers
├── .terraform.lock.hcl         # Dependency lock file
├── terraform.tfstate           # State file (don't commit!)
├── terraform.tfstate.backup    # State backup
├── modules/                    # Reusable modules
│   ├── vpc/
│   ├── security_group/
│   └── compute/
└── environments/               # Environment configs
    ├── dev/
    └── prod/
```

### Basic Workflow

```
1. Write Configuration (HCL)
   ↓
2. terraform init (Initialize)
   ↓
3. terraform plan (Review changes)
   ↓
4. terraform apply (Apply changes)
   ↓
5. terraform destroy (Clean up)
```

### Simple Configuration

```hcl
# main.tf
terraform {
  required_version = ">= 1.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "example" {
  bucket = "my-unique-bucket-name"
}

output "bucket_name" {
  value = aws_s3_bucket.example.id
}
```

---

## 4. Configuration Files

### HCL Syntax

```hcl
# Blocks
block_type "block_label" "block_name" {
  key = value
}

# Comments
# Single-line comment
/* Multi-line
   comment */

# Strings
string = "value"
interpolated = "Hello, ${var.name}!"

# Numbers
number = 42
float = 3.14

# Booleans
enabled = true
disabled = false

# Lists
list = ["item1", "item2", "item3"]

# Maps/Objects
map = {
  key1 = "value1"
  key2 = "value2"
}

# Tuples (typed list)
tuple = ["string", 42, true]

# Set (unique values)
set = toset(["a", "b", "c"])

# Conditional expression
value = var.environment == "prod" ? "large" : "small"

# For expressions
list = [for item in var.items : item.name]
map = {for k, v in var.items : k => v.name}

# Splat expression
values = aws_instance.example[*].id
```

### Terraform Block

```hcl
# versions.tf
terraform {
  required_version = ">= 1.0, < 2.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = "~> 2.0"
    }
  }
  
  # Remote state configuration
  cloud {
    organization = "my-org"
    
    workspaces {
      name = "example"
    }
  }
  
  # Or use backend
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}
```

### Provider Configuration

```hcl
# providers.tf
provider "aws" {
  region = "us-east-1"
  
  default_tags {
    tags = {
      Environment = var.environment
      Project     = var.project_name
      ManagedBy   = "Terraform"
    }
  }
}

provider "aws" {
  alias  = "secondary"
  region = "us-west-2"
}

provider "azurerm" {
  features {}
  
  subscription_id = var.azure_subscription_id
  tenant_id       = var.azure_tenant_id
}

provider "kubernetes" {
  host                   = aws_eks_cluster.example.endpoint
  cluster_ca_certificate = base64decode(aws_eks_cluster.example.certificate_authority[0].data)
  token                  = data.aws_eks_cluster_auth.example.token
}
```

---

## 5. Providers

### AWS Provider

```hcl
# providers.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
  
  assume_role {
    role_arn = "arn:aws:iam::ACCOUNT_ID:role/TerraformRole"
  }
  
  default_tags {
    tags = {
      Environment = var.environment
      Project     = var.project
      CreatedBy   = "Terraform"
    }
  }
  
  # Endpoint overrides for local testing
  skip_credentials_validation = false
  skip_region_validation      = false
  skip_requesting_account_id  = false
}

# Authenticate with AWS
# Option 1: AWS CLI
# aws configure

# Option 2: Environment variables
# export AWS_ACCESS_KEY_ID="..."
# export AWS_SECRET_ACCESS_KEY="..."
# export AWS_SESSION_TOKEN="..."

# Option 3: AWS SSO
# aws sso login --profile my-profile
# export AWS_PROFILE=my-profile

# Option 4: IAM Role (EC2, ECS, Lambda)
# Automatic - no configuration needed
```

### Azure Provider

```hcl
# providers.tf
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }
  }
}

provider "azurerm" {
  features {}
  
  subscription_id = var.azure_subscription_id
  tenant_id       = var.azure_tenant_id
  client_id       = var.client_id
  client_secret   = var.client_secret
  
  skip_provider_registration = false
}

# Authenticate with Azure
# Option 1: Azure CLI
# az login

# Option 2: Environment variables
# export ARM_SUBSCRIPTION_ID="..."
# export ARM_CLIENT_ID="..."
# export ARM_CLIENT_SECRET="..."
# export ARM_TENANT_ID="..."

# Option 3: Managed Identity
# Running on Azure VM/AKS
```

### GCP Provider

```hcl
# providers.tf
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "~> 5.0"
    }
  }
}

provider "google" {
  project = var.gcp_project
  region  = var.gcp_region
  zone    = var.gcp_zone
  
  credentials = file(var.gcp_credentials_file)
}

# Authenticate with GCP
# Option 1: GCloud CLI
# gcloud auth application-default login

# Option 2: Service account JSON
# export GOOGLE_APPLICATION_CREDENTIALS="/path/to/key.json"

# Option 3: Workload Identity (GKE)
# Automatic - uses pod service account
```

### Multiple Providers

```hcl
# Deploy to multiple regions
provider "aws" {
  region = "us-east-1"
}

provider "aws" {
  alias  = "us-west"
  region = "us-west-2"
}

provider "aws" {
  alias  = "eu-west"
  region = "eu-west-1"
}

# Use providers
resource "aws_instance" "primary" {
  provider      = aws
  instance_type = "t3.micro"
  ami           = data.aws_ami.ubuntu.id
}

resource "aws_instance" "secondary" {
  provider      = aws.us-west
  instance_type = "t3.micro"
  ami           = data.aws_ami.ubuntu.id
}

resource "aws_instance" "europe" {
  provider      = aws.eu-west
  instance_type = "t3.micro"
  ami           = data.aws_ami.ubuntu.id
}
```

---

## 6. Resources & Data Sources

### AWS Resources

```hcl
# VPC and Networking
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = {
    Name = "main-vpc"
  }
}

resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.1.0/24"
  availability_zone       = data.aws_availability_zones.available.names[0]
  map_public_ip_on_launch = true
  
  tags = {
    Name = "public-subnet"
  }
}

resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
  
  tags = {
    Name = "main-igw"
  }
}

resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id
  
  route {
    cidr_block      = "0.0.0.0/0"
    gateway_id      = aws_internet_gateway.main.id
  }
  
  tags = {
    Name = "public-rt"
  }
}

resource "aws_route_table_association" "public" {
  subnet_id      = aws_subnet.public.id
  route_table_id = aws_route_table.public.id
}

# Security Groups
resource "aws_security_group" "web" {
  name        = "web-sg"
  description = "Security group for web servers"
  vpc_id      = aws_vpc.main.id
  
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = {
    Name = "web-sg"
  }
}

# EC2 Instances
resource "aws_instance" "web" {
  count                = 3
  ami                  = data.aws_ami.ubuntu.id
  instance_type        = "t3.micro"
  subnet_id            = aws_subnet.public.id
  vpc_security_group_ids = [aws_security_group.web.id]
  
  user_data = base64encode(file("${path.module}/user_data.sh"))
  
  tags = {
    Name = "web-server-${count.index + 1}"
  }
  
  depends_on = [aws_internet_gateway.main]
}

# Load Balancer
resource "aws_lb" "main" {
  name               = "main-alb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.web.id]
  subnets            = [aws_subnet.public.id]
  
  enable_deletion_protection = false
  
  tags = {
    Name = "main-alb"
  }
}

resource "aws_lb_target_group" "web" {
  name        = "web-tg"
  port        = 80
  protocol    = "HTTP"
  vpc_id      = aws_vpc.main.id
  
  health_check {
    healthy_threshold   = 2
    unhealthy_threshold = 2
    timeout             = 3
    interval            = 30
    path                = "/"
    matcher             = "200"
  }
  
  tags = {
    Name = "web-tg"
  }
}

resource "aws_lb_target_group_attachment" "web" {
  count            = length(aws_instance.web)
  target_group_arn = aws_lb_target_group.web.arn
  target_id        = aws_instance.web[count.index].id
  port             = 80
}

resource "aws_lb_listener" "web" {
  load_balancer_arn = aws_lb.main.arn
  port              = "80"
  protocol          = "HTTP"
  
  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.web.arn
  }
}

# S3 Bucket
resource "aws_s3_bucket" "main" {
  bucket = "my-unique-bucket-${var.environment}-${data.aws_caller_identity.current.account_id}"
  
  tags = {
    Name = "main-bucket"
  }
}

resource "aws_s3_bucket_versioning" "main" {
  bucket = aws_s3_bucket.main.id
  
  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_s3_bucket_server_side_encryption_configuration" "main" {
  bucket = aws_s3_bucket.main.id
  
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}

# RDS Database
resource "aws_db_subnet_group" "main" {
  name       = "main-db-subnet-group"
  subnet_ids = [aws_subnet.public.id, aws_subnet.private.id]
  
  tags = {
    Name = "main-db-subnet-group"
  }
}

resource "aws_db_instance" "main" {
  allocated_storage    = 20
  storage_type         = "gp2"
  engine               = "postgres"
  engine_version       = "14.7"
  instance_class       = "db.t3.micro"
  
  db_name  = "mydb"
  username = "postgres"
  password = var.db_password
  
  db_subnet_group_name   = aws_db_subnet_group.main.name
  vpc_security_group_ids = [aws_security_group.db.id]
  
  skip_final_snapshot            = true
  copy_tags_to_snapshot          = true
  backup_retention_period        = 7
  multi_az                       = var.environment == "prod" ? true : false
  
  tags = {
    Name = "main-db"
  }
}

# IAM Role and Policy
resource "aws_iam_role" "ec2_role" {
  name = "ec2-role"
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
      }
    ]
  })
  
  tags = {
    Name = "ec2-role"
  }
}

resource "aws_iam_role_policy" "s3_access" {
  name = "s3-access"
  role = aws_iam_role.ec2_role.id
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "s3:GetObject",
          "s3:PutObject"
        ]
        Resource = "${aws_s3_bucket.main.arn}/*"
      }
    ]
  })
}

resource "aws_iam_instance_profile" "ec2" {
  name = "ec2-profile"
  role = aws_iam_role.ec2_role.name
}

# CloudWatch Alarm
resource "aws_cloudwatch_metric_alarm" "cpu" {
  alarm_name          = "high-cpu-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = "2"
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = "300"
  statistic           = "Average"
  threshold           = "80"
  alarm_description   = "This alarm monitors CPU utilization"
  
  dimensions = {
    InstanceId = aws_instance.web[0].id
  }
}

# SNS Topic
resource "aws_sns_topic" "alerts" {
  name = "alerts-topic"
  
  tags = {
    Name = "alerts-topic"
  }
}

resource "aws_sns_topic_subscription" "alerts_email" {
  topic_arn = aws_sns_topic.alerts.arn
  protocol  = "email"
  endpoint  = var.alert_email
}

# SQS Queue
resource "aws_sqs_queue" "main" {
  name                       = "main-queue"
  delay_seconds              = 0
  max_message_size           = 262144
  message_retention_seconds  = 345600
  receive_wait_time_seconds  = 0
  visibility_timeout_seconds = 30
  
  tags = {
    Name = "main-queue"
  }
}

# Lambda Function
resource "aws_lambda_function" "main" {
  filename      = "lambda.zip"
  function_name = "main-function"
  role          = aws_iam_role.lambda_role.arn
  handler       = "index.handler"
  runtime       = "python3.11"
  
  source_code_hash = filebase64sha256("lambda.zip")
  
  environment {
    variables = {
      ENVIRONMENT = var.environment
    }
  }
  
  tags = {
    Name = "main-function"
  }
}

# DynamoDB Table
resource "aws_dynamodb_table" "main" {
  name           = "main-table"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "id"
  
  attribute {
    name = "id"
    type = "S"
  }
  
  ttl {
    attribute_name = "expire_at"
    enabled        = true
  }
  
  tags = {
    Name = "main-table"
  }
}
```

### Data Sources

```hcl
# Get current AWS account ID
data "aws_caller_identity" "current" {}

# Get available AZs
data "aws_availability_zones" "available" {
  state = "available"
}

# Get latest Ubuntu AMI
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"] # Canonical
  
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
  
  filter {
    name   = "root-device-type"
    values = ["ebs"]
  }
  
  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
}

# Reference in resources
resource "aws_instance" "example" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
}

output "ami_id" {
  value = data.aws_ami.ubuntu.id
}

output "account_id" {
  value = data.aws_caller_identity.current.account_id
}

output "availability_zones" {
  value = data.aws_availability_zones.available.names
}
```

---

## 7. Variables & Outputs

### Variable Definition

```hcl
# variables.tf
variable "aws_region" {
  type        = string
  description = "AWS region"
  default     = "us-east-1"
}

variable "environment" {
  type        = string
  description = "Environment name"
  
  validation {
    condition     = contains(["dev", "test", "prod"], var.environment)
    error_message = "Environment must be dev, test, or prod."
  }
}

variable "instance_count" {
  type        = number
  description = "Number of instances"
  default     = 1
  
  validation {
    condition     = var.instance_count > 0 && var.instance_count <= 10
    error_message = "Instance count must be between 1 and 10."
  }
}

variable "enable_ssl" {
  type        = bool
  description = "Enable SSL"
  default     = false
}

variable "tags" {
  type        = map(string)
  description = "Resource tags"
  default = {
    Project = "MyProject"
    Owner   = "DevOps"
  }
}

variable "availability_zones" {
  type        = list(string)
  description = "Availability zones"
  default     = ["us-east-1a", "us-east-1b"]
}

variable "subnet_config" {
  type = object({
    cidr_block       = string
    availability_zone = string
  })
  description = "Subnet configuration"
}

variable "db_config" {
  type = object({
    engine   = string
    version  = string
    instance = string
  })
  description = "Database configuration"
  
  sensitive = false
}

variable "db_password" {
  type        = string
  description = "Database password"
  sensitive   = true
  
  validation {
    condition     = length(var.db_password) >= 12
    error_message = "Database password must be at least 12 characters."
  }
}

variable "instance_type" {
  type        = string
  description = "EC2 instance type"
  default     = "t3.micro"
  nullable    = false
}
```

### Variable Assignment

```hcl
# terraform.tfvars
aws_region    = "us-west-2"
environment   = "prod"
instance_count = 3
enable_ssl    = true

tags = {
  Project    = "MyProject"
  Owner      = "DevOps"
  CostCenter = "Engineering"
}

availability_zones = ["us-west-2a", "us-west-2b", "us-west-2c"]

subnet_config = {
  cidr_block        = "10.0.1.0/24"
  availability_zone = "us-west-2a"
}

db_config = {
  engine   = "postgres"
  version  = "14"
  instance = "db.t3.micro"
}

# Environment-specific files
# terraform.dev.tfvars
environment    = "dev"
instance_count = 1

# terraform.prod.tfvars
environment    = "prod"
instance_count = 3
```

### Using Variables

```hcl
# main.tf
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  
  tags = merge(
    var.tags,
    {
      Name        = "main-vpc"
      Environment = var.environment
    }
  )
}

resource "aws_instance" "web" {
  count             = var.instance_count
  ami               = data.aws_ami.ubuntu.id
  instance_type     = var.instance_type
  availability_zone = var.availability_zones[count.index % length(var.availability_zones)]
  
  tags = {
    Name = "web-${count.index + 1}"
  }
}

resource "aws_db_instance" "main" {
  engine               = var.db_config.engine
  engine_version       = var.db_config.version
  instance_class       = var.db_config.instance
  password             = var.db_password
  
  multi_az             = var.environment == "prod" ? true : false
  backup_retention_period = var.environment == "prod" ? 30 : 7
}
```

### Outputs

```hcl
# outputs.tf
output "vpc_id" {
  value       = aws_vpc.main.id
  description = "VPC ID"
}

output "instance_ids" {
  value       = aws_instance.web[*].id
  description = "Instance IDs"
}

output "instance_public_ips" {
  value       = aws_instance.web[*].public_ip
  description = "Instance public IPs"
}

output "load_balancer_dns" {
  value       = aws_lb.main.dns_name
  description = "Load balancer DNS name"
}

output "database_endpoint" {
  value       = aws_db_instance.main.endpoint
  description = "Database endpoint"
  sensitive   = true
}

output "infrastructure_summary" {
  value = {
    vpc_id     = aws_vpc.main.id
    instances  = aws_instance.web[*].id
    load_balancer = aws_lb.main.dns_name
    database   = aws_db_instance.main.endpoint
  }
  description = "Infrastructure summary"
}

output "resource_tags" {
  value       = var.tags
  description = "Resource tags"
}
```

### Local Values

```hcl
# locals.tf
locals {
  common_tags = merge(
    var.tags,
    {
      CreatedAt = timestamp()
      CreatedBy = "Terraform"
    }
  )
  
  instance_type = var.environment == "prod" ? "t3.large" : "t3.micro"
  
  db_backup_retention = {
    dev  = 7
    test = 14
    prod = 30
  }
  
  naming_prefix = "${var.project_name}-${var.environment}"
}

# Use locals
resource "aws_instance" "web" {
  instance_type = local.instance_type
  
  tags = merge(
    local.common_tags,
    {
      Name = "${local.naming_prefix}-web"
    }
  )
}

resource "aws_db_instance" "main" {
  backup_retention_period = local.db_backup_retention[var.environment]
  
  tags = local.common_tags
}
```

---

## 8. State Management

### Local State

```bash
# terraform.tfstate - Local state file
# Contains all resource state in JSON format

# View state
terraform state list
terraform state show aws_instance.web[0]
terraform state show aws_vpc.main

# Modify state (dangerous!)
terraform state mv aws_instance.web[0] aws_instance.web[1]
terraform state rm aws_instance.web[0]
terraform state replace-provider hashicorp/aws registry.terraform.io/hashicorp/aws

# Pull state
terraform state pull > state.json

# Push state
terraform state push state.json

# Lock state
terraform state lock

# Unlock state
terraform state unlock
```

### Remote State (S3 + DynamoDB)

```hcl
# versions.tf
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}

# Create S3 bucket and DynamoDB table first
# bootstrap.tf
resource "aws_s3_bucket" "terraform_state" {
  bucket = "my-terraform-state"
  
  lifecycle {
    prevent_destroy = true
  }
  
  tags = {
    Name = "Terraform state"
  }
}

resource "aws_s3_bucket_versioning" "terraform_state" {
  bucket = aws_s3_bucket.terraform_state.id
  
  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_s3_bucket_server_side_encryption_configuration" "terraform_state" {
  bucket = aws_s3_bucket.terraform_state.id
  
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}

resource "aws_dynamodb_table" "terraform_locks" {
  name           = "terraform-locks"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "LockID"
  
  attribute {
    name = "LockID"
    type = "S"
  }
  
  lifecycle {
    prevent_destroy = true
  }
  
  tags = {
    Name = "Terraform state lock table"
  }
}
```

### Terraform Cloud

```hcl
# versions.tf
terraform {
  cloud {
    organization = "my-organization"
    
    workspaces {
      name = "my-workspace"
    }
  }
}

# Alternative workspace configuration
terraform {
  cloud {
    organization = "my-organization"
    
    workspaces {
      tags = ["production"]
    }
  }
}
```

```bash
# Login to Terraform Cloud
terraform login

# Create workspace
terraform workspace new prod

# Work with workspaces
terraform workspace list
terraform workspace select dev
terraform workspace delete dev
terraform workspace show
```

### State Locking

```bash
# Manual locking (useful for concurrent operations)
terraform state lock

# Unlock state
terraform state unlock <lock-id>

# DynamoDB table provides automatic locking
# Lock table must have:
# - LockID as primary key
# - Hash key type: String
```

---

## 9. Modules

### Module Structure

```
modules/
├── vpc/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── README.md
├── security_group/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── README.md
└── compute/
    ├── main.tf
    ├── variables.tf
    ├── outputs.tf
    └── README.md
```

### Create Module

```hcl
# modules/vpc/variables.tf
variable "vpc_name" {
  type        = string
  description = "VPC name"
}

variable "cidr_block" {
  type        = string
  description = "VPC CIDR block"
  default     = "10.0.0.0/16"
}

variable "enable_dns_hostnames" {
  type        = bool
  description = "Enable DNS hostnames"
  default     = true
}

variable "tags" {
  type        = map(string)
  description = "Tags to apply"
  default     = {}
}

# modules/vpc/main.tf
resource "aws_vpc" "main" {
  cidr_block           = var.cidr_block
  enable_dns_hostnames = var.enable_dns_hostnames
  enable_dns_support   = true
  
  tags = merge(
    var.tags,
    {
      Name = var.vpc_name
    }
  )
}

resource "aws_subnet" "main" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = data.aws_availability_zones.available.names[0]
  
  tags = {
    Name = "${var.vpc_name}-subnet"
  }
}

data "aws_availability_zones" "available" {
  state = "available"
}

# modules/vpc/outputs.tf
output "vpc_id" {
  value       = aws_vpc.main.id
  description = "VPC ID"
}

output "vpc_cidr" {
  value       = aws_vpc.main.cidr_block
  description = "VPC CIDR block"
}

output "subnet_id" {
  value       = aws_subnet.main.id
  description = "Subnet ID"
}
```

### Use Module

```hcl
# main.tf
module "vpc" {
  source = "./modules/vpc"
  
  vpc_name = "main-vpc"
  cidr_block = "10.0.0.0/16"
  enable_dns_hostnames = true
  
  tags = {
    Environment = var.environment
    Project     = var.project_name
  }
}

module "security_group" {
  source = "./modules/security_group"
  
  vpc_id = module.vpc.vpc_id
  name   = "web-sg"
  
  ingress_rules = [
    {
      from_port   = 80
      to_port     = 80
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    },
    {
      from_port   = 443
      to_port     = 443
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  ]
}

module "compute" {
  source = "./modules/compute"
  
  instance_count    = 3
  instance_type     = "t3.micro"
  ami               = data.aws_ami.ubuntu.id
  subnet_id         = module.vpc.subnet_id
  security_group_id = module.security_group.id
  
  tags = {
    Environment = var.environment
  }
}

# Access module outputs
output "vpc_id" {
  value = module.vpc.vpc_id
}

output "instance_ids" {
  value = module.compute.instance_ids
}
```

### Terraform Registry

```hcl
# Use modules from Terraform Registry
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.0"
  
  name = "main-vpc"
  cidr = "10.0.0.0/16"
  
  azs            = ["us-east-1a", "us-east-1b"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets = ["10.0.101.0/24", "10.0.102.0/24"]
  
  enable_nat_gateway = true
  enable_vpn_gateway = true
  
  tags = {
    Environment = "prod"
  }
}

module "security_group" {
  source  = "terraform-aws-modules/security-group/aws"
  version = "5.0"
  
  name        = "web-sg"
  description = "Web server security group"
  vpc_id      = module.vpc.vpc_id
  
  ingress_cidr_blocks = ["0.0.0.0/0"]
  ingress_rules       = ["http-80-tcp", "https-443-tcp"]
  
  egress_rules = ["all-all"]
  
  tags = {
    Environment = "prod"
  }
}
```

---

## 10. Functions & Expressions

### String Functions

```hcl
# String concatenation
concatenated = "${var.name}-${var.environment}"

# String functions
lowercase = lower("HELLO")  # "hello"
uppercase = upper("hello")  # "HELLO"
trimmed = trim("  hello  ")  # "hello"
substr = substr("hello", 1, 3)  # "ell"
startswith = startswith("hello", "hel")  # true
endswith = endswith("hello", "lo")  # true
contains = contains("hello", "ll")  # true
replace = replace("hello", "ll", "y")  # "heyo"

# String splitting
split_result = split(",", "a,b,c")  # ["a", "b", "c"]
join_result = join(",", ["a", "b", "c"])  # "a,b,c"

# Regular expressions
regex = regex("[0-9]+", "abc123def")  # "123"
regexall = regexall("[0-9]+", "a1b2c3")  # ["1", "2", "3"]
```

### Number Functions

```hcl
# Math functions
min_value = min(1, 2, 3)  # 1
max_value = max(1, 2, 3)  # 3
ceil_value = ceil(1.2)  # 2
floor_value = floor(1.9)  # 1
round_value = round(1.5)  # 2

# Type conversion
num_from_str = tonumber("42")  # 42
str_from_num = tostring(42)  # "42"
```

### List Functions

```hcl
# List operations
length_of_list = length(["a", "b", "c"])  # 3
reversed = reverse(["a", "b", "c"])  # ["c", "b", "a"]
sorted = sort(["c", "a", "b"])  # ["a", "b", "c"]
distinct = distinct(["a", "b", "a"])  # ["a", "b"]
unique = unique(["a", "b", "a"])  # ["a", "b"]
concat_lists = concat(["a"], ["b"], ["c"])  # ["a", "b", "c"]

# List contains
list_contains = contains(["a", "b", "c"], "b")  # true
index = index(["a", "b", "c"], "b")  # 1

# Slice
sliced = slice(["a", "b", "c", "d"], 1, 3)  # ["b", "c"]

# Flatten
flattened = flatten([["a"], ["b", "c"]])  # ["a", "b", "c"]

# Compact
compacted = compact(["a", "", "b", null, "c"])  # ["a", "b", "c"]
```

### Map Functions

```hcl
# Map operations
keys_list = keys(var.tags)  # ["key1", "key2"]
values_list = values(var.tags)  # ["value1", "value2"]

# Merge maps
merged_maps = merge(var.default_tags, var.custom_tags)

# Map lookup
value = lookup(var.tags, "Environment", "default")

# Map transformation
map_keys = {for k in keys(var.tags) : k => upper(k)}
map_values = {for k, v in var.tags : k => upper(v)}

# Filter map
filtered = {for k, v in var.tags : k => v if k != "Environment"}
```

### Conditional & Logic Functions

```hcl
# Ternary operator
result = var.environment == "prod" ? "large" : "small"

# Conditional functions
coalesce = coalesce(var.custom_value, var.default_value)

# Try-catch
try_result = try(var.optional_var, "default_value")

# Logical operators
and_result = true && false  # false
or_result = true || false  # true
not_result = !true  # false
```

### File & Path Functions

```hcl
# File operations
file_content = file("${path.module}/config.txt")
file_base64 = filebase64("${path.module}/image.png")
file_exists = fileexists("${path.module}/config.txt")  # true/false

# Path functions
module_path = path.module  # Current module directory
root_path = path.root  # Root directory
cwd_path = path.cwd  # Current working directory

# Base64 encoding
encoded = base64encode("hello")  # "aGVsbG8="
decoded = base64decode("aGVsbG8=")  # "hello"

# JSON encoding
json_encoded = jsonencode(var.tags)
json_decoded = jsondecode(var.json_string)

# YAML encoding
yaml_encoded = yamlencode(var.config)
yaml_decoded = yamldecode(var.yaml_string)
```

### Type Functions

```hcl
# Type checking
is_string = type("hello") == "string"  # true
is_number = type(42) == "number"  # true
is_bool = type(true) == "bool"  # true
is_list = type([]) == "list"  # true
is_map = type({}) == "object"  # true

# Type conversion
to_string = tostring(42)  # "42"
to_number = tonumber("42")  # 42
to_list = tolist("value")  # ["value"]
to_map = tomap({"key" = "value"})  # {"key" = "value"}

# Type checking function
can_convert = can(tonumber(var.value))  # true/false
```

### For Expressions

```hcl
# For with list
doubled = [for num in [1, 2, 3] : num * 2]  # [2, 4, 6]

# For with condition
evens = [for num in [1, 2, 3, 4, 5] : num if num % 2 == 0]  # [2, 4]

# For with map
transformed = {for k, v in var.tags : k => upper(v)}

# For with grouping
grouped = {for item in var.items : item.category => item.name...}

# For with filtering and transformation
result = [for item in var.items : item.name if item.active]

# Splat expression (shorthand)
instance_ids = aws_instance.web[*].id
instance_ips = aws_instance.web[*].public_ip
```

---

## 11. Provisioners & Meta-Arguments

### Provisioners (Use Sparingly)

```hcl
# Local-exec provisioner
resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  
  provisioner "local-exec" {
    command = "echo ${self.public_ip} > /tmp/ip.txt"
  }
  
  provisioner "local-exec" {
    when    = destroy
    command = "echo 'Instance ${self.public_ip} is being destroyed'"
  }
}

# Remote-exec provisioner
resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  key_name      = aws_key_pair.deployer.key_name
  
  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx",
      "sudo systemctl start nginx"
    ]
    
    connection {
      type        = "ssh"
      user        = "ubuntu"
      private_key = file("~/.ssh/id_rsa")
      host        = self.public_ip
    }
  }
}

# File provisioner
resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  key_name      = aws_key_pair.deployer.key_name
  
  provisioner "file" {
    source      = "local/path/config.conf"
    destination = "/tmp/config.conf"
    
    connection {
      type        = "ssh"
      user        = "ubuntu"
      private_key = file("~/.ssh/id_rsa")
      host        = self.public_ip
    }
  }
}

# User data (preferred over provisioners)
resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  
  user_data = base64encode(templatefile("${path.module}/user_data.sh", {
    environment = var.environment
    project     = var.project_name
  }))
}
```

### Meta-Arguments

```hcl
# count - Create multiple instances
resource "aws_instance" "web" {
  count         = var.instance_count
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  
  tags = {
    Name = "web-${count.index + 1}"
  }
}

# Access count values
output "instance_ids" {
  value = aws_instance.web[*].id
}

# for_each - Create multiple instances from map
resource "aws_instance" "servers" {
  for_each = {
    web1 = "t3.micro"
    web2 = "t3.small"
    db1  = "t3.large"
  }
  
  ami           = data.aws_ami.ubuntu.id
  instance_type = each.value
  
  tags = {
    Name = each.key
  }
}

# Access for_each values
output "instance_ids" {
  value = {for name, instance in aws_instance.servers : name => instance.id}
}

# depends_on - Explicit dependencies
resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  
  depends_on = [
    aws_internet_gateway.main,
    aws_subnet.public
  ]
}

# lifecycle - Lifecycle rules
resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  
  lifecycle {
    # Prevent accidental destruction
    prevent_destroy = true
    
    # Ignore certain attribute changes
    ignore_changes = [
      ami,
      user_data
    ]
    
    # Create new before destroying old
    create_before_destroy = true
  }
}

# dynamic blocks - Create dynamic nested blocks
resource "aws_security_group" "main" {
  name = "main-sg"
  
  dynamic "ingress" {
    for_each = var.ingress_rules
    
    content {
      from_port   = ingress.value.from_port
      to_port     = ingress.value.to_port
      protocol    = ingress.value.protocol
      cidr_blocks = ingress.value.cidr_blocks
    }
  }
}
```

---

## 12. Workspaces

### Workspace Management

```bash
# List workspaces
terraform workspace list

# Create new workspace
terraform workspace new dev
terraform workspace new test
terraform workspace new prod

# Select workspace
terraform workspace select dev
terraform workspace select prod

# Show current workspace
terraform workspace show

# Delete workspace
terraform workspace delete dev

# Delete workspace safely
terraform workspace list
terraform workspace select default
terraform workspace delete dev
```

### Using Workspaces

```hcl
# Use workspace name in configuration
variable "instance_count" {
  type    = number
  default = {
    default = 1
    dev     = 1
    test    = 2
    prod    = 3
  }[terraform.workspace]
}

variable "instance_type" {
  type    = string
  default = {
    default = "t3.micro"
    dev     = "t3.micro"
    test    = "t3.small"
    prod    = "t3.large"
  }[terraform.workspace]
}

# Reference workspace
locals {
  environment = terraform.workspace
}

resource "aws_instance" "web" {
  count         = var.instance_count
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type
  
  tags = {
    Environment = local.environment
    Workspace   = terraform.workspace
  }
}
```

---

## 13. Deployment

### Planning & Applying

```bash
# Initialize working directory
terraform init

# Format code
terraform fmt
terraform fmt -recursive

# Validate syntax
terraform validate

# Preview changes
terraform plan
terraform plan -out=tfplan

# Apply plan
terraform apply
terraform apply tfplan

# Apply with auto-approval
terraform apply -auto-approve

# Apply specific resource
terraform apply -target aws_instance.web[0]
terraform apply -target module.vpc

# Apply with variables
terraform apply -var "instance_count=5"
terraform apply -var-file="prod.tfvars"

# Destroy infrastructure
terraform destroy
terraform destroy -auto-approve
terraform destroy -target aws_instance.web[0]

# Refresh state
terraform refresh

# Validate without applying
terraform plan -out=tfplan
# Review changes
terraform plan -destroy
```

### Output & Debugging

```bash
# Show state
terraform state list
terraform state show aws_instance.web[0]

# Show outputs
terraform output
terraform output vpc_id
terraform output -json

# Show plan
terraform show tfplan
terraform show terraform.tfstate

# Debug
TF_LOG=DEBUG terraform apply
TF_LOG=TRACE terraform plan

# Graph resources
terraform graph
terraform graph | dot -Tpng > graph.png

# Console (interactive shell)
terraform console
```

### Importing Resources

```bash
# Import existing resource
terraform import aws_instance.web i-1234567890abcdef0

# Import with module
terraform import module.vpc.aws_vpc.main vpc-12345678

# Import to state file
terraform import -state=new.tfstate aws_instance.web i-1234567890abcdef0
```

---

## 14. Best Practices

### Project Organization

```
production/
├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
├── providers.tf
├── versions.tf
└── modules/
    ├── vpc/
    ├── security_group/
    └── compute/

development/
├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
├── providers.tf
└── modules/ → (symlink or separate)
```

### Module Best Practices

```hcl
# Good: Clear inputs and outputs
module "vpc" {
  source = "./modules/vpc"
  
  name            = var.vpc_name
  cidr_block      = var.cidr_block
  availability_zones = var.availability_zones
  
  tags = var.tags
}

# Good: Descriptive naming
variable "vpc_cidr_block" {
  type        = string
  description = "CIDR block for VPC"
}

variable "enable_nat_gateway" {
  type        = bool
  description = "Enable NAT Gateway"
  default     = true
}

# Good: Documentation
output "vpc_id" {
  value       = aws_vpc.main.id
  description = "The ID of the VPC"
}
```

### Security Best Practices

```hcl
# Use sensitive for passwords/keys
variable "db_password" {
  type        = string
  description = "Database password"
  sensitive   = true
}

# Don't hardcode secrets
# Wrong:
password = "supersecret"

# Right:
password = var.db_password  # Set via environment or var file

# Use AWS Secrets Manager
data "aws_secretsmanager_secret_version" "db_password" {
  secret_id = "my-database-password"
}

resource "aws_db_instance" "main" {
  password = data.aws_secretsmanager_secret_version.db_password.secret_string
}

# Use IAM for authentication
resource "aws_iam_role" "terraform" {
  name = "terraform-role"
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Effect = "Allow"
        Principal = {
          AWS = "arn:aws:iam::ACCOUNT:user/terraform"
        }
      }
    ]
  })
}

# Enable versioning on state bucket
resource "aws_s3_bucket_versioning" "terraform_state" {
  bucket = aws_s3_bucket.terraform_state.id
  
  versioning_configuration {
    status = "Enabled"
  }
}

# Enable encryption on state bucket
resource "aws_s3_bucket_server_side_encryption_configuration" "terraform_state" {
  bucket = aws_s3_bucket.terraform_state.id
  
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}

# Enable MFA for state modifications
# Configure via backend configuration
```

### Naming Conventions

```hcl
# Use consistent naming
locals {
  project  = "myproject"
  env      = var.environment
  prefix   = "${local.project}-${local.env}"
  
  common_tags = {
    Project     = local.project
    Environment = local.env
    ManagedBy   = "Terraform"
    CreatedAt   = timestamp()
  }
}

# Apply to resources
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  
  tags = merge(
    local.common_tags,
    {
      Name = "${local.prefix}-vpc"
    }
  )
}

resource "aws_subnet" "main" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.1.0/24"
  
  tags = merge(
    local.common_tags,
    {
      Name = "${local.prefix}-subnet"
    }
  )
}
```

### DRY Principle

```hcl
# Use locals to avoid repetition
locals {
  common_tags = {
    Project     = "MyProject"
    Environment = var.environment
    ManagedBy   = "Terraform"
  }
  
  instance_config = {
    dev = {
      instance_type = "t3.micro"
      count         = 1
    }
    prod = {
      instance_type = "t3.large"
      count         = 3
    }
  }
}

# Reference locals
resource "aws_instance" "web" {
  count         = local.instance_config[var.environment].count
  instance_type = local.instance_config[var.environment].instance_type
  ami           = data.aws_ami.ubuntu.id
  
  tags = local.common_tags
}
```

---

## 15. Troubleshooting

### Common Issues

```bash
# Issue: State lock
# Error: Error acquiring the state lock
# Solution: Check DynamoDB table, force unlock if needed
terraform force-unlock <LOCK_ID>

# Issue: Provider not found
# Error: Error finding version(s) matching...
# Solution: Run init again
terraform init -upgrade

# Issue: Variable not set
# Error: var.instance_count evaluated to null
# Solution: Set variable or provide default value
terraform apply -var="instance_count=2"

# Issue: Resource already exists
# Error: Error creating resource...already exists
# Solution: Import or rename resource
terraform import aws_instance.web i-1234567890abcdef0

# Issue: Invalid resource name
# Error: Invalid resource name...only alphanumeric characters and underscores allowed
# Solution: Use valid naming conventions

# Issue: API rate limiting
# Error: ThrottlingException
# Solution: Wait and retry, or set provisioned throughput
```

### Debugging

```bash
# Enable debug logging
TF_LOG=DEBUG terraform apply

# Save logs to file
TF_LOG=DEBUG TF_LOG_PATH=/tmp/terraform.log terraform apply

# Inspect state
terraform state list
terraform state show aws_instance.web[0]

# Dry-run
terraform plan -out=tfplan
# Review tfplan carefully before applying

# Graph resources
terraform graph > graph.dot
dot -Tpng graph.dot -o graph.png

# Console (interactive)
terraform console
> aws_instance.web[0].id
> var.environment

# Inspect data source
terraform console
> data.aws_ami.ubuntu.id

# Show all resources
terraform show
terraform show terraform.tfstate | less
```

### Performance Optimization

```hcl
# Parallelize operations
# terraform apply -parallelism=20

# Skip dependency sorting
# terraform apply -skip-dependency-sort

# Cache provider plugins
# export TF_PLUGIN_CACHE_DIR="$HOME/.terraform.d/plugin-cache"

# Use count instead of for_each for performance
# for_each is slower for large sets
```

### Testing

```hcl
# Terraform testing example
# test.tf
run "basic_deployment" {
  command = plan
  
  assert {
    condition     = length(aws_instance.web) > 0
    error_message = "Should create instances"
  }
  
  assert {
    condition     = aws_vpc.main.cidr_block == "10.0.0.0/16"
    error_message = "VPC CIDR should be 10.0.0.0/16"
  }
}
```

---

## Quick Reference Commands

```bash
# Core Commands
terraform init                              # Initialize
terraform validate                          # Validate syntax
terraform fmt                               # Format code
terraform plan                              # Preview changes
terraform apply                             # Deploy
terraform destroy                           # Clean up

# State Management
terraform state list                        # List resources
terraform state show aws_instance.web       # Show resource state
terraform state mv old new                  # Rename resource
terraform state rm aws_instance.web[0]      # Remove from state
terraform refresh                           # Refresh state

# Workspaces
terraform workspace list                    # List workspaces
terraform workspace new dev                 # Create workspace
terraform workspace select dev              # Select workspace
terraform workspace delete dev              # Delete workspace

# Variable/Output
terraform apply -var="key=value"            # Set variable
terraform apply -var-file="prod.tfvars"    # Load variables
terraform output                            # Show outputs
terraform output vpc_id                     # Show specific output

# Debugging
TF_LOG=DEBUG terraform apply                # Debug logging
terraform console                           # Interactive shell
terraform graph                             # Show resource graph
terraform show                              # Show state

# Import
terraform import aws_instance.web i-123456  # Import resource

# Cleanup
terraform fmt -recursive                    # Format all files
terraform validate                          # Validate all
```

---

## Essential Terraform Concepts

### Resources
Infrastructure components you want to create/manage

### State
Current state of your infrastructure stored locally or remotely

### Providers
Plugins that interact with cloud providers (AWS, Azure, GCP)

### Variables
Input values that make configurations reusable and dynamic

### Outputs
Values exported after infrastructure deployment

### Modules
Reusable components combining multiple resources

### Workspace
Isolated infrastructure environments (dev, test, prod)

### Data Sources
Read-only information from provider

### Backend
Remote storage for state files (S3, Terraform Cloud, etc.)

---

## File Checklists

### .gitignore

```
# Local .terraform directories
**/.terraform/*

# .tfstate files
*.tfstate
*.tfstate.*

# Crash log files
crash.log
crash.*.log

# Exclude all .tfvars files except examples
*.tfvars
*.tfvars.json
!example.tfvars

# Ignore override files
override.tf
override.tf.json
*_override.tf
*_override.tf.json

# Ignore CLI configuration files
.terraformrc
terraform.rc

# Ignore plan files
*.tfplan

# Ignore Mac .DS_Store files
.DS_Store

# IDE
.vscode/
.idea/
*.swp
*.swo
*~

# Logs
*.log
```

---

## Resources

| Resource | URL |
|----------|-----|
| **Official Documentation** | https://www.terraform.io/docs |
| **Terraform Registry** | https://registry.terraform.io |
| **AWS Provider** | https://registry.terraform.io/providers/hashicorp/aws |
| **Azure Provider** | https://registry.terraform.io/providers/hashicorp/azurerm |
| **GCP Provider** | https://registry.terraform.io/providers/hashicorp/google |
| **Best Practices** | https://www.terraform.io/docs/language |
| **Community Forum** | https://discuss.hashicorp.com/c/terraform |

---

**Last Updated:** 2026
**Terraform Version:** 1.6+
**HCL Version:** 2.x

For latest information, visit [Terraform Official Documentation](https://www.terraform.io/docs)