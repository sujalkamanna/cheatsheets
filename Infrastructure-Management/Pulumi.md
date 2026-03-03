# Pulumi Cheatsheet

![Pulumi Logo](https://www.pulumi.com/images/logo-dark.png)

---

## Table of Contents
1. [What is Pulumi?](#1-what-is-pulumi)
2. [Installation & Setup](#2-installation--setup)
3. [Project Structure](#3-project-structure)
4. [Basic Concepts](#4-basic-concepts)
5. [Resources](#5-resources)
6. [Stacks](#6-stacks)
7. [Configuration](#7-configuration)
8. [Secrets Management](#8-secrets-management)
9. [Functions & Exports](#9-functions--exports)
10. [Deployment](#10-deployment)
11. [Best Practices](#11-best-practices)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. What is Pulumi?

Pulumi is an Infrastructure as Code (IaC) platform that enables you to write cloud infrastructure in programming languages like Python, Go, TypeScript, and C#. It supports all major cloud providers and automates infrastructure deployment and management.

**Key Features:**
- Infrastructure as Code in general-purpose languages
- Multi-cloud support (AWS, Azure, GCP, Kubernetes)
- Automatic dependency resolution
- State management
- Stack management
- Secrets management
- Policy as Code
- Preview before deployment
- Cross-language support
- Rich SDK ecosystem
- Easy testing

---

## 2. Installation & Setup

### Install Pulumi CLI

```bash
# macOS with Homebrew
brew install pulumi/tap/pulumi

# Ubuntu/Debian
curl -fsSL https://get.pulumi.com | sh
export PATH=$PATH:$HOME/.pulumi/bin

# Windows (PowerShell)
choco install pulumi

# Windows (direct)
curl -fsSL https://get.pulumi.com/pulumi-windows-x64.zip -o pulumi.zip
# Extract and add to PATH

# Verify installation
pulumi version
```

### Install Language SDK

```bash
# Python
pip install pulumi

# TypeScript/JavaScript
npm install -g @pulumi/pulumi
npm install -g pulumi

# Go
go get github.com/pulumi/pulumi/sdk/v3/go/pulumi

# .NET
dotnet global tool install Pulumi
```

### Cloud Provider Setup

```bash
# AWS - Configure credentials
aws configure

# Azure - Login
az login

# GCP - Authenticate
gcloud auth application-default login

# Kubernetes
kubectl config current-context
```

### Project Initialization

```bash
# Create new project
pulumi new aws-python
pulumi new azure-python
pulumi new gcp-python
pulumi new kubernetes-python

# Create from template
pulumi new https://github.com/pulumi/examples/tree/master/aws-py-webserver

# Initialize existing project
pulumi init

# List available templates
pulumi new --list
```

---

## 3. Project Structure

### Basic Project Layout

```
my-pulumi-project/
├── __main__.py                 # Main program
├── Pulumi.yaml                 # Project metadata
├── Pulumi.dev.yaml            # Dev stack config
├── Pulumi.prod.yaml           # Prod stack config
├── venv/                       # Virtual environment
├── requirements.txt            # Python dependencies
├── .gitignore
├── .pulumi/
│   └── .gitignore
└── README.md
```

### Pulumi.yaml Configuration

```yaml
# Pulumi.yaml
name: my-infrastructure
runtime: python
description: Infrastructure as Code with Pulumi

config:
  aws:region:
    description: AWS region
    default: us-east-1
```

### Stack Configuration

```yaml
# Pulumi.dev.yaml
config:
  aws:region: us-east-1
  environment: development
  instance_type: t3.micro
  instance_count: 1

# Pulumi.prod.yaml
config:
  aws:region: us-east-1
  environment: production
  instance_type: t3.large
  instance_count: 3
```

---

## 4. Basic Concepts

### Hello World Program

```python
# __main__.py
import pulumi
import pulumi_aws as aws

# Get config
config = pulumi.Config()
region = config.get('aws:region') or 'us-east-1'

# Create S3 bucket
bucket = aws.s3.Bucket('my-bucket',
    bucket='my-unique-bucket-name',
    acl='private')

# Export output
pulumi.export('bucket_name', bucket.id)
pulumi.export('bucket_arn', bucket.arn)
```

### Core Concepts

```python
# __main__.py
import pulumi
import pulumi_aws as aws

# 1. Resources - Cloud infrastructure
vpc = aws.ec2.Vpc('my-vpc',
    cidr_block='10.0.0.0/16')

subnet = aws.ec2.Subnet('my-subnet',
    vpc_id=vpc.id,
    cidr_block='10.0.1.0/24')

# 2. Automatic Dependencies
security_group = aws.ec2.SecurityGroup('my-sg',
    vpc_id=vpc.id,  # Automatically depends on vpc
    ingress=[
        aws.ec2.SecurityGroupIngressArgs(
            protocol='tcp',
            from_port=80,
            to_port=80,
            cidr_blocks=['0.0.0.0/0'])
    ])

# 3. Exports - Output values
pulumi.export('vpc_id', vpc.id)
pulumi.export('subnet_id', subnet.id)
pulumi.export('security_group_id', security_group.id)

# 4. Locals - Local values
stack_name = pulumi.get_stack()
environment = 'dev' if stack_name == 'dev' else 'prod'

# 5. Stack Reference
other_stack = pulumi.StackReference(f"organization/project/{stack_name}")
vpc_id = other_stack.get_output('vpc_id')
```

---

## 5. Resources

### AWS Resources

```python
# __main__.py
import pulumi
import pulumi_aws as aws
import json

# VPC
vpc = aws.ec2.Vpc('my-vpc',
    cidr_block='10.0.0.0/16',
    enable_dns_hostnames=True,
    enable_dns_support=True)

# Subnet
subnet = aws.ec2.Subnet('my-subnet',
    vpc_id=vpc.id,
    cidr_block='10.0.1.0/24',
    availability_zone='us-east-1a')

# Internet Gateway
igw = aws.ec2.InternetGateway('my-igw',
    vpc_id=vpc.id)

# Route Table
route_table = aws.ec2.RouteTable('my-rt',
    vpc_id=vpc.id,
    routes=[
        aws.ec2.RouteTableRouteArgs(
            cidr_block='0.0.0.0/0',
            gateway_id=igw.id)
    ])

# Route Table Association
rta = aws.ec2.RouteTableAssociation('my-rta',
    subnet_id=subnet.id,
    route_table_id=route_table.id)

# Security Group
security_group = aws.ec2.SecurityGroup('my-sg',
    vpc_id=vpc.id,
    description='Enable HTTP and HTTPS',
    ingress=[
        aws.ec2.SecurityGroupIngressArgs(
            protocol='tcp',
            from_port=80,
            to_port=80,
            cidr_blocks=['0.0.0.0/0']),
        aws.ec2.SecurityGroupIngressArgs(
            protocol='tcp',
            from_port=443,
            to_port=443,
            cidr_blocks=['0.0.0.0/0']),
        aws.ec2.SecurityGroupIngressArgs(
            protocol='tcp',
            from_port=22,
            to_port=22,
            cidr_blocks=['0.0.0.0/0']),
    ],
    egress=[
        aws.ec2.SecurityGroupEgressArgs(
            protocol='-1',
            from_port=0,
            to_port=0,
            cidr_blocks=['0.0.0.0/0']),
    ])

# EC2 Instance
instance = aws.ec2.Instance('my-instance',
    instance_type='t3.micro',
    ami='ami-0c55b159cbfafe1f0',
    subnet_id=subnet.id,
    vpc_security_group_ids=[security_group.id],
    associate_public_ip_address=True,
    user_data='''#!/bin/bash
        apt-get update
        apt-get install -y nginx
        systemctl start nginx''',
    tags={'Name': 'web-server'})

# S3 Bucket
bucket = aws.s3.Bucket('my-bucket',
    bucket='my-unique-bucket-name',
    acl='private',
    versioning=aws.s3.BucketVersioningArgs(
        enabled=True),
    server_side_encryption_configuration=aws.s3.BucketServerSideEncryptionConfigurationArgs(
        rule=aws.s3.BucketServerSideEncryptionConfigurationRuleArgs(
            apply_server_side_encryption_by_default=aws.s3.BucketServerSideEncryptionConfigurationRuleApplyServerSideEncryptionByDefaultArgs(
                sse_algorithm='AES256'))))

# RDS Database
db_subnet_group = aws.rds.SubnetGroup('db-subnet-group',
    subnet_ids=[subnet.id])

database = aws.rds.Instance('my-database',
    identifier='mydb',
    engine='postgres',
    engine_version='12.4',
    instance_class='db.t3.micro',
    allocated_storage=20,
    db_subnet_group_name=db_subnet_group.name,
    vpc_security_group_ids=[security_group.id],
    db_name='mydb',
    username='postgres',
    password='SomePassword123!',
    skip_final_snapshot=True)

# RDS Cluster (Aurora)
db_cluster = aws.rds.Cluster('my-cluster',
    cluster_identifier='aurora-cluster',
    engine='aurora-postgresql',
    engine_version='11.7',
    database_name='mydb',
    master_username='postgres',
    master_password='SomePassword123!',
    db_subnet_group_name=db_subnet_group.name)

# ALB
alb = aws.lb.LoadBalancer('my-alb',
    internal=False,
    load_balancer_type='application',
    security_groups=[security_group.id],
    subnets=[subnet.id])

# ALB Target Group
target_group = aws.lb.TargetGroup('my-tg',
    port=80,
    protocol='HTTP',
    vpc_id=vpc.id,
    target_type='instance')

# ALB Listener
listener = aws.lb.Listener('my-listener',
    load_balancer_arn=alb.arn,
    port=80,
    protocol='HTTP',
    default_actions=[
        aws.lb.ListenerDefaultActionArgs(
            type='forward',
            target_group_arn=target_group.arn)
    ])

# CloudWatch Alarm
alarm = aws.cloudwatch.MetricAlarm('cpu-alarm',
    comparison_operator='GreaterThanThreshold',
    evaluation_periods=2,
    metric_name='CPUUtilization',
    namespace='AWS/EC2',
    period=300,
    statistic='Average',
    threshold=70,
    alarm_actions=[])

# Lambda Function
lambda_role = aws.iam.Role('lambda-role',
    assume_role_policy=json.dumps({
        'Version': '2012-10-17',
        'Statement': [{
            'Action': 'sts:AssumeRole',
            'Effect': 'Allow',
            'Principal': {
                'Service': 'lambda.amazonaws.com'
            }
        }]
    }))

lambda_func = aws.lambda_.Function('my-function',
    code=pulumi.FileArchive('./lambda'),
    role=lambda_role.arn,
    handler='index.handler',
    runtime='python3.9')

# SNS Topic
topic = aws.sns.Topic('my-topic',
    name='my-topic')

# SQS Queue
queue = aws.sqs.Queue('my-queue',
    visibility_timeout_seconds=300)
```

### Azure Resources

```python
# __main__.py
import pulumi
import pulumi_azure as azure

# Resource Group
resource_group = azure.core.ResourceGroup('my-rg',
    name='my-resource-group',
    location='eastus')

# Virtual Network
vnet = azure.network.VirtualNetwork('my-vnet',
    resource_group_name=resource_group.name,
    address_spaces=['10.0.0.0/16'],
    location='eastus')

# Subnet
subnet = azure.network.Subnet('my-subnet',
    resource_group_name=resource_group.name,
    virtual_network_name=vnet.name,
    address_prefix='10.0.1.0/24')

# Storage Account
storage_account = azure.storage.Account('mystorageaccount',
    resource_group_name=resource_group.name,
    account_tier='Standard',
    account_replication_type='LRS')

# App Service Plan
app_service_plan = azure.appservice.Plan('my-plan',
    resource_group_name=resource_group.name,
    kind='Linux',
    reserved=True,
    sku=azure.appservice.PlanSkuArgs(
        tier='Basic',
        size='B1'))

# App Service
app_service = azure.appservice.AppService('my-app',
    resource_group_name=resource_group.name,
    app_service_plan_id=app_service_plan.id,
    app_settings={
        'WEBSITES_ENABLE_APP_SERVICE_STORAGE': 'false'
    })
```

### Kubernetes Resources

```python
# __main__.py
import pulumi
import pulumi_kubernetes as k8s

# Get kubeconfig
k8s_provider = k8s.Provider('k8s-provider')

# Namespace
namespace = k8s.core.v1.Namespace('my-namespace',
    metadata={'name': 'my-namespace'},
    opts=pulumi.ResourceOptions(provider=k8s_provider))

# Deployment
deployment = k8s.apps.v1.Deployment('my-deployment',
    metadata={
        'namespace': namespace.metadata['name'],
        'labels': {'app': 'my-app'}
    },
    spec=k8s.apps.v1.DeploymentSpecArgs(
        replicas=3,
        selector=k8s.meta.v1.LabelSelectorArgs(
            match_labels={'app': 'my-app'}
        ),
        template=k8s.core.v1.PodTemplateSpecArgs(
            metadata={'labels': {'app': 'my-app'}},
            spec=k8s.core.v1.PodSpecArgs(
                containers=[
                    k8s.core.v1.ContainerArgs(
                        name='my-app',
                        image='nginx:1.14-alpine',
                        ports=[k8s.core.v1.ContainerPortArgs(
                            container_port=80
                        )]
                    )
                ]
            )
        )
    ),
    opts=pulumi.ResourceOptions(provider=k8s_provider))

# Service
service = k8s.core.v1.Service('my-service',
    metadata={
        'namespace': namespace.metadata['name'],
    },
    spec=k8s.core.v1.ServiceSpecArgs(
        type='LoadBalancer',
        selector={'app': 'my-app'},
        ports=[k8s.core.v1.ServicePortArgs(
            port=80,
            target_port=80
        )]
    ),
    opts=pulumi.ResourceOptions(provider=k8s_provider))
```

---

## 6. Stacks

### Stack Management

```bash
# Create new stack
pulumi stack init dev
pulumi stack init prod

# List stacks
pulumi stack ls

# Select stack
pulumi stack select dev

# Rename stack
pulumi stack rename dev dev-backup

# Remove stack
pulumi stack rm dev

# Get stack info
pulumi stack
pulumi stack output
```

### Stack Configuration

```python
# __main__.py
import pulumi

# Get current stack name
stack_name = pulumi.get_stack()

# Get config
config = pulumi.Config()

# Read typed config
instance_type = config.get('instance_type') or 't3.micro'
instance_count = config.get_int('instance_count') or 1
enable_https = config.get_bool('enable_https') or False

# Environment-specific logic
if stack_name == 'prod':
    instance_type = 't3.large'
    instance_count = 3
    enable_https = True

# Create resources based on config
for i in range(instance_count):
    instance = aws.ec2.Instance(f'instance-{i}',
        instance_type=instance_type)
```

### Stack References

```python
# __main__.py
import pulumi

# Reference another stack in same project
dev_stack = pulumi.StackReference(f'organization/project/dev')
prod_vpc_id = dev_stack.get_output('vpc_id')

# Reference stack in different project
other_project = pulumi.StackReference('organization/other-project/dev')
other_output = other_project.get_output('some_value')

# Use in resources
instance = aws.ec2.Instance('my-instance',
    subnet_id=prod_vpc_id)
```

---

## 7. Configuration

### Pulumi Config

```bash
# Set config values
pulumi config set instance_type t3.large
pulumi config set instance_count 3
pulumi config set --path 'database.password' 'secretpassword'
pulumi config set enable_https true --path
pulumi config set port 8080 --path

# Get config values
pulumi config get instance_type
pulumi config get --path 'database.password'

# List all config
pulumi config

# Delete config
pulumi config rm instance_type
```

### Config in Code

```python
# __main__.py
import pulumi

config = pulumi.Config()

# Simple get
region = config.get('aws:region')

# With defaults
instance_type = config.get('instance_type') or 't3.micro'

# Get typed values
instance_count = config.get_int('instance_count') or 1
enable_debug = config.get_bool('enable_debug') or False

# Get nested config
db_config = config.get_object('database') or {}
db_name = db_config.get('name', 'mydb')
db_engine = db_config.get('engine', 'postgres')

# Require config (error if not set)
api_key = config.require('api_key')

# Use in infrastructure
instance = aws.ec2.Instance('my-instance',
    instance_type=instance_type)
```

### Pulumi.yaml Values

```yaml
# Pulumi.yaml
name: my-project
runtime: python
description: My infrastructure

config:
  aws:region:
    description: AWS region
    default: us-east-1
  instance_type:
    description: EC2 instance type
    default: t3.micro
  database:
    description: Database configuration
```

---

## 8. Secrets Management

### Handle Secrets

```python
# __main__.py
import pulumi

config = pulumi.Config()

# Get secret (never logged or printed)
db_password = config.require_secret('db_password')

# Use secret in resources
database = aws.rds.Instance('my-database',
    engine='postgres',
    master_password=db_password)

# Apply secret to output
pulumi.export('db_connection_string',
    pulumi.Output.concat('postgres://user:',
        db_password,
        '@localhost:5432/mydb'))
```

### Set Secrets

```bash
# Set secret value (encrypted at rest)
pulumi config set --secret db_password 'MySecretPassword123!'

# Set secret from file
pulumi config set --secret db_password < password.txt

# Set nested secret
pulumi config set --secret --path 'database.password' 'secret'

# View encrypted config
cat Pulumi.dev.yaml

# Decrypt for viewing
pulumi config get --secret db_password
```

### AWS Secrets Manager Integration

```python
# __main__.py
import pulumi
import pulumi_aws as aws

# Get secret from AWS Secrets Manager
secret = aws.secretsmanager.get_secret(name='my-secret')
secret_value = aws.secretsmanager.get_secret_version(
    secret_id=secret.id,
    version_id=secret.version_ids[0])

# Use in resources
database = aws.rds.Instance('my-database',
    master_password=secret_value.secret_string)

# Create secret
new_secret = aws.secretsmanager.Secret('my-secret',
    name='my-new-secret')

aws.secretsmanager.SecretVersion('my-secret-version',
    secret_id=new_secret.id,
    secret_string='mysecretvalue')
```

---

## 9. Functions & Exports

### Export Outputs

```python
# __main__.py
import pulumi
import pulumi_aws as aws

# Create resources
bucket = aws.s3.Bucket('my-bucket',
    bucket='my-unique-bucket-name')

instance = aws.ec2.Instance('my-instance',
    instance_type='t3.micro',
    ami='ami-0c55b159cbfafe1f0')

vpc = aws.ec2.Vpc('my-vpc',
    cidr_block='10.0.0.0/16')

# Export outputs (visible after deployment)
pulumi.export('bucket_name', bucket.id)
pulumi.export('bucket_arn', bucket.arn)
pulumi.export('instance_id', instance.id)
pulumi.export('instance_public_ip', instance.public_ip)
pulumi.export('vpc_id', vpc.id)

# Export multiple values as object
pulumi.export('infrastructure', {
    'bucket': bucket.id,
    'instance': instance.id,
    'vpc': vpc.id
})

# Conditional exports
stack_name = pulumi.get_stack()
if stack_name == 'prod':
    pulumi.export('production_endpoint', instance.public_ip)
```

### Using Outputs

```python
# __main__.py
import pulumi
import pulumi_aws as aws

# Outputs are async - can't use directly
bucket = aws.s3.Bucket('my-bucket',
    bucket='my-bucket-name')

# Wrong - can't concatenate with Output directly
# url = f"s3://{bucket.id}"

# Right - use apply() or Output.concat()
url = bucket.id.apply(lambda name: f"s3://{name}")
pulumi.export('bucket_url', url)

# Using Output.concat()
website_url = pulumi.Output.concat('https://', bucket.id, '.s3.amazonaws.com')
pulumi.export('website_url', website_url)

# Multiple outputs - combine them
instance = aws.ec2.Instance('my-instance',
    instance_type='t3.micro',
    ami='ami-0c55b159cbfafe1f0')

connection_info = pulumi.Output.all(
    instance.id,
    instance.public_ip
).apply(lambda args: {
    'instance_id': args[0],
    'public_ip': args[1]
})

pulumi.export('connection_info', connection_info)
```

---

## 10. Deployment

### Deploy Infrastructure

```bash
# Preview changes (dry-run)
pulumi preview
pulumi preview --stack dev
pulumi preview --diff

# Deploy infrastructure
pulumi up
pulumi up --stack dev
pulumi up --yes  # Auto-approve

# Deploy specific resources
pulumi up --target 'aws:ec2/instance:Instance::my-instance'
pulumi up --target 'aws:s3/bucket:Bucket::my-bucket'

# Target dependents
pulumi up --target-dependents

# Refresh state
pulumi refresh

# View stack outputs
pulumi stack output
pulumi stack output bucket_name
pulumi stack output infrastructure

# Destroy infrastructure
pulumi destroy
pulumi destroy --stack dev
pulumi destroy --yes  # Auto-approve

# Cancel deployment
pulumi cancel
```

### Deployment Options

```bash
# Parallel operations
pulumi up --parallel 10

# Skip dependency check
pulumi up --skip-dependency-sort

# Detailed logging
pulumi up --verbose
pulumi up -v

# Replace specific resource
pulumi up --replace 'aws:ec2/instance:Instance::my-instance'

# Export deployment plan
pulumi up --plan plan.json

# Import deployment plan
pulumi up --plan plan.json

# Show detailed updates
pulumi up --show-secrets
pulumi up --show-replacement
```

### Viewing History

```bash
# View stack history
pulumi history

# Show specific update
pulumi history --show <update-id>

# List stack resources
pulumi stack --show-urns

# Export stack to JSON
pulumi stack export > stack.json

# Import stack from JSON
pulumi stack import < stack.json
```

---

## 11. Best Practices

### Component Classes

```python
# components.py
import pulumi
import pulumi_aws as aws
from typing import Optional

class WebServerComponent(pulumi.ComponentResource):
    '''Custom component for web server'''
    
    public_ip: pulumi.Output[str]
    private_ip: pulumi.Output[str]
    
    def __init__(self,
                 name: str,
                 instance_type: str = 't3.micro',
                 ami: str = 'ami-0c55b159cbfafe1f0',
                 vpc_id: Optional[str] = None,
                 opts: pulumi.ResourceOptions = None):
        
        super().__init__('custom:WebServer', name, None, opts)
        
        # Create security group
        sg = aws.ec2.SecurityGroup(f'{name}-sg',
            vpc_id=vpc_id,
            ingress=[
                aws.ec2.SecurityGroupIngressArgs(
                    protocol='tcp',
                    from_port=80,
                    to_port=80,
                    cidr_blocks=['0.0.0.0/0']
                )
            ],
            opts=pulumi.ResourceOptions(parent=self))
        
        # Create instance
        instance = aws.ec2.Instance(f'{name}-instance',
            instance_type=instance_type,
            ami=ami,
            vpc_security_group_ids=[sg.id],
            opts=pulumi.ResourceOptions(parent=self))
        
        self.public_ip = instance.public_ip
        self.private_ip = instance.private_ip
        
        self.register_outputs({
            'publicIp': self.public_ip,
            'privateIp': self.private_ip
        })

# __main__.py
import pulumi
from components import WebServerComponent

webserver = WebServerComponent('my-webserver',
    instance_type='t3.small')

pulumi.export('web_server_ip', webserver.public_ip)
```

### Modular Organization

```python
# vpc.py
import pulumi
import pulumi_aws as aws

def create_vpc(name, cidr_block):
    vpc = aws.ec2.Vpc(f'{name}-vpc',
        cidr_block=cidr_block)
    
    subnet = aws.ec2.Subnet(f'{name}-subnet',
        vpc_id=vpc.id,
        cidr_block='10.0.1.0/24')
    
    return {
        'vpc': vpc,
        'subnet': subnet
    }

# compute.py
import pulumi
import pulumi_aws as aws

def create_instance(name, instance_type, subnet_id):
    instance = aws.ec2.Instance(f'{name}-instance',
        instance_type=instance_type,
        ami='ami-0c55b159cbfafe1f0',
        subnet_id=subnet_id)
    
    return instance

# __main__.py
import pulumi
from vpc import create_vpc
from compute import create_instance

vpc_resources = create_vpc('my', '10.0.0.0/16')
instance = create_instance('web', 't3.micro',
    vpc_resources['subnet'].id)

pulumi.export('vpc_id', vpc_resources['vpc'].id)
pulumi.export('instance_id', instance.id)
```

### Environment-Specific Configuration

```python
# __main__.py
import pulumi

config = pulumi.Config()
stack = pulumi.get_stack()

# Stack-specific settings
settings = {
    'dev': {
        'instance_type': 't3.micro',
        'instance_count': 1,
        'enable_backup': False
    },
    'test': {
        'instance_type': 't3.small',
        'instance_count': 2,
        'enable_backup': True
    },
    'prod': {
        'instance_type': 't3.large',
        'instance_count': 3,
        'enable_backup': True
    }
}

stack_config = settings.get(stack, settings['dev'])

# Use config values
instance_type = stack_config['instance_type']
instance_count = stack_config['instance_count']
enable_backup = stack_config['enable_backup']

# Create infrastructure
for i in range(instance_count):
    instance = aws.ec2.Instance(f'instance-{i}',
        instance_type=instance_type)
```

---

## 12. Troubleshooting

### Debug & Logs

```bash
# Enable debug logging
PULUMI_DEBUG_COMMANDS=true pulumi up

# Verbose output
pulumi up --verbose
pulumi up -v

# Show secrets
pulumi up --show-secrets

# Detailed stack trace
pulumi up --show-replacement --show-sames

# Log level
PULUMI_LOG_LEVEL=debug pulumi up

# Save log to file
PULUMI_LOG=pulumi.log pulumi up
```

### Common Issues

```python
# Issue: Output not directly usable
# Wrong:
bucket_url = f"https://{bucket.id}.s3.amazonaws.com"

# Right:
bucket_url = bucket.id.apply(
    lambda id: f"https://{id}.s3.amazonaws.com")

# Issue: Circular dependency
# Wrong - circular reference between resources
resource_a = depends_on(resource_b)
resource_b = depends_on(resource_a)

# Right - resolve circular dependency
resource_a = create_resource_a()
resource_b = create_resource_b(
    depends_on=[resource_a])

# Issue: Secret not encrypted
# Wrong:
config.set('password', 'secret')

# Right:
pulumi config set --secret password 'secret'

# Issue: Invalid resource property
# Check documentation
pulumi_aws.ec2.Instance documentation
```

### Validation Commands

```bash
# Check syntax
python __main__.py

# Validate program
pulumi preview

# Refresh state
pulumi refresh

# Check for updates
pulumi stack check

# Verify credentials
pulumi whoami

# Show version
pulumi version

# Get logs
pulumi logs --follow
pulumi logs --resource my-resource

# Debug specific update
pulumi history --show <update-id>
```

### State Management

```bash
# Export state
pulumi stack export > backup.json

# Import state
pulumi stack import < backup.json

# Clear exports
pulumi stack output rm key_name

# Refresh state from cloud
pulumi refresh --force

# View state file location
cat ~/.pulumi/stacks/<organization>/<project>/<stack>.json
```

---

## Quick Reference Commands

```bash
# Project Management
pulumi new aws-python                             # Create new project
pulumi init                                       # Initialize project
pulumi plugin install resource aws 5.0.0         # Install provider plugin

# Stack Operations
pulumi stack init dev                             # Create stack
pulumi stack select dev                           # Select stack
pulumi stack ls                                   # List stacks
pulumi stack rm dev                               # Delete stack

# Deployment
pulumi preview                                    # Preview changes
pulumi up                                         # Deploy infrastructure
pulumi up --yes                                   # Deploy without confirmation
pulumi destroy                                    # Destroy infrastructure
pulumi refresh                                    # Refresh state

# Configuration
pulumi config set key value                       # Set config
pulumi config get key                             # Get config
pulumi config set --secret password 'secret'     # Set secret
pulumi config rm key                              # Delete config

# Outputs
pulumi stack output                               # List outputs
pulumi stack output key_name                      # Get specific output
pulumi export > stack.json                        # Export state

# Debugging
pulumi up --verbose                               # Verbose output
pulumi logs --follow                              # View logs
pulumi history                                    # View history
```

---

## Essential Pulumi Concepts

### Resources
Cloud infrastructure components (instances, buckets, etc.)

### Outputs
Values exported from infrastructure (IPs, ARNs, etc.)

### Stacks
Isolated deployment environments (dev, test, prod)

### Configuration
Stack-specific settings and secrets

### Component
Reusable custom resource combining multiple resources

### State
Current infrastructure configuration stored remotely

### Provider
Cloud provider integration (AWS, Azure, GCP)

---

## Resources

| Resource | URL |
|----------|-----|
| **Official Documentation** | https://www.pulumi.com/docs/ |
| **Python SDK** | https://www.pulumi.com/docs/reference/pkg/python/ |
| **AWS Provider** | https://www.pulumi.com/docs/reference/pkg/aws/ |
| **Azure Provider** | https://www.pulumi.com/docs/reference/pkg/azure/ |
| **GCP Provider** | https://www.pulumi.com/docs/reference/pkg/gcp/ |
| **Examples** | https://github.com/pulumi/examples |
| **Community** | https://pulumi-community.slack.com |

---

**Last Updated:** 2024
**Pulumi Version:** 3.x+
**Python Version:** 3.6+

For latest information, visit [Pulumi Official Documentation](https://www.pulumi.com/docs/)