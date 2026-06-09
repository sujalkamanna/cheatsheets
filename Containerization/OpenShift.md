# OpenShift Cheatsheet

![OpenShift Logo](https://www.redhat.com/cms/managed-files/openshift-logo-horizontal.svg)

---

# 📚 Table of Contents

| **Core OpenShift**                                       | **Advanced OpenShift**                                           |
| -------------------------------------------------------- | ---------------------------------------------------------------- |
| [1. OpenShift Fundamentals](#1-openshift-fundamentals)   | [18. Operators](#18-operators)                                   |
| [2. OpenShift Architecture](#2-openshift-architecture)   | [19. Operator Lifecycle Manager](#19-operator-lifecycle-manager) |
| [3. OpenShift vs Kubernetes](#3-openshift-vs-kubernetes) | [20. OperatorHub](#20-operatorhub)                               |
| [4. OpenShift Editions](#4-openshift-editions)           | [21. OpenShift Pipelines](#21-openshift-pipelines)               |
| [5. Cluster Components](#5-cluster-components)           | [22. Tekton](#22-tekton)                                         |
| [6. Control Plane](#6-control-plane)                     | [23. OpenShift GitOps](#23-openshift-gitops)                     |
| [7. Worker Nodes](#7-worker-nodes)                       | [24. Argo CD](#24-argo-cd)                                       |
| [8. Projects](#8-projects)                               | [25. Monitoring](#25-monitoring)                                 |
| [9. Users & Groups](#9-users--groups)                    | [26. Logging](#26-logging)                                       |
| [10. RBAC](#10-rbac)                                     | [27. Alerting](#27-alerting)                                     |
| [11. Pods](#11-pods)                                     | [28. OpenShift Service Mesh](#28-openshift-service-mesh)         |
| [12. Deployments](#12-deployments)                       | [29. OpenShift Virtualization](#29-openshift-virtualization)     |
| [13. DeploymentConfigs](#13-deploymentconfigs)           | [30. ACM](#30-acm)                                               |
| [14. BuildConfigs](#14-buildconfigs)                     | [31. Multi-Cluster Management](#31-multi-cluster-management)     |
| [15. ImageStreams](#15-imagestreams)                     | [32. Disaster Recovery](#32-disaster-recovery)                   |
| [16. Services](#16-services)                             | [33. Troubleshooting](#33-troubleshooting)                       |
| [17. Routes](#17-routes)                                 | [34. Interview Questions](#34-interview-questions)               |

---

# 1. OpenShift Fundamentals

## What is OpenShift?

OpenShift is an enterprise Kubernetes platform developed by Red Hat.

It extends Kubernetes by adding:

```text id="ocp1a1"
Developer Tools

Enterprise Security

CI/CD

Integrated Registry

Monitoring

Logging

Operators

Automation
```

---

## Simple Definition

```text id="ocp1a2"
OpenShift
     =
Kubernetes
     +
Enterprise Features
```

---

## Why OpenShift?

Organizations often find Kubernetes powerful but complex.

OpenShift simplifies:

```text id="ocp1a3"
Installation

Upgrades

Security

Networking

Application Deployment
```

---

## OpenShift Goals

```text id="ocp1a4"
Enterprise Container Platform

Developer Self-Service

Automation

Cloud-Native Applications

Hybrid Cloud
```

---

# 2. OpenShift Architecture

OpenShift follows Kubernetes architecture.

---

## High-Level Architecture

```text id="ocp1a5"
Users
  │
  ▼
OpenShift Console
  │
  ▼
API Server
  │
  ▼
Control Plane
  │
  ▼
Worker Nodes
  │
  ▼
Containers
```

---

## Major Components

```text id="ocp1a6"
Control Plane

Worker Nodes

etcd

Operators

Registry

Monitoring

Networking
```

---

# 3. OpenShift vs Kubernetes

Most common interview question.

---

## Comparison Table

| Feature             | Kubernetes | OpenShift |
| ------------------- | ---------- | --------- |
| Open Source         | Yes        | Yes       |
| Enterprise Support  | Optional   | Built-In  |
| Web Console         | Basic      | Advanced  |
| Integrated Registry | No         | Yes       |
| CI/CD               | External   | Built-In  |
| Security Defaults   | Moderate   | Strong    |
| Operators           | Optional   | Native    |
| Installation        | Manual     | Automated |

---

## Architecture View

```text id="ocp1a7"
Kubernetes
      │
      ▼
Foundation

OpenShift
      │
      ▼
Enterprise Kubernetes
```

---

## Key Difference

Kubernetes provides:

```text id="ocp1a8"
Container Orchestration
```

OpenShift provides:

```text id="ocp1a9"
Complete Enterprise Platform
```

---

# 4. OpenShift Editions

OpenShift is available in multiple forms.

---

## OpenShift Container Platform (OCP)

Enterprise version.

---

## OpenShift Dedicated

Managed by Red Hat.

---

## ROSA

ROSA = Red Hat OpenShift Service on AWS.

---

## ARO

ARO = Azure Red Hat OpenShift.

---

## OpenShift Local

Single-node development environment.

Previously called:

```text id="ocp1a10"
CodeReady Containers (CRC)
```

---

## Editions Overview

```text id="ocp1a11"
OpenShift Local

↓

Development

OpenShift Container Platform

↓

Enterprise

ROSA / ARO

↓

Managed Service
```

---

# 5. Cluster Components

An OpenShift cluster contains multiple components.

---

## Components

```text id="ocp1a12"
API Server

Scheduler

Controller Manager

etcd

Operators

Nodes

Registry

Monitoring Stack
```

---

## Architecture

```text id="ocp1a13"
Cluster
 │
 ├── Control Plane
 │
 ├── Worker Nodes
 │
 ├── Registry
 │
 └── Operators
```

---

# 6. Control Plane

The control plane manages the cluster.

---

## Components

```text id="ocp1a14"
API Server

Scheduler

Controller Manager

etcd
```

---

## Responsibilities

```text id="ocp1a15"
Scheduling

State Management

Cluster Control

Authentication
```

---

## Architecture

```text id="ocp1a16"
API Server
     │
     ▼
etcd
     │
     ▼
Controllers
```

---

# 7. Worker Nodes

Worker nodes run workloads.

---

## Components

```text id="ocp1a17"
kubelet

CRI-O

OpenShift SDN

Pods
```

---

## Responsibilities

```text id="ocp1a18"
Run Containers

Storage Mounts

Networking

Health Checks
```

---

## Architecture

```text id="ocp1a19"
Worker Node
      │
      ▼
CRI-O
      │
      ▼
Containers
```

---

# CRI-O in OpenShift

One major OpenShift difference:

---

## Kubernetes Runtime History

```text id="ocp1a20"
Docker

↓

containerd

↓

CRI-O
```

---

## OpenShift Runtime

```text id="ocp1a21"
CRI-O
```

is the default runtime.

---

## Benefits

```text id="ocp1a22"
Lightweight

Kubernetes Native

Secure

High Performance
```

---

# 8. OpenShift Projects

Projects are OpenShift's version of Namespaces.

---

## Kubernetes

```text id="ocp1a23"
Namespace
```

---

## OpenShift

```text id="ocp1a24"
Project
```

---

## Architecture

```text id="ocp1a25"
Cluster
 │
 ├── Project A
 │
 ├── Project B
 │
 └── Project C
```

---

## Benefits

```text id="ocp1a26"
Isolation

RBAC

Resource Management
```

---

# Project Commands

Create Project:

```bash id="ocp1a27"
oc new-project dev
```

---

View Projects:

```bash id="ocp1a28"
oc get projects
```

---

Switch Project:

```bash id="ocp1a29"
oc project dev
```

---

# 9. OpenShift CLI (oc)

OpenShift uses:

```text id="ocp1a30"
oc
```

instead of:

```text id="ocp1a31"
kubectl
```

although both work.

---

## Examples

```bash id="ocp1a32"
oc get pods

oc get projects

oc get routes

oc get nodes
```

---

## Why oc?

Provides OpenShift-specific features.

---

## Examples

```text id="ocp1a33"
Projects

Routes

BuildConfigs

ImageStreams

Operators
```

---

# 10. OpenShift Web Console

One of OpenShift's strongest features.

---

## Capabilities

```text id="ocp1a34"
Deploy Applications

View Logs

Manage Storage

Manage Networking

Manage Operators
```

---

## Architecture

```text id="ocp1a35"
Browser
   │
   ▼
OpenShift Console
   │
   ▼
Cluster
```

---

## Benefits

```text id="ocp1a36"
Easy Management

Developer Friendly

Enterprise Visibility
```

---

# OpenShift Architecture Summary

```text id="ocp1a37"
Users
  │
  ▼
OpenShift Console
  │
  ▼
API Server
  │
  ▼
Control Plane
  │
  ▼
Worker Nodes
  │
  ▼
CRI-O
  │
  ▼
Containers
```

---

# Important Commands

```bash id="ocp1a38"
oc login

oc whoami

oc get nodes

oc get pods

oc get projects

oc new-project dev

oc project dev

oc status

oc cluster-info

oc version
```

---

# 11. Users and Groups

OpenShift supports centralized identity and access management.

Users authenticate to the cluster and are assigned permissions through RBAC.

---

## Architecture

```text id="ocp2a1"
User
 │
 ▼
Authentication
 │
 ▼
Group
 │
 ▼
Role
 │
 ▼
Permissions
```

---

## User Types

```text id="ocp2a2"
Cluster Admin

Developer

Operator

Service Account

System User
```

---

## View Current User

```bash id="ocp2a3"
oc whoami
```

---

## View User Information

```bash id="ocp2a4"
oc describe user USERNAME
```

---

## View Groups

```bash id="ocp2a5"
oc get groups
```

---

# 12. Authentication

Authentication verifies identity.

---

## Authentication Sources

```text id="ocp2a6"
Local Users

LDAP

Active Directory

GitHub

GitLab

OpenID Connect

HTPasswd
```

---

## Authentication Flow

```text id="ocp2a7"
User
 │
 ▼
Identity Provider
 │
 ▼
OAuth Server
 │
 ▼
API Server
```

---

## Check Authentication

```bash id="ocp2a8"
oc whoami
```

---

## Login

```bash id="ocp2a9"
oc login https://api.cluster.example.com
```

---

# 13. Authorization

Authorization determines what actions users can perform.

---

## Architecture

```text id="ocp2a10"
Authentication
      │
      ▼
Authorization
      │
      ▼
Allow / Deny
```

---

## OpenShift Uses

```text id="ocp2a11"
RBAC
```

---

## Example

```text id="ocp2a12"
User Authenticated

↓

Can Create Pods?

↓

RBAC Checks
```

---

# 14. RBAC

RBAC = Role Based Access Control

---

## Components

```text id="ocp2a13"
Role

ClusterRole

RoleBinding

ClusterRoleBinding
```

---

## Architecture

```text id="ocp2a14"
User
 │
 ▼
RoleBinding
 │
 ▼
Role
 │
 ▼
Permissions
```

---

## Benefits

```text id="ocp2a15"
Least Privilege

Security

Compliance
```

---

# 15. Built-In Roles

OpenShift includes predefined roles.

---

## Common Roles

```text id="ocp2a16"
cluster-admin

admin

edit

view

self-provisioner
```

---

## cluster-admin

Full cluster access.

---

## admin

Project administration.

---

## edit

Can modify resources.

---

## view

Read-only access.

---

## Verify Roles

```bash id="ocp2a17"
oc get clusterroles
```

---

# 16. Assign Roles

Grant permissions to users.

---

## Grant Admin Role

```bash id="ocp2a18"
oc adm policy add-role-to-user \
admin john \
-n dev
```

---

## Grant View Role

```bash id="ocp2a19"
oc adm policy add-role-to-user \
view john \
-n dev
```

---

## Architecture

```text id="ocp2a20"
User
 │
 ▼
Role Assignment
 │
 ▼
Permissions
```

---

# 17. Service Accounts

Applications use Service Accounts.

---

## Architecture

```text id="ocp2a21"
Application
     │
     ▼
Service Account
     │
     ▼
API Server
```

---

## View Service Accounts

```bash id="ocp2a22"
oc get sa
```

---

## Create Service Account

```bash id="ocp2a23"
oc create sa app-sa
```

---

## Benefits

```text id="ocp2a24"
Application Identity

API Access

Automation
```

---

# 18. OpenShift Objects

Everything in OpenShift is an API object.

---

## Examples

```text id="ocp2a25"
Pod

Deployment

Route

Service

BuildConfig

ImageStream
```

---

## Architecture

```text id="ocp2a26"
API Server
     │
     ▼
Objects
     │
     ▼
Cluster State
```

---

# 19. OpenShift Templates

Templates simplify deployments.

---

## Purpose

```text id="ocp2a27"
Reusable Deployments

Parameterized Resources

Automation
```

---

## Architecture

```text id="ocp2a28"
Template
    │
    ▼
Parameters
    │
    ▼
Generated Resources
```

---

## Create From Template

```bash id="ocp2a29"
oc process -f template.yaml
```

---

# 20. OpenShift Template Example

---

## Basic Template

```yaml id="ocp2a30"
apiVersion: template.openshift.io/v1

kind: Template

metadata:
  name: nginx-template

parameters:

- name: APP_NAME

  required: true

objects:

- apiVersion: apps/v1

  kind: Deployment

  metadata:

    name: ${APP_NAME}
```

---

## Process Template

```bash id="ocp2a31"
oc process -f nginx-template.yaml \
-p APP_NAME=myapp
```

---

# 21. Labels

Labels organize resources.

---

## Example

```yaml id="ocp2a32"
labels:

  app: nginx

  env: dev

  tier: frontend
```

---

## Benefits

```text id="ocp2a33"
Filtering

Grouping

Selection
```

---

## View Labels

```bash id="ocp2a34"
oc get pods --show-labels
```

---

# 22. Selectors

Selectors locate resources.

---

## Example

```yaml id="ocp2a35"
selector:

  app: nginx
```

---

## Architecture

```text id="ocp2a36"
Service
   │
   ▼
Selector
   │
   ▼
Pods
```

---

# 23. Annotations

Store metadata.

---

## Example

```yaml id="ocp2a37"
annotations:

  owner: devops

  build: v1.0
```

---

## Difference

| Labels             | Annotations            |
| ------------------ | ---------------------- |
| Used For Selection | Not Used For Selection |
| Small Metadata     | Large Metadata         |

---

# 24. OpenShift Resource Management

Control resource usage.

---

## Components

```text id="ocp2a38"
Requests

Limits

Quotas

LimitRanges
```

---

## Benefits

```text id="ocp2a39"
Prevent Resource Abuse

Cluster Stability
```

---

# 25. Resource Requests

Minimum resources required.

---

## Example

```yaml id="ocp2a40"
resources:

  requests:

    cpu: "250m"

    memory: "256Mi"
```

---

## Scheduler Uses

```text id="ocp2a41"
Requests
```

to place Pods.

---

# 26. Resource Limits

Maximum resources allowed.

---

## Example

```yaml id="ocp2a42"
resources:

  limits:

    cpu: "1000m"

    memory: "1Gi"
```

---

## Benefits

```text id="ocp2a43"
Prevent Noisy Neighbors

Protect Cluster
```

---

# 27. Resource Quotas

Limit project-wide resource consumption.

---

## Example

```yaml id="ocp2a44"
apiVersion: v1

kind: ResourceQuota

metadata:

  name: quota
```

---

## Verify

```bash id="ocp2a45"
oc get quota
```

---

# 28. LimitRanges

Set default limits.

---

## Example

```yaml id="ocp2a46"
apiVersion: v1

kind: LimitRange
```

---

## Benefits

```text id="ocp2a47"
Enforce Standards

Avoid Missing Limits
```

---

# 29. OpenShift CLI Productivity Commands

---

## View Cluster

```bash id="ocp2a48"
oc status
```

---

## View All Resources

```bash id="ocp2a49"
oc get all
```

---

## Explain Resources

```bash id="ocp2a50"
oc explain deployment

oc explain route
```

---

## Get YAML

```bash id="ocp2a51"
oc get deployment nginx -o yaml
```

---

## Export Resource

```bash id="ocp2a52"
oc get svc myservice -o yaml
```

---

# 30. OpenShift Administration Basics

---

## Cluster Administrator Responsibilities

```text id="ocp2a53"
User Management

RBAC

Security

Operators

Storage

Networking

Monitoring

Upgrades
```

---

## Administrative Commands

```bash id="ocp2a54"
oc get nodes

oc get projects

oc get clusterroles

oc get users

oc get groups

oc adm top nodes

oc adm top pods
```

---

# OpenShift Access Management Architecture

```text id="ocp2a55"
User
 │
 ▼
Identity Provider
 │
 ▼
OAuth
 │
 ▼
RBAC
 │
 ▼
Project Access
 │
 ▼
Resources
```

---

# Important Commands

```bash id="ocp2a56"
oc login

oc logout

oc whoami

oc get users

oc get groups

oc get projects

oc project PROJECT

oc get roles

oc get rolebindings

oc get clusterroles

oc get sa

oc create sa app-sa

oc get quota

oc get limitrange

oc status

oc get all
```

---

# 31. Pods in OpenShift

Pods are the smallest deployable unit in OpenShift.

A Pod contains one or more containers.

---

## Architecture

```text id="ocp3a1"
Pod
 │
 ├── Container 1
 │
 ├── Container 2
 │
 └── Storage
```

---

## Characteristics

```text id="ocp3a2"
Shared Network

Shared Storage

Shared Lifecycle
```

---

## View Pods

```bash id="ocp3a3"
oc get pods
```

---

## Detailed View

```bash id="ocp3a4"
oc describe pod POD_NAME
```

---

# 32. Pod YAML Example

---

## Basic Pod

```yaml id="ocp3a5"
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

```bash id="ocp3a6"
oc apply -f pod.yaml
```

---

## Verify

```bash id="ocp3a7"
oc get pods
```

---

# 33. Pod Lifecycle

---

## States

```text id="ocp3a8"
Pending

Running

Succeeded

Failed

Unknown
```

---

## Lifecycle

```text id="ocp3a9"
Create
  │
  ▼
Pending
  │
  ▼
Running
  │
  ▼
Succeeded / Failed
```

---

## Check Status

```bash id="ocp3a10"
oc get pods
```

---

# 34. Multi-Container Pods

A Pod can run multiple containers.

---

## Example

```text id="ocp3a11"
Application Container

+

Logging Sidecar
```

---

## Architecture

```text id="ocp3a12"
Pod
 │
 ├── App
 │
 └── Sidecar
```

---

## Benefits

```text id="ocp3a13"
Shared Data

Shared Networking

Auxiliary Services
```

---

# 35. Init Containers

Init containers run before application containers.

---

## Workflow

```text id="ocp3a14"
Init Container

↓

Complete

↓

Application Starts
```

---

## Use Cases

```text id="ocp3a15"
Database Checks

Configuration Generation

Waiting For Services
```

---

## Example

```yaml id="ocp3a16"
initContainers:

- name: init-db

  image: busybox

  command:

  - sh

  - -c

  - echo Initializing
```

---

# 36. OpenShift Deployments

Deployments manage Pods.

---

## Responsibilities

```text id="ocp3a17"
Create Pods

Update Pods

Scale Pods

Recover Pods
```

---

## Architecture

```text id="ocp3a18"
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

```text id="ocp3a19"
Self Healing

Scaling

Rolling Updates
```

---

# 37. Deployment YAML

---

## Example

```yaml id="ocp3a20"
apiVersion: apps/v1

kind: Deployment

metadata:

  name: nginx

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

```bash id="ocp3a21"
oc apply -f deployment.yaml
```

---

## Verify

```bash id="ocp3a22"
oc get deployments
```

---

# 38. Scaling Deployments

Increase or decrease Pod count.

---

## Scale Up

```bash id="ocp3a23"
oc scale deployment nginx \
--replicas=5
```

---

## Scale Down

```bash id="ocp3a24"
oc scale deployment nginx \
--replicas=2
```

---

## Architecture

```text id="ocp3a25"
Deployment
     │
     ▼
Desired Replicas
```

---

# 39. Rolling Updates

Default deployment strategy.

---

## Workflow

```text id="ocp3a26"
Old Pods

↓

New Pods Created

↓

Old Pods Removed
```

---

## Benefits

```text id="ocp3a27"
Zero Downtime

Safe Upgrades
```

---

## Verify

```bash id="ocp3a28"
oc rollout status deployment nginx
```

---

# 40. Rollbacks

Return to previous version.

---

## Architecture

```text id="ocp3a29"
Version 2

↓

Rollback

↓

Version 1
```

---

## Command

```bash id="ocp3a30"
oc rollout undo deployment nginx
```

---

## Benefits

```text id="ocp3a31"
Quick Recovery

Reduced Risk
```

---

# 41. ReplicaSets

ReplicaSets maintain Pod count.

---

## Example

Desired:

```text id="ocp3a32"
3 Pods
```

Actual:

```text id="ocp3a33"
2 Pods
```

ReplicaSet creates:

```text id="ocp3a34"
1 New Pod
```

---

## Architecture

```text id="ocp3a35"
ReplicaSet
      │
      ▼
Pods
```

---

## View ReplicaSets

```bash id="ocp3a36"
oc get rs
```

---

# 42. DeploymentConfigs (OpenShift Specific)

OpenShift introduced DeploymentConfigs before Kubernetes Deployments matured.

---

## Architecture

```text id="ocp3a37"
DeploymentConfig
        │
        ▼
ReplicationController
        │
        ▼
Pods
```

---

## Kubernetes Deployment

```text id="ocp3a38"
Deployment
     │
     ▼
ReplicaSet
```

---

## OpenShift DeploymentConfig

```text id="ocp3a39"
DeploymentConfig
      │
      ▼
ReplicationController
```

---

# 43. Deployment vs DeploymentConfig

Important interview question.

---

## Comparison

| Deployment        | DeploymentConfig            |
| ----------------- | --------------------------- |
| Kubernetes Native | OpenShift Specific          |
| Uses ReplicaSets  | Uses ReplicationControllers |
| Recommended       | Legacy                      |
| Faster            | Slower                      |
| Modern            | Older                       |

---

## Recommendation

```text id="ocp3a40"
Use Deployments
```

for new workloads.

---

# 44. DeploymentConfig YAML

---

## Example

```yaml id="ocp3a41"
apiVersion: apps.openshift.io/v1

kind: DeploymentConfig

metadata:

  name: myapp

spec:

  replicas: 2

  selector:

    app: myapp
```

---

## Create

```bash id="ocp3a42"
oc apply -f dc.yaml
```

---

## Verify

```bash id="ocp3a43"
oc get dc
```

---

# 45. ReplicationControllers

Older OpenShift workload controller.

---

## Architecture

```text id="ocp3a44"
ReplicationController
         │
         ▼
Pods
```

---

## Responsibilities

```text id="ocp3a45"
Maintain Replicas

Recover Failed Pods
```

---

## View

```bash id="ocp3a46"
oc get rc
```

---

# 46. Rollout Management

OpenShift provides advanced rollout commands.

---

## Status

```bash id="ocp3a47"
oc rollout status dc/myapp
```

---

## Latest Rollout

```bash id="ocp3a48"
oc rollout latest dc/myapp
```

---

## History

```bash id="ocp3a49"
oc rollout history dc/myapp
```

---

# 47. OpenShift Application Creation

Quick application deployment.

---

## Create App

```bash id="ocp3a50"
oc new-app nginx
```

---

## From Git Repository

```bash id="ocp3a51"
oc new-app \
https://github.com/user/app.git
```

---

## Workflow

```text id="ocp3a52"
Source Code
     │
     ▼
Build
     │
     ▼
Image
     │
     ▼
Deployment
```

---

# 48. Labels and Deployments

Deployments use labels heavily.

---

## Example

```yaml id="ocp3a53"
labels:

  app: nginx

  env: dev
```

---

## Selector

```yaml id="ocp3a54"
selector:

  matchLabels:

    app: nginx
```

---

## Architecture

```text id="ocp3a55"
Deployment
      │
      ▼
Labels
      │
      ▼
Pods
```

---

# 49. Pod Health Monitoring

---

## Status Check

```bash id="ocp3a56"
oc get pods
```

---

## Describe

```bash id="ocp3a57"
oc describe pod POD
```

---

## Logs

```bash id="ocp3a58"
oc logs POD
```

---

## Previous Logs

```bash id="ocp3a59"
oc logs --previous POD
```

---

# 50. Workload Architecture Summary

```text id="ocp3a60"
Deployment
     │
     ▼
ReplicaSet
     │
     ▼
Pods
     │
     ▼
Containers
```

---

# Important Commands

```bash id="ocp3a61"
oc get pods

oc describe pod POD

oc logs POD

oc logs --previous POD

oc exec -it POD -- sh

oc get deployments

oc scale deployment nginx --replicas=5

oc rollout status deployment nginx

oc rollout undo deployment nginx

oc get rs

oc get dc

oc get rc

oc new-app nginx
```

---

# 51. Services in OpenShift

Applications running inside Pods need a stable endpoint.

Pods are ephemeral and can be recreated at any time.

Services provide:

```text id="ocp4a1"
Stable IP

Stable DNS

Load Balancing

Service Discovery
```

---

## Architecture

```text id="ocp4a2"
Client
  │
  ▼
Service
  │
  ▼
Pods
```

---

## Service Benefits

```text id="ocp4a3"
Stable Networking

Load Distribution

Pod Independence
```

---

# 52. Service Types

OpenShift supports Kubernetes Service types.

---

## Types

```text id="ocp4a4"
ClusterIP

NodePort

LoadBalancer

ExternalName
```

---

## Architecture

```text id="ocp4a5"
Service
 │
 ├── ClusterIP
 │
 ├── NodePort
 │
 ├── LoadBalancer
 │
 └── ExternalName
```

---

# 53. ClusterIP Service

Default Service type.

Internal cluster communication only.

---

## Architecture

```text id="ocp4a6"
Pod A
  │
  ▼
ClusterIP
  │
  ▼
Pod B
```

---

## Example YAML

```yaml id="ocp4a7"
apiVersion: v1

kind: Service

metadata:

  name: nginx-service

spec:

  selector:

    app: nginx

  ports:

  - port: 80

    targetPort: 80
```

---

## Create

```bash id="ocp4a8"
oc apply -f service.yaml
```

---

## Verify

```bash id="ocp4a9"
oc get svc
```

---

# 54. NodePort Service

Exposes service through node ports.

---

## Architecture

```text id="ocp4a10"
External User
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

## Example YAML

```yaml id="ocp4a11"
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

## Verify

```bash id="ocp4a12"
oc get svc
```

---

# 55. LoadBalancer Service

Used primarily in cloud environments.

---

## Architecture

```text id="ocp4a13"
Cloud Load Balancer
         │
         ▼
Service
         │
         ▼
Pods
```

---

## Example YAML

```yaml id="ocp4a14"
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

```bash id="ocp4a15"
oc get svc
```

---

# 56. ExternalName Service

Maps service to external DNS.

---

## Example

```yaml id="ocp4a16"
apiVersion: v1

kind: Service

metadata:

  name: external-db

spec:

  type: ExternalName

  externalName: db.company.com
```

---

## Use Cases

```text id="ocp4a17"
External Databases

External APIs

Legacy Systems
```

---

# 57. Service Discovery

OpenShift automatically provides DNS.

---

## Architecture

```text id="ocp4a18"
Pod
 │
 ▼
DNS
 │
 ▼
Service
```

---

## Example

```text id="ocp4a19"
nginx-service
```

accessible via:

```text id="ocp4a20"
nginx-service.project.svc.cluster.local
```

---

## Benefits

```text id="ocp4a21"
No Hardcoded IPs

Automatic Discovery
```

---

# 58. Endpoints

Endpoints represent backend Pods.

---

## Architecture

```text id="ocp4a22"
Service
   │
   ▼
Endpoints
   │
   ▼
Pods
```

---

## View Endpoints

```bash id="ocp4a23"
oc get endpoints
```

---

## Describe

```bash id="ocp4a24"
oc describe endpoints
```

---

# 59. OpenShift Routes

One of OpenShift's most important features.

Routes expose applications externally.

---

## Kubernetes Equivalent

```text id="ocp4a25"
Ingress
```

---

## OpenShift Equivalent

```text id="ocp4a26"
Route
```

---

## Architecture

```text id="ocp4a27"
Internet
    │
    ▼
Route
    │
    ▼
Service
    │
    ▼
Pods
```

---

# 60. Why Routes?

OpenShift simplifies external access.

---

## Without Route

```text id="ocp4a28"
User

↓

LoadBalancer

↓

Ingress

↓

Service
```

---

## With Route

```text id="ocp4a29"
User

↓

Route

↓

Service
```

---

## Benefits

```text id="ocp4a30"
Simple

Fast

Integrated
```

---

# 61. Create Route

Automatically expose service.

---

## Command

```bash id="ocp4a31"
oc expose svc nginx-service
```

---

## Verify

```bash id="ocp4a32"
oc get routes
```

---

## Describe

```bash id="ocp4a33"
oc describe route
```

---

# 62. Route YAML Example

---

## Basic Route

```yaml id="ocp4a34"
apiVersion: route.openshift.io/v1

kind: Route

metadata:

  name: nginx-route

spec:

  to:

    kind: Service

    name: nginx-service
```

---

## Apply

```bash id="ocp4a35"
oc apply -f route.yaml
```

---

# 63. Route Hostnames

Each route receives a hostname.

---

## Example

```text id="ocp4a36"
nginx.apps.cluster.example.com
```

---

## Verify

```bash id="ocp4a37"
oc get routes
```

---

# 64. Secure Routes (TLS)

OpenShift supports HTTPS natively.

---

## Architecture

```text id="ocp4a38"
User
 │
 ▼
HTTPS
 │
 ▼
Route
 │
 ▼
Service
```

---

## Benefits

```text id="ocp4a39"
Encryption

Security

Compliance
```

---

# 65. TLS Route Example

---

## YAML

```yaml id="ocp4a40"
spec:

  tls:

    termination: edge
```

---

## Edge Termination

TLS ends at Router.

---

# 66. Route TLS Types

Interview topic.

---

## Types

```text id="ocp4a41"
Edge

Passthrough

Re-encrypt
```

---

## Architecture

```text id="ocp4a42"
TLS
 │
 ├── Edge
 │
 ├── Passthrough
 │
 └── Re-encrypt
```

---

# 67. Edge Route

Most commonly used.

---

## Flow

```text id="ocp4a43"
Client HTTPS

↓

Router

↓

HTTP

↓

Application
```

---

## Benefits

```text id="ocp4a44"
Simple

Fast

Popular
```

---

# 68. Passthrough Route

TLS terminates inside application.

---

## Flow

```text id="ocp4a45"
Client HTTPS

↓

Router

↓

Application HTTPS
```

---

## Use Cases

```text id="ocp4a46"
End-to-End Encryption
```

---

# 69. Re-encrypt Route

Most secure route type.

---

## Flow

```text id="ocp4a47"
Client HTTPS

↓

Router HTTPS

↓

Application HTTPS
```

---

## Benefits

```text id="ocp4a48"
Maximum Security
```

---

# 70. OpenShift Router

Router handles Route traffic.

---

## Architecture

```text id="ocp4a49"
Route
 │
 ▼
Router
 │
 ▼
Service
 │
 ▼
Pods
```

---

## Responsibilities

```text id="ocp4a50"
Load Balancing

TLS

External Access
```

---

# 71. OpenShift Ingress Controller

Modern OpenShift uses Ingress Controllers.

---

## Architecture

```text id="ocp4a51"
Ingress Controller
        │
        ▼
Router Pods
        │
        ▼
Applications
```

---

## Verify

```bash id="ocp4a52"
oc get ingresscontroller \
-n openshift-ingress-operator
```

---

# 72. OpenShift Networking

Every Pod gets its own IP.

---

## Architecture

```text id="ocp4a53"
Pod A
  │
  ▼
Network
  │
  ▼
Pod B
```

---

## Benefits

```text id="ocp4a54"
Direct Connectivity

No NAT Required
```

---

# 73. OpenShift SDN

Traditional OpenShift networking.

---

## Components

```text id="ocp4a55"
Overlay Network

Pod Networking

Network Policies
```

---

## Architecture

```text id="ocp4a56"
Node A
 │
 ▼
OpenShift SDN
 │
 ▼
Node B
```

---

# 74. OVN-Kubernetes

Default networking solution in modern OpenShift.

---

## Benefits

```text id="ocp4a57"
Scalable

High Performance

Cloud Native
```

---

## Architecture

```text id="ocp4a58"
OVN
 │
 ▼
Virtual Networking
 │
 ▼
Pods
```

---

# 75. Networking Summary

```text id="ocp4a59"
Internet
    │
    ▼
Route
    │
    ▼
Router
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

# Important Commands

```bash id="ocp4a60"
oc get svc

oc describe svc

oc get endpoints

oc get routes

oc describe route

oc expose svc nginx-service

oc get ingresscontroller \
-n openshift-ingress-operator

oc get pods -n openshift-ingress

oc get networkpolicy

oc get all
```

---

# 76. Security in OpenShift

Security is one of the biggest reasons enterprises choose OpenShift.

OpenShift ships with stronger default security than Kubernetes.

---

## Security Layers

```text id="ocp5a1"
Authentication

Authorization

SCC

RBAC

Network Policies

Secrets

TLS

Audit Logs
```

---

## Security Architecture

```text id="ocp5a2"
User
 │
 ▼
Authentication
 │
 ▼
Authorization
 │
 ▼
SCC
 │
 ▼
Pod
```

---

# 77. OpenShift Security Model

OpenShift follows a defense-in-depth approach.

---

## Layers

```text id="ocp5a3"
Identity

Access

Network

Containers

Infrastructure
```

---

## Goal

```text id="ocp5a4"
Least Privilege
```

---

# 78. Security Context Constraints (SCC)

One of the most important OpenShift concepts.

Most common interview question.

---

## What is SCC?

SCC controls:

```text id="ocp5a5"
User Permissions

Container Permissions

Pod Security
```

---

## Kubernetes Equivalent

```text id="ocp5a6"
Pod Security Policies (Deprecated)

Pod Security Standards
```

---

## OpenShift Equivalent

```text id="ocp5a7"
Security Context Constraints
```

---

# 79. Why SCC?

Prevent containers from gaining excessive privileges.

---

## Example

Without SCC:

```text id="ocp5a8"
Container Runs As Root
```

Possible risk.

---

## With SCC

```text id="ocp5a9"
Restricted Access
```

---

## Benefits

```text id="ocp5a10"
Security

Compliance

Isolation
```

---

# 80. SCC Architecture

```text id="ocp5a11"
User
 │
 ▼
SCC
 │
 ▼
Pod Validation
 │
 ▼
Allow / Deny
```

---

## Examples

```text id="ocp5a12"
restricted

anyuid

hostnetwork

privileged
```

---

# 81. View SCCs

---

## List SCCs

```bash id="ocp5a13"
oc get scc
```

---

## Describe SCC

```bash id="ocp5a14"
oc describe scc restricted
```

---

## Output

```text id="ocp5a15"
Allowed Users

Capabilities

RunAsUser

Volumes
```

---

# 82. Restricted SCC

Default SCC.

---

## Characteristics

```text id="ocp5a16"
Non-Root Containers

Limited Capabilities

Restricted Volumes
```

---

## Benefits

```text id="ocp5a17"
Most Secure
```

---

## Recommended

```text id="ocp5a18"
Production Workloads
```

---

# 83. anyuid SCC

Allows arbitrary user IDs.

---

## Use Cases

```text id="ocp5a19"
Legacy Applications

Vendor Applications
```

---

## Risks

```text id="ocp5a20"
Higher Privileges
```

---

## Assign

```bash id="ocp5a21"
oc adm policy add-scc-to-user \
anyuid \
-z app-sa
```

---

# 84. Privileged SCC

Highest privilege level.

---

## Allows

```text id="ocp5a22"
Host Access

Root Access

Host Network

Host PID
```

---

## Use Cases

```text id="ocp5a23"
Storage Drivers

Monitoring Agents

Infrastructure Components
```

---

## Warning

```text id="ocp5a24"
Avoid For Applications
```

---

# 85. SCC vs Pod Security Standards

Interview Question.

---

## Comparison

| SCC              | Pod Security Standards |
| ---------------- | ---------------------- |
| OpenShift        | Kubernetes             |
| More Granular    | Simpler                |
| Mature           | Newer                  |
| Enterprise Focus | Generic                |

---

## Summary

```text id="ocp5a25"
OpenShift

↓

SCC

Kubernetes

↓

PSS
```

---

# 86. Security Context

Defines security settings for containers.

---

## Example

```yaml id="ocp5a26"
securityContext:

  runAsNonRoot: true

  runAsUser: 1001
```

---

## Benefits

```text id="ocp5a27"
Non-Root Containers

Improved Security
```

---

# 87. RunAsUser

Specifies container user.

---

## Example

```yaml id="ocp5a28"
securityContext:

  runAsUser: 1001
```

---

## Architecture

```text id="ocp5a29"
Container

↓

UID 1001
```

---

# 88. runAsNonRoot

Prevents root execution.

---

## Example

```yaml id="ocp5a30"
securityContext:

  runAsNonRoot: true
```

---

## Benefits

```text id="ocp5a31"
Reduced Attack Surface
```

---

# 89. Linux Capabilities

Fine-grained privileges.

---

## Examples

```text id="ocp5a32"
NET_ADMIN

SYS_ADMIN

CHOWN

SETUID
```

---

## Example

```yaml id="ocp5a33"
capabilities:

  drop:

  - ALL
```

---

## Best Practice

```text id="ocp5a34"
Drop Unused Capabilities
```

---

# 90. Secrets

Store sensitive information.

---

## Examples

```text id="ocp5a35"
Passwords

Tokens

Certificates

API Keys
```

---

## Architecture

```text id="ocp5a36"
Secret
   │
   ▼
Pod
```

---

## Create Secret

```bash id="ocp5a37"
oc create secret generic db-secret \
--from-literal=password=secret123
```

---

# 91. Secret YAML

---

## Example

```yaml id="ocp5a38"
apiVersion: v1

kind: Secret

metadata:

  name: db-secret

type: Opaque

data:

  password: c2VjcmV0
```

---

## Verify

```bash id="ocp5a39"
oc get secrets
```

---

# 92. Mount Secrets

---

## Example

```yaml id="ocp5a40"
volumes:

- name: secret-volume

  secret:

    secretName: db-secret
```

---

## Benefits

```text id="ocp5a41"
Secure Distribution
```

---

# 93. ConfigMaps

Store non-sensitive configuration.

---

## Examples

```text id="ocp5a42"
Application Config

URLs

Feature Flags
```

---

## Create

```bash id="ocp5a43"
oc create configmap app-config \
--from-literal=env=prod
```

---

## Verify

```bash id="ocp5a44"
oc get configmaps
```

---

# 94. ConfigMap YAML

---

## Example

```yaml id="ocp5a45"
apiVersion: v1

kind: ConfigMap

metadata:

  name: app-config

data:

  APP_ENV: prod
```

---

# 95. Service CA

OpenShift automatically generates certificates.

---

## Benefits

```text id="ocp5a46"
Internal TLS

Service Encryption

Automation
```

---

## Architecture

```text id="ocp5a47"
Service

↓

Certificate

↓

TLS
```

---

# 96. OpenShift OAuth

OpenShift includes built-in OAuth.

---

## Architecture

```text id="ocp5a48"
User
 │
 ▼
OAuth Server
 │
 ▼
API Server
```

---

## Benefits

```text id="ocp5a49"
SSO

Central Authentication
```

---

# 97. Audit Logging

Tracks cluster activity.

---

## Captures

```text id="ocp5a50"
Logins

API Requests

Resource Changes
```

---

## Benefits

```text id="ocp5a51"
Compliance

Forensics

Security Monitoring
```

---

# 98. Network Policies

Control Pod communication.

---

## Architecture

```text id="ocp5a52"
Pod A
 │
 ▼
Network Policy
 │
 ▼
Pod B
```

---

## Benefits

```text id="ocp5a53"
Micro-Segmentation

Zero Trust
```

---

# 99. Example Network Policy

---

## Allow Frontend → Backend

```yaml id="ocp5a54"
apiVersion: networking.k8s.io/v1

kind: NetworkPolicy

metadata:

  name: allow-frontend

spec:

  podSelector:

    matchLabels:

      app: backend
```

---

## Apply

```bash id="ocp5a55"
oc apply -f networkpolicy.yaml
```

---

# 100. OpenShift Security Summary

```text id="ocp5a56"
Authentication
       │
       ▼
OAuth
       │
       ▼
RBAC
       │
       ▼
SCC
       │
       ▼
Security Context
       │
       ▼
Network Policies
       │
       ▼
Secrets
```

---

# Important Commands

```bash id="ocp5a57"
oc get scc

oc describe scc restricted

oc get secrets

oc create secret generic

oc get configmaps

oc create configmap

oc get networkpolicy

oc describe networkpolicy

oc get users

oc get groups

oc auth can-i create pods

oc adm policy add-scc-to-user

oc get oauth
```

---

# 101. Storage in OpenShift

Applications often require persistent storage.

Containers are temporary, but data must survive restarts.

---

## Storage Types

```text id="ocp6a1"
Ephemeral Storage

Persistent Storage
```

---

## Architecture

```text id="ocp6a2"
Application
     │
     ▼
Persistent Volume Claim
     │
     ▼
Persistent Volume
     │
     ▼
Storage Backend
```

---

## Benefits

```text id="ocp6a3"
Data Persistence

Reliability

Stateful Applications
```

---

# 102. Ephemeral Storage

Temporary storage tied to Pod lifecycle.

---

## Characteristics

```text id="ocp6a4"
Pod Deleted

↓

Data Lost
```

---

## Examples

```text id="ocp6a5"
emptyDir

Container Writable Layer
```

---

## Use Cases

```text id="ocp6a6"
Caching

Temporary Files

Scratch Space
```

---

# 103. emptyDir Volume

Created when Pod starts.

Deleted when Pod ends.

---

## Example

```yaml id="ocp6a7"
volumes:

- name: cache

  emptyDir: {}
```

---

## Architecture

```text id="ocp6a8"
Pod Start
    │
    ▼
emptyDir Created
    │
    ▼
Pod Deleted
    │
    ▼
Volume Deleted
```

---

# 104. Persistent Storage

Persistent storage survives Pod recreation.

---

## Architecture

```text id="ocp6a9"
Pod
 │
 ▼
PVC
 │
 ▼
PV
 │
 ▼
Storage
```

---

## Benefits

```text id="ocp6a10"
Database Storage

File Storage

Application Data
```

---

# 105. Persistent Volumes (PV)

PV represents actual storage.

---

## Examples

```text id="ocp6a11"
NFS

AWS EBS

Azure Disk

Ceph

vSphere
```

---

## Architecture

```text id="ocp6a12"
Storage Device
       │
       ▼
Persistent Volume
```

---

## View PVs

```bash id="ocp6a13"
oc get pv
```

---

# 106. Persistent Volume YAML

---

## Example

```yaml id="ocp6a14"
apiVersion: v1

kind: PersistentVolume

metadata:

  name: pv-data

spec:

  capacity:

    storage: 10Gi

  accessModes:

  - ReadWriteOnce

  hostPath:

    path: /data
```

---

## Create

```bash id="ocp6a15"
oc apply -f pv.yaml
```

---

# 107. Persistent Volume Claims (PVC)

PVC requests storage.

---

## Architecture

```text id="ocp6a16"
Application
      │
      ▼
PVC Request
      │
      ▼
PV Assigned
```

---

## Analogy

```text id="ocp6a17"
PV = Apartment

PVC = Rental Request
```

---

## View PVCs

```bash id="ocp6a18"
oc get pvc
```

---

# 108. PVC YAML Example

---

## Example

```yaml id="ocp6a19"
apiVersion: v1

kind: PersistentVolumeClaim

metadata:

  name: app-pvc

spec:

  accessModes:

  - ReadWriteOnce

  resources:

    requests:

      storage: 5Gi
```

---

## Apply

```bash id="ocp6a20"
oc apply -f pvc.yaml
```

---

# 109. Access Modes

Define how storage is mounted.

---

## Modes

```text id="ocp6a21"
ReadWriteOnce (RWO)

ReadOnlyMany (ROX)

ReadWriteMany (RWX)
```

---

## Explanation

### ReadWriteOnce

```text id="ocp6a22"
Single Node

Read + Write
```

---

### ReadOnlyMany

```text id="ocp6a23"
Multiple Nodes

Read Only
```

---

### ReadWriteMany

```text id="ocp6a24"
Multiple Nodes

Read + Write
```

---

# 110. PVC Status

---

## Common States

```text id="ocp6a25"
Pending

Bound

Lost
```

---

## Verify

```bash id="ocp6a26"
oc get pvc
```

---

## Describe

```bash id="ocp6a27"
oc describe pvc app-pvc
```

---

# 111. Mount PVC into Pod

---

## Example

```yaml id="ocp6a28"
volumes:

- name: app-storage

  persistentVolumeClaim:

    claimName: app-pvc
```

---

## Mount Path

```yaml id="ocp6a29"
volumeMounts:

- mountPath: /data

  name: app-storage
```

---

## Architecture

```text id="ocp6a30"
Pod
 │
 ▼
PVC
 │
 ▼
PV
 │
 ▼
Storage
```

---

# 112. Storage Classes

StorageClasses automate storage provisioning.

---

## Without StorageClass

```text id="ocp6a31"
Admin Creates PV

User Creates PVC
```

---

## With StorageClass

```text id="ocp6a32"
User Creates PVC

↓

Storage Automatically Created
```

---

## Benefits

```text id="ocp6a33"
Automation

Scalability

Self-Service
```

---

# 113. View Storage Classes

---

## Command

```bash id="ocp6a34"
oc get storageclass
```

---

## Describe

```bash id="ocp6a35"
oc describe storageclass
```

---

## Output

```text id="ocp6a36"
Provisioner

Parameters

Policies
```

---

# 114. StorageClass YAML

---

## Example

```yaml id="ocp6a37"
apiVersion: storage.k8s.io/v1

kind: StorageClass

metadata:

  name: fast-storage

provisioner: kubernetes.io/aws-ebs
```

---

## Create

```bash id="ocp6a38"
oc apply -f storageclass.yaml
```

---

# 115. Dynamic Provisioning

Most modern OpenShift clusters use dynamic provisioning.

---

## Workflow

```text id="ocp6a39"
PVC Created
      │
      ▼
StorageClass
      │
      ▼
Provisioner
      │
      ▼
PV Created Automatically
```

---

## Benefits

```text id="ocp6a40"
No Manual PV Creation
```

---

# 116. CSI (Container Storage Interface)

Modern storage integration standard.

---

## Architecture

```text id="ocp6a41"
OpenShift
     │
     ▼
CSI Driver
     │
     ▼
Storage System
```

---

## Benefits

```text id="ocp6a42"
Vendor Neutral

Extensible

Cloud Native
```

---

# 117. Common CSI Drivers

---

## Examples

```text id="ocp6a43"
AWS EBS CSI

Azure Disk CSI

Azure Files CSI

GCP PD CSI

Ceph CSI

vSphere CSI
```

---

## View CSI Drivers

```bash id="ocp6a44"
oc get csidrivers
```

---

# 118. OpenShift Data Foundation (ODF)

Enterprise storage solution for OpenShift.

Formerly called:

```text id="ocp6a45"
OpenShift Container Storage (OCS)
```

---

## Features

```text id="ocp6a46"
Block Storage

File Storage

Object Storage
```

---

## Architecture

```text id="ocp6a47"
Applications
      │
      ▼
ODF
      │
      ▼
Ceph Storage
```

---

# 119. Ceph Storage

Popular storage backend.

---

## Provides

```text id="ocp6a48"
Block

File

Object
```

storage.

---

## Benefits

```text id="ocp6a49"
Highly Available

Scalable

Enterprise Ready
```

---

# 120. Storage Troubleshooting

Most common storage issues.

---

## PVC Pending

Causes:

```text id="ocp6a50"
No StorageClass

No PV

Provisioner Failure
```

---

## Verify

```bash id="ocp6a51"
oc describe pvc
```

---

## PV Not Bound

```text id="ocp6a52"
Capacity Mismatch

Access Mode Mismatch
```

---

## Volume Mount Failure

```text id="ocp6a53"
Permissions

Node Problems

Storage Backend Issues
```

---

# 121. Stateful Applications

Applications requiring persistent storage.

---

## Examples

```text id="ocp6a54"
PostgreSQL

MySQL

MongoDB

Redis

Kafka
```

---

## Architecture

```text id="ocp6a55"
Application
      │
      ▼
Persistent Storage
      │
      ▼
Data Survives Restart
```

---

# 122. Storage Best Practices

---

## Recommendations

```text id="ocp6a56"
Use Dynamic Provisioning

Use StorageClasses

Use CSI Drivers

Monitor Capacity

Backup Regularly
```

---

## Avoid

```text id="ocp6a57"
hostPath In Production

Manual PV Management
```

---

# 123. OpenShift Storage Architecture

```text id="ocp6a58"
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
CSI Driver
      │
      ▼
Storage Backend
```

---

# Important Commands

```bash id="ocp6a59"
oc get pv

oc get pvc

oc describe pvc

oc describe pv

oc get storageclass

oc describe storageclass

oc get csidrivers

oc get volumesnapshotclass

oc get pods

oc get events
```

---

# 124. Operators in OpenShift

Operators are one of OpenShift's most powerful features.

They automate deployment, configuration, upgrades, backups, and lifecycle management of applications.

---

## Traditional Management

```text id="ocp7a1"
Install

Configure

Upgrade

Backup

Monitor
```

All manual.

---

## Operator-Based Management

```text id="ocp7a2"
Install Operator

↓

Operator Automates Everything
```

---

## Benefits

```text id="ocp7a3"
Automation

Consistency

Reliability

Self-Healing
```

---

# 125. What is an Operator?

An Operator extends Kubernetes using:

```text id="ocp7a4"
Custom Resources

Controllers

Automation Logic
```

---

## Architecture

```text id="ocp7a5"
Custom Resource
       │
       ▼
Operator
       │
       ▼
Application
```

---

## Examples

```text id="ocp7a6"
PostgreSQL

MongoDB

Kafka

Prometheus

OpenShift Components
```

---

# 126. Operator Pattern

Operators continuously watch resources.

---

## Workflow

```text id="ocp7a7"
User Creates Resource
         │
         ▼
Operator Watches
         │
         ▼
Operator Acts
         │
         ▼
Desired State Achieved
```

---

## Example

```text id="ocp7a8"
Database Resource

↓

Operator

↓

Database Created
```

---

# 127. Why OpenShift Uses Operators

OpenShift itself is managed using Operators.

---

## Managed Components

```text id="ocp7a9"
Monitoring

Ingress

Registry

Networking

Storage

Authentication
```

---

## Architecture

```text id="ocp7a10"
OpenShift
    │
    ▼
Operators
    │
    ▼
Cluster Services
```

---

# 128. Operator Lifecycle Manager (OLM)

OLM manages Operators.

---

## Responsibilities

```text id="ocp7a11"
Install Operators

Upgrade Operators

Manage Dependencies

Lifecycle Management
```

---

## Architecture

```text id="ocp7a12"
OperatorHub
     │
     ▼
OLM
     │
     ▼
Operator
```

---

# 129. OLM Components

---

## Components

```text id="ocp7a13"
Catalog Sources

Subscriptions

Install Plans

CSV
```

---

## Architecture

```text id="ocp7a14"
Catalog
   │
   ▼
Subscription
   │
   ▼
Install Plan
   │
   ▼
Operator
```

---

# 130. Cluster Service Version (CSV)

Describes Operator metadata.

---

## Contains

```text id="ocp7a15"
Version

Permissions

Resources

Install Instructions
```

---

## View CSVs

```bash id="ocp7a16"
oc get csv -A
```

---

## Describe CSV

```bash id="ocp7a17"
oc describe csv
```

---

# 131. Catalog Sources

Catalogs contain Operators.

---

## Examples

```text id="ocp7a18"
Red Hat Catalog

Certified Catalog

Community Catalog
```

---

## View Catalogs

```bash id="ocp7a19"
oc get catalogsource \
-n openshift-marketplace
```

---

# 132. Subscriptions

Subscriptions tell OLM what Operator to install.

---

## Architecture

```text id="ocp7a20"
Subscription
      │
      ▼
Operator Installed
```

---

## View

```bash id="ocp7a21"
oc get subscriptions -A
```

---

## Describe

```bash id="ocp7a22"
oc describe subscription
```

---

# 133. Install Plans

Install Plans define installation actions.

---

## Workflow

```text id="ocp7a23"
Subscription
      │
      ▼
Install Plan
      │
      ▼
Operator Installation
```

---

## View

```bash id="ocp7a24"
oc get installplans -A
```

---

# 134. OperatorHub

OperatorHub is the Operator marketplace.

---

## Architecture

```text id="ocp7a25"
OperatorHub
      │
      ▼
Operators
      │
      ▼
Cluster
```

---

## Categories

```text id="ocp7a26"
Database

Monitoring

Security

Storage

CI/CD
```

---

# 135. Access OperatorHub

---

## Web Console

```text id="ocp7a27"
Operators

↓

OperatorHub
```

---

## Benefits

```text id="ocp7a28"
Easy Installation

Certified Operators

Lifecycle Management
```

---

# 136. Installing an Operator

---

## Workflow

```text id="ocp7a29"
OperatorHub
     │
     ▼
Select Operator
     │
     ▼
Install
     │
     ▼
Subscription Created
     │
     ▼
Operator Running
```

---

## Verify

```bash id="ocp7a30"
oc get csv -A
```

---

# 137. Namespaced Operators

Operate within a single namespace.

---

## Architecture

```text id="ocp7a31"
Namespace
    │
    ▼
Operator
    │
    ▼
Applications
```

---

## Benefits

```text id="ocp7a32"
Isolation

Security
```

---

# 138. Cluster-Wide Operators

Manage entire clusters.

---

## Examples

```text id="ocp7a33"
Ingress

Monitoring

Storage

Networking
```

---

## Architecture

```text id="ocp7a34"
Cluster
  │
  ▼
Operator
  │
  ▼
All Namespaces
```

---

# 139. OpenShift Cluster Operators

Special Operators managing OpenShift itself.

---

## Examples

```text id="ocp7a35"
Authentication

Ingress

Image Registry

Monitoring

Storage
```

---

## View Operators

```bash id="ocp7a36"
oc get clusteroperators
```

---

# 140. Cluster Operator Status

Most common OpenShift admin command.

---

## Command

```bash id="ocp7a37"
oc get co
```

---

## Example Output

```text id="ocp7a38"
Available

Progressing

Degraded
```

---

# 141. Important Cluster Operators

---

## Authentication Operator

```text id="ocp7a39"
OAuth

Identity Providers
```

---

## Ingress Operator

```text id="ocp7a40"
Routers

Ingress Controllers
```

---

## Registry Operator

```text id="ocp7a41"
Internal Image Registry
```

---

## Monitoring Operator

```text id="ocp7a42"
Prometheus

Alertmanager

Grafana Components
```

---

# 142. Machine API Operator

Manages infrastructure nodes.

---

## Responsibilities

```text id="ocp7a43"
Node Provisioning

Scaling

Replacement
```

---

## Architecture

```text id="ocp7a44"
MachineSet
     │
     ▼
Machine
     │
     ▼
Node
```

---

# 143. MachineSets

MachineSets are similar to ReplicaSets for Nodes.

---

## Workflow

```text id="ocp7a45"
MachineSet
      │
      ▼
Machines
      │
      ▼
Nodes
```

---

## View

```bash id="ocp7a46"
oc get machinesets -A
```

---

# 144. MachineConfigs

Manage node configuration.

---

## Examples

```text id="ocp7a47"
Kernel Settings

Systemd

OS Configuration
```

---

## Architecture

```text id="ocp7a48"
MachineConfig
       │
       ▼
Machine Config Operator
       │
       ▼
Nodes
```

---

# 145. Machine Config Operator (MCO)

Applies node configuration changes.

---

## Responsibilities

```text id="ocp7a49"
OS Updates

Configuration Changes

Node Reboots
```

---

## Verify

```bash id="ocp7a50"
oc get mcp
```

---

# 146. Machine Config Pools (MCP)

Group nodes for configuration.

---

## Examples

```text id="ocp7a51"
Master

Worker

Custom Pools
```

---

## View

```bash id="ocp7a52"
oc get mcp
```

---

# 147. Operator Troubleshooting

---

## Check Operators

```bash id="ocp7a53"
oc get co
```

---

## Check CSVs

```bash id="ocp7a54"
oc get csv -A
```

---

## Check Subscriptions

```bash id="ocp7a55"
oc get subscriptions -A
```

---

## Check Install Plans

```bash id="ocp7a56"
oc get installplans -A
```

---

# 148. Common Operator Issues

---

## Operator Not Installing

```text id="ocp7a57"
Catalog Issues

Subscription Issues

Permission Issues
```

---

## CSV Failed

```text id="ocp7a58"
Dependency Problems

Resource Problems
```

---

## Operator Degraded

```text id="ocp7a59"
Backend Failure

Configuration Issues
```

---

# 149. Operator Architecture Summary

```text id="ocp7a60"
OperatorHub
      │
      ▼
OLM
      │
      ▼
Subscription
      │
      ▼
CSV
      │
      ▼
Operator
      │
      ▼
Managed Application
```

---

# 150. OpenShift Operator Ecosystem

```text id="ocp7a61"
Cluster Operators
      │
      ├── Authentication
      ├── Ingress
      ├── Registry
      ├── Monitoring
      └── Storage

OperatorHub
      │
      ├── Databases
      ├── Messaging
      ├── Security
      ├── CI/CD
      └── Observability
```

---

# Important Commands

```bash id="ocp7a62"
oc get co

oc describe co

oc get csv -A

oc describe csv

oc get subscriptions -A

oc get installplans -A

oc get catalogsource \
-n openshift-marketplace

oc get machinesets -A

oc get machineconfigs

oc get mcp

oc describe mcp
```

---

# 151. OpenShift Image Registry

OpenShift includes a built-in container image registry.

This is one of the major differences between Kubernetes and OpenShift.

---

## Purpose

Store container images inside the cluster.

---

## Architecture

```text id="ocp8a1"
Developer
    │
    ▼
Build
    │
    ▼
Internal Registry
    │
    ▼
Deployment
```

---

## Benefits

```text id="ocp8a2"
Integrated

Secure

Enterprise Ready

No External Registry Required
```

---

# 152. Registry Operator

The Image Registry is managed by an Operator.

---

## Architecture

```text id="ocp8a3"
Registry Operator
        │
        ▼
Image Registry
        │
        ▼
Container Images
```

---

## Verify

```bash id="ocp8a4"
oc get clusteroperator image-registry
```

---

## View Registry Pods

```bash id="ocp8a5"
oc get pods \
-n openshift-image-registry
```

---

# 153. Image Registry Architecture

```text id="ocp8a6"
Source Code
     │
     ▼
Build
     │
     ▼
Image Registry
     │
     ▼
Deployment
     │
     ▼
Pods
```

---

## Components

```text id="ocp8a7"
Registry

Storage

Authentication

ImageStreams
```

---

# 154. ImageStreams

One of OpenShift's most unique features.

---

## What is an ImageStream?

Tracks container images.

Acts as an abstraction layer.

---

## Architecture

```text id="ocp8a8"
Container Image
        │
        ▼
ImageStream
        │
        ▼
Deployment
```

---

## Benefits

```text id="ocp8a9"
Version Tracking

Automation

Image Updates
```

---

# 155. Why ImageStreams?

Instead of referencing images directly:

```yaml id="ocp8a10"
image: nginx:latest
```

Use:

```text id="ocp8a11"
ImageStream
```

---

## Advantages

```text id="ocp8a12"
Controlled Updates

Version History

Triggers
```

---

# 156. Create ImageStream

---

## Command

```bash id="ocp8a13"
oc create imagestream nginx
```

---

## Verify

```bash id="ocp8a14"
oc get imagestreams
```

---

## Short Form

```bash id="ocp8a15"
oc get is
```

---

# 157. ImageStream YAML

---

## Example

```yaml id="ocp8a16"
apiVersion: image.openshift.io/v1

kind: ImageStream

metadata:

  name: nginx
```

---

## Apply

```bash id="ocp8a17"
oc apply -f imagestream.yaml
```

---

# 158. Import Images

Import external images into OpenShift.

---

## Example

```bash id="ocp8a18"
oc import-image nginx \
--from=docker.io/library/nginx
```

---

## Verify

```bash id="ocp8a19"
oc get is
```

---

# 159. Builds in OpenShift

OpenShift includes a built-in build system.

---

## Workflow

```text id="ocp8a20"
Source Code
      │
      ▼
Build
      │
      ▼
Image
      │
      ▼
Registry
```

---

## Benefits

```text id="ocp8a21"
Integrated CI

Automation

Developer Friendly
```

---

# 160. BuildConfig

BuildConfig defines build processes.

---

## Architecture

```text id="ocp8a22"
BuildConfig
      │
      ▼
Build
      │
      ▼
Container Image
```

---

## View

```bash id="ocp8a23"
oc get bc
```

---

## Describe

```bash id="ocp8a24"
oc describe bc
```

---

# 161. Build Strategies

OpenShift supports multiple build types.

---

## Types

```text id="ocp8a25"
Source-to-Image (S2I)

Docker Build

Custom Build

Pipeline Build
```

---

## Most Common

```text id="ocp8a26"
S2I
```

---

# 162. Source-to-Image (S2I)

Unique OpenShift feature.

Builds images directly from source code.

---

## Architecture

```text id="ocp8a27"
Source Code
      │
      ▼
Builder Image
      │
      ▼
Application Image
```

---

## Benefits

```text id="ocp8a28"
No Dockerfile Required

Fast

Developer Friendly
```

---

# 163. S2I Workflow

```text id="ocp8a29"
Git Repository
      │
      ▼
S2I Build
      │
      ▼
Image Registry
      │
      ▼
Deployment
```

---

## Supported Languages

```text id="ocp8a30"
Java

Node.js

Python

PHP

Ruby

Go
```

---

# 164. BuildConfig YAML

---

## Example

```yaml id="ocp8a31"
apiVersion: build.openshift.io/v1

kind: BuildConfig

metadata:

  name: myapp-build
```

---

## Verify

```bash id="ocp8a32"
oc get bc
```

---

# 165. Start Builds

---

## Trigger Build

```bash id="ocp8a33"
oc start-build myapp-build
```

---

## Follow Logs

```bash id="ocp8a34"
oc start-build myapp-build \
--follow
```

---

## View Builds

```bash id="ocp8a35"
oc get builds
```

---

# 166. Build Triggers

Builds can start automatically.

---

## Trigger Types

```text id="ocp8a36"
Git Changes

Image Changes

Manual Triggers

Webhooks
```

---

## Workflow

```text id="ocp8a37"
Git Push
    │
    ▼
Build Trigger
    │
    ▼
Build Starts
```

---

# 167. Deployment Triggers

Deploy automatically after builds.

---

## Architecture

```text id="ocp8a38"
Build
 │
 ▼
New Image
 │
 ▼
Deployment Trigger
 │
 ▼
Deployment
```

---

## Benefits

```text id="ocp8a39"
Continuous Delivery
```

---

# 168. Binary Builds

Upload local source code.

---

## Example

```bash id="ocp8a40"
oc start-build myapp \
--from-dir=.
```

---

## Use Cases

```text id="ocp8a41"
Local Development

Testing

CI/CD
```

---

# 169. OpenShift Pipelines

Modern CI/CD solution.

Based on:

```text id="ocp8a42"
Tekton
```

---

## Architecture

```text id="ocp8a43"
Source
  │
  ▼
Pipeline
  │
  ▼
Build
  │
  ▼
Deploy
```

---

## Benefits

```text id="ocp8a44"
Cloud Native

Kubernetes Native

Scalable
```

---

# 170. Tekton

Tekton powers OpenShift Pipelines.

---

## Components

```text id="ocp8a45"
Tasks

Pipelines

PipelineRuns

Triggers
```

---

## Architecture

```text id="ocp8a46"
Task
 │
 ▼
Pipeline
 │
 ▼
Execution
```

---

# 171. Tekton Tasks

Smallest execution unit.

---

## Example

```text id="ocp8a47"
Build

Test

Deploy
```

---

## Workflow

```text id="ocp8a48"
Task 1

↓

Task 2

↓

Task 3
```

---

# 172. Pipelines

Collection of tasks.

---

## Example Flow

```text id="ocp8a49"
Clone Repo

↓

Build

↓

Test

↓

Deploy
```

---

## Benefits

```text id="ocp8a50"
Automation

Consistency
```

---

# 173. OpenShift GitOps

GitOps implementation for OpenShift.

Based on:

```text id="ocp8a51"
Argo CD
```

---

## Architecture

```text id="ocp8a52"
Git
 │
 ▼
ArgoCD
 │
 ▼
Cluster
```

---

## Benefits

```text id="ocp8a53"
Version Control

Auditability

Automation
```

---

# 174. Argo CD

Popular GitOps platform.

---

## Workflow

```text id="ocp8a54"
Git Commit
      │
      ▼
ArgoCD
      │
      ▼
Cluster Sync
```

---

## Features

```text id="ocp8a55"
Auto Sync

Rollback

Drift Detection
```

---

# 175. CI/CD Architecture in OpenShift

```text id="ocp8a56"
Developer
      │
      ▼
Git Repository
      │
      ▼
BuildConfig
      │
      ▼
ImageStream
      │
      ▼
Registry
      │
      ▼
Deployment
```

---

# Important Commands

```bash id="ocp8a57"
oc get is

oc get imagestreams

oc create imagestream

oc import-image

oc get bc

oc describe bc

oc start-build

oc start-build --follow

oc get builds

oc logs build/BUILD_NAME

oc get pipelines

oc get tasks

oc get pipelineruns

oc get applications
```

---

# 176. OpenShift Monitoring

Monitoring is built into OpenShift by default.

Unlike vanilla Kubernetes, OpenShift automatically deploys a complete monitoring stack.

---

## Components

```text id="ocp9a1"
Prometheus

Alertmanager

Thanos

Grafana Components

Monitoring Operators
```

---

## Architecture

```text id="ocp9a2"
Applications
      │
      ▼
Metrics
      │
      ▼
Prometheus
      │
      ▼
Alertmanager
      │
      ▼
Administrators
```

---

## Benefits

```text id="ocp9a3"
Health Monitoring

Alerting

Capacity Planning

Troubleshooting
```

---

# 177. OpenShift Monitoring Stack

Installed automatically.

---

## Components

```text id="ocp9a4"
Cluster Monitoring Operator

Prometheus

Alertmanager

Thanos Querier

Node Exporter

Kube State Metrics
```

---

## Verify

```bash id="ocp9a5"
oc get pods \
-n openshift-monitoring
```

---

# 178. Cluster Monitoring Operator

Manages monitoring components.

---

## Responsibilities

```text id="ocp9a6"
Deploy Monitoring Stack

Upgrades

Configuration

Self-Healing
```

---

## Verify

```bash id="ocp9a7"
oc get co monitoring
```

---

# 179. Prometheus

Prometheus is the metrics collection engine.

---

## Responsibilities

```text id="ocp9a8"
Collect Metrics

Store Metrics

Query Metrics
```

---

## Architecture

```text id="ocp9a9"
Applications
      │
      ▼
Prometheus
      │
      ▼
Time Series Database
```

---

## Verify

```bash id="ocp9a10"
oc get pods \
-n openshift-monitoring
```

---

# 180. Metrics

Metrics are numerical measurements.

---

## Examples

```text id="ocp9a11"
CPU Usage

Memory Usage

Network Traffic

Disk Usage

Pod Count
```

---

## Example Metric

```text id="ocp9a12"
container_cpu_usage_seconds_total
```

---

# 181. PromQL

Prometheus Query Language.

---

## Example

```text id="ocp9a13"
up
```

Returns all targets.

---

## CPU Query

```text id="ocp9a14"
rate(
container_cpu_usage_seconds_total[5m]
)
```

---

## Memory Query

```text id="ocp9a15"
container_memory_usage_bytes
```

---

# 182. Alertmanager

Handles alerts.

---

## Workflow

```text id="ocp9a16"
Metric
  │
  ▼
Alert Rule
  │
  ▼
Alertmanager
  │
  ▼
Notification
```

---

## Notifications

```text id="ocp9a17"
Email

Slack

PagerDuty

Webhook
```

---

# 183. Alerting Rules

Define alert conditions.

---

## Example

```text id="ocp9a18"
CPU > 90%
```

for:

```text id="ocp9a19"
5 Minutes
```

---

## Workflow

```text id="ocp9a20"
Metric
 │
 ▼
Threshold
 │
 ▼
Alert
```

---

# 184. Thanos

Provides long-term metric storage.

---

## Benefits

```text id="ocp9a21"
Long Retention

Global Queries

Scalability
```

---

## Architecture

```text id="ocp9a22"
Prometheus
      │
      ▼
Thanos
      │
      ▼
Object Storage
```

---

# 185. Node Exporter

Collects node metrics.

---

## Metrics

```text id="ocp9a23"
CPU

Memory

Disk

Filesystem

Network
```

---

## Architecture

```text id="ocp9a24"
Node
 │
 ▼
Node Exporter
 │
 ▼
Prometheus
```

---

# 186. kube-state-metrics

Provides Kubernetes object metrics.

---

## Examples

```text id="ocp9a25"
Deployments

Pods

Nodes

Services
```

---

## Architecture

```text id="ocp9a26"
Kubernetes API
        │
        ▼
kube-state-metrics
        │
        ▼
Prometheus
```

---

# 187. User Workload Monitoring

Monitor application workloads.

---

## Disabled By Default

Can be enabled.

---

## Benefits

```text id="ocp9a27"
Application Metrics

Custom Dashboards

Business Metrics
```

---

## Architecture

```text id="ocp9a28"
Application
      │
      ▼
User Monitoring
      │
      ▼
Prometheus
```

---

# 188. Logging in OpenShift

Logging provides centralized log management.

---

## Components

```text id="ocp9a29"
Collectors

Storage

Visualization
```

---

## Traditional Stack

```text id="ocp9a30"
Fluentd

Elasticsearch

Kibana
```

---

## Modern Stack

```text id="ocp9a31"
Vector

Loki

Grafana
```

---

# 189. OpenShift Logging Operator

Manages logging stack.

---

## Responsibilities

```text id="ocp9a32"
Deploy Logging

Configure Collection

Manage Upgrades
```

---

## Verify

```bash id="ocp9a33"
oc get csv | grep logging
```

---

# 190. Log Collection Flow

```text id="ocp9a34"
Container Logs
       │
       ▼
Collector
       │
       ▼
Storage
       │
       ▼
Visualization
```

---

# 191. Fluentd

Traditional OpenShift log collector.

---

## Responsibilities

```text id="ocp9a35"
Collect Logs

Forward Logs

Transform Logs
```

---

## Architecture

```text id="ocp9a36"
Node
 │
 ▼
Fluentd
 │
 ▼
Storage
```

---

# 192. Vector

Modern collector replacing Fluentd.

---

## Benefits

```text id="ocp9a37"
Faster

Lower Resource Usage

Modern Architecture
```

---

## Workflow

```text id="ocp9a38"
Logs
 │
 ▼
Vector
 │
 ▼
Loki
```

---

# 193. Loki

Modern log storage backend.

---

## Benefits

```text id="ocp9a39"
Lightweight

Scalable

Cost Efficient
```

---

## Architecture

```text id="ocp9a40"
Applications
      │
      ▼
Logs
      │
      ▼
Loki
```

---

# 194. Kibana

Traditional log visualization platform.

---

## Features

```text id="ocp9a41"
Search

Dashboards

Analysis
```

---

## Architecture

```text id="ocp9a42"
Elasticsearch
      │
      ▼
Kibana
```

---

# 195. Grafana

Visualization platform.

---

## Uses

```text id="ocp9a43"
Metrics

Logs

Dashboards
```

---

## Architecture

```text id="ocp9a44"
Prometheus
      │
      ▼
Grafana
```

---

# 196. Cluster Logging Architecture

```text id="ocp9a45"
Applications
      │
      ▼
Logs
      │
      ▼
Vector / Fluentd
      │
      ▼
Loki / Elasticsearch
      │
      ▼
Grafana / Kibana
```

---

# 197. OpenShift Events

Events provide cluster activity information.

---

## Examples

```text id="ocp9a46"
Pod Started

Pod Failed

Volume Mounted

Image Pulled
```

---

## View Events

```bash id="ocp9a47"
oc get events
```

---

## Sorted Events

```bash id="ocp9a48"
oc get events \
--sort-by=.metadata.creationTimestamp
```

---

# 198. Health Monitoring

Monitor cluster health.

---

## Check Operators

```bash id="ocp9a49"
oc get co
```

---

## Check Nodes

```bash id="ocp9a50"
oc get nodes
```

---

## Check Pods

```bash id="ocp9a51"
oc get pods -A
```

---

# 199. Cluster Health Workflow

```text id="ocp9a52"
Operators
     │
     ▼
Nodes
     │
     ▼
Pods
     │
     ▼
Applications
```

---

# 200. Observability Architecture

```text id="ocp9a53"
Applications
      │
      ├── Metrics
      │      │
      │      ▼
      │  Prometheus
      │      │
      │      ▼
      │  Alertmanager
      │
      └── Logs
             │
             ▼
       Vector/Fluentd
             │
             ▼
         Loki
             │
             ▼
        Grafana
```

---

# Important Commands

```bash id="ocp9a54"
oc get co

oc get nodes

oc get pods -A

oc get events

oc adm top nodes

oc adm top pods

oc get pods \
-n openshift-monitoring

oc get pods \
-n openshift-logging

oc logs POD_NAME

oc describe pod POD_NAME

oc get alertmanager \
-n openshift-monitoring
```

---

# 201. OpenShift Virtualization

OpenShift Virtualization allows running Virtual Machines (VMs) alongside containers on the same OpenShift cluster.

Based on:

```text id="ocp10a1"
KubeVirt
```

---

## Why OpenShift Virtualization?

Many enterprises still run:

```text id="ocp10a2"
Windows Servers

Linux VMs

Legacy Applications

Traditional Middleware
```

OpenShift Virtualization enables migration without abandoning Kubernetes.

---

## Architecture

```text id="ocp10a3"
OpenShift Cluster
        │
        ├── Containers
        │
        └── Virtual Machines
```

---

## Benefits

```text id="ocp10a4"
Single Platform

Reduced Infrastructure

Hybrid Workloads

Cloud Native Transformation
```

---

# 202. KubeVirt

KubeVirt is the technology behind OpenShift Virtualization.

---

## Purpose

Run Virtual Machines as Kubernetes resources.

---

## Architecture

```text id="ocp10a5"
Virtual Machine
        │
        ▼
KubeVirt
        │
        ▼
Kubernetes
```

---

## Benefits

```text id="ocp10a6"
VM Management via Kubernetes

Unified Operations

Automation
```

---

# 203. Virtual Machine Architecture

---

## Components

```text id="ocp10a7"
VirtualMachine

VirtualMachineInstance

DataVolume

Storage
```

---

## Architecture

```text id="ocp10a8"
VirtualMachine
       │
       ▼
VirtualMachineInstance
       │
       ▼
Node
```

---

# 204. VirtualMachine (VM)

Declarative definition of a VM.

---

## Similar To

```text id="ocp10a9"
Deployment
```

for containers.

---

## Responsibilities

```text id="ocp10a10"
Desired State

CPU

Memory

Storage
```

---

# 205. VirtualMachineInstance (VMI)

Running instance of a VM.

---

## Similar To

```text id="ocp10a11"
Pod
```

in Kubernetes.

---

## Relationship

```text id="ocp10a12"
VirtualMachine

↓

VirtualMachineInstance
```

---

## Verify

```bash id="ocp10a13"
oc get vmi
```

---

# 206. VM Lifecycle

---

## Workflow

```text id="ocp10a14"
Create VM

↓

Start VM

↓

Running

↓

Stop VM
```

---

## Commands

```bash id="ocp10a15"
oc get vm

oc get vmi
```

---

# 207. VM YAML Example

---

## Basic VM

```yaml id="ocp10a16"
apiVersion: kubevirt.io/v1

kind: VirtualMachine

metadata:

  name: rhel-vm

spec:

  running: true
```

---

## Apply

```bash id="ocp10a17"
oc apply -f vm.yaml
```

---

# 208. DataVolumes

Used to provision VM storage.

---

## Architecture

```text id="ocp10a18"
DataVolume
     │
     ▼
PVC
     │
     ▼
Storage
```

---

## Benefits

```text id="ocp10a19"
Automated Storage Provisioning
```

---

# 209. VM Networking

VMs use Kubernetes networking.

---

## Architecture

```text id="ocp10a20"
Virtual Machine
        │
        ▼
Pod Network
        │
        ▼
Services
```

---

## Benefits

```text id="ocp10a21"
Unified Networking
```

---

# 210. Live Migration

Move running VMs between nodes.

---

## Workflow

```text id="ocp10a22"
Node A
  │
  ▼
Migration
  │
  ▼
Node B
```

---

## Benefits

```text id="ocp10a23"
Maintenance

High Availability
```

---

# 211. VM Templates

Predefined VM configurations.

---

## Examples

```text id="ocp10a24"
RHEL

Ubuntu

Windows

CentOS
```

---

## Benefits

```text id="ocp10a25"
Fast Deployment
```

---

# 212. OpenShift Service Mesh

Provides secure service-to-service communication.

---

## Based On

```text id="ocp10a26"
Istio
```

---

## Components

```text id="ocp10a27"
Istio

Envoy

Jaeger

Kiali
```

---

## Architecture

```text id="ocp10a28"
Service A

↓

Envoy

↓

Envoy

↓

Service B
```

---

# 213. What is a Service Mesh?

Infrastructure layer managing service communication.

---

## Responsibilities

```text id="ocp10a29"
Traffic Management

Security

Observability

Policy Enforcement
```

---

## Benefits

```text id="ocp10a30"
No Application Changes
```

---

# 214. Envoy Proxy

Data plane component.

---

## Architecture

```text id="ocp10a31"
Application
     │
     ▼
Envoy Sidecar
     │
     ▼
Network
```

---

## Responsibilities

```text id="ocp10a32"
Routing

TLS

Metrics

Tracing
```

---

# 215. Sidecar Pattern

Every application gets an Envoy proxy.

---

## Architecture

```text id="ocp10a33"
Pod
 │
 ├── Application
 │
 └── Envoy
```

---

## Benefits

```text id="ocp10a34"
Security

Observability

Traffic Control
```

---

# 216. Istio Control Plane

Manages the service mesh.

---

## Components

```text id="ocp10a35"
istiod
```

---

## Responsibilities

```text id="ocp10a36"
Configuration

Certificates

Policies
```

---

# 217. Mutual TLS (mTLS)

Encrypts service communication.

---

## Workflow

```text id="ocp10a37"
Service A

⇄

Encrypted

⇄

Service B
```

---

## Benefits

```text id="ocp10a38"
Authentication

Encryption

Zero Trust
```

---

# 218. Traffic Management

Control application traffic.

---

## Features

```text id="ocp10a39"
Canary Releases

Blue-Green Deployments

A/B Testing
```

---

## Example

```text id="ocp10a40"
90% → v1

10% → v2
```

---

# 219. Kiali

Service mesh visualization.

---

## Features

```text id="ocp10a41"
Traffic Graphs

Health Status

Topology
```

---

## Architecture

```text id="ocp10a42"
Istio
 │
 ▼
Kiali
 │
 ▼
Visualization
```

---

# 220. Jaeger

Distributed tracing system.

---

## Purpose

Track requests across microservices.

---

## Workflow

```text id="ocp10a43"
Request

↓

Service A

↓

Service B

↓

Service C
```

---

## Benefits

```text id="ocp10a44"
Performance Analysis

Troubleshooting
```

---

# 221. OpenShift Advanced Cluster Management (ACM)

Manage multiple OpenShift clusters.

---

## Purpose

Centralized cluster management.

---

## Architecture

```text id="ocp10a45"
ACM Hub
    │
    ├── Cluster A
    ├── Cluster B
    └── Cluster C
```

---

## Benefits

```text id="ocp10a46"
Governance

Security

Fleet Management
```

---

# 222. ACM Components

---

## Components

```text id="ocp10a47"
Hub Cluster

Managed Clusters

Policies

Applications
```

---

## Architecture

```text id="ocp10a48"
Hub Cluster
      │
      ▼
Managed Clusters
```

---

# 223. Cluster Governance

Apply policies across clusters.

---

## Examples

```text id="ocp10a49"
Security Policies

RBAC Policies

Compliance Policies
```

---

## Workflow

```text id="ocp10a50"
Policy

↓

ACM

↓

All Clusters
```

---

# 224. Multi-Cluster Application Management

Deploy applications to multiple clusters.

---

## Architecture

```text id="ocp10a51"
Application
      │
      ▼
ACM
      │
      ▼
Multiple Clusters
```

---

## Benefits

```text id="ocp10a52"
Centralized Deployment
```

---

# 225. OpenShift Enterprise Architecture

```text id="ocp10a53"
OpenShift
    │
    ├── Containers
    │
    ├── Virtual Machines
    │
    ├── Service Mesh
    │
    ├── GitOps
    │
    ├── Monitoring
    │
    ├── Logging
    │
    ├── Operators
    │
    └── Multi-Cluster Management
```

---

# Important Commands

```bash id="ocp10a54"
oc get vm

oc get vmi

oc get dv

oc get virtualmachine

oc get virtualmachineinstance

oc get smcp

oc get servicemeshmemberroll

oc get kiali

oc get jaeger

oc get managedclusters

oc get placementrules

oc get policies

oc get applications
```

---

# 226. OpenShift Installation and Cluster Deployment

Installing OpenShift is significantly different from installing Kubernetes.

OpenShift automates most cluster deployment tasks.

---

## Supported Platforms

```text id="ocp11a1"
AWS

Azure

Google Cloud

VMware vSphere

Bare Metal

OpenStack

IBM Cloud
```

---

## Installation Methods

```text id="ocp11a2"
Installer Provisioned Infrastructure (IPI)

User Provisioned Infrastructure (UPI)

OpenShift Local
```

---

## Recommendation

```text id="ocp11a3"
IPI
```

for most deployments.

---

# 227. Installer Provisioned Infrastructure (IPI)

OpenShift automatically creates infrastructure.

---

## Architecture

```text id="ocp11a4"
Installer
    │
    ▼
Cloud Resources
    │
    ▼
OpenShift Cluster
```

---

## Benefits

```text id="ocp11a5"
Fast

Automated

Supported
```

---

## Workflow

```text id="ocp11a6"
Install Config

↓

Create Cluster

↓

Deploy Infrastructure

↓

Configure OpenShift
```

---

# 228. User Provisioned Infrastructure (UPI)

Infrastructure is created manually.

---

## Architecture

```text id="ocp11a7"
Admin Creates Infrastructure

↓

Installer Deploys OpenShift
```

---

## Use Cases

```text id="ocp11a8"
Strict Enterprise Requirements

Custom Networking

Custom Security
```

---

## Drawback

```text id="ocp11a9"
More Complex
```

---

# 229. OpenShift Local

Single-node development environment.

---

## Former Name

```text id="ocp11a10"
CodeReady Containers (CRC)
```

---

## Use Cases

```text id="ocp11a11"
Learning

Development

Testing
```

---

## Components

```text id="ocp11a12"
Single Node

Built-In Registry

Built-In Console
```

---

# 230. Cluster Architecture

Production cluster architecture.

---

## High Availability Architecture

```text id="ocp11a13"
3 Control Plane Nodes

3+ Worker Nodes
```

---

## Diagram

```text id="ocp11a14"
Users
 │
 ▼
Load Balancer
 │
 ▼
Control Plane
 ├── Master 1
 ├── Master 2
 └── Master 3
 │
 ▼
Workers
 ├── Worker 1
 ├── Worker 2
 └── Worker 3
```

---

# 231. Control Plane Nodes

Manage cluster operations.

---

## Components

```text id="ocp11a15"
API Server

etcd

Scheduler

Controller Manager
```

---

## Responsibilities

```text id="ocp11a16"
Scheduling

Cluster State

Authentication
```

---

# 232. Worker Nodes

Run workloads.

---

## Components

```text id="ocp11a17"
CRI-O

kubelet

Pods

Networking
```

---

## Responsibilities

```text id="ocp11a18"
Run Applications

Mount Storage

Network Connectivity
```

---

# 233. etcd

Most critical component.

Stores cluster state.

---

## Contains

```text id="ocp11a19"
Pods

Deployments

Secrets

Nodes

Services
```

---

## Architecture

```text id="ocp11a20"
API Server
     │
     ▼
etcd
```

---

## Important

```text id="ocp11a21"
If etcd is lost

Cluster state is lost
```

---

# 234. etcd Backup

Critical administrative task.

---

## Backup

```bash id="ocp11a22"
oc adm must-gather
```

OpenShift also provides etcd backup scripts.

---

## Best Practice

```text id="ocp11a23"
Daily Backups
```

---

# 235. Cluster Upgrades

One of OpenShift's strongest features.

---

## Managed By

```text id="ocp11a24"
Cluster Version Operator (CVO)
```

---

## Workflow

```text id="ocp11a25"
Current Version

↓

Upgrade

↓

New Version
```

---

## Benefits

```text id="ocp11a26"
Automated

Supported

Safe
```

---

# 236. Cluster Version Operator (CVO)

Controls OpenShift upgrades.

---

## Responsibilities

```text id="ocp11a27"
Version Management

Operator Updates

Component Updates
```

---

## Verify

```bash id="ocp11a28"
oc get clusterversion
```

---

# 237. Upgrade Channels

---

## Examples

```text id="ocp11a29"
stable

fast

candidate
```

---

## Recommended

```text id="ocp11a30"
stable
```

for production.

---

# 238. Upgrade Process

---

## Workflow

```text id="ocp11a31"
Check Health

↓

Backup

↓

Upgrade

↓

Verify
```

---

## Verify Health

```bash id="ocp11a32"
oc get co
```

---

# 239. Node Maintenance

Perform maintenance safely.

---

## Cordon Node

Prevent scheduling.

```bash id="ocp11a33"
oc adm cordon NODE
```

---

## Drain Node

Move workloads.

```bash id="ocp11a34"
oc adm drain NODE
```

---

## Uncordon

```bash id="ocp11a35"
oc adm uncordon NODE
```

---

# 240. Machine API

Automates node lifecycle.

---

## Responsibilities

```text id="ocp11a36"
Provision Nodes

Replace Nodes

Scale Nodes
```

---

## Architecture

```text id="ocp11a37"
MachineSet
     │
     ▼
Machine
     │
     ▼
Node
```

---

# 241. Infrastructure Nodes

Dedicated nodes for platform services.

---

## Run

```text id="ocp11a38"
Registry

Monitoring

Logging

Routers
```

---

## Benefits

```text id="ocp11a39"
Workload Isolation
```

---

# 242. OpenShift Networking Architecture

---

## Components

```text id="ocp11a40"
OVN-Kubernetes

Ingress

Routes

Services
```

---

## Diagram

```text id="ocp11a41"
Internet
 │
 ▼
Router
 │
 ▼
Service
 │
 ▼
Pod
```

---

# 243. OpenShift DNS

Built-in DNS service.

---

## Responsibilities

```text id="ocp11a42"
Service Discovery

Route Resolution
```

---

## Verify

```bash id="ocp11a43"
oc get dns.operator/default
```

---

# 244. OpenShift Ingress Operator

Manages ingress controllers.

---

## Verify

```bash id="ocp11a44"
oc get ingresscontroller \
-n openshift-ingress-operator
```

---

## Responsibilities

```text id="ocp11a45"
External Traffic

Route Processing

TLS
```

---

# 245. OpenShift Disaster Recovery

Essential enterprise topic.

---

## Recovery Areas

```text id="ocp11a46"
etcd

Applications

Storage

Cluster Configurations
```

---

## Strategy

```text id="ocp11a47"
Backup

Replication

Recovery Testing
```

---

# 246. Backup Strategy

---

## What To Backup

```text id="ocp11a48"
etcd

PV Data

Cluster Configurations

GitOps Repositories
```

---

## Architecture

```text id="ocp11a49"
Cluster
   │
   ▼
Backup Storage
```

---

# 247. Must-Gather

Most important troubleshooting tool.

---

## Purpose

Collect diagnostic information.

---

## Command

```bash id="ocp11a50"
oc adm must-gather
```

---

## Workflow

```text id="ocp11a51"
Cluster
   │
   ▼
Must Gather
   │
   ▼
Diagnostic Bundle
```

---

# 248. Cluster Administration Checklist

---

## Daily

```text id="ocp11a52"
Check Operators

Check Nodes

Check Pods

Check Alerts
```

---

## Weekly

```text id="ocp11a53"
Review Capacity

Review Logs

Review Security Events
```

---

## Monthly

```text id="ocp11a54"
Backup Validation

Upgrade Planning

Compliance Review
```

---

# 249. OpenShift Production Best Practices

---

## Infrastructure

```text id="ocp11a55"
Use HA Control Plane

Use Dedicated Infra Nodes

Use Dynamic Storage
```

---

## Security

```text id="ocp11a56"
Use SCC

Use RBAC

Use Network Policies

Rotate Secrets
```

---

## Operations

```text id="ocp11a57"
Monitor Everything

Backup etcd

Test Recovery
```

---

# 250. OpenShift Administration Architecture

```text id="ocp11a58"
Cluster Version Operator
          │
          ▼
Cluster Operators
          │
          ▼
Control Plane
          │
          ▼
Worker Nodes
          │
          ▼
Applications
```

---

# Important Commands

```bash id="ocp11a59"
oc get clusterversion

oc get co

oc get nodes

oc adm cordon NODE

oc adm drain NODE

oc adm uncordon NODE

oc adm must-gather

oc get machinesets -A

oc get machineconfigs

oc get mcp

oc get dns.operator/default

oc get ingresscontroller \
-n openshift-ingress-operator

oc get clusteroperators
```

---

# 251. OpenShift Troubleshooting

Troubleshooting is one of the most important OpenShift administrator skills.

A large portion of the EX280 exam and real-world operations focuses on troubleshooting.

---

## Troubleshooting Methodology

Always follow a structured approach.

```text id="ocp12a1"
Identify

↓

Investigate

↓

Isolate

↓

Fix

↓

Verify
```

---

## Golden Rule

```text id="ocp12a2"
Do Not Guess

Collect Evidence First
```

---

# 252. OpenShift Troubleshooting Flow

```text id="ocp12a3"
User Reports Issue
         │
         ▼
Application
         │
         ▼
Pod
         │
         ▼
Node
         │
         ▼
Storage
         │
         ▼
Networking
         │
         ▼
Cluster Operators
```

---

# 253. First Commands to Run

---

## Cluster Health

```bash id="ocp12a4"
oc get co
```

---

## Nodes

```bash id="ocp12a5"
oc get nodes
```

---

## Pods

```bash id="ocp12a6"
oc get pods -A
```

---

## Events

```bash id="ocp12a7"
oc get events -A
```

---

## Cluster Version

```bash id="ocp12a8"
oc get clusterversion
```

---

# 254. Cluster Operator Troubleshooting

Cluster Operators are critical.

---

## Check Status

```bash id="ocp12a9"
oc get co
```

---

## Healthy Example

```text id="ocp12a10"
Available=True

Progressing=False

Degraded=False
```

---

## Unhealthy Example

```text id="ocp12a11"
Available=False

Degraded=True
```

---

## Investigate

```bash id="ocp12a12"
oc describe co ingress
```

---

# 255. Node Troubleshooting

---

## View Nodes

```bash id="ocp12a13"
oc get nodes
```

---

## Healthy

```text id="ocp12a14"
Ready
```

---

## Common States

```text id="ocp12a15"
NotReady

SchedulingDisabled

Unknown
```

---

## Detailed Information

```bash id="ocp12a16"
oc describe node NODE
```

---

# 256. Node NotReady

Most common node issue.

---

## Causes

```text id="ocp12a17"
kubelet Failure

Network Failure

Disk Pressure

Memory Pressure

CPU Pressure
```

---

## Check

```bash id="ocp12a18"
oc describe node NODE
```

---

## Events

```bash id="ocp12a19"
oc get events
```

---

# 257. Disk Pressure

Node storage exhausted.

---

## Symptoms

```text id="ocp12a20"
Pods Evicted

Image Pull Failures

Node Degraded
```

---

## Verify

```bash id="ocp12a21"
oc describe node NODE
```

---

## Condition

```text id="ocp12a22"
DiskPressure=True
```

---

# 258. Memory Pressure

Node memory exhausted.

---

## Symptoms

```text id="ocp12a23"
OOMKilled

Evictions

Performance Problems
```

---

## Verify

```bash id="ocp12a24"
oc adm top nodes
```

---

# 259. CPU Pressure

CPU saturation.

---

## Verify

```bash id="ocp12a25"
oc adm top nodes
```

---

## Symptoms

```text id="ocp12a26"
Slow Scheduling

Slow Applications
```

---

# 260. Pod Troubleshooting

Start with:

```bash id="ocp12a27"
oc get pods
```

---

## Detailed Information

```bash id="ocp12a28"
oc describe pod POD
```

---

## Logs

```bash id="ocp12a29"
oc logs POD
```

---

# 261. Pod States

---

## Common States

```text id="ocp12a30"
Pending

Running

Completed

Failed

CrashLoopBackOff

ImagePullBackOff
```

---

## Verify

```bash id="ocp12a31"
oc get pods
```

---

# 262. Pending Pods

Pod cannot start.

---

## Common Causes

```text id="ocp12a32"
No Resources

Storage Issues

Node Selector Issues

Taints
```

---

## Investigate

```bash id="ocp12a33"
oc describe pod POD
```

---

# 263. CrashLoopBackOff

One of the most common errors.

---

## Meaning

```text id="ocp12a34"
Container Starts

↓

Crashes

↓

Restarts

↓

Repeats
```

---

## Causes

```text id="ocp12a35"
Bad Configuration

Application Bugs

Missing Dependencies

Database Unavailable
```

---

## Investigation

```bash id="ocp12a36"
oc logs POD

oc logs POD --previous
```

---

# 264. ImagePullBackOff

Container image cannot be downloaded.

---

## Causes

```text id="ocp12a37"
Wrong Image Name

Registry Unavailable

Authentication Failure

Image Missing
```

---

## Verify

```bash id="ocp12a38"
oc describe pod POD
```

---

## Typical Error

```text id="ocp12a39"
Failed To Pull Image
```

---

# 265. ErrImagePull

Image pull failure.

---

## Workflow

```text id="ocp12a40"
Pull Image

↓

Failure

↓

ErrImagePull
```

---

## Check

```bash id="ocp12a41"
oc describe pod POD
```

---

# 266. OOMKilled

Container exceeded memory limit.

---

## Meaning

```text id="ocp12a42"
Out Of Memory
```

---

## Verify

```bash id="ocp12a43"
oc describe pod POD
```

---

## Example

```text id="ocp12a44"
Reason: OOMKilled
```

---

## Solution

```text id="ocp12a45"
Increase Memory Limit

Optimize Application
```

---

# 267. CreateContainerConfigError

Container configuration problem.

---

## Causes

```text id="ocp12a46"
Missing Secret

Missing ConfigMap

Invalid Configuration
```

---

## Investigation

```bash id="ocp12a47"
oc describe pod POD
```

---

# 268. CreateContainerError

Container creation failure.

---

## Causes

```text id="ocp12a48"
Permission Issues

Invalid Commands

Volume Problems
```

---

## Check

```bash id="ocp12a49"
oc describe pod POD
```

---

# 269. Init Container Failures

---

## Verify

```bash id="ocp12a50"
oc describe pod POD
```

---

## Logs

```bash id="ocp12a51"
oc logs POD -c INIT_CONTAINER
```

---

## Common Causes

```text id="ocp12a52"
Database Unreachable

Bad Script

Configuration Errors
```

---

# 270. Container Cannot Start

---

## Causes

```text id="ocp12a53"
Wrong Command

Wrong Entrypoint

Permission Denied
```

---

## Investigation

```bash id="ocp12a54"
oc logs POD
```

---

# 271. Networking Troubleshooting

---

## Verify Services

```bash id="ocp12a55"
oc get svc
```

---

## Verify Endpoints

```bash id="ocp12a56"
oc get endpoints
```

---

## Verify Routes

```bash id="ocp12a57"
oc get routes
```

---

# 272. Service Not Working

Most common causes.

---

## Causes

```text id="ocp12a58"
Label Mismatch

No Endpoints

Wrong Port
```

---

## Verify

```bash id="ocp12a59"
oc describe svc SERVICE
```

---

# 273. Route Not Accessible

---

## Causes

```text id="ocp12a60"
DNS Failure

TLS Failure

Router Problems

Service Problems
```

---

## Verify

```bash id="ocp12a61"
oc describe route
```

---

# 274. DNS Problems

---

## Symptoms

```text id="ocp12a62"
Cannot Resolve Service Names
```

---

## Verify

```bash id="ocp12a63"
oc get dns.operator/default
```

---

## Test

```bash id="ocp12a64"
oc exec POD -- nslookup SERVICE
```

---

# 275. Storage Troubleshooting

---

## PVC Pending

Most common storage issue.

---

## Causes

```text id="ocp12a65"
No StorageClass

Provisioner Failure

Capacity Issues
```

---

## Check

```bash id="ocp12a66"
oc describe pvc PVC
```

---

# 276. PV Not Bound

---

## Causes

```text id="ocp12a67"
Size Mismatch

Access Mode Mismatch
```

---

## Verify

```bash id="ocp12a68"
oc get pv
```

---

# 277. Volume Mount Failure

---

## Causes

```text id="ocp12a69"
Permissions

Node Issues

Storage Failure
```

---

## Verify

```bash id="ocp12a70"
oc describe pod POD
```

---

# 278. Authentication Problems

---

## Verify User

```bash id="ocp12a71"
oc whoami
```

---

## Verify Access

```bash id="ocp12a72"
oc auth can-i create pods
```

---

## Common Causes

```text id="ocp12a73"
RBAC

Expired Credentials

OAuth Issues
```

---

# 279. SCC Problems

Very common OpenShift-specific issue.

---

## Symptoms

```text id="ocp12a74"
Pod Rejected

Permission Denied
```

---

## Example

```text id="ocp12a75"
Unable To Validate Against SCC
```

---

## Verify

```bash id="ocp12a76"
oc describe pod POD
```

---

# 280. OpenShift Troubleshooting Workflow

```text id="ocp12a77"
Cluster Operators
        │
        ▼
Nodes
        │
        ▼
Pods
        │
        ▼
Containers
        │
        ▼
Networking
        │
        ▼
Storage
        │
        ▼
Security
```

---

# Important Commands

```bash id="ocp12a78"
oc get co

oc describe co

oc get nodes

oc describe node NODE

oc adm top nodes

oc get pods -A

oc describe pod POD

oc logs POD

oc logs POD --previous

oc get svc

oc get endpoints

oc get routes

oc get pvc

oc describe pvc

oc auth can-i

oc whoami

oc get events -A

oc adm must-gather
```

---

# 281. OpenShift Certification Guide

OpenShift certifications are provided by [Red Hat Training & Certification](https://www.redhat.com/en/services/training-and-certification).

These certifications are highly respected in enterprise environments.

---

# 282. OpenShift Certification Roadmap

```text id="ocp13a1"
Linux Fundamentals
       │
       ▼
RHCSA
       │
       ▼
DO180
       │
       ▼
DO280 + EX280
       │
       ▼
EX288
       │
       ▼
EX380
       │
       ▼
OpenShift Architect
```

---

# 283. DO180

## Red Hat OpenShift Administration I

Entry-level OpenShift course.

---

## Covers

```text id="ocp13a2"
Containers

Pods

Deployments

Services

Routes

Storage

Projects
```

---

## Target Audience

```text id="ocp13a3"
Developers

Junior DevOps Engineers

System Administrators
```

---

## Difficulty

```text id="ocp13a4"
Beginner
```

---

# 284. DO280

## Red Hat OpenShift Administration II

Advanced OpenShift administration.

---

## Covers

```text id="ocp13a5"
Cluster Administration

Operators

Storage

Networking

Security

Troubleshooting
```

---

## Difficulty

```text id="ocp13a6"
Intermediate
```

---

## Recommended Before

```text id="ocp13a7"
EX280
```

---

# 285. EX280

## Red Hat Certified Specialist in OpenShift Administration

Most popular OpenShift certification.

---

## Exam Type

```text id="ocp13a8"
Hands-On Lab

Performance Based
```

---

## No MCQs

You perform real administration tasks.

---

## Duration

```text id="ocp13a9"
Approximately 4 Hours
```

---

# 286. EX280 Domains

---

## User Management

```text id="ocp13a10"
Users

Groups

Authentication

RBAC
```

---

## Workloads

```text id="ocp13a11"
Pods

Deployments

DeploymentConfigs

Scaling
```

---

## Networking

```text id="ocp13a12"
Services

Routes

DNS
```

---

## Storage

```text id="ocp13a13"
PV

PVC

StorageClasses
```

---

## Security

```text id="ocp13a14"
SCC

Secrets

RBAC
```

---

## Troubleshooting

```text id="ocp13a15"
Pods

Nodes

Storage

Networking
```

---

# 287. EX280 Exam Strategy

---

## Step 1

Read all questions first.

---

## Step 2

```text id="ocp13a16"
Complete Easy Tasks First
```

---

## Step 3

```text id="ocp13a17"
Save Troubleshooting Tasks For Later
```

---

## Step 4

```text id="ocp13a18"
Verify Everything
```

---

# 288. EX280 Most Important Commands

```bash id="ocp13a19"
oc get all

oc describe

oc logs

oc get events

oc get co

oc get nodes

oc adm top nodes

oc auth can-i
```

---

# 289. EX280 Lab Topics

---

## Must Know

```text id="ocp13a20"
Projects

Users

Groups

RBAC

SCC

Storage

Routes

Deployments
```

---

## Practice Daily

```text id="ocp13a21"
Troubleshooting
```

---

# 290. EX288

## Red Hat Certified Specialist in OpenShift Application Development

Developer-focused certification.

---

## Covers

```text id="ocp13a22"
BuildConfig

S2I

ImageStreams

CI/CD

GitOps

Pipelines
```

---

## Target Audience

```text id="ocp13a23"
Developers

DevOps Engineers
```

---

## Difficulty

```text id="ocp13a24"
Intermediate
```

---

# 291. EX380

## Red Hat Certified Specialist in OpenShift Automation and Integration

Advanced automation certification.

---

## Covers

```text id="ocp13a25"
Ansible

Automation

OpenShift

Infrastructure Automation
```

---

## Difficulty

```text id="ocp13a26"
Advanced
```

---

# 292. OpenShift Interview Questions (Beginner)

### What is OpenShift?

```text id="ocp13a27"
Enterprise Kubernetes Platform
```

---

### OpenShift vs Kubernetes?

```text id="ocp13a28"
OpenShift

=

Kubernetes

+

Enterprise Features
```

---

### What is a Project?

```text id="ocp13a29"
OpenShift Namespace
```

---

### What is a Route?

```text id="ocp13a30"
OpenShift Ingress Equivalent
```

---

### What is SCC?

```text id="ocp13a31"
Security Context Constraints
```

---

# 293. Intermediate Interview Questions

### Deployment vs DeploymentConfig?

| Deployment  | DeploymentConfig      |
| ----------- | --------------------- |
| Modern      | Legacy                |
| ReplicaSet  | ReplicationController |
| Recommended | Older                 |

---

### What is ImageStream?

Tracks container images.

---

### What is BuildConfig?

Defines image build process.

---

### What is S2I?

```text id="ocp13a32"
Source To Image
```

Builds images directly from source code.

---

### What is OLM?

```text id="ocp13a33"
Operator Lifecycle Manager
```

---

# 294. Advanced Interview Questions

### What is Machine API?

Manages node lifecycle.

---

### What is MCO?

```text id="ocp13a34"
Machine Config Operator
```

---

### What is ACM?

```text id="ocp13a35"
Advanced Cluster Management
```

---

### What is KubeVirt?

Technology behind OpenShift Virtualization.

---

### What is Service Mesh?

Infrastructure layer for service communication.

---

# 295. OpenShift Administrator Daily Checklist

---

## Morning Checks

```bash id="ocp13a36"
oc get co

oc get nodes

oc get clusterversion
```

---

## Workload Checks

```bash id="ocp13a37"
oc get pods -A

oc get events -A
```

---

## Storage Checks

```bash id="ocp13a38"
oc get pvc -A

oc get storageclass
```

---

## Networking Checks

```bash id="ocp13a39"
oc get routes -A

oc get svc -A
```

---

# 296. OpenShift Administrator Skill Matrix

| Skill           | Importance |
| --------------- | ---------- |
| Pods            | High       |
| Deployments     | High       |
| Routes          | High       |
| Storage         | High       |
| SCC             | Very High  |
| Operators       | Very High  |
| Troubleshooting | Critical   |
| Upgrades        | Critical   |
| Networking      | Critical   |

---

# 297. OpenShift Learning Roadmap

```text id="ocp13a40"
OpenShift Fundamentals
          │
          ▼
Projects
          │
          ▼
Pods
          │
          ▼
Deployments
          │
          ▼
Services
          │
          ▼
Routes
          │
          ▼
Storage
          │
          ▼
Security
          │
          ▼
Operators
          │
          ▼
Monitoring
          │
          ▼
Troubleshooting
          │
          ▼
Administration
```

---

# 298. OpenShift Career Paths

```text id="ocp13a41"
OpenShift Administrator

DevOps Engineer

Platform Engineer

Site Reliability Engineer

Cloud Engineer

Infrastructure Engineer

Cloud Architect
```

---

# 299. OpenShift Mastery Checklist

```text id="ocp13a42"
✓ Projects

✓ Pods

✓ Deployments

✓ DeploymentConfigs

✓ Services

✓ Routes

✓ Storage

✓ Security

✓ SCC

✓ Operators

✓ ImageStreams

✓ BuildConfigs

✓ Monitoring

✓ Logging

✓ Virtualization

✓ Service Mesh

✓ ACM

✓ Cluster Administration

✓ Troubleshooting
```

---

# 300. OpenShift Enterprise Ecosystem Summary

```text id="ocp13a43"
OpenShift
    │
    ├── Kubernetes
    ├── Operators
    ├── Registry
    ├── Build System
    ├── GitOps
    ├── Monitoring
    ├── Logging
    ├── Virtualization
    ├── Service Mesh
    ├── ACM
    └── Enterprise Security
```

---

# Important Commands

```bash id="ocp13a44"
oc get all

oc get co

oc get nodes

oc get projects

oc get routes

oc get svc

oc get pvc

oc get storageclass

oc get imagestreams

oc get bc

oc get csv

oc get subscriptions

oc adm top nodes

oc adm must-gather

oc auth can-i

oc whoami
```

---

# 301. OpenShift `oc` Command Encyclopedia

The `oc` command is the primary CLI tool used to manage OpenShift clusters.

Think of it as:

```text
kubectl
   +
OpenShift Features
   =
oc
```

---

# 302. Login and Authentication Commands

## Login

```bash
oc login https://api.cluster.example.com:6443
```

---

## Login Using Token

```bash
oc login \
--token=<TOKEN> \
--server=https://api.cluster.example.com:6443
```

---

## Logout

```bash
oc logout
```

---

## Current User

```bash
oc whoami
```

---

## Current Token

```bash
oc whoami -t
```

---

## Current Server

```bash
oc whoami --show-server
```

---

# 303. Cluster Information Commands

## Cluster Information

```bash
oc cluster-info
```

---

## Version

```bash
oc version
```

---

## API Resources

```bash
oc api-resources
```

---

## API Versions

```bash
oc api-versions
```

---

## Explain Resource

```bash
oc explain pod

oc explain deployment

oc explain route
```

---

# 304. Project Commands

Projects are OpenShift namespaces.

---

## List Projects

```bash
oc get projects
```

---

## Create Project

```bash
oc new-project dev
```

---

## Switch Project

```bash
oc project dev
```

---

## Delete Project

```bash
oc delete project dev
```

---

## Current Project

```bash
oc project
```

---

# 305. Resource Discovery Commands

## Get Everything

```bash
oc get all
```

---

## Wide Output

```bash
oc get all -o wide
```

---

## YAML Output

```bash
oc get deployment nginx -o yaml
```

---

## JSON Output

```bash
oc get deployment nginx -o json
```

---

## Watch Resources

```bash
oc get pods -w
```

---

# 306. Pod Commands

## List Pods

```bash
oc get pods
```

---

## All Namespaces

```bash
oc get pods -A
```

---

## Wide View

```bash
oc get pods -o wide
```

---

## Describe Pod

```bash
oc describe pod POD_NAME
```

---

## Delete Pod

```bash
oc delete pod POD_NAME
```

---

## Force Delete

```bash
oc delete pod POD_NAME \
--force \
--grace-period=0
```

---

# 307. Pod Log Commands

## View Logs

```bash
oc logs POD_NAME
```

---

## Follow Logs

```bash
oc logs -f POD_NAME
```

---

## Previous Logs

```bash
oc logs POD_NAME --previous
```

---

## Specific Container

```bash
oc logs POD_NAME -c CONTAINER
```

---

# 308. Pod Access Commands

## Execute Shell

```bash
oc exec -it POD_NAME -- sh
```

---

## Bash

```bash
oc exec -it POD_NAME -- bash
```

---

## Run Command

```bash
oc exec POD_NAME -- ls /tmp
```

---

## Copy Files

```bash
oc cp file.txt POD:/tmp
```

---

# 309. Deployment Commands

## List Deployments

```bash
oc get deployments
```

---

## Describe Deployment

```bash
oc describe deployment nginx
```

---

## Scale Deployment

```bash
oc scale deployment nginx \
--replicas=5
```

---

## Delete Deployment

```bash
oc delete deployment nginx
```

---

# 310. Rollout Commands

## Status

```bash
oc rollout status deployment nginx
```

---

## History

```bash
oc rollout history deployment nginx
```

---

## Undo

```bash
oc rollout undo deployment nginx
```

---

## Restart

```bash
oc rollout restart deployment nginx
```

---

# 311. DeploymentConfig Commands

## View DeploymentConfigs

```bash
oc get dc
```

---

## Describe

```bash
oc describe dc myapp
```

---

## Latest Rollout

```bash
oc rollout latest dc/myapp
```

---

## Rollout Status

```bash
oc rollout status dc/myapp
```

---

## Rollout History

```bash
oc rollout history dc/myapp
```

---

# 312. Service Commands

## List Services

```bash
oc get svc
```

---

## Describe Service

```bash
oc describe svc nginx
```

---

## Expose Deployment

```bash
oc expose deployment nginx \
--port=80
```

---

## Delete Service

```bash
oc delete svc nginx
```

---

# 313. Route Commands

## List Routes

```bash
oc get routes
```

---

## Describe Route

```bash
oc describe route nginx
```

---

## Create Route

```bash
oc expose svc nginx
```

---

## Delete Route

```bash
oc delete route nginx
```

---

## Route YAML

```bash
oc get route nginx -o yaml
```

---

# 314. ReplicaSet Commands

## View ReplicaSets

```bash
oc get rs
```

---

## Describe ReplicaSet

```bash
oc describe rs
```

---

## Delete ReplicaSet

```bash
oc delete rs NAME
```

---

# 315. ReplicationController Commands

## View RC

```bash
oc get rc
```

---

## Describe RC

```bash
oc describe rc
```

---

## Delete RC

```bash
oc delete rc NAME
```

---

# 316. Namespace and Label Commands

## Show Labels

```bash
oc get pods --show-labels
```

---

## Add Label

```bash
oc label pod POD_NAME app=nginx
```

---

## Remove Label

```bash
oc label pod POD_NAME app-
```

---

## Filter By Label

```bash
oc get pods -l app=nginx
```

---

# 317. Annotation Commands

## Add Annotation

```bash
oc annotate pod POD_NAME \
owner=devops
```

---

## Remove Annotation

```bash
oc annotate pod POD_NAME \
owner-
```

---

## View Annotations

```bash
oc describe pod POD_NAME
```

---

# 318. Resource Editing Commands

## Edit Resource

```bash
oc edit deployment nginx
```

---

## Replace Resource

```bash
oc replace -f deployment.yaml
```

---

## Apply Resource

```bash
oc apply -f deployment.yaml
```

---

## Delete Resource File

```bash
oc delete -f deployment.yaml
```

---

# 319. Generate Resource YAML

## Dry Run

```bash
oc create deployment nginx \
--image=nginx \
--dry-run=client \
-o yaml
```

---

## Export Deployment

```bash
oc get deployment nginx -o yaml
```

---

## Save To File

```bash
oc get deployment nginx -o yaml \
> deployment.yaml
```

---

# 320. OpenShift Quick Command Categories

```text
Authentication
-------------
oc login
oc logout
oc whoami

Projects
--------
oc project
oc new-project
oc get projects

Pods
----
oc get pods
oc logs
oc exec
oc describe

Deployments
-----------
oc get deployment
oc scale
oc rollout

Networking
----------
oc get svc
oc get routes
oc expose

Administration
--------------
oc get co
oc get nodes
oc adm top nodes
oc adm must-gather

Troubleshooting
---------------
oc logs
oc describe
oc get events
oc adm inspect
```

---

# 321. Advanced `oc` Commands

These commands are frequently used by OpenShift administrators and EX280 candidates.

---

## Run Temporary Pod

```bash
oc run nginx \
--image=nginx
```

---

## Interactive Pod

```bash
oc run test \
--image=busybox \
-it --rm \
-- sh
```

---

## Port Forward

```bash
oc port-forward pod/nginx \
8080:80
```

---

## Proxy API Server

```bash
oc proxy
```

---

## API Raw Access

```bash
oc get --raw /healthz
```

---

## Resource Usage

```bash
oc adm top nodes

oc adm top pods
```

---

## Resource Utilization

```bash
oc adm top pods -A
```

---

## Events

```bash
oc get events

oc get events -A

oc get events \
--sort-by=.metadata.creationTimestamp
```

---

## Inspect Resources

```bash
oc adm inspect ns/dev
```

---

## Must Gather

```bash
oc adm must-gather
```

---

## Debug Node

```bash
oc debug node/NODE_NAME
```

---

## Debug Pod

```bash
oc debug pod/POD_NAME
```

---

## Node Shell

```bash
chroot /host
```

inside node debug session.

---

# 322. BuildConfig Commands

BuildConfig is OpenShift's native build mechanism.

---

## View BuildConfigs

```bash
oc get bc
```

---

## Describe BuildConfig

```bash
oc describe bc
```

---

## Start Build

```bash
oc start-build myapp
```

---

## Follow Build Logs

```bash
oc start-build myapp \
--follow
```

---

## Binary Build

```bash
oc start-build myapp \
--from-dir=.
```

---

## Upload Archive

```bash
oc start-build myapp \
--from-archive=app.zip
```

---

## Upload File

```bash
oc start-build myapp \
--from-file=index.html
```

---

## Build Logs

```bash
oc logs build/myapp-1
```

---

## View Builds

```bash
oc get builds
```

---

## Describe Build

```bash
oc describe build myapp-1
```

---

## Delete Build

```bash
oc delete build myapp-1
```

---

# 323. ImageStream Commands

One of the most important OpenShift-specific features.

---

## View ImageStreams

```bash
oc get is
```

---

## Describe ImageStream

```bash
oc describe is nginx
```

---

## Create ImageStream

```bash
oc create imagestream nginx
```

---

## Import External Image

```bash
oc import-image nginx \
--from=docker.io/library/nginx
```

---

## Tag Images

```bash
oc tag nginx:latest nginx:v1
```

---

## View Image Tags

```bash
oc describe is nginx
```

---

## Delete ImageStream

```bash
oc delete is nginx
```

---

## View Images

```bash
oc get images
```

---

## Describe Image

```bash
oc describe image IMAGE_ID
```

---

# 324. Operator Commands

Essential for OpenShift administration.

---

## View Operators

```bash
oc get co
```

---

## Describe Operator

```bash
oc describe co ingress
```

---

## Operator Status

```bash
oc get co
```

---

## Healthy Operator

```text
Available=True

Progressing=False

Degraded=False
```

---

## View CSVs

```bash
oc get csv -A
```

---

## Describe CSV

```bash
oc describe csv
```

---

## View Subscriptions

```bash
oc get subscriptions -A
```

---

## View Install Plans

```bash
oc get installplans -A
```

---

## View Catalog Sources

```bash
oc get catalogsource \
-n openshift-marketplace
```

---

# 325. MachineConfig Commands

MachineConfig manages node configuration.

---

## View MachineConfigs

```bash
oc get machineconfigs
```

---

## Describe MachineConfig

```bash
oc describe machineconfig
```

---

## View MCP

```bash
oc get mcp
```

---

## Describe MCP

```bash
oc describe mcp worker
```

---

## Check MCP Status

```bash
oc get mcp
```

---

## View Machines

```bash
oc get machines -A
```

---

## View MachineSets

```bash
oc get machinesets -A
```

---

## Describe MachineSet

```bash
oc describe machineset
```

---

# 326. Monitoring Commands

---

## Monitoring Namespace

```bash
oc get pods \
-n openshift-monitoring
```

---

## Prometheus Pods

```bash
oc get pods \
-n openshift-monitoring \
| grep prometheus
```

---

## Alertmanager Pods

```bash
oc get pods \
-n openshift-monitoring \
| grep alertmanager
```

---

## Monitoring Operator

```bash
oc get co monitoring
```

---

## Node Metrics

```bash
oc adm top nodes
```

---

## Pod Metrics

```bash
oc adm top pods
```

---

## Namespace Metrics

```bash
oc adm top pods -n myproject
```

---

# 327. Logging Commands

---

## Logging Namespace

```bash
oc get pods \
-n openshift-logging
```

---

## Logging Operator

```bash
oc get csv | grep logging
```

---

## Collector Pods

```bash
oc get pods \
-n openshift-logging
```

---

## Loki Components

```bash
oc get pods \
-n openshift-logging
```

---

## View Application Logs

```bash
oc logs POD_NAME
```

---

## Follow Logs

```bash
oc logs -f POD_NAME
```

---

## Previous Logs

```bash
oc logs POD_NAME --previous
```

---

# 328. Security Commands

---

## Current User

```bash
oc whoami
```

---

## Check Permissions

```bash
oc auth can-i create pods
```

---

## Check Specific Namespace

```bash
oc auth can-i create pods \
-n dev
```

---

## View SCC

```bash
oc get scc
```

---

## Describe SCC

```bash
oc describe scc restricted
```

---

## Add SCC

```bash
oc adm policy add-scc-to-user \
anyuid \
-z app-sa
```

---

## View Roles

```bash
oc get roles
```

---

## View ClusterRoles

```bash
oc get clusterroles
```

---

## View RoleBindings

```bash
oc get rolebindings
```

---

## View ClusterRoleBindings

```bash
oc get clusterrolebindings
```

---

## View Service Accounts

```bash
oc get sa
```

---

## View Secrets

```bash
oc get secrets
```

---

## Describe Secret

```bash
oc describe secret SECRET
```

---

# 329. OpenShift Administrator Emergency Commands

---

## Check Cluster Health

```bash
oc get co
```

---

## Check Nodes

```bash
oc get nodes
```

---

## Check Critical Namespaces

```bash
oc get pods -A
```

---

## Check Events

```bash
oc get events -A
```

---

## Check Cluster Version

```bash
oc get clusterversion
```

---

## Check Storage

```bash
oc get pvc -A

oc get pv
```

---

## Check Routes

```bash
oc get routes -A
```

---

## Check Machine Config Pools

```bash
oc get mcp
```

---

## Collect Diagnostics

```bash
oc adm must-gather
```

---

# 330. OpenShift Command Reference Architecture

```text
OpenShift Administration
         │
         ├── Authentication
         │      ├── oc login
         │      ├── oc logout
         │      └── oc whoami
         │
         ├── Workloads
         │      ├── oc get pods
         │      ├── oc logs
         │      └── oc rollout
         │
         ├── Networking
         │      ├── oc get svc
         │      ├── oc get routes
         │      └── oc expose
         │
         ├── Storage
         │      ├── oc get pvc
         │      ├── oc get pv
         │      └── oc get storageclass
         │
         ├── Operators
         │      ├── oc get co
         │      ├── oc get csv
         │      └── oc get subscriptions
         │
         ├── Monitoring
         │      ├── oc adm top nodes
         │      ├── oc adm top pods
         │      └── Prometheus
         │
         └── Troubleshooting
                ├── oc describe
                ├── oc logs
                ├── oc debug
                └── oc adm must-gather
```

---

# 331. 100 Advanced OpenShift Interview Questions

## Core OpenShift

### 1. What is OpenShift?

Enterprise Kubernetes platform developed by [Red Hat](https://www.redhat.com).

---

### 2. OpenShift vs Kubernetes?

```text
OpenShift
=
Kubernetes
+
Enterprise Features
```

Additional features:

```text
Routes

SCC

Operators

Registry

Build System

Monitoring

Logging
```

---

### 3. What is a Project?

OpenShift equivalent of a Kubernetes Namespace.

---

### 4. What is a Route?

OpenShift-native way to expose applications externally.

---

### 5. What is SCC?

Security Context Constraints.

Controls pod security policies.

---

### 6. What is OLM?

Operator Lifecycle Manager.

---

### 7. What is a Cluster Operator?

Operator managing OpenShift platform components.

---

### 8. What is ImageStream?

Tracks container image versions and updates.

---

### 9. What is BuildConfig?

Defines how container images are built.

---

### 10. What is S2I?

Source-to-Image.

Builds images directly from source code.

---

## Architecture Questions

### 11. What runs on control plane nodes?

```text
API Server

Scheduler

Controller Manager

etcd
```

---

### 12. What runs on worker nodes?

```text
CRI-O

kubelet

Pods
```

---

### 13. Why is etcd important?

Stores entire cluster state.

---

### 14. What happens if etcd fails?

Cluster becomes unavailable.

---

### 15. Why use 3 masters?

Quorum and High Availability.

---

### 16. What is OVN-Kubernetes?

Default OpenShift networking implementation.

---

### 17. What is an Ingress Controller?

Handles external traffic.

---

### 18. Difference between Service and Route?

```text
Service
=
Internal Access

Route
=
External Access
```

---

### 19. What is Cluster Version Operator?

Manages OpenShift upgrades.

---

### 20. What is Machine API?

Manages node lifecycle.

---

## Security Questions

### 21. SCC vs PSP?

SCC is OpenShift-specific and more powerful.

---

### 22. What is anyuid SCC?

Allows containers to run as arbitrary UIDs.

---

### 23. What is privileged SCC?

Provides elevated permissions.

---

### 24. What is RBAC?

Role-Based Access Control.

---

### 25. Role vs ClusterRole?

```text
Role
=
Namespace

ClusterRole
=
Cluster Wide
```

---

### 26. What is a Service Account?

Identity used by applications.

---

### 27. How do you check permissions?

```bash
oc auth can-i
```

---

### 28. How do you view SCC?

```bash
oc get scc
```

---

### 29. What is OAuth?

Authentication system used by OpenShift.

---

### 30. What are Network Policies?

Control pod-to-pod communication.

---

## Storage Questions

### 31. What is PV?

Persistent Volume.

---

### 32. What is PVC?

Persistent Volume Claim.

---

### 33. What is StorageClass?

Dynamic storage provisioning template.

---

### 34. What is CSI?

Container Storage Interface.

---

### 35. Dynamic vs Static Provisioning?

```text
Static
=
Manual PV

Dynamic
=
Automatic PV
```

---

### 36. Common PVC states?

```text
Pending

Bound

Lost
```

---

### 37. What causes PVC Pending?

```text
No StorageClass

No PV

Provisioner Failure
```

---

### 38. What is ODF?

OpenShift Data Foundation.

---

### 39. What storage backend does ODF use?

Ceph.

---

### 40. Why avoid hostPath?

Not production-safe.

---

## Operators Questions

### 41. What is an Operator?

Application-specific automation controller.

---

### 42. Why Operators?

Automate lifecycle management.

---

### 43. What is CSV?

Cluster Service Version.

---

### 44. What is Subscription?

Tracks Operator updates.

---

### 45. What is CatalogSource?

Operator repository.

---

### 46. What is InstallPlan?

Operator installation workflow.

---

### 47. How to view Operators?

```bash
oc get co
```

---

### 48. How to view CSVs?

```bash
oc get csv -A
```

---

### 49. What is OperatorHub?

Marketplace for Operators.

---

### 50. What are Cluster Operators?

Operators managing OpenShift itself.

---

## Troubleshooting Questions

### 51. What is CrashLoopBackOff?

Container repeatedly crashes.

---

### 52. What causes CrashLoopBackOff?

```text
Bad Config

Bad Application

Dependency Failure
```

---

### 53. What is OOMKilled?

Container exceeded memory limit.

---

### 54. What is ImagePullBackOff?

Image cannot be downloaded.

---

### 55. What is ErrImagePull?

Image pull failed.

---

### 56. What is CreateContainerConfigError?

Configuration issue.

---

### 57. What is CreateContainerError?

Container startup failure.

---

### 58. First command during troubleshooting?

```bash
oc describe pod
```

---

### 59. How to see previous logs?

```bash
oc logs POD --previous
```

---

### 60. What is must-gather?

Diagnostic collection utility.

---

## Monitoring Questions

### 61. What monitoring tool is used?

Prometheus.

---

### 62. What is Alertmanager?

Alert processing component.

---

### 63. What is Thanos?

Long-term metric storage.

---

### 64. What is Grafana?

Visualization platform.

---

### 65. What is PromQL?

Prometheus Query Language.

---

### 66. What is kube-state-metrics?

Kubernetes object metrics exporter.

---

### 67. What is Node Exporter?

Node metrics collector.

---

### 68. What is User Workload Monitoring?

Application monitoring.

---

### 69. What is Loki?

Log storage platform.

---

### 70. What is Vector?

Log collector.

---

## CI/CD Questions

### 71. What is BuildConfig?

Build definition.

---

### 72. What is ImageStream?

Image lifecycle abstraction.

---

### 73. What is S2I?

Source-to-Image.

---

### 74. What is Tekton?

OpenShift Pipelines engine.

---

### 75. What is ArgoCD?

GitOps platform.

---

### 76. What is GitOps?

Git as source of truth.

---

### 77. What is Binary Build?

Upload source directly.

---

### 78. Build Trigger Types?

```text
Manual

Git

Image

Webhook
```

---

### 79. What is PipelineRun?

Pipeline execution instance.

---

### 80. What is Task?

Smallest Tekton unit.

---

## Advanced Questions

### 81. What is Service Mesh?

Service communication layer.

---

### 82. What powers Service Mesh?

Istio.

---

### 83. What is Envoy?

Service mesh proxy.

---

### 84. What is mTLS?

Mutual TLS.

---

### 85. What is Jaeger?

Distributed tracing.

---

### 86. What is Kiali?

Mesh visualization tool.

---

### 87. What is ACM?

Advanced Cluster Management.

---

### 88. What is KubeVirt?

VM platform for Kubernetes.

---

### 89. What is VMI?

VirtualMachineInstance.

---

### 90. What is Live Migration?

Move running VM between nodes.

---

## Administrator Questions

### 91. How to check cluster health?

```bash
oc get co
```

---

### 92. How to check nodes?

```bash
oc get nodes
```

---

### 93. How to check cluster version?

```bash
oc get clusterversion
```

---

### 94. How to cordon node?

```bash
oc adm cordon NODE
```

---

### 95. How to drain node?

```bash
oc adm drain NODE
```

---

### 96. How to uncordon node?

```bash
oc adm uncordon NODE
```

---

### 97. How to check routes?

```bash
oc get routes
```

---

### 98. How to check storage?

```bash
oc get pvc
```

---

### 99. How to collect diagnostics?

```bash
oc adm must-gather
```

---

### 100. Most important OpenShift skill?

```text
Troubleshooting
```

---

# 332. OpenShift Architecture Diagrams

## Complete OpenShift Architecture

```text
                        Users
                          │
                          ▼
                     Load Balancer
                          │
      ┌───────────────────┼───────────────────┐
      │                   │                   │
      ▼                   ▼                   ▼
   Master-1           Master-2           Master-3
      │                   │                   │
      └───────────────┬───┴───────────────────┘
                      │
                      ▼
                    etcd
                      │
       ┌──────────────┼──────────────┐
       │              │              │
       ▼              ▼              ▼
    Worker-1       Worker-2       Worker-3
       │              │              │
       ▼              ▼              ▼
      Pods           Pods           Pods
```

---

## Request Flow

```text
Internet
   │
   ▼
Route
   │
   ▼
Router
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

## Build Flow

```text
Git Repo
    │
    ▼
BuildConfig
    │
    ▼
Build
    │
    ▼
ImageStream
    │
    ▼
Registry
    │
    ▼
Deployment
```

---

## Operator Flow

```text
OperatorHub
      │
      ▼
Subscription
      │
      ▼
InstallPlan
      │
      ▼
CSV
      │
      ▼
Operator
```

---

# 333. OpenShift Production Best Practices

## Security

```text
Use SCC

Use RBAC

Use Network Policies

Use TLS Everywhere

Rotate Secrets
```

---

## Storage

```text
Use Dynamic Provisioning

Use CSI Drivers

Backup PV Data

Monitor Capacity
```

---

## Operations

```text
Monitor Operators

Monitor Nodes

Monitor Storage

Monitor Networking
```

---

## Upgrades

```text
Use Stable Channel

Test First

Backup etcd

Validate Operators
```

---

## Performance

```text
Use Resource Limits

Use Resource Requests

Use HPA

Use Cluster Autoscaling
```

---

# 334. EX280 Exam Preparation Guide

## Most Important Topics

```text
Projects

Pods

Deployments

Routes

Storage

RBAC

SCC

Operators

Troubleshooting
```

---

## Practice Commands Daily

```bash
oc get all

oc describe

oc logs

oc rollout

oc auth can-i

oc get routes

oc get pvc
```

---

## Exam Strategy

```text
Read All Questions

Complete Easy Tasks

Verify Work

Check Routes

Check Storage

Check Pods
```

---

## Biggest Mistake

```text
Not Verifying Changes
```

---

# 335. OpenShift Learning Resources

## Official Training

[Red Hat OpenShift Training](https://www.redhat.com/en/services/training-and-certification/red-hat-openshift-administration-i-do180)

---

## OpenShift Documentation

[OpenShift Documentation](https://docs.redhat.com/en/documentation/openshift_container_platform)

---

## Kubernetes Foundation

[Kubernetes Documentation](https://kubernetes.io/docs/)

---

## Operator Framework

[Operator Framework](https://operatorframework.io/)

---

## KubeVirt

[KubeVirt Documentation](https://kubevirt.io/)

---

## Istio

[Istio Documentation](https://istio.io/latest/docs/)

---

## Argo CD

[Argo CD Documentation](https://argo-cd.readthedocs.io/)

---

## Tekton

[Tekton Documentation](https://tekton.dev/docs/)

---

# 336. Official Documentation

For production usage, always refer to official documentation before implementing changes.

### Primary Sources

* [OpenShift Container Platform Docs](https://docs.redhat.com/en/documentation/openshift_container_platform)
* [Red Hat Knowledgebase](https://access.redhat.com/)
* [Kubernetes Docs](https://kubernetes.io/docs/)
* [Operator Framework Docs](https://operatorframework.io/)

---

# 337. Copyright

```text
Copyright © 2026

This cheatsheet is an original educational compilation
created for learning, certification preparation,
interview preparation, and operational reference.

OpenShift®, Red Hat®, Kubernetes®, Prometheus®,
Grafana®, Istio®, Argo CD®, Tekton®, KubeVirt®,
and related trademarks belong to their respective owners.

All trademarks, product names, company names,
and logos remain the property of their respective owners.
```

---

# 338. Disclaimer & Attribution

```text
This cheatsheet is intended for educational
and informational purposes only.

While every effort has been made to ensure
accuracy, technologies evolve rapidly and
commands, features, and best practices may change.

Always validate production implementations
against official vendor documentation.

This document is not affiliated with,
endorsed by, or sponsored by Red Hat.

OpenShift documentation, concepts,
and command references are based on publicly
available documentation and industry practices.
```

---

# 339. OpenShift Mastery Checklist

```text
FOUNDATIONS
✓ Containers
✓ Kubernetes
✓ OpenShift Architecture
✓ Projects
✓ Pods
✓ Deployments

NETWORKING
✓ Services
✓ Routes
✓ DNS
✓ OVN-Kubernetes
✓ Ingress Controllers

STORAGE
✓ PV
✓ PVC
✓ StorageClass
✓ CSI
✓ ODF

SECURITY
✓ SCC
✓ RBAC
✓ OAuth
✓ Service Accounts
✓ Network Policies

OPERATORS
✓ OLM
✓ CSV
✓ Subscriptions
✓ Cluster Operators

CI/CD
✓ ImageStreams
✓ BuildConfigs
✓ S2I
✓ Pipelines
✓ GitOps

OBSERVABILITY
✓ Monitoring
✓ Prometheus
✓ Alertmanager
✓ Logging
✓ Loki
✓ Grafana

ADVANCED
✓ Service Mesh
✓ ACM
✓ Virtualization
✓ KubeVirt

ADMINISTRATION
✓ Cluster Upgrades
✓ Machine API
✓ Machine Config Operator
✓ Disaster Recovery

TROUBLESHOOTING
✓ Nodes
✓ Pods
✓ Routes
✓ Storage
✓ SCC
✓ Operators
✓ Cluster Health
```

---

# 340. OpenShift Cheatsheet Conclusion

```text
If you understand and can perform:

Projects
Pods
Deployments
Services
Routes
Storage
Security
Operators
Monitoring
Logging
CI/CD
Troubleshooting

you are already operating at a strong
OpenShift Administrator level.

If you can additionally troubleshoot:
CrashLoopBackOff
ImagePullBackOff
PVC Pending
SCC Failures
Operator Degraded States
Node NotReady Issues

you are well prepared for:

DO180
DO280
EX280

and most OpenShift Administrator,
Platform Engineer, DevOps Engineer,
and SRE interview roles.
```

---

```text
OPENSHIFT MASTER CHEATSHEET
Version: 1.0

Coverage:
✓ OpenShift Fundamentals
✓ Architecture
✓ Pods & Deployments
✓ Services & Routes
✓ Storage
✓ Security & SCC
✓ Operators
✓ ImageStreams & BuildConfigs
✓ Monitoring & Logging
✓ Virtualization
✓ Service Mesh
✓ ACM
✓ Cluster Administration
✓ Troubleshooting
✓ EX280 Preparation
✓ Advanced Interview Questions
```