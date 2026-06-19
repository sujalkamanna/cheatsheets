# Kubernetes on AWS (Amazon EKS) Cheatsheet

---

![Kubernetes Logo](https://upload.wikimedia.org/wikipedia/commons/3/39/Kubernetes_logo_without_workmark.svg)

---

**Last Updated:** 2026
**Platform:** Kubernetes on AWS (Amazon EKS)
**Version:** Kubernetes v1.33+ / Amazon EKS Latest Supported Release
**License:** Kubernetes (Apache License 2.0), AWS Services (Proprietary)

For latest information, visit:
https://docs.aws.amazon.com/eks

---

## Table of Contents

* [1. What is Kubernetes on AWS?](#1-what-is-kubernetes-on-aws)
* [2. Kubernetes Fundamentals](#2-kubernetes-fundamentals)
* [3. Why Run Kubernetes on AWS?](#3-why-run-kubernetes-on-aws)
* [4. Amazon EKS Overview](#4-amazon-eks-overview)
* [5. EKS Architecture](#5-eks-architecture)
* [6. Control Plane vs Worker Nodes](#6-control-plane-vs-worker-nodes)
* [7. kubectl Fundamentals](#7-kubectl-fundamentals)
* [8. EKS Cluster Creation Concepts](#8-eks-cluster-creation-concepts)
* [9. IAM for EKS](#9-iam-for-eks)
* [10. High Availability Concepts](#10-high-availability-concepts)
* [11. Scalability Concepts](#11-scalability-concepts)
  
* [Additional Resources](#additional-resources)
* [Quick Reference](#quick-reference)
* [📎 Official Documentation](#-official-documentation)
* [📄 Copyright](#-copyright)
* [📎 Disclaimer & Attribution](#-disclaimer--attribution)
* [🎉 Conclusion](#-conclusion) 

---

# 1. What is Kubernetes on AWS?

Kubernetes on AWS refers to running Kubernetes workloads using AWS infrastructure.

The most common service is:

```text
Amazon Elastic Kubernetes Service (EKS)
```

which provides a managed Kubernetes control plane.

---

## Purpose

Run:

```text
Containers
Microservices
Cloud Native Applications
DevOps Platforms
AI/ML Workloads
```

on AWS infrastructure.

---

## Traditional Deployment

```text
Application
     │
     ▼
Virtual Machine
     │
     ▼
Operating System
```

---

## Kubernetes Deployment

```text
Application
     │
     ▼
Container
     │
     ▼
Kubernetes
     │
     ▼
AWS Infrastructure
```

---

## Benefits

```text
Container Orchestration
High Availability
Auto Scaling
Self Healing
Cloud Native Architecture
```

---

# 2. Kubernetes Fundamentals

Kubernetes is a container orchestration platform.

---

## Core Components

```text
Cluster
Node
Pod
Deployment
Service
Ingress
```

---

## Cluster Architecture

```text
Kubernetes Cluster
        │
 ┌──────┴──────┐
 ▼             ▼
Control Plane  Worker Nodes
```

---

## Pod

Smallest deployable Kubernetes object.

```text
Pod
 │
 ├── Container
 └── Container
```

---

## Node

A machine running Kubernetes workloads.

```text
Node
 │
 ├── Pods
 ├── kubelet
 └── Container Runtime
```

---

## Deployment

Manages Pods.

```text
Deployment
     │
     ▼
ReplicaSet
     │
     ▼
Pods
```

---

# 3. Why Run Kubernetes on AWS?

AWS provides enterprise-grade infrastructure.

---

## AWS Advantages

```text
Global Infrastructure
Managed Services
Security
Monitoring
Scalability
```

---

## Kubernetes + AWS

```text
Kubernetes
      +
AWS Infrastructure
      =
Cloud Native Platform
```

---

## Common Workloads

```text
Web Applications
Microservices
DevOps Platforms
Data Processing
Machine Learning
```

---

## Typical Architecture

```text
Users
  │
  ▼
Load Balancer
  │
  ▼
EKS Cluster
  │
  ▼
Pods
```

---

# 4. Amazon EKS Overview

Amazon EKS is AWS's managed Kubernetes service.

---

## What AWS Manages

```text
Control Plane
API Server
etcd
Availability
Upgrades
```

---

## What You Manage

```text
Applications
Pods
Services
Worker Nodes
Security
```

---

## EKS Architecture

```text
AWS
 │
 ▼
Managed Control Plane
 │
 ▼
Worker Nodes
 │
 ▼
Pods
```

---

## Benefits

```text
Managed Kubernetes
AWS Integration
Security
Reliability
```

---

# 5. EKS Architecture

An EKS cluster consists of multiple components.

---

## Architecture Diagram

```text
Users
  │
  ▼
Application Load Balancer
  │
  ▼
EKS Cluster
  │
  ├── Control Plane
  │
  └── Worker Nodes
           │
           ▼
          Pods
```

---

## Components

### Control Plane

```text
API Server
Scheduler
Controller Manager
etcd
```

---

### Data Plane

```text
Worker Nodes
Pods
Services
```

---

## AWS Services Used

```text
EKS
EC2
VPC
IAM
EBS
EFS
CloudWatch
```

---

# 6. Control Plane vs Worker Nodes

Understanding this distinction is critical.

---

## Control Plane

Responsible for cluster management.

```text
API Requests
Scheduling
Cluster State
Health Monitoring
```

---

## Worker Nodes

Responsible for application execution.

```text
Containers
Pods
Services
Applications
```

---

## Comparison

| Control Plane  | Worker Nodes      |
| -------------- | ----------------- |
| Managed by AWS | Managed by User   |
| Scheduling     | Runs Workloads    |
| Cluster State  | Application State |
| API Server     | Pods              |

---

## Architecture

```text
Control Plane
      │
      ▼
Schedules Pods
      │
      ▼
Worker Nodes
```

---

# 7. kubectl Fundamentals

kubectl is the Kubernetes command-line tool.

---

## Verify Installation

```bash
kubectl version --client
```

---

## View Cluster Info

```bash
kubectl cluster-info
```

---

## View Nodes

```bash
kubectl get nodes
```

---

## View Pods

```bash
kubectl get pods
```

---

## View Services

```bash
kubectl get svc
```

---

## View All Resources

```bash
kubectl get all
```

---

## Describe Pod

```bash
kubectl describe pod POD_NAME
```

---

## Delete Pod

```bash
kubectl delete pod POD_NAME
```

---

# 8. EKS Cluster Creation Concepts

Before creating a cluster, several AWS resources are required.

---

## Required Components

```text
VPC
Subnets
IAM Roles
Security Groups
EKS Cluster
Worker Nodes
```

---

## Creation Flow

```text
Create VPC
      │
      ▼
Create IAM Roles
      │
      ▼
Create EKS Cluster
      │
      ▼
Create Node Group
      │
      ▼
Deploy Applications
```

---

## Networking Requirement

At least:

```text
2 Availability Zones
```

Recommended:

```text
3 Availability Zones
```

for production workloads.

---

## Example Architecture

```text
Region
 │
 ├── AZ-1
 ├── AZ-2
 └── AZ-3
```

---

# 9. IAM for EKS

IAM controls permissions.

---

## Core Components

```text
Users
Groups
Roles
Policies
```

---

## Common Roles

### Cluster Role

```text
Manage EKS Cluster
```

---

### Node Role

```text
Allow Worker Nodes
to join cluster
```

---

### Service Account Role

```text
Pod-Level AWS Access
```

---

## IAM Architecture

```text
IAM
 │
 ▼
Roles
 │
 ▼
EKS Resources
```

---

## Best Practices

```text
Least Privilege
Use IAM Roles
Avoid Static Credentials
Use IRSA
```

---

# 10. High Availability Concepts

Production clusters must survive failures.

---

## Single AZ

```text
Users
 │
 ▼
EKS
 │
 ▼
AZ-1
```

Risk:

```text
AZ Failure
=
Downtime
```

---

## Multi-AZ

```text
EKS Cluster
    │
 ┌──┼──┐
 ▼  ▼  ▼
AZ1 AZ2 AZ3
```

---

## Benefits

```text
Fault Tolerance
Availability
Business Continuity
```

---

## Best Practices

```text
Use Multiple AZs
Use Multiple Nodes
Use Load Balancers
Use Replicas
```

---

# 11. Scalability Concepts

Kubernetes automatically scales workloads.

---

## Horizontal Pod Scaling

```text
Pod1
Pod2
Pod3
Pod4
```

---

## Cluster Scaling

```text
Node1
Node2
Node3
Node4
```

---

## Scaling Layers

```text
Application
      │
      ▼
Pods
      │
      ▼
Nodes
```

---

## Kubernetes Scaling Tools

```text
Horizontal Pod Autoscaler
Vertical Pod Autoscaler
Cluster Autoscaler
Karpenter
```

---

## Example Scaling Flow

```text
Traffic ↑
   │
   ▼
More Pods
   │
   ▼
More Nodes
```

---

# 12. Worker Nodes

Worker Nodes run Kubernetes workloads.

---

## Purpose

Worker nodes execute:

```text
Pods
Containers
Applications
Services
```

---

## Architecture

```text
Control Plane
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
kubelet
Container Runtime
kube-proxy
Pods
```

---

## Responsibilities

```text
Run Pods
Report Status
Pull Images
Execute Containers
```

---

# 13. Managed Node Groups

Managed Node Groups are AWS-managed worker nodes.

---

## Benefits

```text
Automatic Updates
Auto Scaling
Simplified Management
AWS Integration
```

---

## Architecture

```text
EKS
 │
 ▼
Managed Node Group
 │
 ├── Node 1
 ├── Node 2
 └── Node 3
```

---

## Common Use Cases

```text
Production Clusters
Development Environments
Microservices
```

---

## Best Practices

```text
Multiple AZs
Use Auto Scaling
Use Latest AMIs
Monitor Node Health
```

---

# 14. Self-Managed Nodes

Self-managed nodes are fully controlled by the user.

---

## Advantages

```text
Full Customization
Custom AMIs
Special Configurations
```

---

## Disadvantages

```text
Manual Updates
Higher Maintenance
More Operational Overhead
```

---

## Architecture

```text
EKS Cluster
     │
     ▼
EC2 Instances
     │
     ▼
Worker Nodes
```

---

## When to Use

```text
Custom Operating Systems
Special Security Requirements
Legacy Environments
```

---

# 15. AWS Fargate for EKS

AWS Fargate provides serverless Kubernetes compute.

---

## Concept

Instead of managing nodes:

```text
Pods
 │
 ▼
Fargate
 │
 ▼
AWS Managed Infrastructure
```

---

## Benefits

```text
No Node Management
Serverless
Automatic Scaling
Pay Per Usage
```

---

## Use Cases

```text
Microservices
Batch Jobs
APIs
Event-Driven Workloads
```

---

## Limitations

```text
Less Node Control
Higher Cost
Some Kubernetes Restrictions
```

---

# 16. Pods

Pods are the smallest deployable unit in Kubernetes.

---

## Pod Structure

```text
Pod
 │
 ├── Container A
 ├── Container B
 └── Shared Network
```

---

## Characteristics

```text
Ephemeral
Unique IP Address
Shared Storage
```

---

## Create Pod

```bash
kubectl run nginx \
--image=nginx
```

---

## List Pods

```bash
kubectl get pods
```

---

## Pod Lifecycle

```text
Pending
   │
   ▼
Running
   │
   ▼
Succeeded/Failed
```

---

# 17. Deployments

Deployments manage Pods.

---

## Purpose

```text
Scaling
Updates
Rollbacks
Self-Healing
```

---

## Architecture

```text
Deployment
     │
     ▼
ReplicaSet
     │
     ▼
Pods
```

---

## Create Deployment

```bash
kubectl create deployment nginx \
--image=nginx
```

---

## Scale Deployment

```bash
kubectl scale deployment nginx \
--replicas=5
```

---

## Benefits

```text
High Availability
Rolling Updates
Automation
```

---

# 18. ReplicaSets

ReplicaSets maintain desired Pod count.

---

## Example

Desired:

```text
3 Pods
```

Current:

```text
2 Pods
```

ReplicaSet creates:

```text
1 Additional Pod
```

---

## Architecture

```text
ReplicaSet
     │
     ▼
Desired State
     │
     ▼
Running Pods
```

---

## Benefits

```text
Self Healing
Availability
Automation
```

---

# 19. Services

Services expose applications.

---

## Purpose

Pods are temporary.

Services provide:

```text
Stable Networking
Service Discovery
Load Balancing
```

---

## Architecture

```text
Service
   │
   ▼
Pods
```

---

## Service Types

### ClusterIP

```text
Internal Access
```

---

### NodePort

```text
External Access
```

---

### LoadBalancer

```text
AWS Load Balancer
```

---

## View Services

```bash
kubectl get svc
```

---

# 20. ConfigMaps

ConfigMaps store configuration data.

---

## Purpose

Separate configuration from application code.

---

## Example

```text
Database Host
Application Port
Environment Variables
```

---

## Architecture

```text
ConfigMap
     │
     ▼
Pod
```

---

## Benefits

```text
Centralized Configuration
Easy Updates
Environment Separation
```

---

# 21. Secrets

Secrets store sensitive information.

---

## Examples

```text
Passwords
API Keys
Tokens
Certificates
```

---

## Architecture

```text
 Secret
   │
   ▼
  Pod
```

---

## Create Secret

```bash
kubectl create secret generic db-secret
```

---

## Best Practices

```text
Encrypt Secrets
Use IAM Integration
Avoid Hardcoding
```

---

# 22. Persistent Volumes (PV)

Persistent Volumes provide storage.

---

## Purpose

Data survives Pod restarts.

---

## Architecture

```text
Persistent Volume
        │
        ▼
Application Data
```

---

## Common Backends

```text
EBS
EFS
FSx
```

---

## Benefits

```text
Durability
Persistence
Storage Abstraction
```

---

# 23. Persistent Volume Claims (PVC)

PVCs request storage from Persistent Volumes.

---

## Architecture

```text
Pod
 │
 ▼
PVC
 │
 ▼
PV
```

---

## Purpose

```text
Abstract Storage
Simplify Management
```

---

## Benefits

```text
Portability
Automation
Storage Flexibility
```

---

# 24. Storage Classes

Storage Classes automate volume provisioning.

---

## Architecture

```text
Storage Class
      │
      ▼
Persistent Volume
      │
      ▼
Pod
```

---

## Common AWS Storage Classes

```text
gp3
gp2
io1
io2
```

---

## Benefits

```text
Dynamic Provisioning
Automation
Performance Selection
```

---

# 25. Amazon EBS CSI Driver

Provides EBS integration with Kubernetes.

---

## Purpose

```text
Dynamic Volume Provisioning
Persistent Storage
```

---

## Architecture

```text
Pod
 │
 ▼
PVC
 │
 ▼
EBS CSI Driver
 │
 ▼
Amazon EBS
```

---

## Benefits

```text
Automation
AWS Integration
Persistent Storage
```

---

# 26. Amazon EFS CSI Driver

Provides EFS integration with Kubernetes.

---

## Characteristics

```text
Shared Storage
Multi-Node Access
Elastic Capacity
```

---

## Architecture

```text
Pods
  │
  ▼
EFS CSI Driver
  │
  ▼
Amazon EFS
```

---

## Common Use Cases

```text
Shared Content
CMS Platforms
Analytics
```

---

# 27. Stateful Applications

Stateful applications require persistent storage.

---

## Examples

```text
MySQL
PostgreSQL
MongoDB
Redis
```

---

## Architecture

```text
StatefulSet
      │
      ▼
Persistent Volumes
      │
      ▼
Database Pods
```

---

## Kubernetes Objects

```text
StatefulSets
PVCs
Storage Classes
Services
```

---

## Best Practices

```text
Use Persistent Storage
Enable Backups
Use Multi-AZ Storage
Monitor Capacity
```

---

# 28. Kubernetes Networking

Networking allows communication between Pods, Services, and external users.

---

## Kubernetes Networking Goals

```text id="y8j2kn"
Pod-to-Pod Communication
Pod-to-Service Communication
External Access
Service Discovery
```

---

## Network Architecture

```text id="e6r3vu"
Users
  │
  ▼
Ingress
  │
  ▼
Service
  │
  ▼
Pods
```

---

## Requirements

```text id="t0x7mj"
Every Pod Gets an IP
No NAT Between Pods
Flat Network Model
```

---

## Benefits

```text id="b2w4as"
Scalability
Service Discovery
Cloud Native Design
```

---

# 29. Amazon VPC CNI

Amazon VPC CNI provides networking for EKS Pods.

---

## Purpose

Assign AWS VPC IP addresses directly to Pods.

---

## Architecture

```text id="w4j7ne"
VPC
 │
 ▼
Worker Node
 │
 ▼
Pod IPs
```

---

## Advantages

```text id="r6f8hd"
Native AWS Networking
High Performance
Direct VPC Integration
```

---

## Components

```text id="m9q2tv"
aws-node
ENI
Pod IP Allocation
```

---

## Benefits

```text id="c8x4yb"
Security Group Integration
VPC Visibility
Low Latency
```

---

# 30. Ingress

Ingress manages external HTTP and HTTPS access.

---

## Purpose

Provide centralized routing.

---

## Architecture

```text id="g5n9aw"
Users
  │
  ▼
Ingress
  │
  ▼
Services
  │
  ▼
Pods
```

---

## Features

```text id="h2u4eg"
Path Routing
Host Routing
TLS Termination
Load Balancing
```

---

## Example

```text id="u6w7pk"
app.example.com
        │
        ▼
      Service A

api.example.com
        │
        ▼
      Service B
```

---

# 31. AWS Load Balancer Controller

Integrates Kubernetes with AWS Load Balancers.

---

## Purpose

Automatically creates:

```text id="d3m6rt"
Application Load Balancers
Network Load Balancers
```

---

## Architecture

```text id="s8k4yv"
Ingress
   │
   ▼
AWS Load Balancer Controller
   │
   ▼
Application Load Balancer
```

---

## Benefits

```text id="f4r7qn"
Automation
AWS Integration
Scalability
```

---

## Common Use Cases

```text id="v9n2wd"
Web Applications
Microservices
APIs
```

---

# 32. CoreDNS

CoreDNS provides DNS inside Kubernetes.

---

## Purpose

Service discovery.

---

## Architecture

```text id="k3j8mx"
Pod
 │
 ▼
CoreDNS
 │
 ▼
Service IP
```

---

## Example

```text id="w7t4cp"
mysql.default.svc.cluster.local
```

---

## Benefits

```text id="m2f9da"
Service Discovery
Automation
Reliable Networking
```

---

# 33. Network Policies

Network Policies control Pod traffic.

---

## Purpose

Restrict communication.

---

## Architecture

```text id="z5p1hb"
Pod A
  │
  ▼
Network Policy
  │
  ▼
Pod B
```

---

## Controls

```text id="q6r8vk"
Ingress Traffic
Egress Traffic
Allowed Sources
Allowed Destinations
```

---

## Benefits

```text id="p7w2ct"
Zero Trust Networking
Security
Compliance
```

---

# 34. Security Groups for Pods

Allows AWS Security Groups to be attached to Pods.

---

## Traditional Model

```text id="c4u7je"
Security Group
      │
      ▼
Worker Node
```

---

## Pod Security Model

```text id="g8n3fd"
Security Group
      │
      ▼
Pod
```

---

## Benefits

```text id="s2v9xa"
Fine-Grained Security
AWS Integration
Isolation
```

---

## Common Use Cases

```text id="m5k8pr"
Databases
Sensitive Applications
Multi-Tenant Clusters
```

---

# 35. IAM Roles for Service Accounts (IRSA)

IRSA provides AWS permissions to Pods.

---

## Problem

Avoid storing:

```text id="r4t8qh"
Access Keys
Secret Keys
```

inside containers.

---

## Architecture

```text id="w9m2zk"
Pod
 │
 ▼
Service Account
 │
 ▼
IAM Role
 │
 ▼
AWS Services
```

---

## Benefits

```text id="k7x4cv"
Least Privilege
Security
No Static Credentials
```

---

## Common Use Cases

```text id="n3d8yb"
S3 Access
DynamoDB Access
Secrets Manager
CloudWatch
```

---

# 36. Cluster Autoscaler

Automatically adjusts node count.

---

## Purpose

Scale nodes based on Pod demand.

---

## Architecture

```text id="a4j9mp"
Pending Pods
      │
      ▼
Cluster Autoscaler
      │
      ▼
More Nodes
```

---

## Scale Down

```text id="d8w5ny"
Unused Nodes
      │
      ▼
Removed
```

---

## Benefits

```text id="f3r6te"
Cost Optimization
Automation
Scalability
```

---

# 37. Karpenter

Modern Kubernetes node provisioning solution.

---

## Purpose

Provision nodes dynamically.

---

## Architecture

```text id="m7c2vk"
Pods
 │
 ▼
Karpenter
 │
 ▼
EC2 Instances
```

---

## Benefits

```text id="p5w8rq"
Faster Scaling
Better Bin Packing
Cost Reduction
```

---

## Compared to Cluster Autoscaler

```text id="u1k6yd"
Cluster Autoscaler
    │
    ▼
Scale Node Groups

Karpenter
    │
    ▼
Create Nodes Directly
```

---

# 38. Monitoring

Monitoring provides visibility into cluster health.

---

## What to Monitor

```text id="x4n9eh"
Nodes
Pods
CPU
Memory
Storage
Networking
```

---

## Architecture

```text id="a8r2cw"
Cluster
   │
   ▼
Monitoring Stack
   │
   ▼
Dashboards
```

---

## Benefits

```text id="j2m7tf"
Observability
Troubleshooting
Performance Analysis
```

---

# 39. Logging

Logs provide application and cluster visibility.

---

## Log Sources

```text id="w6p4zs"
Applications
Containers
Nodes
System Components
```

---

## Architecture

```text id="m1q8vx"
Containers
    │
    ▼
Logging Agent
    │
    ▼
Log Storage
```

---

## Benefits

```text id="t4j6pb"
Debugging
Auditing
Monitoring
```

---

# 40. Prometheus

Prometheus is the standard Kubernetes monitoring solution.

---

## Purpose

Collect metrics.

---

## Architecture

```text id="y7n3da"
Applications
     │
     ▼
Prometheus
     │
     ▼
Metrics Database
```

---

## Common Metrics

```text id="r8f4ku"
CPU Usage
Memory Usage
Request Rates
Latency
```

---

## Benefits

```text id="g5w2qp"
Powerful Querying
Alerting
Cloud Native Integration
```

---

# 41. Grafana

Grafana visualizes monitoring data.

---

## Architecture

```text id="b4r9xy"
Prometheus
     │
     ▼
Grafana
     │
     ▼
Dashboards
```

---

## Features

```text id="f8n6kj"
Dashboards
Alerts
Visualizations
Reports
```

---

## Benefits

```text id="m9w1td"
Operational Visibility
Business Metrics
Performance Analysis
```

---

# 42. Fluent Bit

Fluent Bit collects and forwards logs.

---

## Architecture

```text id="k2v8rh"
Containers
     │
     ▼
Fluent Bit
     │
     ▼
CloudWatch
```

---

## Features

```text id="n6j4cx"
Log Collection
Log Filtering
Log Routing
```

---

## Benefits

```text id="u5f7qb"
Lightweight
Efficient
Cloud Native
```

---

# 43. CloudWatch Integration

CloudWatch integrates AWS monitoring with EKS.

---

## Monitors

```text id="p4n2zk"
Nodes
Pods
Applications
AWS Resources
```

---

## Architecture

```text id="w8k3mc"
EKS Cluster
     │
     ▼
CloudWatch
     │
     ▼
Dashboards & Alerts
```

---

## Benefits

```text id="x7r9vd"
AWS Visibility
Centralized Monitoring
Alerting
```

---

# 44. GitOps Concepts

GitOps uses Git as the source of truth.

---

## Workflow

```text id="m3f8tk"
Git Repository
      │
      ▼
GitOps Tool
      │
      ▼
Kubernetes Cluster
```

---

## Principles

```text id="d6w4pa"
Declarative Configuration
Version Control
Automation
```

---

## Benefits

```text id="j9n2qe"
Consistency
Auditability
Rollback Capability
```

---

# 45. ArgoCD on EKS

ArgoCD implements GitOps for Kubernetes.

---

## Architecture

```text id="t7m5kr"
Git Repository
      │
      ▼
ArgoCD
      │
      ▼
EKS Cluster
```

---

## Features

```text id="v4r8yx"
Continuous Delivery
Synchronization
Drift Detection
Rollbacks
```

---

## Workflow

```text id="c8n1wb"
Developer
     │
     ▼
Git Push
     │
     ▼
ArgoCD Sync
     │
     ▼
EKS Deployment
```

---

## Benefits

```text id="f6j9pm"
Automation
Reliability
Git-Based Operations
```

---

Then continue with:

---

# 46. High Availability Design

High Availability ensures applications remain available during failures.

---

## Multi-AZ Architecture

```text
Users
  │
  ▼
Load Balancer
  │
  ▼
EKS Cluster
  │
 ┌┼┐
 ▼▼▼
AZ1 AZ2 AZ3
```

---

## Benefits

```text
Fault Tolerance
Automatic Recovery
Business Continuity
```

---

## Best Practices

```text
Multiple Availability Zones
Multiple Worker Nodes
Pod Replicas
Load Balancers
Managed Node Groups
```

---

# 47. Scalability Patterns

Kubernetes scales applications and infrastructure independently.

---

## Pod Scaling

```text
Traffic ↑
   │
   ▼
More Pods
```

Tools:

```text
HPA
VPA
```

---

## Node Scaling

```text
Traffic ↑
   │
   ▼
More Nodes
```

Tools:

```text
Cluster Autoscaler
Karpenter
```

---

## Scaling Layers

```text
Users
  │
  ▼
Pods
  │
  ▼
Nodes
  │
  ▼
AWS Infrastructure
```

---

# 48. Disaster Recovery

Disaster Recovery protects against major failures.

---

## Backup Architecture

```text
EKS Cluster
     │
     ▼
Velero
     │
     ▼
S3 Bucket
```

---

## Recovery Components

```text
Cluster Resources
Persistent Volumes
Secrets
Configurations
```

---

## Common Tools

```text
Velero
AWS Backup
EBS Snapshots
EFS Backups
```

---

## Multi-Region DR

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

# 49. Cost Optimization

---

## Common Cost Drivers

```text
Worker Nodes
Load Balancers
Storage
Data Transfer
CloudWatch
```

---

## Optimization Techniques

```text
Use Spot Instances
Right Size Nodes
Enable Autoscaling
Delete Unused Resources
```

---

## Spot Node Architecture

```text
Managed Node Group
        │
   ┌────┴────┐
   ▼         ▼
On-Demand  Spot
```

---

## Karpenter Benefits

```text
Faster Provisioning
Better Utilization
Lower Costs
```

---

# 50. Real-World EKS Architectures

## Microservices Platform

```text
Users
  │
  ▼
ALB
  │
  ▼
Ingress
  │
  ▼
Services
  │
  ▼
Pods
```

---

## GitOps Platform

```text
Git
 │
 ▼
ArgoCD
 │
 ▼
EKS
```

---

## DevOps Platform

```text
Developer
    │
    ▼
GitHub
    │
    ▼
Jenkins
    │
    ▼
EKS
```

---

## Observability Platform

```text
Applications
      │
      ▼
Prometheus
      │
      ▼
Grafana
```

---

# 51. Security Best Practices

---

## Identity Security

Use:

```text
IAM
IRSA
MFA
Least Privilege
```

---

## Network Security

Use:

```text
Security Groups
Network Policies
Private Subnets
```

---

## Secrets Security

Use:

```text
AWS Secrets Manager
Kubernetes Secrets
KMS Encryption
```

---

## Container Security

```text
Image Scanning
Signed Images
Minimal Base Images
```

---

## Cluster Security

```text
RBAC
Pod Security Standards
Audit Logging
```

---

# 52. Troubleshooting

---

## Pods Not Starting

Check:

```bash
kubectl get pods

kubectl describe pod POD_NAME
```

---

## Node Issues

```bash
kubectl get nodes
```

---

Check:

```text
Node Health
Resources
Networking
```

---

## Service Not Reachable

Verify:

```bash
kubectl get svc
```

---

Check:

```text
Ingress
Security Groups
ALB
DNS
```

---

## Storage Issues

Verify:

```text
PVC
PV
Storage Class
CSI Drivers
```

---

## Useful Commands

View Nodes:

```bash
kubectl get nodes
```

---

View Pods:

```bash
kubectl get pods -A
```

---

View Services:

```bash
kubectl get svc -A
```

---

View Deployments:

```bash
kubectl get deployments -A
```

---

# 53. Interview Questions

---

## What is Amazon EKS?

Amazon EKS is AWS's managed Kubernetes service.

---

## Difference Between Pod and Deployment?

| Pod            | Deployment   |
| -------------- | ------------ |
| Runs Container | Manages Pods |
| Temporary      | Declarative  |

---

## What is IRSA?

IAM Roles for Service Accounts provide AWS permissions to Pods without storing credentials.

---

## What is Karpenter?

Dynamic node provisioning solution for Kubernetes on AWS.

---

## Difference Between HPA and Cluster Autoscaler?

| HPA               | Cluster Autoscaler   |
| ----------------- | -------------------- |
| Scales Pods       | Scales Nodes         |
| Application Layer | Infrastructure Layer |

---

## Why Use Managed Node Groups?

```text
Automatic Updates
Simplified Operations
AWS Integration
```

---

# 54. Additional Resources

## Official Resources

* Kubernetes Documentation
* Amazon EKS Documentation
* AWS Architecture Center
* CNCF Documentation

---

## Certifications

* Certified Kubernetes Administrator (CKA)
* Certified Kubernetes Application Developer (CKAD)
* AWS Certified Developer
* AWS Certified Solutions Architect

---

## Learning Resources

* Kubernetes.io
* AWS Workshops
* CNCF Landscape
* Kubernetes Blog

---

# 55. Quick Reference

## Cluster

```bash
kubectl cluster-info
```

---

## Nodes

```bash
kubectl get nodes
```

---

## Pods

```bash
kubectl get pods -A

kubectl describe pod POD_NAME
```

---

## Deployments

```bash
kubectl get deployments

kubectl scale deployment APP --replicas=5
```

---

## Services

```bash
kubectl get svc
```

---

## Logs

```bash
kubectl logs POD_NAME
```

---

# 📎 Official Documentation

* Kubernetes: [https://kubernetes.io/docs](https://kubernetes.io/docs)
* Amazon EKS: [https://docs.aws.amazon.com/eks](https://docs.aws.amazon.com/eks)
* AWS Architecture Center: [https://aws.amazon.com/architecture](https://aws.amazon.com/architecture)
* CNCF: [https://www.cncf.io](https://www.cncf.io)

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, Kubernetes administration, DevOps, SRE, and cloud-native engineering.

Primary references include:

* Kubernetes Documentation
* Amazon EKS Documentation
* CNCF Documentation
* AWS Architecture Center

Kubernetes is a trademark of The Linux Foundation. AWS, Amazon EKS, EC2, EBS, EFS, CloudWatch and related services are trademarks of Amazon Web Services, Inc.

All trademarks belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

You now understand:

* Kubernetes Fundamentals
* Amazon EKS
* Worker Nodes
* Managed Node Groups
* Fargate
* Pods
* Deployments
* Services
* Storage
* Networking
* Security
* Monitoring
* GitOps
* ArgoCD
* High Availability
* Scalability
* Disaster Recovery
* Cost Optimization
* Real-World Architectures

Kubernetes on AWS combines:

```text
Kubernetes
+
AWS Infrastructure
+
Managed Services
+
Security
+
Observability
+
Scalability
+
Cloud Native Architecture
```

to provide a production-ready platform for modern applications.

---

**Last Updated:** 2026
**Platform:** Kubernetes on AWS (Amazon EKS)
**Version:** Kubernetes v1.33+ / Amazon EKS Latest Supported Release
**License:** Kubernetes (Apache License 2.0), AWS Services (Proprietary)

For latest information, visit https://docs.aws.amazon.com/eks