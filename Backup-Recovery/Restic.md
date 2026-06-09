# Restic Backup & Recovery Cheatsheet

![Restic Logo](https://restic.net/images/logo.png)

---

## Table of Contents

- [1. What is Restic?](#1-what-is-restic)
- [2. Core Concepts](#2-core-concepts)
- [3. Installation and Setup](#3-installation-and-setup)
- [4. Architecture](#4-architecture)
- [5. Repository Configuration](#5-repository-configuration)
- [6. Backup Operations](#6-backup-operations)
- [7. Snapshot Management](#7-snapshot-management)
- [8. Restore Operations](#8-restore-operations)
- [9. Cloud Storage Backends](#9-cloud-storage-backends)
- [10. Security & Encryption](#10-security--encryption)
- [11. Kubernetes Backup & Recovery](#11-kubernetes-backup--recovery)
- [12. Automation (cron & systemd)](#12-automation-cron--systemd)
- [13. Monitoring & Logging](#13-monitoring--logging)
- [14. Advanced Features](#14-advanced-features)
- [15. Troubleshooting](#15-troubleshooting)
- [16. Disaster Recovery Scenarios](#16-disaster-recovery-scenarios)
- [17. Best Practices](#17-best-practices)
- [18. Interview Questions](#18-interview-questions)
- [19. Additional Resources](#19-additional-resources)
- [20. Quick Reference](#20-quick-reference)
- [📎 Official Documentation](#-official-documentation)
- [📄 Copyright](#-copyright)
- [📎 Disclaimer & Attribution](#-disclaimer--attribution)
- [🎉 Conclusion](#-conclusion)

---

# 1. What is Restic?

Restic is a modern, secure, fast, and efficient backup program designed for protecting data across local systems, cloud environments, and Kubernetes workloads.

It provides:

* End-to-end encryption
* Snapshot-based recovery
* Multi-cloud support
* Disaster recovery capabilities
* Incremental backups
* Deduplication
* Encryption
* Snapshot-based recovery

Restic is commonly used in:

* Linux servers
* Kubernetes clusters
* Cloud-native environments
* AWS
* Azure
* Google Cloud
* MinIO object storage
* Home lab environments
* Enterprise backup solutions

---

## Key Benefits

* Open Source
* Single Binary
* Fast Backups
* Fast Restore
* Built-in Encryption
* Deduplication
* Snapshot-Based Recovery
* Multi-Cloud Support
* Automation Friendly
* Kubernetes Compatible

---

## Common Use Cases

### Server Backup

```text
Linux Server
     │
     ▼
  Restic
     │
     ▼
 AWS S3 Bucket
```

---

### Kubernetes Backup

```text
Pods
 │
 ▼
Persistent Volumes
 │
 ▼
Restic
 │
 ▼
Object Storage
```

---

### Disaster Recovery

```text
Production Server
        │
        ▼
      Backup
        │
        ▼
     Restic Repo
        │
        ▼
 Restore to New Server
```

---

# 2. Core Concepts

Understanding these concepts is critical.

---

## Repository

A repository is the location where backups are stored.

Examples:

```text
Local Disk
AWS S3
Azure Blob
Google Cloud Storage
MinIO
Backblaze B2
SFTP Server
```

Example:

```bash
restic -r /backup-repo init
```

---

## Snapshot

A snapshot represents the state of files at a specific time.

```text
Snapshot A
│
├── app.log
├── nginx.conf
└── database.sql

Snapshot B
│
├── app.log
├── nginx.conf
├── database.sql
└── backup.sql
```

Each backup creates a snapshot.

---

## Repository Password

Every repository is encrypted.

Example:

```bash
export RESTIC_PASSWORD="StrongPassword123"
```

Without the password:

```text
No Restore
No Access
No Recovery
```

---

## Deduplication

Restic stores only changed blocks.

```text
Backup 1
10 GB

Backup 2
10.2 GB

Actual Storage
10.2 GB
```

Benefits:

* Reduced storage costs
* Faster backups
* Faster synchronization

---

## Incremental Backup

After the first backup:

```text
Backup 1
 └── Full Backup

Backup 2
 └── Changed Data Only

Backup 3
 └── Changed Data Only
```

---

## Restore

Recovery of data from snapshots.

```text
Snapshot
   │
   ▼
 Restore
   │
   ▼
 Files
```

---

# 3. Installation and Setup

---

## Linux Installation

### Ubuntu

```bash
sudo apt update
sudo apt install restic -y
```

Verify:

```bash
restic version
```

---

### RHEL/CentOS

```bash
sudo yum install restic -y
```

---

### Fedora

```bash
sudo dnf install restic -y
```

---

## macOS Installation

Using Homebrew:

```bash
brew install restic
```

Verify:

```bash
restic version
```

---

## Windows Installation

Using Chocolatey:

```powershell
choco install restic
```

Verify:

```powershell
restic version
```

---

## Manual Installation

Download:

```bash
wget https://github.com/restic/restic/releases/latest/download/restic_linux_amd64.bz2
```

Extract:

```bash
bunzip2 restic_linux_amd64.bz2
```

Make executable:

```bash
chmod +x restic_linux_amd64
```

Move:

```bash
sudo mv restic_linux_amd64 /usr/local/bin/restic
```

---

## Verify Installation

```bash
restic version
```

Example Output:

```text
restic 0.18.0 compiled with go1.24
```

---

# 4. Architecture

---

## High-Level Architecture

```text
Source Data
     │
     ▼
  Restic
     │
     ▼
 Encryption
     │
     ▼
 Deduplication
     │
     ▼
 Compression
     │
     ▼
 Repository
```

---

## Cloud Architecture

```text
Linux Server
      │
      ▼
    Restic
      │
      ▼
 AWS S3 Bucket
      │
      ▼
 Snapshot Storage
```

---

## Kubernetes Architecture

```text
Pod
 │
 ▼
Persistent Volume
 │
 ▼
Restic
 │
 ▼
Object Storage
 │
 ▼
Disaster Recovery
```

---

## Multi-Region Backup Architecture

```text
Production
      │
      ▼
    Restic
      │
      ▼
 Primary S3 Bucket
      │
      ▼
 Cross Region Replication
      │
      ▼
 Secondary Region
```

---

# 5. Repository Configuration

---

## Initialize Local Repository

Create repository:

```bash
restic init --repo /backup/repository
```

Output:

```text
created restic repository
```

---

## Initialize Repository with Password

```bash
export RESTIC_PASSWORD="MyStrongPassword"

restic init \
-r /backup/repository
```

---

## Using Password File

Create:

```bash
echo "StrongPassword123" > password.txt
```

Use:

```bash
restic \
-r /backup/repository \
--password-file password.txt \
snapshots
```

---

## Environment Variables

Repository:

```bash
export RESTIC_REPOSITORY=/backup/repository
```

Password:

```bash
export RESTIC_PASSWORD=StrongPassword123
```

Now commands become:

```bash
restic snapshots
```

instead of:

```bash
restic \
-r /backup/repository \
snapshots
```

---

## Repository Structure

```text
repository/
├── config
├── data
├── index
├── keys
├── locks
└── snapshots
```

---

### Config

Contains repository settings.

```text
config
```

---

### Data

Stores encrypted backup blocks.

```text
data/
```

---

### Snapshots

Stores backup metadata.

```text
snapshots/
```

---

### Keys

Encryption keys.

```text
keys/
```

---

## Check Repository

```bash
restic check
```

Verify integrity:

```bash
restic check --read-data
```

---

## Repository Statistics

```bash
restic stats
```

Example:

```text
Total Size: 50 GB
Snapshots: 25
Files: 1.2M
```

---

## List Snapshots

```bash
restic snapshots
```

Example:

```text
ID        Date
abc123    2026-01-01
def456    2026-01-02
ghi789    2026-01-03
```

---

## Repository Maintenance

### Rebuild Index

```bash
restic rebuild-index
```

---

### Unlock Repository

```bash
restic unlock
```

Useful after:

```text
Interrupted Backup
Server Crash
Network Failure
```

---

### Prune Repository

Remove unused data:

```bash
restic prune
```

Benefits:

* Save storage
* Remove orphaned blobs
* Improve efficiency

---

## Repository Migration

Copy repository:

```bash
rsync -av \
/backup/repository \
/new-location
```

Verify:

```bash
restic check
```

---

## Repository Best Practices

### Use Strong Passwords

Good:

```text
T!9xP#8qV$2L@7
```

Bad:

```text
password123
```

---

### Store Password Securely

Recommended:

```text
Vault
AWS Secrets Manager
Bitwarden
1Password
KeePass
```

---

### Verify Backups Regularly

```bash
restic check
```

Weekly verification:

```bash
restic check --read-data-subset=10%
```

---

### Keep Multiple Copies

```text
Production
     │
     ▼
 Primary Repository
     │
     ▼
 Secondary Repository
```

Follow:

```text
3-2-1 Backup Rule
```

* 3 Copies
* 2 Different Media Types
* 1 Offsite Copy

---

## Section Summary

Key Commands:

```bash
restic init
restic snapshots
restic check
restic stats
restic unlock
restic prune
```

These commands form the foundation of every Restic deployment.

---

# 6. Backup Operations

Backing up data is the primary function of Restic.

A backup operation scans files, encrypts data, performs deduplication, and stores the backup as a snapshot inside the repository.

---

## Basic Backup

Backup a directory:

```bash
restic backup /data
```

Example:

```bash
restic backup /home/ubuntu
```

---

## Backup Multiple Directories

```bash
restic backup \
/etc \
/var/www \
/home
```

---

## Backup Entire System

Linux:

```bash
sudo restic backup /
```

Typically combined with exclusions:

```bash
sudo restic backup / \
--exclude /proc \
--exclude /sys \
--exclude /dev \
--exclude /tmp
```

---

## Backup Specific Files

```bash
restic backup \
/etc/nginx/nginx.conf \
/etc/hosts
```

---

## Backup with Tags

Tags help organize backups.

Example:

```bash
restic backup \
/data \
--tag production
```

Multiple tags:

```bash
restic backup \
/data \
--tag production \
--tag database
```

View tagged snapshots:

```bash
restic snapshots --tag production
```

---

## Backup with Hostname

```bash
restic backup \
/data \
--host webserver01
```

Useful in multi-server environments.

---

## Excluding Files

Exclude log files:

```bash
restic backup /home \
--exclude "*.log"
```

---

## Excluding Directories

```bash
restic backup / \
--exclude /tmp \
--exclude /cache
```

---

## Exclude File

Create:

```text
exclude.txt
```

Example:

```text
*.log
tmp/
cache/
node_modules/
```

Use:

```bash
restic backup / \
--exclude-file exclude.txt
```

---

## Dry Run

Preview backup operation:

```bash
restic backup /data \
--dry-run
```

---

## Backup Progress

```bash
restic backup /data \
--verbose
```

---

## Backup Statistics

```bash
restic stats
```

Example:

```text
Total Size: 150 GB
Snapshots: 40
Files: 1.5M
```

---

## Verify Backup Success

List snapshots:

```bash
restic snapshots
```

Check latest backup:

```bash
restic snapshots latest
```

---

## Backup Workflow

```text
Files
  │
  ▼
Restic Scan
  │
  ▼
Deduplication
  │
  ▼
Encryption
  │
  ▼
Repository
  │
  ▼
Snapshot Created
```

---

# 7. Snapshot Management

Snapshots are point-in-time backup records.

Every backup creates a new snapshot.

---

## List Snapshots

```bash
restic snapshots
```

Example:

```text
ID        Date
abc123    2026-01-01
def456    2026-01-02
ghi789    2026-01-03
```

---

## View Latest Snapshot

```bash
restic snapshots latest
```

---

## Filter by Host

```bash
restic snapshots \
--host webserver01
```

---

## Filter by Tag

```bash
restic snapshots \
--tag production
```

---

## Snapshot Statistics

```bash
restic stats
```

---

## Forget Old Snapshots

Keep last 10 snapshots:

```bash
restic forget \
--keep-last 10
```

---

## Keep Daily Snapshots

```bash
restic forget \
--keep-daily 7
```

---

## Keep Weekly Snapshots

```bash
restic forget \
--keep-weekly 4
```

---

## Keep Monthly Snapshots

```bash
restic forget \
--keep-monthly 12
```

---

## Keep Yearly Snapshots

```bash
restic forget \
--keep-yearly 5
```

---

## Combined Retention Policy

```bash
restic forget \
--keep-daily 7 \
--keep-weekly 4 \
--keep-monthly 12 \
--prune
```

Production-ready policy.

---

## Delete Specific Snapshot

```bash
restic forget abc123
```

---

## Dry Run Snapshot Cleanup

```bash
restic forget \
--keep-last 10 \
--dry-run
```

---

## Snapshot Lifecycle

```text
Backup
   │
   ▼
Snapshot Created
   │
   ▼
Retention Policy
   │
   ▼
Forget
   │
   ▼
Prune
```

---

# 8. Restore Operations

Restoring data is the most critical backup operation.

Backups are useless if restores fail.

---

## Restore Latest Snapshot

```bash
restic restore latest \
--target restore
```

---

## Restore Specific Snapshot

```bash
restic restore abc123 \
--target restore
```

---

## Restore to Alternate Location

```bash
restic restore latest \
--target /tmp/recovery
```

---

## Restore Single File

First list files:

```bash
restic ls latest
```

Restore:

```bash
restic restore latest \
--include /etc/nginx/nginx.conf \
--target restore
```

---

## Restore Directory

```bash
restic restore latest \
--include /home/user \
--target restore
```

---

## List Files in Snapshot

```bash
restic ls latest
```

---

## Mount Repository

Browse snapshots:

```bash
mkdir /mnt/restic

restic mount /mnt/restic
```

Structure:

```text
/mnt/restic
 ├── snapshots
 ├── hosts
 └── tags
```

---

## Extract File Contents

```bash
restic dump latest \
/etc/hosts
```

Useful for quick inspection.

---

## Verify Restore

Compare:

```bash
diff original restored
```

Checksum:

```bash
sha256sum file
```

---

## Restore Workflow

```text
Repository
     │
     ▼
Choose Snapshot
     │
     ▼
Restore
     │
     ▼
Verify Files
     │
     ▼
Production Ready
```

---

# 9. Cloud Storage Backends

Restic supports many storage providers.

---

## Supported Backends

```text
AWS S3
MinIO
Azure Blob Storage
Google Cloud Storage
Backblaze B2
SFTP
Local Storage
```

---

## AWS S3

Set credentials:

```bash
export AWS_ACCESS_KEY_ID=ACCESS_KEY
export AWS_SECRET_ACCESS_KEY=SECRET_KEY
```

Initialize:

```bash
restic \
-r s3:s3.amazonaws.com/mybucket \
init
```

Backup:

```bash
restic \
-r s3:s3.amazonaws.com/mybucket \
backup /data
```

---

## MinIO

Set credentials:

```bash
export AWS_ACCESS_KEY_ID=minioadmin
export AWS_SECRET_ACCESS_KEY=minioadmin
```

Repository:

```bash
restic \
-r s3:http://minio:9000/backups \
init
```

Backup:

```bash
restic \
-r s3:http://minio:9000/backups \
backup /data
```

---

## Azure Blob Storage

Environment:

```bash
export AZURE_ACCOUNT_NAME=myaccount
export AZURE_ACCOUNT_KEY=mykey
```

Initialize:

```bash
restic \
-r azure:container:/repo \
init
```

---

## Google Cloud Storage

Authenticate:

```bash
gcloud auth application-default login
```

Initialize:

```bash
restic \
-r gs:mybucket:/repo \
init
```

---

## Backblaze B2

Credentials:

```bash
export B2_ACCOUNT_ID=id
export B2_ACCOUNT_KEY=key
```

Initialize:

```bash
restic \
-r b2:bucket:repo \
init
```

---

## SFTP Backend

Initialize:

```bash
restic \
-r sftp:user@host:/backup/repo \
init
```

Backup:

```bash
restic \
-r sftp:user@host:/backup/repo \
backup /data
```

---

## Multi-Cloud Architecture

```text
Restic
   │
   ├── AWS S3
   ├── Azure Blob
   ├── GCS
   ├── MinIO
   └── Backblaze B2
```

---

# 10. Security & Encryption

Security is one of Restic's strongest features.

---

## End-to-End Encryption

Every repository is encrypted.

```text
Files
  │
  ▼
Encrypt
  │
  ▼
Store
```

Storage providers never see plaintext data.

---

## Encryption Features

```text
AES-256 Encryption
Repository Password
Encrypted Metadata
Integrity Verification
```

---

## Repository Password

Set:

```bash
export RESTIC_PASSWORD="StrongPassword"
```

---

## Password File

Create:

```bash
echo "StrongPassword" > password.txt
```

Use:

```bash
restic \
--password-file password.txt \
snapshots
```

---

## Secure Password Storage

Recommended:

```text
HashiCorp Vault
AWS Secrets Manager
Azure Key Vault
GCP Secret Manager
Bitwarden
1Password
KeePass
```

---

## Least Privilege Principle

Use dedicated backup accounts.

Example:

```text
Backup User
   │
   ▼
Access Only Backup Bucket
```

Avoid:

```text
Administrator Access
```

---

## IAM Best Practices

AWS:

```text
Allow:
- List Bucket
- Get Object
- Put Object

Deny:
- Unrelated Services
```

---

## Verify Repository Integrity

```bash
restic check
```

Deep verification:

```bash
restic check --read-data
```

---

## Verify Partial Data

```bash
restic check \
--read-data-subset=10%
```

Useful for large repositories.

---

## Security Workflow

```text
Files
 │
 ▼
Encryption
 │
 ▼
Repository
 │
 ▼
Cloud Storage
 │
 ▼
Integrity Check
```

---

## Security Best Practices

### Strong Passwords

Good:

```text
N8!vK#2sP@9LmQ
```

Bad:

```text
password123
```

---

### Separate Backup Accounts

```text
Production
     │
     ▼
Backup Account
```

---

### Enable MFA

Protect:

```text
AWS Accounts
Azure Accounts
GCP Accounts
```

---

### Test Recovery

Regularly perform:

```bash
restic restore
```

---

### Monitor Repository Health

```bash
restic check
```

Weekly recommended.

---

Most Important Commands:

```bash
restic backup
restic snapshots
restic forget
restic restore
restic ls
restic dump
restic mount
restic check
restic prune
```

These commands represent the core operational workflow of Restic in production environments.

# 11. Kubernetes Backup & Recovery

Restic is widely used in Kubernetes environments for backing up:

* Persistent Volumes (PVs)
* Persistent Volume Claims (PVCs)
* Application Data
* StatefulSets
* Databases
* Disaster Recovery Workflows

---

## Kubernetes Backup Flow

```text
Application Pod
       │
       ▼
Persistent Volume
       │
       ▼
    Restic
       │
       ▼
Object Storage
(S3 / MinIO / Azure / GCS)
```

---

## Common Kubernetes Use Cases

### Database Backups

```text
MySQL PVC
PostgreSQL PVC
MongoDB PVC
Redis Persistence
```

---

### Application Data Backup

```text
WordPress Uploads
Jenkins Home
GitLab Data
Prometheus Storage
Grafana Storage
```

---

## Example Pod Backup

```bash
kubectl exec -it backup-pod -- \
restic backup /data
```

---

## Velero + Restic

A popular combination:

```text
Velero
   │
   ▼
 Restic
   │
   ▼
 Object Storage
```

Benefits:

* Cluster backup
* Namespace backup
* PVC backup
* Restore automation

---

# 12. Automation (Cron & Systemd)

Backups should be automated.

Never rely on manual backups.

---

## Cron Job Backup

Edit:

```bash
crontab -e
```

Daily backup:

```cron
0 2 * * * restic backup /data
```

Runs:

```text
2:00 AM Every Day
```

---

## Weekly Backup

```cron
0 3 * * 0 restic backup /data
```

---

## Backup Script

```bash
#!/bin/bash

export RESTIC_REPOSITORY=/backup/repository
export RESTIC_PASSWORD=StrongPassword

restic backup /data
```

Save:

```text
backup.sh
```

Run:

```bash
chmod +x backup.sh
./backup.sh
```

---

## Systemd Service

```ini
[Unit]
Description=Restic Backup

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup.sh
```

Save:

```text
/etc/systemd/system/restic-backup.service
```

---

## Systemd Timer

```ini
[Unit]
Description=Daily Restic Backup

[Timer]
OnCalendar=daily

[Install]
WantedBy=timers.target
```

Save:

```text
/etc/systemd/system/restic-backup.timer
```

Enable:

```bash
sudo systemctl enable \
restic-backup.timer
```

Start:

```bash
sudo systemctl start \
restic-backup.timer
```

---

# 13. Monitoring & Logging

Backups must be monitored.

A backup that fails silently is useless.

---

## Log Backup Output

```bash
restic backup /data \
>> backup.log 2>&1
```

---

## Monitor Snapshots

```bash
restic snapshots
```

Verify recent backups exist.

---

## Check Repository Health

```bash
restic check
```

---

## Prometheus Integration

Example Exporter Flow:

```text
Restic
   │
   ▼
 Script
   │
   ▼
 Prometheus
   │
   ▼
 Grafana
```

Metrics:

```text
Last Backup Time
Backup Status
Snapshot Count
Repository Size
```

---

## Grafana Dashboard Ideas

Monitor:

```text
Backup Duration
Failed Backups
Repository Growth
Storage Usage
Restore Success Rate
```

---

# 14. Advanced Features

---

## Dry Run

Preview actions:

```bash
restic forget \
--keep-last 10 \
--dry-run
```

---

## Include Files

```bash
restic backup \
/etc \
/home
```

---

## Exclude Files

```bash
restic backup /home \
--exclude "*.log"
```

---

## Exclude Directories

```bash
restic backup / \
--exclude /tmp
```

---

## Exclude File List

```bash
restic backup / \
--exclude-file excludes.txt
```

Example:

```text
*.log
tmp/
cache/
```

---

## Tags

Create backup:

```bash
restic backup \
/data \
--tag production
```

---

View:

```bash
restic snapshots \
--tag production
```

---

## Host-Based Snapshots

```bash
restic backup \
/data \
--host webserver01
```

---

## JSON Output

```bash
restic snapshots --json
```

Useful for:

```text
Automation
Monitoring
Dashboards
CI/CD
```

---

# 15. Troubleshooting

---

## Repository Locked

Error:

```text
repository is already locked
```

Fix:

```bash
restic unlock
```

---

## Wrong Password

Error:

```text
wrong password
```

Fix:

```text
Use Correct Password
Use Password File
Verify Environment Variables
```

---

## Cannot Access S3

Check:

```bash
aws s3 ls
```

Verify:

```text
IAM Permissions
Bucket Policy
Credentials
Network Access
```

---

## Repository Corruption

Verify:

```bash
restic check
```

Deep Validation:

```bash
restic check --read-data
```

---

## Backup Too Slow

Possible Causes:

```text
Large Dataset
Slow Disk
Slow Network
Millions of Small Files
```

---

# 16. Disaster Recovery Scenarios

---

## Scenario 1: Deleted File

Restore:

```bash
restic restore latest \
--target restore
```

Recover file.

---

## Scenario 2: Server Failure

```text
Old Server
   │
   ✖
Failure
   │
   ▼
New Server
   │
   ▼
Restore Snapshot
```

---

## Scenario 3: Ransomware

```text
Production Files
      │
Encrypted
      ▼
Restore Snapshot
      ▼
Recovery
```

Snapshots remain safe because:

```text
Encrypted
Immutable
Versioned
```

---

## Scenario 4: Kubernetes Cluster Loss

```text
Cluster Lost
      │
      ▼
Restore PVC Data
      │
      ▼
Redeploy Applications
```

---

## Recovery Workflow

```text
Failure
   │
   ▼
Locate Snapshot
   │
   ▼
Restore Data
   │
   ▼
Validate Services
   │
   ▼
Production Ready
```

---

# 17. Best Practices

---

## Follow 3-2-1 Rule

```text
3 Copies
2 Storage Types
1 Offsite Copy
```

---

## Encrypt Everything

Always use:

```text
Repository Password
```

---

## Verify Backups

Weekly:

```bash
restic check
```

---

## Test Restores

Many teams verify backups.

Few teams verify restores.

Always test:

```bash
restic restore
```

---

## Separate Backup Accounts

Use dedicated:

```text
AWS IAM User
Azure Service Principal
GCP Service Account
```

---

## Lifecycle Policies

For S3:

```text
Move Old Backups
Archive Snapshots
Reduce Storage Cost
```

---

## Monitor Failures

Alert On:

```text
Failed Backup
No Backup Created
Repository Corruption
Storage Threshold
```

---

# 18. Interview Questions

---

## What is Restic?

A modern backup tool providing:

```text
Encryption
Deduplication
Snapshots
Cloud Support
```

---

## What is a Snapshot?

A point-in-time backup state.

---

## What is Deduplication?

Only changed blocks are stored.

---

## How does Restic secure backups?

```text
AES-256 Encryption
Repository Password
Encrypted Metadata
```

---

## Difference Between Backup and Snapshot?

```text
Backup
└── Process

Snapshot
└── Result
```

---

## Why is Restic popular in Kubernetes?

```text
PVC Backup
Cloud Storage Support
Velero Integration
Disaster Recovery
```

---

## How do you verify backups?

```bash
restic check
```

---

## How do you restore a file?

```bash
restic restore
```

---

# 19. Additional Resources

### Official Resources

* https://restic.net
* https://github.com/restic/restic
* https://restic.readthedocs.io

---

### Cloud Documentation

* AWS S3 Documentation
* Azure Blob Documentation
* Google Cloud Storage Documentation
* MinIO Documentation

---

### Kubernetes Resources

* Velero Documentation
* Kubernetes Documentation

---

# 20. Quick Reference

## Repository

```bash
restic init
restic snapshots
restic stats
restic check
restic unlock
restic prune
```

---

## Backup

```bash
restic backup /data
restic backup /etc
restic backup /home
```

---

## Restore

```bash
restic restore latest \
--target restore
```

---

## Snapshots

```bash
restic snapshots
restic forget
restic prune
```

---

## S3

```bash
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=

restic -r s3:s3.amazonaws.com/bucket snapshots
```

---

## Maintenance

```bash
restic check
restic rebuild-index
restic unlock
restic prune
```

---

# 📎 Official Documentation

Official Website:

https://restic.net

Documentation:

https://restic.readthedocs.io

GitHub Repository:

https://github.com/restic/restic

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, DevOps practice, cloud-native backup workflows, and Kubernetes disaster recovery scenarios.

Primary references include:

* Restic Official Documentation
* Restic GitHub Repository
* Cloud Provider Documentation
* Kubernetes Documentation
* Velero Documentation

Restic is an open-source project maintained by its contributors.

All trademarks, service names, and project names belong to their respective owners.

For authoritative information, always refer to the official Restic documentation and project maintainers.

---

# 🎉 Conclusion

You now understand:

* Restic Architecture
* Repository Management
* Backup Operations
* Snapshot Management
* Restore Procedures
* Cloud Storage Integration
* Kubernetes Backup & Recovery
* Automation
* Monitoring
* Troubleshooting
* Disaster Recovery
* Security Best Practices

Restic is one of the most powerful backup and recovery tools in modern cloud-native environments because it combines:

```text
Security
+
Encryption
+
Deduplication
+
Cloud Storage
+
Kubernetes Support
+
Disaster Recovery
```

into a single lightweight solution.

---

**Last Updated:** 2026
**Tool Version:** Restic 0.18+
**License:** BSD 2-Clause License

For latest information, visit https://restic.net