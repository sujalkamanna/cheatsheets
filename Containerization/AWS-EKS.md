# AWS EKS Cheatsheet

![AWS EKS Logo](https://d1.awsstatic.com/product-marketing/EKS/Amazon-EKS-product-icon.5f8c0a6d6a2dff7e94ece66d3bb5dc408dc3254d.png)

---

## Table of Contents

* [1. What is AWS EKS?](#1-what-is-aws-eks)
* [2. Why AWS EKS?](#2-why-aws-eks)
* [3. EKS Architecture](#3-eks-architecture)
* [4. Core Components](#4-core-components)
* [5. Control Plane](#5-control-plane)
* [6. Worker Nodes](#6-worker-nodes)
* [7. Node Groups](#7-node-groups)
* [8. Managed Node Groups](#8-managed-node-groups)
* [9. Self-Managed Node Groups](#9-self-managed-node-groups)
* [10. Fargate Profiles](#10-fargate-profiles)
* [11. EKS Cluster Creation](#11-eks-cluster-creation)
* [12. kubectl Integration](#12-kubectl-integration)
* [13. eksctl Fundamentals](#13-eksctl-fundamentals)
* [14. Kubernetes Objects in EKS](#14-kubernetes-objects-in-eks)
* [15. EKS Networking Fundamentals](#15-eks-networking-fundamentals)

* [Additional Resources](#additional-resources)
* [Quick Reference](#quick-reference)
* [📎 Official Documentation](#-official-documentation)
* [📄 Copyright](#-copyright)
* [📎 Disclaimer & Attribution](#-disclaimer--attribution)
* [🎉 Conclusion](#-conclusion) 

---

# 1. What is AWS EKS?

Amazon EKS (Elastic Kubernetes Service) is a fully managed Kubernetes service provided by AWS.

It allows organizations to run Kubernetes without managing the Kubernetes control plane infrastructure.

---

## Purpose

```text id="eks1a1"
Managed Kubernetes
Container Orchestration
Cloud Native Applications
Microservices
Platform Engineering
```

---

## High-Level Architecture

```text id="eks1a2"
Amazon EKS
      │
      ▼
Control Plane
      │
 ┌────┼────┐
 ▼    ▼    ▼
Node Node Node
      │
      ▼
Pods
```

---

## Common Use Cases

```text id="eks1a3"
Microservices
Platform Engineering
Cloud Native Applications
DevOps Platforms
Enterprise Kubernetes
```

---

## Benefits

```text id="eks1a4"
Managed Control Plane
High Availability
Scalability
Security
AWS Integration
```

---

# 2. Why AWS EKS?

Running Kubernetes manually requires managing control plane components, upgrades, security, and availability.

EKS removes much of this operational burden.

---

## Traditional Kubernetes

```text id="eks1a5"
Control Plane
      │
      ▼
Management
Upgrades
Security
Monitoring
```

---

## EKS Approach

```text id="eks1a6"
Amazon EKS
      │
      ▼
Managed Control Plane
      │
      ▼
Kubernetes Workloads
```

---

## Advantages

```text id="eks1a7"
Managed Kubernetes
AWS Native
High Availability
Reduced Operations
```

---

## Common Integrations

```text id="eks1a8"
ECR
CloudWatch
IAM
VPC
Route53
```

---

# 3. EKS Architecture

EKS consists of AWS-managed and customer-managed components.

---

## Architecture

```text id="eks1a9"
AWS Managed
     │
     ▼
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

```text id="eks1a10"
Control Plane
Worker Nodes
Node Groups
Pods
Services
```

---

## Workflow

```text id="eks1a11"
Deploy Application
        │
        ▼
Kubernetes Objects
        │
        ▼
Pods
```

---

## Benefits

```text id="eks1a12"
Automation
Reliability
Scalability
```

---

# 4. Core Components

EKS includes several Kubernetes and AWS components.

---

## Main Components

```text id="eks1a13"
Control Plane
Node Groups
Pods
Services
Ingress
```

---

## Supporting AWS Services

```text id="eks1a14"
IAM
ECR
CloudWatch
VPC
Route53
```

---

## Architecture

```text id="eks1a15"
Amazon EKS
     │
 ┌───┼───┐
 ▼   ▼   ▼
Nodes Pods Services
```

---

## Benefits

```text id="eks1a16"
Integrated Platform
Managed Operations
Cloud Native
```

---

# 5. Control Plane

The Control Plane manages the Kubernetes cluster.

AWS manages it automatically.

---

## Components

```text id="eks1a17"
API Server
Scheduler
Controller Manager
etcd
```

---

## Architecture

```text id="eks1a18"
Control Plane
      │
 ┌────┼────┐
 ▼    ▼    ▼
API Scheduler etcd
```

---

## Responsibilities

```text id="eks1a19"
Scheduling
Cluster State
API Requests
Automation
```

---

## Benefits

```text id="eks1a20"
Managed Service
High Availability
Automatic Updates
```

---

# 6. Worker Nodes

Worker nodes run Kubernetes workloads.

---

## Architecture

```text id="eks1a21"
Worker Node
      │
      ▼
Kubelet
      │
      ▼
Pods
```

---

## Responsibilities

```text id="eks1a22"
Run Pods
Provide Resources
Communicate with Control Plane
```

---

## Common Types

```text id="eks1a23"
EC2 Nodes
Fargate Nodes
```

---

## Benefits

```text id="eks1a24"
Scalability
Workload Execution
Flexibility
```

---

# 7. Node Groups

Node Groups manage collections of worker nodes.

---

## Architecture

```text id="eks1a25"
Node Group
     │
 ┌───┼───┐
 ▼   ▼   ▼
Node Node Node
```

---

## Purpose

```text id="eks1a26"
Simplified Management
Scaling
Automation
```

---

## Types

```text id="eks1a27"
Managed
Self-Managed
```

---

## Benefits

```text id="eks1a28"
Consistency
Operational Simplicity
```

---

# 8. Managed Node Groups

AWS manages the worker nodes.

---

## Architecture

```text id="eks1a29"
AWS
 │
 ▼
Managed Nodes
 │
 ▼
Pods
```

---

## Features

```text id="eks1a30"
Automatic Updates
Health Monitoring
Scaling
```

---

## Benefits

```text id="eks1a31"
Reduced Operations
Automation
Reliability
```

---

## Common Use Cases

```text id="eks1a32"
Production Clusters
Enterprise Workloads
```

---

# 9. Self-Managed Node Groups

You manage EC2 worker nodes manually.

---

## Architecture

```text id="eks1a33"
Administrator
      │
      ▼
EC2 Instances
      │
      ▼
Pods
```

---

## Responsibilities

```text id="eks1a34"
Updates
Patching
Scaling
Maintenance
```

---

## Benefits

```text id="eks1a35"
Maximum Control
Customization
```

---

## Common Use Cases

```text id="eks1a36"
Custom AMIs
Specialized Workloads
```

---

# 10. Fargate Profiles

Fargate allows running pods without managing nodes.

---

## Architecture

```text id="eks1a37"
Amazon EKS
      │
      ▼
Fargate
      │
      ▼
Pods
```

---

## Advantages

```text id="eks1a38"
No Node Management
Serverless Containers
Simplified Operations
```

---

## Benefits

```text id="eks1a39"
Operational Efficiency
Security
Scalability
```

---

## Common Use Cases

```text id="eks1a40"
Microservices
APIs
Event Driven Applications
```

---

# 11. EKS Cluster Creation

Clusters can be created using AWS Console, AWS CLI, or eksctl.

---

## Architecture

```text id="eks1a41"
Create Cluster
      │
      ▼
Control Plane
      │
      ▼
Node Groups
```

---

## Common Tools

```text id="eks1a42"
AWS Console
AWS CLI
eksctl
Terraform
```

---

## Example

```bash id="eks1a43"
eksctl create cluster
```

---

## Benefits

```text id="eks1a44"
Automation
Fast Provisioning
```

---

# 12. kubectl Integration

kubectl is the primary Kubernetes command-line tool.

---

## Architecture

```text id="eks1a45"
kubectl
   │
   ▼
API Server
   │
   ▼
Cluster
```

---

## Common Commands

```bash id="eks1a46"
kubectl get nodes

kubectl get pods

kubectl get services
```

---

## Benefits

```text id="eks1a47"
Cluster Management
Troubleshooting
Automation
```

---

# 13. eksctl Fundamentals

eksctl is the most popular CLI tool for EKS.

---

## Purpose

Simplify EKS cluster management.

---

## Architecture

```text id="eks1a48"
eksctl
   │
   ▼
Amazon EKS
```

---

## Common Commands

```bash id="eks1a49"
eksctl create cluster

eksctl delete cluster

eksctl get nodegroup
```

---

## Benefits

```text id="eks1a50"
Simple Operations
Automation
Reduced Complexity
```

---

# 14. Kubernetes Objects in EKS

EKS uses standard Kubernetes resources.

---

## Common Objects

```text id="eks1a51"
Pods
Deployments
Services
Ingress
ConfigMaps
Secrets
```

---

## Architecture

```text id="eks1a52"
Deployment
     │
     ▼
ReplicaSet
     │
     ▼
Pods
```

---

## Benefits

```text id="eks1a53"
Portability
Standardization
Cloud Native Operations
```

---

# 15. EKS Networking Fundamentals

Networking enables communication between Kubernetes workloads.

---

## Architecture

```text id="eks1a54"
Pod
 │
 ▼
Service
 │
 ▼
VPC Network
```

---

## Components

```text id="eks1a55"
VPC
Subnets
Security Groups
Route Tables
Load Balancers
```

---

## Benefits

```text id="eks1a56"
Connectivity
Isolation
Security
```

---

## Workflow

```text id="eks1a57"
Pod
 │
 ▼
Service
 │
 ▼
Load Balancer
 │
 ▼
User
```

---

# 16. Amazon VPC CNI

Amazon VPC CNI enables Kubernetes pods to receive native VPC networking.

---

## Purpose

Allow pods to obtain VPC IP addresses directly.

---

## Architecture

```text id="eks2a1"
Pod
 │
 ▼
VPC CNI
 │
 ▼
VPC IP Address
```

---

## Benefits

```text id="eks2a2"
Native AWS Networking
High Performance
Simplified Connectivity
```

---

## Common Use Cases

```text id="eks2a3"
Microservices
Cloud Native Platforms
Production Workloads
```

---

# 17. Pods

Pods are the smallest deployable unit in Kubernetes.

---

## Architecture

```text id="eks2a4"
Pod
 │
 ├── Container
 ├── Container
 └── Shared Network
```

---

## Responsibilities

```text id="eks2a5"
Run Containers
Provide Networking
Provide Storage
```

---

## Benefits

```text id="eks2a6"
Container Grouping
Resource Sharing
Isolation
```

---

## Example

```yaml id="eks2a7"
apiVersion: v1
kind: Pod
metadata:
  name: nginx
```

---

# 18. Deployments

Deployments manage application rollout and updates.

---

## Architecture

```text id="eks2a8"
Deployment
     │
     ▼
ReplicaSet
     │
     ▼
Pods
```

---

## Responsibilities

```text id="eks2a9"
Scaling
Updates
Rollbacks
```

---

## Benefits

```text id="eks2a10"
Automation
Reliability
High Availability
```

---

## Common Command

```bash id="eks2a11"
kubectl get deployments
```

---

# 19. ReplicaSets

ReplicaSets maintain a desired number of pod replicas.

---

## Architecture

```text id="eks2a12"
ReplicaSet
     │
 ┌───┼───┐
 ▼   ▼   ▼
Pod Pod Pod
```

---

## Purpose

```text id="eks2a13"
Maintain Availability
Recover Failed Pods
Scale Applications
```

---

## Benefits

```text id="eks2a14"
Self Healing
Consistency
Reliability
```

---

# 20. Services

Services provide stable access to pods.

---

## Architecture

```text id="eks2a15"
Service
   │
   ▼
Pods
```

---

## Types

```text id="eks2a16"
ClusterIP
NodePort
LoadBalancer
ExternalName
```

---

## Benefits

```text id="eks2a17"
Service Discovery
Load Balancing
Stable Endpoints
```

---

# 21. ClusterIP

ClusterIP is the default Kubernetes Service type.

---

## Architecture

```text id="eks2a18"
Service
   │
   ▼
Cluster Internal IP
   │
   ▼
Pods
```

---

## Characteristics

```text id="eks2a19"
Internal Access Only
Default Service Type
```

---

## Benefits

```text id="eks2a20"
Internal Communication
Security
```

---

# 22. NodePort

NodePort exposes services through node IP addresses.

---

## Architecture

```text id="eks2a21"
User
 │
 ▼
Node IP:Port
 │
 ▼
Service
 │
 ▼
Pods
```

---

## Port Range

```text id="eks2a22"
30000 - 32767
```

---

## Benefits

```text id="eks2a23"
External Access
Simple Exposure
```

---

# 23. LoadBalancer Services

LoadBalancer services create AWS load balancers automatically.

---

## Architecture

```text id="eks2a24"
User
 │
 ▼
AWS Load Balancer
 │
 ▼
Service
 │
 ▼
Pods
```

---

## Benefits

```text id="eks2a25"
High Availability
Traffic Distribution
External Access
```

---

## Common Use Cases

```text id="eks2a26"
Web Applications
APIs
Public Services
```

---

# 24. Ingress

Ingress provides advanced HTTP/HTTPS routing.

---

## Architecture

```text id="eks2a27"
User
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

## Features

```text id="eks2a28"
Path Routing
Host Routing
SSL Termination
```

---

## Benefits

```text id="eks2a29"
Centralized Routing
Cost Optimization
```

---

# 25. AWS Load Balancer Controller

The AWS Load Balancer Controller manages ALB and NLB resources.

---

## Architecture

```text id="eks2a30"
Ingress
   │
   ▼
AWS Load Balancer Controller
   │
   ▼
ALB / NLB
```

---

## Responsibilities

```text id="eks2a31"
Create Load Balancers
Configure Routing
Manage Targets
```

---

## Benefits

```text id="eks2a32"
Automation
AWS Integration
Scalability
```

---

# 26. CoreDNS

CoreDNS provides DNS services within Kubernetes.

---

## Architecture

```text id="eks2a33"
Pod
 │
 ▼
CoreDNS
 │
 ▼
Service Discovery
```

---

## Responsibilities

```text id="eks2a34"
DNS Resolution
Service Discovery
Cluster Networking
```

---

## Benefits

```text id="eks2a35"
Automatic DNS
Simplified Communication
```

---

# 27. kube-proxy

kube-proxy manages service networking.

---

## Architecture

```text id="eks2a36"
Service
   │
   ▼
kube-proxy
   │
   ▼
Pods
```

---

## Responsibilities

```text id="eks2a37"
Traffic Routing
Load Balancing
Network Rules
```

---

## Benefits

```text id="eks2a38"
Connectivity
Service Access
```

---

# 28. Namespaces

Namespaces logically separate workloads.

---

## Architecture

```text id="eks2a39"
Cluster
 │
 ├── Namespace A
 ├── Namespace B
 └── Namespace C
```

---

## Common Namespaces

```text id="eks2a40"
default
kube-system
monitoring
dev
prod
```

---

## Benefits

```text id="eks2a41"
Isolation
Organization
Multi-Tenancy
```

---

# 29. Resource Requests & Limits

Requests and limits control resource allocation.

---

## Architecture

```text id="eks2a42"
Pod
 │
 ├── CPU Request
 ├── Memory Request
 ├── CPU Limit
 └── Memory Limit
```

---

## Example

```yaml id="eks2a43"
resources:
  requests:
    memory: "256Mi"
    cpu: "250m"
```

---

## Benefits

```text id="eks2a44"
Resource Control
Cluster Stability
Fair Usage
```

---

# 30. Horizontal Pod Autoscaler

HPA automatically scales pods.

---

## Architecture

```text id="eks2a45"
Metrics
  │
  ▼
HPA
  │
  ▼
Pods
```

---

## Common Metrics

```text id="eks2a46"
CPU Utilization
Memory Utilization
Custom Metrics
```

---

## Workflow

```text id="eks2a47"
High Load
    │
    ▼
HPA
    │
    ▼
More Pods
```

---

## Benefits

```text id="eks2a48"
Automatic Scaling
Performance
Cost Optimization
```

---

## Common Command

```bash id="eks2a49"
kubectl get hpa
```

---

# 31. IAM Integration

AWS IAM integrates directly with EKS for authentication and authorization.

---

## Purpose

Control access to EKS clusters and AWS resources.

---

## Architecture

```text id="eks3a1"
IAM User/Role
      │
      ▼
Amazon EKS
      │
      ▼
Kubernetes Cluster
```

---

## Common Uses

```text id="eks3a2"
Cluster Administration
Developer Access
Automation
CI/CD Pipelines
```

---

## Benefits

```text id="eks3a3"
Centralized Security
Fine-Grained Permissions
AWS Integration
```

---

# 32. IAM Roles for Service Accounts (IRSA)

IRSA allows Kubernetes Pods to securely access AWS services.

---

## Architecture

```text id="eks3a4"
Pod
 │
 ▼
Service Account
 │
 ▼
IAM Role
 │
 ▼
AWS Service
```

---

## Common Services

```text id="eks3a5"
S3
DynamoDB
Secrets Manager
SQS
SNS
```

---

## Benefits

```text id="eks3a6"
No Static Credentials
Improved Security
Least Privilege Access
```

---

## Workflow

```text id="eks3a7"
Pod
 │
 ▼
IRSA
 │
 ▼
Temporary Credentials
 │
 ▼
AWS API
```

---

# 33. RBAC

Role-Based Access Control controls Kubernetes permissions.

---

## Architecture

```text id="eks3a8"
User
 │
 ▼
Role
 │
 ▼
Permissions
 │
 ▼
Resources
```

---

## Components

```text id="eks3a9"
Role
ClusterRole
RoleBinding
ClusterRoleBinding
```

---

## Benefits

```text id="eks3a10"
Security
Access Control
Governance
```

---

# 34. Security Groups for Pods

Security Groups can be assigned directly to Pods.

---

## Architecture

```text id="eks3a11"
Pod
 │
 ▼
Security Group
 │
 ▼
Network Traffic
```

---

## Benefits

```text id="eks3a12"
Micro-Segmentation
Enhanced Security
Traffic Isolation
```

---

## Common Use Cases

```text id="eks3a13"
Databases
Sensitive Applications
Regulated Workloads
```

---

# 35. Secrets Management

Secrets store sensitive information securely.

---

## Examples

```text id="eks3a14"
Passwords
API Keys
Certificates
Tokens
```

---

## Architecture

```text id="eks3a15"
Secret
  │
  ▼
Pod
```

---

## Common Solutions

```text id="eks3a16"
Kubernetes Secrets
AWS Secrets Manager
Parameter Store
```

---

## Benefits

```text id="eks3a17"
Security
Centralized Management
Compliance
```

---

# 36. ConfigMaps

ConfigMaps store non-sensitive configuration data.

---

## Architecture

```text id="eks3a18"
ConfigMap
    │
    ▼
Pod
```

---

## Examples

```text id="eks3a19"
Application Settings
URLs
Feature Flags
```

---

## Benefits

```text id="eks3a20"
Configuration Management
Flexibility
Portability
```

---

## Common Command

```bash id="eks3a21"
kubectl get configmaps
```

---

# 37. Persistent Volumes

Persistent Volumes provide storage independent of Pod lifecycle.

---

## Architecture

```text id="eks3a22"
Persistent Volume
        │
        ▼
Persistent Data
```

---

## Characteristics

```text id="eks3a23"
Persistent
Reusable
Cluster Resource
```

---

## Benefits

```text id="eks3a24"
Data Durability
Storage Abstraction
```

---

# 38. Persistent Volume Claims

PVCs request storage from Persistent Volumes.

---

## Architecture

```text id="eks3a25"
Pod
 │
 ▼
PVC
 │
 ▼
PV
```

---

## Benefits

```text id="eks3a26"
Storage Flexibility
Abstraction
Automation
```

---

## Workflow

```text id="eks3a27"
Application
     │
     ▼
PVC
     │
     ▼
Storage
```

---

# 39. EBS Integration

Amazon EBS provides block storage for EKS.

---

## Architecture

```text id="eks3a28"
Pod
 │
 ▼
PVC
 │
 ▼
Amazon EBS
```

---

## Characteristics

```text id="eks3a29"
Block Storage
Persistent
High Performance
```

---

## Benefits

```text id="eks3a30"
Reliable Storage
Production Ready
```

---

## Common Use Cases

```text id="eks3a31"
Databases
Stateful Applications
```

---

# 40. EFS Integration

Amazon EFS provides shared file storage.

---

## Architecture

```text id="eks3a32"
Multiple Pods
      │
      ▼
Amazon EFS
```

---

## Characteristics

```text id="eks3a33"
Shared Storage
Elastic Capacity
Multi-AZ
```

---

## Benefits

```text id="eks3a34"
Scalability
Shared Access
```

---

## Common Use Cases

```text id="eks3a35"
CMS
Shared Data
Analytics
```

---

# 41. ECR Integration

Amazon ECR stores container images for EKS.

---

## Architecture

```text id="eks3a36"
Docker Image
      │
      ▼
Amazon ECR
      │
      ▼
Pod
```

---

## Workflow

```text id="eks3a37"
Build
 │
 ▼
Push ECR
 │
 ▼
Deploy EKS
```

---

## Benefits

```text id="eks3a38"
Native AWS Integration
Security
Scalability
```

---

# 42. CloudWatch Integration

CloudWatch provides monitoring for EKS.

---

## Architecture

```text id="eks3a39"
EKS Cluster
     │
     ▼
CloudWatch
     │
     ▼
Metrics
```

---

## Common Metrics

```text id="eks3a40"
CPU
Memory
Network
Pod Status
```

---

## Benefits

```text id="eks3a41"
Visibility
Monitoring
Alerting
```

---

# 43. Container Insights

Container Insights provides advanced observability.

---

## Architecture

```text id="eks3a42"
Cluster
   │
   ▼
Container Insights
   │
   ▼
Dashboards
```

---

## Metrics

```text id="eks3a43"
Node Metrics
Pod Metrics
Container Metrics
Network Metrics
```

---

## Benefits

```text id="eks3a44"
Operational Visibility
Troubleshooting
Performance Analysis
```

---

# 44. Logging

Logging provides operational visibility.

---

## Architecture

```text id="eks3a45"
Pods
 │
 ▼
Logs
 │
 ▼
CloudWatch / ELK
```

---

## Common Solutions

```text id="eks3a46"
CloudWatch Logs
Fluent Bit
OpenSearch
ELK Stack
```

---

## Benefits

```text id="eks3a47"
Auditing
Troubleshooting
Monitoring
```

---

## Workflow

```text id="eks3a48"
Application
     │
     ▼
Container Logs
     │
     ▼
Centralized Storage
```

---

# 45. Monitoring

Monitoring ensures cluster health and performance.

---

## Architecture

```text id="eks3a49"
Cluster
   │
   ▼
Metrics
   │
   ▼
Monitoring Platform
```

---

## Common Tools

```text id="eks3a50"
Prometheus
Grafana
CloudWatch
Container Insights
```

---

## What to Monitor

```text id="eks3a51"
CPU Usage
Memory Usage
Pod Health
Node Health
Storage
Network
```

---

## Benefits

```text id="eks3a52"
Reliability
Capacity Planning
Performance Optimization
```

---

## Common Monitoring Stack

```text id="eks3a53"
Prometheus
     │
     ▼
Grafana
     │
     ▼
Dashboards & Alerts
```

---

# 46. Cluster Autoscaler

Cluster Autoscaler automatically adjusts node counts based on workload demand.

---

## Purpose

Add or remove worker nodes automatically.

---

## Architecture

```text id="eks4a1"
Pending Pods
      │
      ▼
Cluster Autoscaler
      │
      ▼
Node Group Scaling
```

---

## Workflow

```text id="eks4a2"
More Pods
    │
    ▼
Insufficient Capacity
    │
    ▼
Add Nodes
```

---

## Benefits

```text id="eks4a3"
Cost Optimization
Automation
Scalability
```

---

# 47. Karpenter

Karpenter is AWS's next-generation Kubernetes node provisioning system.

---

## Architecture

```text id="eks4a4"
Pending Pods
      │
      ▼
Karpenter
      │
      ▼
Launch EC2 Nodes
```

---

## Advantages

```text id="eks4a5"
Fast Scaling
Flexible Instance Selection
Cost Optimization
```

---

## Benefits

```text id="eks4a6"
Efficient Resource Usage
Reduced Costs
Improved Scheduling
```

---

## Common Use Cases

```text id="eks4a7"
Production Clusters
Dynamic Workloads
Large Kubernetes Platforms
```

---

# 48. Fargate on EKS

Fargate enables serverless Kubernetes workloads.

---

## Architecture

```text id="eks4a8"
Amazon EKS
      │
      ▼
Fargate Profile
      │
      ▼
Pods
```

---

## Features

```text id="eks4a9"
No Node Management
Serverless Execution
Automatic Scaling
```

---

## Benefits

```text id="eks4a10"
Operational Simplicity
Security
Efficiency
```

---

# 49. CI/CD Integration

CI/CD automates application delivery to EKS.

---

## Architecture

```text id="eks4a11"
Source Code
     │
     ▼
CI/CD Pipeline
     │
     ▼
Amazon EKS
```

---

## Workflow

```text id="eks4a12"
Build
 │
 ▼
Test
 │
 ▼
Deploy
```

---

## Benefits

```text id="eks4a13"
Automation
Consistency
Rapid Delivery
```

---

# 50. GitHub Actions Integration

GitHub Actions can automate EKS deployments.

---

## Architecture

```text id="eks4a14"
GitHub
   │
   ▼
Actions Workflow
   │
   ▼
Amazon EKS
```

---

## Typical Pipeline

```text id="eks4a15"
Commit
  │
  ▼
Build
  │
  ▼
Push Image
  │
  ▼
Deploy
```

---

## Benefits

```text id="eks4a16"
Automation
Version Control Integration
Continuous Delivery
```

---

# 51. ArgoCD Integration

ArgoCD is a GitOps deployment tool for Kubernetes.

---

## Architecture

```text id="eks4a17"
Git Repository
      │
      ▼
ArgoCD
      │
      ▼
Amazon EKS
```

---

## Workflow

```text id="eks4a18"
Git Commit
      │
      ▼
Sync
      │
      ▼
Cluster Update
```

---

## Benefits

```text id="eks4a19"
GitOps
Automated Deployments
Rollback Support
```

---

# 52. Helm Integration

Helm is the package manager for Kubernetes.

---

## Architecture

```text id="eks4a20"
Helm Chart
     │
     ▼
Helm
     │
     ▼
EKS Cluster
```

---

## Common Uses

```text id="eks4a21"
Install Applications
Manage Releases
Upgrade Workloads
```

---

## Benefits

```text id="eks4a22"
Reusability
Simplified Deployments
Version Management
```

---

## Common Commands

```bash id="eks4a23"
helm install

helm upgrade

helm rollback

helm uninstall
```

---

# 53. Service Mesh Overview

A Service Mesh manages communication between microservices.

---

## Architecture

```text id="eks4a24"
Service A
    │
Sidecar Proxy
    │
    ▼
Service B
```

---

## Features

```text id="eks4a25"
Traffic Control
Security
Observability
```

---

## Benefits

```text id="eks4a26"
Reliability
Monitoring
Encryption
```

---

# 54. Istio Integration

Istio is one of the most popular service meshes.

---

## Architecture

```text id="eks4a27"
Istio Control Plane
         │
         ▼
Envoy Sidecars
         │
         ▼
Applications
```

---

## Features

```text id="eks4a28"
Traffic Management
mTLS
Observability
```

---

## Benefits

```text id="eks4a29"
Advanced Networking
Security
Monitoring
```

---

# 55. Security Best Practices

Security should be implemented throughout the cluster lifecycle.

---

## Recommendations

```text id="eks4a30"
Use IRSA
Enable RBAC
Encrypt Secrets
Restrict Network Access
```

---

## Security Layers

```text id="eks4a31"
IAM
RBAC
Network Policies
Secrets
Encryption
```

---

## Benefits

```text id="eks4a32"
Compliance
Governance
Risk Reduction
```

---

# 56. Cost Optimization

Optimize resources to reduce Kubernetes costs.

---

## Strategies

```text id="eks4a33"
Cluster Autoscaler
Karpenter
Spot Instances
Resource Limits
```

---

## Architecture

```text id="eks4a34"
Workloads
    │
    ▼
Optimization
    │
    ▼
Reduced Cost
```

---

## Benefits

```text id="eks4a35"
Efficiency
Lower Costs
Better Utilization
```

---

# 57. High Availability

EKS supports highly available Kubernetes platforms.

---

## Architecture

```text id="eks4a36"
AZ-A
 │
 ▼
Nodes

AZ-B
 │
 ▼
Nodes

AZ-C
 │
 ▼
Nodes
```

---

## Best Practices

```text id="eks4a37"
Multiple AZs
Load Balancers
ReplicaSets
Auto Scaling
```

---

## Benefits

```text id="eks4a38"
Fault Tolerance
Reliability
Business Continuity
```

---

# 58. Disaster Recovery

Disaster Recovery protects Kubernetes workloads.

---

## Architecture

```text id="eks4a39"
Primary Cluster
       │
       ▼
Backup
       │
       ▼
Recovery Cluster
```

---

## Recovery Components

```text id="eks4a40"
EBS Snapshots
EFS Backups
Velero
GitOps Repositories
```

---

## Benefits

```text id="eks4a41"
Business Continuity
Risk Reduction
Recovery Capability
```

---

# 59. Troubleshooting

Troubleshooting helps diagnose cluster issues.

---

## Common Issues

```text id="eks4a42"
Pending Pods
CrashLoopBackOff
Image Pull Errors
Networking Issues
```

---

## Workflow

```text id="eks4a43"
Issue
 │
 ▼
Logs
 │
 ▼
Analysis
 │
 ▼
Resolution
```

---

## Useful Commands

```bash id="eks4a44"
kubectl describe pod

kubectl logs

kubectl get events
```

---

## Benefits

```text id="eks4a45"
Faster Resolution
Operational Visibility
```

---

# 60. Common Commands

## Cluster Information

```bash id="eks4a46"
kubectl cluster-info
```

---

## View Nodes

```bash id="eks4a47"
kubectl get nodes
```

---

## View Pods

```bash id="eks4a48"
kubectl get pods
```

---

## View Deployments

```bash id="eks4a49"
kubectl get deployments
```

---

## View Services

```bash id="eks4a50"
kubectl get svc
```

---

## Describe Pod

```bash id="eks4a51"
kubectl describe pod POD_NAME
```

---

## Pod Logs

```bash id="eks4a52"
kubectl logs POD_NAME
```

---

## Execute Commands

```bash id="eks4a53"
kubectl exec -it POD_NAME -- sh
```

---

## Apply Configuration

```bash id="eks4a54"
kubectl apply -f app.yaml
```

---

## Delete Resource

```bash id="eks4a55"
kubectl delete -f app.yaml
```

---

# 61. Real-World Architectures

---

## EKS + ECR Architecture

```text id="eks5a1"
Developer
    │
    ▼
Amazon ECR
    │
    ▼
Amazon EKS
    │
    ▼
Pods
```

---

## Production EKS Architecture

```text id="eks5a2"
Users
  │
  ▼
Route53
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

## GitOps Architecture

```text id="eks5a3"
Git Repository
      │
      ▼
ArgoCD
      │
      ▼
Amazon EKS
```

---

## Enterprise Platform Architecture

```text id="eks5a4"
Developers
     │
     ▼
CI/CD
     │
     ▼
Amazon ECR
     │
     ▼
Amazon EKS
     │
     ▼
Monitoring Stack
```

---

## Multi-AZ Production Architecture

```text id="eks5a5"
ALB
 │
 ├────────────┬────────────┐
 ▼            ▼            ▼
AZ-A         AZ-B         AZ-C
 │            │            │
 ▼            ▼            ▼
Pods         Pods         Pods
```

---

# 62. EKS vs ECS

| Feature        | EKS        | ECS         |
| -------------- | ---------- | ----------- |
| Orchestrator   | Kubernetes | AWS Native  |
| Complexity     | Higher     | Lower       |
| Learning Curve | Steeper    | Easier      |
| Portability    | High       | AWS Focused |
| Ecosystem      | Kubernetes | AWS         |

---

## Recommendation

```text id="eks5a6"
EKS → Kubernetes Skills

ECS → Simpler AWS Operations
```

---

## Architecture

```text id="eks5a7"
EKS
 │
 ▼
Kubernetes

ECS
 │
 ▼
AWS Native Containers
```

---

# 63. EKS vs Kubernetes Self-Managed

| Feature           | EKS        | Self-Managed Kubernetes |
| ----------------- | ---------- | ----------------------- |
| Control Plane     | Managed    | Self Managed            |
| Upgrades          | Simplified | Manual                  |
| High Availability | Built In   | User Managed            |
| Operations        | Lower      | Higher                  |

---

## Benefits of EKS

```text id="eks5a8"
Managed Control Plane
Integrated Security
Reduced Operations
```

---

## Common Use Cases

```text id="eks5a9"
Enterprise Kubernetes
Production Clusters
```

---

# 64. EKS vs OpenShift

| Feature             | EKS  | OpenShift |
| ------------------- | ---- | --------- |
| Provider            | AWS  | Red Hat   |
| Kubernetes Based    | Yes  | Yes       |
| Managed Service     | Yes  | Optional  |
| Enterprise Features | Good | Extensive |

---

## Architecture

```text id="eks5a10"
EKS
 │
 ▼
AWS Ecosystem

OpenShift
 │
 ▼
Red Hat Ecosystem
```

---

## Recommendation

```text id="eks5a11"
EKS → AWS Native

OpenShift → Enterprise Platforms
```

---

# 65. EKS vs AKS

| Feature         | EKS     | AKS            |
| --------------- | ------- | -------------- |
| Cloud Provider  | AWS     | Azure          |
| IAM Integration | AWS IAM | Azure Entra ID |
| Registry        | ECR     | ACR            |
| Networking      | VPC     | Azure VNet     |

---

## Architecture

```text id="eks5a12"
AWS
 │
 ▼
EKS

Azure
 │
 ▼
AKS
```

---

## Common Recommendation

```text id="eks5a13"
Choose the Kubernetes service aligned with your cloud provider.
```

---

# 66. EKS vs GKE

| Feature            | EKS | GKE               |
| ------------------ | --- | ----------------- |
| Cloud Provider     | AWS | Google Cloud      |
| Registry           | ECR | Artifact Registry |
| Networking         | VPC | VPC               |
| Managed Kubernetes | Yes | Yes               |

---

## Architecture

```text id="eks5a14"
AWS
 │
 ▼
EKS

Google Cloud
 │
 ▼
GKE
```

---

## Common Recommendation

```text id="eks5a15"
EKS → AWS Environments

GKE → Google Cloud Environments
```

---

# 67. Common Use Cases

---

## Microservices

```text id="eks5a16"
Frontend
    │
 ┌──┼──┐
 ▼  ▼  ▼
API API API
```

---

## Platform Engineering

```text id="eks5a17"
Developers
     │
     ▼
Self-Service Platform
     │
     ▼
Amazon EKS
```

---

## CI/CD Platforms

```text id="eks5a18"
Git
 │
 ▼
Pipeline
 │
 ▼
EKS
```

---

## Machine Learning Platforms

```text id="eks5a19"
Training
    │
    ▼
Kubernetes Jobs
```

---

## Enterprise Applications

```text id="eks5a20"
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

# 68. Interview Questions

---

## What is Amazon EKS?

Amazon EKS is a managed Kubernetes service provided by AWS.

---

## What components are managed by AWS?

```text id="eks5a21"
API Server
etcd
Scheduler
Controller Manager
```

---

## What is a Node Group?

A collection of worker nodes managed together.

---

## Difference Between Managed and Self-Managed Node Groups?

| Managed           | Self-Managed   |
| ----------------- | -------------- |
| AWS Managed       | User Managed   |
| Easier Operations | More Control   |
| Automated Updates | Manual Updates |

---

## What is IRSA?

IAM Roles for Service Accounts allow Pods to access AWS services securely.

---

## What is the Amazon VPC CNI?

The networking plugin that assigns VPC IP addresses directly to Pods.

---

## What is Karpenter?

A modern node provisioning solution for Kubernetes on AWS.

---

## Difference Between Cluster Autoscaler and Karpenter?

| Cluster Autoscaler   | Karpenter                 |
| -------------------- | ------------------------- |
| Scales Node Groups   | Provisions Nodes Directly |
| Traditional Approach | Modern Approach           |
| Slower               | Faster                    |

---

## What is a Fargate Profile?

Defines which Pods run on AWS Fargate.

---

## Why use EKS instead of self-managed Kubernetes?

Because AWS manages the Kubernetes control plane, reducing operational complexity.

---

# 69. Additional Resources

## Official Resources

* Amazon EKS Documentation
* Kubernetes Documentation
* eksctl Documentation
* AWS IAM Documentation
* Amazon ECR Documentation

---

## Learning Resources

* AWS Skill Builder
* AWS Workshops
* Kubernetes Documentation
* CNCF Training
* AWS Architecture Center

---

## Related Technologies

```text id="eks5a22"
Docker
Kubernetes
Helm
ArgoCD
Amazon ECR
AWS Fargate
Karpenter
```

---

# 70. Quick Reference

## Cluster Management

```bash id="eks5a23"
aws eks list-clusters

aws eks describe-cluster
```

---

## eksctl

```bash id="eks5a24"
eksctl create cluster

eksctl delete cluster

eksctl get nodegroup
```

---

## Kubernetes Resources

```bash id="eks5a25"
kubectl get nodes

kubectl get pods

kubectl get deployments

kubectl get services
```

---

## Troubleshooting

```bash id="eks5a26"
kubectl describe pod

kubectl logs

kubectl get events
```

---

## Apply Changes

```bash id="eks5a27"
kubectl apply -f app.yaml

kubectl delete -f app.yaml
```

---

## Helm

```bash id="eks5a28"
helm install

helm upgrade

helm rollback
```

---

# 📎 Official Documentation

* [Amazon EKS Documentation](https://docs.aws.amazon.com/eks/)
* [Kubernetes Documentation](https://kubernetes.io/docs/)
* [eksctl Documentation](https://eksctl.io/)
* [Amazon ECR Documentation](https://docs.aws.amazon.com/ecr/)
* [AWS IAM Documentation](https://docs.aws.amazon.com/iam/)

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, DevOps engineering, platform engineering, cloud-native engineering, and Kubernetes administration.

Primary references include:

* Amazon EKS Documentation
* Kubernetes Documentation
* eksctl Documentation
* AWS IAM Documentation
* Amazon ECR Documentation

Amazon EKS is a managed Kubernetes service provided by [Amazon Web Services](https://aws.amazon.com).

Kubernetes is a project of the [Cloud Native Computing Foundation (CNCF)](https://www.cncf.io).

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

You now understand:

* Amazon EKS Fundamentals
* Kubernetes Architecture
* Control Plane
* Worker Nodes
* Node Groups
* Fargate Profiles
* Pods
* Deployments
* Services
* Ingress
* AWS Load Balancer Controller
* IAM Integration
* IRSA
* RBAC
* Storage
* EBS & EFS
* Monitoring
* CloudWatch
* Container Insights
* Helm
* ArgoCD
* Karpenter
* Autoscaling
* Security
* Troubleshooting
* Production Architectures

Amazon EKS provides:

```text id="eks5a29"
Managed Kubernetes
+
Cloud Native Platform
+
High Availability
+
AWS Integration
+
Autoscaling
+
Security
+
Enterprise Scalability
```

and remains one of the most widely adopted managed Kubernetes platforms for running production-grade containerized workloads on AWS.

---

```md id="eks5a30"
**Last Updated:** 2026
**Platform:** AWS EKS
**Version:** Kubernetes 1.33+
**License:** Proprietary AWS Managed Service

For latest information, visit https://docs.aws.amazon.com/eks/
```