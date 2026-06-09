# Docker Swarm Cheatsheet

![Docker Swarm Logo](https://www.docker.com/wp-content/uploads/2022/03/Moby-logo.png)

---

**Last Updated:** 2026
**Platform:** Docker Swarm
**Version:** Docker Engine 28.x+
**License:** Apache License 2.0

For latest information, visit https://docs.docker.com/engine/swarm/

---

## Table of Contents

* [1. What is Docker Swarm?](#1-what-is-docker-swarm)
* [2. Why Docker Swarm?](#2-why-docker-swarm)
* [3. Swarm Architecture](#3-swarm-architecture)
* [4. Installation & Verification](#4-installation--verification)
* [5. Initialize a Swarm](#5-initialize-a-swarm)
* [6. Manager Nodes](#6-manager-nodes)
* [7. Worker Nodes](#7-worker-nodes)
* [8. Node Management](#8-node-management)
* [9. Services](#9-services)
* [10. Tasks](#10-tasks)
* [11. Replicated Services](#11-replicated-services)
* [12. Global Services](#12-global-services)
* [13. Overlay Networks](#13-overlay-networks)
* [14. Ingress Network](#14-ingress-network)
* [15. Service Discovery](#15-service-discovery)
  
* [Additional Resources](#additional-resources)
* [Quick Reference](#quick-reference)
* [📎 Official Documentation](#-official-documentation)
* [📄 Copyright](#-copyright)
* [📎 Disclaimer & Attribution](#-disclaimer--attribution)
* [🎉 Conclusion](#-conclusion) 

---

# 1. What is Docker Swarm?

Docker Swarm is Docker's native container orchestration platform used to deploy, manage, and scale containerized applications across multiple Docker hosts.

It transforms a group of Docker hosts into a single virtual cluster.

---

## Purpose

```text
Container Orchestration
High Availability
Load Balancing
Scaling
Cluster Management
```

---

## High-Level Architecture

```text
Manager Node
      │
 ┌────┼────┐
 ▼    ▼    ▼
Worker Worker Worker
```

---

## Common Use Cases

```text
Microservices
Web Applications
Internal Platforms
Development Clusters
Production Workloads
```

---

## Benefits

```text
Easy Setup
Docker Native
Lightweight
High Availability
Simple Management
```

---

# 2. Why Docker Swarm?

Managing containers manually becomes difficult as applications grow.

---

## Single Host Problem

```text
Docker Host
     │
     ▼
Many Containers
```

Challenges:

```text
Scaling
Availability
Management
Recovery
```

---

## Swarm Solution

```text
Swarm Cluster
      │
 ┌────┼────┐
 ▼    ▼    ▼
Node Node Node
```

---

## Advantages

```text
Clustering
Service Management
Load Balancing
Fault Tolerance
```

---

## Comparison with Standalone Docker

| Feature        | Docker  | Docker Swarm |
| -------------- | ------- | ------------ |
| Single Host    | Yes     | No           |
| Multi-Host     | No      | Yes          |
| Scaling        | Manual  | Automatic    |
| Load Balancing | Limited | Built-In     |

---

# 3. Swarm Architecture

A Swarm consists of Manager and Worker nodes.

---

## Architecture

```text
Manager Node
      │
 ┌────┼────┐
 ▼    ▼    ▼
Worker Worker Worker
```

---

## Components

```text
Manager Nodes
Worker Nodes
Services
Tasks
Networks
Volumes
```

---

## Workflow

```text
User
 │
 ▼
Manager
 │
 ▼
Workers
 │
 ▼
Containers
```

---

## Benefits

```text
Centralized Control
Scalability
High Availability
```

---

# 4. Installation & Verification

Docker Swarm is included with Docker Engine.

---

## Verify Docker

```bash
docker version
```

---

## Verify Swarm Availability

```bash
docker info
```

Look for:

```text
Swarm: inactive
```

or

```text
Swarm: active
```

---

## Check Engine Status

```bash
systemctl status docker
```

---

## Installation Workflow

```text
Install Docker
      │
      ▼
Start Docker
      │
      ▼
Initialize Swarm
```

---

# 5. Initialize a Swarm

Create a new Swarm cluster.

---

## Initialize Swarm

```bash
docker swarm init
```

---

## Example Output

```text
Swarm initialized
```

---

## Architecture

```text
Docker Host
     │
     ▼
Manager Node
```

---

## Verify

```bash
docker node ls
```

---

## Benefits

```text
Cluster Creation
Central Management
Orchestration
```

---

# 6. Manager Nodes

Manager nodes control the cluster.

---

## Responsibilities

```text
Scheduling
Cluster Management
Service Deployment
State Management
```

---

## Architecture

```text
Manager
   │
   ▼
Worker Nodes
```

---

## View Managers

```bash
docker node ls
```

---

## High Availability

Recommended:

```text
3 Managers
5 Managers
7 Managers
```

Use odd numbers for quorum.

---

## Benefits

```text
Control Plane
Scheduling
Fault Tolerance
```

---

# 7. Worker Nodes

Worker nodes run application containers.

---

## Responsibilities

```text
Run Tasks
Execute Containers
Provide Resources
```

---

## Architecture

```text
Manager
   │
   ▼
Workers
   │
   ▼
Containers
```

---

## Join Worker

Example:

```bash
docker swarm join \
--token TOKEN \
IP:2377
```

---

## Benefits

```text
Workload Execution
Scalability
Resource Capacity
```

---

# 8. Node Management

Nodes can be added, removed, promoted, and demoted.

---

## List Nodes

```bash
docker node ls
```

---

## Inspect Node

```bash
docker node inspect NODE
```

---

## Promote Worker

```bash
docker node promote NODE
```

---

## Demote Manager

```bash
docker node demote NODE
```

---

## Architecture

```text
Worker
  │
  ▼
Promote
  │
  ▼
Manager
```

---

## Benefits

```text
Flexible Management
Cluster Growth
Role Changes
```

---

# 9. Services

Services define the desired state of applications.

---

## Create Service

```bash
docker service create nginx
```

---

## Architecture

```text
Service
   │
   ▼
Tasks
   │
   ▼
Containers
```

---

## Service Responsibilities

```text
Deployment
Scaling
Updates
Recovery
```

---

## Benefits

```text
Declarative Management
Automation
Consistency
```

---

# 10. Tasks

Tasks are individual container instances.

---

## Architecture

```text
Service
   │
   ▼
Task
   │
   ▼
Container
```

---

## View Tasks

```bash
docker service ps SERVICE
```

---

## Lifecycle

```text
Assigned
  │
  ▼
Running
  │
  ▼
Completed
```

---

## Benefits

```text
Scheduling
Tracking
Recovery
```

---

# 11. Replicated Services

Replicated services run multiple instances.

---

## Example

```bash
docker service create \
--replicas 3 nginx
```

---

## Architecture

```text
Service
   │
 ┌─┼─┐
 ▼ ▼ ▼
T1 T2 T3
```

---

## Benefits

```text
High Availability
Load Distribution
Scaling
```

---

## Common Use Cases

```text
Web Servers
APIs
Microservices
```

---

# 12. Global Services

Global services run exactly one task on every node.

---

## Example

```bash
docker service create \
--mode global nginx
```

---

## Architecture

```text
Node1 → Task
Node2 → Task
Node3 → Task
```

---

## Common Use Cases

```text
Monitoring Agents
Logging Agents
Security Agents
```

---

## Benefits

```text
Node Coverage
Automation
Consistency
```

---

# 13. Overlay Networks

Overlay networks connect containers across nodes.

---

## Architecture

```text
Node1
  │
  ▼
Overlay Network
  │
  ▼
Node2
```

---

## Create Network

```bash
docker network create \
-d overlay mynetwork
```

---

## Benefits

```text
Cross-Host Communication
Isolation
Scalability
```

---

## Common Uses

```text
Microservices
Databases
Distributed Applications
```

---

# 14. Ingress Network

The ingress network provides routing mesh functionality.

---

## Purpose

Enable access to services from any node.

---

## Architecture

```text
User Request
      │
      ▼
Ingress Network
      │
 ┌────┼────┐
 ▼    ▼    ▼
Node Node Node
```

---

## Benefits

```text
Load Balancing
High Availability
Routing Mesh
```

---

## Built-In Network

```text
ingress
```

Created automatically by Swarm.

---

# 15. Service Discovery

Services can discover each other automatically.

---

## Architecture

```text
Frontend
    │
    ▼
Backend
    │
    ▼
Database
```

---

## Example

```text
frontend
backend
database
```

Service names act as DNS names.

---

## Benefits

```text
Automatic DNS
Simplified Networking
Easy Communication
```

---

## Workflow

```text
Service Name
      │
      ▼
Internal DNS
      │
      ▼
Target Service
```

---

# 16. Load Balancing

Docker Swarm provides built-in load balancing through the Routing Mesh.

---

## Purpose

Distribute traffic across service replicas automatically.

---

## Architecture

```text
User Request
      │
      ▼
Routing Mesh
      │
 ┌────┼────┐
 ▼    ▼    ▼
Task1 Task2 Task3
```

---

## Benefits

```text
Traffic Distribution
High Availability
Scalability
```

---

## Example

```bash
docker service create \
--replicas 3 \
-p 80:80 nginx
```

---

## Workflow

```text
Request
   │
   ▼
Swarm Load Balancer
   │
   ▼
Available Replica
```

---

# 17. Scaling Services

Services can be scaled horizontally.

---

## Scale Service

```bash
docker service scale web=5
```

---

## Architecture

```text
Service
   │
   ▼
5 Replicas
```

---

## Verify

```bash
docker service ls
```

---

## Benefits

```text
Capacity Growth
Fault Tolerance
Performance
```

---

# 18. Rolling Updates

Swarm updates services gradually.

---

## Example

```bash
docker service update \
--image nginx:latest web
```

---

## Architecture

```text
Old Tasks
    │
    ▼
Rolling Update
    │
    ▼
New Tasks
```

---

## Benefits

```text
Minimal Downtime
Safer Deployments
Controlled Updates
```

---

## Common Options

```bash
--update-delay
--update-parallelism
```

---

# 19. Rollbacks

Rollback returns to a previous version.

---

## Rollback Service

```bash
docker service rollback web
```

---

## Architecture

```text
Version 1
    │
    ▼
Version 2
    │
 Error
    │
    ▼
Rollback
    │
    ▼
Version 1
```

---

## Benefits

```text
Fast Recovery
Reduced Downtime
Safer Releases
```

---

# 20. Volumes & Persistent Storage

Volumes provide persistent data storage.

---

## Problem

Containers are ephemeral.

---

## Solution

```text
Container
     │
     ▼
Volume
     │
     ▼
Persistent Data
```

---

## Example

```bash
docker service create \
--mount source=dbdata,target=/data \
mysql
```

---

## Benefits

```text
Persistence
Backups
Data Protection
```

---

# 21. Secrets Management

Secrets securely store sensitive data.

---

## Examples

```text
Passwords
API Keys
Certificates
Tokens
```

---

## Create Secret

```bash
echo "mypassword" | \
docker secret create db_pass -
```

---

## List Secrets

```bash
docker secret ls
```

---

## Architecture

```text
Secret
  │
  ▼
Service
  │
  ▼
Container
```

---

## Benefits

```text
Security
Compliance
Central Management
```

---

# 22. Configs

Configs provide non-sensitive configuration data.

---

## Create Config

```bash
docker config create \
nginx_conf nginx.conf
```

---

## Architecture

```text
Config
  │
  ▼
Service
  │
  ▼
Container
```

---

## Common Uses

```text
Application Configs
Nginx Configs
Environment Settings
```

---

## Benefits

```text
Configuration Management
Consistency
Flexibility
```

---

# 23. Health Checks

Health checks monitor service health.

---

## Example

```dockerfile
HEALTHCHECK CMD curl -f http://localhost
```

---

## Architecture

```text
Container
    │
    ▼
Health Check
    │
 ┌──┴──┐
 ▼     ▼
Healthy Unhealthy
```

---

## Benefits

```text
Reliability
Automation
Monitoring
```

---

# 24. Placement Constraints

Control where services run.

---

## Example

```bash
docker service create \
--constraint node.role==worker \
nginx
```

---

## Architecture

```text
Constraint
    │
    ▼
Scheduler
    │
    ▼
Selected Node
```

---

## Common Constraints

```text
Node Role
Node Labels
Regions
Availability Zones
```

---

## Benefits

```text
Control
Compliance
Optimization
```

---

# 25. Resource Limits

Limit CPU and memory usage.

---

## Example

```bash
docker service create \
--limit-memory 512M \
nginx
```

---

## Architecture

```text
Resources
    │
    ▼
Limits
    │
    ▼
Container
```

---

## Benefits

```text
Stability
Fair Usage
Cost Control
```

---

# 26. Monitoring & Logging

Monitoring provides visibility into the cluster.

---

## What to Monitor

```text
CPU
Memory
Network
Tasks
Services
```

---

## Logging Architecture

```text
Containers
     │
     ▼
Logs
     │
     ▼
Monitoring Stack
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

## Benefits

```text
Observability
Performance Analysis
Troubleshooting
```

---

# 27. Troubleshooting

---

## View Services

```bash
docker service ls
```

---

## View Tasks

```bash
docker service ps SERVICE
```

---

## Inspect Node

```bash
docker node inspect NODE
```

---

## View Logs

```bash
docker service logs SERVICE
```

---

## Common Issues

```text
Node Failures
Network Problems
Task Scheduling Issues
Image Pull Failures
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
Fix
```

---

# 28. Common Commands

## Initialize Swarm

```bash
docker swarm init
```

---

## Join Cluster

```bash
docker swarm join
```

---

## View Nodes

```bash
docker node ls
```

---

## Create Service

```bash
docker service create nginx
```

---

## List Services

```bash
docker service ls
```

---

## Service Tasks

```bash
docker service ps SERVICE
```

---

## Scale Service

```bash
docker service scale web=5
```

---

## Update Service

```bash
docker service update SERVICE
```

---

## Rollback Service

```bash
docker service rollback SERVICE
```

---

# 29. Real-World Architectures

## Web Application

```text
User
 │
 ▼
Load Balancer
 │
 ▼
Web Service
 │
 ▼
Database
```

---

## Microservices Platform

```text
Frontend
    │
 ┌──┼──┐
 ▼  ▼  ▼
API1 API2 API3
    │
    ▼
Database
```

---

## Monitoring Stack

```text
Prometheus
     │
     ▼
Grafana
```

---

## Logging Stack

```text
Elasticsearch
      │
      ▼
Logstash
      │
      ▼
Kibana
```

---

# 30. Swarm vs Kubernetes

| Feature             | Docker Swarm | Kubernetes |
| ------------------- | ------------ | ---------- |
| Setup               | Easy         | Complex    |
| Learning Curve      | Low          | High       |
| Scaling             | Good         | Excellent  |
| Ecosystem           | Smaller      | Huge       |
| Enterprise Features | Basic        | Advanced   |

---

## Architecture Comparison

```text
Docker Swarm
     │
     ▼
Single Cluster
```

---

```text
Kubernetes
     │
     ▼
Large Distributed Platform
```

---

## Common Recommendation

```text
Docker Swarm
  → Small Teams
  → Learning
  → Simpler Deployments

Kubernetes
  → Large Scale
  → Enterprise
  → Cloud Native Platforms
```

---

# 31. Production Best Practices

Docker Swarm can run production workloads when designed properly.

---

## Best Practices

```text id="4x8d1v"
Use Multiple Managers
Enable Monitoring
Use Secrets
Use Overlay Networks
Backup Swarm State
```

---

## Recommended Cluster

```text id="5m1c8n"
Manager
   │
 ┌─┼─┐
 ▼ ▼ ▼
Worker Worker Worker
```

---

## Security Recommendations

```text id="7v2k4m"
Least Privilege
Private Registries
Encrypted Secrets
Regular Updates
```

---

## Benefits

```text id="9r6w1p"
Reliability
Scalability
Security
```

---

# 32. CI/CD Integration

Docker Swarm integrates easily with CI/CD pipelines.

---

## Pipeline Architecture

```text id="3n7v8k"
Developer
    │
    ▼
Git Repository
    │
    ▼
CI/CD Pipeline
    │
    ▼
Docker Swarm
```

---

## Common Workflow

```text id="6p4m2w"
Build Image
     │
     ▼
Push Registry
     │
     ▼
Deploy Service
```

---

## Example Deployment

```bash id="1v9r5m"
docker service update \
--image myapp:v2 app
```

---

## Common Tools

```text id="2k8w7n"
GitHub Actions
GitLab CI
Jenkins
Azure DevOps
```

---

# 33. Security Best Practices

Security should be implemented throughout the cluster.

---

## Secure Access

```text id="8m3v1p"
TLS Encryption
Role Separation
Access Control
```

---

## Secrets Usage

Use:

```text id="4r7k2w"
Docker Secrets
```

Avoid:

```text id="9n1m6v"
Hardcoded Passwords
```

---

## Network Security

```text id="7p5w3k"
Overlay Networks
Restricted Ports
Firewall Rules
```

---

## Best Practices

```text id="5m8r1v"
Least Privilege
Regular Patching
Image Scanning
Secure Registries
```

---

# 34. Disaster Recovery

Recovery planning is critical.

---

## Important Assets

```text id="2v6k9m"
Manager Nodes
Secrets
Configs
Volumes
```

---

## Architecture

```text id="8w3n1r"
Cluster State
      │
      ▼
Backup
      │
      ▼
Recovery
```

---

## Recovery Workflow

```text id="4p7m2v"
Failure
   │
   ▼
Restore State
   │
   ▼
Rejoin Nodes
```

---

## Best Practices

```text id="9r4k8w"
Backup Regularly
Document Procedures
Test Recovery
```

---

# 35. Common Use Cases

---

## Web Applications

```text id="6m1p9v"
Frontend
   │
   ▼
Backend
   │
   ▼
Database
```

---

## Microservices

```text id="2w7n4k"
Gateway
  │
  ▼
Services
  │
  ▼
Databases
```

---

## Internal Platforms

```text id="5v8r3m"
Dev Tools
CI/CD
Monitoring
```

---

## Development Labs

```text id="1n6k9w"
Learning
Testing
Proof of Concept
```

---

# 36. Interview Questions

---

## What is Docker Swarm?

Docker Swarm is Docker's native orchestration platform for managing container clusters.

---

## What is a Manager Node?

A manager node controls cluster operations, scheduling, and state management.

---

## What is a Worker Node?

A worker node executes containers and tasks assigned by managers.

---

## What is a Service?

A service defines the desired state of an application.

---

## What is a Task?

A task is a running container instance created by a service.

---

## Difference Between Replicated and Global Services?

| Replicated               | Global              |
| ------------------------ | ------------------- |
| Specific Number of Tasks | One Task Per Node   |
| Scalable                 | Fixed by Node Count |

---

## What is Overlay Networking?

A network that allows containers on different nodes to communicate.

---

## What is Routing Mesh?

Built-in load balancing mechanism that distributes traffic across service replicas.

---

## Difference Between Swarm and Kubernetes?

Swarm focuses on simplicity, while Kubernetes provides advanced orchestration features and a larger ecosystem.

---

# 37. Additional Resources

## Official Resources

* Docker Swarm Documentation
* Docker Engine Documentation
* Docker CLI Reference
* Docker Hub

---

## Learning Resources

* Play With Docker
* Docker Labs
* Docker GitHub
* Docker Community Forums

---

## Related Technologies

```text id="7k2m5w"
Docker
Docker Compose
Kubernetes
containerd
CRI-O
```

---

# 38. Quick Reference

## Initialize Cluster

```bash id="3v7n1m"
docker swarm init
```

---

## Join Cluster

```bash id="8p4w2k"
docker swarm join
```

---

## List Nodes

```bash id="5m9r3v"
docker node ls
```

---

## Create Service

```bash id="2k7w8n"
docker service create nginx
```

---

## List Services

```bash id="9v1m4p"
docker service ls
```

---

## View Tasks

```bash id="4r8k6w"
docker service ps SERVICE
```

---

## Scale Service

```bash id="7n3m2v"
docker service scale web=5
```

---

## Update Service

```bash id="1w9k5m"
docker service update SERVICE
```

---

## Rollback Service

```bash id="6v2r8n"
docker service rollback SERVICE
```

---

# 📎 Official Documentation

* Docker Swarm Documentation: [https://docs.docker.com/engine/swarm/](https://docs.docker.com/engine/swarm/)
* Docker Engine Documentation: [https://docs.docker.com/engine/](https://docs.docker.com/engine/)
* Docker CLI Documentation: [https://docs.docker.com/reference/cli/](https://docs.docker.com/reference/cli/)
* Docker Hub: [https://hub.docker.com/](https://hub.docker.com/)

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, DevOps engineering, platform engineering, and container orchestration.

Primary references include:

* Docker Swarm Documentation
* Docker Engine Documentation
* Docker CLI Documentation
* Docker Hub

Docker and Docker Swarm are products of Docker, Inc.

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

You now understand:

* Docker Swarm Fundamentals
* Manager Nodes
* Worker Nodes
* Services
* Tasks
* Overlay Networks
* Routing Mesh
* Service Discovery
* Scaling
* Rolling Updates
* Rollbacks
* Secrets
* Configs
* Monitoring
* Troubleshooting
* Disaster Recovery

Docker Swarm provides:

```text id="5n8r2m"
Container Orchestration
+
Built-in Load Balancing
+
Service Discovery
+
Scaling
+
Rolling Updates
+
High Availability
+
Simple Operations
```

and remains a lightweight orchestration platform for teams seeking a simpler alternative to Kubernetes.

---

```md id="4v7k1w"
**Last Updated:** 2026
**Platform:** Docker Swarm
**Version:** Docker Engine 28.x+
**License:** Apache License 2.0

For latest information, visit https://docs.docker.com/engine/swarm/
```