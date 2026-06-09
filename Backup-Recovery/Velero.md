# Velero Backup & Disaster Recovery

![Velero Logo](https://velero.io/img/Velero.svg)

---

## Table of Contents

- [1. What is Velero?](#1-what-is-velero)
- [2. Core Concepts](#2-core-concepts)
- [3. Architecture](#3-architecture)
- [4. Prerequisites](#4-prerequisites)
- [5. Installation & Setup](#5-installation--setup)
- [6. Backup Storage Locations (BSL)](#6-backup-storage-locations-bsl)
- [7. Volume Snapshot Locations (VSL)](#7-volume-snapshot-locations-vsl)
- [8. Backup Operations](#8-backup-operations)
- [9. Restore Operations](#9-restore-operations)
- [10. Backup Scheduling](#10-backup-scheduling)
- [11. Namespace & Resource Filtering](#11-namespace--resource-filtering)
- [12. Persistent Volume & CSI Snapshot Backups](#12-persistent-volume--csi-snapshot-backups)
- [13. Disaster Recovery](#13-disaster-recovery)
- [14. Cluster Migration](#14-cluster-migration)
- [15. Multi-Cluster Recovery](#15-multi-cluster-recovery)
- [16. Velero Node-Agent](#16-velero-node-agent)
- [17. Velero and Restic](#17-velero-and-restic)
- [18. Monitoring & Logging](#18-monitoring--logging)
- [19. Security](#19-security)
- [20. Troubleshooting](#20-troubleshooting)
- [21. Best Practices](#21-best-practices)
- [22. Interview Questions](#22-interview-questions)
- [23. Additional Resources](#23-additional-resources)
- [24. Quick Reference](#24-quick-reference)
- [📎 Official Documentation](#-official-documentation)
- [📎 Disclaimer & Attribution](#-disclaimer--attribution)
- [🎉 Conclusion](#-conclusion)

---

# 1. What is Velero?

Velero is an open-source Kubernetes backup, restore, migration, and disaster recovery tool.

It helps protect Kubernetes workloads by backing up:

* Namespaces
* Deployments
* StatefulSets
* DaemonSets
* Services
* ConfigMaps
* Secrets
* Persistent Volumes
* Custom Resources (CRDs)

Velero is widely used in:

* Kubernetes Clusters
* Amazon EKS
* Azure AKS
* Google GKE
* OpenShift
* Rancher
* Self-Managed Kubernetes

---

## Why Velero?

Kubernetes itself does not provide native disaster recovery.

If a cluster fails:

```text
Pods
Namespaces
Secrets
ConfigMaps
Persistent Volumes

can be lost.
```

Velero provides:

```text
Backup
Restore
Migration
Disaster Recovery
```

for Kubernetes resources.

---

## Common Use Cases

### Cluster Backup

```text
Kubernetes Cluster
        │
        ▼
      Velero
        │
        ▼
     AWS S3
```

---

### Namespace Recovery

```text
Deleted Namespace
        │
        ▼
      Restore
        │
        ▼
Namespace Recovered
```

---

### Cluster Migration

```text
Old Cluster
      │
      ▼
    Backup
      │
      ▼
Object Storage
      │
      ▼
Restore
      │
      ▼
New Cluster
```

---

# 2. Core Concepts

Understanding these concepts is essential.

---

## Backup

A backup contains:

```text
Kubernetes Resources
+
Persistent Volume Data
```

Example:

```bash
velero backup create prod-backup
```

---

## Restore

Restores resources from a backup.

Example:

```bash
velero restore create \
--from-backup prod-backup
```

---

## Backup Storage Location (BSL)

Defines where backups are stored.

Examples:

```text
AWS S3
Azure Blob
Google Cloud Storage
MinIO
```

---

## Volume Snapshot Location (VSL)

Defines where volume snapshots are stored.

Examples:

```text
AWS EBS
Azure Disk
GCP Persistent Disk
```

---

## Schedule

Automated recurring backups.

Example:

```bash
velero schedule create daily-backup
```

---

## Snapshot

Point-in-time copy of storage.

```text
Persistent Volume
       │
       ▼
 Snapshot
```

---

## Disaster Recovery

Recover:

```text
Cluster
Namespace
Application
Storage
```

after failures.

---

# 3. Architecture

---

## High-Level Architecture

```text
Kubernetes Cluster
        │
        ▼
      Velero
        │
        ▼
 Backup Storage
(S3/MinIO/Azure/GCS)
```

---

## Backup Flow

```text
Resources
     │
     ▼
 Velero Server
     │
     ▼
 Object Storage
```

---

## Restore Flow

```text
Object Storage
      │
      ▼
     Velero
      │
      ▼
 Kubernetes Cluster
```

---

## Volume Backup Architecture

```text
PVC
 │
 ▼
Snapshot
 │
 ▼
Cloud Provider
 │
 ▼
Recovery
```

---

## Multi-Cluster Migration

```text
Cluster A
    │
    ▼
 Backup
    │
    ▼
Object Storage
    │
    ▼
 Restore
    │
    ▼
Cluster B
```

---

# 4. Prerequisites

Before installing Velero:

---

## Kubernetes Cluster

Supported:

```text
EKS
AKS
GKE
OpenShift
K3s
RKE
Vanilla Kubernetes
```

---

## Kubectl

Verify:

```bash
kubectl version
```

---

## Cluster Access

Verify:

```bash
kubectl get nodes
```

---

## Object Storage

Choose one:

```text
AWS S3
Azure Blob
Google Cloud Storage
MinIO
```

---

## Velero CLI

Required for administration.

---

# 5. Installation & Setup

---

## Install Velero CLI

### Linux

```bash
wget https://github.com/vmware-tanzu/velero/releases/latest/download/velero-linux-amd64.tar.gz
```

Extract:

```bash
tar -xvf velero-linux-amd64.tar.gz
```

Move:

```bash
sudo mv velero /usr/local/bin/
```

Verify:

```bash
velero version
```

---

### macOS

```bash
brew install velero
```

---

### Windows

```powershell
choco install velero
```

---

## Create Namespace

```bash
kubectl create namespace velero
```

---

## Verify Namespace

```bash
kubectl get ns velero
```

---

# 6. Backup Storage Locations (BSL)

Backup Storage Location defines where backups are stored.

---

## Supported Providers

```text
AWS S3
Azure Blob
Google Cloud Storage
MinIO
```

---

## AWS S3 Example

Create bucket:

```text
velero-backups
```

---

Credentials:

```bash
export AWS_ACCESS_KEY_ID=ACCESS_KEY
export AWS_SECRET_ACCESS_KEY=SECRET_KEY
```

---

Install Velero:

```bash
velero install \
--provider aws \
--bucket velero-backups \
--plugins velero/velero-plugin-for-aws
```

---

Verify:

```bash
velero backup-location get
```

---

## MinIO Example

```text
MinIO
   │
   ▼
Velero Backup Storage
```

Install:

```bash
velero install \
--provider aws \
--bucket velero \
--plugins velero/velero-plugin-for-aws
```

---

## View BSL

```bash
velero backup-location get
```

Example:

```text
NAME      PHASE
default   Available
```

---

# 7. Volume Snapshot Locations (VSL)

Volume Snapshot Locations define where volume snapshots are stored.

---

## AWS EBS

```text
PVC
 │
 ▼
EBS Volume
 │
 ▼
Snapshot
```

---

## Azure Disk

```text
PVC
 │
 ▼
Managed Disk
 │
 ▼
Snapshot
```

---

## GCP Persistent Disk

```text
PVC
 │
 ▼
Persistent Disk
 │
 ▼
Snapshot
```

---

## View Snapshot Locations

```bash
velero snapshot-location get
```

---

Example:

```text
NAME      PROVIDER
default   aws
```

---

# 8. Backup Operations

Creating backups is Velero's primary function.

---

## Create Backup

```bash
velero backup create backup-01
```

---

## View Backups

```bash
velero backup get
```

Example:

```text
NAME
backup-01
backup-02
```

---

## Backup Entire Cluster

```bash
velero backup create cluster-backup
```

---

## Describe Backup

```bash
velero backup describe cluster-backup
```

---

## Detailed Output

```bash
velero backup describe \
cluster-backup \
--details
```

---

## Backup Logs

```bash
velero backup logs cluster-backup
```

---

## Backup Specific Namespace

```bash
velero backup create app-backup \
--include-namespaces production
```

---

## Multiple Namespaces

```bash
velero backup create app-backup \
--include-namespaces prod,staging
```

---

## Exclude Namespace

```bash
velero backup create backup \
--exclude-namespaces kube-system
```

---

## Backup Workflow

```text
Resources
    │
    ▼
 Velero
    │
    ▼
 Object Storage
    │
    ▼
 Backup Created
```

---

# 9. Restore Operations

Restores recover data from backups.

---

## Restore Backup

```bash
velero restore create \
--from-backup cluster-backup
```

---

## View Restores

```bash
velero restore get
```

---

## Describe Restore

```bash
velero restore describe restore-01
```

---

## Restore Logs

```bash
velero restore logs restore-01
```

---

## Restore Namespace

```bash
velero restore create \
--from-backup app-backup
```

---

## Restore Workflow

```text
Backup
   │
   ▼
 Velero
   │
   ▼
 Restore
   │
   ▼
 Resources Recovered
```

---

# 10. Backup Scheduling

Automation is critical.

---

## Create Daily Backup

```bash
velero schedule create daily-backup \
--schedule="0 2 * * *"
```

Runs:

```text
Every Day at 2:00 AM
```

---

## Weekly Backup

```bash
velero schedule create weekly-backup \
--schedule="0 3 * * 0"
```

---

## View Schedules

```bash
velero schedule get
```

---

## Describe Schedule

```bash
velero schedule describe daily-backup
```

---

## Delete Schedule

```bash
velero schedule delete daily-backup
```

---

## Schedule Architecture

```text
Schedule
    │
    ▼
 Automatic Backup
    │
    ▼
 Object Storage
```

---

# 11. Namespace & Resource Filtering

Velero allows selective backups.

---

## Include Namespace

```bash
velero backup create app-backup \
--include-namespaces prod
```

---

## Exclude Namespace

```bash
velero backup create app-backup \
--exclude-namespaces kube-system
```

---

## Include Resources

```bash
velero backup create app-backup \
--include-resources deployments
```

---

## Exclude Resources

```bash
velero backup create app-backup \
--exclude-resources secrets
```

---

## Label Selector

```bash
velero backup create app-backup \
--selector app=frontend
```

---

## Example

```text
Namespace
    │
    ▼
 Deployments
 Services
 ConfigMaps
 Secrets
```

Only selected resources are backed up.

---

# 12. Persistent Volume & CSI Snapshot Backups

Persistent storage is often the most important data.

---

## Traditional Snapshot Method

```text
PVC
 │
 ▼
Cloud Snapshot
 │
 ▼
Recovery
```

---

## CSI Snapshot Support

Modern Kubernetes environments use CSI snapshots.

Benefits:

```text
Faster
Native
Cloud-Aware
Efficient
```

---

## Enable CSI

Install Velero with:

```bash
velero install \
--features=EnableCSI
```

---

## Verify CSI Support

```bash
kubectl get volumesnapshotclass
```

---

## Backup PVC Data

```bash
velero backup create pvc-backup
```

---

## Restore PVC

```bash
velero restore create \
--from-backup pvc-backup
```

---

## Storage Backup Workflow

```text
Application
      │
      ▼
 Persistent Volume
      │
      ▼
 CSI Snapshot
      │
      ▼
 Backup Storage
      │
      ▼
 Recovery
```

---

### Most Important Commands

```bash
velero backup create
velero backup get
velero backup logs

velero restore create
velero restore get

velero schedule create
velero schedule get

velero backup-location get
velero snapshot-location get
```
---

# 13. Disaster Recovery

Disaster Recovery (DR) is one of Velero's primary use cases.

When a Kubernetes cluster experiences failure, Velero enables rapid recovery of workloads and data.

---

## Common Disaster Scenarios

```text
Accidental Deletion
Cluster Failure
Region Failure
Cloud Outage
Ransomware Attack
Configuration Corruption
Storage Failure
```

---

## Disaster Recovery Workflow

```text
Production Cluster
        │
        ▼
     Velero
        │
        ▼
 Object Storage
        │
        ▼
   Backup Data
        │
        ▼
    New Cluster
        │
        ▼
      Restore
```

---

## Recover Entire Cluster

Restore all resources:

```bash
velero restore create \
--from-backup cluster-backup
```

---

## Recover Namespace

```bash
velero restore create \
--from-backup prod-backup
```

---

## Recover Deleted Deployment

```text
Deployment Deleted
        │
        ▼
 Backup Exists
        │
        ▼
 Restore
        │
        ▼
 Deployment Recovered
```

---

## DR Best Practice

Always maintain:

```text
Primary Cluster
      │
      ▼
 Backup Storage
      │
      ▼
 Secondary Recovery Cluster
```

---

# 14. Cluster Migration

Velero is widely used for Kubernetes migration.

---

## Migration Workflow

```text
Source Cluster
      │
      ▼
 Backup
      │
      ▼
 Object Storage
      │
      ▼
 Restore
      │
      ▼
Target Cluster
```

---

## Example Use Cases

```text
EKS → EKS
AKS → AKS
GKE → GKE
OpenShift → OpenShift
On-Prem → Cloud
Cloud → Cloud
```

---

## Backup Source Cluster

```bash
velero backup create migration-backup
```

---

## Install Velero on Target Cluster

```bash
velero install
```

---

## Restore

```bash
velero restore create \
--from-backup migration-backup
```

---

## Migration Benefits

```text
Minimal Downtime
Simple Rollback
Repeatable Process
Disaster Recovery Ready
```

---

# 15. Multi-Cluster Recovery

Many organizations maintain multiple Kubernetes clusters.

---

## Example Architecture

```text
Production Cluster
         │
         ▼
      Velero
         │
         ▼
     AWS S3
         │
         ▼
 DR Cluster
```

---

## Recovery Scenario

```text
Cluster A Fails
       │
       ▼
Restore Backup
       │
       ▼
Cluster B Active
```

---

## Benefits

```text
Business Continuity
Reduced Downtime
Cross-Region Recovery
Cloud Migration
```

---

# 16. Velero Node-Agent

Node-Agent is the modern replacement for many Restic-based backup workflows.

---

## Why Node-Agent?

Older Velero versions relied heavily on Restic.

Modern versions use:

```text
Node-Agent
```

for file-system level backups.

---

## Architecture

```text
Velero
   │
   ▼
Node-Agent
   │
   ▼
Persistent Volume
   │
   ▼
Backup Storage
```

---

## Advantages

```text
Better Performance
Simpler Deployment
Native Integration
Improved Reliability
```

---

## Verify Node-Agent

```bash
kubectl get daemonset \
-n velero
```

---

Expected:

```text
node-agent
```

---

## Node-Agent Workflow

```text
PVC
 │
 ▼
Node-Agent
 │
 ▼
Object Storage
```

---

# 17. Velero and Restic

Many engineers encounter both tools.

---

## Velero

Focuses on:

```text
Kubernetes Resources
Cluster Recovery
Migration
Scheduling
```

---

## Restic

Focuses on:

```text
File Backups
Snapshots
Encryption
Deduplication
```

---

## Comparison

| Feature             | Velero             | Restic |
| ------------------- | ------------------ | ------ |
| Kubernetes Backup   | ✅                  | ❌      |
| Cluster Restore     | ✅                  | ❌      |
| Migration           | ✅                  | ❌      |
| File Backup         | ⚠️                 | ✅      |
| Encryption          | Depends on Storage | ✅      |
| Snapshot Management | ✅                  | ✅      |

---

## Modern Recommendation

```text
Kubernetes DR
      │
      ▼
    Velero

File-Level Backup
      │
      ▼
    Restic
```

---

# 18. Monitoring & Logging

Backups must be monitored continuously.

---

## View Backup Status

```bash
velero backup get
```

---

Example:

```text
NAME           STATUS
daily-backup   Completed
weekly-backup  Completed
```

---

## Backup Details

```bash
velero backup describe \
daily-backup
```

---

## Backup Logs

```bash
velero backup logs \
daily-backup
```

---

## Restore Status

```bash
velero restore get
```

---

## Restore Logs

```bash
velero restore logs restore-01
```

---

## Velero Pods

```bash
kubectl get pods \
-n velero
```

---

## Velero Logs

```bash
kubectl logs deployment/velero \
-n velero
```

---

## Monitoring Architecture

```text
Velero
   │
   ▼
Logs
   │
   ▼
Prometheus
   │
   ▼
Grafana
```

---

## Metrics to Monitor

```text
Backup Success Rate
Backup Duration
Restore Success Rate
Failed Backups
Failed Restores
Storage Consumption
```

---

# 19. Security

Backup systems contain sensitive data.

Protect them properly.

---

## IAM Best Practices

Use dedicated backup accounts.

Example:

```text
Velero IAM User
      │
      ▼
S3 Bucket Access Only
```

---

## Avoid

```text
Administrator Access
```

---

## Secure Object Storage

Enable:

```text
Encryption at Rest
Versioning
Access Logging
```

---

## Kubernetes Security

Use:

```text
RBAC
Service Accounts
Least Privilege
```

---

## Verify Access

```bash
kubectl auth can-i '*' '*'
```

---

## Recommended Controls

```text
MFA
Audit Logging
Network Policies
Secret Management
```

---

# 20. Troubleshooting

---

## Backup Failed

Check:

```bash
velero backup describe backup-01
```

---

Logs:

```bash
velero backup logs backup-01
```

---

## Restore Failed

```bash
velero restore describe restore-01
```

---

Logs:

```bash
velero restore logs restore-01
```

---

## Storage Location Unavailable

Check:

```bash
velero backup-location get
```

---

Expected:

```text
STATUS
Available
```

---

## Pod Issues

```bash
kubectl get pods \
-n velero
```

---

Describe Pod:

```bash
kubectl describe pod \
<velero-pod> \
-n velero
```

---

## Common Problems

```text
Invalid Credentials
Bucket Permissions
Network Connectivity
Plugin Issues
Snapshot Failures
```

---

# 21. Best Practices

---

## Use Scheduled Backups

```bash
velero schedule create daily-backup
```

---

## Store Backups Offsite

```text
Cluster
   │
   ▼
Object Storage
```

Never store backups only inside the cluster.

---

## Test Restores

Regularly verify:

```bash
velero restore create
```

---

## Backup Critical Namespaces

```text
Production
Monitoring
Logging
Ingress
Databases
```

---

## Enable CSI Snapshots

Recommended for modern Kubernetes environments.

---

## Monitor Backup Status

```bash
velero backup get
```

---

## Follow 3-2-1 Rule

```text
3 Copies
2 Storage Types
1 Offsite Copy
```

---

# 22. Interview Questions

---

## What is Velero?

An open-source Kubernetes backup, restore, migration, and disaster recovery tool.

---

## What is a Backup Storage Location?

Location where backups are stored.

Examples:

```text
AWS S3
Azure Blob
GCS
MinIO
```

---

## What is a Volume Snapshot Location?

Defines where storage snapshots are created.

---

## Difference Between Backup and Restore?

```text
Backup
└── Creates Recovery Point

Restore
└── Recovers Resources
```

---

## What is Velero Used For?

```text
Backup
Restore
Migration
Disaster Recovery
```

---

## What is CSI Snapshot Support?

Native Kubernetes volume snapshot capability used by Velero.

---

## How Do You Verify Backups?

```bash
velero backup get
```

---

## How Do You View Backup Logs?

```bash
velero backup logs backup-name
```

---

## Velero vs Restic?

```text
Velero
└── Kubernetes Disaster Recovery

Restic
└── File-Level Backup
```

---

# 23. Additional Resources

## Official Documentation

* https://velero.io
* https://velero.io/docs
* https://github.com/vmware-tanzu/velero

---

## Cloud Documentation

* AWS EKS Documentation
* Azure AKS Documentation
* Google GKE Documentation

---

## Kubernetes Documentation

* https://kubernetes.io/docs

---

# 24. Quick Reference

## Backup

```bash
velero backup create backup-01
velero backup get
velero backup describe backup-01
velero backup logs backup-01
```

---

## Restore

```bash
velero restore create \
--from-backup backup-01

velero restore get
velero restore logs restore-01
```

---

## Scheduling

```bash
velero schedule create daily-backup
velero schedule get
velero schedule describe daily-backup
```

---

## Storage

```bash
velero backup-location get
velero snapshot-location get
```

---

## Troubleshooting

```bash
kubectl get pods -n velero
kubectl logs deployment/velero -n velero

velero backup logs backup-01
velero restore logs restore-01
```

---

# 📎 Official Documentation

Official Website:

https://velero.io

Documentation:

https://velero.io/docs

GitHub Repository:

https://github.com/vmware-tanzu/velero

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, Kubernetes disaster recovery, backup, restore, and migration workflows.

Primary references include:

* Velero Official Documentation
* Kubernetes Documentation
* Cloud Provider Documentation
* CSI Snapshot Documentation

Velero is an open-source project maintained by its contributors.

All trademarks, service names, and project names belong to their respective owners.

For authoritative information, always refer to official project documentation.

---

# 🎉 Conclusion

You now understand:

* Velero Architecture
* Backup Operations
* Restore Operations
* Scheduling
* CSI Snapshots
* Backup Storage Locations
* Disaster Recovery
* Cluster Migration
* Multi-Cluster Recovery
* Node-Agent
* Monitoring
* Security
* Troubleshooting
* Best Practices

Velero is one of the most important tools in Kubernetes disaster recovery because it provides:

```text
Backup
+
Restore
+
Migration
+
Disaster Recovery
+
Kubernetes Awareness
```

in a single platform designed specifically for cloud-native environments.

---

**Last Updated:** 2026
**Tool Version:** Velero 1.17+
**License:** Apache License 2.0

For latest information, visit https://velero.io