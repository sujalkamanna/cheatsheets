# Docker Compose Cheatsheet

![Docker Compose Logo](https://www.docker.com/wp-content/uploads/2022/03/Moby-logo.png)

---

## Table of Contents

* [1. What is Docker Compose?](#1-what-is-docker-compose)
* [2. Why Docker Compose?](#2-why-docker-compose)
* [3. Docker Compose Architecture](#3-docker-compose-architecture)
* [4. Installation & Verification](#4-installation--verification)
* [5. docker-compose.yml Fundamentals](#5-docker-composeyml-fundamentals)
* [6. Compose File Structure](#6-compose-file-structure)
* [7. Services](#7-services)
* [8. Images](#8-images)
* [9. Containers](#9-containers)
* [10. Ports](#10-ports)
* [11. Environment Variables](#11-environment-variables)
* [12. Volumes](#12-volumes)
* [13. Bind Mounts](#13-bind-mounts)
* [14. Named Volumes](#14-named-volumes)
* [15. Networks](#15-networks)

* [Additional Resources](#additional-resources)
* [Quick Reference](#quick-reference)
* [📎 Official Documentation](#-official-documentation)
* [📄 Copyright](#-copyright)
* [📎 Disclaimer & Attribution](#-disclaimer--attribution)
* [🎉 Conclusion](#-conclusion) 

---

# 1. What is Docker Compose?

Docker Compose is a tool used to define and manage multi-container Docker applications using a YAML configuration file.

Instead of manually starting multiple containers, Docker Compose allows you to manage an entire application stack with a single command.

---

## Purpose

```text
Define Services
Manage Containers
Configure Networks
Manage Volumes
Run Multi-Container Applications
```

---

## Traditional Docker Approach

```text
docker run nginx

docker run mysql

docker run redis
```

Multiple commands required.

---

## Docker Compose Approach

```text
docker-compose.yml
        │
        ▼
docker compose up
```

Entire application starts automatically.

---

## Benefits

```text
Automation
Consistency
Simplified Deployment
Easy Management
```

---

# 2. Why Docker Compose?

Modern applications typically consist of multiple services.

---

## Example Application

```text
Frontend
   │
   ▼
Backend API
   │
   ▼
Database
```

---

## Without Compose

```text
Start Frontend
Start Backend
Start Database
Configure Networking
Configure Storage
```

---

## With Compose

```text
docker compose up
```

Everything starts automatically.

---

## Common Use Cases

```text
Development Environments
Microservices
Testing
Local Kubernetes Alternatives
```

---

## Advantages

```text
Single Configuration
Reproducibility
Easy Collaboration
```

---

# 3. Docker Compose Architecture

Docker Compose sits above Docker Engine.

---

## Architecture

```text
docker-compose.yml
        │
        ▼
Docker Compose
        │
        ▼
Docker Engine
        │
 ┌──────┼──────┐
 ▼      ▼      ▼
Web     DB    Redis
```

---

## Components

```text
Services
Networks
Volumes
Containers
Images
```

---

## Workflow

```text
Configuration
      │
      ▼
Compose
      │
      ▼
Containers
```

---

## Benefits

```text
Centralized Management
Automation
Consistency
```

---

# 4. Installation & Verification

Docker Compose v2 is bundled with modern Docker installations.

---

## Verify Docker

```bash
docker version
```

---

## Verify Compose

```bash
docker compose version
```

---

## Expected Output

```text
Docker Compose version v2.x.x
```

---

## Verify Engine

```bash
docker info
```

---

## Installation Workflow

```text
Install Docker
      │
      ▼
Verify Docker
      │
      ▼
Verify Compose
```

---

# 5. docker-compose.yml Fundamentals

Docker Compose uses YAML configuration files.

---

## Default File

```text
docker-compose.yml
```

---

## Minimal Example

```yaml
services:
  nginx:
    image: nginx
```

---

## Structure

```text
Version
Services
Networks
Volumes
```

---

## Workflow

```text
YAML File
    │
    ▼
Compose
    │
    ▼
Containers
```

---

# 6. Compose File Structure

A Compose file contains application definitions.

---

## Basic Layout

```yaml
services:

volumes:

networks:
```

---

## Architecture

```text
Compose File
     │
 ┌───┼───┐
 ▼   ▼   ▼
Services
Volumes
Networks
```

---

## Main Sections

```text
services
volumes
networks
configs
secrets
```

---

## Benefits

```text
Organization
Readability
Maintainability
```

---

# 7. Services

Services define containers.

---

## Example

```yaml
services:
  web:
    image: nginx
```

---

## Service Architecture

```text
Service
   │
   ▼
Container
```

---

## Common Service Types

```text
Web Server
Database
Cache
API
Worker
```

---

## Benefits

```text
Isolation
Scalability
Organization
```

---

# 8. Images

Services are typically created from images.

---

## Example

```yaml
services:
  nginx:
    image: nginx:latest
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

## Common Images

```text
nginx
redis
mysql
postgres
rabbitmq
```

---

## Benefits

```text
Portability
Consistency
Version Control
```

---

# 9. Containers

Services create containers.

---

## Architecture

```text
Service
   │
   ▼
Container
```

---

## View Running Containers

```bash
docker compose ps
```

---

## Start Containers

```bash
docker compose up
```

---

## Stop Containers

```bash
docker compose down
```

---

## Lifecycle

```text
Create
  │
  ▼
Run
  │
  ▼
Stop
  │
  ▼
Remove
```

---

# 10. Ports

Ports expose container services.

---

## Example

```yaml
ports:
  - "8080:80"
```

---

## Meaning

```text
Host Port:Container Port

8080:80
```

---

## Architecture

```text
User
 │
 ▼
Host Port 8080
 │
 ▼
Container Port 80
```

---

## Common Ports

```text
80
443
3306
5432
6379
```

---

# 11. Environment Variables

Environment variables provide configuration values.

---

## Example

```yaml
environment:
  MYSQL_ROOT_PASSWORD: secret
```

---

## Architecture

```text
Environment Variable
         │
         ▼
Container
```

---

## Common Variables

```text
Passwords
API Keys
Database URLs
Application Settings
```

---

## Benefits

```text
Flexibility
Configuration Management
Portability
```

---

# 12. Volumes

Volumes provide persistent storage.

---

## Problem

Container data disappears when containers are removed.

---

## Solution

```yaml
volumes:
  - db_data:/var/lib/mysql
```

---

## Architecture

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

## Benefits

```text
Persistence
Backup Support
Data Sharing
```

---

# 13. Bind Mounts

Bind mounts connect host directories to containers.

---

## Example

```yaml
volumes:
  - ./app:/app
```

---

## Architecture

```text
Host Directory
       │
       ▼
Container Directory
```

---

## Common Uses

```text
Development
Configuration Files
Source Code
Logs
```

---

## Benefits

```text
Real-Time Updates
Easy Development
Host Visibility
```

---

# 14. Named Volumes

Named volumes are managed by Docker.

---

## Example

```yaml
volumes:
  db_data:
```

---

## Usage

```yaml
services:
  mysql:
    volumes:
      - db_data:/var/lib/mysql
```

---

## Architecture

```text
Docker Volume
      │
      ▼
Container
```

---

## Benefits

```text
Persistence
Portability
Management Simplicity
```

---

# 15. Networks

Docker Compose automatically creates networks.

---

## Default Network

```text
project_default
```

---

## Architecture

```text
Web
 │
 ▼
Compose Network
 │
 ▼
Database
```

---

## Example

```yaml
networks:
  backend:
```

---

## Benefits

```text
Service Discovery
Isolation
Secure Communication
```

---

## Service Communication

```text
web
 │
 ▼
database
```

Services can communicate using service names as hostnames.

---

# 16. Multi-Container Applications

Docker Compose is designed for applications consisting of multiple services.

---

## Typical Architecture

```text
Frontend
    │
    ▼
Backend API
    │
    ▼
Database
```

---

## Compose Architecture

```text
docker-compose.yml
        │
        ▼
Docker Compose
        │
 ┌──────┼──────┐
 ▼      ▼      ▼
Web     API    DB
```

---

## Example

```yaml
services:
  frontend:
    image: nginx

  backend:
    image: node

  database:
    image: postgres
```

---

## Benefits

```text
Centralized Configuration
Easy Deployment
Service Isolation
```

---

# 17. Service Dependencies

Services often depend on other services.

---

## Example

```yaml
services:
  web:
    depends_on:
      - database
```

---

## Architecture

```text
Web
 │
 ▼
Database
```

---

## Purpose

Ensure services start in the correct order.

---

## Benefits

```text
Predictable Startup
Reduced Errors
Better Reliability
```

---

# 18. Restart Policies

Restart policies control container recovery behavior.

---

## Common Policies

```yaml
restart: no
restart: always
restart: on-failure
restart: unless-stopped
```

---

## Example

```yaml
services:
  nginx:
    restart: always
```

---

## Workflow

```text
Container Crash
        │
        ▼
Restart Policy
        │
        ▼
Container Recovery
```

---

## Benefits

```text
Availability
Resilience
Automation
```

---

# 19. Resource Limits

Limit CPU and memory usage.

---

## Purpose

Prevent containers from consuming excessive resources.

---

## Example

```yaml
services:
  app:
    deploy:
      resources:
        limits:
          memory: 512M
```

---

## Architecture

```text
Host Resources
       │
       ▼
Resource Limits
       │
       ▼
Container
```

---

## Benefits

```text
Stability
Fair Resource Usage
Cost Control
```

---

# 20. Health Checks

Health checks determine whether a container is healthy.

---

## Example

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost"]
```

---

## Workflow

```text
Container
    │
    ▼
Health Check
    │
 ┌──┴──┐
 ▼     ▼
Pass  Fail
```

---

## Benefits

```text
Reliability
Monitoring
Self-Healing
```

---

# 21. Build Configuration

Compose can build images directly.

---

## Example

```yaml
services:
  app:
    build: .
```

---

## Architecture

```text
Source Code
      │
      ▼
Dockerfile
      │
      ▼
Image
      │
      ▼
Container
```

---

## Build Command

```bash
docker compose build
```

---

## Benefits

```text
Automation
Consistency
Simplified Workflow
```

---

# 22. Dockerfiles with Compose

Compose commonly works together with Dockerfiles.

---

## Example Structure

```text
project/
│
├── Dockerfile
├── docker-compose.yml
└── app/
```

---

## Example

```yaml
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile
```

---

## Workflow

```text
Dockerfile
     │
     ▼
Build Image
     │
     ▼
Run Container
```

---

## Benefits

```text
Custom Images
Automation
Version Control
```

---

# 23. Scaling Services

Compose can run multiple container replicas.

---

## Example

```bash
docker compose up --scale web=3
```

---

## Architecture

```text
Load Balancer
      │
 ┌────┼────┐
 ▼    ▼    ▼
Web1 Web2 Web3
```

---

## Benefits

```text
High Availability
Load Distribution
Horizontal Scaling
```

---

## Use Cases

```text
Web Servers
API Services
Microservices
```

---

# 24. Profiles

Profiles allow selective service startup.

---

## Example

```yaml
services:
  monitoring:
    profiles:
      - monitoring
```

---

## Start Profile

```bash
docker compose \
--profile monitoring up
```

---

## Architecture

```text
Compose File
      │
      ▼
Profile Selection
      │
      ▼
Selected Services
```

---

## Benefits

```text
Flexibility
Environment Control
Resource Savings
```

---

# 25. Secrets

Secrets securely store sensitive information.

---

## Examples

```text
Passwords
API Keys
Certificates
Tokens
```

---

## Example

```yaml
secrets:
  db_password:
    file: ./password.txt
```

---

## Architecture

```text
Secret
  │
  ▼
Container
```

---

## Benefits

```text
Security
Compliance
Separation of Concerns
```

---

# 26. Configs

Configs provide application configuration files.

---

## Purpose

Store non-sensitive configuration data.

---

## Example

```yaml
configs:
  nginx_conf:
    file: ./nginx.conf
```

---

## Architecture

```text
Config File
     │
     ▼
Container
```

---

## Benefits

```text
Centralized Configuration
Maintainability
Consistency
```

---

# 27. Logging

Compose applications generate logs.

---

## View Logs

```bash
docker compose logs
```

---

## Follow Logs

```bash
docker compose logs -f
```

---

## View Service Logs

```bash
docker compose logs web
```

---

## Architecture

```text
Container
    │
    ▼
Logs
    │
    ▼
Operator
```

---

## Benefits

```text
Troubleshooting
Monitoring
Auditing
```

---

# 28. Monitoring

Monitoring tracks application health and performance.

---

## Common Metrics

```text
CPU
Memory
Network
Disk
Availability
```

---

## Architecture

```text
Containers
     │
     ▼
Metrics
     │
     ▼
Monitoring Platform
```

---

## Common Tools

```text
Prometheus
Grafana
cAdvisor
ELK Stack
```

---

## Benefits

```text
Visibility
Performance Analysis
Capacity Planning
```

---

# 29. Compose Commands

---

## Start Services

```bash
docker compose up
```

---

## Start in Background

```bash
docker compose up -d
```

---

## Stop Services

```bash
docker compose down
```

---

## Restart Services

```bash
docker compose restart
```

---

## List Containers

```bash
docker compose ps
```

---

## Build Images

```bash
docker compose build
```

---

## Pull Images

```bash
docker compose pull
```

---

## Validate Configuration

```bash
docker compose config
```

---

# 30. Troubleshooting

---

## Validate Configuration

```bash
docker compose config
```

---

## View Logs

```bash
docker compose logs
```

---

## Check Running Containers

```bash
docker compose ps
```

---

## Restart Stack

```bash
docker compose restart
```

---

## Remove Everything

```bash
docker compose down
```

---

## Common Issues

```text
Port Conflicts
Missing Volumes
Network Issues
Environment Variables
Image Pull Failures
```

---

## Troubleshooting Workflow

```text
Problem
   │
   ▼
Logs
   │
   ▼
Diagnosis
   │
   ▼
Resolution
```

---

# 31. Production Best Practices

Docker Compose is primarily designed for development and small-to-medium deployments.

---

## Best Practices

```text id="d1r7mx"
Use Environment Files
Use Named Volumes
Implement Health Checks
Use Restart Policies
Separate Environments
```

---

## Recommended Architecture

```text id="v5n3wp"
Application
     │
     ▼
Docker Compose
     │
 ┌───┼───┐
 ▼   ▼   ▼
Web API Database
```

---

## Security Recommendations

```text id="k8m2vr"
Avoid Root Containers
Use Secrets
Restrict Ports
Use Official Images
```

---

## Benefits

```text id="r4p9wk"
Reliability
Maintainability
Security
```

---

# 32. CI/CD Integration

Docker Compose is commonly used in CI/CD pipelines.

---

## Pipeline Workflow

```text id="q7m4vn"
Code Commit
     │
     ▼
Build
     │
     ▼
Docker Compose
     │
     ▼
Testing
     │
     ▼
Deployment
```

---

## Example

```bash id="m3w8pk"
docker compose up -d
```

Run application stack during automated testing.

---

## Common Platforms

```text id="t5n1wr"
GitHub Actions
GitLab CI
Jenkins
Azure DevOps
CircleCI
```

---

## Benefits

```text id="v8p3mk"
Automation
Consistency
Fast Testing
```

---

# 33. Compose vs Docker Run

---

## Docker Run

```bash id="x2m7vp"
docker run nginx
```

Single container.

---

## Docker Compose

```bash id="n6r4wk"
docker compose up
```

Multiple containers.

---

## Comparison

| Feature               | Docker Run | Docker Compose |
| --------------------- | ---------- | -------------- |
| Single Container      | Yes        | Yes            |
| Multi-Container       | No         | Yes            |
| YAML Configuration    | No         | Yes            |
| Networking Automation | Limited    | Yes            |
| Volume Management     | Manual     | Easier         |

---

## Architecture

```text id="k9w2pr"
Docker Run
    │
    ▼
One Container

Docker Compose
    │
    ▼
Application Stack
```

---

# 34. Compose vs Kubernetes

---

## Purpose

Both manage containerized applications.

---

## Docker Compose

```text id="p4n8vx"
Development
Testing
Small Deployments
```

---

## Kubernetes

```text id="f7m2wr"
Production
Large Scale
Orchestration
```

---

## Comparison

| Feature          | Compose | Kubernetes |
| ---------------- | ------- | ---------- |
| Learning Curve   | Low     | High       |
| Setup Complexity | Simple  | Complex    |
| Scaling          | Basic   | Advanced   |
| Self-Healing     | Limited | Yes        |
| Enterprise Scale | Limited | Excellent  |

---

## Architecture

```text id="w3r7mk"
Compose
   │
   ▼
Single Host

Kubernetes
   │
   ▼
Cluster
```

---

# 35. Real-World Architectures

---

## Web Application

```text id="r8p4vn"
User
 │
 ▼
Nginx
 │
 ▼
Backend
 │
 ▼
Database
```

---

## Microservices

```text id="u2m9wk"
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

```text id="m5r1vx"
Prometheus
     │
     ▼
Grafana
```

---

## ELK Stack

```text id="v7n3pk"
Elasticsearch
      │
      ▼
Logstash
      │
      ▼
Kibana
```

---

## Development Environment

```text id="x4m8wr"
Application
     │
     ▼
Database
     │
     ▼
Redis
```

---

# 36. Common Use Cases

---

## Local Development

```text id="n1p6vk"
Application Development
Database Testing
Microservices
```

---

## Testing

```text id="r9w2mn"
Integration Tests
Acceptance Tests
CI/CD Pipelines
```

---

## Learning

```text id="t3m8wp"
Docker Fundamentals
Networking
Volumes
Containers
```

---

## Small Deployments

```text id="q5n1vr"
Internal Applications
Proof of Concepts
Labs
```

---

# 37. Interview Questions

---

## What is Docker Compose?

Docker Compose is a tool used to define and run multi-container Docker applications using YAML files.

---

## What File Does Docker Compose Use?

```text id="v6m4pk"
docker-compose.yml
```

---

## What Command Starts Services?

```bash id="p2r7wn"
docker compose up
```

---

## What Command Stops Services?

```bash id="m8n3vx"
docker compose down
```

---

## What Are Services?

Services define containers within a Compose application.

---

## What Is a Named Volume?

A Docker-managed persistent storage volume.

---

## Difference Between Bind Mount and Volume?

| Bind Mount          | Volume              |
| ------------------- | ------------------- |
| Host Managed        | Docker Managed      |
| Direct Host Access  | Abstracted Storage  |
| Development Focused | Persistence Focused |

---

## What Is depends_on?

A mechanism used to define startup dependencies between services.

---

## Difference Between Docker and Docker Compose?

Docker manages containers.

Docker Compose manages multi-container applications.

---

# 38. Additional Resources

## Official Resources

* Docker Documentation
* Docker Compose Documentation
* Docker Hub
* Docker CLI Reference

---

## Learning Resources

* Docker Labs
* Play With Docker
* Docker GitHub
* Docker Community Forums

---

## Related Technologies

```text id="g4w8pr"
Docker
Kubernetes
containerd
Podman
BuildKit
```

---

# 39. Quick Reference

## Start Services

```bash id="w7m2vk"
docker compose up
```

---

## Background Mode

```bash id="n5r9wp"
docker compose up -d
```

---

## Stop Services

```bash id="p3m6vr"
docker compose down
```

---

## Restart Services

```bash id="x8n1wk"
docker compose restart
```

---

## View Containers

```bash id="r4p7mn"
docker compose ps
```

---

## View Logs

```bash id="k2m9vx"
docker compose logs
```

---

## Follow Logs

```bash id="u6r3wp"
docker compose logs -f
```

---

## Build Images

```bash id="m9n5vk"
docker compose build
```

---

## Pull Images

```bash id="t4w8pr"
docker compose pull
```

---

## Validate Configuration

```bash id="v1m7wn"
docker compose config
```

---

# 📎 Official Documentation

* Docker Compose Documentation: https://docs.docker.com/compose/
* Docker Documentation: https://docs.docker.com
* Docker Hub: https://hub.docker.com
* Docker CLI Reference: https://docs.docker.com/reference/cli/

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, DevOps engineering, cloud engineering, platform engineering, and containerized application development.

Primary references include:

* Docker Documentation
* Docker Compose Documentation
* Docker CLI Documentation
* Docker Hub

Docker and Docker Compose are products of Docker, Inc.

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

You now understand:

* Docker Compose Fundamentals
* Services
* Containers
* Images
* Networks
* Volumes
* Bind Mounts
* Named Volumes
* Environment Variables
* Health Checks
* Scaling
* Secrets
* Configs
* Logging
* Monitoring
* CI/CD Integration
* Production Best Practices

Docker Compose provides:

```text id="f3m8vk"
Multi-Container Applications
+
Networking
+
Persistent Storage
+
Configuration Management
+
Development Workflows
+
Automation
```

and remains one of the most popular tools for local development, testing, CI/CD pipelines, and small-to-medium containerized deployments.

---

**Last Updated:** 2026
**Platform:** Docker Compose
**Version:** Docker Compose v2+
**License:** Apache License 2.0

For latest information, visit https://docs.docker.com/compose/