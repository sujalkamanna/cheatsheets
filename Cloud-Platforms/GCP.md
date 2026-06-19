# Google Cloud Platform (GCP) Cheatsheet

![Google Cloud Logo](https://cloud.google.com/_static/cloud/images/social-icon-google-cloud-1200-630.png)

---

## Table of Contents

* [1. What is Google Cloud Platform (GCP)?](#1-what-is-google-cloud-platform-gcp)
* [2. Core Concepts](#2-core-concepts)
* [3. GCP Global Infrastructure](#3-gcp-global-infrastructure)
* [4. Shared Responsibility Model](#4-shared-responsibility-model)
* [5. Identity & Access Management (IAM)](#5-identity--access-management-iam)
* [6. Service Accounts](#6-service-accounts)
* [7. Google Cloud CLI (gcloud)](#7-google-cloud-cli-gcloud)
* [8. Security Fundamentals](#8-security-fundamentals)
* [9. Projects, Billing & Organizations](#9-projects-billing--organizations)
* [10. High Availability Concepts](#10-high-availability-concepts)
* [11. Scalability Concepts](#11-scalability-concepts)

* [Additional Resources](#additional-resources)
* [Quick Reference](#quick-reference)
* [📎 Official Documentation](#-official-documentation)
* [📄 Copyright](#-copyright)
* [📎 Disclaimer & Attribution](#-disclaimer--attribution)
* [🎉 Conclusion](#-conclusion) 

---

# 1. What is Google Cloud Platform (GCP)?

Google Cloud Platform (GCP) is Google's cloud computing platform that provides infrastructure, platform, and managed services for building modern applications.

GCP offers:

* Compute
* Storage
* Networking
* Databases
* Containers
* Artificial Intelligence
* Machine Learning
* Analytics
* Security
* DevOps Services

---

## Why GCP?

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
Google Cloud Services
     │
     ▼
Global Cloud Infrastructure
```

---

## Key Benefits

```text
Pay-As-You-Go
Global Network
High Performance
Managed Services
Elastic Scaling
Built-in Security
```

---

## Common Use Cases

### Web Applications

```text
Users
  │
  ▼
Load Balancer
  │
  ▼
Compute Engine
```

---

### Kubernetes Platforms

```text
Users
  │
  ▼
GKE
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
API Gateway
  │
  ▼
Cloud Functions
  │
  ▼
Firestore
```

---

# 2. Core Concepts

---

## Cloud Computing

Cloud computing provides resources over the internet.

Examples:

```text
Virtual Machines
Storage
Databases
Networking
Containers
```

---

## Regions

Regions are physical geographic locations.

Examples:

```text
us-central1
us-east1
europe-west1
asia-south1
asia-east1
```

---

## Zones

Each region contains multiple zones.

Example:

```text
us-central1
   │
 ┌─┼─┐
 ▼ ▼ ▼
a b c
```

---

## Benefits

```text
Fault Tolerance
High Availability
Scalability
```

---

## Resource Hierarchy

```text
Organization
      │
      ▼
Folder
      │
      ▼
Project
      │
      ▼
Resources
```

---

## Elasticity

Resources automatically adjust.

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

Ability to support growing workloads.

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

# 3. GCP Global Infrastructure

Google operates one of the world's largest private networks.

---

## Infrastructure Components

```text
Region
   │
   ▼
Zone
   │
   ▼
Data Center
```

---

## Global Architecture

```text
Users
   │
   ▼
Google Edge Network
   │
   ▼
Region
   │
   ▼
Zones
```

---

## Example Region

```text
asia-south1
(Mumbai)
```

Contains multiple zones.

---

## Multi-Zone Design

```text
Region
 │
 ├── Zone A
 ├── Zone B
 └── Zone C
```

---

## Benefits

```text
High Availability
Fault Isolation
Business Continuity
```

---

## Global Load Balancing

Google provides:

```text
Global Anycast Network
```

Architecture:

```text
Users
   │
   ▼
Global Load Balancer
   │
   ▼
Applications
```

---

# 4. Shared Responsibility Model

Security responsibilities are shared.

---

## Google Responsibility

Google manages:

```text
Physical Datacenters
Networking Hardware
Storage Hardware
Cloud Infrastructure
```

---

## Customer Responsibility

Customers manage:

```text
Data
Applications
Identity
Permissions
Configurations
```

---

## Model

```text
Google
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

Compute Engine VM:

Google Secures:

```text
Physical Servers
Storage Systems
Network Infrastructure
```

Customer Secures:

```text
Operating System
Applications
Users
Data
```

---

# 5. Identity & Access Management (IAM)

IAM controls access to GCP resources.

---

## Core Components

```text
Users
Groups
Roles
Policies
Service Accounts
```

---

## Users

Represents a human identity.

Example:

```text
admin@example.com
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

## Roles

Define permissions.

---

### Primitive Roles

```text
Owner
Editor
Viewer
```

---

### Predefined Roles

Example:

```text
Compute Admin
Storage Admin
Kubernetes Admin
```

---

### Custom Roles

Create organization-specific permissions.

---

## IAM Architecture

```text
User
 │
 ▼
Role
 │
 ▼
Resource
```

---

## IAM Best Practices

```text
Least Privilege
Use Groups
Avoid Excessive Owner Access
Audit Permissions
```

---

# 6. Service Accounts

Service Accounts provide identities for applications.

---

## Purpose

Allow services to securely access GCP resources.

---

## Architecture

```text
Application
      │
      ▼
Service Account
      │
      ▼
GCP Resources
```

---

## Common Uses

```text
Compute Engine
GKE
Cloud Functions
Cloud Run
```

---

## Benefits

```text
No Passwords
Automation
Secure Access
```

---

## Best Practices

```text
Least Privilege
Avoid Long-Lived Keys
Rotate Credentials
```

---

# 7. Google Cloud CLI (gcloud)

The gcloud CLI manages GCP resources.

---

## Verify Installation

```bash
gcloud version
```

---

## Authenticate

```bash
gcloud auth login
```

---

## Show Current Account

```bash
gcloud auth list
```

---

## List Projects

```bash
gcloud projects list
```

---

## Set Active Project

```bash
gcloud config set project PROJECT_ID
```

---

## List Compute Instances

```bash
gcloud compute instances list
```

---

## Common Commands

```text
gcloud compute
gcloud storage
gcloud container
gcloud iam
gcloud projects
```

---

# 8. Security Fundamentals

Security is a core part of GCP.

---

## Security Layers

```text
IAM
Service Accounts
Firewall Rules
Encryption
Cloud Audit Logs
Security Command Center
```

---

## Encryption

Google encrypts data by default.

---

### At Rest

Examples:

```text
Cloud Storage
Persistent Disk
Cloud SQL
```

---

### In Transit

```text
HTTPS
TLS
SSL
```

---

## Multi-Factor Authentication

Protect:

```text
Administrators
Privileged Accounts
Developers
```

---

## Security Command Center

Provides:

```text
Security Insights
Threat Detection
Compliance Monitoring
```

---

## Security Best Practices

```text
Enable MFA
Use IAM Roles
Use Service Accounts
Audit Activity
```

---

# 9. Projects, Billing & Organizations

Projects are the primary resource containers in GCP.

---

## Hierarchy

```text
Organization
      │
      ▼
Folder
      │
      ▼
Project
      │
      ▼
Resources
```

---

## Organization

Top-level container.

Example:

```text
company.com
```

---

## Folder

Used for grouping projects.

Examples:

```text
Production
Development
Testing
```

---

## Project

Contains resources.

Examples:

```text
Compute Engine
Cloud Storage
GKE
Cloud SQL
```

---

## Billing

Billing accounts pay for resources.

Architecture:

```text
Billing Account
       │
       ▼
Projects
       │
       ▼
Resources
```

---

## Cost Management

Provides:

```text
Budgets
Cost Reports
Forecasting
Alerts
```

---

# 10. High Availability Concepts

High Availability keeps applications available during failures.

---

## Single Zone Architecture

```text
Users
  │
  ▼
VM
  │
  ▼
Zone A
```

Risk:

```text
Zone Failure
=
Downtime
```

---

## Multi-Zone Architecture

```text
Users
   │
   ▼
Load Balancer
   │
   ├── VM (Zone A)
   └── VM (Zone B)
```

---

## Benefits

```text
Fault Tolerance
Resilience
Automatic Recovery
```

---

## HA Services

```text
Managed Instance Groups
Cloud SQL HA
GKE
Load Balancing
```

---

# 11. Scalability Concepts

Scalability allows systems to grow with demand.

---

## Vertical Scaling

Increase resource size.

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

## Managed Instance Groups

Automatically manage VM scaling.

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
Managed Instance Group
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
Project
Region
Zone
IAM
Service Account
High Availability
Scalability
```

---

## Most Important Commands

```bash
gcloud auth login

gcloud projects list

gcloud config set project PROJECT_ID

gcloud compute instances list
```

---

# 12. Compute Services

Compute services provide processing power for applications.

GCP offers:

```text
Compute Engine
Managed Instance Groups
Cloud Run
Cloud Functions
GKE
App Engine
```

---

## Compute Service Selection

```text
Need Virtual Machines?
        │
        ▼
Compute Engine

Need Kubernetes?
        │
        ▼
GKE

Need Serverless?
        │
        ▼
Cloud Functions
Cloud Run
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
  ├── Compute Engine
  ├── GKE
  ├── Cloud Run
  └── Cloud Functions
```

---

# 13. Compute Engine

Compute Engine provides virtual machines on Google Cloud.

---

## Common Use Cases

```text
Web Servers
Application Servers
Jenkins
GitLab
Databases
Monitoring Platforms
```

---

## Architecture

```text
Compute Engine VM
        │
        ├── CPU
        ├── Memory
        ├── Disk
        └── Network
```

---

## Machine Families

### General Purpose

```text
E2
N2
N2D
```

---

### Compute Optimized

```text
C2
C3
```

---

### Memory Optimized

```text
M1
M2
```

---

### Accelerator Optimized

```text
A2
GPU Instances
```

---

## Create VM

```bash
gcloud compute instances create vm-1
```

---

## List VMs

```bash
gcloud compute instances list
```

---

## Start VM

```bash
gcloud compute instances start vm-1
```

---

## Stop VM

```bash
gcloud compute instances stop vm-1
```

---

## Delete VM

```bash
gcloud compute instances delete vm-1
```

---

## Best Practices

```text
Use Service Accounts
Enable Monitoring
Use Managed Instance Groups
Avoid Public Exposure
Use Labels
```

---

# 14. Managed Instance Groups (MIG)

Managed Instance Groups provide automated VM management.

---

## Purpose

Automatically scale infrastructure.

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

## Architecture

```text
Users
  │
  ▼
Load Balancer
  │
  ▼
Managed Instance Group
  │
  ├── VM1
  ├── VM2
  └── VM3
```

---

## Features

```text
Auto Scaling
Auto Healing
Rolling Updates
High Availability
```

---

## Benefits

```text
Reduced Operations
Automatic Recovery
Scalability
```

---

# 15. Load Balancing

Google Cloud provides global load balancing.

---

## Types

### HTTP(S) Load Balancer

```text
Layer 7
Global
```

---

### TCP Load Balancer

```text
Layer 4
High Performance
```

---

### Internal Load Balancer

```text
Private Traffic
```

---

## Architecture

```text
Users
   │
   ▼
Load Balancer
   │
   ├── VM1
   ├── VM2
   └── VM3
```

---

## Benefits

```text
Global Reach
Automatic Scaling
High Availability
```

---

# 16. Storage Services

GCP provides multiple storage options.

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
Cloud Storage
 └── Object Storage

Persistent Disk
 └── Block Storage

Filestore
 └── File Storage

Archive
 └── Long-Term Storage
```

---

# 17. Cloud Storage

Cloud Storage is Google's object storage service.

---

## Features

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
Media Files
Data Lakes
```

---

## Architecture

```text
Applications
      │
      ▼
Bucket
      │
      ├── File1
      ├── File2
      └── File3
```

---

## Storage Classes

### Standard

```text
Frequently Accessed Data
```

---

### Nearline

```text
Monthly Access
```

---

### Coldline

```text
Rare Access
```

---

### Archive

```text
Long-Term Retention
```

---

## Create Bucket

```bash
gcloud storage buckets create gs://my-bucket
```

---

## List Buckets

```bash
gcloud storage buckets list
```

---

## Upload File

```bash
gcloud storage cp file.txt gs://my-bucket
```

---

## Best Practices

```text
Enable Versioning
Use Lifecycle Rules
Encrypt Data
Use IAM Controls
```

---

# 18. Persistent Disk

Persistent Disk provides block storage for Compute Engine.

---

## Architecture

```text
Virtual Machine
       │
       ▼
Persistent Disk
```

---

## Types

### Standard HDD

```text
Cost Effective
```

---

### Balanced SSD

```text
General Purpose
```

---

### SSD

```text
High Performance
```

---

### Extreme PD

```text
Maximum IOPS
```

---

## Common Uses

```text
Operating Systems
Applications
Databases
```

---

## Best Practices

```text
Enable Snapshots
Monitor Performance
Use Encryption
```

---

# 19. Filestore

Managed file storage service.

---

## Architecture

```text
VM1
 │
 ├── Filestore
 │
VM2
```

---

## Features

```text
NFS Support
Shared Storage
Managed Service
```

---

## Common Uses

```text
Containers
Shared Applications
Enterprise Storage
```

---

## Benefits

```text
High Performance
Managed Operations
Scalable Capacity
```

---

# 20. Database Services

Google Cloud provides multiple database solutions.

---

## Categories

```text
Relational
NoSQL
Analytics
In-Memory
```

---

## Services

```text
Cloud SQL
Firestore
Bigtable
Spanner
Memorystore
```

---

# 21. Cloud SQL

Managed relational database service.

---

## Supported Engines

```text
MySQL
PostgreSQL
SQL Server
```

---

## Architecture

```text
Application
      │
      ▼
Cloud SQL
      │
      ▼
Database
```

---

## Benefits

```text
Managed Backups
Automatic Patching
High Availability
Monitoring
```

---

## Use Cases

```text
Web Applications
Business Systems
Enterprise Applications
```

---

# 22. Firestore

Serverless NoSQL database.

---

## Characteristics

```text
Serverless
Scalable
Fully Managed
```

---

## Architecture

```text
Application
      │
      ▼
Firestore
```

---

## Common Uses

```text
Mobile Apps
Web Apps
Microservices
```

---

## Benefits

```text
Real-Time Sync
Global Availability
Automatic Scaling
```

---

# 23. Bigtable

Massively scalable NoSQL database.

---

## Characteristics

```text
Petabyte Scale
Millisecond Latency
High Throughput
```

---

## Common Uses

```text
IoT
Analytics
Time-Series Data
Financial Systems
```

---

## Architecture

```text
Applications
      │
      ▼
Bigtable Cluster
```

---

# 24. Serverless Computing

Serverless removes infrastructure management.

---

## GCP Serverless Services

```text
Cloud Functions
Cloud Run
Eventarc
Workflows
```

---

## Benefits

```text
No Servers
Automatic Scaling
Pay Per Use
```

---

# 25. Cloud Functions

Event-driven serverless compute platform.

---

## Workflow

```text
Event
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
Cloud Storage
Pub/Sub
Cloud Audit Logs
```

---

## Common Uses

```text
Automation
File Processing
API Backends
Scheduled Tasks
```

---

## Benefits

```text
Serverless
Cost Efficient
Auto Scaling
```

---

# 26. Container Services

GCP supports modern container workloads.

---

## Services

```text
GKE
Cloud Run
Artifact Registry
```

---

## Selection

```text
Need Kubernetes?
      │
      ▼
      GKE

Need Serverless Containers?
      │
      ▼
   Cloud Run
```

---

# 27. Google Kubernetes Engine (GKE)

Managed Kubernetes service.

---

## Architecture

```text
Users
  │
  ▼
GKE Control Plane
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
Nodes
Pods
Services
Ingress
```

---

## Benefits

```text
Managed Kubernetes
High Availability
Automatic Upgrades
Integrated Security
```

---

## Common Uses

```text
Microservices
DevOps Platforms
Cloud Native Applications
```

---

## Workflow

```text
Developer
     │
     ▼
kubectl
     │
     ▼
GKE Cluster
     │
     ▼
Pods
```

---

# 28. Networking Fundamentals

Networking connects cloud resources and enables secure communication.

---

## Network Architecture

```text id="sjq7ea"
Internet
    │
    ▼
Cloud DNS
    │
    ▼
Load Balancer
    │
    ▼
VPC Network
    │
    ▼
Applications
```

---

## Core Networking Components

```text id="bznn8x"
VPC
Subnets
Routes
Cloud NAT
Firewall Rules
Cloud DNS
Cloud CDN
```

---

## Typical Traffic Flow

```text id="wrhrvp"
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

# 29. Virtual Private Cloud (VPC)

VPC is Google's private networking service.

---

## Purpose

Provides:

```text id="fkhj0f"
Network Isolation
Private Communication
Security
Routing Control
```

---

## Architecture

```text id="qfjlwm"
Google Cloud
      │
      ▼
      VPC
      │
 ┌────┴────┐
 ▼         ▼
Subnet   Subnet
```

---

## Example CIDR

```text id="mjlwmh"
10.0.0.0/16
```

---

## Benefits

```text id="1ch0z6"
Private Networking
Scalability
Security
```

---

## Best Practices

```text id="7x5b4t"
Plan IP Ranges
Separate Environments
Use Private Resources
Avoid Overlapping CIDRs
```

---

# 30. Subnets

Subnets divide VPC networks into smaller segments.

---

## Purpose

Provide logical separation.

---

## Architecture

```text id="frdtyv"
VPC
 │
 ├── Public Subnet
 │
 └── Private Subnet
```

---

## Common Uses

Public Subnet:

```text id="1xvr5w"
Load Balancers
Bastion Hosts
```

---

Private Subnet:

```text id="v63n4d"
Applications
Databases
GKE Nodes
```

---

## Benefits

```text id="dyz6s4"
Security
Organization
Scalability
```

---

# 31. Routes

Routes determine traffic paths.

---

## Route Structure

```text id="x5g3e0"
Destination
     │
     ▼
Next Hop
```

---

## Example Routes

```text id="p0r1a8"
0.0.0.0/0
      │
      ▼
Internet Gateway

10.0.0.0/16
      │
      ▼
Local
```

---

## Common Uses

```text id="cyk1zs"
Internet Access
Private Connectivity
Hybrid Networking
```

---

# 32. Internet Connectivity

Provides communication with external systems.

---

## Architecture

```text id="06yh6z"
Internet
    │
    ▼
Load Balancer
    │
    ▼
Applications
```

---

## Public IP Addresses

Provide:

```text id="qvht4i"
Inbound Access
Outbound Access
```

---

## Best Practices

```text id="8w0wud"
Minimize Public Exposure
Use Load Balancers
Use Firewall Rules
Use Cloud Armor
```

---

# 33. Cloud NAT

Cloud NAT provides outbound internet access.

---

## Purpose

Allows private resources to reach the internet.

---

## Architecture

```text id="7w8bvl"
Private VM
     │
     ▼
Cloud NAT
     │
     ▼
Internet
```

---

## Common Uses

```text id="t09b8n"
Private VMs
Private GKE Clusters
Application Servers
```

---

## Benefits

```text id="xtdgbf"
No Public IPs
Secure Outbound Traffic
Scalable Connectivity
```

---

# 34. Firewall Rules

Firewall Rules control network access.

---

## Characteristics

```text id="z0h7pi"
Allow Rules
Deny Rules
Ingress Rules
Egress Rules
```

---

## Architecture

```text id="n3h6bs"
Internet
    │
    ▼
Firewall Rule
    │
    ▼
VM
```

---

## Common Ports

SSH:

```text id="c5ibn9"
TCP 22
```

---

HTTP:

```text id="53clcf"
TCP 80
```

---

HTTPS:

```text id="a0yz4i"
TCP 443
```

---

## Best Practices

```text id="nmtvlg"
Least Privilege
Restrict SSH
Audit Rules
Use Tags
```

---

# 35. Cloud DNS

Managed DNS service.

---

## Purpose

Resolves:

```text id="m9ax7m"
example.com
```

into:

```text id="wjlwm4"
IP Address
```

---

## Architecture

```text id="s0tlxk"
User
 │
 ▼
Cloud DNS
 │
 ▼
Application
```

---

## Benefits

```text id="8t3fj6"
Global Availability
Fast Resolution
Managed Service
```

---

## Common Uses

```text id="5opn4w"
Websites
Applications
Hybrid DNS
```

---

# 36. Cloud CDN

Content Delivery Network service.

---

## Purpose

Cache content closer to users.

---

## Architecture

```text id="6f34yx"
Users
   │
   ▼
Cloud CDN
   │
   ▼
Origin
```

---

## Origins

```text id="jsa1y0"
Cloud Storage
Load Balancer
Applications
```

---

## Benefits

```text id="4ud4bo"
Lower Latency
Global Delivery
Reduced Origin Load
```

---

# 37. Cloud Monitoring

Monitoring and observability platform.

---

## Monitors

```text id="7n8s77"
VMs
GKE
Databases
Applications
Networks
```

---

## Architecture

```text id="0y9zxy"
Resources
    │
    ▼
Cloud Monitoring
    │
    ▼
Metrics
```

---

## Common Metrics

```text id="a1nkgu"
CPU Usage
Memory Usage
Disk Usage
Network Traffic
```

---

## Alerting

Example:

```text id="k6pnru"
CPU > 80%
```

---

Notifications:

```text id="0k8rd4"
Email
SMS
Webhook
```

---

# 38. Cloud Logging

Centralized logging service.

---

## Purpose

Collect logs from resources.

---

## Architecture

```text id="6kg1lj"
Applications
      │
      ▼
Cloud Logging
```

---

## Sources

```text id="vnvqzr"
Compute Engine
GKE
Cloud Functions
Cloud SQL
```

---

## Benefits

```text id="a9av50"
Troubleshooting
Monitoring
Auditing
```

---

# 39. Cloud Audit Logs

Audit service for GCP activities.

---

## Tracks

```text id="vcllmz"
Who
What
When
Where
```

---

## Example

```text id="kv6tv4"
User Deleted VM
```

---

Audit Log Records:

```text id="e5lcb5"
User
Timestamp
Resource
Action
```

---

## Benefits

```text id="mj6fw5"
Compliance
Security
Troubleshooting
```

---

# 40. Organization Policies

Governance service.

---

## Purpose

Enforce standards.

---

## Examples

```text id="5ph6s6"
Restrict Regions
Require Labels
Restrict Public IPs
```

---

## Architecture

```text id="djlwm0"
Organization Policy
        │
        ▼
Projects
```

---

## Benefits

```text id="h4m4kn"
Compliance
Governance
Standardization
```

---

# 41. DevOps Services

GCP provides services for CI/CD and software delivery.

---

## Core Services

```text id="whj5j0"
Cloud Source Repositories
Cloud Build
Artifact Registry
Cloud Deploy
```

---

## Workflow

```text id="xwjlwm"
Developer
    │
    ▼
Repository
    │
    ▼
Build
    │
    ▼
Deploy
```

---

# 42. Cloud Source Repositories

Private Git repositories hosted on GCP.

---

## Features

```text id="rfzyi8"
Git Repositories
Version Control
Code Storage
```

---

## Benefits

```text id="2bp8j0"
Collaboration
Security
Integration
```

---

# 43. Cloud Build

Continuous Integration service.

---

## Workflow

```text id="w9mrmg"
Code
 │
 ▼
Build
 │
 ▼
Test
 │
 ▼
Artifact
```

---

## Features

```text id="jef0qj"
Build Automation
Testing
Container Builds
```

---

## Benefits

```text id="tzlt8f"
CI/CD
Automation
Consistency
```

---

# 44. Artifact Registry

Stores software artifacts.

---

## Supported Artifacts

```text id="5kvkgg"
Docker Images
Helm Charts
Maven Packages
npm Packages
Python Packages
```

---

## Architecture

```text id="6a1e4n"
Build
 │
 ▼
Artifact Registry
 │
 ▼
Deployment
```

---

## Benefits

```text id="aqh56k"
Version Control
Secure Storage
Integration
```

---

# 45. Cloud Deploy

Managed continuous delivery service.

---

## Workflow

```text id="jhj9aq"
Build
 │
 ▼
Artifact
 │
 ▼
Cloud Deploy
 │
 ▼
Production
```

---

## Features

```text id="3mjlwm"
Progressive Delivery
Approvals
Deployment Automation
```

---

## Common Targets

```text id="49x6bh"
GKE
Cloud Run
Hybrid Environments
```

---

## Benefits

```text id="9lcp5n"
Safer Releases
Automation
Standardization
```

---

# 46. High Availability

High Availability (HA) ensures applications remain available during failures.

---

## Single Zone Architecture

```text id="p1m4v8"
Users
  │
  ▼
VM
  │
  ▼
Zone A
```

Risk:

```text id="e4s9k2"
Zone Failure
=
Application Downtime
```

---

## Multi-Zone Architecture

```text id="g7d2q5"
Users
   │
   ▼
Load Balancer
   │
   ├── VM (Zone A)
   ├── VM (Zone B)
   └── VM (Zone C)
```

---

## Regional Deployments

```text id="u6y1r3"
Region
 │
 ├── Zone A
 ├── Zone B
 └── Zone C
```

Benefits:

```text id="t9w5h7"
Fault Tolerance
Automatic Recovery
Improved Reliability
```

---

## High Availability Best Practices

```text id="c3j8m1"
Use Multiple Zones
Use Load Balancers
Use Managed Services
Enable Backups
Avoid Single Points of Failure
```

---

# 47. Scalability Patterns

Scalability allows systems to handle increased workloads.

---

## Vertical Scaling

Increase resources of a single instance.

```text id="v2r8n6"
2 vCPU
   ▼
8 vCPU
```

---

## Horizontal Scaling

Add more instances.

```text id="y5q3p9"
VM1
VM2
VM3
VM4
```

---

## Auto Scaling Architecture

```text id="m8s2w4"
Users
  │
  ▼
Load Balancer
  │
  ▼
Managed Instance Group
  │
  ├── VM
  ├── VM
  └── VM
```

---

## Services Supporting Scaling

```text id="h1f7k3"
Managed Instance Groups
GKE
Cloud Run
Cloud Functions
Bigtable
Spanner
```

---

# 48. Disaster Recovery

Disaster Recovery (DR) helps restore services after failures.

---

## Recovery Architecture

```text id="r4d8u2"
Primary Region
      │
      ▼
Replication
      │
      ▼
Secondary Region
```

---

## Backup Strategy

```text id="k7w1n5"
Application
      │
      ▼
Backup
      │
      ▼
Cloud Storage
      │
      ▼
Recovery
```

---

## Recovery Models

### Backup and Restore

```text id="j9v2x6"
Lowest Cost
Longest Recovery Time
```

---

### Pilot Light

```text id="d3h8s1"
Minimal Running Environment
```

---

### Warm Standby

```text id="a6m5t9"
Partially Running Environment
```

---

### Active-Active

```text id="z8r4q7"
Region A
   │
   ▼
Region B

Both Active
```

---

## Disaster Recovery Services

```text id="n5p1j4"
Cloud Storage
Snapshots
Cloud SQL Backups
Persistent Disk Snapshots
```

---

# 49. Cost Optimization

Cloud costs should be monitored continuously.

---

## Cost Optimization Pillars

```text id="q2c7k8"
Right Sizing
Auto Scaling
Monitoring
Storage Lifecycle Policies
```

---

## Common Cost Issues

```text id="x6v9f2"
Unused VMs
Unused Disks
Idle Resources
Unused Static IPs
```

---

## Right Sizing

Bad:

```text id="m4t7n3"
n2-standard-32
10% CPU Usage
```

Good:

```text id="b8h5w1"
e2-medium
```

---

## Spot VMs

Use for:

```text id="y1j4r8"
Testing
CI/CD
Batch Processing
Development
```

Benefits:

```text id="s3n7k5"
Significant Cost Savings
```

---

## Storage Optimization

Use:

```text id="c9p2m6"
Lifecycle Policies
Nearline
Coldline
Archive Storage
```

---

# 50. Real-World GCP Architectures

---

## Three-Tier Web Application

```text id="f7w3k1"
Users
  │
  ▼
Load Balancer
  │
  ▼
Application Layer
(Compute Engine / GKE)
  │
  ▼
Database Layer
(Cloud SQL)
```

---

## Serverless Architecture

```text id="u4x9h2"
Users
  │
  ▼
API Gateway
  │
  ▼
Cloud Functions
  │
  ▼
Firestore
```

---

## Kubernetes Architecture

```text id="e6q1m7"
Users
  │
  ▼
Load Balancer
  │
  ▼
GKE Cluster
  │
  ▼
Pods
```

---

## DevOps Architecture

```text id="p8k4s3"
Developer
    │
    ▼
Source Repository
    │
    ▼
Cloud Build
    │
    ▼
Artifact Registry
    │
    ▼
GKE
```

---

## Monitoring Architecture

```text id="g2t8v9"
Applications
      │
      ▼
Cloud Monitoring
      │
      ▼
Alerts
      │
      ▼
Notifications
```

---

# 51. Security Best Practices

Security should exist at every layer.

---

## Identity Security

Use:

```text id="r5m1j7"
IAM
Service Accounts
MFA
Least Privilege
```

Avoid:

```text id="k3p9d2"
Excessive Admin Access
Shared Accounts
```

---

## Encryption

Enable encryption for:

```text id="w8s2n4"
Cloud Storage
Persistent Disks
Databases
Backups
```

---

## Network Security

Use:

```text id="y6h4q1"
Firewall Rules
Private Services
Cloud Armor
VPC Isolation
```

---

## Monitoring and Auditing

Enable:

```text id="v7k2c8"
Cloud Monitoring
Cloud Logging
Cloud Audit Logs
Security Command Center
```

---

## Secrets Management

Use:

```text id="f9r5m3"
Secret Manager
```

Never:

```text id="h4d8q7"
Store Secrets in Code
```

---

# 52. Troubleshooting

---

## VM Not Reachable

Check:

```text id="a1m7v5"
Firewall Rules
Routes
Public IP
Instance Status
```

---

## Website Not Loading

Verify:

```text id="b9s2k4"
Load Balancer
DNS
TLS Certificates
Backend Health
```

---

## Storage Access Issues

Check:

```text id="t3w8h1"
IAM Permissions
Bucket Policies
Network Restrictions
```

---

## GKE Issues

Verify:

```text id="u5p4n9"
Nodes
Pods
Services
Ingress
```

---

## Cloud Functions Failures

Check:

```text id="x8k1q6"
Logs
Permissions
Timeouts
Memory Limits
```

---

## Useful Commands

Current Project:

```bash id="g4n9k2"
gcloud config list
```

---

List Compute Instances:

```bash id="m2w7p5"
gcloud compute instances list
```

---

List Buckets:

```bash id="s6q1h8"
gcloud storage buckets list
```

---

# 53. Interview Questions

---

## What is GCP?

Google Cloud Platform is Google's cloud computing platform providing infrastructure, platform, and managed services.

---

## What is a Region?

A geographic location containing one or more zones.

---

## What is a Zone?

An isolated deployment area within a region.

---

## Difference Between Project and Organization?

| Project            | Organization        |
| ------------------ | ------------------- |
| Resource Container | Top-Level Container |
| Owns Resources     | Owns Projects       |

---

## What is IAM?

Identity and Access Management service used to control permissions.

---

## Difference Between Cloud Storage and Persistent Disk?

| Service         | Type           |
| --------------- | -------------- |
| Cloud Storage   | Object Storage |
| Persistent Disk | Block Storage  |

---

## What is GKE?

Google Kubernetes Engine is Google's managed Kubernetes service.

---

## What is Cloud Monitoring?

Monitoring and observability platform for GCP resources.

---

## What is Cloud NAT?

Provides outbound internet access for private resources.

---

## Difference Between Cloud Functions and Cloud Run?

```text id="j7p3m8"
Cloud Functions
 └── Event Driven

Cloud Run
 └── Container Based
```

---

# 54. Additional Resources

## Official Learning Resources

* Google Cloud Skills Boost
* Google Cloud Documentation
* Google Cloud Architecture Center
* Google Cloud Blog

---

## Certifications

* Cloud Digital Leader
* Associate Cloud Engineer
* Professional Cloud Architect
* Professional Cloud DevOps Engineer
* Professional Cloud Security Engineer

---

## Recommended Learning Areas

```text id="v3n8h5"
Compute Engine
Cloud Storage
IAM
Networking
GKE
DevOps
Security
```

---

# 55. Quick Reference

## Authentication

```bash id="c8q4m1"
gcloud auth login
```

---

## Projects

```bash id="h5r9k2"
gcloud projects list

gcloud config set project PROJECT_ID
```

---

## Compute Engine

```bash id="u2m7p4"
gcloud compute instances list

gcloud compute instances start VM_NAME

gcloud compute instances stop VM_NAME
```

---

## Storage

```bash id="y9w3k6"
gcloud storage buckets list

gcloud storage cp file.txt gs://bucket
```

---

## Kubernetes

```bash id="r4h8n1"
kubectl get nodes

kubectl get pods -A
```

---

## Monitoring

```bash id="n7v2q5"
gcloud monitoring metrics list
```

---

# 📎 Official Documentation

* Google Cloud Platform: https://cloud.google.com
* Google Cloud Documentation: https://cloud.google.com/docs
* Google Cloud Architecture Center: https://cloud.google.com/architecture
* Google Cloud Skills Boost: https://www.cloudskillsboost.google

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, cloud engineering, DevOps, SRE, and architecture studies.

Primary references include:

* Google Cloud Documentation
* Google Cloud Architecture Center
* Google Cloud Skills Boost
* Google Cloud Certification Resources

Google Cloud Platform (GCP), Google Kubernetes Engine (GKE), Compute Engine, Cloud Storage, and related services are trademarks of Google LLC.

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official Google Cloud documentation.

---

# 🎉 Conclusion

You now understand:

* GCP Fundamentals
* Global Infrastructure
* IAM & Service Accounts
* Compute Engine
* Managed Instance Groups
* Cloud Storage
* Persistent Disk
* Filestore
* Cloud SQL
* Firestore
* Bigtable
* Cloud Functions
* GKE
* Networking
* Monitoring
* DevOps Services
* High Availability
* Scalability
* Disaster Recovery
* Cost Optimization
* Security Best Practices

GCP combines:

```text id="z1h7p4"
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

into a highly scalable cloud platform for modern applications and enterprise workloads.

---

**Last Updated:** 2026
**Platform:** Google Cloud Platform (GCP)
**License:** Proprietary Cloud Platform

For latest information, visit https://cloud.google.com/docs