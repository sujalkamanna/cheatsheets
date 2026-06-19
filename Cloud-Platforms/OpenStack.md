# OpenStack Cheatsheet

---

![OpenStack Logo](https://www.openstack.org/themes/openstack/images/openstack-logo/full.svg)

---

## Table of Contents

* [1. What is OpenStack?](#1-what-is-openstack)
* [2. Why OpenStack?](#2-why-openstack)
* [3. OpenStack Architecture](#3-openstack-architecture)
* [4. Core Components Overview](#4-core-components-overview)
* [5. Keystone (Identity Service)](#5-keystone-identity-service)
* [6. Glance (Image Service)](#6-glance-image-service)
* [7. Nova (Compute Service)](#7-nova-compute-service)
* [8. Neutron (Networking Service)](#8-neutron-networking-service)
* [9. Horizon Dashboard](#9-horizon-dashboard)
* [10. OpenStack CLI Basics](#10-openstack-cli-basics)
* [11. Projects, Users & Roles](#11-projects-users--roles)
* [12. Availability Zones](#12-availability-zones)
* [13. Security Groups](#13-security-groups)

* [Additional Resources](#additional-resources)
* [Quick Reference](#quick-reference)
* [📎 Official Documentation](#-official-documentation)
* [📄 Copyright](#-copyright)
* [📎 Disclaimer & Attribution](#-disclaimer--attribution)
* [🎉 Conclusion](#-conclusion) 

---

# 1. What is OpenStack?

OpenStack is an open-source cloud computing platform used to build and manage private and public clouds.

It provides infrastructure services similar to AWS, Azure, and GCP.

---

## What OpenStack Provides

```text id="6p3v2s"
Compute
Networking
Storage
Identity
Images
Dashboard
```

---

## OpenStack Goal

```text id="u7f1kx"
Physical Infrastructure
          │
          ▼
OpenStack
          │
          ▼
Cloud Services
```

---

## Common Use Cases

```text id="f5m9dy"
Private Cloud
Enterprise Cloud
Telecom Infrastructure
Research Environments
Internal Developer Platforms
```

---

## Benefits

```text id="g8n2wr"
Open Source
Vendor Neutral
Scalable
Highly Customizable
Large Ecosystem
```

---

# 2. Why OpenStack?

Organizations use OpenStack to create cloud platforms on their own infrastructure.

---

## Traditional Infrastructure

```text id="b2v4qp"
Applications
     │
     ▼
Physical Servers
```

---

## OpenStack Infrastructure

```text id="t6k8yn"
Applications
     │
     ▼
Virtual Machines
     │
     ▼
OpenStack
     │
     ▼
Physical Infrastructure
```

---

## Advantages

```text id="h4x9me"
Self-Service Infrastructure
API Driven
Automation Friendly
Private Cloud Control
```

---

## Popular Industries

```text id="w7q3sf"
Telecommunications
Financial Services
Government
Research
Large Enterprises
```

---

# 3. OpenStack Architecture

OpenStack consists of multiple services working together.

---

## High-Level Architecture

```text id="k3m7vb"
Users
  │
  ▼
Horizon Dashboard
  │
  ▼
OpenStack APIs
  │
  ├── Keystone
  ├── Nova
  ├── Neutron
  ├── Glance
  ├── Cinder
  └── Swift
```

---

## Logical Architecture

```text id="q5j2cw"
Identity
    │
    ▼
Compute
    │
    ▼
Networking
    │
    ▼
Storage
```

---

## Main Building Blocks

```text id="a9t4hr"
Control Plane
Compute Nodes
Storage Nodes
Network Nodes
```

---

## Typical Deployment

```text id="m8y1pk"
Users
 │
 ▼
Controller Nodes
 │
 ▼
Compute Nodes
 │
 ▼
Storage
```

---

# 4. Core Components Overview

OpenStack consists of several core services.

---

## Core Services

| Service  | Purpose        |
| -------- | -------------- |
| Keystone | Identity       |
| Nova     | Compute        |
| Neutron  | Networking     |
| Glance   | Images         |
| Cinder   | Block Storage  |
| Swift    | Object Storage |
| Horizon  | Dashboard      |

---

## Service Relationships

```text id="r6d8tx"
Keystone
    │
    ▼
Authentication
    │
    ▼
Nova
Neutron
Glance
Cinder
```

---

## Service Flow

```text id="c2n7jq"
User
 │
 ▼
Keystone
 │
 ▼
Requested Service
```

---

# 5. Keystone (Identity Service)

Keystone provides authentication and authorization.

---

## Responsibilities

```text id="f9w3bd"
Authentication
Authorization
Service Catalog
Token Management
```

---

## Architecture

```text id="v4p6mx"
User
 │
 ▼
Keystone
 │
 ▼
Token
 │
 ▼
OpenStack Services
```

---

## Example Workflow

```text id="j7r2ka"
Login
  │
  ▼
Token Issued
  │
  ▼
Access Services
```

---

## Benefits

```text id="n5t8ys"
Centralized Identity
Security
Access Control
```

---

# 6. Glance (Image Service)

Glance manages virtual machine images.

---

## Purpose

Store VM templates.

---

## Examples

```text id="d3m7ph"
Ubuntu
CentOS
RHEL
Debian
Windows
```

---

## Architecture

```text id="x8q4wn"
Image
  │
  ▼
Glance
  │
  ▼
Nova
```

---

## Benefits

```text id="y6v1jt"
Fast VM Deployment
Centralized Images
Version Management
```

---

## Typical Workflow

```text id="s4n8fc"
Upload Image
      │
      ▼
Store in Glance
      │
      ▼
Launch Instance
```

---

# 7. Nova (Compute Service)

Nova manages virtual machine instances.

---

## Responsibilities

```text id="g1w7mz"
Create Instances
Delete Instances
Manage Hypervisors
Schedule Workloads
```

---

## Architecture

```text id="p6v2kr"
User
 │
 ▼
Nova API
 │
 ▼
Compute Nodes
 │
 ▼
Virtual Machines
```

---

## Supported Hypervisors

```text id="b8m4ty"
KVM
QEMU
VMware
Hyper-V
```

---

## Example Workflow

```text id="c7x5ns"
Request VM
    │
    ▼
Nova Scheduler
    │
    ▼
Compute Node
```

---

## Benefits

```text id="w3j9kp"
Scalable Compute
Automation
Multi-Hypervisor Support
```

---

# 8. Neutron (Networking Service)

Neutron provides networking services.

---

## Responsibilities

```text id="n4k8xb"
Networks
Subnets
Routers
Floating IPs
Security Groups
```

---

## Architecture

```text id="e9p1vt"
Instance
   │
   ▼
Neutron
   │
   ▼
Network
```

---

## Network Components

```text id="r7x3mh"
Network
Subnet
Router
Port
Floating IP
```

---

## Example Network Flow

```text id="q1w6dy"
Internet
   │
   ▼
Router
   │
   ▼
Subnet
   │
   ▼
Instance
```

---

## Benefits

```text id="u5m9ka"
Flexible Networking
Isolation
Multi-Tenant Support
```

---

# 9. Horizon Dashboard

Horizon is the web interface for OpenStack.

---

## Purpose

Manage OpenStack through a browser.

---

## Architecture

```text id="h8n2pc"
User
 │
 ▼
Browser
 │
 ▼
Horizon
 │
 ▼
OpenStack APIs
```

---

## Common Tasks

```text id="y4r7ts"
Launch Instances
Manage Networks
Create Volumes
Upload Images
Manage Users
```

---

## Benefits

```text id="k9v5mj"
Easy Administration
Visual Interface
Centralized Management
```

---

# 10. OpenStack CLI Basics

The OpenStack CLI manages resources from the command line.

---

## Verify Access

```bash id="x7n3pb"
openstack token issue
```

---

## List Services

```bash id="w2m8dy"
openstack service list
```

---

## List Images

```bash id="c5r1qt"
openstack image list
```

---

## List Networks

```bash id="j8p4vx"
openstack network list
```

---

## List Instances

```bash id="n6t9wk"
openstack server list
```

---

## List Projects

```bash id="u3x7mr"
openstack project list
```

---

## Common CLI Categories

```text id="f1v5dc"
server
network
image
volume
project
user
role
```

---

# 11. Projects, Users & Roles

OpenStack uses role-based access control.

---

## Project

A project is a logical resource container.

---

## Examples

```text id="q8y4pm"
Development
Production
Testing
```

---

## User

Represents a person or service.

---

## Role

Defines permissions.

---

## Architecture

```text id="d6w1tx"
User
 │
 ▼
Role
 │
 ▼
Project
```

---

## Common Roles

```text id="z4m8kv"
Admin
Member
Reader
```

---

# 12. Availability Zones

Availability Zones provide workload distribution.

---

## Purpose

Improve reliability.

---

## Architecture

```text id="g7x2pd"
OpenStack Cloud
       │
 ┌─────┼─────┐
 ▼     ▼     ▼
AZ1   AZ2   AZ3
```

---

## Benefits

```text id="n1w5rm"
Fault Isolation
High Availability
Workload Distribution
```

---

## Common Usage

```text id="u8v3jk"
Production Workloads
Critical Applications
```

---

# 13. Security Groups

Security Groups act as virtual firewalls.

---

## Purpose

Control traffic.

---

## Architecture

```text id="m4p7xt"
Security Group
      │
      ▼
Instance
```

---

## Example Rules

SSH:

```text id="v9n2cf"
TCP 22
```

---

HTTP:

```text id="r5j8my"
TCP 80
```

---

HTTPS:

```text id="x1w6pt"
TCP 443
```

---

## Benefits

```text id="c8q4vh"
Network Security
Access Control
Isolation
```

---

## Best Practices

```text id="b6t9wn"
Least Privilege
Restrict SSH
Review Rules Regularly
Use Separate Security Groups
```

---

# 14. Cinder (Block Storage)

Cinder provides persistent block storage for OpenStack instances.

---

## Purpose

Attach storage volumes to virtual machines.

---

## Architecture

```text id="a4c8vn"
Instance
   │
   ▼
Cinder Volume
```

---

## Common Uses

```text id="k7m2wp"
Databases
Application Data
Persistent Storage
Boot Volumes
```

---

## Benefits

```text id="r5x9td"
Persistent Storage
Flexible Capacity
Snapshots
High Performance
```

---

## Workflow

```text id="u1j6ms"
Create Volume
     │
     ▼
Attach to Instance
     │
     ▼
Use Storage
```

---

# 15. Swift (Object Storage)

Swift is OpenStack's object storage service.

---

## Purpose

Store unstructured data.

---

## Examples

```text id="f8v3cp"
Backups
Images
Videos
Documents
Logs
```

---

## Architecture

```text id="z2m7rw"
Applications
      │
      ▼
Swift
      │
      ▼
Object Storage
```

---

## Characteristics

```text id="c5p1kn"
Highly Scalable
Distributed
Durable
Redundant
```

---

## Benefits

```text id="n8w4tj"
Massive Scalability
Low Cost Storage
Data Durability
```

---

# 16. Manila (Shared File Storage)

Manila provides shared file systems.

---

## Purpose

Offer network-based file shares.

---

## Architecture

```text id="v4t8qy"
Instance A
    │
    ├─────── Shared Storage
    │
Instance B
```

---

## Supported Protocols

```text id="m7x2jd"
NFS
SMB
CIFS
```

---

## Use Cases

```text id="p3n6rv"
Shared Applications
Web Content
User Home Directories
```

---

## Benefits

```text id="h9c5wp"
Shared Access
Centralized Storage
Easy Management
```

---

# 17. Instance Lifecycle

Instances move through multiple states.

---

## Lifecycle

```text id="d8w3mt"
Create
  │
  ▼
Build
  │
  ▼
Active
  │
  ▼
Stopped
  │
  ▼
Deleted
```

---

## Common Actions

```text id="q2v7kx"
Start
Stop
Reboot
Resize
Delete
```

---

## Instance Workflow

```text id="r6n4jc"
Image
  │
  ▼
Nova
  │
  ▼
Instance
```

---

# 18. Flavors

Flavors define VM resources.

---

## Purpose

Specify:

```text id="t4m8qn"
CPU
Memory
Disk
```

---

## Example Flavors

```text id="x9j2wv"
small
medium
large
xlarge
```

---

## Architecture

```text id="w3c7fp"
Flavor
  │
  ▼
Instance Size
```

---

## Example

```text id="k1r5mz"
2 vCPU
4 GB RAM
40 GB Disk
```

---

# 19. Images

Images are templates used to create instances.

---

## Common Images

```text id="g7n3wc"
Ubuntu
CentOS
Debian
RHEL
Windows
```

---

## Architecture

```text id="m5v9pk"
Image
  │
  ▼
Glance
  │
  ▼
Instance
```

---

## Benefits

```text id="c4t8qx"
Fast Provisioning
Consistency
Reusable Templates
```

---

# 20. Volumes

Volumes provide persistent storage.

---

## Characteristics

```text id="j2w6nr"
Persistent
Independent
Attachable
```

---

## Architecture

```text id="u7m1cv"
Volume
  │
  ▼
Instance
```

---

## Operations

```text id="n4p9wy"
Create
Attach
Detach
Delete
Snapshot
```

---

## Benefits

```text id="f8r3kd"
Data Persistence
Flexible Storage
Backup Support
```

---

# 21. Networks

Networks connect OpenStack resources.

---

## Purpose

Provide communication.

---

## Architecture

```text id="y6v2jt"
Network
   │
   ▼
Instances
```

---

## Types

```text id="p5n8mr"
Private Network
Provider Network
External Network
```

---

## Benefits

```text id="k9c4wx"
Isolation
Connectivity
Multi-Tenant Support
```

---

# 22. Routers

Routers connect networks.

---

## Purpose

Enable communication between subnets and external networks.

---

## Architecture

```text id="r3t7vp"
Subnet A
   │
   ▼
 Router
   │
   ▼
Subnet B
```

---

## Internet Access

```text id="h6m1qx"
Private Network
      │
      ▼
Router
      │
      ▼
Internet
```

---

## Benefits

```text id="w8c5jn"
Network Connectivity
External Access
Routing Control
```

---

# 23. Floating IPs

Floating IPs provide public access.

---

## Purpose

Map public IPs to instances.

---

## Architecture

```text id="t2n9kw"
Internet
   │
   ▼
Floating IP
   │
   ▼
Instance
```

---

## Benefits

```text id="m7r4pv"
External Connectivity
Flexible Assignment
Public Access
```

---

## Common Uses

```text id="c8j1wy"
SSH
Web Servers
APIs
```

---

# 24. OpenStack Networking Architecture

Networking is managed by Neutron.

---

## Full Architecture

```text id="x5v8mn"
Internet
   │
   ▼
External Network
   │
   ▼
Router
   │
   ▼
Private Network
   │
   ▼
Instances
```

---

## Components

```text id="j3n7wp"
Network
Subnet
Router
Port
Floating IP
Security Group
```

---

## Traffic Flow

```text id="g6c2mr"
User
 │
 ▼
Floating IP
 │
 ▼
Router
 │
 ▼
Instance
```

---

# 25. High Availability

High Availability ensures workloads survive failures.

---

## Single Controller

```text id="q9m4vt"
Controller
    │
    ▼
Cloud Services
```

Risk:

```text id="v7n1kp"
Single Point of Failure
```

---

## HA Architecture

```text id="p4w8cx"
Controller 1
Controller 2
Controller 3
```

---

## Benefits

```text id="t5m2jr"
Fault Tolerance
Service Continuity
Reliability
```

---

## Best Practices

```text id="c7v6wn"
Multiple Controllers
Redundant Networking
Database Replication
```

---

# 26. Scalability

OpenStack can scale horizontally.

---

## Compute Scaling

```text id="x2n5mr"
Compute Node 1
Compute Node 2
Compute Node 3
```

---

## Storage Scaling

```text id="k4w9pt"
Storage Node 1
Storage Node 2
Storage Node 3
```

---

## Architecture

```text id="h8m3vc"
Controller
    │
    ▼
Additional Compute Nodes
```

---

## Benefits

```text id="p1r7wx"
Capacity Growth
Performance Improvement
Elastic Infrastructure
```

---

# 27. Monitoring & Logging

Monitoring provides visibility into cloud operations.

---

## What to Monitor

```text id="m6t4qn"
Compute Nodes
Storage
Networking
Instances
APIs
```

---

## Logging Sources

```text id="y3v8kp"
Nova
Neutron
Keystone
Glance
Cinder
```

---

## Architecture

```text id="d7m2wr"
Services
   │
   ▼
Logs
   │
   ▼
Monitoring Platform
```

---

## Common Tools

```text id="f5n9cx"
Prometheus
Grafana
ELK Stack
OpenSearch
```

---

## Benefits

```text id="r8w4jt"
Troubleshooting
Performance Analysis
Capacity Planning
```

---

# 28. Disaster Recovery

Disaster Recovery (DR) helps restore cloud services after major failures.

---

## DR Architecture

```text id="x7m4vp"
Primary Site
     │
     ▼
Backup / Replication
     │
     ▼
Secondary Site
```

---

## Recovery Components

```text id="p3n8wt"
Instances
Volumes
Images
Databases
Configurations
```

---

## Common Backup Targets

```text id="r5j2mc"
Cinder Volumes
Swift Storage
Glance Images
Databases
```

---

## Recovery Workflow

```text id="k8v6qn"
Backup
  │
  ▼
Store
  │
  ▼
Restore
```

---

## Best Practices

```text id="t4w1px"
Regular Backups
Test Recovery Procedures
Replicate Critical Data
Document Recovery Plans
```

---

# 29. OpenStack Security Best Practices

Security must be implemented across all layers.

---

## Identity Security

Use:

```text id="m9c5rt"
Keystone
RBAC
Strong Password Policies
MFA (if supported)
```

---

## Network Security

Use:

```text id="p7n3vx"
Security Groups
Network Segmentation
Private Networks
```

---

## Infrastructure Security

```text id="x2m8wk"
Patch Systems
Update OpenStack Services
Secure APIs
```

---

## Data Protection

```text id="v6r4jp"
Volume Encryption
TLS
Secure Backups
```

---

## Security Principles

```text id="c1t9mn"
Least Privilege
Defense in Depth
Audit Access
Monitor Activity
```

---

# 30. Troubleshooting

Troubleshooting involves identifying and resolving issues.

---

## Instance Not Starting

Check:

```text id="y5n2wr"
Nova Service
Image Availability
Compute Node Health
```

---

## Network Issues

Check:

```text id="u8p4cv"
Neutron
Router Configuration
Security Groups
Floating IPs
```

---

## Storage Issues

Check:

```text id="m3w7kt"
Cinder
Storage Backend
Volume Status
```

---

## Authentication Problems

Check:

```text id="q6n1rp"
Keystone
Tokens
Roles
Permissions
```

---

## Useful Diagnostic Commands

Services:

```bash id="w4m8cy"
openstack service list
```

---

Instances:

```bash id="r2v7tn"
openstack server list
```

---

Networks:

```bash id="x9c3mp"
openstack network list
```

---

Volumes:

```bash id="k5w1jr"
openstack volume list
```

---

# 31. Common OpenStack CLI Commands

---

## Authentication

Issue Token:

```bash id="p8v4wn"
openstack token issue
```

---

## Projects

List Projects:

```bash id="m1c7rx"
openstack project list
```

---

## Users

List Users:

```bash id="q4w9tn"
openstack user list
```

---

## Instances

List Instances:

```bash id="n7p3mv"
openstack server list
```

Create Instance:

```bash id="r8c1wk"
openstack server create
```

Delete Instance:

```bash id="v2m6qp"
openstack server delete
```

---

## Images

List Images:

```bash id="x5w8rn"
openstack image list
```

---

## Networks

List Networks:

```bash id="k3p7vt"
openstack network list
```

---

## Volumes

List Volumes:

```bash id="m9r2wc"
openstack volume list
```

---

# 32. Real-World OpenStack Architecture

---

## Private Cloud Architecture

```text id="a7v5nk"
Users
  │
  ▼
Horizon
  │
  ▼
Controller Nodes
  │
  ▼
Compute Nodes
  │
  ▼
Storage
```

---

## Enterprise Architecture

```text id="d4m8wp"
Users
  │
  ▼
Load Balancer
  │
  ▼
Controllers
  │
  ▼
Compute Cluster
  │
  ▼
Storage Cluster
```

---

## Telecom Architecture

```text id="y1p6cr"
NFV Workloads
      │
      ▼
OpenStack
      │
      ▼
Compute Infrastructure
```

---

## Multi-Tenant Architecture

```text id="v8w3mq"
Project A
Project B
Project C
     │
     ▼
OpenStack Cloud
```

---

# 33. OpenStack vs VMware

| Feature        | OpenStack   | VMware        |
| -------------- | ----------- | ------------- |
| License        | Open Source | Proprietary   |
| Cost           | Lower       | Higher        |
| Flexibility    | High        | Moderate      |
| Vendor Lock-In | Low         | Higher        |
| Community      | Large       | Vendor Driven |

---

## Architecture Comparison

```text id="c2n8wr"
OpenStack
    │
    ▼
Open Source Cloud

VMware
    │
    ▼
Commercial Virtualization
```

---

# 34. OpenStack vs AWS

| Feature        | OpenStack           | AWS                 |
| -------------- | ------------------- | ------------------- |
| Deployment     | Self Hosted         | Managed Cloud       |
| Infrastructure | User Owned          | AWS Owned           |
| Cost Model     | Capital Expense     | Operational Expense |
| Control        | Full Control        | Managed Services    |
| Scalability    | Depends on Hardware | Virtually Unlimited |

---

## Comparison

```text id="h6m4vp"
OpenStack
   │
   ▼
Private Cloud

AWS
   │
   ▼
Public Cloud
```

---

# 35. Interview Questions

---

## What is OpenStack?

OpenStack is an open-source cloud computing platform used to build private and public clouds.

---

## What is Keystone?

Keystone is the Identity service responsible for authentication and authorization.

---

## What is Nova?

Nova is the Compute service responsible for managing virtual machines.

---

## What is Neutron?

Neutron provides networking services such as networks, routers, and floating IPs.

---

## What is Glance?

Glance manages virtual machine images.

---

## Difference Between Cinder and Swift?

| Service | Type           |
| ------- | -------------- |
| Cinder  | Block Storage  |
| Swift   | Object Storage |

---

## What is a Floating IP?

A public IP address that can be dynamically assigned to an instance.

---

## What is Horizon?

The web-based dashboard for OpenStack management.

---

## Difference Between Project and User?

| Project            | User               |
| ------------------ | ------------------ |
| Resource Container | Identity           |
| Owns Resources     | Accesses Resources |

---

# 36. Additional Resources

## Official Resources

* OpenStack Documentation
* OpenStack Community
* OpenInfra Foundation
* OpenStack Administrator Guides

---

## Certifications

* Certified OpenStack Administrator (COA)
* Red Hat OpenStack Certifications
* OpenInfra Training Programs

---

## Learning Resources

* OpenStack Docs
* OpenInfra Foundation
* OpenStack Blog
* OpenStack Summit Content

---

# 37. Quick Reference

## Authentication

```bash id="q1m8vr"
openstack token issue
```

---

## Projects

```bash id="n4w7cp"
openstack project list
```

---

## Users

```bash id="k8p3mt"
openstack user list
```

---

## Instances

```bash id="d5v1rx"
openstack server list
```

---

## Images

```bash id="m7c4wn"
openstack image list
```

---

## Networks

```bash id="r2p9vk"
openstack network list
```

---

## Volumes

```bash id="t6m3wc"
openstack volume list
```

---

# 📎 Official Documentation

* OpenStack Documentation: https://docs.openstack.org
* OpenInfra Foundation: https://openinfra.org
* OpenStack Community: https://www.openstack.org
* OpenStack Administrator Guides: https://docs.openstack.org/admin-guide

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, cloud engineering, DevOps, SRE, platform engineering, and private cloud administration.

Primary references include:

* OpenStack Documentation
* OpenInfra Foundation
* OpenStack Community Resources
* Administrator Guides

OpenStack is an open-source cloud computing platform governed by the OpenInfra Foundation.

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official OpenStack documentation.

---

# 🎉 Conclusion

You now understand:

* OpenStack Fundamentals
* Keystone
* Nova
* Neutron
* Glance
* Cinder
* Swift
* Manila
* Projects & Roles
* Networking
* Volumes
* Security Groups
* Availability Zones
* High Availability
* Scalability
* Disaster Recovery
* Security Best Practices
* Monitoring & Logging
* Troubleshooting

OpenStack provides:

```text id="f4n8wy"
Compute
+
Networking
+
Storage
+
Identity
+
Images
+
Multi-Tenancy
+
Private Cloud Infrastructure
```

through a fully open-source cloud platform suitable for enterprise, telecom, research, and private cloud environments.

---

```md
**Last Updated:** 2026
**Platform:** OpenStack
**Version:** OpenStack 2026.x+
**License:** Apache License 2.0

For latest information, visit https://docs.openstack.org
```