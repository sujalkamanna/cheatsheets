# Kubernetes Cheatsheet

![Kubernetes Logo](https://kubernetes.io/images/kubernetes-horizontal-color.png)

---

# 📚 Table of Contents

| **Core Kubernetes**                                        | **Advanced Kubernetes**                                                  |
| ---------------------------------------------------------- | ------------------------------------------------------------------------ |
| [1. Kubernetes Fundamentals](#1-kubernetes-fundamentals)   | [18. Cloud-Native Ecosystem](#18-cloud-native-ecosystem)                 |
| [2. Kubernetes Architecture](#2-kubernetes-architecture)   | [19. Advanced Enterprise Topics](#19-advanced-enterprise-topics)         |
| [3. Control Plane Components](#3-control-plane-components) | [20. Kubernetes Troubleshooting](#20-kubernetes-troubleshooting)         |
| [4. Worker Node Components](#4-worker-node-components)     | [21. Expert-Level Topics](#21-expert-level-topics)                       |
| [5. Kubernetes Objects](#5-kubernetes-objects)             | [22. Kubernetes Certification Guide](#22-kubernetes-certification-guide) |
| [6. YAML Fundamentals](#6-yaml-fundamentals)               | [23. KCNA Certification](#23-kcna-certification)                         |
| [7. Pods](#7-pods)                                         | [24. CKA Certification](#24-cka-certification)                           |
| [8. ReplicaSets](#8-replicasets)                           | [25. CKAD Certification](#25-ckad-certification)                         |
| [9. Deployments](#9-deployments)                           | [26. CKS Certification](#26-cks-certification)                           |
| [10. Namespaces](#10-namespaces)                           | [27. Kubernetes Career Roadmap](#27-kubernetes-career-roadmap)           |
| [11. Labels & Selectors](#11-labels--selectors)            | [28. Kubernetes Interview Questions](#28-kubernetes-interview-questions) |
| [12. Services](#12-services)                               | [29. kubectl Command Encyclopedia](#29-kubectl-command-encyclopedia)     |
| [13. DNS & Service Discovery](#13-dns--service-discovery)  | [30. CKA Quick Revision Sheet](#30-cka-quick-revision-sheet)             |
| [14. Ingress](#14-ingress)                                 | [31. CKAD Quick Revision Sheet](#31-ckad-quick-revision-sheet)           |
| [15. Network Policies](#15-network-policies)               | [32. CKS Quick Revision Sheet](#32-cks-quick-revision-sheet)             |
| [16. ConfigMaps & Secrets](#16-configmaps--secrets)        | [33. Official Documentation](#33-official-documentation)                 |
| [17. Storage & Volumes](#17-storage--volumes)              | [34. Copyright](#34-copyright)                                           |

---

| **Workloads & Operations**                                      | **Platform Engineering & GitOps**                                    |
| --------------------------------------------------------------- | -------------------------------------------------------------------- |
| [35. StatefulSets](#35-statefulsets)                            | [52. Helm](#52-helm)                                                 |
| [36. DaemonSets](#36-daemonsets)                                | [53. Kustomize](#53-kustomize)                                       |
| [37. Jobs](#37-jobs)                                            | [54. GitOps](#54-gitops)                                             |
| [38. CronJobs](#38-cronjobs)                                    | [55. Argo CD](#55-argo-cd)                                           |
| [39. Scheduling](#39-scheduling)                                | [56. Flux CD](#56-flux-cd)                                           |
| [40. Node Affinity](#40-node-affinity)                          | [57. CI/CD Integration](#57-cicd-integration)                        |
| [41. Pod Affinity](#41-pod-affinity)                            | [58. Progressive Delivery](#58-progressive-delivery)                 |
| [42. Taints & Tolerations](#42-taints--tolerations)             | [59. Canary Deployments](#59-canary-deployments)                     |
| [43. Authentication](#43-authentication)                        | [60. Blue-Green Deployments](#60-blue-green-deployments)             |
| [44. Authorization](#44-authorization)                          | [61. Secret Management](#61-secret-management)                       |
| [45. RBAC](#45-rbac)                                            | [62. HashiCorp Vault](#62-hashicorp-vault)                           |
| [46. Service Accounts](#46-service-accounts)                    | [63. OPA](#63-opa)                                                   |
| [47. Admission Controllers](#47-admission-controllers)          | [64. Kyverno](#64-kyverno)                                           |
| [48. Pod Security Standards](#48-pod-security-standards)        | [65. Platform Engineering](#65-platform-engineering)                 |
| [49. Security Contexts](#49-security-contexts)                  | [66. Internal Developer Platforms](#66-internal-developer-platforms) |
| [50. Logging](#50-logging)                                      | [67. Multi-Cluster Kubernetes](#67-multi-cluster-kubernetes)         |
| [51. Monitoring & Observability](#51-monitoring--observability) | [68. Cluster API](#68-cluster-api)                                   |

---

| **Networking & Storage**                                     | **Cluster Operations**                                   |
| ------------------------------------------------------------ | -------------------------------------------------------- |
| [69. CNI Fundamentals](#69-cni-fundamentals)                 | [84. Cluster Installation](#84-cluster-installation)     |
| [70. Calico](#70-calico)                                     | [85. kubeadm](#85-kubeadm)                               |
| [71. Cilium](#71-cilium)                                     | [86. Node Management](#86-node-management)               |
| [72. Flannel](#72-flannel)                                   | [87. Cluster Upgrades](#87-cluster-upgrades)             |
| [73. Service Mesh](#73-service-mesh)                         | [88. Certificate Management](#88-certificate-management) |
| [74. Istio](#74-istio)                                       | [89. etcd Backup & Restore](#89-etcd-backup--restore)    |
| [75. Linkerd](#75-linkerd)                                   | [90. High Availability](#90-high-availability)           |
| [76. Gateway API](#76-gateway-api)                           | [91. Disaster Recovery](#91-disaster-recovery)           |
| [77. Volume Types](#77-volume-types)                         | [92. HPA](#92-hpa)                                       |
| [78. Persistent Volumes](#78-persistent-volumes)             | [93. VPA](#93-vpa)                                       |
| [79. Persistent Volume Claims](#79-persistent-volume-claims) | [94. Cluster Autoscaler](#94-cluster-autoscaler)         |
| [80. Storage Classes](#80-storage-classes)                   | [95. Pod Disruption Budgets](#95-pod-disruption-budgets) |
| [81. CSI Drivers](#81-csi-drivers)                           | [96. Resource Quotas](#96-resource-quotas)               |
| [82. Volume Snapshots](#82-volume-snapshots)                 | [97. Limit Ranges](#97-limit-ranges)                     |
| [83. Stateful Storage](#83-stateful-storage)                 | [98. Multi-Tenancy](#98-multi-tenancy)                   |

---

| **Kubernetes Internals**                                    | **Reference Sections**                                           |
| ----------------------------------------------------------- | ---------------------------------------------------------------- |
| [99. API Machinery](#99-api-machinery)                      | [111. Architecture Diagrams](#111-architecture-diagrams)         |
| [100. Controllers](#100-controllers)                        | [112. YAML Examples](#112-yaml-examples)                         |
| [101. Reconciliation Loop](#101-reconciliation-loop)        | [113. Production Checklists](#113-production-checklists)         |
| [102. Informers](#102-informers)                            | [114. Troubleshooting Scenarios](#114-troubleshooting-scenarios) |
| [103. Watches](#103-watches)                                | [115. Common Error Reference](#115-common-error-reference)       |
| [104. CRDs](#104-crds)                                      | [116. Best Practices](#116-best-practices)                       |
| [105. Operators](#105-operators)                            | [117. Learning Resources](#117-learning-resources)               |
| [106. Admission Webhooks](#106-admission-webhooks)          | [118. Labs & Practice Platforms](#118-labs--practice-platforms)  |
| [107. Aggregated APIs](#107-aggregated-apis)                | [119. Disclaimer & Attribution](#119-disclaimer--attribution)    |
| [108. Scheduler Internals](#108-scheduler-internals)        | [120. Conclusion](#120-conclusion)                               |
| [109. Runtime Security](#109-runtime-security)              |                                                                  |
| [110. eBPF & Cilium Internals](#110-ebpf--cilium-internals) |                                                                  |

---

* [Additional Resources](#additional-resources)

* [Quick Reference](#quick-reference)

* [📎 Official Documentation](#-official-documentation)

* [📄 Copyright](#-copyright)

* [📎 Disclaimer & Attribution](#-disclaimer--attribution)

* [🎉 Conclusion](#-conclusion)

---

# 1. Why Containers?

Before Kubernetes existed, applications were typically deployed directly on servers or Virtual Machines.

This caused dependency conflicts, portability issues, and inconsistent deployments.

Containers solve these problems.

---

## Traditional Deployment

```text
Application A
Java 8

Application B
Java 17

Conflict ❌
```

---

## Container-Based Deployment

```text
Container A
 └── Java 8

Container B
 └── Java 17
```

---

## Benefits

```text
Portability
Isolation
Consistency
Fast Deployment
Resource Efficiency
```

---

# 2. Virtual Machines vs Containers

---

## Virtual Machines

```text
Hardware
   │
   ▼
Hypervisor
   │
 ┌─┴─────┐
 ▼       ▼
VM1     VM2
 │       │
OS      OS
 │       │
Apps    Apps
```

---

### Characteristics

```text
Full OS
Heavyweight
Slower Startup
More Resource Usage
```

---

## Containers

```text
Hardware
   │
   ▼
Host OS
   │
Container Runtime
   │
 ┌──┴───┐
 ▼      ▼
C1     C2
```

---

### Characteristics

```text
Shared Kernel
Lightweight
Fast Startup
Less Resource Usage
```

---

## VM vs Container

| Feature        | VM       | Container |
| -------------- | -------- | --------- |
| OS             | Separate | Shared    |
| Startup        | Slow     | Fast      |
| Size           | GBs      | MBs       |
| Performance    | Lower    | Higher    |
| Resource Usage | High     | Low       |

---

# 3. Docker vs Kubernetes

One of the most common interview questions.

---

## Docker

Docker creates and runs containers.

```text
Docker
   │
   ▼
Container
```

---

## Kubernetes

Kubernetes manages containers at scale.

```text
Kubernetes
      │
      ▼
1000+ Containers
```

---

## Responsibilities

| Docker            | Kubernetes     |
| ----------------- | -------------- |
| Build Images      | Orchestration  |
| Run Containers    | Scaling        |
| Container Runtime | Self Healing   |
| Image Packaging   | Load Balancing |

---

## Real World Example

```text
Docker = Builds Cars

Kubernetes = Manages Traffic
```

---

# 4. Container Runtime Fundamentals

A Container Runtime actually runs containers.

---

## Common Runtimes

```text
containerd
CRI-O
Docker Engine
```

---

## Architecture

```text
Kubernetes
     │
     ▼
CRI
     │
     ▼
containerd
     │
     ▼
Containers
```

---

## What is CRI?

CRI = Container Runtime Interface

It allows Kubernetes to communicate with runtimes.

---

## Benefits

```text
Standardization
Flexibility
Vendor Independence
```

---

# 5. What is Kubernetes?

Kubernetes is an open-source container orchestration platform.

It automates:

```text
Deployment
Scaling
Networking
Storage
Monitoring
Recovery
```

---

## Origin

Created by Google.

Based on Google's internal Borg system.

Now maintained by the Cloud Native Computing Foundation.

---

## Architecture

```text
Developer
    │
    ▼
Kubernetes
    │
    ▼
Containers
```

---

## Benefits

```text
Automation
High Availability
Scalability
Self Healing
```

---

# 6. Why Kubernetes?

Managing hundreds of containers manually is impossible.

---

## Without Kubernetes

```text
Container Failure
      │
      ▼
Manual Recovery
```

---

## With Kubernetes

```text
Container Failure
      │
      ▼
Automatic Recovery
```

---

## Advantages

```text
Self Healing
Scaling
Rolling Updates
Load Balancing
```

---

# 7. Kubernetes Features

---

## Core Features

```text
Service Discovery
Load Balancing
Storage Orchestration
Self Healing
Autoscaling
Secret Management
```

---

## Enterprise Features

```text
Multi-Tenancy
RBAC
Network Policies
Audit Logging
```

---

## Benefits

```text
Production Ready
Cloud Native
Highly Available
```

---

# 8. Kubernetes Architecture

Kubernetes follows a master-worker architecture.

---

## Architecture

```text
                 Cluster
                    │
     ┌──────────────┴──────────────┐
     ▼                             ▼

Control Plane                 Worker Nodes

API Server                    Pods
Scheduler                     Containers
Controller Manager            kubelet
etcd                          kube-proxy
```

---

## Components

```text
Control Plane
Worker Nodes
Pods
Networking
Storage
```

---

# 9. Cluster Components

---

## High-Level View

```text
Cluster
 │
 ├── Control Plane
 │
 └── Worker Nodes
```

---

## Control Plane

```text
API Server
Scheduler
Controller Manager
etcd
```

---

## Worker Node

```text
kubelet
kube-proxy
Container Runtime
Pods
```

---

# 10. Control Plane

Control Plane makes decisions for the cluster.

---

## Components

```text
API Server
Scheduler
Controller Manager
etcd
```

---

## Responsibilities

```text
Scheduling
State Management
Scaling Decisions
API Processing
```

---

## Architecture

```text
User
 │
 ▼
API Server
 │
 ▼
Control Plane
```

---

# 11. Worker Nodes

Worker Nodes run actual applications.

---

## Components

```text
kubelet
kube-proxy
containerd
Pods
```

---

## Architecture

```text
Worker Node
     │
 ┌───┼────┐
 ▼   ▼    ▼
Pods Runtime kubelet
```

---

## Responsibilities

```text
Run Containers
Report Status
Networking
Storage Mounting
```

---

# 12. Kubernetes Request Flow

Very important interview topic.

---

## kubectl apply Flow

```text
kubectl apply
      │
      ▼
API Server
      │
      ▼
etcd
      │
      ▼
Scheduler
      │
      ▼
Worker Node
      │
      ▼
Pod Created
```

---

## Detailed Flow

```text
User
 │
 ▼
kubectl

 │

 ▼
API Server

 │

 ▼
etcd

 │

 ▼
Scheduler

 │

 ▼
kubelet

 │

 ▼
Container Runtime

 │

 ▼
Container
```

---

# 13. Declarative vs Imperative Management

---

## Imperative

Command-based.

```bash
kubectl run nginx --image=nginx
```

---

## Declarative

YAML-based.

```bash
kubectl apply -f deployment.yaml
```

---

## Comparison

| Declarative     | Imperative      |
| --------------- | --------------- |
| YAML            | Commands        |
| Version Control | Difficult       |
| Repeatable      | Less Repeatable |
| GitOps Friendly | No              |

---

## Recommendation

```text
Production → Declarative

Learning → Imperative
```

---

# 14. YAML Fundamentals

Almost everything in Kubernetes is defined using YAML.

---

## Basic Structure

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx

spec:
  containers:
  - name: nginx
    image: nginx
```

---

## Important Fields

```text
apiVersion
kind
metadata
spec
status
```

---

## YAML Hierarchy

```text
Object
 │
 ├── Metadata
 │
 └── Spec
```

---

# 15. Kubernetes Object Structure

Every Kubernetes resource follows the same structure.

---

## Generic Object

```yaml
apiVersion: apps/v1

kind: Deployment

metadata:
  name: my-app

spec:
  replicas: 3
```

---

## Common Objects

```text
Pod
Deployment
ReplicaSet
Service
ConfigMap
Secret
PVC
Ingress
```

---

## Universal Architecture

```text
apiVersion
     │
     ▼
kind
     │
     ▼
metadata
     │
     ▼
spec
```

---

## Important Commands

```bash
kubectl cluster-info

kubectl get nodes

kubectl get pods

kubectl get all

kubectl describe pod POD_NAME

kubectl logs POD_NAME
```

---

# 16. Pods

A Pod is the smallest deployable unit in Kubernetes.

A Pod can contain:

```text id="k8s2a1"
One Container

OR

Multiple Containers
```

All containers inside a Pod share:

```text id="k8s2a2"
Network Namespace
IP Address
Storage Volumes
```

---

## Architecture

```text id="k8s2a3"
Pod
 │
 └── Container
```

---

## Single Container Pod

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx-pod

spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

---

## Create Pod

```bash
kubectl apply -f pod.yaml
```

---

## Verify Pod

```bash
kubectl get pods
```

---

## Describe Pod

```bash
kubectl describe pod nginx-pod
```

---

## Pod Lifecycle

```text id="k8s2a4"
Create
  │
  ▼
Schedule
  │
  ▼
Pull Image
  │
  ▼
Create Container
  │
  ▼
Running
```

---

# 17. Pod Lifecycle

Pods move through different states.

---

## Pod States

```text id="k8s2a5"
Pending
Running
Succeeded
Failed
Unknown
Terminating
```

---

## Pending

```text id="k8s2a6"
Pod Created
Waiting For Node
Waiting For Resources
Waiting For Image
```

---

## Running

```text id="k8s2a7"
Container Started
Application Running
```

---

## Succeeded

```text id="k8s2a8"
Task Completed Successfully
```

Usually seen in Jobs.

---

## Failed

```text id="k8s2a9"
Application Failed
Container Exited
```

---

## Check Pod Status

```bash
kubectl get pods
```

Example:

```text
NAME        READY   STATUS    RESTARTS
nginx-pod   1/1     Running   0
```

---

# 18. Multi-Container Pods

A Pod can contain multiple containers.

---

## Why?

Containers may need to work together.

Examples:

```text id="k8s2a10"
Application Container
Logging Container

Application Container
Monitoring Container
```

---

## Architecture

```text id="k8s2a11"
Pod
 │
 ├── App Container
 │
 └── Sidecar Container
```

---

## Example

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: multi-container

spec:
  containers:

  - name: nginx
    image: nginx

  - name: busybox
    image: busybox
    command:
    - sleep
    - "3600"
```

---

## View Containers

```bash
kubectl describe pod multi-container
```

---

# 19. Init Containers

Init Containers run before application containers.

---

## Use Cases

```text id="k8s2a12"
Database Checks
Configuration Download
Dependency Validation
```

---

## Architecture

```text id="k8s2a13"
Init Container
       │
       ▼
Complete
       │
       ▼
Main Container Starts
```

---

## Example YAML

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: init-demo

spec:

  initContainers:
  - name: init-container
    image: busybox
    command:
    - sh
    - -c
    - "echo Initialization Complete"

  containers:
  - name: nginx
    image: nginx
```

---

## Benefits

```text id="k8s2a14"
Controlled Startup
Dependency Validation
Improved Reliability
```

---

# 20. Sidecar Pattern

A Sidecar container supports the main application.

---

## Architecture

```text id="k8s2a15"
Pod
 │
 ├── Application
 │
 └── Sidecar
```

---

## Common Examples

```text id="k8s2a16"
Log Collection
Metrics Collection
Proxy Containers
```

---

## Example

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: sidecar-demo

spec:

  containers:

  - name: app
    image: nginx

  - name: logger
    image: busybox
    command:
    - sleep
    - "3600"
```

---

## Benefits

```text id="k8s2a17"
Separation Of Concerns
Observability
Security
```

---

# 21. ReplicaSets

ReplicaSet ensures a specified number of Pods are running.

---

## Problem Solved

Without ReplicaSet:

```text id="k8s2a18"
Pod Crashes
     │
     ▼
Application Down
```

---

## With ReplicaSet

```text id="k8s2a19"
Pod Crashes
     │
     ▼
ReplicaSet
     │
     ▼
New Pod Created
```

---

## Architecture

```text id="k8s2a20"
ReplicaSet
      │
 ┌────┼────┐
 ▼    ▼    ▼
Pod  Pod  Pod
```

---

## ReplicaSet YAML

```yaml
apiVersion: apps/v1
kind: ReplicaSet

metadata:
  name: nginx-rs

spec:

  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:

    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx
```

---

## Create ReplicaSet

```bash
kubectl apply -f replicaset.yaml
```

---

## View ReplicaSets

```bash
kubectl get rs
```

---

## Scale ReplicaSet

```bash
kubectl scale rs nginx-rs --replicas=5
```

---

# 22. Deployments

Deployment is the most commonly used Kubernetes object.

A Deployment manages ReplicaSets.

---

## Architecture

```text id="k8s2a21"
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

```text id="k8s2a22"
Rolling Updates
Rollbacks
Version Control
Scaling
```

---

## Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-deployment

spec:

  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:

    metadata:
      labels:
        app: nginx

    spec:

      containers:

      - name: nginx
        image: nginx:latest

        ports:
        - containerPort: 80
```

---

## Create Deployment

```bash
kubectl apply -f deployment.yaml
```

---

## View Deployments

```bash
kubectl get deployments
```

---

## View ReplicaSets

```bash
kubectl get rs
```

---

## View Pods

```bash
kubectl get pods
```

---

# 23. Rolling Updates

Rolling Update updates Pods gradually.

---

## Architecture

```text id="k8s2a23"
Old Pods
    │
    ▼
Replace One By One
    │
    ▼
New Pods
```

---

## Update Image

```bash
kubectl set image deployment/nginx-deployment \
nginx=nginx:1.25
```

---

## Check Rollout

```bash
kubectl rollout status deployment/nginx-deployment
```

---

## Benefits

```text id="k8s2a24"
Zero Downtime
Safe Deployment
Controlled Updates
```

---

# 24. Rollbacks

Rollback restores a previous Deployment version.

---

## Architecture

```text id="k8s2a25"
Deployment v2
      │
      ▼
Rollback
      │
      ▼
Deployment v1
```

---

## Rollback Command

```bash
kubectl rollout undo deployment/nginx-deployment
```

---

## Benefits

```text id="k8s2a26"
Fast Recovery
Reduced Downtime
```

---

# 25. Revision History

Kubernetes stores Deployment revisions.

---

## View History

```bash
kubectl rollout history deployment/nginx-deployment
```

---

## Example

```text
REVISION
1
2
3
```

---

## View Specific Revision

```bash
kubectl rollout history deployment/nginx-deployment \
--revision=2
```

---

# 26. Namespaces

Namespaces provide logical isolation.

---

## Default Namespaces

```text id="k8s2a27"
default
kube-system
kube-public
kube-node-lease
```

---

## Architecture

```text id="k8s2a28"
Cluster
 │
 ├── Dev Namespace
 │
 ├── QA Namespace
 │
 └── Production Namespace
```

---

## Create Namespace

```yaml
apiVersion: v1
kind: Namespace

metadata:
  name: production
```

---

## Create Namespace

```bash
kubectl apply -f namespace.yaml
```

---

## List Namespaces

```bash
kubectl get namespaces
```

---

# 27. Labels

Labels are key-value pairs attached to objects.

---

## Example

```yaml
labels:
  app: nginx
  env: prod
  team: devops
```

---

## Benefits

```text id="k8s2a29"
Grouping
Filtering
Selection
```

---

## View Labels

```bash
kubectl get pods --show-labels
```

---

# 28. Selectors

Selectors identify resources using labels.

---

## MatchLabels Example

```yaml
selector:
  matchLabels:
    app: nginx
```

---

## MatchExpressions Example

```yaml
selector:
  matchExpressions:

  - key: environment

    operator: In

    values:
    - production
```

---

## Architecture

```text id="k8s2a30"
Labels
   │
   ▼
Selector
   │
   ▼
Target Pods
```

---

# 29. Annotations

Annotations store metadata.

Unlike labels, they are not used for selection.

---

## Example

```yaml
metadata:

  annotations:

    owner: devops-team

    description: production app
```

---

## Common Uses

```text id="k8s2a31"
Documentation
Build Information
Audit Information
Monitoring Metadata
```

---

# 30. Owner References & Finalizers

Advanced Kubernetes object lifecycle concepts.

---

## Owner References

Owner References define relationships.

Example:

```text id="k8s2a32"
Deployment
     │
     ▼
ReplicaSet
     │
     ▼
Pod
```

---

Deleting Deployment automatically removes child resources.

---

## Finalizers

Finalizers prevent deletion until cleanup is complete.

---

## Example

```yaml
metadata:
  finalizers:
  - kubernetes
```

---

## Workflow

```text id="k8s2a33"
Delete Request
      │
      ▼
Finalizer
      │
      ▼
Cleanup
      │
      ▼
Deletion
```

---

## Benefits

```text id="k8s2a34"
Controlled Cleanup
Data Protection
Resource Management
```

---

# 31. Services

Pods are ephemeral (temporary).

If a Pod dies and gets recreated, its IP address changes.

Services provide a stable endpoint for accessing Pods.

---

## Problem Without Service

```text id="k8s3a1"
Client
  │
  ▼
Pod IP

Pod Dies ❌

New Pod Created

New IP Assigned
```

---

## With Service

```text id="k8s3a2"
Client
  │
  ▼
Service
  │
  ▼
Pods
```

---

## Benefits

```text id="k8s3a3"
Stable IP
Load Balancing
Service Discovery
Decoupling
```

---

## Service Types

```text id="k8s3a4"
ClusterIP
NodePort
LoadBalancer
ExternalName
```

---

# 32. ClusterIP Service

Default Service type.

Accessible only inside the cluster.

---

## Architecture

```text id="k8s3a5"
Pod A
  │
  ▼
ClusterIP Service
  │
  ▼
Pod B
```

---

## YAML Example

```yaml id="k8s3a6"
apiVersion: v1
kind: Service

metadata:
  name: nginx-service

spec:

  type: ClusterIP

  selector:
    app: nginx

  ports:
  - port: 80
    targetPort: 80
```

---

## Create Service

```bash id="k8s3a7"
kubectl apply -f clusterip.yaml
```

---

## Verify

```bash id="k8s3a8"
kubectl get svc
```

---

## Use Cases

```text id="k8s3a9"
Microservices
Internal APIs
Backend Services
```

---

# 33. NodePort Service

Exposes application through a Node IP and Port.

---

## Architecture

```text id="k8s3a10"
Internet
    │
    ▼
NodeIP:30080
    │
    ▼
Service
    │
    ▼
Pods
```

---

## Port Range

```text id="k8s3a11"
30000-32767
```

---

## YAML Example

```yaml id="k8s3a12"
apiVersion: v1
kind: Service

metadata:
  name: nginx-nodeport

spec:

  type: NodePort

  selector:
    app: nginx

  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080
```

---

## Access Application

```text id="k8s3a13"
http://NODE_IP:30080
```

---

## Verify

```bash id="k8s3a14"
kubectl get svc
```

---

## Use Cases

```text id="k8s3a15"
Testing
Development
Lab Environments
```

---

# 34. LoadBalancer Service

Exposes application externally through a Cloud Load Balancer.

---

## Architecture

```text id="k8s3a16"
Internet
    │
    ▼
Load Balancer
    │
    ▼
Service
    │
    ▼
Pods
```

---

## YAML Example

```yaml id="k8s3a17"
apiVersion: v1
kind: Service

metadata:
  name: nginx-lb

spec:

  type: LoadBalancer

  selector:
    app: nginx

  ports:
  - port: 80
    targetPort: 80
```

---

## Verify

```bash id="k8s3a18"
kubectl get svc
```

---

Example:

```text id="k8s3a19"
EXTERNAL-IP
34.x.x.x
```

---

## Use Cases

```text id="k8s3a20"
Production Applications
Public APIs
Websites
```

---

# 35. ExternalName Service

Maps a Service to an external DNS name.

---

## Architecture

```text id="k8s3a21"
Application
     │
     ▼
ExternalName
     │
     ▼
External DNS
```

---

## YAML Example

```yaml id="k8s3a22"
apiVersion: v1
kind: Service

metadata:
  name: external-db

spec:

  type: ExternalName

  externalName: database.company.com
```

---

## Benefits

```text id="k8s3a23"
DNS Abstraction
Easy Migration
Simplified Configuration
```

---

# 36. Service Selectors

Services find Pods using labels.

---

## Architecture

```text id="k8s3a24"
Service
   │
Selector
   │
   ▼
Matching Pods
```

---

## Example

```yaml id="k8s3a25"
selector:
  app: nginx
```

---

## Verify Matching Pods

```bash id="k8s3a26"
kubectl get pods --show-labels
```

---

## Benefits

```text id="k8s3a27"
Dynamic Discovery
Loose Coupling
```

---

# 37. Endpoints

Endpoints represent actual Pod IPs behind Services.

---

## Architecture

```text id="k8s3a28"
Service
   │
   ▼
Endpoints
   │
   ▼
Pod IPs
```

---

## View Endpoints

```bash id="k8s3a29"
kubectl get endpoints
```

---

## Example

```text id="k8s3a30"
nginx-service

10.244.0.10
10.244.0.11
10.244.0.12
```

---

## Benefits

```text id="k8s3a31"
Load Balancing
Service Routing
```

---

# 38. DNS in Kubernetes

Every Kubernetes cluster includes DNS.

Usually provided by CoreDNS.

---

## Architecture

```text id="k8s3a32"
Pod
 │
 ▼
CoreDNS
 │
 ▼
Service
```

---

## Service DNS Format

```text id="k8s3a33"
service.namespace.svc.cluster.local
```

---

## Example

```text id="k8s3a34"
nginx.default.svc.cluster.local
```

---

## Verify DNS

```bash id="k8s3a35"
nslookup nginx-service
```

---

# 39. CoreDNS

CoreDNS is the DNS server inside Kubernetes.

---

## Architecture

```text id="k8s3a36"
Pods
 │
 ▼
CoreDNS
 │
 ▼
DNS Records
```

---

## Verify CoreDNS

```bash id="k8s3a37"
kubectl get pods -n kube-system
```

---

## Common Functions

```text id="k8s3a38"
Service Discovery
DNS Resolution
Cluster Networking
```

---

# 40. Service Discovery

Applications locate services through DNS.

---

## Without Service Discovery

```text id="k8s3a39"
Application
     │
     ▼
Hardcoded IP
```

---

## With Service Discovery

```text id="k8s3a40"
Application
     │
     ▼
DNS Name
```

---

## Benefits

```text id="k8s3a41"
Flexibility
Scalability
Maintainability
```

---

# 41. Ingress

Ingress provides HTTP and HTTPS routing.

---

## Problem

Without Ingress:

```text id="k8s3a42"
Service A → LoadBalancer

Service B → LoadBalancer

Service C → LoadBalancer
```

Multiple Load Balancers = Higher Cost.

---

## With Ingress

```text id="k8s3a43"
Internet
    │
    ▼
Ingress
 │     │
 ▼     ▼
AppA  AppB
```

---

## Benefits

```text id="k8s3a44"
Centralized Routing
SSL Termination
Lower Cost
```

---

# 42. Ingress Controllers

Ingress resources require an Ingress Controller.

---

## Popular Controllers

```text id="k8s3a45"
NGINX Ingress
Traefik
HAProxy
AWS ALB Controller
```

---

## Architecture

```text id="k8s3a46"
Ingress
   │
   ▼
Controller
   │
   ▼
Services
```

---

## Verify

```bash id="k8s3a47"
kubectl get pods -A
```

---

# 43. Ingress YAML Example

---

## Example

```yaml id="k8s3a48"
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: app-ingress

spec:

  rules:

  - host: app.example.com

    http:

      paths:

      - path: /

        pathType: Prefix

        backend:

          service:

            name: nginx-service

            port:

              number: 80
```

---

## Create Ingress

```bash id="k8s3a49"
kubectl apply -f ingress.yaml
```

---

## Verify

```bash id="k8s3a50"
kubectl get ingress
```

---

# 44. Network Policies

Network Policies control Pod-to-Pod communication.

Think of them as firewall rules for Pods.

---

## Architecture

```text id="k8s3a51"
Pod A
  │
NetworkPolicy
  │
  ▼
Pod B
```

---

## Benefits

```text id="k8s3a52"
Zero Trust Networking
Security
Isolation
```

---

# 45. Allow Traffic Policy

---

## YAML Example

```yaml id="k8s3a53"
apiVersion: networking.k8s.io/v1

kind: NetworkPolicy

metadata:
  name: allow-nginx

spec:

  podSelector:

    matchLabels:
      app: nginx

  policyTypes:
  - Ingress
```

---

## Create Policy

```bash id="k8s3a54"
kubectl apply -f policy.yaml
```

---

# 46. Deny All Traffic Policy

Very common interview question.

---

## YAML Example

```yaml id="k8s3a55"
apiVersion: networking.k8s.io/v1

kind: NetworkPolicy

metadata:
  name: deny-all

spec:

  podSelector: {}

  policyTypes:
  - Ingress
  - Egress
```

---

## Result

```text id="k8s3a56"
No Incoming Traffic

No Outgoing Traffic
```

---

# 47. Namespace Isolation

Network Policies can isolate namespaces.

---

## Architecture

```text id="k8s3a57"
Dev Namespace
      │
      ✖
      │
Prod Namespace
```

---

## Benefits

```text id="k8s3a58"
Security
Multi-Tenancy
Compliance
```

---

# 48. Pod Networking

Every Pod gets a unique IP.

---

## Kubernetes Networking Rules

```text id="k8s3a59"
Pod-to-Pod Communication

Node-to-Pod Communication

Pod-to-Service Communication

Without NAT
```

---

## Architecture

```text id="k8s3a60"
Pod A
  │
  ▼
Pod B
```

---

## Benefits

```text id="k8s3a61"
Simple Networking
Flat Network Model
```

---

# 49. Kubernetes Networking Flow

---

## Service Flow

```text id="k8s3a62"
Client
  │
  ▼
Service
  │
  ▼
Endpoints
  │
  ▼
Pods
```

---

## Ingress Flow

```text id="k8s3a63"
Internet
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

# 50. kube-proxy

kube-proxy manages Service networking.

---

## Architecture

```text id="k8s3a64"
Service
   │
   ▼
kube-proxy
   │
   ▼
  Pod
```

---

## Responsibilities

```text id="k8s3a65"
Load Balancing
Packet Forwarding
Service Discovery
```

---

## Verify kube-proxy

```bash id="k8s3a66"
kubectl get pods -n kube-system
```

---

## Common Commands

```bash id="k8s3a67"
kubectl get svc

kubectl get endpoints

kubectl get ingress

kubectl describe svc nginx-service

kubectl describe ingress app-ingress

kubectl get networkpolicy

kubectl describe networkpolicy
```

---

# 51. ConfigMaps

ConfigMaps store non-sensitive configuration data.

Instead of hardcoding configuration inside containers, Kubernetes stores it separately.

---

## Examples

```text id="k8s4a1"
Application URLs
Environment Variables
Feature Flags
Configuration Files
```

---

## Architecture

```text id="k8s4a2"
ConfigMap
    │
    ▼
Pod
```

---

## Benefits

```text id="k8s4a3"
Configuration Management
Reusability
Portability
```

---

## ConfigMap YAML

```yaml id="k8s4a4"
apiVersion: v1
kind: ConfigMap

metadata:
  name: app-config

data:
  APP_ENV: production
  APP_PORT: "8080"
```

---

## Create ConfigMap

```bash id="k8s4a5"
kubectl apply -f configmap.yaml
```

---

## Verify

```bash id="k8s4a6"
kubectl get configmaps
```

---

# 52. ConfigMap as Environment Variables

Inject ConfigMap values directly into containers.

---

## YAML Example

```yaml id="k8s4a7"
apiVersion: v1
kind: Pod

metadata:
  name: config-demo

spec:

  containers:

  - name: app

    image: nginx

    env:

    - name: APP_ENV

      valueFrom:

        configMapKeyRef:

          name: app-config

          key: APP_ENV
```

---

## Architecture

```text id="k8s4a8"
ConfigMap
    │
    ▼
Environment Variable
    │
    ▼
Container
```

---

# 53. ConfigMap as Mounted Files

ConfigMaps can be mounted as files.

---

## Architecture

```text id="k8s4a9"
ConfigMap
    │
    ▼
Volume
    │
    ▼
Container
```

---

## YAML Example

```yaml id="k8s4a10"
volumes:

- name: config

  configMap:

    name: app-config
```

---

## Benefits

```text id="k8s4a11"
Externalized Configuration
Dynamic Updates
```

---

# 54. Secrets

Secrets store sensitive information.

---

## Examples

```text id="k8s4a12"
Passwords
API Keys
Certificates
Tokens
```

---

## Architecture

```text id="k8s4a13"
Secret
  │
  ▼
Pod
```

---

## Benefits

```text id="k8s4a14"
Security
Confidentiality
Access Control
```

---

# 55. Opaque Secrets

Most commonly used Secret type.

---

## Create Secret YAML

```yaml id="k8s4a15"
apiVersion: v1
kind: Secret

metadata:
  name: db-secret

type: Opaque

data:
  username: YWRtaW4=
  password: cGFzc3dvcmQ=
```

---

## Important

Values must be Base64 encoded.

---

## Encode Values

```bash id="k8s4a16"
echo -n admin | base64

echo -n password | base64
```

---

# 56. TLS Secrets

Used for SSL/TLS certificates.

---

## Architecture

```text id="k8s4a17"
TLS Secret
     │
     ▼
Ingress
     │
     ▼
HTTPS
```

---

## Create TLS Secret

```bash id="k8s4a18"
kubectl create secret tls tls-secret \
--cert=cert.crt \
--key=key.key
```

---

## Use Cases

```text id="k8s4a19"
HTTPS Websites
Ingress Controllers
SSL Termination
```

---

# 57. Docker Registry Secrets

Used to pull images from private registries.

---

## Architecture

```text id="k8s4a20"
Private Registry
        │
        ▼
Registry Secret
        │
        ▼
Pod
```

---

## Create Secret

```bash id="k8s4a21"
kubectl create secret docker-registry regcred \
--docker-server=myregistry.com \
--docker-username=user \
--docker-password=password
```

---

## Example Usage

```yaml id="k8s4a22"
imagePullSecrets:

- name: regcred
```

---

# 58. Resource Management

Kubernetes allows resource control.

---

## Resources

```text id="k8s4a23"
CPU
Memory
Ephemeral Storage
```

---

## Why Important?

```text id="k8s4a24"
Prevent Resource Starvation
Predictable Performance
Cluster Stability
```

---

# 59. Requests

Requests define minimum required resources.

---

## Architecture

```text id="k8s4a25"
Container
     │
     ▼
Request Resources
```

---

## YAML Example

```yaml id="k8s4a26"
resources:

  requests:

    memory: "128Mi"

    cpu: "250m"
```

---

## Scheduler Behavior

```text id="k8s4a27"
Scheduler uses Requests

to choose Nodes
```

---

# 60. Limits

Limits define maximum resources a container can consume.

---

## YAML Example

```yaml id="k8s4a28"
resources:

  limits:

    memory: "512Mi"

    cpu: "1000m"
```

---

## Architecture

```text id="k8s4a29"
Container
     │
     ▼
Maximum Resource Usage
```

---

## Benefits

```text id="k8s4a30"
Prevent Abuse
Protect Cluster
```

---

# 61. Requests vs Limits

Common interview question.

---

## Comparison

| Feature              | Request | Limit   |
| -------------------- | ------- | ------- |
| Purpose              | Minimum | Maximum |
| Used By Scheduler    | Yes     | No      |
| Guarantees Resources | Yes     | No      |
| Restricts Usage      | No      | Yes     |

---

## Example

```yaml id="k8s4a31"
resources:

  requests:
    cpu: "250m"
    memory: "256Mi"

  limits:
    cpu: "500m"
    memory: "512Mi"
```

---

# 62. QoS Classes

QoS = Quality of Service

Determines eviction priority.

---

## Classes

```text id="k8s4a32"
Guaranteed
Burstable
BestEffort
```

---

## Guaranteed

Requests = Limits

```yaml id="k8s4a33"
requests:
  cpu: "500m"

limits:
  cpu: "500m"
```

---

## Burstable

Requests < Limits

```yaml id="k8s4a34"
requests:
  cpu: "250m"

limits:
  cpu: "500m"
```

---

## BestEffort

No Requests
No Limits

```yaml id="k8s4a35"
containers:
- name: app
  image: nginx
```

---

# 63. Volumes

Volumes provide storage for Pods.

---

## Problem

Container Filesystem is temporary.

When Pod dies:

```text id="k8s4a36"
Data Lost ❌
```

---

## Solution

```text id="k8s4a37"
Volumes
```

---

## Architecture

```text id="k8s4a38"
Pod
 │
 ▼
Volume
```

---

# 64. emptyDir Volume

Temporary storage.

Created when Pod starts.

Deleted when Pod is removed.

---

## Architecture

```text id="k8s4a39"
Pod
 │
 ▼
emptyDir
```

---

## YAML Example

```yaml id="k8s4a40"
volumes:

- name: cache

  emptyDir: {}
```

---

## Use Cases

```text id="k8s4a41"
Cache
Temporary Files
Scratch Space
```

---

# 65. hostPath Volume

Mounts a directory from the Node.

---

## Architecture

```text id="k8s4a42"
Node Filesystem
       │
       ▼
hostPath
       │
       ▼
Pod
```

---

## YAML Example

```yaml id="k8s4a43"
volumes:

- name: host-volume

  hostPath:

    path: /data
```

---

## Warning

```text id="k8s4a44"
Not Recommended
for Production
```

---

# 66. Persistent Storage

Persistent Storage survives Pod restarts.

---

## Architecture

```text id="k8s4a45"
Pod
 │
 ▼
Persistent Storage
```

---

## Components

```text id="k8s4a46"
PV
PVC
StorageClass
```

---

## Benefits

```text id="k8s4a47"
Data Durability
Reliability
```

---

# 67. Persistent Volumes (PV)

PV is actual storage resource.

---

## Architecture

```text id="k8s4a48"
Persistent Volume
       │
       ▼
Storage
```

---

## PV YAML

```yaml id="k8s4a49"
apiVersion: v1
kind: PersistentVolume

metadata:
  name: pv-demo

spec:

  capacity:
    storage: 5Gi

  accessModes:
  - ReadWriteOnce

  hostPath:
    path: /data
```

---

## Verify

```bash id="k8s4a50"
kubectl get pv
```

---

# 68. Persistent Volume Claims (PVC)

PVC requests storage.

---

## Architecture

```text id="k8s4a51"
Pod
 │
 ▼
PVC
 │
 ▼
PV
```

---

## PVC YAML

```yaml id="k8s4a52"
apiVersion: v1
kind: PersistentVolumeClaim

metadata:
  name: pvc-demo

spec:

  accessModes:
  - ReadWriteOnce

  resources:

    requests:

      storage: 2Gi
```

---

## Verify

```bash id="k8s4a53"
kubectl get pvc
```

---

# 69. Access Modes

Interview favorite.

---

## ReadWriteOnce (RWO)

```text id="k8s4a54"
Single Node
Read + Write
```

---

## ReadOnlyMany (ROX)

```text id="k8s4a55"
Multiple Nodes
Read Only
```

---

## ReadWriteMany (RWX)

```text id="k8s4a56"
Multiple Nodes
Read + Write
```

---

## Comparison

| Access Mode | Multiple Nodes | Write Access |
| ----------- | -------------- | ------------ |
| RWO         | No             | Yes          |
| ROX         | Yes            | No           |
| RWX         | Yes            | Yes          |

---

# 70. Pod Using PVC

---

## Complete YAML Example

```yaml id="k8s4a57"
apiVersion: v1
kind: Pod

metadata:
  name: storage-pod

spec:

  containers:

  - name: app

    image: nginx

    volumeMounts:

    - mountPath: "/data"

      name: storage

  volumes:

  - name: storage

    persistentVolumeClaim:

      claimName: pvc-demo
```

---

## Verify Mount

```bash id="k8s4a58"
kubectl describe pod storage-pod
```

---

## Important Commands

```bash id="k8s4a59"
kubectl get configmaps

kubectl get secrets

kubectl describe secret

kubectl get pv

kubectl get pvc

kubectl describe pvc

kubectl get storageclass

kubectl top pods

kubectl top nodes
```

---

# 71. Storage Classes

StorageClass enables dynamic storage provisioning.

Instead of manually creating PVs, Kubernetes automatically provisions storage.

---

## Architecture

```text id="k8s5a1"
PVC
 │
 ▼
StorageClass
 │
 ▼
PV
 │
 ▼
Storage
```

---

## Benefits

```text id="k8s5a2"
Automation
Dynamic Provisioning
Cloud Integration
```

---

## View Storage Classes

```bash id="k8s5a3"
kubectl get storageclass
```

---

## StorageClass YAML

```yaml id="k8s5a4"
apiVersion: storage.k8s.io/v1
kind: StorageClass

metadata:
  name: fast-storage

provisioner: kubernetes.io/aws-ebs

parameters:
  type: gp3
```

---

# 72. Dynamic Provisioning

Storage is automatically created when a PVC is requested.

---

## Traditional Flow

```text id="k8s5a5"
Admin Creates PV
      │
      ▼
User Creates PVC
```

---

## Dynamic Flow

```text id="k8s5a6"
PVC
 │
 ▼
StorageClass
 │
 ▼
PV Created Automatically
```

---

## Benefits

```text id="k8s5a7"
Automation
Scalability
Reduced Administration
```

---

# 73. CSI (Container Storage Interface)

CSI provides a standard interface for storage systems.

---

## Architecture

```text id="k8s5a8"
Kubernetes
     │
     ▼
CSI Driver
     │
     ▼
Storage Platform
```

---

## Common CSI Drivers

```text id="k8s5a9"
AWS EBS CSI
AWS EFS CSI
Azure Disk CSI
Azure Files CSI
GCE PD CSI
Ceph CSI
```

---

## Benefits

```text id="k8s5a10"
Vendor Independence
Standardization
Extensibility
```

---

# 74. Volume Modes

Volume Modes define how storage is presented.

---

## Types

```text id="k8s5a11"
Filesystem
Block
```

---

## Filesystem Mode

```text id="k8s5a12"
Storage
   │
   ▼
Filesystem
   │
   ▼
Pod
```

---

## Block Mode

```text id="k8s5a13"
Storage
   │
   ▼
Raw Block Device
   │
   ▼
Pod
```

---

## Use Cases

```text id="k8s5a14"
Filesystem → Most Applications

Block → Databases
```

---

# 75. StatefulSets

StatefulSets manage stateful applications.

---

## Examples

```text id="k8s5a15"
MySQL
PostgreSQL
MongoDB
Kafka
Elasticsearch
```

---

## Deployment vs StatefulSet

| Deployment      | StatefulSet     |
| --------------- | --------------- |
| Stateless       | Stateful        |
| Random Names    | Stable Names    |
| Shared Identity | Unique Identity |

---

## Architecture

```text id="k8s5a16"
StatefulSet
     │
 ┌───┼───┐
 ▼   ▼   ▼
db-0 db-1 db-2
```

---

## Benefits

```text id="k8s5a17"
Persistent Identity
Stable Storage
Ordered Deployment
```

---

# 76. StatefulSet YAML

---

## Example

```yaml id="k8s5a18"
apiVersion: apps/v1
kind: StatefulSet

metadata:
  name: mysql

spec:

  serviceName: mysql

  replicas: 3

  selector:
    matchLabels:
      app: mysql

  template:

    metadata:
      labels:
        app: mysql

    spec:

      containers:

      - name: mysql

        image: mysql:8
```

---

## Verify

```bash id="k8s5a19"
kubectl get statefulsets
```

---

# 77. StatefulSet Characteristics

---

## Stable Identity

```text id="k8s5a20"
mysql-0
mysql-1
mysql-2
```

---

## Ordered Deployment

```text id="k8s5a21"
mysql-0

then

mysql-1

then

mysql-2
```

---

## Ordered Deletion

```text id="k8s5a22"
mysql-2

then

mysql-1

then

mysql-0
```

---

# 78. DaemonSets

DaemonSets run one Pod on every Node.

---

## Common Use Cases

```text id="k8s5a23"
Fluentd
Prometheus Node Exporter
Datadog Agent
Security Agents
```

---

## Architecture

```text id="k8s5a24"
Node1 → Pod

Node2 → Pod

Node3 → Pod
```

---

## Benefits

```text id="k8s5a25"
Node Monitoring
Logging
Security
```

---

# 79. DaemonSet YAML

---

## Example

```yaml id="k8s5a26"
apiVersion: apps/v1
kind: DaemonSet

metadata:
  name: node-exporter

spec:

  selector:
    matchLabels:
      app: exporter

  template:

    metadata:
      labels:
        app: exporter

    spec:

      containers:

      - name: exporter

        image: prom/node-exporter
```

---

## Verify

```bash id="k8s5a27"
kubectl get daemonsets
```

---

# 80. Jobs

Jobs execute tasks once and then exit.

---

## Examples

```text id="k8s5a28"
Database Migration
Backup Jobs
Report Generation
Batch Processing
```

---

## Architecture

```text id="k8s5a29"
Job
 │
 ▼
Pod
 │
 ▼
Complete
```

---

## Benefits

```text id="k8s5a30"
One-Time Execution
Retry Support
Batch Processing
```

---

# 81. Job YAML

---

## Example

```yaml id="k8s5a31"
apiVersion: batch/v1
kind: Job

metadata:
  name: hello-job

spec:

  template:

    spec:

      containers:

      - name: hello

        image: busybox

        command:
        - echo
        - "Hello Kubernetes"

      restartPolicy: Never
```

---

## Verify

```bash id="k8s5a32"
kubectl get jobs
```

---

# 82. CronJobs

CronJobs schedule Jobs.

---

## Similar To Linux Cron

```text id="k8s5a33"
0 2 * * *
```

Run every day at 2 AM.

---

## Architecture

```text id="k8s5a34"
CronJob
    │
    ▼
Job
    │
    ▼
Pod
```

---

## Benefits

```text id="k8s5a35"
Scheduling
Automation
Recurring Tasks
```

---

# 83. CronJob YAML

---

## Example

```yaml id="k8s5a36"
apiVersion: batch/v1
kind: CronJob

metadata:
  name: backup-job

spec:

  schedule: "0 2 * * *"

  jobTemplate:

    spec:

      template:

        spec:

          containers:

          - name: backup

            image: busybox

            command:
            - echo
            - "Backup Started"

          restartPolicy: OnFailure
```

---

## Verify

```bash id="k8s5a37"
kubectl get cronjobs
```

---

# 84. Scheduling Fundamentals

The Scheduler decides where Pods run.

---

## Architecture

```text id="k8s5a38"
Pod
 │
 ▼
Scheduler
 │
 ▼
Node
```

---

## Factors Considered

```text id="k8s5a39"
CPU
Memory
Affinity
Taints
Topology
```

---

## Benefits

```text id="k8s5a40"
Efficient Resource Usage
Balanced Workloads
```

---

# 85. nodeSelector

Simplest scheduling mechanism.

---

## Architecture

```text id="k8s5a41"
Pod
 │
 ▼
nodeSelector
 │
 ▼
Matching Node
```

---

## Label Node

```bash id="k8s5a42"
kubectl label node worker1 disk=ssd
```

---

## YAML Example

```yaml id="k8s5a43"
spec:

  nodeSelector:

    disk: ssd
```

---

## Verify

```bash id="k8s5a44"
kubectl get nodes --show-labels
```

---

# 86. Node Affinity

More powerful than nodeSelector.

---

## Architecture

```text id="k8s5a45"
Pod
 │
 ▼
Affinity Rules
 │
 ▼
Node
```

---

## YAML Example

```yaml id="k8s5a46"
affinity:

  nodeAffinity:

    requiredDuringSchedulingIgnoredDuringExecution:

      nodeSelectorTerms:

      - matchExpressions:

        - key: disk

          operator: In

          values:

          - ssd
```

---

## Benefits

```text id="k8s5a47"
Flexible Scheduling
Advanced Placement
```

---

# 87. Pod Affinity

Place Pods together.

---

## Architecture

```text id="k8s5a48"
App Pod
   │
   ▼
Database Pod
```

---

## Use Cases

```text id="k8s5a49"
Reduce Latency
Improve Communication
```

---

# 88. Pod Anti-Affinity

Keep Pods apart.

---

## Architecture

```text id="k8s5a50"
Pod A

Node 1

Pod B

Node 2
```

---

## Benefits

```text id="k8s5a51"
High Availability
Fault Tolerance
```

---

# 89. Taints and Tolerations

Control which Pods can run on Nodes.

---

## Architecture

```text id="k8s5a52"
Node
 │
 ▼
Taint
 │
 ▼
Reject Pods
```

---

## Add Taint

```bash id="k8s5a53"
kubectl taint nodes worker1 \
gpu=true:NoSchedule
```

---

## Toleration Example

```yaml id="k8s5a54"
tolerations:

- key: gpu

  operator: Equal

  value: "true"

  effect: NoSchedule
```

---

## Benefits

```text id="k8s5a55"
Dedicated Nodes
Workload Isolation
```

---

# 90. Scheduler Workflow

---

## Complete Flow

```text id="k8s5a56"
Pod Created
      │
      ▼
API Server
      │
      ▼
Scheduler
      │
      ▼
Node Selection
      │
      ▼
kubelet
      │
      ▼
Pod Running
```

---

## Important Commands

```bash id="k8s5a57"
kubectl get storageclass

kubectl get pv

kubectl get pvc

kubectl get statefulsets

kubectl get daemonsets

kubectl get jobs

kubectl get cronjobs

kubectl describe pod POD_NAME

kubectl get nodes

kubectl get nodes --show-labels
```

---

# 91. Authentication

Authentication verifies who is accessing the cluster.

---

## Architecture

```text id="k8s6a1"
User
 │
 ▼
Authentication
 │
 ▼
API Server
```

---

## Authentication Methods

```text id="k8s6a2"
Certificates
Service Accounts
OIDC
Bearer Tokens
OpenID Connect
```

---

## Authentication Flow

```text id="k8s6a3"
User
 │
 ▼
Certificate
 │
 ▼
API Server
 │
 ▼
Access Granted
```

---

## Benefits

```text id="k8s6a4"
Identity Verification
Security
Access Control
```

---

# 92. Authorization

Authorization determines what a user can do.

---

## Architecture

```text id="k8s6a5"
Authentication
      │
      ▼
Authorization
      │
      ▼
Allow / Deny
```

---

## Common Authorization Modes

```text id="k8s6a6"
RBAC
ABAC
Webhook
Node
```

---

## Example

```text id="k8s6a7"
User Authenticated ✓

Can Create Pods? ❓

Authorization Checks
```

---

# 93. RBAC Overview

RBAC = Role Based Access Control

Most widely used authorization mechanism.

---

## Architecture

```text id="k8s6a8"
User
 │
 ▼
Role
 │
 ▼
Permissions
```

---

## Components

```text id="k8s6a9"
Role
ClusterRole
RoleBinding
ClusterRoleBinding
```

---

## Benefits

```text id="k8s6a10"
Least Privilege
Security
Governance
```

---

# 94. Roles

Roles define permissions inside a namespace.

---

## Architecture

```text id="k8s6a11"
Role
 │
 ▼
Permissions
 │
 ▼
Namespace
```

---

## Role YAML

```yaml id="k8s6a12"
apiVersion: rbac.authorization.k8s.io/v1
kind: Role

metadata:
  namespace: dev
  name: pod-reader

rules:

- apiGroups: [""]

  resources:
  - pods

  verbs:
  - get
  - list
  - watch
```

---

## Verify

```bash id="k8s6a13"
kubectl get roles
```

---

# 95. ClusterRoles

ClusterRoles provide cluster-wide permissions.

---

## Architecture

```text id="k8s6a14"
ClusterRole
      │
      ▼
Entire Cluster
```

---

## Example

```yaml id="k8s6a15"
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole

metadata:
  name: pod-reader

rules:

- apiGroups: [""]

  resources:
  - pods

  verbs:
  - get
  - list
```

---

## Verify

```bash id="k8s6a16"
kubectl get clusterroles
```

---

# 96. RoleBindings

RoleBindings assign Roles to users.

---

## Architecture

```text id="k8s6a17"
User
 │
 ▼
RoleBinding
 │
 ▼
Role
```

---

## YAML Example

```yaml id="k8s6a18"
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding

metadata:
  name: read-pods

subjects:

- kind: User

  name: john

roleRef:

  kind: Role

  name: pod-reader

  apiGroup: rbac.authorization.k8s.io
```

---

## Benefits

```text id="k8s6a19"
Permission Assignment
Namespace Security
```

---

# 97. ClusterRoleBindings

Assign ClusterRoles cluster-wide.

---

## Architecture

```text id="k8s6a20"
User
 │
 ▼
ClusterRoleBinding
 │
 ▼
ClusterRole
```

---

## YAML Example

```yaml id="k8s6a21"
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding

metadata:
  name: cluster-admin-binding

subjects:

- kind: User

  name: admin

roleRef:

  kind: ClusterRole

  name: cluster-admin

  apiGroup: rbac.authorization.k8s.io
```

---

## Verify

```bash id="k8s6a22"
kubectl get clusterrolebindings
```

---

# 98. Service Accounts

Pods use Service Accounts to communicate with Kubernetes.

---

## Architecture

```text id="k8s6a23"
Pod
 │
 ▼
ServiceAccount
 │
 ▼
API Server
```

---

## Default Service Account

Every namespace gets one.

```text id="k8s6a24"
default
```

---

## Verify

```bash id="k8s6a25"
kubectl get sa
```

---

# 99. ServiceAccount YAML

---

## Example

```yaml id="k8s6a26"
apiVersion: v1
kind: ServiceAccount

metadata:
  name: app-sa
```

---

## Use ServiceAccount

```yaml id="k8s6a27"
spec:

  serviceAccountName: app-sa
```

---

## Benefits

```text id="k8s6a28"
Pod Identity
Fine-Grained Access
Security
```

---

# 100. Admission Controllers

Admission Controllers intercept API requests.

---

## Architecture

```text id="k8s6a29"
Request
  │
  ▼
API Server
  │
  ▼
Admission Controller
  │
  ▼
Stored In etcd
```

---

## Responsibilities

```text id="k8s6a30"
Validation
Mutation
Policy Enforcement
```

---

# 101. Mutating Admission Controllers

Modify objects before creation.

---

## Example

```text id="k8s6a31"
Add Labels

Inject Sidecars

Modify Resources
```

---

## Architecture

```text id="k8s6a32"
Request
 │
 ▼
Mutating Controller
 │
 ▼
Modified Object
```

---

# 102. Validating Admission Controllers

Validate requests.

---

## Example

```text id="k8s6a33"
Deny Privileged Pods

Enforce Naming Rules

Check Security Policies
```

---

## Architecture

```text id="k8s6a34"
Request
 │
 ▼
Validation
 │
 ▼
Allow / Deny
```

---

# 103. Pod Security Standards (PSS)

Pod Security Standards replace Pod Security Policies.

---

## Levels

```text id="k8s6a35"
Privileged
Baseline
Restricted
```

---

## Architecture

```text id="k8s6a36"
Pod
 │
 ▼
Security Policy
 │
 ▼
Allowed / Denied
```

---

## Benefits

```text id="k8s6a37"
Security Hardening
Compliance
```

---

# 104. Security Contexts

Security Context defines container security settings.

---

## Architecture

```text id="k8s6a38"
Container
    │
    ▼
Security Context
```

---

## Common Settings

```text id="k8s6a39"
runAsUser
runAsGroup
fsGroup
Capabilities
```

---

## YAML Example

```yaml id="k8s6a40"
securityContext:

  runAsUser: 1000

  runAsNonRoot: true
```

---

# 105. Linux Capabilities

Capabilities define container privileges.

---

## Examples

```text id="k8s6a41"
NET_ADMIN
SYS_TIME
SYS_ADMIN
```

---

## Drop All Capabilities

```yaml id="k8s6a42"
securityContext:

  capabilities:

    drop:
    - ALL
```

---

## Add Specific Capability

```yaml id="k8s6a43"
securityContext:

  capabilities:

    add:
    - NET_ADMIN
```

---

# 106. Secrets Management Best Practices

---

## Recommendations

```text id="k8s6a44"
Never Store Secrets In Git

Use Encryption At Rest

Use External Secret Stores

Rotate Credentials
```

---

## Enterprise Solutions

```text id="k8s6a45"
HashiCorp Vault
External Secrets Operator
AWS Secrets Manager
Azure Key Vault
```

---

# 107. Image Security

Container images are a major attack vector.

---

## Best Practices

```text id="k8s6a46"
Use Trusted Images

Use Minimal Images

Scan Images

Sign Images
```

---

## Common Tools

```text id="k8s6a47"
Trivy
Grype
Clair
Docker Scout
```

---

# 108. Supply Chain Security

Protect the entire software delivery pipeline.

---

## Architecture

```text id="k8s6a48"
Source Code
    │
    ▼
Build
    │
    ▼
Container Image
    │
    ▼
Registry
    │
    ▼
Cluster
```

---

## Security Measures

```text id="k8s6a49"
Image Signing
SBOM
Vulnerability Scanning
Policy Enforcement
```

---

# 109. Network Security

Protect Pod communication.

---

## Layers

```text id="k8s6a50"
Network Policies

Service Mesh

mTLS
```

---

## Architecture

```text id="k8s6a51"
Pod A
  │
 mTLS
  │
  ▼
Pod B
```

---

# 110. Kubernetes Security Workflow

```text id="k8s6a52"
Authentication
      │
      ▼
Authorization
      │
      ▼
Admission Control
      │
      ▼
Pod Security
      │
      ▼
Network Security
      │
      ▼
Runtime Security
```

---

## Important Commands

```bash id="k8s6a53"
kubectl get roles

kubectl get rolebindings

kubectl get clusterroles

kubectl get clusterrolebindings

kubectl get sa

kubectl auth can-i create pods

kubectl auth can-i delete deployments

kubectl describe role ROLE_NAME

kubectl describe clusterrole ROLE_NAME
```

---

# 111. Logging in Kubernetes

Logs are the first place to check when troubleshooting applications.

---

## Architecture

```text id="k8s7a1"
Application
     │
     ▼
Container Logs
     │
     ▼
kubectl logs
```

---

## View Logs

```bash id="k8s7a2"
kubectl logs POD_NAME
```

---

## Example

```bash id="k8s7a3"
kubectl logs nginx-pod
```

---

## Benefits

```text id="k8s7a4"
Troubleshooting
Monitoring
Auditing
```

---

# 112. Multi-Container Pod Logs

Specify the container name.

---

## Command

```bash id="k8s7a5"
kubectl logs POD_NAME -c CONTAINER_NAME
```

---

## Example

```bash id="k8s7a6"
kubectl logs multi-container -c nginx
```

---

## Architecture

```text id="k8s7a7"
Pod
 │
 ├── Container A
 │
 └── Container B
```

---

# 113. Follow Live Logs

Equivalent to Linux tail -f.

---

## Command

```bash id="k8s7a8"
kubectl logs -f POD_NAME
```

---

## Example

```bash id="k8s7a9"
kubectl logs -f nginx-pod
```

---

## Use Cases

```text id="k8s7a10"
Real-Time Monitoring
Debugging
Production Support
```

---

# 114. Previous Container Logs

Useful when containers restart.

---

## Command

```bash id="k8s7a11"
kubectl logs --previous POD_NAME
```

---

## Example

```bash id="k8s7a12"
kubectl logs --previous app-pod
```

---

## Common Use Cases

```text id="k8s7a13"
CrashLoopBackOff
Application Crashes
Startup Failures
```

---

# 115. Cluster Logging Architecture

Production clusters use centralized logging.

---

## Architecture

```text id="k8s7a14"
Pods
 │
 ▼
Log Agent
 │
 ▼
Log Store
 │
 ▼
Dashboard
```

---

## Popular Solutions

```text id="k8s7a15"
EFK Stack

Elasticsearch
Fluentd
Kibana

Loki
Promtail
Grafana
```

---

# 116. Events

Events show what Kubernetes is doing.

---

## View Events

```bash id="k8s7a16"
kubectl get events
```

---

## Sort By Time

```bash id="k8s7a17"
kubectl get events \
--sort-by=.metadata.creationTimestamp
```

---

## Examples

```text id="k8s7a18"
Scheduled
Pulled
Created
Started
Failed
```

---

# 117. Monitoring Fundamentals

Monitoring tracks cluster health.

---

## Architecture

```text id="k8s7a19"
Cluster
 │
 ▼
Metrics
 │
 ▼
Monitoring System
```

---

## Metrics Categories

```text id="k8s7a20"
CPU
Memory
Network
Storage
Application Metrics
```

---

# 118. Metrics Server

Metrics Server provides basic cluster metrics.

---

## Architecture

```text id="k8s7a21"
Nodes
 │
 ▼
Metrics Server
 │
 ▼
kubectl top
```

---

## Commands

```bash id="k8s7a22"
kubectl top nodes

kubectl top pods
```

---

## Benefits

```text id="k8s7a23"
Resource Visibility
Autoscaling Support
```

---

# 119. Prometheus

Prometheus is the most popular Kubernetes monitoring solution.

---

## Architecture

```text id="k8s7a24"
Pods
 │
 ▼
Prometheus
 │
 ▼
Metrics Database
```

---

## Responsibilities

```text id="k8s7a25"
Metrics Collection
Alerting
Monitoring
```

---

## Benefits

```text id="k8s7a26"
Cloud Native
Highly Scalable
Open Source
```

---

# 120. Grafana

Grafana visualizes metrics.

---

## Architecture

```text id="k8s7a27"
Prometheus
     │
     ▼
Grafana
     │
     ▼
Dashboards
```

---

## Common Dashboards

```text id="k8s7a28"
Node Metrics
Pod Metrics
Cluster Metrics
Application Metrics
```

---

# 121. Monitoring Stack

Most common production stack.

---

## Architecture

```text id="k8s7a29"
Node Exporter
      │
      ▼
Prometheus
      │
      ▼
Grafana
      │
      ▼
Users
```

---

## Components

```text id="k8s7a30"
Prometheus
Grafana
Alertmanager
Node Exporter
```

---

# 122. Health Checks

Health Checks determine application health.

---

## Types

```text id="k8s7a31"
Liveness Probe
Readiness Probe
Startup Probe
```

---

## Benefits

```text id="k8s7a32"
Self Healing
Reliable Deployments
```

---

# 123. Liveness Probes

Checks whether application is alive.

---

## Architecture

```text id="k8s7a33"
Probe Fails
     │
     ▼
Restart Container
```

---

## YAML Example

```yaml id="k8s7a34"
livenessProbe:

  httpGet:

    path: /

    port: 80

  initialDelaySeconds: 10

  periodSeconds: 5
```

---

## Benefits

```text id="k8s7a35"
Automatic Recovery
Crash Detection
```

---

# 124. Readiness Probes

Checks whether application can receive traffic.

---

## Architecture

```text id="k8s7a36"
Ready
  │
  ▼
Receive Traffic

Not Ready
    │
    ▼
No Traffic
```

---

## YAML Example

```yaml id="k8s7a37"
readinessProbe:

  httpGet:

    path: /

    port: 80

  initialDelaySeconds: 5
```

---

## Benefits

```text id="k8s7a38"
Safe Deployments
Traffic Control
```

---

# 125. Startup Probes

Used for slow-starting applications.

---

## Architecture

```text id="k8s7a39"
Application Starting
         │
         ▼
Startup Probe
         │
         ▼
Success
         │
         ▼
Liveness Starts
```

---

## YAML Example

```yaml id="k8s7a40"
startupProbe:

  httpGet:

    path: /

    port: 80

  failureThreshold: 30

  periodSeconds: 10
```

---

## Benefits

```text id="k8s7a41"
Prevent False Restarts
Support Slow Applications
```

---

# 126. OpenTelemetry

OpenTelemetry is the standard observability framework.

---

## Signals

```text id="k8s7a42"
Metrics
Logs
Traces
```

---

## Architecture

```text id="k8s7a43"
Application
     │
     ▼
OpenTelemetry
     │
     ▼
Observability Platform
```

---

## Benefits

```text id="k8s7a44"
Unified Observability
Vendor Neutral
```

---

# 127. Distributed Tracing

Tracks requests across services.

---

## Architecture

```text id="k8s7a45"
User Request
      │
      ▼
Service A
      │
      ▼
Service B
      │
      ▼
Database
```

---

## Benefits

```text id="k8s7a46"
Performance Analysis
Root Cause Detection
```

---

# 128. Jaeger

Popular distributed tracing system.

---

## Architecture

```text id="k8s7a47"
Application
     │
     ▼
Jaeger Agent
     │
     ▼
Jaeger UI
```

---

## Benefits

```text id="k8s7a48"
Request Tracing
Latency Analysis
```

---

# 129. Alertmanager

Prometheus Alertmanager handles alerts.

---

## Architecture

```text id="k8s7a49"
Prometheus
     │
     ▼
Alertmanager
     │
     ▼
Email
Slack
PagerDuty
```

---

## Benefits

```text id="k8s7a50"
Alert Routing
Deduplication
Notifications
```

---

# 130. SLI, SLO, and SLA

Important SRE concepts.

---

## SLI

Service Level Indicator

```text id="k8s7a51"
Actual Measurement
```

Example:

```text id="k8s7a52"
99.95% Availability
```

---

## SLO

Service Level Objective

```text id="k8s7a53"
Target Goal
```

Example:

```text id="k8s7a54"
99.9% Availability
```

---

## SLA

Service Level Agreement

```text id="k8s7a55"
Business Contract
```

Example:

```text id="k8s7a56"
99.5% Guaranteed Uptime
```

---

## Relationship

```text id="k8s7a57"
SLI
 │
 ▼
SLO
 │
 ▼
SLA
```

---

## Important Commands

```bash id="k8s7a58"
kubectl logs POD_NAME

kubectl logs -f POD_NAME

kubectl logs --previous POD_NAME

kubectl top pods

kubectl top nodes

kubectl get events

kubectl describe pod POD_NAME

kubectl get pods

kubectl get svc
```

---

# 131. CNI (Container Network Interface)

CNI is the networking layer of Kubernetes.

It provides Pod networking.

Without a CNI plugin:

```text id="k8s8a1"
Pods Cannot Communicate
```

---

## Architecture

```text id="k8s8a2"
Kubernetes
     │
     ▼
CNI Plugin
     │
     ▼
Pod Networking
```

---

## Popular CNI Plugins

```text id="k8s8a3"
Calico
Cilium
Flannel
Weave
Canal
AWS VPC CNI
Azure CNI
```

---

## Responsibilities

```text id="k8s8a4"
Assign Pod IPs
Route Traffic
Network Policies
```

---

# 132. Pod Networking Deep Dive

Every Pod gets its own IP address.

---

## Rules

```text id="k8s8a5"
Pod-to-Pod Communication

Node-to-Pod Communication

Without NAT
```

---

## Architecture

```text id="k8s8a6"
Pod A
  │
  ▼
Pod B
```

---

## Benefits

```text id="k8s8a7"
Flat Network
Simple Communication
```

---

# 133. Calico

Calico is one of the most popular Kubernetes networking solutions.

---

## Features

```text id="k8s8a8"
Networking
Network Policies
BGP Routing
Security
```

---

## Architecture

```text id="k8s8a9"
Pods
 │
 ▼
Calico
 │
 ▼
Network Fabric
```

---

## Benefits

```text id="k8s8a10"
Scalable
Secure
Production Ready
```

---

# 134. Cilium

Cilium uses eBPF for networking and security.

---

## Architecture

```text id="k8s8a11"
Pods
 │
 ▼
eBPF
 │
 ▼
Cilium
```

---

## Features

```text id="k8s8a12"
High Performance
Observability
Security
```

---

## Benefits

```text id="k8s8a13"
Modern Networking
Deep Visibility
Low Overhead
```

---

# 135. Flannel

Flannel provides simple overlay networking.

---

## Architecture

```text id="k8s8a14"
Node A
  │
Overlay Network
  │
Node B
```

---

## Benefits

```text id="k8s8a15"
Simple
Easy Setup
Lightweight
```

---

## Limitations

```text id="k8s8a16"
No Network Policies
Limited Features
```

---

# 136. Service Mesh

Service Mesh manages service-to-service communication.

---

## Problems Solved

```text id="k8s8a17"
Encryption
Traffic Routing
Observability
Retries
```

---

## Architecture

```text id="k8s8a18"
Service A
    │
 Sidecar Proxy
    │
 Service Mesh
    │
 Sidecar Proxy
    │
Service B
```

---

## Popular Service Meshes

```text id="k8s8a19"
Istio
Linkerd
Consul
```

---

# 137. Istio

Most popular Service Mesh.

---

## Components

```text id="k8s8a20"
Envoy Proxy
Control Plane
Gateway
```

---

## Architecture

```text id="k8s8a21"
Application
      │
      ▼
Envoy Sidecar
      │
      ▼
Istio
```

---

## Features

```text id="k8s8a22"
mTLS
Traffic Routing
Tracing
Observability
```

---

# 138. Linkerd

Lightweight Service Mesh.

---

## Architecture

```text id="k8s8a23"
Application
      │
      ▼
Linkerd Proxy
      │
      ▼
Service Mesh
```

---

## Benefits

```text id="k8s8a24"
Simple
Lightweight
Easy Adoption
```

---

# 139. Gateway API

Successor to traditional Ingress.

---

## Why Gateway API?

Ingress has limitations.

Gateway API provides:

```text id="k8s8a25"
More Flexibility
Better Routing
Multi-Team Ownership
```

---

## Architecture

```text id="k8s8a26"
Gateway
   │
   ▼
HTTPRoute
   │
   ▼
Services
```

---

## Components

```text id="k8s8a27"
GatewayClass
Gateway
HTTPRoute
TCPRoute
TLSRoute
```

---

# 140. Multi-Cluster Networking

Communication across multiple clusters.

---

## Architecture

```text id="k8s8a28"
Cluster A
    │
    ▼
Service Mesh
    │
    ▼
Cluster B
```

---

## Benefits

```text id="k8s8a29"
Disaster Recovery
Global Applications
Hybrid Cloud
```

---

# 141. Egress Traffic Control

Controls outbound traffic.

---

## Architecture

```text id="k8s8a30"
Pod
 │
 ▼
Egress Policy
 │
 ▼
Internet
```

---

## Use Cases

```text id="k8s8a31"
Compliance
Security
Traffic Restrictions
```

---

# 142. Volume Snapshots

Backup persistent volumes.

---

## Architecture

```text id="k8s8a32"
PVC
 │
 ▼
Snapshot
 │
 ▼
Restore
```

---

## Benefits

```text id="k8s8a33"
Backup
Recovery
Disaster Recovery
```

---

## VolumeSnapshot YAML

```yaml id="k8s8a34"
apiVersion: snapshot.storage.k8s.io/v1
kind: VolumeSnapshot

metadata:
  name: app-snapshot

spec:

  source:

    persistentVolumeClaimName: app-pvc
```

---

# 143. Volume Expansion

Increase storage without recreating volumes.

---

## Architecture

```text id="k8s8a35"
PVC
 │
 ▼
Expand
 │
 ▼
More Storage
```

---

## Example

```yaml id="k8s8a36"
resources:

  requests:

    storage: 100Gi
```

---

## Benefits

```text id="k8s8a37"
Scalability
Flexibility
```

---

# 144. Reclaim Policies

Determine what happens after PVC deletion.

---

## Types

```text id="k8s8a38"
Retain
Delete
Recycle (Deprecated)
```

---

## Retain

```text id="k8s8a39"
PVC Deleted

Data Preserved
```

---

## Delete

```text id="k8s8a40"
PVC Deleted

Storage Deleted
```

---

## PV Example

```yaml id="k8s8a41"
persistentVolumeReclaimPolicy: Retain
```

---

# 145. Backup and Restore Concepts

Critical for production clusters.

---

## Components

```text id="k8s8a42"
etcd Backup
Volume Backup
Configuration Backup
```

---

## Architecture

```text id="k8s8a43"
Cluster
 │
 ▼
Backup
 │
 ▼
Recovery
```

---

## Benefits

```text id="k8s8a44"
Business Continuity
Disaster Recovery
```

---

# 146. StatefulSet VolumeClaimTemplates

Automatically create PVCs per Pod.

---

## Architecture

```text id="k8s8a45"
StatefulSet
     │
     ▼
PVC Per Pod
```

---

## Example

```yaml id="k8s8a46"
volumeClaimTemplates:

- metadata:

    name: data

  spec:

    accessModes:

    - ReadWriteOnce

    resources:

      requests:

        storage: 10Gi
```

---

## Result

```text id="k8s8a47"
mysql-0 → PVC

mysql-1 → PVC

mysql-2 → PVC
```

---

# 147. Storage Lifecycle

Important interview topic.

---

## Complete Flow

```text id="k8s8a48"
Application
     │
     ▼
Pod
     │
     ▼
PVC
     │
     ▼
StorageClass
     │
     ▼
PV
     │
     ▼
Storage Backend
```

---

# 148. Common Storage Backends

---

## Cloud Storage

```text id="k8s8a49"
AWS EBS
AWS EFS

Azure Disk
Azure Files

Google Persistent Disk
Google Filestore
```

---

## On-Prem Storage

```text id="k8s8a50"
NFS
Ceph
GlusterFS
SAN
NAS
```

---

# 149. Storage Troubleshooting

---

## PVC Pending

Possible Causes:

```text id="k8s8a51"
No StorageClass

No Matching PV

CSI Driver Missing
```

---

## Commands

```bash id="k8s8a52"
kubectl describe pvc PVC_NAME

kubectl get pv

kubectl get storageclass
```

---

## Volume Mount Errors

```text id="k8s8a53"
Permission Issues

Node Issues

Storage Backend Issues
```

---

# 150. Advanced Networking & Storage Summary

---

## Networking Stack

```text id="k8s8a54"
CNI
 │
 ▼
Calico / Cilium / Flannel
 │
 ▼
Pods
 │
 ▼
Services
 │
 ▼
Ingress / Gateway API
 │
 ▼
Internet
```

---

## Storage Stack

```text id="k8s8a55"
Application
     │
     ▼
Pod
     │
     ▼
PVC
     │
     ▼
StorageClass
     │
     ▼
PV
     │
     ▼
Storage Backend
```

---

## Important Commands

```bash id="k8s8a56"
kubectl get volumesnapshot

kubectl get storageclass

kubectl get pvc

kubectl get pv

kubectl describe pvc PVC_NAME

kubectl get gateway

kubectl get httproute

kubectl get networkpolicy

kubectl describe networkpolicy
```

---

# 151. Cluster Administration

Cluster Administration involves managing, maintaining, securing, and upgrading Kubernetes clusters.

---

## Responsibilities

```text id="k8s9a1"
Cluster Setup
Node Management
Upgrades
Backups
Security
Monitoring
```

---

## Architecture

```text id="k8s9a2"
Administrator
      │
      ▼
Control Plane
      │
      ▼
Worker Nodes
```

---

# 152. kubeadm

kubeadm is the official Kubernetes cluster bootstrap tool.

---

## Purpose

```text id="k8s9a3"
Create Cluster
Join Nodes
Upgrade Cluster
Manage Certificates
```

---

## Architecture

```text id="k8s9a4"
kubeadm
   │
   ▼
Control Plane
   │
   ▼
Worker Nodes
```

---

## Initialize Cluster

```bash id="k8s9a5"
kubeadm init
```

---

## Join Worker Node

```bash id="k8s9a6"
kubeadm join CONTROL_PLANE_IP:6443 \
--token TOKEN \
--discovery-token-ca-cert-hash HASH
```

---

# 153. Cluster Installation Process

---

## Complete Flow

```text id="k8s9a7"
Install Container Runtime
          │
          ▼
Install kubeadm
          │
          ▼
Initialize Control Plane
          │
          ▼
Install CNI
          │
          ▼
Join Worker Nodes
```

---

## Verify Cluster

```bash id="k8s9a8"
kubectl get nodes
```

---

# 154. Managed Kubernetes

Cloud providers manage control plane components.

---

## Popular Managed Services

* Amazon EKS
* Azure AKS
* Google GKE

---

## Benefits

```text id="k8s9a9"
Managed Control Plane
Automatic Upgrades
Reduced Operations
```

---

## Comparison

| Self Managed            | Managed                  |
| ----------------------- | ------------------------ |
| Full Control            | Less Management          |
| More Effort             | Easier Operations        |
| Lower Vendor Dependency | Cloud Native Integration |

---

# 155. Node Management

Nodes are worker machines running Pods.

---

## Architecture

```text id="k8s9a10"
Cluster
 │
 ├── Node1
 ├── Node2
 └── Node3
```

---

## View Nodes

```bash id="k8s9a11"
kubectl get nodes
```

---

## Describe Node

```bash id="k8s9a12"
kubectl describe node NODE_NAME
```

---

# 156. Node Maintenance

Safely remove workloads before maintenance.

---

## Step 1: Cordon

Prevent new Pods.

```bash id="k8s9a13"
kubectl cordon NODE_NAME
```

---

## Architecture

```text id="k8s9a14"
Node
 │
 ▼
No New Pods
```

---

## Verify

```bash id="k8s9a15"
kubectl get nodes
```

---

# 157. Drain Node

Move workloads away from a node.

---

## Command

```bash id="k8s9a16"
kubectl drain NODE_NAME \
--ignore-daemonsets
```

---

## Architecture

```text id="k8s9a17"
Node
 │
 ▼
Evacuate Pods
 │
 ▼
Other Nodes
```

---

## Benefits

```text id="k8s9a18"
Safe Maintenance
Minimal Downtime
```

---

# 158. Uncordon Node

Return node to service.

---

## Command

```bash id="k8s9a19"
kubectl uncordon NODE_NAME
```

---

## Workflow

```text id="k8s9a20"
Maintenance Complete
        │
        ▼
Uncordon
        │
        ▼
Accept New Pods
```

---

# 159. Cluster Upgrades

Kubernetes versions must be upgraded carefully.

---

## Upgrade Flow

```text id="k8s9a21"
Control Plane
      │
      ▼
Worker Nodes
```

---

## Why?

Control plane must always be upgraded first.

---

## Verify Version

```bash id="k8s9a22"
kubectl version
```

---

## Check Nodes

```bash id="k8s9a23"
kubectl get nodes
```

---

# 160. Upgrade Process

---

## Workflow

```text id="k8s9a24"
Backup etcd
     │
     ▼
Upgrade Control Plane
     │
     ▼
Upgrade Worker Nodes
     │
     ▼
Validation
```

---

## Benefits

```text id="k8s9a25"
Stability
Reduced Risk
```

---

# 161. Certificate Management

Kubernetes uses certificates for authentication.

---

## Architecture

```text id="k8s9a26"
User
 │
 ▼
Certificate
 │
 ▼
API Server
```

---

## Check Certificates

```bash id="k8s9a27"
kubeadm certs check-expiration
```

---

## Renew Certificates

```bash id="k8s9a28"
kubeadm certs renew all
```

---

# 162. etcd

etcd is Kubernetes' database.

---

## Stores

```text id="k8s9a29"
Cluster State
Configuration
Secrets
Metadata
```

---

## Architecture

```text id="k8s9a30"
API Server
     │
     ▼
etcd
```

---

## Importance

```text id="k8s9a31"
No etcd
   =
No Cluster
```

---

# 163. etcd Backup

Most important Kubernetes backup.

---

## Backup Command

```bash id="k8s9a32"
ETCDCTL_API=3 etcdctl snapshot save backup.db
```

---

## Verify Backup

```bash id="k8s9a33"
ETCDCTL_API=3 etcdctl snapshot status backup.db
```

---

## Architecture

```text id="k8s9a34"
etcd
 │
 ▼
Snapshot
 │
 ▼
Backup
```

---

# 164. etcd Restore

Restore cluster state.

---

## Command

```bash id="k8s9a35"
ETCDCTL_API=3 etcdctl snapshot restore backup.db
```

---

## Workflow

```text id="k8s9a36"
Backup
 │
 ▼
Restore
 │
 ▼
Cluster Recovery
```

---

# 165. High Availability (HA)

Production clusters should avoid single points of failure.

---

## Single Control Plane

```text id="k8s9a37"
Control Plane

Failure ❌

Cluster Down
```

---

## HA Control Plane

```text id="k8s9a38"
CP1
 │
CP2
 │
CP3
```

---

## Benefits

```text id="k8s9a39"
Fault Tolerance
Availability
Resilience
```

---

# 166. HA Architecture

---

## Architecture

```text id="k8s9a40"
Load Balancer
      │
 ┌────┼────┐
 ▼    ▼    ▼
CP1  CP2  CP3
      │
      ▼
    etcd
```

---

## Recommended

```text id="k8s9a41"
3 Control Plane Nodes
```

---

# 167. Disaster Recovery

Plan for complete cluster failure.

---

## Components

```text id="k8s9a42"
etcd Backup
PV Backup
YAML Backup
Application Backup
```

---

## Recovery Flow

```text id="k8s9a43"
Failure
   │
   ▼
Restore Backups
   │
   ▼
Recover Services
```

---

# 168. Backup Strategy

---

## What To Backup

```text id="k8s9a44"
etcd
Persistent Volumes
Helm Charts
Git Repositories
Secrets
```

---

## Best Practice

```text id="k8s9a45"
Automate Backups
Test Restores
Store Offsite Copies
```

---

# 169. Cluster Maintenance Best Practices

---

## Recommendations

```text id="k8s9a46"
Regular Upgrades
Monitor Certificates
Monitor Node Health
Backup etcd
Audit Security
```

---

## Benefits

```text id="k8s9a47"
Stability
Reliability
Security
```

---

# 170. Cluster Administration Workflow

---

## End-to-End Flow

```text id="k8s9a48"
Cluster Creation
      │
      ▼
Node Management
      │
      ▼
Monitoring
      │
      ▼
Backup
      │
      ▼
Upgrade
      │
      ▼
Disaster Recovery
```

---

## Important Commands

```bash id="k8s9a49"
kubectl get nodes

kubectl describe node NODE

kubectl cordon NODE

kubectl drain NODE

kubectl uncordon NODE

kubectl version

kubeadm init

kubeadm join

kubeadm certs check-expiration

kubeadm certs renew all

etcdctl snapshot save backup.db

etcdctl snapshot restore backup.db
```

---

## Cluster Administrator Checklist

```text id="k8s9a50"
✓ Control Plane Healthy

✓ Nodes Healthy

✓ etcd Backups Working

✓ Certificates Valid

✓ Monitoring Active

✓ Storage Healthy

✓ Disaster Recovery Tested

✓ Security Policies Applied
```

---

# 171. Horizontal Pod Autoscaler (HPA)

HPA automatically scales Pods based on metrics.

---

## Problem

Without HPA:

```text id="k8s10a1"
Traffic Increases
       │
       ▼
Application Slows
```

---

## With HPA

```text id="k8s10a2"
Traffic Increases
       │
       ▼
HPA
       │
       ▼
More Pods Created
```

---

## Architecture

```text id="k8s10a3"
Metrics Server
      │
      ▼
HPA
      │
      ▼
Deployment
      │
      ▼
Pods
```

---

## Benefits

```text id="k8s10a4"
Automatic Scaling
Cost Optimization
High Availability
```

---

# 172. HPA YAML Example

---

## Example

```yaml id="k8s10a5"
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler

metadata:
  name: nginx-hpa

spec:

  scaleTargetRef:

    apiVersion: apps/v1

    kind: Deployment

    name: nginx-deployment

  minReplicas: 2

  maxReplicas: 10

  metrics:

  - type: Resource

    resource:

      name: cpu

      target:

        type: Utilization

        averageUtilization: 70
```

---

## Verify

```bash id="k8s10a6"
kubectl get hpa
```

---

## Describe

```bash id="k8s10a7"
kubectl describe hpa
```

---

# 173. HPA Workflow

---

## Complete Flow

```text id="k8s10a8"
CPU Usage
    │
    ▼
Metrics Server
    │
    ▼
HPA
    │
    ▼
Scale Deployment
```

---

## Scale Up Example

```text id="k8s10a9"
2 Pods

CPU 90%

↓

5 Pods
```

---

## Scale Down Example

```text id="k8s10a10"
5 Pods

CPU 20%

↓

2 Pods
```

---

# 174. Vertical Pod Autoscaler (VPA)

VPA adjusts Pod resources instead of Pod count.

---

## HPA vs VPA

| HPA                | VPA              |
| ------------------ | ---------------- |
| More Pods          | More Resources   |
| Horizontal Scaling | Vertical Scaling |
| Scale Count        | Scale CPU/Memory |

---

## Architecture

```text id="k8s10a11"
Pod
 │
 ▼
VPA
 │
 ▼
Increase CPU/Memory
```

---

## Benefits

```text id="k8s10a12"
Resource Optimization
Better Performance
```

---

# 175. VPA Example

---

## Architecture

```text id="k8s10a13"
Current:
CPU = 250m

Recommended:
CPU = 1000m
```

---

## Use Cases

```text id="k8s10a14"
Databases
Memory Intensive Apps
Analytics Workloads
```

---

# 176. Cluster Autoscaler

Adds or removes Nodes automatically.

---

## Architecture

```text id="k8s10a15"
Pods Pending
      │
      ▼
Cluster Autoscaler
      │
      ▼
New Node Added
```

---

## Benefits

```text id="k8s10a16"
Infrastructure Scaling
Cost Reduction
Automation
```

---

## Example

```text id="k8s10a17"
Node Full

↓

Add New Node

↓

Schedule Pod
```

---

# 177. Autoscaling Stack

---

## Complete Architecture

```text id="k8s10a18"
Application Load
        │
        ▼
HPA
        │
        ▼
More Pods
        │
        ▼
Cluster Full
        │
        ▼
Cluster Autoscaler
        │
        ▼
More Nodes
```

---

## Production Setup

```text id="k8s10a19"
HPA
+
Cluster Autoscaler
```

---

# 178. Pod Disruption Budgets (PDB)

Protect applications during disruptions.

---

## Problem

Without PDB:

```text id="k8s10a20"
Node Maintenance

↓

All Pods Deleted

↓

Application Down
```

---

## With PDB

```text id="k8s10a21"
Minimum Pods Protected
```

---

## Benefits

```text id="k8s10a22"
Availability
Controlled Maintenance
```

---

# 179. PDB YAML Example

---

## Example

```yaml id="k8s10a23"
apiVersion: policy/v1
kind: PodDisruptionBudget

metadata:
  name: nginx-pdb

spec:

  minAvailable: 2

  selector:

    matchLabels:

      app: nginx
```

---

## Verify

```bash id="k8s10a24"
kubectl get pdb
```

---

# 180. Resource Quotas

Limit resource usage per namespace.

---

## Purpose

Prevent a team from consuming all cluster resources.

---

## Architecture

```text id="k8s10a25"
Namespace
    │
    ▼
ResourceQuota
```

---

## ResourceQuota YAML

```yaml id="k8s10a26"
apiVersion: v1
kind: ResourceQuota

metadata:
  name: quota

spec:

  hard:

    pods: "20"

    requests.cpu: "10"

    requests.memory: 20Gi
```

---

## Verify

```bash id="k8s10a27"
kubectl get resourcequota
```

---

# 181. LimitRanges

Set default resource limits.

---

## Architecture

```text id="k8s10a28"
Namespace
    │
    ▼
LimitRange
    │
    ▼
Pods
```

---

## LimitRange YAML

```yaml id="k8s10a29"
apiVersion: v1
kind: LimitRange

metadata:
  name: limits

spec:

  limits:

  - default:

      cpu: 500m

      memory: 512Mi

    defaultRequest:

      cpu: 250m

      memory: 256Mi

    type: Container
```

---

## Benefits

```text id="k8s10a30"
Resource Governance
Predictable Usage
```

---

# 182. Multi-Tenancy

Multiple teams share one cluster.

---

## Architecture

```text id="k8s10a31"
Cluster
 │
 ├── Team A Namespace
 │
 ├── Team B Namespace
 │
 └── Team C Namespace
```

---

## Components

```text id="k8s10a32"
Namespaces
RBAC
Network Policies
Quotas
```

---

## Benefits

```text id="k8s10a33"
Isolation
Cost Savings
Centralized Management
```

---

# 183. Namespace Resource Governance

---

## Architecture

```text id="k8s10a34"
Namespace
    │
    ▼
Quota
    │
    ▼
Resource Limits
```

---

## Common Controls

```text id="k8s10a35"
CPU Limits
Memory Limits
Pod Count
Storage Limits
```

---

# 184. Workload Prioritization

Important workloads should get resources first.

---

## Priority Classes

```text id="k8s10a36"
High Priority
Medium Priority
Low Priority
```

---

## Architecture

```text id="k8s10a37"
Scheduler
    │
    ▼
PriorityClass
    │
    ▼
Pod Placement
```

---

# 185. PriorityClass YAML

---

## Example

```yaml id="k8s10a38"
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass

metadata:
  name: high-priority

value: 100000

globalDefault: false

description: "Critical workloads"
```

---

## Pod Example

```yaml id="k8s10a39"
spec:

  priorityClassName: high-priority
```

---

# 186. Topology Spread Constraints

Distribute Pods evenly.

---

## Problem

All Pods on one Node:

```text id="k8s10a40"
Node Failure

↓

Application Failure
```

---

## Solution

```text id="k8s10a41"
Spread Pods Across Nodes
```

---

## Architecture

```text id="k8s10a42"
Node1 → Pod

Node2 → Pod

Node3 → Pod
```

---

# 187. Topology Spread YAML

---

## Example

```yaml id="k8s10a43"
topologySpreadConstraints:

- maxSkew: 1

  topologyKey: kubernetes.io/hostname

  whenUnsatisfiable: DoNotSchedule

  labelSelector:

    matchLabels:

      app: nginx
```

---

# 188. Production Autoscaling Architecture

---

## Architecture

```text id="k8s10a44"
Traffic
   │
   ▼
Ingress
   │
   ▼
HPA
   │
   ▼
Pods
   │
   ▼
Cluster Autoscaler
   │
   ▼
Nodes
```

---

# 189. Enterprise Resource Management

---

## Components

```text id="k8s10a45"
ResourceQuota

LimitRange

PriorityClass

HPA

VPA

Cluster Autoscaler
```

---

## Benefits

```text id="k8s10a46"
Efficiency
Scalability
Cost Control
```

---

# 190. Autoscaling & Governance Summary

---

## Complete Flow

```text id="k8s10a47"
User Traffic
      │
      ▼
Deployment
      │
      ▼
HPA
      │
      ▼
Pods
      │
      ▼
Cluster Autoscaler
      │
      ▼
Nodes
      │
      ▼
ResourceQuota
      │
      ▼
Governance
```

---

## Important Commands

```bash id="k8s10a48"
kubectl get hpa

kubectl describe hpa

kubectl get pdb

kubectl get resourcequota

kubectl get limitrange

kubectl get priorityclass

kubectl top pods

kubectl top nodes

kubectl describe quota

kubectl describe limitrange
```

---

# 191. Kubernetes API Machinery

The Kubernetes API is the heart of the cluster.

Everything in Kubernetes communicates through the API Server.

---

## Architecture

```text id="k8s11a1"
kubectl
   │
   ▼
API Server
   │
   ▼
etcd
```

---

## Examples

```text id="k8s11a2"
Pods
Deployments
Services
ConfigMaps
Secrets
```

Everything is an API Object.

---

## Benefits

```text id="k8s11a3"
Centralized Control
Consistency
Extensibility
```

---

# 192. Kubernetes API Groups

Resources belong to API groups.

---

## Examples

```text id="k8s11a4"
core/v1

apps/v1

batch/v1

networking.k8s.io/v1

rbac.authorization.k8s.io/v1
```

---

## Example

```yaml id="k8s11a5"
apiVersion: apps/v1

kind: Deployment
```

---

## Why API Groups?

```text id="k8s11a6"
Organization
Versioning
Scalability
```

---

# 193. Kubernetes API Request Lifecycle

Very important interview topic.

---

## Complete Flow

```text id="k8s11a7"
kubectl apply
      │
      ▼
Authentication
      │
      ▼
Authorization
      │
      ▼
Admission Controllers
      │
      ▼
API Server
      │
      ▼
etcd
```

---

## Example

```bash id="k8s11a8"
kubectl apply -f deployment.yaml
```

---

## What Happens?

```text id="k8s11a9"
Validate

Store Desired State

Controllers React

Cluster Updated
```

---

# 194. Controllers

Controllers continuously watch resources.

---

## Responsibility

```text id="k8s11a10"
Current State

vs

Desired State
```

---

## Architecture

```text id="k8s11a11"
Desired State
      │
      ▼
Controller
      │
      ▼
Actual State
```

---

## Examples

```text id="k8s11a12"
Deployment Controller

ReplicaSet Controller

Node Controller

Job Controller
```

---

# 195. Reconciliation Loop

Core Kubernetes concept.

---

## Definition

Controllers continuously compare:

```text id="k8s11a13"
Desired State

Actual State
```

---

## Example

Desired:

```text id="k8s11a14"
3 Pods
```

Actual:

```text id="k8s11a15"
2 Pods
```

Controller:

```text id="k8s11a16"
Creates 1 Pod
```

---

## Workflow

```text id="k8s11a17"
Desired State
      │
      ▼
Controller
      │
      ▼
Current State
      │
      ▼
Fix Drift
```

---

# 196. Informers

Informers efficiently monitor Kubernetes resources.

---

## Problem

Without Informers:

```text id="k8s11a18"
Constant API Calls
```

---

## Solution

```text id="k8s11a19"
Informer Cache
```

---

## Architecture

```text id="k8s11a20"
API Server
     │
     ▼
Informer
     │
     ▼
Controller
```

---

## Benefits

```text id="k8s11a21"
Performance
Reduced API Load
Scalability
```

---

# 197. Watches

Watch API streams resource changes.

---

## Architecture

```text id="k8s11a22"
API Server
     │
     ▼
Watch
     │
     ▼
Controller
```

---

## Example

```bash id="k8s11a23"
kubectl get pods --watch
```

---

## Benefits

```text id="k8s11a24"
Real-Time Updates
Efficiency
```

---

# 198. Custom Resources (CR)

Custom Resources extend Kubernetes.

---

## Built-in Resources

```text id="k8s11a25"
Pod
Service
Deployment
```

---

## Custom Resources

```text id="k8s11a26"
Database
Backup
Application
```

---

## Example

```text id="k8s11a27"
MyDatabase
```

Custom resource created by users.

---

# 199. CRDs (Custom Resource Definitions)

CRDs define new Kubernetes resource types.

---

## Architecture

```text id="k8s11a28"
CRD
 │
 ▼
Custom Resource
```

---

## Example

```text id="k8s11a29"
kind: Database
```

---

## Benefits

```text id="k8s11a30"
Extensibility
Platform Engineering
Automation
```

---

# 200. CRD YAML Example

---

## Example

```yaml id="k8s11a31"
apiVersion: apiextensions.k8s.io/v1

kind: CustomResourceDefinition

metadata:

  name: databases.company.com

spec:

  group: company.com

  names:

    kind: Database

    plural: databases

  scope: Namespaced

  versions:

  - name: v1

    served: true

    storage: true
```

---

## Verify

```bash id="k8s11a32"
kubectl get crd
```

---

# 201. Custom Resources Example

---

## YAML

```yaml id="k8s11a33"
apiVersion: company.com/v1

kind: Database

metadata:

  name: mysql-prod

spec:

  size: large

  replicas: 3
```

---

## Architecture

```text id="k8s11a34"
Custom Resource
       │
       ▼
Controller
       │
       ▼
Infrastructure
```

---

# 202. Operators

Operators automate complex applications.

---

## Traditional Deployment

```text id="k8s11a35"
Install

Configure

Manage

Backup

Upgrade
```

Manual work.

---

## Operator

```text id="k8s11a36"
Operator Automates Everything
```

---

## Examples

```text id="k8s11a37"
MongoDB Operator

Prometheus Operator

Kafka Operator
```

---

# 203. Operator Pattern

---

## Architecture

```text id="k8s11a38"
CRD
 │
 ▼
Custom Resource
 │
 ▼
Operator
 │
 ▼
Application
```

---

## Workflow

```text id="k8s11a39"
User Creates Resource

↓

Operator Watches

↓

Operator Acts

↓

Application Created
```

---

# 204. Admission Webhooks

Dynamic Admission Controllers.

---

## Types

```text id="k8s11a40"
Mutating Webhook

Validating Webhook
```

---

## Architecture

```text id="k8s11a41"
Request
   │
   ▼
Webhook
   │
   ▼
Allow / Modify / Reject
```

---

## Use Cases

```text id="k8s11a42"
Security Enforcement

Policy Validation

Automatic Sidecars
```

---

# 205. Mutating Webhooks

Modify resources.

---

## Example

```text id="k8s11a43"
Inject Istio Sidecar

Add Labels

Add Annotations
```

---

## Workflow

```text id="k8s11a44"
Pod Creation
      │
      ▼
Webhook
      │
      ▼
Modified Pod
```

---

# 206. Validating Webhooks

Validate resources.

---

## Example

```text id="k8s11a45"
Block Privileged Containers

Require Labels

Enforce Standards
```

---

## Workflow

```text id="k8s11a46"
Request
 │
 ▼
Validation
 │
 ▼
Approve / Reject
```

---

# 207. API Aggregation

Allows external APIs to appear as Kubernetes APIs.

---

## Architecture

```text id="k8s11a47"
API Server
     │
     ▼
Aggregated API
     │
     ▼
External Service
```

---

## Benefits

```text id="k8s11a48"
Extensibility
Unified API Experience
```

---

# 208. API Priority and Fairness

Protects API Server from overload.

---

## Problem

```text id="k8s11a49"
Too Many Requests
```

---

## Solution

```text id="k8s11a50"
Priority Queues

Rate Limiting
```

---

## Benefits

```text id="k8s11a51"
Stability
Fair Resource Usage
```

---

# 209. Kubernetes Control Loop Internals

The most important Kubernetes internal concept.

---

## Workflow

```text id="k8s11a52"
User Creates Resource
         │
         ▼
API Server
         │
         ▼
etcd
         │
         ▼
Controller Watches
         │
         ▼
Controller Reconciles
         │
         ▼
Desired State Achieved
```

---

# 210. Kubernetes Internals Summary

---

## Internal Components

```text id="k8s11a53"
API Server

etcd

Controllers

Informers

Watches

CRDs

Operators

Admission Webhooks
```

---

## Internal Architecture

```text id="k8s11a54"
User
 │
 ▼
API Server
 │
 ▼
etcd
 │
 ▼
Controllers
 │
 ▼
Reconciliation
 │
 ▼
Cluster State
```

---

## Important Commands

```bash id="k8s11a55"
kubectl api-resources

kubectl api-versions

kubectl explain deployment

kubectl get crd

kubectl describe crd

kubectl get validatingwebhookconfigurations

kubectl get mutatingwebhookconfigurations

kubectl get apiservices

kubectl get events

kubectl get pods --watch
```

---

# 211. Helm

Helm is the package manager for Kubernetes.

Think of Helm as:

```text id="k8s12a1"
apt for Ubuntu

yum for RHEL

npm for Node.js

helm for Kubernetes
```

---

## Problem Without Helm

Managing hundreds of YAML files becomes difficult.

```text id="k8s12a2"
deployment.yaml

service.yaml

ingress.yaml

configmap.yaml

secret.yaml
```

---

## Solution

```text id="k8s12a3"
Helm Chart
     │
     ▼
Single Package
```

---

## Architecture

```text id="k8s12a4"
Helm
 │
 ▼
Chart
 │
 ▼
Release
 │
 ▼
Cluster
```

---

# 212. Helm Charts

Charts are Kubernetes application packages.

---

## Chart Structure

```text id="k8s12a5"
mychart/

├── Chart.yaml

├── values.yaml

├── charts/

└── templates/
```

---

## Components

```text id="k8s12a6"
Chart.yaml

values.yaml

Templates

Dependencies
```

---

# 213. Chart.yaml

Stores chart metadata.

---

## Example

```yaml id="k8s12a7"
apiVersion: v2

name: nginx

description: Nginx Helm Chart

version: 1.0.0

appVersion: "1.25"
```

---

## Purpose

```text id="k8s12a8"
Name

Version

Description

Dependencies
```

---

# 214. values.yaml

Stores configurable values.

---

## Example

```yaml id="k8s12a9"
replicaCount: 3

image:
  repository: nginx
  tag: latest
```

---

## Benefits

```text id="k8s12a10"
Customization

Reusability

Environment Specific Configurations
```

---

# 215. Helm Templates

Templates generate Kubernetes manifests.

---

## Example

```yaml id="k8s12a11"
replicas: {{ .Values.replicaCount }}
```

---

## Workflow

```text id="k8s12a12"
Template
    │
    ▼
Values
    │
    ▼
Final YAML
```

---

## Benefits

```text id="k8s12a13"
Reusable
Parameterized
Dynamic
```

---

# 216. Helm Repository

Stores Helm charts.

---

## Examples

```text id="k8s12a14"
Bitnami

ArtifactHub

Internal Repositories
```

---

## Commands

```bash id="k8s12a15"
helm repo add bitnami \
https://charts.bitnami.com/bitnami

helm repo update

helm repo list
```

---

# 217. Install Helm Chart

---

## Search

```bash id="k8s12a16"
helm search repo nginx
```

---

## Install

```bash id="k8s12a17"
helm install my-nginx bitnami/nginx
```

---

## Verify

```bash id="k8s12a18"
helm list
```

---

# 218. Upgrade Helm Release

---

## Command

```bash id="k8s12a19"
helm upgrade my-nginx bitnami/nginx
```

---

## Workflow

```text id="k8s12a20"
Old Release
      │
      ▼
Upgrade
      │
      ▼
New Release
```

---

# 219. Rollback Helm Release

---

## Command

```bash id="k8s12a21"
helm rollback my-nginx 1
```

---

## Architecture

```text id="k8s12a22"
Release v2
     │
     ▼
Rollback
     │
     ▼
Release v1
```

---

# 220. Kustomize

Kustomize customizes Kubernetes manifests.

Built directly into kubectl.

---

## Benefits

```text id="k8s12a23"
No Templates

Environment Specific Config

Simple Overlay Model
```

---

## Architecture

```text id="k8s12a24"
Base YAML
    │
    ▼
Overlay
    │
    ▼
Final YAML
```

---

# 221. Kustomization File

---

## Example

```yaml id="k8s12a25"
resources:

- deployment.yaml

- service.yaml
```

---

## Apply

```bash id="k8s12a26"
kubectl apply -k .
```

---

# 222. GitOps

Git becomes the source of truth.

---

## Traditional Deployment

```text id="k8s12a27"
Engineer
    │
    ▼
kubectl apply
```

---

## GitOps

```text id="k8s12a28"
Git
 │
 ▼
Automation
 │
 ▼
Cluster
```

---

## Benefits

```text id="k8s12a29"
Auditability

Version Control

Automation

Consistency
```

---

# 223. GitOps Workflow

---

## Architecture

```text id="k8s12a30"
Developer
     │
     ▼
Git Commit
     │
     ▼
Git Repository
     │
     ▼
ArgoCD / Flux
     │
     ▼
Cluster
```

---

# 224. Argo CD

Most popular GitOps tool.

---

## Architecture

```text id="k8s12a31"
Git Repository
      │
      ▼
ArgoCD
      │
      ▼
Kubernetes
```

---

## Features

```text id="k8s12a32"
Auto Sync

Drift Detection

Rollback

Visualization
```

---

## Verify Applications

```bash id="k8s12a33"
argocd app list
```

---

# 225. Argo CD Workflow

---

## Complete Flow

```text id="k8s12a34"
Git Change
     │
     ▼
ArgoCD Detects Change
     │
     ▼
Sync
     │
     ▼
Cluster Updated
```

---

# 226. Flux CD

Alternative GitOps platform.

---

## Architecture

```text id="k8s12a35"
Git Repository
      │
      ▼
Flux
      │
      ▼
Cluster
```

---

## Benefits

```text id="k8s12a36"
Git Native

Lightweight

CNCF Project
```

---

# 227. Jenkins Integration

Kubernetes is commonly integrated with Jenkins.

---

## Architecture

```text id="k8s12a37"
Code
 │
 ▼
Jenkins
 │
 ▼
Build Image
 │
 ▼
Deploy To Kubernetes
```

---

## Pipeline Flow

```text id="k8s12a38"
Code
 │
 ▼
Build
 │
 ▼
Test
 │
 ▼
Push Image
 │
 ▼
Deploy
```

---

# 228. GitHub Actions Integration

Modern CI/CD platform.

---

## Workflow

```text id="k8s12a39"
GitHub
    │
    ▼
Action
    │
    ▼
Docker Build
    │
    ▼
Deploy To Cluster
```

---

## Benefits

```text id="k8s12a40"
Cloud Native

Integrated

Automation
```

---

# 229. GitLab CI Integration

---

## Architecture

```text id="k8s12a41"
GitLab
  │
  ▼
Pipeline
  │
  ▼
Cluster
```

---

## Benefits

```text id="k8s12a42"
Integrated SCM

CI/CD

Container Registry
```

---

# 230. Progressive Delivery

Advanced deployment strategy.

---

## Types

```text id="k8s12a43"
Canary

Blue-Green

A/B Testing
```

---

## Benefits

```text id="k8s12a44"
Reduced Risk

Safer Deployments
```

---

# 231. Canary Deployment

Release to a small percentage of users first.

---

## Architecture

```text id="k8s12a45"
Users
 │
 ├── 90% → Version 1

 └── 10% → Version 2
```

---

## Workflow

```text id="k8s12a46"
Deploy New Version

↓

Test Traffic

↓

Increase Traffic

↓

Full Rollout
```

---

# 232. Blue-Green Deployment

Maintain two environments.

---

## Architecture

```text id="k8s12a47"
Blue Environment

(Current)

Green Environment

(New)
```

---

## Workflow

```text id="k8s12a48"
Deploy Green

↓

Test Green

↓

Switch Traffic

↓

Remove Blue
```

---

# 233. Deployment Strategies Comparison

| Strategy       | Downtime | Risk   |
| -------------- | -------- | ------ |
| Recreate       | High     | High   |
| Rolling Update | Low      | Medium |
| Canary         | Very Low | Low    |
| Blue-Green     | Very Low | Low    |

---

# 234. GitOps Best Practices

---

## Recommendations

```text id="k8s12a49"
Everything In Git

Immutable Images

Automated Sync

Code Reviews

Protected Branches
```

---

# 235. GitOps Architecture Summary

```text id="k8s12a50"
Git Repository
      │
      ▼
ArgoCD / Flux
      │
      ▼
Kubernetes Cluster
      │
      ▼
Helm / Kustomize
      │
      ▼
Applications
```

---

# 236. Important Commands

```bash id="k8s12a51"
helm repo add

helm repo update

helm search repo

helm install

helm upgrade

helm rollback

helm list

kubectl apply -k .

argocd app list

argocd app sync
```

---

# 237. Cloud Native Ecosystem

Kubernetes is not a standalone tool.

It is the foundation of the Cloud Native ecosystem.

---

## CNCF Landscape

```text id="k8s13a1"
Kubernetes
     │
     ├── Helm
     ├── Prometheus
     ├── ArgoCD
     ├── Envoy
     ├── Cilium
     ├── Falco
     ├── Fluentd
     ├── OpenTelemetry
     └── Harbor
```

---

## Architecture

```text id="k8s13a2"
Applications
      │
      ▼
Kubernetes
      │
      ▼
Cloud Native Tools
```

---

## Benefits

```text id="k8s13a3"
Scalability

Automation

Observability

Security
```

---

# 238. Secret Management

Storing secrets inside Kubernetes Secrets is often insufficient for enterprise environments.

---

## Problems

```text id="k8s13a4"
Secret Rotation

Central Management

Compliance

Auditing
```

---

## Enterprise Solutions

```text id="k8s13a5"
HashiCorp Vault

AWS Secrets Manager

Azure Key Vault

Google Secret Manager
```

---

# 239. HashiCorp Vault

Most popular enterprise secret management platform.

---

## Architecture

```text id="k8s13a6"
Application
     │
     ▼
Vault
     │
     ▼
Secrets
```

---

## Benefits

```text id="k8s13a7"
Encryption

Secret Rotation

Dynamic Credentials

Auditing
```

---

## Workflow

```text id="k8s13a8"
Pod
 │
 ▼
Vault Agent
 │
 ▼
Vault Server
 │
 ▼
Secret Retrieved
```

---

# 240. External Secrets Operator

Synchronizes external secrets into Kubernetes.

---

## Architecture

```text id="k8s13a9"
AWS Secrets Manager
          │
          ▼
External Secrets Operator
          │
          ▼
Kubernetes Secret
```

---

## Benefits

```text id="k8s13a10"
Centralized Secrets

Automatic Synchronization

Improved Security
```

---

# 241. Open Policy Agent (OPA)

OPA provides policy-based governance.

---

## Purpose

Control what can be deployed.

---

## Examples

```text id="k8s13a11"
Require Labels

Block Privileged Containers

Enforce Security Rules
```

---

## Architecture

```text id="k8s13a12"
Pod Request
      │
      ▼
OPA
      │
      ▼
Allow / Deny
```

---

# 242. OPA Example

---

## Policy Example

```text id="k8s13a13"
Deny Containers

Running As Root
```

---

## Benefits

```text id="k8s13a14"
Compliance

Security

Governance
```

---

# 243. Kyverno

Kubernetes-native policy engine.

---

## Why Kyverno?

No need to learn Rego language.

Policies are YAML based.

---

## Architecture

```text id="k8s13a15"
Resource
    │
    ▼
Kyverno
    │
    ▼
Validation
```

---

## Features

```text id="k8s13a16"
Validation

Mutation

Generation
```

---

# 244. Kyverno Policy Example

---

## Require Labels

```yaml id="k8s13a17"
apiVersion: kyverno.io/v1
kind: ClusterPolicy

metadata:
  name: require-labels
```

---

## Benefits

```text id="k8s13a18"
Simple

Kubernetes Native

Easy Adoption
```

---

# 245. Runtime Security

Protect running containers.

---

## Traditional Security

```text id="k8s13a19"
Build Time Security
```

---

## Runtime Security

```text id="k8s13a20"
Detect Attacks

Detect Anomalies

Block Threats
```

---

# 246. Falco

Runtime security monitoring platform.

---

## Architecture

```text id="k8s13a21"
Kernel Events
      │
      ▼
Falco
      │
      ▼
Alerts
```

---

## Detects

```text id="k8s13a22"
Privilege Escalation

Shell Access

Suspicious Activity
```

---

## Benefits

```text id="k8s13a23"
Threat Detection

Compliance

Auditing
```

---

# 247. Platform Engineering

One of the fastest growing Kubernetes domains.

---

## Goal

Create internal platforms for developers.

---

## Architecture

```text id="k8s13a24"
Developers
      │
      ▼
Platform Team
      │
      ▼
Kubernetes Platform
```

---

## Benefits

```text id="k8s13a25"
Developer Productivity

Standardization

Self-Service
```

---

# 248. Internal Developer Platforms (IDP)

Provide self-service infrastructure.

---

## Workflow

```text id="k8s13a26"
Developer
      │
      ▼
Portal
      │
      ▼
Kubernetes
```

---

## Examples

```text id="k8s13a27"
Backstage

Humanitec

Port
```

---

# 249. Multi-Cluster Kubernetes

Large organizations use multiple clusters.

---

## Reasons

```text id="k8s13a28"
Disaster Recovery

Environment Isolation

Geographic Distribution

Compliance
```

---

## Architecture

```text id="k8s13a29"
Cluster A

Cluster B

Cluster C
```

---

# 250. Multi-Cluster Challenges

---

## Common Problems

```text id="k8s13a30"
Configuration Drift

Policy Consistency

Networking

Observability
```

---

## Solutions

```text id="k8s13a31"
GitOps

Service Mesh

Central Monitoring
```

---

# 251. Cluster API (CAPI)

Cluster API manages Kubernetes clusters using Kubernetes APIs.

---

## Architecture

```text id="k8s13a32"
Management Cluster
        │
        ▼
Workload Clusters
```

---

## Benefits

```text id="k8s13a33"
Declarative Clusters

Automation

Lifecycle Management
```

---

# 252. Rancher

Popular multi-cluster management platform.

---

## Features

```text id="k8s13a34"
Cluster Management

RBAC

Monitoring

Policy Management
```

---

## Architecture

```text id="k8s13a35"
Rancher
   │
   ▼
Multiple Clusters
```

---

# 253. OpenShift

Enterprise Kubernetes platform from [Red Hat](https://www.redhat.com).

---

## Features

```text id="k8s13a36"
Built-in CI/CD

Security

Developer Tools

Operators
```

---

## Benefits

```text id="k8s13a37"
Enterprise Ready

Support

Compliance
```

---

# 254. Amazon EKS

Managed Kubernetes service from [AWS](https://aws.amazon.com/eks/).

---

## Architecture

```text id="k8s13a38"
AWS
 │
 ▼
EKS Control Plane
 │
 ▼
Worker Nodes
```

---

## Benefits

```text id="k8s13a39"
Managed Control Plane

AWS Integration

High Availability
```

---

# 255. Azure AKS

Managed Kubernetes service from [Microsoft Azure](https://azure.microsoft.com/en-us/products/kubernetes-service/).

---

## Benefits

```text id="k8s13a40"
Managed Operations

Azure Integration

Scalability
```

---

# 256. Google GKE

Managed Kubernetes service from [Google Cloud](https://cloud.google.com/kubernetes-engine).

---

## Benefits

```text id="k8s13a41"
Google Expertise

Autopilot Mode

Advanced Networking
```

---

# 257. EKS vs AKS vs GKE

| Feature               | EKS       | AKS       | GKE       |
| --------------------- | --------- | --------- | --------- |
| Cloud                 | AWS       | Azure     | GCP       |
| Managed Control Plane | Yes       | Yes       | Yes       |
| Autopilot             | No        | Limited   | Yes       |
| AWS Integration       | Excellent | No        | No        |
| Azure Integration     | No        | Excellent | No        |
| GCP Integration       | No        | No        | Excellent |

---

# 258. Cost Optimization (FinOps)

Kubernetes can become expensive without governance.

---

## Common Issues

```text id="k8s13a42"
Overprovisioning

Idle Resources

Unused Volumes

Oversized Nodes
```

---

## Optimization Methods

```text id="k8s13a43"
HPA

Cluster Autoscaler

Right Sizing

Spot Instances
```

---

# 259. Compliance

Many organizations must meet regulatory requirements.

---

## Common Standards

```text id="k8s13a44"
PCI-DSS

SOC2

HIPAA

ISO 27001
```

---

## Requirements

```text id="k8s13a45"
Encryption

Auditing

Access Control

Logging
```

---

# 260. Enterprise Kubernetes Architecture

---

## End-to-End Architecture

```text id="k8s13a46"
Developers
      │
      ▼
Git Repository
      │
      ▼
CI/CD
      │
      ▼
ArgoCD
      │
      ▼
Kubernetes
      │
      ├── OPA/Kyverno
      ├── Vault
      ├── Prometheus
      ├── Grafana
      ├── Falco
      └── Service Mesh
```

---

## Important Commands

```bash id="k8s13a47"
kubectl get clusterpolicy

kubectl get validatingwebhookconfigurations

kubectl get mutatingwebhookconfigurations

kubectl get crd

kubectl get nodes

kubectl top nodes

kubectl top pods

kubectl api-resources

kubectl get events
```

---

# 261. Kubernetes Troubleshooting Fundamentals

Troubleshooting is one of the most important Kubernetes skills.

Most real-world Kubernetes work involves diagnosing issues rather than deploying applications.

---

## Troubleshooting Workflow

```text id="k8s14a1"
Problem
   │
   ▼
Identify Resource
   │
   ▼
Check Events
   │
   ▼
Check Logs
   │
   ▼
Check Configuration
   │
   ▼
Fix Issue
```

---

## Golden Commands

```bash id="k8s14a2"
kubectl get all

kubectl describe pod POD_NAME

kubectl logs POD_NAME

kubectl logs --previous POD_NAME

kubectl exec -it POD_NAME -- sh

kubectl get events

kubectl top pods

kubectl top nodes
```

---

# 262. kubectl describe

Most useful troubleshooting command.

---

## Command

```bash id="k8s14a3"
kubectl describe pod POD_NAME
```

---

## Information Provided

```text id="k8s14a4"
Events

Conditions

Image Pulls

Scheduling Issues

Volumes

Probes
```

---

## Example

```text id="k8s14a5"
FailedScheduling

FailedMount

ImagePullBackOff
```

---

# 263. kubectl logs

Check application logs.

---

## Commands

```bash id="k8s14a6"
kubectl logs POD

kubectl logs -f POD

kubectl logs POD -c CONTAINER

kubectl logs --previous POD
```

---

## Architecture

```text id="k8s14a7"
Application
     │
     ▼
Container Logs
     │
     ▼
kubectl logs
```

---

# 264. kubectl exec

Access container shell.

---

## Commands

```bash id="k8s14a8"
kubectl exec -it POD -- sh

kubectl exec -it POD -- bash
```

---

## Use Cases

```text id="k8s14a9"
Check Files

Network Testing

Configuration Validation
```

---

## Example

```bash id="k8s14a10"
kubectl exec -it nginx-pod -- sh
```

---

# 265. Pod Status Troubleshooting

---

## View Pods

```bash id="k8s14a11"
kubectl get pods
```

---

## Common Statuses

```text id="k8s14a12"
Running

Pending

ContainerCreating

CrashLoopBackOff

ImagePullBackOff

Completed

Error

Terminating
```

---

# 266. Pending Pods

Pod cannot be scheduled.

---

## Symptoms

```text id="k8s14a13"
STATUS = Pending
```

---

## Common Causes

```text id="k8s14a14"
Insufficient CPU

Insufficient Memory

Node Selector Mismatch

Taints

PVC Pending
```

---

## Troubleshooting

```bash id="k8s14a15"
kubectl describe pod POD_NAME
```

---

## Example

```text id="k8s14a16"
0/5 nodes available
```

---

# 267. ContainerCreating

Pod scheduled but container not started.

---

## Causes

```text id="k8s14a17"
Image Pull Delay

Volume Mount Delay

Network Setup Delay
```

---

## Commands

```bash id="k8s14a18"
kubectl describe pod POD_NAME
```

---

## Architecture

```text id="k8s14a19"
Pod Scheduled
      │
      ▼
ContainerCreating
      │
      ▼
Running
```

---

# 268. CrashLoopBackOff

Most common interview question.

Container starts and repeatedly crashes.

---

## Architecture

```text id="k8s14a20"
Start
 │
 ▼
Crash
 │
 ▼
Restart
 │
 ▼
Crash
```

---

## Common Causes

```text id="k8s14a21"
Application Bug

Wrong Configuration

Database Unreachable

Missing Environment Variables

Port Conflicts
```

---

## Troubleshooting

```bash id="k8s14a22"
kubectl logs POD

kubectl logs --previous POD

kubectl describe pod POD
```

---

# 269. ImagePullBackOff

Kubernetes cannot pull image.

---

## Architecture

```text id="k8s14a23"
Pod
 │
 ▼
Pull Image
 │
 ✖
Failure
```

---

## Causes

```text id="k8s14a24"
Wrong Image Name

Private Registry

Authentication Failure

Network Issue
```

---

## Example

```yaml id="k8s14a25"
image: nginxx
```

Wrong image.

---

## Fix

```text id="k8s14a26"
Correct Image Name

Configure imagePullSecrets
```

---

# 270. ErrImagePull

Image download failed.

---

## Causes

```text id="k8s14a27"
Image Missing

Registry Down

DNS Failure
```

---

## Verify

```bash id="k8s14a28"
kubectl describe pod POD_NAME
```

---

# 271. OOMKilled

Container exceeded memory limit.

OOM = Out Of Memory

---

## Symptoms

```text id="k8s14a29"
Container Restarts

Reason: OOMKilled
```

---

## Example

```yaml id="k8s14a30"
limits:
  memory: 256Mi
```

Application uses:

```text id="k8s14a31"
512Mi
```

---

## Fix

```text id="k8s14a32"
Increase Memory

Optimize Application
```

---

# 272. CreateContainerConfigError

Configuration problem.

---

## Causes

```text id="k8s14a33"
Missing ConfigMap

Missing Secret

Wrong Reference
```

---

## Example

```yaml id="k8s14a34"
configMapRef:
  name: missing-config
```

---

## Verify

```bash id="k8s14a35"
kubectl describe pod POD
```

---

# 273. CreateContainerError

Container creation failed.

---

## Causes

```text id="k8s14a36"
Invalid Commands

Bad Entrypoint

Volume Problems
```

---

## Troubleshooting

```bash id="k8s14a37"
kubectl logs POD

kubectl describe pod POD
```

---

# 274. RunContainerError

Container runtime failed to start.

---

## Causes

```text id="k8s14a38"
Permission Issues

Bad Startup Commands

Filesystem Errors
```

---

## Verify

```bash id="k8s14a39"
kubectl describe pod POD
```

---

# 275. InvalidImageName

Invalid image format.

---

## Example

```yaml id="k8s14a40"
image: nginx::
```

---

## Result

```text id="k8s14a41"
InvalidImageName
```

---

## Fix

```yaml id="k8s14a42"
image: nginx:latest
```

---

# 276. Probe Failures

---

## Types

```text id="k8s14a43"
Liveness Probe Failed

Readiness Probe Failed

Startup Probe Failed
```

---

## Troubleshooting

```bash id="k8s14a44"
kubectl describe pod POD
```

---

## Common Causes

```text id="k8s14a45"
Wrong Port

Wrong Path

Slow Application
```

---

# 277. Liveness Probe Failed

Container repeatedly restarted.

---

## Architecture

```text id="k8s14a46"
Probe Fails
     │
     ▼
Restart Container
```

---

## Example

```yaml id="k8s14a47"
livenessProbe:

  httpGet:

    path: /health

    port: 8080
```

---

## Issue

```text id="k8s14a48"
/health Endpoint Missing
```

---

# 278. Readiness Probe Failed

Pod removed from Service endpoints.

---

## Architecture

```text id="k8s14a49"
Probe Fails
     │
     ▼
No Traffic
```

---

## Benefits

```text id="k8s14a50"
Protect Users

Avoid Broken Requests
```

---

# 279. Startup Probe Failed

Slow applications killed too early.

---

## Fix

```yaml id="k8s14a51"
startupProbe:

  failureThreshold: 30
```

---

## Common Applications

```text id="k8s14a52"
Java Applications

Spring Boot

Large Databases
```

---

# 280. Troubleshooting Workflow Summary

```text id="k8s14a53"

kubectl get pods
       │
       ▼
kubectl describe pod

       │
       ▼
kubectl logs
       │
       ▼
kubectl exec
       │
       ▼
Fix Problem
```

---

## Most Common Errors

| Error                      | Root Cause         |
| -------------------------- | ------------------ |
| Pending                    | Scheduler Issues   |
| ContainerCreating          | Startup Delay      |
| CrashLoopBackOff           | App Crash          |
| ImagePullBackOff           | Image Pull Failure |
| ErrImagePull               | Registry Issue     |
| OOMKilled                  | Memory Limit       |
| CreateContainerConfigError | Missing Config     |
| InvalidImageName           | Bad Image          |
| Probe Failure              | Health Check Issue |

---

## Important Commands

```bash id="k8s14a54"
kubectl get pods

kubectl describe pod POD

kubectl logs POD

kubectl logs --previous POD

kubectl exec -it POD -- sh

kubectl get events

kubectl top pods

kubectl top nodes
```

---

# 281. Service Troubleshooting

Many applications run perfectly but are not reachable because of Service misconfigurations.

---

## Troubleshooting Flow

```text id="k8s15a1"
Application
     │
     ▼
Pod
     │
     ▼
Service
     │
     ▼
Ingress
     │
     ▼
User
```

---

## Verify Service

```bash id="k8s15a2"
kubectl get svc

kubectl describe svc SERVICE_NAME
```

---

## Verify Endpoints

```bash id="k8s15a3"
kubectl get endpoints

kubectl describe endpoints SERVICE_NAME
```

---

# 282. Service Has No Endpoints

Very common issue.

---

## Symptoms

```text id="k8s15a4"
Service Exists

Pods Running

Application Unreachable
```

---

## Root Cause

Label mismatch.

---

## Example

Service:

```yaml id="k8s15a5"
selector:
  app: nginx
```

Pod:

```yaml id="k8s15a6"
labels:
  app: apache
```

---

## Fix

Labels and selectors must match.

---

## Verify

```bash id="k8s15a7"
kubectl get pods --show-labels

kubectl describe svc SERVICE_NAME
```

---

# 283. Wrong Port Configuration

Another common issue.

---

## Example

Pod:

```yaml id="k8s15a8"
containerPort: 8080
```

Service:

```yaml id="k8s15a9"
targetPort: 80
```

---

## Result

```text id="k8s15a10"
Connection Refused
```

---

## Fix

```yaml id="k8s15a11"
targetPort: 8080
```

---

# 284. DNS Resolution Failures

---

## Symptoms

```text id="k8s15a12"
Service Name Not Found
```

---

## Test DNS

```bash id="k8s15a13"
nslookup SERVICE_NAME

dig SERVICE_NAME
```

---

## Verify CoreDNS

```bash id="k8s15a14"
kubectl get pods -n kube-system
```

---

## Common Causes

```text id="k8s15a15"
CoreDNS Down

Network Issues

Wrong Namespace
```

---

# 285. Network Policy Issues

Network Policies may block traffic.

---

## Symptoms

```text id="k8s15a16"
Pods Running

No Connectivity
```

---

## Verify Policies

```bash id="k8s15a17"
kubectl get networkpolicy

kubectl describe networkpolicy
```

---

## Architecture

```text id="k8s15a18"
Pod A
 │
 ✖
 │
 ▼
Pod B
```

---

# 286. Ingress Troubleshooting

---

## Verify Ingress

```bash id="k8s15a19"
kubectl get ingress

kubectl describe ingress
```

---

## Common Issues

```text id="k8s15a20"
Wrong Backend Service

DNS Misconfiguration

TLS Problems

Ingress Controller Missing
```

---

# 287. HTTP 404 Errors

---

## Cause

Ingress routing issue.

---

## Example

```text id="k8s15a21"
Host Rule Missing

Path Rule Missing
```

---

## Verify

```bash id="k8s15a22"
kubectl describe ingress
```

---

# 288. HTTP 503 Errors

---

## Cause

Backend service unavailable.

---

## Workflow

```text id="k8s15a23"
Ingress
   │
   ▼
Service
   │
   ✖
   ▼
Pods
```

---

## Verify

```bash id="k8s15a24"
kubectl get endpoints
```

---

# 289. TLS Errors

---

## Symptoms

```text id="k8s15a25"
Certificate Error

HTTPS Failure
```

---

## Verify Secrets

```bash id="k8s15a26"
kubectl get secrets

kubectl describe secret TLS_SECRET
```

---

## Verify Ingress

```bash id="k8s15a27"
kubectl describe ingress
```

---

# 290. PVC Pending

Most common storage issue.

---

## Symptoms

```text id="k8s15a28"
PVC Status = Pending
```

---

## Verify

```bash id="k8s15a29"
kubectl get pvc

kubectl describe pvc PVC_NAME
```

---

## Common Causes

```text id="k8s15a30"
No StorageClass

No Matching PV

CSI Driver Missing
```

---

# 291. Volume Mount Failures

---

## Symptoms

```text id="k8s15a31"
FailedMount
```

---

## Causes

```text id="k8s15a32"
Permission Issues

Storage Issues

Node Issues
```

---

## Verify

```bash id="k8s15a33"
kubectl describe pod POD
```

---

# 292. Node NotReady

Critical cluster issue.

---

## Verify

```bash id="k8s15a34"
kubectl get nodes
```

---

## Example

```text id="k8s15a35"
worker-1

STATUS: NotReady
```

---

## Causes

```text id="k8s15a36"
Kubelet Down

Network Failure

Disk Failure
```

---

# 293. DiskPressure

Node storage exhausted.

---

## Symptoms

```text id="k8s15a37"
New Pods Not Scheduled
```

---

## Verify

```bash id="k8s15a38"
kubectl describe node NODE
```

---

## Fix

```text id="k8s15a39"
Delete Unused Images

Free Disk Space
```

---

# 294. MemoryPressure

Node running out of memory.

---

## Symptoms

```text id="k8s15a40"
Pods Evicted
```

---

## Verify

```bash id="k8s15a41"
kubectl describe node NODE
```

---

## Fix

```text id="k8s15a42"
Add Memory

Optimize Applications
```

---

# 295. API Server Issues

API Server is the brain of Kubernetes.

---

## Symptoms

```text id="k8s15a43"
kubectl Commands Fail
```

---

## Verify

```bash id="k8s15a44"
kubectl cluster-info
```

---

## Check Pods

```bash id="k8s15a45"
kubectl get pods -n kube-system
```

---

# 296. etcd Failures

Critical issue.

---

## Symptoms

```text id="k8s15a46"
Cluster State Lost

API Errors
```

---

## Verify

```bash id="k8s15a47"
etcdctl endpoint health
```

---

## Recovery

```text id="k8s15a48"
Restore Snapshot
```

---

# 297. Scheduler Failures

Pods remain Pending.

---

## Symptoms

```text id="k8s15a49"
Pods Never Scheduled
```

---

## Verify

```bash id="k8s15a50"
kubectl get pods

kubectl describe pod POD
```

---

## Causes

```text id="k8s15a51"
Scheduler Down

Resource Constraints

Affinity Issues
```

---

# 298. Controller Manager Failures

Controllers stop reconciling resources.

---

## Symptoms

```text id="k8s15a52"
Deployments Not Scaling

ReplicaSets Not Recovering
```

---

## Verify

```bash id="k8s15a53"
kubectl get pods -n kube-system
```

---

# 299. Production Troubleshooting Workflow

---

## Step-by-Step Process

```text id="k8s15a54"
Check Pods
     │
     ▼
Describe Pod
     │
     ▼
Check Events
     │
     ▼
Check Logs
     │
     ▼
Check Services
     │
     ▼
Check Networking
     │
     ▼
Check Storage
     │
     ▼
Check Nodes
```

---

# 300. Kubernetes Troubleshooting Cheat Flow

```text id="k8s15a55"
Pod Issue?
 │
 ▼
kubectl describe pod

 │

 ▼

Logs?

 │

 ▼

kubectl logs

 │

 ▼

Network?

 │

 ▼

kubectl get svc

kubectl get endpoints

 │

 ▼

Storage?

 │

 ▼

kubectl get pvc

kubectl describe pvc

 │

 ▼

Node?

 │

 ▼

kubectl describe node

 │

 ▼

Control Plane?

 │

 ▼

kubectl get pods -n kube-system
```

---

## Essential Troubleshooting Commands

```bash id="k8s15a56"
kubectl get all -A

kubectl get events --sort-by=.metadata.creationTimestamp

kubectl describe pod POD

kubectl logs POD

kubectl logs --previous POD

kubectl exec -it POD -- sh

kubectl get svc

kubectl get endpoints

kubectl get ingress

kubectl get pvc

kubectl get pv

kubectl get nodes

kubectl describe node NODE

kubectl top pods

kubectl top nodes

kubectl cluster-info
```

---

## Kubernetes Troubleshooting Mindset

```text id="k8s15a57"
Pod
 │
 ▼
Container
 │
 ▼
Service
 │
 ▼
Ingress
 │
 ▼
Storage
 │
 ▼
Node
 │
 ▼
Control Plane
```

Always troubleshoot from the smallest component outward:

```text id="k8s15a58"
Container
   →
Pod
   →
Service
   →
Ingress
   →
Node
   →
Cluster
```

This approach solves the majority of production Kubernetes incidents efficiently.

---

# 301. Custom Controllers

Controllers are the core building blocks of Kubernetes automation.

A Custom Controller extends Kubernetes functionality.

---

## Built-in Controllers

```text id="k8s16a1"
Deployment Controller

ReplicaSet Controller

Job Controller

StatefulSet Controller
```

---

## Custom Controller

```text id="k8s16a2"
Your Logic

↓

Kubernetes Automation
```

---

## Architecture

```text id="k8s16a3"
Custom Resource
       │
       ▼
Custom Controller
       │
       ▼
Infrastructure
```

---

## Benefits

```text id="k8s16a4"
Automation

Consistency

Platform Engineering
```

---

# 302. Custom Controller Workflow

---

## Example

Create:

```text id="k8s16a5"
Database Resource
```

Controller:

```text id="k8s16a6"
Creates

Database

Storage

Backup Jobs
```

Automatically.

---

## Flow

```text id="k8s16a7"
CRD
 │
 ▼
Custom Resource
 │
 ▼
Controller
 │
 ▼
Desired State
```

---

# 303. Operator SDK

Operator SDK simplifies operator development.

---

## Architecture

```text id="k8s16a8"
Operator SDK
      │
      ▼
Operator
      │
      ▼
Kubernetes
```

---

## Benefits

```text id="k8s16a9"
Scaffolding

Code Generation

Testing Tools
```

---

## Common Languages

```text id="k8s16a10"
Go

Ansible

Helm
```

---

# 304. Kubernetes API Extensions

Kubernetes APIs can be extended.

---

## Methods

```text id="k8s16a11"
CRDs

Aggregated APIs

Webhooks
```

---

## Architecture

```text id="k8s16a12"
Kubernetes API
       │
       ▼
Extensions
       │
       ▼
New Functionality
```

---

# 305. Scheduler Internals

The Scheduler decides where Pods run.

---

## Workflow

```text id="k8s16a13"
Pod Created
      │
      ▼
Scheduler
      │
      ▼
Node Selected
```

---

## Factors

```text id="k8s16a14"
CPU

Memory

Affinity

Taints

Topology
```

---

# 306. Scheduler Phases

---

## Filtering Phase

Remove unsuitable Nodes.

```text id="k8s16a15"
Node A ✓

Node B ✖

Node C ✓
```

---

## Scoring Phase

Choose best Node.

```text id="k8s16a16"
Node A = 90

Node C = 70

Choose Node A
```

---

## Architecture

```text id="k8s16a17"
Filter
  │
  ▼
Score
  │
  ▼
Bind
```

---

# 307. Scheduler Plugins

Modern Kubernetes scheduler is plugin-based.

---

## Plugins

```text id="k8s16a18"
NodeResourcesFit

PodTopologySpread

TaintToleration

NodeAffinity
```

---

## Benefits

```text id="k8s16a19"
Extensible

Custom Scheduling
```

---

# 308. Runtime Classes

Choose different container runtimes.

---

## Architecture

```text id="k8s16a20"
Pod
 │
 ▼
RuntimeClass
 │
 ▼
Runtime
```

---

## Examples

```text id="k8s16a21"
containerd

CRI-O

gVisor

Kata Containers
```

---

## RuntimeClass YAML

```yaml id="k8s16a22"
apiVersion: node.k8s.io/v1
kind: RuntimeClass

metadata:
  name: gvisor

handler: runsc
```

---

# 309. gVisor

Sandboxed container runtime.

---

## Architecture

```text id="k8s16a23"
Container
     │
     ▼
gVisor
     │
     ▼
Kernel
```

---

## Benefits

```text id="k8s16a24"
Isolation

Security
```

---

# 310. Kata Containers

Lightweight virtual machines.

---

## Architecture

```text id="k8s16a25"
Container
     │
     ▼
Micro VM
     │
     ▼
Host
```

---

## Benefits

```text id="k8s16a26"
Strong Isolation

Security
```

---

# 311. eBPF

Extended Berkeley Packet Filter.

One of the hottest Kubernetes technologies.

---

## Traditional Networking

```text id="k8s16a27"
Packet

↓

Kernel Networking Stack
```

---

## eBPF

```text id="k8s16a28"
Packet

↓

eBPF Program

↓

Fast Processing
```

---

## Benefits

```text id="k8s16a29"
Performance

Security

Observability
```

---

# 312. Cilium Internals

Cilium uses eBPF.

---

## Architecture

```text id="k8s16a30"
Pod
 │
 ▼
eBPF
 │
 ▼
Cilium
 │
 ▼
Network
```

---

## Features

```text id="k8s16a31"
Network Policies

Observability

Security

Load Balancing
```

---

# 313. Falco Deep Dive

Falco provides runtime threat detection.

---

## Detects

```text id="k8s16a32"
Reverse Shells

Privilege Escalation

Suspicious Processes
```

---

## Architecture

```text id="k8s16a33"
Kernel Events
      │
      ▼
Falco
      │
      ▼
Alert
```

---

# 314. Kubernetes Source Code Architecture

Interview-level advanced topic.

---

## Major Components

```text id="k8s16a34"
API Server

Scheduler

Controller Manager

Kubelet

kubectl
```

---

## Repository Structure

```text id="k8s16a35"
cmd/

pkg/

staging/

vendor/
```

---

## Architecture

```text id="k8s16a36"
User
 │
 ▼
API Server
 │
 ▼
etcd
 │
 ▼
Controllers
 │
 ▼
Nodes
```

---

# 315. Kubernetes Enhancement Proposals (KEPs)

KEPs are Kubernetes design proposals.

---

## Purpose

```text id="k8s16a37"
New Features

Major Changes

Architecture Evolution
```

---

## Examples

```text id="k8s16a38"
Gateway API

Pod Security Standards

Sidecar Containers
```

---

# 316. CNCF Ecosystem

Cloud Native Computing Foundation manages major projects.

---

## Popular Projects

```text id="k8s16a39"
Kubernetes

Helm

Prometheus

ArgoCD

Envoy

OpenTelemetry

Falco

Cilium
```

---

## Architecture

```text id="k8s16a40"
CNCF
 │
 ▼
Cloud Native Projects
```

---

# 317. Kubernetes Contribution Workflow

---

## Process

```text id="k8s16a41"
Fork Repository

Create Branch

Write Code

Submit PR

Review

Merge
```

---

## Skills Required

```text id="k8s16a42"
Go

Git

Linux

Containers

Distributed Systems
```

---

# 318. Platform Engineering Architecture

---

## Enterprise View

```text id="k8s16a43"
Developers
      │
      ▼
IDP
      │
      ▼
GitOps
      │
      ▼
Kubernetes
      │
      ▼
Cloud Infrastructure
```

---

# 319. Expert Kubernetes Stack

---

## Complete Stack

```text id="k8s16a44"
GitHub

↓

CI/CD

↓

Container Registry

↓

ArgoCD

↓

Kubernetes

↓

Cilium

↓

Istio

↓

Prometheus

↓

Grafana

↓

Falco

↓

Vault
```

---

# 320. Expert-Level Kubernetes Roadmap

```text id="k8s16a45"
Containers

↓

Docker

↓

Kubernetes Fundamentals

↓

Networking

↓

Storage

↓

Security

↓

Observability

↓

Cluster Administration

↓

GitOps

↓

Platform Engineering

↓

Operators

↓

eBPF

↓

Cilium

↓

Kubernetes Internals

↓

Contribute To Kubernetes
```

---

## Important Commands

```bash id="k8s16a46"
kubectl api-resources

kubectl api-versions

kubectl explain pod

kubectl explain deployment

kubectl get runtimeclass

kubectl get crd

kubectl get validatingwebhookconfigurations

kubectl get mutatingwebhookconfigurations

kubectl get apiservices

kubectl get events

kubectl get nodes
```

---

## Expert Interview Questions

### What is the Reconciliation Loop?

```text id="k8s16a47"
Desired State

vs

Actual State
```

Controllers continuously reconcile the difference.

---

### Difference Between CRD and Operator?

```text id="k8s16a48"
CRD

Defines Resource

Operator

Manages Resource
```

---

### Difference Between Cilium and Calico?

```text id="k8s16a49"
Calico

Traditional Networking

Cilium

eBPF Based Networking
```

---

### Difference Between gVisor and Kata?

```text id="k8s16a50"
gVisor

User-space Kernel

Kata

Micro VM
```

---

### What is eBPF?

```text id="k8s16a51"
Kernel-Level Programmability

for Networking

Security

Observability
```

---

### What Happens When kubectl apply Runs?

```text id="k8s16a52"
kubectl

↓

API Server

↓

Authentication

↓

Authorization

↓

Admission Controller

↓

etcd

↓

Controller

↓

Scheduler

↓

Node
```

---

# 321. Kubernetes Certification Guide

Kubernetes certifications are maintained by the [Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/) and administered through [The Linux Foundation Training & Certification](https://training.linuxfoundation.org/).

---

## Certification Roadmap

```text
Beginner
   │
   ▼
KCNA

   │
   ▼
CKA

   │
   ▼
CKAD

   │
   ▼
CKS

   │
   ▼
Advanced Platform Engineering
```

---

# 322. KCNA (Kubernetes and Cloud Native Associate)

Best starting certification.

---

## Covers

```text
Cloud Native Fundamentals
Containers
Kubernetes Basics
CNCF Ecosystem
DevOps Concepts
```

---

## Difficulty

```text
Easy
```

---

## Recommended For

```text
Freshers
Developers
Cloud Beginners
DevOps Beginners
```

---

# 323. CKA (Certified Kubernetes Administrator)

Most recognized Kubernetes certification.

---

## Focus Areas

```text
Cluster Architecture

Installation

Configuration

Workloads

Scheduling

Networking

Storage

Troubleshooting
```

---

## Difficulty

```text
Intermediate to Advanced
```

---

## Exam Type

```text
Performance Based

Hands-On Lab Exam
```

---

# 324. CKA Domains

---

## Cluster Architecture

```text
Control Plane

Worker Nodes

kubeadm

High Availability
```

---

## Workloads

```text
Pods

Deployments

StatefulSets

DaemonSets

Jobs
```

---

## Networking

```text
Services

Ingress

DNS

Network Policies
```

---

## Storage

```text
PV

PVC

StorageClass

CSI
```

---

## Troubleshooting

```text
Pods

Nodes

Networking

Storage
```

---

# 325. CKA Preparation Strategy

---

## Week 1

```text
Architecture

Control Plane

Pods

Deployments
```

---

## Week 2

```text
Services

Ingress

DNS

Networking
```

---

## Week 3

```text
Storage

PV

PVC

StatefulSets
```

---

## Week 4

```text
RBAC

Security

Troubleshooting
```

---

## Week 5

```text
Mock Exams

Speed Practice
```

---

# 326. CKA Must-Know Commands

```bash
kubectl run

kubectl create deployment

kubectl expose

kubectl scale

kubectl rollout

kubectl logs

kubectl exec

kubectl describe

kubectl get events

kubectl cordon

kubectl drain

kubectl uncordon
```

---

# 327. CKA Exam Tips

---

## Important

```text
Practice Daily

Use kubectl explain

Memorize YAML Structure

Learn Fast Troubleshooting
```

---

## Time Management

```text
Easy Questions First

Flag Difficult Questions

Return Later
```

---

# 328. CKAD (Certified Kubernetes Application Developer)

Developer-focused certification.

---

## Focus Areas

```text
Pods

Deployments

ConfigMaps

Secrets

Services

Ingress

Helm

Troubleshooting
```

---

## Difficulty

```text
Intermediate
```

---

## Best For

```text
Developers

DevOps Engineers

Platform Engineers
```

---

# 329. CKAD Preparation

---

## Important Topics

```text
Deployment YAML

ConfigMaps

Secrets

Services

Ingress

Probes

Jobs

CronJobs
```

---

## High Priority

```text
kubectl run

kubectl expose

kubectl create deployment
```

---

# 330. CKS (Certified Kubernetes Security Specialist)

Most advanced Kubernetes certification.

---

## Prerequisite

```text
CKA Required
```

---

## Focus Areas

```text
Cluster Hardening

Supply Chain Security

Runtime Security

RBAC

Network Policies

Falco

Image Scanning
```

---

## Difficulty

```text
Advanced
```

---

# 331. CKS Domains

---

## Cluster Hardening

```text
RBAC

Pod Security Standards

Admission Controllers
```

---

## Supply Chain Security

```text
Image Scanning

Image Signing

SBOM
```

---

## Runtime Security

```text
Falco

Runtime Monitoring

Threat Detection
```

---

## Network Security

```text
Network Policies

Ingress Security

mTLS
```

---

# 332. Certification Resources

---

## Official

* [Linux Foundation Training](https://training.linuxfoundation.org/)
* [CNCF Certifications](https://www.cncf.io/certification/)
* [Kubernetes Documentation](https://kubernetes.io/docs/)

---

## Practice Labs

* [Killercoda](https://killercoda.com/)
* [Play with Kubernetes](https://labs.play-with-k8s.com/)
* [KodeKloud](https://kodekloud.com/)

---

## Kubernetes Sandboxes

* [Katacoda Archive Alternatives (Killercoda)](https://killercoda.com/)
* [Minikube](https://minikube.sigs.k8s.io/docs/)
* [Kind (Kubernetes in Docker)](https://kind.sigs.k8s.io/)

---

# 333. Free Kubernetes Learning Path

---

## Phase 1

```text
Containers

Docker

Container Runtime
```

---

## Phase 2

```text
Kubernetes Fundamentals

Pods

Deployments

Services
```

---

## Phase 3

```text
Networking

Ingress

Storage
```

---

## Phase 4

```text
Security

RBAC

Secrets
```

---

## Phase 5

```text
Monitoring

Prometheus

Grafana
```

---

## Phase 6

```text
GitOps

Helm

ArgoCD
```

---

## Phase 7

```text
Troubleshooting

Mock Exams

Labs
```

---

# 334. Recommended Home Lab

---

## Laptop Setup

```text
Docker Desktop

OR

Minikube

OR

Kind
```

---

## Advanced Lab

```text
3 Node Cluster

Prometheus

Grafana

ArgoCD

Ingress Controller
```

---

# 335. Interview Preparation Roadmap

---

## Junior DevOps

```text
Pods

Deployments

Services

ConfigMaps

Secrets
```

---

## Mid-Level DevOps

```text
Networking

Storage

RBAC

Troubleshooting

Ingress
```

---

## Senior DevOps

```text
HA Clusters

GitOps

Operators

eBPF

Cilium

Platform Engineering
```

---

# 336. Kubernetes Learning Pyramid

```text
Expert
 │
 ├── Operators
 ├── eBPF
 ├── Platform Engineering
 │
 Advanced
 │
 ├── GitOps
 ├── Security
 ├── Observability
 │
 Intermediate
 │
 ├── Networking
 ├── Storage
 ├── Scheduling
 │
 Beginner
 │
 ├── Pods
 ├── Deployments
 ├── Services
 └── kubectl
```

---

# 337. Kubernetes Career Paths

```text
Kubernetes Administrator

DevOps Engineer

Site Reliability Engineer

Platform Engineer

Cloud Engineer

Cloud Architect

Infrastructure Engineer

Security Engineer

MLOps Engineer
```

---

# 338. Kubernetes Mastery Checklist

```text
✓ Pods

✓ Deployments

✓ Services

✓ Ingress

✓ ConfigMaps

✓ Secrets

✓ Storage

✓ Networking

✓ RBAC

✓ Security

✓ Monitoring

✓ Logging

✓ GitOps

✓ Helm

✓ ArgoCD

✓ Troubleshooting

✓ Cluster Administration

✓ High Availability

✓ Operators

✓ eBPF

✓ Platform Engineering
```

---

# 339. Final Certification Advice

```text
For Job Interviews:
Pods
Deployments
Services
Ingress
Storage
Troubleshooting

For DevOps Roles:
CKA

For Developers:
CKAD

For Security Engineers:
CKS

For Beginners:
KCNA
```

---

# 340. Kubernetes Command Encyclopedia (Recommended Appendix)

Add a final appendix containing:

```text
200+ kubectl Commands

100+ YAML Examples

50+ Troubleshooting Scenarios

50+ Interview Questions

CKA Quick Revision Sheet

CKAD Quick Revision Sheet

CKS Quick Revision Sheet

Architecture Diagrams

Production Checklists
```

---

# 341. Kubernetes Interview Questions (Beginner Level)

## Kubernetes Fundamentals

### Q1. What is Kubernetes?

```text id="k8s17a1"
Container Orchestration Platform
```

Used to deploy, manage, scale, and automate containers.

---

### Q2. Why Kubernetes?

```text id="k8s17a2"
Auto Scaling
Self Healing
Load Balancing
Service Discovery
Rolling Updates
```

---

### Q3. What is a Cluster?

```text id="k8s17a3"
Control Plane
+
Worker Nodes
```

---

### Q4. Difference Between Docker and Kubernetes?

| Docker            | Kubernetes             |
| ----------------- | ---------------------- |
| Container Runtime | Container Orchestrator |
| Runs Containers   | Manages Containers     |
| Single Host       | Multi Node             |

---

### Q5. What is a Pod?

Smallest deployable unit in Kubernetes.

---

### Q6. Can a Pod Have Multiple Containers?

Yes.

Example:

```text id="k8s17a4"
Application Container

+

Logging Sidecar
```

---

### Q7. What is a ReplicaSet?

Maintains desired number of Pods.

---

### Q8. What is a Deployment?

Manages ReplicaSets and rolling updates.

---

### Q9. Deployment vs ReplicaSet?

```text id="k8s17a5"
Deployment
     │
     ▼
ReplicaSet
     │
     ▼
Pods
```

Deployment is higher-level.

---

### Q10. What Happens If a Pod Crashes?

```text id="k8s17a6"
ReplicaSet Detects

↓

Creates New Pod
```

---

# 342. Architecture Interview Questions

### Q11. Components of Control Plane?

```text id="k8s17a7"
API Server
etcd
Scheduler
Controller Manager
```

---

### Q12. Role of API Server?

Central communication hub.

---

### Q13. Role of etcd?

Stores cluster state.

---

### Q14. What Happens if etcd Fails?

```text id="k8s17a8"
Cluster State Lost
```

Cluster becomes unusable.

---

### Q15. Role of Scheduler?

Assign Pods to Nodes.

---

### Q16. Role of Controller Manager?

Runs controllers.

---

### Q17. What is kubelet?

Node agent.

---

### Q18. What is kube-proxy?

Handles Service networking.

---

### Q19. What Happens During Pod Creation?

```text id="k8s17a9"
kubectl apply

↓

API Server

↓

etcd

↓

Scheduler

↓

Node

↓

kubelet

↓

Container Runtime
```

---

### Q20. What is Reconciliation Loop?

Controllers continuously compare:

```text id="k8s17a10"
Desired State

vs

Actual State
```

---

# 343. Pod Interview Questions

### Q21. Pod Lifecycle Phases?

```text id="k8s17a11"
Pending
Running
Succeeded
Failed
Unknown
```

---

### Q22. Init Container vs Sidecar?

| Init Container | Sidecar        |
| -------------- | -------------- |
| Runs First     | Runs Alongside |
| Exits          | Keeps Running  |

---

### Q23. Why Not Run Multiple Applications in One Pod?

Breaks microservice principles.

---

### Q24. How to Access Pod Shell?

```bash id="k8s17a12"
kubectl exec -it POD -- sh
```

---

### Q25. How to View Logs?

```bash id="k8s17a13"
kubectl logs POD
```

---

# 344. Deployment Interview Questions

### Q26. Rolling Update?

Update Pods gradually.

---

### Q27. Benefits?

```text id="k8s17a14"
Zero Downtime
```

---

### Q28. Rollback Deployment?

```bash id="k8s17a15"
kubectl rollout undo deployment DEPLOYMENT
```

---

### Q29. Check Rollout Status?

```bash id="k8s17a16"
kubectl rollout status deployment DEPLOYMENT
```

---

### Q30. Scale Deployment?

```bash id="k8s17a17"
kubectl scale deployment nginx --replicas=5
```

---

# 345. Service Interview Questions

### Q31. What is a Service?

Provides stable access to Pods.

---

### Q32. Types of Services?

```text id="k8s17a18"
ClusterIP
NodePort
LoadBalancer
ExternalName
```

---

### Q33. Default Service Type?

```text id="k8s17a19"
ClusterIP
```

---

### Q34. ClusterIP vs NodePort?

| ClusterIP | NodePort        |
| --------- | --------------- |
| Internal  | External        |
| Default   | Opens Node Port |

---

### Q35. LoadBalancer?

Cloud Load Balancer integration.

---

### Q36. What is Service Discovery?

Finding services using DNS.

---

### Q37. DNS Format?

```text id="k8s17a20"
service.namespace.svc.cluster.local
```

---

### Q38. What is CoreDNS?

Cluster DNS server.

---

### Q39. Why Service Has No Endpoints?

Label mismatch.

---

### Q40. Check Endpoints?

```bash id="k8s17a21"
kubectl get endpoints
```

---

# 346. Storage Interview Questions

### Q41. What is a Volume?

Persistent storage for Pods.

---

### Q42. What is emptyDir?

Temporary Pod storage.

---

### Q43. What is PV?

Persistent Volume.

---

### Q44. What is PVC?

Persistent Volume Claim.

---

### Q45. Difference Between PV and PVC?

| PV             | PVC                 |
| -------------- | ------------------- |
| Actual Storage | Request For Storage |

---

### Q46. StorageClass Purpose?

Dynamic provisioning.

---

### Q47. Access Modes?

```text id="k8s17a22"
RWO
ROX
RWX
```

---

### Q48. What is CSI?

Container Storage Interface.

---

### Q49. PVC Pending Causes?

```text id="k8s17a23"
No StorageClass
No PV
CSI Issue
```

---

### Q50. StatefulSet Storage?

Uses:

```text id="k8s17a24"
VolumeClaimTemplates
```

---

# 347. Networking Interview Questions

### Q51. What is CNI?

Container Network Interface.

---

### Q52. Popular CNIs?

```text id="k8s17a25"
Calico
Cilium
Flannel
```

---

### Q53. Pod-to-Pod Communication?

Each Pod gets unique IP.

---

### Q54. What is Ingress?

HTTP/HTTPS traffic routing.

---

### Q55. Ingress vs LoadBalancer?

| Ingress      | LoadBalancer   |
| ------------ | -------------- |
| L7           | L4             |
| HTTP Routing | Basic Exposure |

---

### Q56. What is Gateway API?

Next-generation Ingress.

---

### Q57. What is Network Policy?

Controls Pod traffic.

---

### Q58. Default Network Policy Behavior?

Allow all.

---

### Q59. What is Service Mesh?

Service-to-service networking layer.

---

### Q60. Popular Service Meshes?

```text id="k8s17a26"
Istio
Linkerd
```

---

# 348. Security Interview Questions

### Q61. Authentication vs Authorization?

| Authentication | Authorization    |
| -------------- | ---------------- |
| Who Are You?   | What Can You Do? |

---

### Q62. What is RBAC?

Role-Based Access Control.

---

### Q63. Components of RBAC?

```text id="k8s17a27"
Role
ClusterRole
RoleBinding
ClusterRoleBinding
```

---

### Q64. Role vs ClusterRole?

Namespace vs Cluster.

---

### Q65. ServiceAccount Purpose?

Pod identity.

---

### Q66. What are Secrets?

Sensitive data storage.

---

### Q67. Base64 Encoding?

Used in Secret manifests.

---

### Q68. Pod Security Standards?

```text
Privileged
Baseline
Restricted
```

---

### Q69. Admission Controllers?

Validate or modify requests.

---

### Q70. OPA Purpose?

Policy enforcement.

---

# 349. Troubleshooting Interview Questions

### Q71. CrashLoopBackOff?

Application repeatedly crashes.

---

### Q72. ImagePullBackOff?

Cannot pull image.

---

### Q73. OOMKilled?

Memory limit exceeded.

---

### Q74. CreateContainerConfigError?

Missing Secret or ConfigMap.

---

### Q75. Pending Pod?

Scheduling failure.

---

### Q76. Node NotReady?

Node unavailable.

---

### Q77. DiskPressure?

Disk full.

---

### Q78. MemoryPressure?

Memory exhausted.

---

### Q79. First Troubleshooting Command?

```bash id="k8s17a29"
kubectl describe pod POD
```

---

### Q80. Check Cluster Events?

```bash id="k8s17a30"
kubectl get events
```

---

# 350. Advanced Interview Questions

### Q81. What is a CRD?

Custom Resource Definition.

---

### Q82. What is an Operator?

Automation built around CRDs.

---

### Q83. CRD vs Operator?

```text
CRD = Resource

Operator = Logic
```

---

### Q84. What is GitOps?

Git as source of truth.

---

### Q85. Popular GitOps Tools?

```text
ArgoCD
Flux
```

---

### Q86. Helm Purpose?

Package manager for Kubernetes.

---

### Q87. Helm vs Kustomize?

Templates vs Overlays.

---

### Q88. What is eBPF?

Kernel-level programmability.

---

### Q89. Cilium vs Calico?

eBPF vs traditional networking.

---

### Q90. What is Falco?

Runtime security tool.

---

### Q91. What is HPA?

Horizontal Pod Autoscaler.

---

### Q92. What is VPA?

Vertical Pod Autoscaler.

---

### Q93. What is Cluster Autoscaler?

Scales Nodes.

---

### Q94. What is HA Control Plane?

Multiple control plane nodes.

---

### Q95. What is Multi-Tenancy?

Multiple teams sharing cluster.

---

### Q96. What is OpenTelemetry?

Observability framework.

---

### Q97. What is Prometheus?

Metrics collection platform.

---

### Q98. What is Grafana?

Visualization platform.

---

### Q99. What is ArgoCD?

GitOps deployment controller.

---

### Q100. Most Important Kubernetes Concept?

```text
Desired State
+
Reconciliation Loop
```

---

# 351. kubectl Command Encyclopedia (Quick Reference)

## Resource Discovery

```bash id="k8s17a34"
kubectl api-resources

kubectl api-versions

kubectl explain pod

kubectl explain deployment

kubectl explain deployment.spec
```

---

## Cluster Information

```bash id="k8s17a35"
kubectl cluster-info

kubectl version

kubectl get componentstatuses

kubectl get nodes
```

---

## Pods

```bash id="k8s17a36"
kubectl get pods

kubectl get pods -A

kubectl get pods -o wide

kubectl describe pod POD

kubectl delete pod POD

kubectl logs POD

kubectl logs -f POD

kubectl exec -it POD -- sh
```

---

## Deployments

```bash id="k8s17a37"
kubectl get deployments

kubectl create deployment nginx --image=nginx

kubectl scale deployment nginx --replicas=5

kubectl rollout status deployment nginx

kubectl rollout undo deployment nginx
```

---

## Services

```bash id="k8s17a38"
kubectl get svc

kubectl describe svc

kubectl expose deployment nginx \
--port=80 \
--type=NodePort
```

---

## Storage

```bash id="k8s17a39"
kubectl get pv

kubectl get pvc

kubectl describe pvc PVC

kubectl get storageclass
```

---

## Security

```bash id="k8s17a40"
kubectl get roles

kubectl get rolebindings

kubectl auth can-i create pods

kubectl get sa
```

---

## Node Administration

```bash id="k8s17a41"
kubectl cordon NODE

kubectl drain NODE

kubectl uncordon NODE
```

---

## CKA Quick Revision Sheet

```text
Pods
Deployments
Services
Ingress
Storage
RBAC
Networking
Troubleshooting
Cluster Administration
```

---

## CKAD Quick Revision Sheet

```text
Pods
Deployments
ConfigMaps
Secrets
Jobs
CronJobs
Services
Ingress
Helm
```

---

## CKS Quick Revision Sheet

```
RBAC
Pod Security
Network Policies
Falco
OPA
Kyverno
Image Scanning
Runtime Security
Supply Chain Security
```

---

# 📎 Official Documentation

* [Kubernetes Documentation](https://kubernetes.io/docs/)
* [kubectl Reference](https://kubernetes.io/docs/reference/kubectl/)
* [Kubernetes API Reference](https://kubernetes.io/docs/reference/kubernetes-api/)
* [Kubernetes Concepts](https://kubernetes.io/docs/concepts/)
* [Kubernetes Tasks Guide](https://kubernetes.io/docs/tasks/)
* [Kubernetes Tutorials](https://kubernetes.io/docs/tutorials/)
* [Helm Documentation](https://helm.sh/docs/)
* [Prometheus Documentation](https://prometheus.io/docs/)
* [Argo CD Documentation](https://argo-cd.readthedocs.io/)
* [Cilium Documentation](https://docs.cilium.io/)
* [Istio Documentation](https://istio.io/latest/docs/)
* [OpenTelemetry Documentation](https://opentelemetry.io/docs/)

---

# 📄 Copyright

This cheatsheet is provided for educational, learning, certification preparation, interview preparation, and operational reference purposes.

Kubernetes®, Helm®, Prometheus®, Grafana®, Argo CD®, OpenTelemetry®, Cilium®, Istio®, Falco®, containerd®, CRI-O®, and related trademarks belong to their respective owners.

This document does not claim ownership of any trademark, logo, product, project, or technology referenced herein.

For licensing information, refer to the respective official project documentation.

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for:

```text
DevOps Engineers
Site Reliability Engineers (SRE)
Platform Engineers
Cloud Engineers
Cloud Architects
Kubernetes Administrators
Container Platform Teams
CKA Preparation
CKAD Preparation
CKS Preparation
Interview Preparation
```

Primary references include:

```text
Official Kubernetes Documentation
CNCF Documentation
Helm Documentation
Prometheus Documentation
Argo CD Documentation
OpenTelemetry Documentation
Cilium Documentation
Istio Documentation
Falco Documentation
Industry Best Practices
Production Kubernetes Experience
```

Always refer to official documentation before implementing changes in production environments.

---

# 🎉 Conclusion

You now understand:

```text
Container Fundamentals
Kubernetes Architecture
Control Plane Components
Worker Nodes
Pods
ReplicaSets
Deployments
Services
Ingress
Networking
DNS
Network Policies
ConfigMaps
Secrets
Storage
Persistent Volumes
Persistent Volume Claims
Storage Classes
CSI
StatefulSets
DaemonSets
Jobs
CronJobs
Scheduling
RBAC
Authentication
Authorization
Security Contexts
Pod Security Standards
Observability
Logging
Monitoring
Prometheus
Grafana
OpenTelemetry
Service Mesh
Istio
Cilium
Cluster Administration
High Availability
Disaster Recovery
Autoscaling
HPA
VPA
Cluster Autoscaler
CRDs
Operators
Admission Controllers
Webhooks
GitOps
Helm
Kustomize
Argo CD
Flux
Platform Engineering
Vault
OPA
Kyverno
Falco
Multi-Cluster Kubernetes
EKS
AKS
GKE
Troubleshooting
Kubernetes Internals
eBPF
Runtime Security
Expert-Level Kubernetes Concepts
```

---

```text
Kubernetes provides:

Container Orchestration
+
Self Healing
+
Auto Scaling
+
Service Discovery
+
Load Balancing
+
Storage Orchestration
+
Security
+
Observability
+
GitOps
+
Platform Engineering
+
Cloud Native Operations
```

and remains the industry standard platform for running modern cloud-native applications at scale.

---

```md
**Last Updated:** 2026
**Platform:** Kubernetes
**Version:** Kubernetes 1.34+
**License:** Apache License 2.0

For the latest information, always refer to the official Kubernetes documentation.
```