# containerd Cheatsheet

![containerd Logo](https://containerd.io/img/logos/containerd-icon-color.svg)

---

## Table of Contents

* [1. What is containerd?](#1-what-is-containerd)
* [2. Why containerd?](#2-why-containerd)
* [3. Container Runtime Fundamentals](#3-container-runtime-fundamentals)
* [4. containerd Architecture](#4-containerd-architecture)
* [5. containerd Components](#5-containerd-components)
* [6. Installation & Verification](#6-installation--verification)
* [7. containerd Configuration](#7-containerd-configuration)
* [8. containerd Service Management](#8-containerd-service-management)
* [9. ctr CLI Basics](#9-ctr-cli-basics)
* [10. Images](#10-images)
* [11. Containers](#11-containers)
* [12. Tasks](#12-tasks)
* [13. Namespaces](#13-namespaces)
* [14. Content Store](#14-content-store)
* [15. Snapshotters](#15-snapshotters)

* [Additional Resources](#additional-resources)
* [Quick Reference](#quick-reference)
* [📎 Official Documentation](#-official-documentation)
* [📄 Copyright](#-copyright)
* [📎 Disclaimer & Attribution](#-disclaimer--attribution)
* [🎉 Conclusion](#-conclusion) 

---

# 1. What is containerd?

containerd is an industry-standard container runtime used to manage the complete lifecycle of containers.

It is a graduated CNCF project and is widely used by Kubernetes, Docker, Amazon EKS, Google GKE, Microsoft AKS, and many cloud-native platforms.

---

## Purpose

```
Pull Images
Store Images
Create Containers
Run Containers
Manage Container Lifecycle
```

---

## High-Level Architecture

```
Applications
      │
      ▼
containerd
      │
      ▼
runc
      │
      ▼
Linux Kernel
```

---

## Common Use Cases

```
Kubernetes
Cloud Platforms
Microservices
Containers
DevOps Workloads
```

---

## Benefits

```
Lightweight
Reliable
Fast
Cloud Native
Production Ready
```

---

# 2. Why containerd?

Modern container platforms require a dedicated runtime.

---

## Before containerd

```
Docker
 │
 ▼
Monolithic Architecture
```

---

## After containerd

```
Docker
 │
 ▼
containerd
 │
 ▼
runc
```

---

## Advantages

```
Smaller Footprint
Better Performance
Simpler Architecture
Kubernetes Native
```

---

## Common Platforms

```
Kubernetes
Amazon EKS
Google GKE
Microsoft AKS
OpenShift
```

---

# 3. Container Runtime Fundamentals

A container runtime is responsible for running containers.

---

## Runtime Responsibilities

```
Image Management
Container Lifecycle
Storage
Networking
Execution
```

---

## Container Workflow

```
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

```text id="n1v4kp"
Container Runtime
        │
        ▼
OCI Runtime
        │
        ▼
Linux Kernel
```

---

## Examples

```text id="k8w2mr"
containerd
CRI-O
Docker Engine
```

---

# 4. containerd Architecture

containerd follows a layered architecture.

---

## Architecture

```text id="c7m5pv"
Kubernetes
     │
     ▼
CRI Plugin
     │
     ▼
containerd
     │
     ▼
runc
     │
     ▼
Containers
```

---

## Internal Components

```text id="r4n8wk"
API
Plugins
Snapshotters
Content Store
Runtime
```

---

## Lifecycle Flow

```text id="x2p7mv"
Pull Image
    │
    ▼
Create Container
    │
    ▼
Run Task
```

---

## Benefits

```text id="f9w3kr"
Modular
Scalable
Extensible
```

---

# 5. containerd Components

containerd consists of multiple components.

---

## Core Components

```text id="u6m1pt"
Daemon
Plugins
Snapshotters
Content Store
Runtime
```

---

## Architecture

```text id="d3v8qn"
containerd
    │
 ┌──┼──┐
 ▼  ▼  ▼
Images
Containers
Tasks
```

---

## Runtime Layer

```text id="w7n4pk"
containerd
     │
     ▼
runc
```

---

## Storage Layer

```text id="m2p9vr"
Content Store
Snapshotters
Metadata
```

---

# 6. Installation & Verification

containerd is available on most Linux distributions.

---

## Verify Installation

```bash id="h5r1wn"
containerd --version
```

---

## Check Service

```bash id="p8m4vx"
systemctl status containerd
```

---

## View Runtime Version

```bash id="x4n7kp"
ctr version
```

---

## Verify Process

```bash id="n6p2wr"
ps -ef | grep containerd
```

---

## Expected Workflow

```text id="k1v8mq"
Install
   │
   ▼
Start Service
   │
   ▼
Verify
```

---

# 7. containerd Configuration

Configuration is managed through a TOML file.

---

## Default Location

```text id="r9m3wt"
/etc/containerd/config.toml
```

---

## Generate Default Config

```bash id="u2p7vn"
containerd config default
```

---

## Save Default Config

```bash id="q6m4kr"
containerd config default \
> /etc/containerd/config.toml
```

---

## Common Settings

```text id="t8w1pv"
Runtime
Snapshotter
Registry Mirrors
CRI Plugin
```

---

## Configuration Flow

```text id="g5n9mx"
Config File
     │
     ▼
containerd
     │
     ▼
Runtime Behavior
```

---

# 8. containerd Service Management

containerd runs as a Linux service.

---

## Start Service

```bash id="v3r8wk"
systemctl start containerd
```

---

## Stop Service

```bash id="n5p2mx"
systemctl stop containerd
```

---

## Restart Service

```bash id="k7w4pn"
systemctl restart containerd
```

---

## Enable at Boot

```bash id="x9m1vr"
systemctl enable containerd
```

---

## Check Status

```bash id="d4p7wk"
systemctl status containerd
```

---

# 9. ctr CLI Basics

`ctr` is the native containerd command-line tool.

---

## Show Version

```bash id="f8m2vp"
ctr version
```

---

## Show Help

```bash id="r1n9wk"
ctr --help
```

---

## List Plugins

```bash id="t6p3mv"
ctr plugins ls
```

---

## List Namespaces

```bash id="m4w8kr"
ctr namespaces list
```

---

## Architecture

```text id="p7n1vx"
ctr
 │
 ▼
containerd API
 │
 ▼
Resources
```

---

# 10. Images

Images contain application code and dependencies.

---

## Pull Image

```bash id="x5r2wp"
ctr image pull docker.io/library/nginx:latest
```

---

## List Images

```bash id="q8m4vn"
ctr images ls
```

---

## Remove Image

```bash id="k3p7wr"
ctr images rm IMAGE_NAME
```

---

## Image Lifecycle

```text id="n7v1mk"
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

```text id="r2m8wp"
Reusable
Portable
Versioned
```

---

# 11. Containers

Containers are created from images.

---

## Create Container

```bash id="v6p4kr"
ctr container create \
docker.io/library/nginx:latest nginx
```

---

## List Containers

```bash id="x1m7wv"
ctr containers ls
```

---

## Delete Container

```bash id="t4p9mk"
ctr containers rm nginx
```

---

## Architecture

```text id="k8w2pn"
Image
  │
  ▼
Container
```

---

## Characteristics

```text id="m3r6vx"
Isolated
Portable
Lightweight
```

---

# 12. Tasks

A task is a running container process.

---

## Concept

```text id="r5n8wp"
Container
    │
    ▼
Task
    │
    ▼
Running Process
```

---

## Start Task

```bash id="q2m7vr"
ctr task start nginx
```

---

## List Tasks

```bash id="u9p3wk"
ctr tasks ls
```

---

## Stop Task

```bash id="w4m1pn"
ctr tasks kill nginx
```

---

## Benefits

```text id="f7r5vx"
Lifecycle Management
Process Control
```

---

# 13. Namespaces

Namespaces separate containerd resources.

---

## Purpose

Isolation between applications and platforms.

---

## Common Namespaces

```text id="m8p2wk"
default
k8s.io
moby
```

---

## List Namespaces

```bash id="r3n7vp"
ctr namespaces list
```

---

## Architecture

```text id="x6m4wr"
containerd
     │
 ┌───┼───┐
 ▼   ▼   ▼
NS1 NS2 NS3
```

---

## Benefits

```text id="k9w1pn"
Isolation
Organization
Multi-Tenancy
```

---

# 14. Content Store

The Content Store stores image data.

---

## Purpose

Maintain image layers and metadata.

---

## Architecture

```text id="v2m8kr"
Registry
   │
   ▼
Content Store
   │
   ▼
Images
```

---

## Contains

```text id="q5p4wn"
Layers
Metadata
Manifests
Blobs
```

---

## Benefits

```text id="n1r7vx"
Efficient Storage
Layer Reuse
Fast Pulls
```

---

# 15. Snapshotters

Snapshotters manage container filesystem layers.

---

## Purpose

Provide writable container filesystems.

---

## Architecture

```text id="w8m3pk"
Image Layers
      │
      ▼
Snapshotter
      │
      ▼
Container Filesystem
```

---

## Common Snapshotters

```text id="r4p9vn"
overlayfs
native
btrfs
zfs
devmapper
```

---

## Benefits

```text id="m6w2kr"
Efficient Storage
Layer Sharing
Performance
```

---

## Workflow

```text id="t7n5vp"
Image
  │
  ▼
Snapshot
  │
  ▼
Container
```

---

# 16. OCI Standards

containerd follows Open Container Initiative (OCI) standards.

---

## Purpose

Provide standard specifications for containers.

---

## OCI Components

```text id="x4m8vp"
OCI Runtime Spec
OCI Image Spec
OCI Distribution Spec
```

---

## Architecture

```text id="p7n2wk"
OCI Image
    │
    ▼
containerd
    │
    ▼
OCI Runtime
```

---

## Benefits

```text id="m3r6vx"
Portability
Compatibility
Vendor Neutrality
```

---

# 17. containerd and Kubernetes

containerd is the most widely used Kubernetes container runtime.

---

## Architecture

```text id="k8p4wr"
Kubernetes
     │
     ▼
kubelet
     │
     ▼
CRI
     │
     ▼
containerd
```

---

## Responsibilities

```text id="v5n9mk"
Image Pulling
Container Creation
Container Execution
Container Cleanup
```

---

## Common Platforms

```text id="t2w7pn"
Amazon EKS
Google GKE
Azure AKS
OpenShift
```

---

## Benefits

```text id="r6m1vx"
Cloud Native
Reliable
Production Ready
```

---

# 18. CRI (Container Runtime Interface)

CRI is the interface between Kubernetes and container runtimes.

---

## Purpose

Allow Kubernetes to use different runtimes.

---

## Architecture

```text id="q4p8wk"
Kubernetes
     │
     ▼
CRI
     │
     ▼
containerd
```

---

## Benefits

```text id="m9r2vn"
Runtime Independence
Standardization
Flexibility
```

---

## Common CRI Runtimes

```text id="w7n5pk"
containerd
CRI-O
```

---

# 19. nerdctl

nerdctl is a Docker-compatible CLI for containerd.

---

## Purpose

Provide a familiar container management experience.

---

## Examples

Run Container:

```bash id="x2m8wr"
nerdctl run nginx
```

---

List Containers:

```bash id="k5p4vn"
nerdctl ps
```

---

List Images:

```bash id="t8n1wk"
nerdctl images
```

---

## Architecture

```text id="m4r7vx"
nerdctl
    │
    ▼
containerd
```

---

## Benefits

```text id="q1w9pn"
Docker-like Experience
Easy Adoption
Modern Features
```

---

# 20. Networking Concepts

Containers require networking for communication.

---

## Architecture

```text id="p6m2vk"
Container
     │
     ▼
Network Namespace
     │
     ▼
Host Network
```

---

## Networking Components

```text id="r8w4pn"
Bridge
Veth Pair
Network Namespace
CNI
```

---

## Container Communication

```text id="v3n7mx"
Container A
     │
     ▼
Network
     │
     ▼
Container B
```

---

## Benefits

```text id="k9m1wr"
Isolation
Connectivity
Scalability
```

---

# 21. Storage Concepts

containerd manages container storage layers.

---

## Storage Architecture

```text id="m2p8vk"
Image Layers
      │
      ▼
Snapshotter
      │
      ▼
Container Filesystem
```

---

## Common Storage Objects

```text id="w6n4pr"
Images
Snapshots
Content Store
Volumes
```

---

## Benefits

```text id="r4m9wx"
Layer Reuse
Efficiency
Persistence
```

---

# 22. Security Best Practices

Security is critical in container environments.

---

## Runtime Security

```text id="t7p2vn"
Least Privilege
Minimal Images
Read-Only Filesystems
```

---

## Access Control

```text id="m1w8pk"
RBAC
User Separation
Namespace Isolation
```

---

## Image Security

```text id="q5r4vn"
Signed Images
Trusted Registries
Image Scanning
```

---

## Best Practices

```text id="v9n3wk"
Patch Regularly
Restrict Access
Use Secure Registries
Monitor Activity
```

---

# 23. Monitoring & Logging

Observability helps maintain healthy environments.

---

## What to Monitor

```text id="x3m7pr"
CPU
Memory
Disk
Containers
Tasks
```

---

## Log Sources

```text id="k8p2vn"
containerd
Containers
System Logs
```

---

## Architecture

```text id="r1w9mk"
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

```text id="m6n4vx"
Prometheus
Grafana
ELK Stack
OpenSearch
```

---

# 24. Troubleshooting

Troubleshooting helps resolve runtime issues.

---

## Verify Service

```bash id="p2m8wr"
systemctl status containerd
```

---

## Check Logs

```bash id="v5n4pk"
journalctl -u containerd
```

---

## Verify Version

```bash id="q7m1vn"
ctr version
```

---

## Common Problems

```text id="t4w9pr"
Image Pull Failures
Runtime Errors
Storage Problems
Networking Issues
```

---

## Diagnostic Flow

```text id="m8r2vk"
Problem
   │
   ▼
Logs
   │
   ▼
Root Cause
   │
   ▼
Fix
```

---

# 25. Common Commands

---

## Version

```bash id="w2n7pk"
ctr version
```

---

## Images

```bash id="m5r8vn"
ctr images ls
```

---

Pull Image:

```bash id="p1w4mk"
ctr image pull docker.io/library/nginx:latest
```

---

## Containers

```bash id="r9n2vx"
ctr containers ls
```

---

## Tasks

```bash id="k4m7wp"
ctr tasks ls
```

---

## Namespaces

```bash id="v6p1nr"
ctr namespaces list
```

---

## Plugins

```bash id="t8m4vk"
ctr plugins ls
```

---

# 26. Real-World Architecture

---

## Kubernetes Architecture

```text id="q3n8wr"
Applications
      │
      ▼
Kubernetes
      │
      ▼
containerd
      │
      ▼
runc
      │
      ▼
Linux Kernel
```

---

## Cloud Provider Architecture

```text id="m7p2vk"
EKS
GKE
AKS
 │
 ▼
containerd
```

---

## Node Architecture

```text id="r5w1mn"
Worker Node
    │
    ▼
containerd
    │
    ▼
Containers
```

---

# 27. Interview Questions

---

## What is containerd?

containerd is a CNCF container runtime responsible for managing the complete container lifecycle.

---

## What is the difference between containerd and Docker?

| Docker            | containerd            |
| ----------------- | --------------------- |
| Full Platform     | Runtime               |
| Includes CLI      | Backend Runtime       |
| Higher-Level Tool | Lower-Level Component |

---

## What is a Task?

A running process associated with a container.

---

## What is a Namespace?

A logical separation mechanism inside containerd.

---

## What is a Snapshotter?

A component that manages container filesystem layers.

---

## What is CRI?

Container Runtime Interface used by Kubernetes.

---

## What is nerdctl?

A Docker-compatible CLI for containerd.

---

## Why did Kubernetes move away from Dockershim?

To communicate directly with CRI-compliant runtimes such as containerd.

---

# 28. Additional Resources

## Official Resources

* containerd Documentation
* CNCF Documentation
* OCI Specifications
* nerdctl Documentation

---

## Learning Resources

* containerd GitHub Repository
* Kubernetes Documentation
* CNCF Landscape
* OCI Documentation

---

## Related Technologies

```text id="p8m3wr"
Kubernetes
CRI-O
Docker
runc
nerdctl
```

---

# 29. Quick Reference

## Service Management

```bash id="n4w8pk"
systemctl status containerd

systemctl restart containerd
```

---

## Images

```bash id="m1p7vn"
ctr images ls

ctr image pull IMAGE
```

---

## Containers

```bash id="r6m2wk"
ctr containers ls
```

---

## Tasks

```bash id="v9n4pr"
ctr tasks ls
```

---

## Namespaces

```bash id="q2w8mk"
ctr namespaces list
```

---

## Plugins

```bash id="k7m1vn"
ctr plugins ls
```

---

## Version

```bash id="t5p4wr"
ctr version
```

---

# 📎 Official Documentation

* containerd Documentation: https://containerd.io
* containerd GitHub: https://github.com/containerd/containerd
* OCI Specifications: https://opencontainers.org
* Kubernetes Documentation: https://kubernetes.io/docs

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, DevOps engineering, platform engineering, Kubernetes administration, and cloud-native operations.

Primary references include:

* containerd Documentation
* CNCF Documentation
* OCI Specifications
* Kubernetes Documentation

containerd is a graduated CNCF project. Kubernetes is a trademark of The Linux Foundation.

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

You now understand:

* containerd Fundamentals
* OCI Standards
* CRI
* Kubernetes Integration
* Images
* Containers
* Tasks
* Namespaces
* Snapshotters
* Content Store
* Networking
* Storage
* Security
* Monitoring
* Troubleshooting
* nerdctl

containerd provides:

```text id="w1m8vk"
Image Management
+
Container Lifecycle
+
OCI Runtime Support
+
CRI Integration
+
Kubernetes Compatibility
+
Cloud Native Foundation
```

and serves as one of the most widely deployed container runtimes in modern Kubernetes and cloud-native environments.

---

**Last Updated:** 2026
**Platform:** containerd
**Version:** containerd 2.x+
**License:** Apache License 2.0

For latest information, visit https://containerd.io