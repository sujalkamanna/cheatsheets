# CRI-O Cheatsheet

![CRI-O Logo](https://cri-o.io/assets/images/logo/crio-logo.svg)

---

## Table of Contents

* [1. What is CRI-O?](#1-what-is-cri-o)
* [2. Why CRI-O?](#2-why-cri-o)
* [3. Container Runtime Fundamentals](#3-container-runtime-fundamentals)
* [4. CRI-O Architecture](#4-cri-o-architecture)
* [5. CRI-O Components](#5-cri-o-components)
* [6. Installation & Verification](#6-installation--verification)
* [7. CRI-O Configuration](#7-cri-o-configuration)
* [8. CRI-O Service Management](#8-cri-o-service-management)
* [9. crictl CLI Basics](#9-crictl-cli-basics)
* [10. Images](#10-images)
* [11. Containers](#11-containers)
* [12. Pods](#12-pods)
* [13. Sandboxes](#13-sandboxes)
* [14. Storage Architecture](#14-storage-architecture)
* [15. OCI Runtime Integration](#15-oci-runtime-integration)

* [Additional Resources](#additional-resources)
* [Quick Reference](#quick-reference)
* [📎 Official Documentation](#-official-documentation)
* [📄 Copyright](#-copyright)
* [📎 Disclaimer & Attribution](#-disclaimer--attribution)
* [🎉 Conclusion](#-conclusion) 

---

# 1. What is CRI-O?

CRI-O is a lightweight container runtime specifically designed for Kubernetes.

It implements the Kubernetes Container Runtime Interface (CRI) and focuses only on running Kubernetes containers.

---

## Purpose

```text
Run Kubernetes Containers
Manage Container Lifecycle
Pull Container Images
Provide CRI Support
Execute OCI Runtimes
```

---

## High-Level Architecture

```text
Kubernetes
     │
     ▼
CRI-O
     │
     ▼
OCI Runtime
(runc/crun)
     │
     ▼
Linux Kernel
```

---

## Common Platforms

```text
Kubernetes
OpenShift
Private Clouds
Cloud Native Platforms
```

---

## Benefits

```text
Lightweight
Kubernetes Native
Secure
Production Ready
Open Source
```

---

# 2. Why CRI-O?

CRI-O was created specifically for Kubernetes.

Unlike Docker, it focuses only on Kubernetes requirements.

---

## Traditional Approach

```text
Kubernetes
     │
     ▼
Docker
     │
     ▼
Containers
```

---

## CRI-O Approach

```text
Kubernetes
     │
     ▼
CRI-O
     │
     ▼
Containers
```

---

## Advantages

```text
Smaller Footprint
Less Complexity
Native CRI Support
Kubernetes Focused
```

---

## Common Use Cases

```text
Kubernetes Clusters
OpenShift Clusters
Container Platforms
Cloud Native Infrastructure
```

---

# 3. Container Runtime Fundamentals

A container runtime manages container execution.

---

## Responsibilities

```text
Image Management
Container Lifecycle
Networking Integration
Storage Management
Process Execution
```

---

## Runtime Workflow

```text
Image
  │
  ▼
Container
  │
  ▼
Running Process
```

---

## Runtime Layers

```text
Kubernetes
     │
     ▼
Container Runtime
     │
     ▼
OCI Runtime
     │
     ▼
Linux Kernel
```

---

## Popular Runtimes

```text
CRI-O
containerd
Docker Engine
```

---

# 4. CRI-O Architecture

CRI-O acts as a bridge between Kubernetes and OCI runtimes.

---

## Architecture Diagram

```text
Kubernetes
     │
     ▼
kubelet
     │
     ▼
CRI
     │
     ▼
CRI-O
     │
     ▼
runc / crun
     │
     ▼
Containers
```

---

## Internal Components

```text
CRI Server
Image Manager
Container Manager
Storage Manager
Runtime Manager
```

---

## Container Lifecycle

```text
Pull Image
    │
    ▼
Create Pod
    │
    ▼
Create Container
    │
    ▼
Start Container
```

---

## Benefits

```text
Modular
Efficient
Kubernetes Optimized
```

---

# 5. CRI-O Components

CRI-O consists of several internal components.

---

## Core Components

```text
CRI API
Image Service
Container Service
Storage Layer
OCI Runtime
```

---

## Architecture

```text
CRI-O
 │
 ├── Images
 ├── Pods
 ├── Containers
 └── Runtime
```

---

## Runtime Layer

```text
CRI-O
   │
   ▼
runc / crun
```

---

## Storage Layer

```text
Image Storage
Container Storage
Overlay Filesystem
```

---

# 6. Installation & Verification

CRI-O is installed on Linux systems running Kubernetes.

---

## Verify Installation

```bash
crio --version
```

---

## Verify Service

```bash
systemctl status crio
```

---

## Verify Runtime

```bash
crictl version
```

---

## Verify Process

```bash
ps -ef | grep crio
```

---

## Workflow

```text
Install
   │
   ▼
Start Service
   │
   ▼
Verify
```

---

# 7. CRI-O Configuration

CRI-O uses a configuration file for runtime settings.

---

## Default Configuration File

```text
/etc/crio/crio.conf
```

---

## Common Configuration Areas

```text
Runtime
Storage
Registries
Networking
Logging
```

---

## Configuration Flow

```text
Configuration File
        │
        ▼
CRI-O
        │
        ▼
Runtime Behavior
```

---

## Common Settings

```text
Default Runtime
Pause Image
Log Directory
Storage Driver
```

---

# 8. CRI-O Service Management

CRI-O runs as a systemd service.

---

## Start Service

```bash
systemctl start crio
```

---

## Stop Service

```bash
systemctl stop crio
```

---

## Restart Service

```bash
systemctl restart crio
```

---

## Enable at Boot

```bash
systemctl enable crio
```

---

## Check Status

```bash
systemctl status crio
```

---

# 9. crictl CLI Basics

crictl is the command-line interface for CRI-compatible runtimes.

---

## Verify Version

```bash
crictl version
```

---

## View Runtime Info

```bash
crictl info
```

---

## List Images

```bash
crictl images
```

---

## List Containers

```bash
crictl ps
```

---

## List Pods

```bash
crictl pods
```

---

## Architecture

```text
crictl
   │
   ▼
CRI API
   │
   ▼
CRI-O
```

---

# 10. Images

Images contain application code and dependencies.

---

## Pull Image

```bash
crictl pull nginx:latest
```

---

## List Images

```bash
crictl images
```

---

## Remove Image

```bash
crictl rmi IMAGE_ID
```

---

## Image Workflow

```text
Registry
   │
   ▼
Image
   │
   ▼
Container
```

---

## Benefits

```text
Portability
Consistency
Versioning
```

---

# 11. Containers

Containers are created from images.

---

## Container Lifecycle

```text
Image
  │
  ▼
Container
  │
  ▼
Running Process
```

---

## List Containers

```bash
crictl ps -a
```

---

## Inspect Container

```bash
crictl inspect CONTAINER_ID
```

---

## Stop Container

```bash
crictl stop CONTAINER_ID
```

---

## Benefits

```text
Isolation
Efficiency
Portability
```

---

# 12. Pods

Pods are the basic execution unit in Kubernetes.

---

## Pod Architecture

```text
Pod
 │
 ├── Container A
 ├── Container B
 └── Shared Network
```

---

## List Pods

```bash
crictl pods
```

---

## Inspect Pod

```bash
crictl inspectp POD_ID
```

---

## Pod Lifecycle

```text
Create
  │
  ▼
Running
  │
  ▼
Terminated
```

---

## Benefits

```text
Shared Networking
Shared Storage
Application Grouping
```

---

# 13. Sandboxes

Each Kubernetes Pod has a sandbox container.

---

## Purpose

Provide the Pod environment.

---

## Architecture

```text
Pod
 │
 ▼
Sandbox
 │
 ├── Network Namespace
 ├── IPC Namespace
 └── Containers
```

---

## View Sandboxes

```bash
crictl pods
```

---

## Benefits

```text
Pod Isolation
Networking
Namespace Management
```

---

# 14. Storage Architecture

CRI-O uses container storage layers.

---

## Storage Components

```text
Images
Containers
Writable Layers
Overlay Storage
```

---

## Architecture

```text
Container Image
       │
       ▼
Overlay Filesystem
       │
       ▼
Container Layer
```

---

## Common Storage Driver

```text
overlayfs
```

---

## Benefits

```text
Efficient Storage
Layer Reuse
Performance
```

---

# 15. OCI Runtime Integration

CRI-O uses OCI-compliant runtimes.

---

## Common OCI Runtimes

```text
runc
crun
kata-containers
```

---

## Architecture

```text
CRI-O
   │
   ▼
OCI Runtime
   │
   ▼
Container
```

---

## OCI Workflow

```text
CRI-O
   │
   ▼
OCI Spec
   │
   ▼
Runtime
   │
   ▼
Container
```

---

## Benefits

```text
Standards Compliance
Flexibility
Runtime Choice
Security
```

---

# 16. OCI Standards

CRI-O follows Open Container Initiative (OCI) standards.

---

## Purpose

Provide standardized container formats and runtimes.

---

## OCI Specifications

```text
OCI Runtime Specification
OCI Image Specification
OCI Distribution Specification
```

---

## Architecture

```text
OCI Image
    │
    ▼
CRI-O
    │
    ▼
OCI Runtime
```

---

## Benefits

```text
Portability
Compatibility
Vendor Neutrality
```

---

# 17. CRI (Container Runtime Interface)

CRI is the interface used by Kubernetes to communicate with runtimes.

---

## Purpose

Allow Kubernetes to support multiple runtimes.

---

## Architecture

```text
Kubernetes
     │
     ▼
CRI
     │
     ▼
CRI-O
```

---

## Benefits

```text
Standardization
Flexibility
Runtime Independence
```

---

## CRI-Compliant Runtimes

```text
CRI-O
containerd
```

---

# 18. CRI-O and Kubernetes

CRI-O is designed specifically for Kubernetes.

---

## Kubernetes Architecture

```text
Kubernetes
     │
     ▼
kubelet
     │
     ▼
CRI-O
     │
     ▼
Containers
```

---

## Responsibilities

```text
Image Management
Pod Lifecycle
Container Execution
Resource Management
```

---

## Benefits

```text
Native Kubernetes Support
Lightweight
Reliable
```

---

## Common Platforms

```text
Kubernetes
OpenShift
Private Clouds
Managed Kubernetes Services
```

---

# 19. crictl Deep Dive

crictl is the primary troubleshooting and management tool.

---

## Show Runtime Information

```bash
crictl info
```

---

## Inspect Container

```bash
crictl inspect CONTAINER_ID
```

---

## Inspect Pod

```bash
crictl inspectp POD_ID
```

---

## Container Logs

```bash
crictl logs CONTAINER_ID
```

---

## Execute Command

```bash
crictl exec -it CONTAINER_ID sh
```

---

## Architecture

```text
crictl
   │
   ▼
CRI API
   │
   ▼
CRI-O
```

---

# 20. Networking Concepts

Containers require networking for communication.

---

## Networking Architecture

```text
Pod
 │
 ▼
Network Namespace
 │
 ▼
CNI Plugin
 │
 ▼
Network
```

---

## Networking Components

```text
CNI
Network Namespace
Veth Pair
Bridge
```

---

## Common CNI Plugins

```text
Calico
Cilium
Flannel
Weave
```

---

## Benefits

```text
Isolation
Connectivity
Scalability
```

---

# 21. Storage Concepts

CRI-O relies on container storage layers.

---

## Storage Architecture

```text
Image
  │
  ▼
Storage Driver
  │
  ▼
Container Filesystem
```

---

## Storage Components

```text
Images
Writable Layers
Volumes
OverlayFS
```

---

## Common Storage Driver

```text
overlayfs
```

---

## Benefits

```text
Efficient Storage
Layer Sharing
Fast Startup
```

---

# 22. Security Best Practices

Security is critical in containerized environments.

---

## Runtime Security

```text
Least Privilege
Read-Only Containers
Minimal Images
```

---

## Image Security

```text
Trusted Registries
Image Scanning
Image Signing
```

---

## Kubernetes Security

```text
RBAC
Network Policies
Pod Security Standards
```

---

## Best Practices

```text
Regular Updates
Namespace Isolation
Access Control
Monitoring
```

---

# 23. Monitoring & Logging

Monitoring provides visibility into runtime operations.

---

## What to Monitor

```text
CPU
Memory
Storage
Containers
Pods
```

---

## Log Sources

```text
CRI-O
Containers
Kubelet
System Logs
```

---

## Architecture

```text
Containers
     │
     ▼
Logs
     │
     ▼
Monitoring Platform
```

---

## Common Tools

```text
Prometheus
Grafana
ELK Stack
OpenSearch
```

---

# 24. Troubleshooting

Troubleshooting helps identify runtime issues.

---

## Check Service

```bash
systemctl status crio
```

---

## View Logs

```bash
journalctl -u crio
```

---

## Verify Runtime

```bash
crictl info
```

---

## Check Containers

```bash
crictl ps -a
```

---

## Common Issues

```text
Image Pull Failures
Pod Startup Failures
Storage Errors
Network Problems
```

---

## Troubleshooting Flow

```text
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

# 25. Common Commands

---

## Version

```bash
crio --version

crictl version
```

---

## Images

```bash
crictl images
```

Pull Image:

```bash
crictl pull nginx:latest
```

---

## Containers

```bash
crictl ps -a
```

---

## Pods

```bash
crictl pods
```

---

## Logs

```bash
crictl logs CONTAINER_ID
```

---

## Runtime Info

```bash
crictl info
```

---

# 26. Real-World Architecture

---

## Kubernetes Cluster

```text
Applications
      │
      ▼
Kubernetes
      │
      ▼
CRI-O
      │
      ▼
OCI Runtime
```

---

## OpenShift Architecture

```text
OpenShift
     │
     ▼
CRI-O
     │
     ▼
Containers
```

---

## Worker Node Architecture

```text
Worker Node
     │
     ▼
CRI-O
     │
     ▼
Pods
```

---

## Enterprise Architecture

```text
Users
  │
  ▼
Kubernetes
  │
  ▼
CRI-O
  │
  ▼
Infrastructure
```

---

# 27. CRI-O vs containerd

| Feature           | CRI-O | containerd     |
| ----------------- | ----- | -------------- |
| CNCF Project      | Yes   | Yes            |
| Kubernetes Focus  | High  | High           |
| CRI Native        | Yes   | Via CRI Plugin |
| Complexity        | Lower | Moderate       |
| OpenShift Default | Yes   | No             |

---

## Architecture Comparison

```text
Kubernetes
     │
     ▼
CRI-O
```

Direct CRI implementation.

---

```text
Kubernetes
     │
     ▼
CRI Plugin
     │
     ▼
containerd
```

Plugin-based CRI implementation.

---

# 28. Interview Questions

---

## What is CRI-O?

CRI-O is a lightweight Kubernetes-native container runtime implementing the CRI standard.

---

## What is CRI?

Container Runtime Interface used by Kubernetes to communicate with runtimes.

---

## What is crictl?

A CLI tool used to manage and troubleshoot CRI-compatible runtimes.

---

## What is a Pod Sandbox?

The infrastructure container that provides networking and namespaces for a Pod.

---

## Difference Between CRI-O and Docker?

| CRI-O              | Docker            |
| ------------------ | ----------------- |
| Runtime Only       | Complete Platform |
| Kubernetes Focused | General Purpose   |
| Lightweight        | Larger Footprint  |

---

## Difference Between CRI-O and containerd?

CRI-O is Kubernetes-specific while containerd is a general-purpose container runtime.

---

## What OCI Runtime Does CRI-O Use?

```text
runc
crun
kata-containers
```

---

# 29. Additional Resources

## Official Resources

* CRI-O Documentation
* Kubernetes Documentation
* OCI Specifications
* OpenShift Documentation

---

## Learning Resources

* CRI-O GitHub Repository
* CNCF Documentation
* Kubernetes Documentation
* OCI Documentation

---

## Related Technologies

```text
Kubernetes
containerd
Docker
runc
crictl
OpenShift
```

---

# 30. Quick Reference

## Service Management

```bash
systemctl status crio

systemctl restart crio
```

---

## Images

```bash
crictl images

crictl pull IMAGE
```

---

## Containers

```bash
crictl ps -a
```

---

## Pods

```bash
crictl pods
```

---

## Logs

```bash
crictl logs CONTAINER_ID
```

---

## Runtime Info

```bash
crictl info
```

---

## Version

```bash
crio --version

crictl version
```

---

# 📎 Official Documentation

* CRI-O Documentation: https://cri-o.io
* CRI-O GitHub: https://github.com/cri-o/cri-o
* Kubernetes Documentation: https://kubernetes.io/docs
* OCI Specifications: https://opencontainers.org

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, Kubernetes administration, DevOps engineering, platform engineering, and cloud-native operations.

Primary references include:

* CRI-O Documentation
* Kubernetes Documentation
* OCI Specifications
* OpenShift Documentation

CRI-O is an open-source Kubernetes container runtime. Kubernetes is a trademark of The Linux Foundation.

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

You now understand:

* CRI-O Fundamentals
* CRI Architecture
* OCI Standards
* Kubernetes Integration
* Images
* Containers
* Pods
* Sandboxes
* Storage
* Networking
* Security
* Monitoring
* Troubleshooting
* crictl

CRI-O provides:

```text
Kubernetes Native Runtime
+
CRI Compliance
+
OCI Runtime Support
+
Pod Lifecycle Management
+
Security
+
Cloud Native Integration
```

and serves as one of the most lightweight and Kubernetes-focused container runtimes available for modern cloud-native platforms.

---

**Last Updated:** 2026
**Platform:** CRI-O
**Version:** CRI-O 1.34+
**License:** Apache License 2.0

For latest information, visit https://cri-o.io