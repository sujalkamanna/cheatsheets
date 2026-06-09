# 🦊 GitLab Cheatsheet

![GitLab Logo](https://about.gitlab.com/images/press/logo/png/gitlab-logo-500.png)

---

# Table of Contents

1. [What is GitLab?](#1-what-is-gitlab)
2. [Why GitLab?](#2-why-gitlab)
3. [GitLab vs GitHub](#3-gitlab-vs-github)
4. [GitLab vs Jenkins](#4-gitlab-vs-jenkins)
5. [Core Concepts](#5-core-concepts)
6. [GitLab Architecture](#6-gitlab-architecture)
7. [GitLab Editions](#7-gitlab-editions)
8. [GitLab SaaS vs Self-Managed](#8-gitlab-saas-vs-self-managed)
9. [Groups](#9-groups)
10. [Projects](#10-projects)
11. [Repositories](#11-repositories)
12. [Branches](#12-branches)
13. [Tags](#13-tags)
14. [Commits](#14-commits)
15. [Merge Requests](#15-merge-requests)
16. [User Roles](#16-user-roles)
17. [Permissions](#17-permissions)
18. [Protected Branches](#18-protected-branches)
19. [Protected Tags](#19-protected-tags)
20. [Installation](#20-installation)

---

# 1. What is GitLab?

GitLab is a complete DevOps platform that provides source code management, CI/CD pipelines, security scanning, container registry, package management, and Kubernetes integration in a single application.

Unlike many tools that require multiple integrations, GitLab aims to provide everything within one platform.

---

## GitLab Provides

```text id="j4m7xp"
Git Repository
+
CI/CD Pipelines
+
Container Registry
+
Security Scanning
+
Package Registry
+
Issue Tracking
+
Kubernetes Integration
```

---

## GitLab Workflow

```text id="r8v2wk"
Developer
     |
     v
GitLab Repository
     |
     v
CI/CD Pipeline
     |
     v
Build
     |
     v
Test
     |
     v
Deploy
```

---

## Common Use Cases

```text id="n5x9qb"
Source Code Management
+
DevOps Automation
+
DevSecOps
+
GitOps
+
Cloud Native Deployments
```

---

# 2. Why GitLab?

Traditional DevOps often requires multiple tools.

---

## Traditional Setup

```text id="v4m8pt"
GitHub
+
Jenkins
+
SonarQube
+
Artifactory
+
Security Tools
```

---

## GitLab Approach

```text id="c7w3nx"
Single Platform

For

Source Control
+
CI/CD
+
Security
+
Deployments
```

---

## Benefits

```text id="k2q8vb"
Single Application
+
Integrated DevOps
+
Built-in CI/CD
+
Built-in Security
+
Cloud Native Support
```

---

## Why Organizations Use GitLab

```text id="m8v5yr"
Simplified Toolchain
+
Lower Operational Overhead
+
Faster Delivery
+
Better Collaboration
```

---

# 3. GitLab vs GitHub

Both GitLab and GitHub are Git repository hosting platforms.

---

## Comparison

| Feature                | GitLab    | GitHub         |
| ---------------------- | --------- | -------------- |
| Source Control         | Yes       | Yes            |
| CI/CD                  | Built-In  | GitHub Actions |
| Container Registry     | Built-In  | Available      |
| DevSecOps              | Strong    | Moderate       |
| Self-Managed           | Excellent | Limited        |
| Kubernetes Integration | Strong    | Strong         |

---

## GitHub Focus

```text id="y7m3qx"
Source Control
+
Developer Collaboration
```

---

## GitLab Focus

```text id="r3v8wt"
Complete DevOps Platform
```

---

## When to Choose GitLab

```text id="q5n2xb"
Integrated DevOps
+
Self Hosting
+
DevSecOps
+
Enterprise Workflows
```

---

# 4. GitLab vs Jenkins

Many people compare GitLab CI/CD with Jenkins.

---

## Jenkins

```text id="u8p4kr"
CI/CD Tool
```

Primary focus:

```text id="v9m5qw"
Build
+
Test
+
Deploy
```

---

## GitLab

```text id="n7x3pb"
DevOps Platform
```

Provides:

```text id="t2v8mk"
Git Repositories
+
CI/CD
+
Security
+
Container Registry
+
Deployments
```

---

## Comparison

| Feature            | GitLab | Jenkins      |
| ------------------ | ------ | ------------ |
| Source Control     | Yes    | No           |
| CI/CD              | Yes    | Yes          |
| Security Scanning  | Yes    | Plugin Based |
| Container Registry | Yes    | No           |
| Setup Complexity   | Lower  | Higher       |
| Plugin Dependency  | Lower  | Higher       |

---

## Common Pattern

```text id="f8w2qc"
GitLab
      |
      v
GitLab CI/CD

Instead Of

GitHub
      |
      v
Jenkins
```

---

# 5. Core Concepts

| Concept       | Description                  |
| ------------- | ---------------------------- |
| Group         | Collection of projects       |
| Project       | Main working unit            |
| Repository    | Git repository               |
| Branch        | Independent development line |
| Commit        | Snapshot of changes          |
| Tag           | Version marker               |
| Merge Request | Request to merge code        |
| Runner        | Executes CI/CD jobs          |
| Pipeline      | Automated workflow           |
| Job           | Individual CI/CD task        |

---

## Relationship

```text id="k7p4xm"
Group
   |
   +--- Project
            |
            +--- Repository
                    |
                    +--- Branches
                    |
                    +--- Commits
                    |
                    +--- Tags
```

---

# 6. GitLab Architecture

GitLab follows a web-based architecture.

---

## High-Level Architecture

```text id="s4v9qt"
Developers
      |
      v
GitLab Server
      |
      +---- Git Repositories
      |
      +---- CI/CD Pipelines
      |
      +---- Security Scans
      |
      +---- Container Registry
```

---

## CI/CD Architecture

```text id="z5m8wr"
GitLab Server
      |
      v
GitLab Runner
      |
      v
Build
Test
Deploy
```

---

## Major Components

```text id="x3n7pt"
GitLab Web UI
+
Git Repository
+
PostgreSQL
+
Redis
+
GitLab Runner
+
Container Registry
```

---

## Workflow

```text id="j8v2qy"
Developer Pushes Code
          |
          v
Repository Updated
          |
          v
Pipeline Triggered
          |
          v
Runner Executes Jobs
```

---

# 7. GitLab Editions

GitLab is available in multiple editions.

---

## Community Edition (CE)

```text id="a6w4pk"
Free
+
Open Source
```

---

## Enterprise Edition (EE)

```text id="p8v5mr"
Advanced Features
+
Enterprise Security
+
Compliance
```

---

## GitLab Plans

```text id="r4m8xy"
Free
+
Premium
+
Ultimate
```

---

## Ultimate Includes

```text id="v2q9kt"
Advanced Security
+
Compliance
+
Portfolio Management
```

---

# 8. GitLab SaaS vs Self-Managed

GitLab can be used in two ways.

---

## GitLab SaaS

Hosted by GitLab.

```text id="w7m3qp"
GitLab Manages

Infrastructure
Updates
Backups
Maintenance
```

---

## GitLab Self-Managed

Hosted by your organization.

```text id="n9v2xt"
Full Control
+
Custom Security
+
Private Environment
```

---

## Comparison

| Feature        | SaaS      | Self-Managed |
| -------------- | --------- | ------------ |
| Maintenance    | GitLab    | Customer     |
| Infrastructure | GitLab    | Customer     |
| Control        | Limited   | Full         |
| Upgrades       | Automatic | Manual       |

---

## Common Choice

```text id="m5x8wr"
Startups
→ SaaS

Enterprises
→ Self Managed
```

---

# 9. Groups

Groups organize projects.

Think of them as folders for repositories.

---

## Example

```text id="j4p8vn"
Company
   |
   +--- Backend Team
   |
   +--- Frontend Team
   |
   +--- Platform Team
```

---

## Benefits

```text id="q8w3xt"
Organization
+
Access Control
+
Shared Resources
```

---

## Group Features

```text id="v5m9kr"
Projects
+
Members
+
CI/CD Variables
+
Shared Runners
```

---

# 10. Projects

Projects are the primary working unit in GitLab.

A project contains:

```text id="n2v7pb"
Repository
+
Pipelines
+
Issues
+
Merge Requests
+
Wiki
```

---

## Example

```text id="r6x3qt"
Project

E-Commerce Application
```

Contains:

```text id="f4m8wy"
Frontend Code
Backend Code
Documentation
CI/CD
```

---

## Project Workflow

```text id="t8v2qn"
Project
     |
     +--- Repository
     |
     +--- Pipelines
     |
     +--- Issues
```

---

# 11. Repositories

A repository stores project source code.

---

## Repository Contains

```text id="y7p5kr"
Source Code
+
Branches
+
Commits
+
Tags
```

---

## Clone Repository

```bash id="k4n8wx"
git clone https://gitlab.com/user/project.git
```

---

## Repository Workflow

```text id="m3v9qt"
Clone
  |
  v
Modify
  |
  v
Commit
  |
  v
Push
```

---

# 12. Branches

Branches allow parallel development.

---

## Common Branches

```text id="x8m2wp"
main
develop
feature/*
hotfix/*
release/*
```

---

## Example

```text id="r2v7kn"
main
  |
  +--- feature-login
  |
  +--- feature-payment
```

---

## Create Branch

```bash id="f8p4xt"
git checkout -b feature-authentication
```

---

# 13. Tags

Tags mark important versions.

Usually used for releases.

---

## Example

```text id="m9x2qw"
v1.0.0
v1.1.0
v2.0.0
```

---

## Create Tag

```bash id="q5v8nr"
git tag v1.0.0
git push origin v1.0.0
```

---

# 14. Commits

A commit is a snapshot of repository changes.

---

## Example

```bash id="t4w9pb"
git add .

git commit -m "Add login feature"
```

---

## Good Commit Message

```text id="y6m3xt"
Add payment validation

Fix authentication bug

Update deployment configuration
```

---

## Benefits

```text id="x2v8kr"
History Tracking
+
Rollback Capability
+
Collaboration
```

---

# 15. Merge Requests

Merge Requests (MRs) are one of GitLab's most important features.

Used to merge code from one branch into another.

---

## Workflow

```text id="n8p4qt"
Feature Branch
      |
      v
Merge Request
      |
      v
Code Review
      |
      v
Approval
      |
      v
Merge
```

---

## Benefits

```text id="v4m7wx"
Code Review
+
Collaboration
+
Quality Control
+
Security Checks
```

---

## Merge Request Features

```text id="p6v2kn"
Approvals
+
Comments
+
Pipeline Status
+
Security Reports
```

---

# 16. User Roles

GitLab provides role-based access control.

---

## Common Roles

| Role       | Access         |
| ---------- | -------------- |
| Guest      | Limited Access |
| Reporter   | Read Access    |
| Developer  | Push Code      |
| Maintainer | Manage Project |
| Owner      | Full Control   |

---

## Typical Usage

```text id="x9m5wr"
Developer
→ Developer Role

Team Lead
→ Maintainer Role

Admin
→ Owner Role
```

---

# 17. Permissions

Permissions determine user capabilities.

---

## Examples

```text id="j2v8qp"
View Repository
Create Branch
Create Merge Request
Run Pipelines
Manage Members
```

---

## Permission Model

```text id="m7x3wt"
Role
    |
    v
Permissions
```

---

# 18. Protected Branches

Protected branches prevent accidental changes.

---

## Common Protected Branch

```text id="q4v9kn"
main
```

---

## Restrictions

```text id="v8m2wr"
No Direct Push
+
No Direct Delete
+
Approval Required
```

---

## Benefits

```text id="x6p7qt"
Security
+
Code Quality
+
Release Stability
```

---

# 19. Protected Tags

Protected tags prevent unauthorized release creation.

---

## Example

```text id="k3v8mx"
v1.0.0
v2.0.0
v3.0.0
```

---

## Benefits

```text id="n5x4wr"
Release Protection
+
Version Integrity
+
Compliance
```

---

# 20. Installation

## Docker Installation

```bash id="q8m3vt"
docker run --detach \
  --hostname gitlab.example.com \
  --publish 443:443 \
  --publish 80:80 \
  --publish 22:22 \
  --name gitlab \
  --restart always \
  gitlab/gitlab-ee:latest
```

---

## Kubernetes Installation

```bash id="m4v7xp"
helm repo add gitlab https://charts.gitlab.io

helm repo update

helm install gitlab gitlab/gitlab
```

---

## Verify Access

```text id="r9w2kn"
http://localhost

or

https://gitlab.example.com
```

---

## Installation Options

```text id="t6v8qy"
GitLab SaaS
+
Docker
+
Kubernetes
+
Virtual Machine
```

---

# 21. GitLab CI/CD Overview

GitLab CI/CD is GitLab's built-in Continuous Integration and Continuous Delivery platform.

It automates:

```text id="n7x4wp"
Build
+
Test
+
Package
+
Deploy
+
Monitor
```

directly from your Git repository.

---

## What is CI?

Continuous Integration means automatically building and testing code whenever changes are pushed.

---

### Traditional Workflow

```text id="q4m8vt"
Developer
    |
    v
Write Code
    |
    v
Manual Build
    |
    v
Manual Testing
```

---

### CI Workflow

```text id="r8v3kn"
Developer Push
       |
       v
Pipeline Starts
       |
       v
Build
       |
       v
Test
       |
       v
Result
```

---

## What is CD?

Continuous Delivery / Deployment automates software releases.

---

### CD Workflow

```text id="x5m2qw"
Build
  |
  v
Test
  |
  v
Package
  |
  v
Deploy
```

---

## GitLab CI/CD Benefits

```text id="p7v9xt"
Automation
+
Faster Releases
+
Improved Quality
+
Reduced Human Error
+
Repeatable Deployments
```

---

## GitLab CI/CD Components

```text id="m4x8wr"
Pipeline
+
Stage
+
Job
+
Runner
+
Artifacts
+
Variables
```

---

## CI/CD Architecture

```text id="j8p4qn"
Developer
      |
      v
Git Push
      |
      v
GitLab
      |
      v
Pipeline
      |
      v
Runner
      |
      v
Build/Test/Deploy
```

---

## Example Pipeline

```text id="w2v7kr"
Build
   |
   v
Test
   |
   v
Security Scan
   |
   v
Deploy
```

---

# 22. CI/CD Architecture

GitLab CI/CD follows a controller-worker architecture.

---

## Architecture

```text id="n3m8xt"
GitLab Server
      |
      v
Pipeline Scheduler
      |
      v
GitLab Runner
      |
      v
Execution Environment
```

---

## Responsibilities

### GitLab Server

```text id="q9v4wp"
Stores Code
+
Creates Pipelines
+
Schedules Jobs
```

---

### GitLab Runner

```text id="k4x7mt"
Executes Jobs
+
Builds Applications
+
Runs Tests
+
Deploys Applications
```

---

## Workflow

```text id="r7p2xv"
Code Push
     |
     v
GitLab
     |
     v
Pipeline Created
     |
     v
Runner Assigned
     |
     v
Job Executed
```

---

# 23. GitLab Runners

GitLab Runner is the component that executes CI/CD jobs.

Without a Runner:

```text id="y5m8qt"
No Job Execution
```

---

## Runner Architecture

```text id="w8v4kr"
GitLab
   |
   v
Runner
   |
   v
Build/Test/Deploy
```

---

## Runner Responsibilities

```text id="p4x7wn"
Execute Jobs
+
Download Code
+
Run Scripts
+
Upload Artifacts
```

---

## Runner Workflow

```text id="q2m9vt"
Pipeline
    |
    v
Runner Picks Job
    |
    v
Execute Script
    |
    v
Return Status
```

---

## Why Runners Matter

```text id="r6v8xp"
Scalability
+
Automation
+
Distributed Execution
```

---

# 24. Runner Types

GitLab supports multiple runner scopes.

---

## Shared Runner

Available to multiple projects.

```text id="m8x3wr"
GitLab
    |
    +---- Project A
    |
    +---- Project B
    |
    +---- Project C
```

---

### Benefits

```text id="j4v9qt"
Easy Setup
+
Shared Resources
```

---

## Group Runner

Available to all projects inside a group.

```text id="r8m2kp"
Group
   |
   +--- Project A
   |
   +--- Project B
```

---

### Benefits

```text id="x6p7wv"
Team Standardization
+
Central Management
```

---

## Project Runner

Dedicated to one project.

```text id="p9v4xt"
Project
    |
    v
Dedicated Runner
```

---

### Benefits

```text id="t4m8qy"
Isolation
+
Security
+
Custom Configuration
```

---

# 25. Runner Executors

Executors determine where jobs run.

---

## Shell Executor

Runs directly on host OS.

```text id="v2x7kr"
GitLab Runner
      |
      v
Linux Shell
```

---

### Example

```yaml id="j8w4pn"
build:
  script:
    - npm install
    - npm run build
```

---

### Advantages

```text id="r5m9xt"
Fast
+
Simple
```

---

### Disadvantages

```text id="k3v8qw"
Less Isolation
```

---

## Docker Executor

Most commonly used.

---

### Architecture

```text id="p8m4xr"
Runner
   |
   v
Docker Container
```

---

### Example

```yaml id="m7x2vt"
image: node:20

build:
  script:
    - npm install
```

---

### Benefits

```text id="q5v8wp"
Isolation
+
Consistency
+
Reproducibility
```

---

## Kubernetes Executor

Runs jobs inside Kubernetes Pods.

---

### Architecture

```text id="w4m9qt"
Runner
   |
   v
Kubernetes Pod
```

---

### Benefits

```text id="r7x3vn"
Scalability
+
Cloud Native
+
Dynamic Resources
```

---

## Executor Comparison

| Executor   | Isolation | Scalability |
| ---------- | --------- | ----------- |
| Shell      | Low       | Medium      |
| Docker     | High      | High        |
| Kubernetes | High      | Very High   |

---

# 26. Registering Runners

A runner must be registered before use.

---

## Registration Process

```text id="n6v4xp"
Install Runner
      |
      v
Register Runner
      |
      v
Connect To GitLab
```

---

## Install Runner

Ubuntu:

```bash id="j4m8qw"
sudo apt install gitlab-runner
```

---

## Register Runner

```bash id="x8p2vt"
sudo gitlab-runner register
```

---

## Information Required

```text id="k9v5wr"
GitLab URL
+
Registration Token
+
Executor Type
+
Runner Name
```

---

## Verify Runner

```bash id="r2m7xt"
gitlab-runner verify
```

---

## Runner Status

```text id="p7v3wn"
Online
Offline
Paused
```

---

# 27. .gitlab-ci.yml

This is the most important GitLab CI/CD file.

It defines the entire pipeline.

---

## Location

```text id="t8x4vq"
Repository Root

.gitlab-ci.yml
```

---

## Basic Example

```yaml id="q3m8wp"
stages:
  - build

build:
  stage: build

  script:
    - echo "Building Application"
```

---

## Pipeline Lifecycle

```text id="r4v9xt"
Git Push
     |
     v
Read .gitlab-ci.yml
     |
     v
Create Pipeline
     |
     v
Execute Jobs
```

---

## Sections

```text id="m7p2xq"
Stages
+
Jobs
+
Variables
+
Artifacts
+
Rules
```

---

## Why It Matters

```text id="x4v8kn"
Pipeline As Code
+
Version Controlled
+
Repeatable
+
Automated
```

---

# 28. Stages

Stages divide pipelines into logical phases.

---

## Example

```yaml id="w7m4qt"
stages:
  - build
  - test
  - deploy
```

---

## Execution Flow

```text id="j5v9wr"
Build
   |
   v
Test
   |
   v
Deploy
```

---

## Benefits

```text id="n8x2kp"
Organization
+
Pipeline Visibility
+
Execution Control
```

---

## Common Stages

```text id="p4m7xt"
Build
+
Test
+
Security
+
Package
+
Deploy
```

---

# 29. Jobs

Jobs are individual tasks executed by runners.

---

## Example

```yaml id="k2v8wr"
build-app:
  stage: build

  script:
    - npm install
    - npm run build
```

---

## Job Structure

```text id="t7m4xq"
Job
 |
 +--- Stage
 |
 +--- Script
 |
 +--- Artifacts
```

---

## Multiple Jobs

```yaml id="r8v2wp"
build:
  stage: build

test:
  stage: test

deploy:
  stage: deploy
```

---

## Benefits

```text id="v3m9xt"
Modularity
+
Parallelism
+
Automation
```

---

# 30. Pipelines

A pipeline is a collection of stages and jobs.

---

## Example Pipeline

```text id="x8v4kn"
Pipeline

├── Build
├── Test
├── Security Scan
└── Deploy
```

---

## Pipeline Trigger Sources

```text id="m5x9wr"
Push
+
Merge Request
+
Tag
+
Schedule
+
API
```

---

## Pipeline View

```text id="r2v8qt"
Pipeline
    |
    +--- Stage
             |
             +--- Jobs
```

---

## Benefits

```text id="p8m4vx"
Automation
+
Visibility
+
Quality Control
+
Faster Delivery
```

---

# 31. Variables

Variables allow dynamic values to be used inside pipelines.

Instead of hardcoding values, variables make pipelines reusable and secure.

---

## Why Use Variables?

Instead of:

```yaml id="j4m8vt"
script:
  - docker login myregistry.com
```

Use:

```yaml id="x7v2wp"
script:
  - docker login $REGISTRY_URL
```

---

## Benefits

```text id="r5m9xq"
Reusability
+
Security
+
Maintainability
+
Environment Separation
```

---

## Variable Types

| Type                  | Purpose                              |
| --------------------- | ------------------------------------ |
| Predefined Variables  | Built-in GitLab variables            |
| Custom Variables      | User-created variables               |
| Protected Variables   | Available only on protected branches |
| Masked Variables      | Hidden in logs                       |
| Environment Variables | Environment-specific values          |

---

## Custom Variables

Project Settings:

```text id="v8m4kr"
Settings
    |
    v
CI/CD
    |
    v
Variables
```

---

## Example

```text id="n2x8qt"
DB_HOST=database.company.com

DB_PORT=5432

APP_ENV=production
```

---

## Using Variables

```yaml id="q7v4wp"
deploy:
  script:
    - echo $APP_ENV
```

---

## Predefined Variables

GitLab automatically provides variables.

---

### Examples

```text id="m4v8xt"
CI_PROJECT_NAME

CI_COMMIT_SHA

CI_COMMIT_BRANCH

CI_PIPELINE_ID

CI_JOB_ID
```

---

## Example Usage

```yaml id="p8x3wn"
build:
  script:
    - echo $CI_COMMIT_BRANCH
```

---

## Protected Variables

Used for production secrets.

```text id="k6m9qt"
Available Only

On Protected Branches
```

---

## Masked Variables

Sensitive values hidden from logs.

Example:

```text id="r4v7xp"
API_KEY

DATABASE_PASSWORD

AWS_SECRET_ACCESS_KEY
```

---

## Best Practices

* Never hardcode secrets
* Use protected variables for production
* Mask sensitive values
* Use environment-specific variables

---

# 32. Secrets

Secrets are sensitive values used during CI/CD execution.

---

## Common Secrets

```text id="v9x2wr"
Passwords
+
API Keys
+
Access Tokens
+
SSH Keys
+
Cloud Credentials
```

---

## Bad Practice

```yaml id="j3m8vq"
script:
  - docker login -u admin -p mypassword
```

---

## Good Practice

```yaml id="q8v4xt"
script:
  - docker login -u $DOCKER_USER -p $DOCKER_PASSWORD
```

---

## Secret Storage

```text id="w5m9kn"
GitLab Variables
+
HashiCorp Vault
+
AWS Secrets Manager
+
Azure Key Vault
```

---

## CI/CD Secret Flow

```text id="r7p3wx"
Secret Stored
      |
      v
Pipeline Starts
      |
      v
Runner Receives Secret
      |
      v
Job Executes
```

---

## Security Best Practices

* Rotate credentials regularly
* Avoid secrets in repositories
* Use masked variables
* Use external secret managers

---

# 33. Artifacts

Artifacts are files generated during a pipeline and shared between jobs.

---

## Examples

```text id="x4m8qt"
Build Output
+
Test Reports
+
Coverage Reports
+
Binaries
+
Packages
```

---

## Without Artifacts

```text id="m9v2xp"
Build Job
      |
      v
Output Lost
```

---

## With Artifacts

```text id="k5x7wr"
Build Job
      |
      v
Artifact
      |
      v
Test Job
```

---

## Example

```yaml id="q2m8vt"
build:
  stage: build

  script:
    - npm run build

  artifacts:
    paths:
      - dist/
```

---

## Download Artifacts

From:

```text id="r8v3wn"
Pipeline
    |
    v
Job
    |
    v
Artifacts
```

---

## Artifact Expiration

```yaml id="t7m4xq"
artifacts:
  expire_in: 7 days
```

---

## Benefits

```text id="n4v9kp"
Job Sharing
+
Reporting
+
Deployment Packaging
```

---

# 34. Cache

Cache speeds up pipelines by reusing files between runs.

---

## Common Cache Targets

```text id="p6m2wr"
node_modules

.maven

.gradle

pip cache

Docker layers
```

---

## Without Cache

```text id="r5v8xt"
Pipeline 1

Install Dependencies

------------------

Pipeline 2

Install Again
```

---

## With Cache

```text id="x2m9qp"
Pipeline 1

Install Dependencies

      |
      v

Cache

      |
      v

Pipeline 2

Reuse Dependencies
```

---

## Example

```yaml id="j8v4wr"
cache:
  paths:
    - node_modules/
```

---

## Benefits

```text id="m7x3qt"
Faster Builds
+
Reduced Network Usage
+
Lower Execution Time
```

---

## Cache vs Artifact

| Feature     | Cache        | Artifact       |
| ----------- | ------------ | -------------- |
| Purpose     | Speed        | Share Files    |
| Persistence | Temporary    | Pipeline Based |
| Use Case    | Dependencies | Build Outputs  |

---

# 35. Rules

Rules determine when jobs should run.

Modern GitLab pipelines use rules extensively.

---

## Why Rules?

```text id="v4m8xn"
Run Only When Needed
```

---

## Example

```yaml id="r9p2wt"
deploy:
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

---

## Result

```text id="x6m7qv"
main Branch
      |
      v
Deploy Job Runs

----------------

feature Branch
      |
      v
Deploy Job Skipped
```

---

## Common Conditions

```text id="m8v4xt"
Branch Name
+
Tag
+
Merge Request
+
Variable Value
```

---

## Multiple Rules

```yaml id="q5m9wr"
rules:
  - if: '$CI_COMMIT_BRANCH == "main"'

  - if: '$CI_COMMIT_TAG'
```

---

## Benefits

```text id="n3v7kp"
Flexibility
+
Reduced Pipeline Time
+
Better Control
```

---

# 36. Workflow Rules

Workflow rules determine whether an entire pipeline should be created.

---

## Difference

```text id="p7m4xt"
Rules
      |
      v
Job Level

----------------

Workflow Rules
      |
      v
Pipeline Level
```

---

## Example

```yaml id="r4x8wn"
workflow:
  rules:
    - if: '$CI_COMMIT_BRANCH'
```

---

## Use Cases

```text id="x8m3qt"
Skip Documentation Changes
+
Run Only Merge Requests
+
Run Only Tags
```

---

## Benefits

```text id="m6v9wp"
Reduced Resource Usage
+
Cleaner Pipelines
+
Better Efficiency
```

---

# 37. Manual Jobs

Manual jobs require user approval before execution.

---

## Example

```yaml id="k2x7wr"
deploy-prod:
  stage: deploy

  when: manual
```

---

## Workflow

```text id="p5m8qt"
Pipeline
     |
     v
Manual Approval
     |
     v
Deployment
```

---

## Common Uses

```text id="v9m4xp"
Production Deployment
+
Database Migration
+
Emergency Changes
```

---

## Benefits

```text id="r3x8wn"
Human Control
+
Reduced Risk
+
Compliance
```

---

# 38. Scheduled Pipelines

Scheduled pipelines run automatically at predefined times.

---

## Example Uses

```text id="m7v2qt"
Nightly Testing
+
Database Backup
+
Security Scans
+
Weekly Reports
```

---

## Schedule Workflow

```text id="q8m4wr"
Schedule
      |
      v
Pipeline
      |
      v
Execution
```

---

## Configuration

```text id="x4v9kp"
Project
     |
     v
CI/CD
     |
     v
Schedules
```

---

## Example Cron

```text id="r7m3xt"
0 2 * * *
```

Meaning:

```text id="n5v8wp"
Every Day

At 2:00 AM
```

---

## Benefits

```text id="t2m9xq"
Automation
+
Regular Maintenance
+
Consistent Execution
```

---

# 39. Parent Child Pipelines

Parent-child pipelines allow one pipeline to trigger another pipeline.

Useful for large projects and microservices.

---

## Traditional Pipeline

```text id="x7m4wp"
Single Pipeline

Build
Test
Security
Deploy
```

---

## Parent Child Architecture

```text id="r5v8qt"
Parent Pipeline
       |
       +---- Child Pipeline A
       |
       +---- Child Pipeline B
       |
       +---- Child Pipeline C
```

---

## Benefits

```text id="m3v9xn"
Modularity
+
Scalability
+
Parallel Execution
+
Better Organization
```

---

## Parent Pipeline Example

```yaml id="p8m4wr"
trigger-child:
  trigger:
    include: child-pipeline.yml
```

---

## Workflow

```text id="q7v2xp"
Parent Pipeline
       |
       v
Trigger Child Pipeline
       |
       v
Execute Jobs
```

---

## Common Use Cases

```text id="n5x8qt"
Microservices
+
Large Applications
+
Platform Engineering
```

---

# 40. Multi-Project Pipelines

Multi-project pipelines connect multiple repositories.

---

## Example

```text id="m8v4wr"
Frontend Repository
        |
        v
Backend Repository
        |
        v
Infrastructure Repository
```

---

## Architecture

```text id="x3m7qt"
Project A
      |
      v
Trigger
      |
      v
Project B
```

---

## Example

```yaml id="q6v2wp"
deploy:
  trigger:
    project: company/backend
```

---

## Benefits

```text id="r4m8xt"
Cross-Team Automation
+
Microservices Support
+
Centralized Delivery
```

---

## Real World Example

```text id="p9v3kn"
Application Repository
       |
       v
Build Container

       |
       v
Infrastructure Repository

       |
       v
Deploy Application
```

---

# 41. Review Apps

Review Apps create temporary environments for testing changes before merging.

---

## Problem

Without Review Apps:

```text id="m2v7xp"
Developer
      |
      v
Merge Code
      |
      v
Deploy
      |
      v
Test
```

---

## With Review Apps

```text id="r7m4qt"
Developer
      |
      v
Create Merge Request
      |
      v
Temporary Environment
      |
      v
Review & Test
      |
      v
Merge
```

---

## Benefits

```text id="x5v9wr"
Early Testing
+
Faster Reviews
+
Improved Quality
```

---

## Example

```yaml id="k8m2xp"
review:
  environment:
    name: review/$CI_COMMIT_REF_NAME
```

---

## Workflow

```text id="t4v8qn"
Feature Branch
      |
      v
Review App
      |
      v
Approval
      |
      v
Production
```

---

# 42. Environments

Environments represent deployment targets.

---

## Common Environments

```text id="v7m3xt"
Development
+
Testing
+
Staging
+
Production
```

---

## Deployment Flow

```text id="n8v4wp"
Development
      |
      v
Testing
      |
      v
Staging
      |
      v
Production
```

---

## Environment Example

```yaml id="p5m8qt"
deploy:
  environment:
    name: production
```

---

## Benefits

```text id="r2v9xn"
Deployment Tracking
+
Visibility
+
Rollback Support
```

---

## Environment Dashboard

```text id="m7x4wr"
GitLab
    |
    v
Operations
    |
    v
Environments
```

---

# 43. Deployment Pipelines

Deployment pipelines automate software releases.

---

## Pipeline Flow

```text id="x8m2qt"
Build
   |
   v
Test
   |
   v
Security Scan
   |
   v
Deploy
```

---

## Example

```yaml id="q4v8wp"
deploy:
  stage: deploy

  script:
    - kubectl apply -f deployment.yaml
```

---

## Deployment Targets

```text id="n6m9xt"
Virtual Machines
+
Docker Hosts
+
Kubernetes
+
Cloud Platforms
```

---

## Benefits

```text id="r5v3kn"
Consistency
+
Automation
+
Faster Delivery
```

---

## Production Deployment Pattern

```text id="m4x7qt"
Build Once
      |
      v
Deploy Everywhere
```

---

# 44. Pipeline Dependencies

Jobs often depend on outputs from previous jobs.

---

## Example

```text id="x7v4wp"
Build
   |
   v
Test
   |
   v
Deploy
```

---

## Dependency Example

```yaml id="k9m2xt"
test:
  dependencies:
    - build
```

---

## Benefits

```text id="q8v5wr"
Controlled Execution
+
Artifact Sharing
+
Reduced Errors
```

---

# 45. Parallel Jobs

Parallel jobs execute simultaneously.

---

## Sequential Execution

```text id="n5m8xp"
Build
   |
   v
Unit Test
   |
   v
Integration Test
```

---

## Parallel Execution

```text id="r8v2qt"
Build
   |
   +---- Unit Test
   |
   +---- Integration Test
   |
   +---- Security Test
```

---

## Example

```yaml id="m3v7wr"
unit-test:
  stage: test

integration-test:
  stage: test
```

---

## Benefits

```text id="x4m9xt"
Faster Pipelines
+
Better Resource Utilization
```

---

# 46. Pipeline Triggers

Triggers allow pipelines to start automatically.

---

## Trigger Sources

```text id="p8v4wn"
Push
+
Merge Request
+
Tag
+
Schedule
+
API
+
Webhook
```

---

## Trigger Flow

```text id="r6m2qt"
Event
   |
   v
Pipeline Created
   |
   v
Runner Execution
```

---

## API Trigger Example

```bash id="v7m9xp"
curl --request POST \
  --form token=TOKEN \
  --form ref=main
```

---

## Benefits

```text id="k4v8wr"
Automation
+
Integration
+
Continuous Delivery
```

---

# 47. CI/CD Best Practices

---

## Keep Pipelines Fast

```text id="x5m7qt"
Use Cache
+
Use Parallel Jobs
+
Optimize Builds
```

---

## Secure Pipelines

```text id="n9v3wp"
Use Secrets
+
Protect Branches
+
Protect Variables
```

---

## Make Pipelines Reusable

```text id="r4m8xt"
Templates
+
Variables
+
Shared Components
```

---

## Use Environment Separation

```text id="m2v7kn"
Dev
+
Test
+
Stage
+
Production
```

---

## Monitor Pipeline Health

Track:

```text id="p6m4wr"
Success Rate
+
Execution Time
+
Failure Rate
```

---

## Production Recommendations

```text id="x8v2qt"
Protected Branches
+
Manual Production Deployments
+
Security Scans
+
Approval Workflows
```

---

## CI/CD Goals

```text id="r7m5xp"
Reliable Builds
+
Automated Testing
+
Secure Deployments
+
Fast Delivery
```

---

# 48. Container Registry

GitLab provides a built-in Container Registry for storing Docker and OCI container images.

This eliminates the need for separate registries in many projects.

---

## Why Container Registry?

Without Registry:

```text id="m8v4xt"
Build Image
      |
      v
Store Somewhere Else
```

---

With GitLab Registry:

```text id="x5m7qp"
Build Image
      |
      v
GitLab Registry
      |
      v
Deploy
```

---

## Benefits

```text id="r4v9wn"
Built-In
+
Secure
+
Versioned Images
+
CI/CD Integration
```

---

## Architecture

```text id="n7m2xt"
GitLab
   |
   +---- Repository
   |
   +---- CI/CD
   |
   +---- Container Registry
```

---

## Container Workflow

```text id="q8v4kp"
Code
 |
 v
Build Image
 |
 v
Push To Registry
 |
 v
Deploy
```

---

## Login Example

```bash id="m3v8wr"
docker login registry.gitlab.com
```

---

## Push Image

```bash id="x7m4qt"
docker build -t registry.gitlab.com/company/app:v1 .

docker push registry.gitlab.com/company/app:v1
```

---

## Pull Image

```bash id="r5v2xp"
docker pull registry.gitlab.com/company/app:v1
```

---

## Common Use Cases

```text id="p9m7wn"
Docker Images
+
Kubernetes Deployments
+
Microservices
+
GitOps
```

---

# 49. Package Registry

GitLab provides a package registry for storing application packages.

---

## Supported Package Types

```text id="v8m3qt"
Maven
+
NPM
+
NuGet
+
PyPI
+
Composer
+
Generic Packages
```

---

## Benefits

```text id="k4v8xp"
Centralized Storage
+
Version Control
+
Security
+
CI/CD Integration
```

---

## Workflow

```text id="n2m9wr"
Build Package
      |
      v
Publish Package
      |
      v
Consume Package
```

---

## Example

```text id="x5v4qt"
company-library

Version 1.0.0

Version 1.1.0

Version 2.0.0
```

---

## Common Use Cases

```text id="r7m8wp"
Shared Libraries
+
Internal Packages
+
Dependency Management
```

---

# 50. SAST (Static Application Security Testing)

SAST scans source code for security vulnerabilities.

---

## Purpose

Find security issues before deployment.

---

## Workflow

```text id="m4v7qt"
Source Code
      |
      v
SAST Scan
      |
      v
Security Report
```

---

## Common Vulnerabilities

```text id="p8m2xt"
SQL Injection
+
Cross Site Scripting
+
Command Injection
+
Hardcoded Secrets
```

---

## Benefits

```text id="r3v8wn"
Shift Left Security
+
Early Detection
+
Reduced Risk
```

---

## GitLab Security Pipeline

```text id="x6m4qp"
Commit
   |
   v
Pipeline
   |
   v
SAST
   |
   v
Report
```

---

## Why Important?

```text id="k7v9xt"
Finding Security Issues

Before Production
```

---

# 51. DAST (Dynamic Application Security Testing)

DAST tests running applications.

Unlike SAST, it analyzes behavior instead of source code.

---

## Workflow

```text id="m8v4wr"
Running Application
       |
       v
DAST Scan
       |
       v
Security Report
```

---

## Common Findings

```text id="p5m8xt"
Authentication Issues
+
Misconfigurations
+
Exposed Endpoints
+
Security Weaknesses
```

---

## Benefits

```text id="r7v3qn"
Runtime Validation
+
Real World Testing
+
Production-Like Checks
```

---

## SAST vs DAST

| Feature         | SAST | DAST |
| --------------- | ---- | ---- |
| Source Code     | Yes  | No   |
| Running App     | No   | Yes  |
| Early Detection | Yes  | No   |
| Runtime Issues  | No   | Yes  |

---

# 52. Secret Detection

Detects secrets accidentally committed to repositories.

---

## Examples

```text id="x9m2wt"
AWS Keys
+
GitHub Tokens
+
Passwords
+
Private Keys
```

---

## Problem

```text id="m3v8qp"
Developer
      |
      v
Commits Secret
      |
      v
Repository
```

---

## Solution

```text id="r5m7xt"
Secret Detection Scan
      |
      v
Alert
```

---

## Benefits

```text id="n8v4wr"
Reduced Exposure
+
Improved Security
+
Compliance
```

---

# 53. Dependency Scanning

Scans third-party libraries for vulnerabilities.

---

## Why?

Applications often contain:

```text id="p6m9xt"
Open Source Packages
+
Frameworks
+
Libraries
```

---

## Example

```text id="x4m2wp"
Application
      |
      +---- Spring Boot

      +---- Log4j

      +---- Jackson
```

---

## Scan Process

```text id="r8m5qt"
Dependencies
      |
      v
Known Vulnerabilities
      |
      v
Report
```

---

## Benefits

```text id="m7v3xp"
Supply Chain Security
+
Risk Reduction
+
Compliance
```

---

# 54. Container Scanning

Scans container images for vulnerabilities.

---

## Workflow

```text id="q5v8wr"
Docker Image
      |
      v
Scan
      |
      v
Vulnerability Report
```

---

## Common Findings

```text id="n3m7xt"
Outdated Packages
+
Known CVEs
+
OS Vulnerabilities
```

---

## Example

```text id="r9v4wp"
Ubuntu Base Image

Contains

Critical Vulnerability
```

---

## Benefits

```text id="x6m2qt"
Secure Containers
+
Safer Deployments
+
Compliance
```

---

# 55. Security Dashboard

GitLab centralizes security findings into one dashboard.

---

## Sources

```text id="m8v5xn"
SAST
+
DAST
+
Secret Detection
+
Dependency Scanning
+
Container Scanning
```

---

## Security Workflow

```text id="p4m9wr"
Security Scan
      |
      v
Finding
      |
      v
Dashboard
      |
      v
Remediation
```

---

## Benefits

```text id="x7v2qt"
Visibility
+
Centralization
+
Risk Management
```

---

# 56. Kubernetes Integration

GitLab integrates directly with Kubernetes clusters.

---

## Benefits

```text id="r6m8xp"
Deployments
+
Environment Tracking
+
GitOps
+
Automation
```

---

## Architecture

```text id="m3v7qt"
GitLab
    |
    v
Kubernetes
    |
    v
Applications
```

---

## Common Platforms

```text id="q8v4wr"
EKS
+
AKS
+
GKE
+
On-Prem Kubernetes
```

---

## Workflow

```text id="x5m9qt"
Code
 |
 v
Pipeline
 |
 v
Kubernetes Deployment
```

---

# 57. GitLab Agent for Kubernetes

The GitLab Agent is the recommended way to connect GitLab and Kubernetes.

---

## Architecture

```text id="r7m3xp"
GitLab
    |
    v
GitLab Agent
    |
    v
Kubernetes Cluster
```

---

## Benefits

```text id="m4v8wr"
Secure Connectivity
+
GitOps Support
+
Cluster Visibility
```

---

## Agent Capabilities

```text id="x2m7qt"
Deploy Applications
+
Monitor Clusters
+
Manage Resources
```

---

## Why Agent?

```text id="p9v4xt"
More Secure

Than Direct Cluster Access
```

---

# 58. GitOps Overview

GitOps uses Git as the single source of truth.

---

## Traditional Deployment

```text id="r5v8wp"
Engineer
      |
      v
kubectl apply
```

---

## GitOps Deployment

```text id="m8v3qt"
Git Repository
      |
      v
Automation
      |
      v
Cluster
```

---

## Benefits

```text id="x7m4wr"
Version Control
+
Auditability
+
Automation
+
Consistency
```

---

## GitOps Principle

```text id="n6v9xt"
Git

Is

Source Of Truth
```

---

# 59. Auto DevOps

Auto DevOps is GitLab's automated CI/CD solution that automatically builds, tests, scans, and deploys applications with minimal configuration.

---

## Goal

Reduce the need for custom CI/CD pipeline creation.

---

## Traditional Approach

```text id="x8m4qt"
Create Pipeline
      |
      v
Configure Build
      |
      v
Configure Test
      |
      v
Configure Deploy
```

---

## Auto DevOps Approach

```text id="r5v8wp"
Push Code
      |
      v
GitLab Handles Everything
```

---

## Auto DevOps Workflow

```text id="m4v7xt"
Code
  |
  v
Build
  |
  v
Test
  |
  v
Security Scan
  |
  v
Package
  |
  v
Deploy
```

---

## Features

```text id="q9m2wr"
Auto Build
+
Auto Test
+
Auto Deploy
+
Security Scanning
+
Container Scanning
```

---

## Best For

```text id="x7v3qt"
Small Teams
+
Proof Of Concepts
+
Rapid Development
```

---

## Benefits

```text id="n5m8xp"
Fast Setup
+
Less Configuration
+
Built-In Best Practices
```

---

# 60. Helm Deployments

Helm is the package manager for Kubernetes.

GitLab pipelines commonly use Helm to deploy applications.

---

## Why Helm?

Without Helm:

```text id="r8v4wn"
Multiple YAML Files
```

---

With Helm:

```text id="m3v9qt"
Single Chart
      |
      v
Deploy Application
```

---

## Helm Architecture

```text id="x5m2wr"
GitLab Pipeline
       |
       v
Helm
       |
       v
Kubernetes
```

---

## Example Deployment

```yaml id="p7v8xt"
deploy:
  script:
    - helm upgrade --install app ./chart
```

---

## Benefits

```text id="q4m9wp"
Versioning
+
Reusable Templates
+
Simplified Deployments
```

---

## Common Workflow

```text id="n8v3qt"
Build Image
      |
      v
Push Image
      |
      v
Helm Deploy
```

---

# 61. EKS Integration

GitLab integrates with Amazon Elastic Kubernetes Service (EKS).

---

## Architecture

```text id="r6m7xp"
GitLab
    |
    v
GitLab Agent
    |
    v
Amazon EKS
```

---

## Benefits

```text id="m4v8wr"
Managed Kubernetes
+
Scalable Infrastructure
+
Cloud Native Deployments
```

---

## Deployment Workflow

```text id="x2m9qt"
Code Push
      |
      v
Pipeline
      |
      v
EKS Deployment
```

---

## Common AWS Services

```text id="p8v4xn"
EKS
+
ECR
+
IAM
+
CloudWatch
```

---

# 62. AKS Integration

GitLab integrates with Azure Kubernetes Service (AKS).

---

## Architecture

```text id="r5m8wp"
GitLab
    |
    v
GitLab Agent
    |
    v
AKS Cluster
```

---

## Benefits

```text id="m7v2qt"
Managed Kubernetes
+
Azure Integration
+
Automated Deployments
```

---

## Common Azure Services

```text id="q8m4wr"
AKS
+
Azure Container Registry
+
Azure Monitor
+
Azure Key Vault
```

---

## Workflow

```text id="x4v9xt"
GitLab Pipeline
       |
       v
AKS Deployment
```

---

# 63. GKE Integration

GitLab integrates with Google Kubernetes Engine.

---

## Architecture

```text id="n5v8xp"
GitLab
    |
    v
GitLab Agent
    |
    v
GKE Cluster
```

---

## Benefits

```text id="r7m3qt"
Managed Kubernetes
+
Google Cloud Integration
+
Automated Delivery
```

---

## Common GCP Services

```text id="m4x8wr"
GKE
+
Artifact Registry
+
Cloud Monitoring
+
Cloud Logging
```

---

## Workflow

```text id="x8m2qt"
GitLab Pipeline
       |
       v
GKE Deployment
```

---

# 64. GitLab DevSecOps Best Practices

---

## Secure Repositories

```text id="p5m9wr"
Protected Branches
+
Protected Tags
+
Mandatory Reviews
```

---

## Secure Pipelines

```text id="q7v4xt"
Protected Variables
+
Masked Variables
+
Least Privilege
```

---

## Security Scanning

Enable:

```text id="r9m2wp"
SAST
+
DAST
+
Secret Detection
+
Dependency Scanning
+
Container Scanning
```

---

## Deployment Security

```text id="x4v8qt"
Approval Gates
+
Manual Production Deployment
+
Environment Protection
```

---

## Container Security

```text id="n8v5wr"
Trusted Base Images
+
Image Scanning
+
Regular Updates
```

---

## Compliance Goals

```text id="m6v3xt"
Auditability
+
Traceability
+
Risk Reduction
```

---

# 65. GitLab CI/CD Interview Questions

## What is GitLab?

```text id="r5v8xn"
Complete DevOps Platform
Providing SCM,
CI/CD,
Security,
And Deployments
```

---

## What is GitLab Runner?

```text id="x2m7qt"
Component Responsible
For Executing CI/CD Jobs
```

---

## What is .gitlab-ci.yml?

```text id="p8v4wr"
Pipeline Definition File
Used To Configure CI/CD
```

---

## Difference Between Job and Stage?

```text id="m7v9xt"
Stage
=
Logical Phase

Job
=
Task Within Stage
```

---

## Difference Between Cache and Artifacts?

```text id="q4m8wp"
Cache
=
Speed Optimization

Artifacts
=
Share Files Between Jobs
```

---

## What is a Pipeline?

```text id="r6v2qt"
Collection Of
Stages And Jobs
```

---

## What is a Merge Request?

```text id="x8m4wr"
Code Review Mechanism
Before Merge
```

---

## What is GitOps?

```text id="n5v7xp"
Using Git
As The Single Source Of Truth
```

---

## What is Auto DevOps?

```text id="m3v8qt"
Automatic Build,
Test,
Scan,
And Deployment
Provided By GitLab
```

---

## What is GitLab Agent?

```text id="p9v4xt"
Secure Integration
Between GitLab
And Kubernetes
```

---

# 66. Monitoring

Monitoring helps track the health, performance, and reliability of GitLab and CI/CD pipelines.

---

## What Can Be Monitored?

```text id="m8v4wp"
GitLab Server
+
Pipelines
+
Runners
+
Repositories
+
Deployments
```

---

## Monitoring Architecture

```text id="x5m7qt"
GitLab
    |
    v
Prometheus
    |
    v
Grafana
```

---

## Key Metrics

```text id="r4v8xn"
CPU Usage
+
Memory Usage
+
Pipeline Duration
+
Job Success Rate
+
Runner Utilization
```

---

## Pipeline Metrics

```text id="n7m3wr"
Pipeline Success Rate
+
Pipeline Duration
+
Failure Rate
+
Deployment Frequency
```

---

## Runner Metrics

```text id="q8v2xt"
Active Jobs
+
Queue Size
+
CPU Usage
+
Memory Usage
```

---

## Benefits

```text id="m4v9qp"
Visibility
+
Performance Insights
+
Capacity Planning
```

---

# 67. Observability

Observability helps understand what is happening inside GitLab environments.

---

## Three Pillars

```text id="x6m8wr"
Metrics
+
Logs
+
Traces
```

---

## Metrics

Example:

```text id="r9v4xt"
Pipeline Count
+
Runner Usage
+
System Load
```

---

## Logs

Example:

```text id="n3m7qt"
Application Logs
+
Audit Logs
+
Runner Logs
```

---

## Traces

Example:

```text id="p5v8wr"
Request Tracking
+
API Performance
+
Service Dependencies
```

---

## Observability Stack

```text id="m7x4qt"
GitLab
   |
   +---- Prometheus
   |
   +---- Grafana
   |
   +---- Loki
   |
   +---- OpenTelemetry
```

---

## Benefits

```text id="q4v9wp"
Troubleshooting
+
Performance Analysis
+
Reliability
```

---

# 68. Backup & Restore

Backups are critical for disaster recovery.

---

## What Should Be Backed Up?

```text id="r8m3xt"
Repositories
+
Database
+
Artifacts
+
Container Registry
+
Configuration
```

---

## Backup Process

```text id="n5v7wr"
GitLab
    |
    v
Backup
    |
    v
Storage
```

---

## Create Backup

```bash id="x2m8qt"
sudo gitlab-backup create
```

---

## Backup Location

```text id="p9v4wr"
/var/opt/gitlab/backups
```

---

## Restore Backup

```bash id="m4v8xt"
sudo gitlab-backup restore
```

---

## Best Practices

```text id="q7m3wp"
Automated Backups
+
Offsite Storage
+
Regular Testing
```

---

# 69. Security Best Practices

Security should be applied across repositories, pipelines, runners, and deployments.

---

## Repository Security

```text id="r5v8qt"
Protected Branches
+
Protected Tags
+
Mandatory Reviews
```

---

## Pipeline Security

```text id="n8m2wr"
Protected Variables
+
Masked Variables
+
Approval Gates
```

---

## Runner Security

```text id="x4v7xt"
Dedicated Runners
+
Least Privilege
+
Network Isolation
```

---

## Access Control

```text id="p6m9wp"
Guest
+
Reporter
+
Developer
+
Maintainer
+
Owner
```

Use minimum required permissions.

---

## Secret Management

```text id="m3v8qt"
GitLab Variables
+
Vault
+
Cloud Secret Managers
```

---

## Security Checklist

```text id="q9v4wr"
Enable MFA
+
Protect Main Branch
+
Scan Containers
+
Scan Dependencies
+
Review Access
```

---

# 70. Performance Optimization

Large GitLab environments require optimization.

---

## Common Bottlenecks

```text id="r7m2xt"
Slow Pipelines
+
Busy Runners
+
Large Repositories
+
Excessive Artifacts
```

---

## Pipeline Optimization

### Use Cache

```yaml id="x5m8wr"
cache:
  paths:
    - node_modules/
```

---

### Use Parallel Jobs

```text id="n4v9qt"
Build
   |
   +---- Test A
   |
   +---- Test B
```

---

### Reduce Artifact Size

Store only required files.

---

## Runner Optimization

```text id="p8m4wp"
More Runners
+
Autoscaling Runners
+
Docker Executors
```

---

## Benefits

```text id="m7v3xt"
Faster Pipelines
+
Better Resource Usage
+
Lower Costs
```

---

# 71. Troubleshooting

A systematic approach is essential.

---

## Troubleshooting Flow

```text id="q4m8wr"
Repository
     |
     v
Pipeline
     |
     v
Runner
     |
     v
Deployment
```

---

## Pipeline Stuck

Possible Causes:

```text id="r8v2qt"
No Available Runner
+
Runner Offline
+
Configuration Error
```

---

## Check Runner Status

```bash id="x6m4wp"
gitlab-runner status
```

---

## Job Failure

Check:

```text id="n5v9xt"
Job Logs
+
Variables
+
Dependencies
```

---

## Pipeline Not Triggering

Verify:

```text id="p7m3wr"
Rules
+
Workflow Rules
+
Branch Conditions
```

---

## Kubernetes Deployment Failure

Check:

```text id="m4v8qt"
Cluster Access
+
Helm Configuration
+
Image Availability
```

---

## Common Problems

| Problem              | Cause                |
| -------------------- | -------------------- |
| Runner Offline       | Service Stopped      |
| Pipeline Failed      | Script Error         |
| Deployment Failed    | Cluster Issue        |
| Registry Push Failed | Authentication Issue |
| Slow Pipeline        | Missing Cache        |

---

# 72. Additional Resources

## Official Documentation

| Resource                  | URL                                                                                                                          |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| GitLab Documentation      | [https://docs.gitlab.com](https://docs.gitlab.com)                                                                           |
| GitLab CI/CD              | [https://docs.gitlab.com/ee/ci/](https://docs.gitlab.com/ee/ci/)                                                             |
| GitLab Runner             | [https://docs.gitlab.com/runner/](https://docs.gitlab.com/runner/)                                                           |
| GitLab Security           | [https://docs.gitlab.com/ee/user/application_security/](https://docs.gitlab.com/ee/user/application_security/)               |
| GitLab Kubernetes         | [https://docs.gitlab.com/ee/user/clusters/](https://docs.gitlab.com/ee/user/clusters/)                                       |
| GitLab Container Registry | [https://docs.gitlab.com/ee/user/packages/container_registry/](https://docs.gitlab.com/ee/user/packages/container_registry/) |

---

# 📎 Official Documentation

* [https://docs.gitlab.com](https://docs.gitlab.com)
* [https://docs.gitlab.com/ee/ci/](https://docs.gitlab.com/ee/ci/)
* [https://docs.gitlab.com/runner/](https://docs.gitlab.com/runner/)
* [https://docs.gitlab.com/ee/user/application_security/](https://docs.gitlab.com/ee/user/application_security/)
* [https://docs.gitlab.com/ee/user/clusters/](https://docs.gitlab.com/ee/user/clusters/)
* [https://docs.gitlab.com/ee/user/packages/container_registry/](https://docs.gitlab.com/ee/user/packages/container_registry/)

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, DevOps engineering, cloud engineering, platform engineering, DevSecOps, CI/CD automation, and GitOps workflows.

Primary references include:

* GitLab Documentation
* GitLab CI/CD Documentation
* GitLab Runner Documentation
* GitLab Security Documentation
* GitLab Kubernetes Documentation

GitLab is a DevOps platform provided by [GitLab Inc.](https://about.gitlab.com/?utm_source=chatgpt.com).

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

```text id="x8m4qt"
Source Code Management
+
CI/CD Automation
+
DevSecOps
+
Container Registry
+
Kubernetes Integration
+
GitOps Workflows
```

GitLab provides a complete DevOps platform that enables teams to manage source code, automate CI/CD pipelines, implement DevSecOps practices, and deploy cloud-native applications from a single integrated solution.

---

```md id="n7v3wr"
**Last Updated:** 2026
**Platform:** GitLab
**Version:** GitLab 18.x+
**License:** Community Edition (MIT) / Enterprise Edition (Commercial)

For latest information, visit https://docs.gitlab.com
```