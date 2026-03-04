# Harbor Cheatsheet

![Harbor Logo](https://goharbor.io/img/harbor-horizontal-color.png)

---

## Table of Contents
1. [What is Harbor?](#1-what-is-harbor)
2. [Core Concepts](#2-core-concepts)
3. [Installation](#3-installation)
4. [Project and Repository Management](#4-project-and-repository-management)
5. [Pushing Images](#5-pushing-images)
6. [Pulling Images](#6-pulling-images)
7. [Image Scanning and Security](#7-image-scanning-and-security)
8. [Helm Charts Management](#8-helm-charts-management)
9. [Replication](#9-replication)
10. [Users and Access Control](#10-users-and-access-control)
11. [Best Practices](#11-best-practices)
12. [Additional Resources](#12-additional-resources)

---

## 1. What is Harbor?

Harbor is an open-source container image registry and Helm chart repository. It provides secure, compliant management of cloud-native artifacts with features like vulnerability scanning, replication, access control, and webhook notifications.

**Key Benefits:**
- Container image registry (Docker/OCI compatible)
- Helm chart repository
- Image vulnerability scanning (Trivy)
- Multi-tenant architecture with RBAC
- Automated replication and synchronization
- Web UI and REST API
- High availability support
- Open-source and free

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Project** | Namespace for repositories (isolated space) |
| **Repository** | Collection of images with same name (e.g., myapp) |
| **Image** | Container image with tags (e.g., myapp:1.0.0) |
| **Artifact** | Stored image or Helm chart |
| **Tag** | Version identifier for images |
| **Scanner** | Tool for vulnerability scanning (Trivy) |
| **Replication** | Sync images between registries |
| **Robot Account** | Service account for automation |
| **Webhook** | Event notifications |

---

## 3. Installation

### Docker Compose Installation

```bash
# Download Harbor
wget https://github.com/goharbor/harbor/releases/download/v2.9.x/harbor-online-installer-v2.9.x.tgz

# Extract
tar xzvf harbor-online-installer-v2.9.x.tgz
cd harbor

# Configure harbor.yml
cp harbor.yml.tmpl harbor.yml
vi harbor.yml

# Key configurations in harbor.yml
# hostname: harbor.example.com
# port: 80 (or 443 for HTTPS)
# harbor_admin_password: Harbor12345
# database:
#   password: root123
# storage_service:
#   filesystem:
#     rootdirectory: /data
```

### Prepare and Install

```bash
# Generate configuration
./prepare

# Start Harbor with Docker Compose
docker-compose up -d

# Check status
docker-compose ps

# View logs
docker-compose logs -f

# Stop Harbor
docker-compose down

# Start Harbor
docker-compose up -d

# Access at http://harbor.example.com
# Default credentials: admin / Harbor12345
```

### Kubernetes Helm Installation

```bash
# Add Harbor Helm repository
helm repo add harbor https://helm.goharbor.io
helm repo update

# Create namespace
kubectl create namespace harbor

# Create values.yaml
cat > values.yaml <<EOF
expose:
  type: ingress
  ingress:
    hosts:
      core: harbor.example.com
      notary: notary.example.com
    className: nginx

externalURL: https://harbor.example.com

harborAdminPassword: "Harbor12345"

persistence:
  enabled: true
  imageChartStorage:
    type: filesystem
    filesystem:
      rootdirectory: /storage
  storageClass: standard

registry:
  replicas: 2

clair:
  enabled: true

trivy:
  enabled: true

notary:
  enabled: true
EOF

# Install Harbor
helm install harbor harbor/harbor \
  --namespace harbor \
  --values values.yaml

# Verify installation
kubectl get pods -n harbor

# Port forward (if using NodePort)
kubectl port-forward -n harbor svc/harbor 8080:80
```

### Post-Installation Setup

```bash
# Test Docker login
docker login harbor.example.com -u admin -p Harbor12345

# Verify installation
docker pull harbor.example.com/library/hello-world
```

---

## 4. Project and Repository Management

### Create Project via UI

```
Harbor UI → Projects → New Project
├── Project Name: my-project
├── Access Level: Private (or Public)
├── Project Members: (optional)
├── CVE Allowlist: (optional)
└── Create Project
```

### Create Project via CLI

```bash
# Using curl to create project
curl -u admin:Harbor12345 -X POST \
  -H "Content-Type: application/json" \
  -d '{"project_name":"my-project","public":false}' \
  http://harbor.example.com/api/v2.0/projects
```

### Project Settings

```
Harbor UI → Projects → my-project → Project Settings
├── General
│   ├── Public/Private toggle
│   └── Enable Content Trust (Notary)
├── Quota
│   └── Set storage quota limit
├── Members
│   ├── Add user/group
│   └── Assign roles (Developer, Guest, Admin)
├── Robot Accounts
│   ├── Create robot account
│   └── Set permissions
├── Webhook
│   ├── Webhook URL
│   ├── Events: push, pull, delete
│   └── Skip certificate verification
└── Labels
    └── Create labels for artifacts
```

### Repository Management

```
Harbor UI → Projects → my-project → Repositories
├── View all repositories
├── Search repositories
├── Delete repository
└── View repository details
    ├── Tags
    ├── Artifacts
    ├── Dependencies
    └── Vulnerabilities
```

---

## 5. Pushing Images

### Docker Push to Harbor

```bash
# Login to Harbor
docker login harbor.example.com -u admin -p Harbor12345

# Tag image
docker tag myapp:1.0.0 harbor.example.com/my-project/myapp:1.0.0

# Push image
docker push harbor.example.com/my-project/myapp:1.0.0

# Push multiple tags
docker tag myapp:1.0.0 harbor.example.com/my-project/myapp:latest
docker push harbor.example.com/my-project/myapp:latest

# Push with build info
docker tag myapp:1.0.0 harbor.example.com/my-project/myapp:build-123
docker push harbor.example.com/my-project/myapp:build-123
```

### Automated Push from CI/CD

```yaml
# GitHub Actions example
name: Push to Harbor

on:
  push:
    branches:
      - main

jobs:
  push:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Build Docker image
        run: docker build -t myapp:${{ github.sha }} .
      
      - name: Login to Harbor
        run: |
          docker login -u robot\$cicd-robot -p ${{ secrets.HARBOR_TOKEN }} harbor.example.com
      
      - name: Tag image
        run: |
          docker tag myapp:${{ github.sha }} harbor.example.com/my-project/myapp:${{ github.sha }}
          docker tag myapp:${{ github.sha }} harbor.example.com/my-project/myapp:latest
      
      - name: Push image
        run: |
          docker push harbor.example.com/my-project/myapp:${{ github.sha }}
          docker push harbor.example.com/my-project/myapp:latest
```

### GitLab CI Push

```yaml
push-to-harbor:
  stage: push
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker login -u robot\$cicd-robot -p $HARBOR_TOKEN harbor.example.com
    - docker build -t harbor.example.com/my-project/myapp:$CI_COMMIT_SHA .
    - docker push harbor.example.com/my-project/myapp:$CI_COMMIT_SHA
  only:
    - main
```

### Jenkins Pipeline Push

```groovy
pipeline {
  agent any
  
  stages {
    stage('Build Image') {
      steps {
        sh 'docker build -t myapp:${BUILD_NUMBER} .'
      }
    }
    
    stage('Push to Harbor') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'harbor-robot',
          usernameVariable: 'HARBOR_USER',
          passwordVariable: 'HARBOR_PASS'
        )]) {
          sh '''
            docker login -u ${HARBOR_USER} -p ${HARBOR_PASS} harbor.example.com
            docker tag myapp:${BUILD_NUMBER} harbor.example.com/my-project/myapp:${BUILD_NUMBER}
            docker push harbor.example.com/my-project/myapp:${BUILD_NUMBER}
          '''
        }
      }
    }
  }
}
```

---

## 6. Pulling Images

### Docker Pull from Harbor

```bash
# Login to Harbor (if private)
docker login harbor.example.com -u admin -p Harbor12345

# Pull image
docker pull harbor.example.com/my-project/myapp:1.0.0

# Pull latest tag
docker pull harbor.example.com/my-project/myapp:latest

# Pull and run
docker run -d harbor.example.com/my-project/myapp:1.0.0
```

### Kubernetes Pull Images

```yaml
# Kubernetes Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
  namespace: default
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      imagePullSecrets:
      - name: harbor-secret
      containers:
      - name: myapp
        image: harbor.example.com/my-project/myapp:1.0.0
        ports:
        - containerPort: 8080
```

### Create Image Pull Secret

```bash
# Create secret for private images
kubectl create secret docker-registry harbor-secret \
  --docker-server=harbor.example.com \
  --docker-username=admin \
  --docker-password=Harbor12345 \
  --docker-email=admin@example.com \
  -n default

# Or using manifest
kubectl apply -f - <<EOF
apiVersion: v1
kind: Secret
metadata:
  name: harbor-secret
  namespace: default
type: kubernetes.io/dockercfg
data:
  .dockercfg: $(cat ~/.docker/config.json | base64 -w0)
EOF
```

### Docker Compose Pull

```yaml
version: '3.8'

services:
  myapp:
    image: harbor.example.com/my-project/myapp:1.0.0
    ports:
      - "8080:8080"
    environment:
      - LOG_LEVEL=info

# Login first
# docker login harbor.example.com

# Then run
# docker-compose up -d
```

---

## 7. Image Scanning and Security

### Enable Vulnerability Scanning

```
Harbor UI → Administration → Configuration → Security
├── Vulnerability Scanner: Trivy (default)
├── Scan Image on Push: Enable
├── Auto Scan: Enable
└── Severity Threshold: Medium
```

### Manual Scan

```
Harbor UI → Projects → my-project → Repositories → myapp
├── Artifacts
├── Select image tag
├── Scan button
└── View results
```

### View Scan Results

```
Harbor UI → Projects → my-project → Repositories → myapp
├── Select image tag
├── Vulnerabilities section
├── Filter by severity: Critical, High, Medium, Low
├── View CVE details
└── Remediation info
```

### Vulnerability Policy

```
Harbor UI → Administration → Configuration → Policy
├── Vulnerability Scanning Policy
├── Prevent pull of vulnerable images: Enable
├── Severity threshold: High, Critical
├── Reject on scan failed: Yes
├── Reject if no artifact scan: Yes
└── Save
```

### CVE Allowlist

```
Harbor UI → Administration → CVE Allowlist
├── Add CVE to allowlist
├── CVE ID: CVE-2023-xxxx
├── Expiration date: (optional)
└── Create

# Or per-project allowlist
Harbor UI → Projects → my-project → Project Settings
├── CVE Allowlist
├── Add CVEs to project allowlist
└── Save
```

### Signed Images (Notary)

```bash
# Enable content trust
export DOCKER_CONTENT_TRUST=1
export DOCKER_CONTENT_TRUST_SERVER=https://notary.harbor.example.com

# Push signed image
docker push harbor.example.com/my-project/myapp:1.0.0

# Pull only signed images
docker pull harbor.example.com/my-project/myapp:1.0.0
```

---

## 8. Helm Charts Management

### Helm Repository Configuration

```bash
# Add Harbor Helm repository
helm repo add my-harbor https://harbor.example.com/chartrepo/my-project \
  --username admin \
  --password Harbor12345

# Update repository
helm repo update

# List charts
helm search repo my-harbor

# Pull chart
helm pull my-harbor/mychart --version 1.0.0
```

### Push Helm Chart to Harbor

```bash
# Install Helm Push plugin
helm plugin install https://github.com/chartmuseum/helm-push

# Push chart to Harbor
helm push ./mychart/ \
  oci://harbor.example.com/my-project

# Or upload via REST API
curl -u admin:Harbor12345 -X POST \
  -F "chart=@mychart-1.0.0.tgz" \
  http://harbor.example.com/api/v2.0/projects/my-project/repositories/mychart/charts
```

### OCI Artifact Support

```bash
# Login to Harbor registry
helm registry login harbor.example.com \
  -u admin \
  -p Harbor12345

# Push Helm chart as OCI artifact
helm push ./mychart \
  oci://harbor.example.com/my-project

# Pull Helm chart
helm pull oci://harbor.example.com/my-project/mychart \
  --version 1.0.0
```

### Install from Harbor Helm Chart

```bash
# Add repository
helm repo add harbor-charts https://harbor.example.com/chartrepo/my-project \
  --username admin \
  --password Harbor12345

# Install chart
helm install my-release harbor-charts/mychart \
  --namespace default \
  --values values.yaml

# Or install from OCI
helm install my-release \
  oci://harbor.example.com/my-project/mychart \
  --version 1.0.0
```

---

## 9. Replication

### Configure Replication Endpoints

```
Harbor UI → Administration → Registries
├── New Endpoint
├── Provider: Docker Registry (or Quay, AWS ECR, Azure ACR, Google GCR)
├── Name: remote-registry
├── Description: Remote Docker Registry
├── URL: https://docker.io
├── Credentials:
│   ├── Username: (if needed)
│   └── Password: (if needed)
├── Test Connection
└── Create
```

### Create Replication Rule

```
Harbor UI → Administration → Replication
├── New Rule
├── Name: replicate-to-remote
├── Description: Replicate images to remote registry
├── Source Registry: Local
├── Destination Registry: remote-registry
├── Repository Name Pattern: my-project/**
├── Tag Name Pattern: *
├── Trigger Mode: Push-based (or Scheduled)
├── Deletion: Enable (to sync deletions)
├── Override: Enable
└── Create Rule
```

### Manual Replication

```
Harbor UI → Projects → my-project → Repositories
├── Select repository
├── More Actions → Replicate
├── Select destination registry
├── Replicate
```

### Scheduled Replication

```
Harbor UI → Administration → Replication
├── Create Rule with Scheduled trigger
├── Trigger Schedule: Cron expression
├── Example: 0 2 * * * (daily at 2 AM)
└── Save
```

### REST API Replication

```bash
# List replication rules
curl -u admin:Harbor12345 \
  http://harbor.example.com/api/v2.0/replication/rules

# Create replication rule
curl -u admin:Harbor12345 -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "name": "replicate-rule",
    "src_registry": null,
    "dest_registry": {"id": 1},
    "src_namespaces": ["my-project"],
    "trigger": {"type": "manual"}
  }' \
  http://harbor.example.com/api/v2.0/replication/rules

# Execute replication
curl -u admin:Harbor12345 -X POST \
  http://harbor.example.com/api/v2.0/replication/executions
```

---

## 10. Users and Access Control

### Create Local User

```
Harbor UI → Administration → Users
├── New User
├── Username: developer1
├── Full Name: John Developer
├── Email: developer@example.com
├── Password: strongpassword123
├── Confirm Password: strongpassword123
└── Create
```

### Create Robot Account

```
Harbor UI → Administration → Robot Accounts
├── New Robot Account
├── Robot Account Name: cicd-robot
├── Namespace: All Projects (or specific project)
├── Permissions:
│   ├── Repository: Push image, Pull image
│   ├── Build project: Build
│   └── Delete artifact
├── Expiration Time: No expiration (or set date)
└── Create
```

### Use Robot Account

```bash
# Get robot account token
# Harbor UI → Administration → Robot Accounts
# Select robot account → Copy token

# Login with robot account
docker login -u robot\$cicd-robot -p <token> harbor.example.com

# Use in CI/CD
export HARBOR_USER="robot\$cicd-robot"
export HARBOR_PASSWORD="<token>"
```

### Project Member Management

```
Harbor UI → Projects → my-project → Members
├── Add Member
├── User: developer1
├── Role:
│   ├── Guest: Read-only
│   ├── Developer: Push/pull images
│   ├── Maintainer: Admin project
│   └── Admin: Full access
├── Add
```

### LDAP Integration

```
Harbor UI → Administration → Configuration → Authentication
├── Authentication Mode: LDAP
├── LDAP Server URL: ldap://ldap.example.com:389
├── LDAP Search Base: dc=example,dc=com
├── LDAP UID: uid
├── LDAP Filter: (objectClass=person)
├── LDAP Bind Root DN: cn=admin,dc=example,dc=com
├── LDAP Bind Root Password: ****
├── Verify Certificate: Yes (if LDAPS)
└── Test LDAP Connection
```

### OIDC Integration

```
Harbor UI → Administration → Configuration → Authentication
├── Authentication Mode: OIDC
├── OIDC Provider Name: okta
├── OIDC Endpoint: https://okta.example.com
├── OIDC Client ID: ****
├── OIDC Client Secret: ****
├── OIDC Groups Claim Name: groups
├── User Claim Name: email
└── Test OIDC Connection
```

---

## 11. Best Practices

### Project Organization

```
Harbor Projects:
├── production (Private)
│   └── Repositories:
│       ├── app-api
│       ├── app-web
│       ├── app-worker
│       └── app-db
├── staging (Private)
│   └── Repositories:
│       ├── app-api
│       ├── app-web
│       ├── app-worker
│       └── app-db
├── development (Public)
│   └── Repositories:
│       ├── app-api-dev
│       ├── app-web-dev
│       └── app-worker-dev
└── base-images (Public/Private)
    └── Repositories:
        ├── ubuntu-20.04
        ├── node-16
        └── python-3.9
```

### Image Tagging Strategy

```
Tagging Conventions:

Release Images:
├── harbor.example.com/production/app-api:1.0.0 (semantic version)
├── harbor.example.com/production/app-api:latest (latest stable)
└── harbor.example.com/production/app-api:stable (stable version)

Development Images:
├── harbor.example.com/development/app-api:develop (development branch)
├── harbor.example.com/development/app-api:main (main branch)
└── harbor.example.com/development/app-api:feature-xyz (feature branch)

Build Images:
├── harbor.example.com/staging/app-api:build-123 (build number)
├── harbor.example.com/staging/app-api:git-abc1234 (commit hash)
└── harbor.example.com/staging/app-api:2024-01-15 (date)
```

### Security Best Practices

```
✓ Enable vulnerability scanning (Trivy)
✓ Set vulnerability policy to prevent pull
✓ Use robot accounts for CI/CD (not personal accounts)
✓ Enable HTTPS/TLS with valid certificates
✓ Implement LDAP/OIDC for user authentication
✓ Use image signing (Notary) for production
✓ Regular backup of Harbor data
✓ Monitor audit logs
✓ Enable project quotas to prevent disk exhaustion
✓ Use private projects by default
✓ Rotate robot account tokens regularly
✓ Implement network policies for Harbor access
```

### Disaster Recovery

```
Harbor Backup:
1. Database backup
   docker-compose exec -T postgresql \
     pg_dump -U postgres postgres > harbor-db.sql

2. Image storage backup
   tar -czf harbor-images.tar.gz /data/

3. Configuration backup
   cp -r harbor.yml /backup/

4. Restore process
   - Restore database
   - Restore storage
   - Restore configuration
   - Restart Harbor
```

### Performance Optimization

```
Harbor Configuration:
├── Database:
│   ├── Connection pool size
│   └── Max connections
├── Registry:
│   ├── Number of replicas
│   ├── Resource limits
│   └── Cache settings
├── Storage:
│   ├── Use object storage (S3, Azure, GCS)
│   ├── Enable compression
│   └── Implement tiering
└── Monitoring:
    ├── Enable Prometheus metrics
    ├── Set up alerting
    └── Monitor disk space
```

### Harbor in Production

```yaml
# Docker Compose production setup
version: '3.8'

services:
  harbor-postgresql:
    image: postgres:14
    environment:
      POSTGRES_PASSWORD: secure_password_123
    volumes:
      - harbor-db:/var/lib/postgresql/data
    restart: always

  harbor-redis:
    image: redis:7
    volumes:
      - harbor-redis:/data
    restart: always

  harbor-core:
    image: goharbor/harbor-core:latest
    environment:
      HARBOR_ADMIN_PASSWORD: Harbor12345
    depends_on:
      - harbor-postgresql
      - harbor-redis
    restart: always

  harbor-registry:
    image: goharbor/registry-photon:latest
    volumes:
      - harbor-storage:/storage
    restart: always

  # Other services...

volumes:
  harbor-db:
  harbor-redis:
  harbor-storage:
```

---

## 12. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://goharbor.io/docs/latest/getting-started/ |
| **Installation Guide** | https://goharbor.io/docs/latest/install-config/ |
| **Docker Compose Install** | https://goharbor.io/docs/latest/install-config/docker-compose-install/ |
| **Kubernetes Install** | https://goharbor.io/docs/latest/install-config/harbor-ha-helm/ |
| **REST API** | https://goharbor.io/docs/latest/build-customize-contribute/api/ |
| **User Guide** | https://goharbor.io/docs/latest/working-with-images/ |
| **Administration** | https://goharbor.io/docs/latest/administration/ |
| **Configuration** | https://goharbor.io/docs/latest/install-config/configure-yml-file/ |
| **Replication** | https://goharbor.io/docs/latest/administration/configuring-replication/ |
| **Security** | https://goharbor.io/docs/latest/install-config/securing-harbor/ |

### Community and Support

| Resource | URL |
|----------|-----|
| **GitHub Repository** | https://github.com/goharbor/harbor |
| **GitHub Discussions** | https://github.com/goharbor/harbor/discussions |
| **GitHub Issues** | https://github.com/goharbor/harbor/issues |
| **Slack Channel** | https://cloud-native.slack.com/messages/harbor |
| **Community Meetings** | https://goharbor.io/community/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Video Tutorials** | https://www.youtube.com/c/Goharborrepo |
| **Documentation Site** | https://goharbor.io/docs/ |
| **Blog** | https://goharbor.io/blog/ |
| **Docker Hub** | https://hub.docker.com/r/goharbor |
| **Helm Charts** | https://helm.goharbor.io/ |

### Related Tools

| Tool | URL |
|------|-----|
| **Trivy** | https://github.com/aquasecurity/trivy |
| **Notary** | https://github.com/notaryproject/notary |
| **Docker** | https://www.docker.com/ |
| **Kubernetes** | https://kubernetes.io/ |
| **Helm** | https://helm.sh/ |

---

## Quick Reference

```bash
# Docker Compose Installation
wget https://github.com/goharbor/harbor/releases/download/v2.9.x/harbor-online-installer-v2.9.x.tgz
tar xzvf harbor-online-installer-v2.9.x.tgz
cd harbor
./prepare
docker-compose up -d

# Docker login
docker login harbor.example.com -u admin -p Harbor12345

# Docker push
docker tag myapp:1.0.0 harbor.example.com/my-project/myapp:1.0.0
docker push harbor.example.com/my-project/myapp:1.0.0

# Docker pull
docker pull harbor.example.com/my-project/myapp:1.0.0

# Create project
curl -u admin:Harbor12345 -X POST \
  -H "Content-Type: application/json" \
  -d '{"project_name":"my-project","public":false}' \
  http://harbor.example.com/api/v2.0/projects

# Create robot account
# Harbor UI → Administration → Robot Accounts → New Robot Account

# Helm push
helm push ./mychart oci://harbor.example.com/my-project

# Helm pull
helm pull oci://harbor.example.com/my-project/mychart --version 1.0.0
```

---

**Last Updated:** 2026
**Harbor Version:** 2.x+
**License:** Apache 2.0 (Open Source)

For latest information, visit [Harbor Documentation](https://goharbor.io/docs/)