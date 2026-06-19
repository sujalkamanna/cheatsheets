# Microsoft Azure Cheatsheet

![Microsoft Azure Logo](https://azure.microsoft.com/svghandler/azure-logo/)

---

## Table of Contents

- [1. What is Microsoft Azure?](#1-what-is-microsoft-azure)
- [2. Why Azure?](#2-why-azure)
- [3. Azure Global Infrastructure](#3-azure-global-infrastructure)
- [4. Azure Core Services Overview](#4-azure-core-services-overview)
- [5. Azure Regions](#5-azure-regions)
- [6. Availability Zones](#6-availability-zones)
- [7. Region Pairs](#7-region-pairs)
- [8. Azure Account Structure](#8-azure-account-structure)
- [9. Azure Resource Manager (ARM)](#9-azure-resource-manager-arm)
- [10. Identity and Access Management](#10-identity-and-access-management)
- [11. Azure Virtual Machines](#11-azure-virtual-machines)
- [12. Azure Storage](#12-azure-storage)
- [13. Azure Networking](#13-azure-networking)
- [14. Azure Database Services](#14-azure-database-services)
- [15. Azure Monitoring Services](#15-azure-monitoring-services)
- [16. Virtual Network (VNet)](#16-virtual-network-vnet)
- [17. Subnets](#17-subnets)
- [18. Route Tables](#18-route-tables)
- [19. Network Security Groups (NSG)](#19-network-security-groups-nsg)
- [20. Azure Firewall](#20-azure-firewall)
- [21. Azure Load Balancer](#21-azure-load-balancer)
- [22. Azure Application Gateway](#22-azure-application-gateway)
- [23. Azure Front Door](#23-azure-front-door)
- [24. Azure DNS](#24-azure-dns)
- [25. Azure Bastion](#25-azure-bastion)
- [26. Azure VPN Gateway](#26-azure-vpn-gateway)
- [27. Azure ExpressRoute](#27-azure-expressroute)
- [28. Azure Virtual WAN](#28-azure-virtual-wan)
- [29. Azure CDN](#29-azure-cdn)
- [30. Azure Traffic Manager](#30-azure-traffic-manager)
- [31. Azure Active Directory (Microsoft Entra ID)](#31-azure-active-directory-microsoft-entra-id)
- [32. Role-Based Access Control (RBAC)](#32-role-based-access-control-rbac)
- [33. Managed Identities](#33-managed-identities)
- [34. Azure Key Vault](#34-azure-key-vault)
- [35. Azure Monitor](#35-azure-monitor)
- [36. Log Analytics Workspace](#36-log-analytics-workspace)
- [37. Application Insights](#37-application-insights)
- [38. Azure Automation](#38-azure-automation)
- [39. Azure Policy](#39-azure-policy)
- [40. Azure Blueprints](#40-azure-blueprints)
- [41. Azure Backup](#41-azure-backup)
- [42. Azure Site Recovery](#42-azure-site-recovery)
- [43. Azure Security Center (Defender for Cloud)](#43-azure-security-center-defender-for-cloud)
- [44. Azure Advisor](#44-azure-advisor)
- [45. Azure Cost Management](#45-azure-cost-management)
- [46. Azure App Service](#46-azure-app-service)
- [47. Azure Functions](#47-azure-functions)
- [48. Azure Container Instances (ACI)](#48-azure-container-instances-aci)
- [49. Azure Container Registry (ACR)](#49-azure-container-registry-acr)
- [50. Azure Kubernetes Service (AKS)](#50-azure-kubernetes-service-aks)
- [51. Azure DevOps](#51-azure-devops)
- [52. GitHub Actions on Azure](#52-github-actions-on-azure)
- [53. ARM Templates](#53-arm-templates)
- [54. Bicep](#54-bicep)
- [55. Terraform on Azure](#55-terraform-on-azure)
- [56. Security Best Practices](#56-security-best-practices)
- [57. Cost Optimization](#57-cost-optimization)
- [58. High Availability](#58-high-availability)
- [59. Disaster Recovery](#59-disaster-recovery)
- [60. Interview Questions](#60-interview-questions)

- [Additional Resources](#additional-resources)
- [Quick Reference](#quick-reference)
- [📎 Official Documentation](#-official-documentation)
- [📄 Copyright](#-copyright)
- [📎 Disclaimer & Attribution](#-disclaimer--attribution)
- [🎉 Conclusion](#-conclusion)

---

# 1. What is Microsoft Azure?

Microsoft Azure is Microsoft's cloud computing platform that provides services for:

* Compute
* Storage
* Networking
* Databases
* Containers
* Artificial Intelligence
* Security
* Analytics
* DevOps
* Monitoring

Azure enables organizations to build, deploy, and manage applications through Microsoft's global cloud infrastructure.

---

## Why Azure?

Traditional Infrastructure:

```text
Application
     │
     ▼
Physical Servers
     │
     ▼
Data Center
```

Cloud Infrastructure:

```text
Application
     │
     ▼
Azure Services
     │
     ▼
Global Cloud Infrastructure
```

---

## Key Benefits

```text
Pay-As-You-Go
Global Reach
Managed Services
High Availability
Elastic Scaling
Enterprise Integration
Hybrid Cloud Support
```

---

## Common Use Cases

### Application Hosting

```text
Users
  │
  ▼
Azure Load Balancer
  │
  ▼
Virtual Machines
```

---

### Kubernetes Platforms

```text
Users
  │
  ▼
AKS
  │
  ▼
Containers
```

---

### Serverless Applications

```text
Users
  │
  ▼
API Management
  │
  ▼
Azure Functions
  │
  ▼
Cosmos DB
```

---

# 2. Core Concepts

---

## Cloud Computing

Cloud computing provides IT resources on demand.

Examples:

```text
Virtual Machines
Storage
Databases
Networking
Containers
```

---

## Azure Regions

Regions are physical geographic locations.

Examples:

```text
East US
West Europe
Central India
Southeast Asia
UK South
```

---

## Availability Zones

Availability Zones are isolated datacenters within a region.

Example:

```text
Central India
    │
 ┌──┼──┐
 ▼  ▼  ▼
AZ1 AZ2 AZ3
```

Benefits:

```text
Fault Tolerance
High Availability
Business Continuity
```

---

## Azure Resource Groups

Resource Groups logically organize resources.

```text
Resource Group
      │
      ├── VM
      ├── Storage
      ├── Database
      └── Network
```

---

## Elasticity

Resources automatically expand or shrink.

```text
Traffic ↑
   │
   ▼
More Resources

Traffic ↓
   │
   ▼
Fewer Resources
```

---

## Scalability

Ability to handle increasing workloads.

### Vertical Scaling

```text
2 CPU
 ▼
8 CPU
```

---

### Horizontal Scaling

```text
VM1
VM2
VM3
VM4
```

---

# 3. Azure Global Infrastructure

Azure operates one of the world's largest cloud infrastructures.

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
Edge Network
   │
   ▼
Azure Region
   │
   ▼
Availability Zones
```

---

## Region Pairs

Azure pairs regions for disaster recovery.

Example:

```text
North Europe
      │
      ▼
West Europe
```

---

## Why Region Pairs?

Benefits:

```text
Disaster Recovery
Replication
Business Continuity
```

---

## Multi-Region Design

```text
Primary Region
      │
      ▼
Backup Region
```

---

# 4. Shared Responsibility Model

One of Azure's most important security concepts.

---

## Microsoft Responsibility

Microsoft manages:

```text
Physical Datacenters
Hardware
Networking
Storage Infrastructure
Cloud Services
```

---

## Customer Responsibility

Customer manages:

```text
Data
Applications
Identity
Access Controls
Operating Systems
```

---

## Model

```text
Microsoft
    │
    ▼
Security OF the Cloud

Customer
    │
    ▼
Security IN the Cloud
```

---

## Example

Virtual Machine:

Microsoft Secures:

```text
Physical Server
Storage Hardware
Networking Hardware
```

Customer Secures:

```text
Windows/Linux OS
Applications
Users
Data
```

---

# 5. Microsoft Entra ID (Azure AD)

Microsoft Entra ID (formerly Azure Active Directory) manages identities.

---

## Core Components

```text
Users
Groups
Applications
Service Principals
Managed Identities
```

---

## Users

Represents an individual identity.

Example:

```text
john@company.com
```

---

## Groups

Collection of users.

Examples:

```text
Developers
Admins
DevOps
Finance
```

---

## Applications

Used for authentication and authorization.

---

## Managed Identities

Provide secure access to Azure resources.

Preferred over:

```text
Passwords
Secrets
Hardcoded Credentials
```

---

## Benefits

```text
Single Sign-On
Identity Management
Multi-Factor Authentication
Conditional Access
```

---

# 6. Role-Based Access Control (RBAC)

RBAC controls permissions in Azure.

---

## RBAC Components

```text
User
Group
Service Principal
Managed Identity
```

assigned to:

```text
Role
```

on:

```text
Resource
Resource Group
Subscription
Management Group
```

---

## Common Roles

### Owner

```text
Full Access
```

---

### Contributor

```text
Manage Resources
No Access Management
```

---

### Reader

```text
Read Only
```

---

## RBAC Architecture

```text
User
  │
  ▼
 Role
  │
  ▼
 Azure Resource
```

---

## Best Practices

```text
Least Privilege
Use Groups
Avoid Excessive Owner Roles
Use Managed Identities
```

---

# 7. Azure CLI

Azure CLI allows managing Azure from the terminal.

---

## Install Azure CLI

Linux:

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

---

Verify Installation:

```bash
az version
```

---

## Login

```bash
az login
```

---

## Show Current Account

```bash
az account show
```

---

## List Subscriptions

```bash
az account list
```

---

## List Resource Groups

```bash
az group list
```

---

## List Virtual Machines

```bash
az vm list
```

---

## Common Azure CLI Commands

```text
az vm
az aks
az storage
az network
az group
az monitor
```

---

# 8. Security Fundamentals

Security is a shared responsibility.

---

## Security Layers

```text
Entra ID
RBAC
Network Security Groups
Azure Firewall
Encryption
Defender for Cloud
```

---

## Multi-Factor Authentication

Protect:

```text
Users
Administrators
Privileged Accounts
```

---

## Encryption

### At Rest

Examples:

```text
Blob Storage
Managed Disks
Azure SQL
```

---

### In Transit

```text
HTTPS
TLS
SSL
```

---

## Network Security

Use:

```text
NSGs
Azure Firewall
Private Endpoints
```

---

## Defender for Cloud

Provides:

```text
Security Recommendations
Threat Detection
Compliance Monitoring
```

---

# 9. Azure Accounts, Subscriptions & Billing

---

## Azure Account

Identity used to access Azure.

Example:

```text
user@company.com
```

---

## Subscription

Billing and resource boundary.

```text
Azure Account
      │
      ▼
Subscription
      │
      ▼
Resources
```

---

## Resource Group

Logical container.

```text
Resource Group
      │
      ├── VM
      ├── Storage
      ├── Network
      └── Database
```

---

## Billing Features

Provides:

```text
Cost Analysis
Budgets
Forecasting
Recommendations
```

---

## Cost Optimization Basics

Review:

```text
Unused VMs
Unused Disks
Unused Public IPs
Idle Resources
```

---

# 10. High Availability Concepts

High Availability keeps applications running during failures.

---

## Single Zone Design

```text
Users
  │
  ▼
VM
  │
  ▼
AZ1
```

Risk:

```text
Zone Failure
=
Downtime
```

---

## Multi-Zone Design

```text
Users
   │
   ▼
Load Balancer
   │
   ├── VM (AZ1)
   └── VM (AZ2)
```

---

## Benefits

```text
Fault Tolerance
Resilience
Automatic Recovery
```

---

## Azure Services Supporting HA

```text
Availability Zones
Load Balancer
Application Gateway
VM Scale Sets
AKS
Azure SQL
```

---

# 11. Scalability Concepts

Azure enables automatic scaling.

---

## Vertical Scaling

Increase resources.

```text
2 CPU
 ▼
8 CPU
```

---

## Horizontal Scaling

Add more instances.

```text
VM1
VM2
VM3
VM4
```

---

## VM Scale Sets

Automatically scale VMs.

```text
Traffic ↑
     │
     ▼
More VMs

Traffic ↓
     │
     ▼
Fewer VMs
```

---

## Scalability Architecture

```text
Users
  │
  ▼
Load Balancer
  │
  ▼
VM Scale Set
  │
  ├── VM
  ├── VM
  └── VM
```

---

## Benefits

```text
High Availability
Elastic Scaling
Cost Optimization
Performance
```

---

## Most Important Concepts

```text
Region
Availability Zone
Resource Group
Subscription
Entra ID
RBAC
High Availability
Scalability
```

---

## Most Important Commands

```bash
az login

az account show

az account list

az group list

az vm list
```

---

# 12. Compute Services

Compute services provide processing power for applications.

Azure offers:

```text
Virtual Machines
Virtual Machine Scale Sets
Azure Functions
Container Instances
AKS
App Services
```

---

## Compute Selection

```text
Need Virtual Machines?
        │
        ▼
       VM

Need Containers?
        │
        ▼
     AKS / ACI

Need Serverless?
        │
        ▼
 Azure Functions
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
  ├── VM
  ├── AKS
  ├── ACI
  └── Functions
```

---

# 13. Azure Virtual Machines (VMs)

Azure Virtual Machines provide Infrastructure as a Service (IaaS).

---

## Common Use Cases

```text
Web Servers
Application Servers
Jenkins
GitLab
Monitoring Tools
Databases
```

---

## VM Architecture

```text
Virtual Machine
      │
      ├── CPU
      ├── Memory
      ├── Storage
      └── Network
```

---

## VM Families

### General Purpose

```text
B-Series
D-Series
Dv5
```

---

### Compute Optimized

```text
F-Series
```

---

### Memory Optimized

```text
E-Series
M-Series
```

---

### Storage Optimized

```text
L-Series
```

---

## Create VM

```bash
az vm create \
--resource-group my-rg \
--name myvm
```

---

## List VMs

```bash
az vm list
```

---

## Start VM

```bash
az vm start \
--name myvm \
--resource-group my-rg
```

---

## Stop VM

```bash
az vm stop \
--name myvm \
--resource-group my-rg
```

---

## Delete VM

```bash
az vm delete \
--name myvm \
--resource-group my-rg
```

---

## VM Best Practices

```text
Use Managed Disks
Use Managed Identities
Enable Monitoring
Use NSGs
Use Availability Zones
```

---

# 14. Virtual Machine Scale Sets (VMSS)

VM Scale Sets automatically manage groups of VMs.

---

## Purpose

Automatically scale applications.

```text
Traffic ↑
     │
     ▼
More VMs

Traffic ↓
     │
     ▼
Fewer VMs
```

---

## Architecture

```text
Users
   │
   ▼
Load Balancer
   │
   ▼
VM Scale Set
   │
   ├── VM1
   ├── VM2
   └── VM3
```

---

## Benefits

```text
High Availability
Auto Scaling
Fault Tolerance
```

---

## Common Use Cases

```text
Web Applications
Microservices
DevOps Platforms
```

---

# 15. Load Balancing Services

Azure provides multiple load balancing solutions.

---

## Azure Load Balancer

Layer 4 Load Balancer.

Supports:

```text
TCP
UDP
```

---

## Application Gateway

Layer 7 Load Balancer.

Supports:

```text
HTTP
HTTPS
SSL Termination
Web Application Firewall
```

---

## Traffic Manager

DNS-based load balancing.

---

## Front Door

Global application delivery service.

---

## Architecture

```text
Users
   │
   ▼
Application Gateway
   │
   ▼
Backend Services
```

---

## Benefits

```text
High Availability
Traffic Distribution
Security
```

---

# 16. Storage Services

Azure provides multiple storage options.

---

## Storage Types

```text
Object Storage
Block Storage
File Storage
Archive Storage
```

---

## Service Mapping

```text
Blob Storage
 └── Object Storage

Managed Disks
 └── Block Storage

Azure Files
 └── File Storage

Archive Tier
 └── Long-Term Storage
```

---

# 17. Azure Blob Storage

Blob Storage is Azure's object storage service.

---

## Features

```text
Highly Available
Scalable
Durable
Secure
```

---

## Use Cases

```text
Backups
Logs
Static Websites
Media Files
Data Lakes
```

---

## Architecture

```text
Applications
      │
      ▼
Blob Container
      │
      ├── File1
      ├── File2
      └── File3
```

---

## Storage Tiers

### Hot Tier

```text
Frequently Accessed
```

---

### Cool Tier

```text
Infrequently Accessed
```

---

### Archive Tier

```text
Long-Term Storage
```

---

## Common Commands

```bash
az storage account list
```

---

## Best Practices

```text
Enable Versioning
Enable Encryption
Use Lifecycle Policies
```

---

# 18. Azure Managed Disks

Managed Disks provide persistent block storage.

---

## Architecture

```text
Virtual Machine
       │
       ▼
 Managed Disk
```

---

## Types

### Standard HDD

```text
Low Cost
```

---

### Standard SSD

```text
Balanced Performance
```

---

### Premium SSD

```text
High Performance
```

---

### Ultra Disk

```text
Highest Performance
```

---

## Use Cases

```text
Operating Systems
Databases
Applications
```

---

## Best Practices

```text
Enable Encryption
Take Snapshots
Monitor Performance
```

---

# 19. Azure Files

Azure Files provides managed file shares.

---

## Architecture

```text
VM1
 │
 ├── Azure Files
 │
VM2
```

---

## Features

```text
SMB Support
NFS Support
Shared Storage
Managed Service
```

---

## Use Cases

```text
Shared Storage
Containers
Applications
```

---

## Benefits

```text
Fully Managed
Highly Available
Scalable
```

---

# 20. Database Services

Azure provides managed database services.

---

## Categories

```text
Relational
NoSQL
In-Memory
Analytics
```

---

## Services

```text
Azure SQL Database
Cosmos DB
PostgreSQL
MySQL
Redis Cache
```

---

# 21. Azure SQL Database

Managed relational database service.

---

## Features

```text
Managed Backups
Automatic Patching
Scaling
Monitoring
```

---

## Architecture

```text
Application
      │
      ▼
Azure SQL
      │
      ▼
Database
```

---

## Benefits

```text
Fully Managed
High Availability
Built-in Security
```

---

## Use Cases

```text
Enterprise Applications
Web Applications
Business Systems
```

---

# 22. Azure Cosmos DB

Globally distributed NoSQL database.

---

## Characteristics

```text
Serverless
Globally Distributed
Low Latency
Highly Scalable
```

---

## Supported APIs

```text
SQL API
MongoDB API
Cassandra API
Gremlin API
Table API
```

---

## Architecture

```text
Application
      │
      ▼
 Cosmos DB
```

---

## Use Cases

```text
Gaming
IoT
Microservices
Global Applications
```

---

# 23. Serverless Computing

Serverless removes infrastructure management.

---

## Azure Serverless Services

```text
Azure Functions
Logic Apps
Event Grid
Service Bus
```

---

## Benefits

```text
No Servers
Auto Scaling
Pay Per Execution
```

---

# 24. Azure Functions

Run code without managing servers.

---

## Workflow

```text
Trigger
   │
   ▼
Function
   │
   ▼
Response
```

---

## Triggers

```text
HTTP
Blob Storage
Timer
Queue
Event Grid
```

---

## Use Cases

```text
Automation
Image Processing
APIs
Scheduled Tasks
```

---

## Benefits

```text
Serverless
Cost Effective
Automatic Scaling
```

---

# 25. Container Services

Azure supports containerized workloads.

---

## Services

```text
AKS
ACI
Container Apps
```

---

## Selection

```text
Need Kubernetes?
      │
      ▼
     AKS

Need Simple Containers?
      │
      ▼
     ACI
```

---

# 26. Azure Container Instances (ACI)

Run containers without managing servers.

---

## Architecture

```text
Container
    │
    ▼
Azure Container Instance
```

---

## Benefits

```text
Fast Deployment
No Infrastructure Management
Simple Container Hosting
```

---

## Use Cases

```text
Testing
Batch Jobs
Short-Lived Workloads
```

---

# 27. Azure Kubernetes Service (AKS)

Managed Kubernetes platform.

---

## Architecture

```text
Users
  │
  ▼
AKS Control Plane
  │
  ▼
Node Pools
  │
  ▼
Pods
```

---

## Components

```text
Control Plane
Node Pools
Pods
Services
Ingress
```

---

## Benefits

```text
Managed Kubernetes
Auto Scaling
Integrated Security
High Availability
```

---

## Common Use Cases

```text
Microservices
DevOps Platforms
Cloud-Native Applications
```

---

## AKS Workflow

```text
Developer
     │
     ▼
kubectl
     │
     ▼
AKS Cluster
     │
     ▼
Pods
```

---

## Most Important Services

```text
VM
Blob Storage
Azure SQL
Cosmos DB
Functions
AKS
Entra ID
```

---

## Most Important Commands

```bash
az vm list

az vm start

az vm stop

az storage account list
```

---

# 28. Networking Fundamentals

Networking forms the backbone of Azure infrastructure.

---

## Network Architecture

```text
Internet
    │
    ▼
Azure DNS
    │
    ▼
Azure Front Door
    │
    ▼
Load Balancer
    │
    ▼
Virtual Network (VNet)
    │
    ▼
Applications
```

---

## Core Networking Components

```text
Virtual Network (VNet)
Subnets
Route Tables
NAT Gateway
Network Security Groups
Azure Firewall
DNS
CDN
```

---

## Typical Traffic Flow

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

# 29. Virtual Network (VNet)

A Virtual Network is Azure's private network.

---

## Purpose

Provides:

```text
Network Isolation
Security
Routing Control
Private Communication
```

---

## VNet Architecture

```text
Azure Region
      │
      ▼
     VNet
      │
 ┌────┴────┐
 ▼         ▼
Public   Private
Subnet   Subnet
```

---

## Example Address Space

```text
10.0.0.0/16
```

Provides:

```text
65,536 Addresses
```

---

## Common Uses

```text
Virtual Machines
AKS Clusters
Databases
Application Services
```

---

## Best Practices

```text
Use Private Subnets
Plan IP Ranges Early
Separate Environments
Use Multiple Availability Zones
```

---

# 30. Subnets

Subnets divide a VNet into smaller logical networks.

---

## Public Subnet

Contains:

```text
Load Balancers
Application Gateways
Bastion Hosts
```

---

## Private Subnet

Contains:

```text
Virtual Machines
Databases
AKS Nodes
```

---

## Architecture

```text
VNet
 │
 ├── Public Subnet
 │       │
 │       ▼
 │      ALB
 │
 └── Private Subnet
         │
         ▼
       VM
```

---

## Multi-AZ Design

```text
AZ-1
 ├─ Public
 └─ Private

AZ-2
 ├─ Public
 └─ Private
```

---

## Benefits

```text
Security
Scalability
Fault Isolation
```

---

# 31. Route Tables

Route tables control network traffic.

---

## Purpose

Determine:

```text
Where Traffic Goes
```

---

## Example Route

```text
Destination     Next Hop
0.0.0.0/0       Internet
10.0.0.0/16     Local
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

## Common Use Cases

```text
Internet Access
VPN Routing
Firewall Routing
Hybrid Connectivity
```

---

# 32. Internet Connectivity

Azure provides internet access through public resources.

---

## Architecture

```text
Internet
    │
    ▼
Public IP
    │
    ▼
Load Balancer
    │
    ▼
Application
```

---

## Public IP Address

Provides:

```text
Inbound Access
Outbound Access
```

---

## Common Services

```text
Load Balancer
Application Gateway
Virtual Machine
Azure Firewall
```

---

## Best Practices

```text
Minimize Public Exposure
Use NSGs
Use Azure Firewall
Use Private Endpoints
```

---

# 33. NAT Gateway

Provides outbound internet access for private resources.

---

## Why NAT?

Private resources need:

```text
OS Updates
Package Downloads
Container Images
API Calls
```

---

## Architecture

```text
Private VM
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
Private Infrastructure
Secure Outbound Traffic
Scalable Connectivity
```

---

## Common Uses

```text
AKS
Virtual Machines
Private Applications
```

---

# 34. Network Security Groups (NSGs)

NSGs act as virtual firewalls.

---

## Characteristics

```text
Stateful
Allow Rules
Deny Rules
Subnet Level
NIC Level
```

---

## Architecture

```text
Internet
    │
    ▼
 NSG
    │
    ▼
 Virtual Machine
```

---

## Common Rules

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

## Best Practices

```text
Least Privilege
Restrict SSH
Use Separate NSGs
Audit Rules Regularly
```

---

# 35. Azure Firewall

Managed cloud firewall service.

---

## Purpose

Provides:

```text
Centralized Security
Traffic Inspection
Threat Protection
```

---

## Architecture

```text
Internet
    │
    ▼
Azure Firewall
    │
    ▼
Applications
```

---

## Features

```text
Application Rules
Network Rules
Threat Intelligence
Logging
```

---

## Common Uses

```text
Enterprise Security
Hybrid Cloud
Centralized Inspection
```

---

# 36. Azure DNS

Managed DNS service.

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
Azure DNS
 │
 ▼
Application
```

---

## Benefits

```text
High Availability
Fast Resolution
Global Reach
```

---

## Common Use Cases

```text
Public Websites
Applications
Hybrid DNS
```

---

# 37. Azure CDN

Content Delivery Network.

---

## Purpose

Deliver content closer to users.

---

## Architecture

```text
Users
   │
   ▼
Azure CDN
   │
   ▼
Origin
```

---

## Origins

```text
Blob Storage
Web Applications
Virtual Machines
```

---

## Benefits

```text
Lower Latency
Global Distribution
Caching
DDoS Protection
```

---

# 38. Azure Monitor

Central monitoring platform.

---

## Monitors

```text
Virtual Machines
AKS
Databases
Applications
Networks
```

---

## Architecture

```text
Azure Resources
        │
        ▼
Azure Monitor
        │
        ▼
Metrics & Logs
```

---

## Metrics Examples

```text
CPU Usage
Memory Usage
Disk Usage
Network Traffic
```

---

## Alerts

Example:

```text
CPU > 80%
```

Trigger:

```text
Email
SMS
Webhook
```

---

# 39. Activity Logs

Azure Activity Logs record platform events.

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
User Deleted VM
```

Activity Log records:

```text
Username
Timestamp
Resource
Action
```

---

## Benefits

```text
Auditing
Compliance
Troubleshooting
```

---

# 40. Azure Policy

Governance and compliance service.

---

## Purpose

Enforce standards.

---

## Examples

Require:

```text
Resource Tags
Approved Regions
Encryption
```

---

## Architecture

```text
Azure Policy
      │
      ▼
Azure Resources
```

---

## Benefits

```text
Governance
Compliance
Standardization
```

---

# 41. Azure DevOps Services

Azure DevOps provides CI/CD and project management.

---

## Core Services

```text
Azure Repos
Azure Pipelines
Azure Boards
Azure Artifacts
Azure Test Plans
```

---

## DevOps Workflow

```text
Developer
    │
    ▼
Azure Repos
    │
    ▼
Azure Pipelines
    │
    ▼
Deployment
```

---

# 42. Azure Repos

Git repository hosting service.

---

## Features

```text
Git Repositories
Branch Policies
Pull Requests
Code Reviews
```

---

## Architecture

```text
Developer
      │
      ▼
Azure Repos
```

---

## Benefits

```text
Source Control
Security
Collaboration
```

---

# 43. Azure Pipelines

CI/CD automation platform.

---

## Workflow

```text
Source Code
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

## Supported Targets

```text
Azure
AWS
GCP
Kubernetes
On-Prem
```

---

## Benefits

```text
Automation
Consistency
Faster Releases
```

---

# 44. Azure Artifacts

Package management service.

---

## Supported Packages

```text
NuGet
npm
Maven
Python
Universal Packages
```

---

## Use Cases

```text
Internal Packages
Dependency Management
Version Control
```

---

# 45. Azure Boards

Project and work tracking service.

---

## Features

```text
Kanban Boards
Sprint Planning
Backlogs
Work Items
```

---

## Workflow

```text
Task
  │
  ▼
Board
  │
  ▼
Sprint
  │
  ▼
Completion
```

---

## Benefits

```text
Agile Planning
Team Collaboration
Project Tracking
```

---

## Most Important Services

```text
VNet
NSG
Azure Firewall
Azure Monitor
Azure DNS
Azure Pipelines
```

---

## Most Important Concepts

```text
Private Networking
Network Security
Monitoring
Governance
CI/CD
DevOps
```

---

# 46. High Availability

High Availability (HA) ensures applications remain operational during failures.

---

## Single Zone Architecture

```text
Users
  │
  ▼
VM
  │
  ▼
Zone 1
```

Risk:

```text
Zone Failure
=
Application Down
```

---

## Multi-Zone Architecture

```text
Users
   │
   ▼
Load Balancer
   │
   ├── VM (Zone 1)
   └── VM (Zone 2)
```

Benefits:

```text
Fault Tolerance
Automatic Recovery
High Availability
```

---

## Availability Sets

Availability Sets distribute VMs across:

```text
Fault Domains
Update Domains
```

Architecture:

```text
Availability Set
      │
      ├── VM1
      ├── VM2
      └── VM3
```

---

## High Availability Best Practices

```text
Use Availability Zones
Use Load Balancers
Use VM Scale Sets
Use Managed Services
Avoid Single Points of Failure
```

---

# 47. Scalability Patterns

Scalability allows applications to handle increased workloads.

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
B2s
 ▼
D4s_v5
```

---

## Horizontal Scaling

Add more instances.

```text
VM1
VM2
VM3
VM4
```

---

## Scale Set Architecture

```text
Users
  │
  ▼
Load Balancer
  │
  ▼
VM Scale Set
  │
  ├── VM
  ├── VM
  └── VM
```

---

## Services Supporting Scaling

```text
VM Scale Sets
AKS
Azure Functions
Cosmos DB
App Service
```

---

# 48. Disaster Recovery

Disaster Recovery focuses on recovering from major failures.

---

## DR Architecture

```text
Primary Region
      │
      ▼
Replication
      │
      ▼
Secondary Region
```

---

## Recovery Strategy

```text
Application
      │
      ▼
Backup
      │
      ▼
Storage
      │
      ▼
Recovery
```

---

## Recovery Models

### Backup and Restore

```text
Lowest Cost
Longest Recovery Time
```

---

### Pilot Light

```text
Minimal Running Environment
Fast Recovery
```

---

### Warm Standby

```text
Partially Running Environment
```

---

### Active-Active

```text
Region A
   │
   ▼
Region B

Both Active
```

---

## Azure Site Recovery

Provides:

```text
VM Replication
Disaster Recovery
Failover
Failback
```

---

## Azure Backup

Supports:

```text
Virtual Machines
SQL Databases
Files
Disks
```

---

# 49. Cost Optimization

Cloud costs must be monitored continuously.

---

## Optimization Pillars

```text
Right Sizing
Auto Scaling
Reserved Capacity
Monitoring
```

---

## Common Cost Issues

```text
Unused VMs
Unused Disks
Unused Public IPs
Idle Load Balancers
Overprovisioned Resources
```

---

## Right Sizing

Bad:

```text
D16s_v5
10% CPU Usage
```

Good:

```text
B2s
```

---

## Reserved Instances

Commit to:

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

## Spot Virtual Machines

Use for:

```text
CI/CD
Batch Jobs
Testing
Development
```

Benefits:

```text
Significant Cost Savings
```

---

## Storage Optimization

Use:

```text
Lifecycle Policies
Cool Tier
Archive Tier
```

---

# 50. Real-World Azure Architectures

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
(VMs / AKS)
  │
  ▼
Database Layer
(Azure SQL)
```

---

## Serverless Architecture

```text
Users
  │
  ▼
API Management
  │
  ▼
Azure Functions
  │
  ▼
Cosmos DB
```

---

## Kubernetes Architecture

```text
Users
  │
  ▼
Application Gateway
  │
  ▼
AKS Cluster
  │
  ▼
Pods
```

---

## DevOps Architecture

```text
Developer
    │
    ▼
Azure Repos
    │
    ▼
Azure Pipelines
    │
    ▼
Container Registry
    │
    ▼
AKS
```

---

## Monitoring Architecture

```text
Applications
      │
      ▼
Azure Monitor
      │
      ▼
Alerts
      │
      ▼
Notifications
```

---

# 51. Security Best Practices

Security should be implemented across every layer.

---

## Identity Security

Use:

```text
Entra ID
RBAC
Managed Identities
MFA
```

Avoid:

```text
Shared Accounts
Excessive Privileges
```

---

## Encryption

Enable encryption for:

```text
Storage Accounts
Managed Disks
Databases
Backups
```

---

## Network Security

Use:

```text
Network Security Groups
Azure Firewall
Private Endpoints
VPN Gateway
```

---

## Monitoring

Enable:

```text
Azure Monitor
Activity Logs
Defender for Cloud
Azure Policy
```

---

## Secrets Management

Use:

```text
Azure Key Vault
```

Never:

```text
Store Secrets in Code
```

---

# 52. Troubleshooting

---

## VM Not Reachable

Check:

```text
NSGs
Firewall Rules
Public IP
Route Tables
```

---

## Website Not Loading

Verify:

```text
Application Gateway
Backend Health
DNS
TLS Certificates
```

---

## Storage Access Issues

Check:

```text
RBAC
Storage Keys
Firewall Rules
Network Access
```

---

## AKS Issues

Verify:

```text
Node Status
Pods
Services
Ingress
```

---

## Function App Failures

Check:

```text
Application Insights
Logs
Permissions
Timeouts
```

---

## Useful Commands

Show Subscription:

```bash
az account show
```

---

List VMs:

```bash
az vm list
```

---

List Resource Groups:

```bash
az group list
```

---

# 53. Interview Questions

---

## What is Azure?

Microsoft Azure is a cloud computing platform providing infrastructure, platform, and software services.

---

## What is a Region?

A physical geographic location containing Azure datacenters.

---

## What is an Availability Zone?

An isolated datacenter location within a region.

---

## Difference Between Resource Group and Subscription?

| Resource Group      | Subscription         |
| ------------------- | -------------------- |
| Logical Container   | Billing Boundary     |
| Organizes Resources | Owns Resource Groups |

---

## What is RBAC?

Role-Based Access Control used to manage permissions.

---

## Difference Between Blob Storage and Azure Files?

| Service      | Type           |
| ------------ | -------------- |
| Blob Storage | Object Storage |
| Azure Files  | File Storage   |

---

## What is AKS?

Azure Kubernetes Service, Microsoft's managed Kubernetes platform.

---

## What is Azure Monitor?

Monitoring and observability platform.

---

## What is Azure Policy?

Governance and compliance enforcement service.

---

## Difference Between VM and VM Scale Set?

```text
VM
 └── Single Instance

VM Scale Set
 └── Multiple Auto-Scaling Instances
```

---

# 54. Additional Resources

## Official Resources

* Microsoft Learn
* Azure Architecture Center
* Azure Well-Architected Framework
* Azure Documentation

---

## Certifications

* Azure Fundamentals (AZ-900)
* Azure Administrator (AZ-104)
* Azure Developer (AZ-204)
* Azure Solutions Architect (AZ-305)
* Azure DevOps Engineer (AZ-400)

---

## Learning Resources

* Microsoft Learn
* Azure Workshops
* Azure Blog
* Azure Architecture Center

---

# 55. Quick Reference

## Identity

```bash
az login

az account show
```

---

## Resource Groups

```bash
az group list

az group create
```

---

## Virtual Machines

```bash
az vm list

az vm start

az vm stop
```

---

## Networking

```bash
az network vnet list

az network nsg list
```

---

## AKS

```bash
az aks list

az aks get-credentials
```

---

## Monitoring

```bash
az monitor metrics list
```

---

# 📎 Official Documentation

* Microsoft Azure: [https://azure.microsoft.com](https://azure.microsoft.com)
* Azure Documentation: [https://learn.microsoft.com/azure](https://learn.microsoft.com/azure)
* Microsoft Learn: [https://learn.microsoft.com](https://learn.microsoft.com)
* Azure Architecture Center: [https://learn.microsoft.com/azure/architecture](https://learn.microsoft.com/azure/architecture)

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, cloud engineering, DevOps, SRE, and architecture studies.

Primary references include:

* Microsoft Learn
* Azure Documentation
* Azure Architecture Center
* Azure Certification Resources

Microsoft Azure, Microsoft Entra ID, Azure Kubernetes Service, and related product names are trademarks of Microsoft Corporation.

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official Microsoft documentation.

---

# 🎉 Conclusion

You now understand:

* Azure Fundamentals
* Global Infrastructure
* Entra ID & RBAC
* Virtual Machines
* VM Scale Sets
* Blob Storage
* Managed Disks
* Azure Files
* Azure SQL
* Cosmos DB
* Azure Functions
* AKS
* Networking
* Security
* Monitoring
* DevOps Services
* High Availability
* Scalability
* Disaster Recovery
* Cost Optimization
* Real-World Architectures

Azure provides:

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

through a fully managed cloud platform designed for enterprise, hybrid, and cloud-native workloads.

---

**Last Updated:** 2026
**Platform:** Microsoft Azure
**License:** Proprietary Cloud Platform

For latest information, visit https://learn.microsoft.com/azure