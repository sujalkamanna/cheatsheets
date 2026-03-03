# ArgoCD CI/CD Cheatsheet

![ArgoCD Logo](https://argo-cd.readthedocs.io/en/stable/assets/logo.png)

---

## Table of Contents
1. [What is ArgoCD?](#1-what-is-argocd)
2. [Core Concepts](#2-core-concepts)
3. [Installation and Setup](#3-installation-and-setup)
4. [Repository Configuration](#4-repository-configuration)
5. [Application Definition](#5-application-definition)
6. [Deployment Strategies](#6-deployment-strategies)
7. [Sync Policies](#7-sync-policies)
8. [Notifications and Webhooks](#8-notifications-and-webhooks)
9. [Multi-Environment Setup](#9-multi-environment-setup)
10. [RBAC and Security](#10-rbac-and-security)
11. [Monitoring and Health](#11-monitoring-and-health)
12. [Advanced Features](#12-advanced-features)
13. [Best Practices](#13-best-practices)
14. [Additional Resources](#14-additional-resources)

---

## 1. What is ArgoCD?

ArgoCD is a declarative, GitOps continuous delivery tool for Kubernetes. It monitors your Git repositories and automatically deploys the declared application state to your Kubernetes clusters.

**Key Benefits:**
- Git as single source of truth
- Automated synchronization with Git state
- Visual representation of application deployments
- Multi-cluster deployment support
- Rollback capabilities

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Application** | A deployed Kubernetes application managed by ArgoCD |
| **Repository** | Git repository containing application manifests |
| **Destination** | Target Kubernetes cluster and namespace |
| **Sync Status** | Comparison between Git state and actual cluster state |
| **Health Status** | Overall health of deployed resources |
| **AppProject** | Logical grouping of applications with access controls |
| **Manifest** | Kubernetes resources (YAML files) defining applications |

---

## 3. Installation and Setup

### Install ArgoCD on Kubernetes

```bash
# Create namespace
kubectl create namespace argocd

# Install ArgoCD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Verify installation
kubectl get pods -n argocd
```

### Access ArgoCD UI

```bash
# Port forward to access UI
kubectl port-forward svc/argocd-server -n argocd 8080:443

# Get initial admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

# Access at: https://localhost:8080
```

### Install ArgoCD CLI

```bash
# macOS
brew install argocd

# Linux
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd

# Verify installation
argocd version
```

---

## 4. Repository Configuration

### Connect Git Repository

```bash
# Login to ArgoCD
argocd login localhost:8080 --username admin --password <password>

# Add repository
argocd repo add https://github.com/username/repo.git \
  --username git-username \
  --password git-token

# List repositories
argocd repo list

# Remove repository
argocd repo rm https://github.com/username/repo.git
```

### Private Repository with SSH

```bash
# Generate SSH key
ssh-keygen -t rsa -b 4096 -f argocd-repo-key -N ""

# Add public key to GitHub
cat argocd-repo-key.pub

# Add repository with SSH
argocd repo add git@github.com:username/repo.git \
  --ssh-private-key-path argocd-repo-key
```

### Repository Secrets

```bash
# Create repository credentials secret
kubectl create secret generic argocd-repo-creds \
  --from-literal=username=git-user \
  --from-literal=password=git-token \
  -n argocd

# View configured repositories
kubectl get secrets -n argocd -l argocd.argoproj.io/secret-type=repository
```

---

## 5. Application Definition

### Basic Application YAML

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/username/repo.git
    targetRevision: HEAD
    path: k8s/
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

### Application with Helm

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-helm-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://charts.example.com
    chart: my-chart
    targetRevision: 1.0.0
    helm:
      values: |
        replicaCount: 3
        image:
          tag: v1.0.0
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    syncOptions:
    - CreateNamespace=true
```

### Application with Kustomize

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-kustomize-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/username/repo.git
    path: overlays/production
    kustomize:
      version: v4.5.4
  destination:
    server: https://kubernetes.default.svc
    namespace: default
```

### Create Application via CLI

```bash
# Create application
argocd app create my-app \
  --repo https://github.com/username/repo.git \
  --path k8s/ \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default

# List applications
argocd app list

# Get application details
argocd app get my-app

# Delete application
argocd app delete my-app
```

---

## 6. Deployment Strategies

### Blue-Green Deployment

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: blue-green-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/username/repo.git
    path: k8s/blue-green/
    targetRevision: main
  destination:
    server: https://kubernetes.default.svc
    namespace: production
  syncPolicy:
    automated:
      prune: false  # Manual prune for blue-green
      selfHeal: true
```

```bash
# Manage blue-green deployments
argocd app sync blue-green-app --revision main

# Monitor sync progress
argocd app wait blue-green-app --sync
```

### Canary Deployment

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: canary-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/username/repo.git
    path: k8s/canary/
    targetRevision: main
  destination:
    server: https://kubernetes.default.svc
    namespace: production
  syncPolicy:
    syncOptions:
    - RespectIgnoreDifferences=true
```

### Rolling Update

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app:v1.0.0
```

---

## 7. Sync Policies

### Automated Sync

```yaml
spec:
  syncPolicy:
    automated:
      prune: true      # Delete resources removed from Git
      selfHeal: true   # Auto-sync on cluster drift
      allowEmpty: false # Prevent deletion of all resources
    syncOptions:
    - CreateNamespace=true
    retry:
      limit: 5
      backoff:
        duration: 5s
        factor: 2
        maxDuration: 3m
```

### Manual Sync

```yaml
spec:
  syncPolicy:
    automated: null  # No automatic sync
```

```bash
# Manually sync application
argocd app sync my-app

# Sync specific revision
argocd app sync my-app --revision main

# Sync with dry-run
argocd app sync my-app --dry-run

# Sync specific resource
argocd app sync my-app --resource apps/v1/Deployment:default/my-app
```

### Partial Sync

```bash
# Sync only specific resources
argocd app sync my-app \
  --resource apps/v1/Deployment:default/api-server \
  --resource v1/Service:default/api-server
```

### Rollback

```bash
# Rollback to previous sync
argocd app rollback my-app

# Rollback to specific revision
argocd app rollback my-app 1

# Rollback to Git revision
argocd app sync my-app --revision abc1234
```

---

## 8. Notifications and Webhooks

### Configure GitHub Webhook

```yaml
# In repository settings, add webhook:
# Payload URL: https://argocd.example.com/api/webhook
# Events: Push events
# Content-type: application/json
```

```bash
# ArgoCD detects changes and auto-syncs
# Notification settings in argocd-notifications ConfigMap
```

### Slack Notifications

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-notifications-cm
  namespace: argocd
data:
  service.slack: |
    token: $slack-token
  trigger.on-sync-succeeded: |
    - when: app.status.operationState.finishedAt != ''
    - oncePer: app.status.operationState.finishedAt
    - if: app.status.operationState.phase in ['Succeeded']
  template.app-health-degraded: |
    message: |
      Application {{.app.metadata.name}} health is degraded
  template.app-sync-succeeded: |
    message: |
      Application {{.app.metadata.name}} has been synced successfully
  trigger.on-health-degraded: |
    - when: app.status.health.status == 'Degraded'
    - oncePer: app.status.health.status == 'Degraded'
```

### Email Notifications

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-notifications-cm
  namespace: argocd
data:
  service.email.gmail: |
    host: smtp.gmail.com
    port: 587
    from: argocd@example.com
    username: $email-username
    password: $email-password
  trigger.on-deployed: |
    - when: app.status.operationState.finishedAt != '' and app.status.operationState.phase in ['Succeeded']
  template.app-deployed: |
    subject: Application {{.app.metadata.name}} deployed
    body: |
      Application {{.app.metadata.name}} has been deployed to {{.app.status.operationState.operation.sync.revision}}
```

---

## 9. Multi-Environment Setup

### Multiple Clusters

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: multi-cluster-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/username/repo.git
    path: k8s/
    targetRevision: main
  destinations:
  - server: https://kubernetes.default.svc
    namespace: production
    name: production-cluster
  - server: https://staging-cluster-api:6443
    namespace: staging
    name: staging-cluster
```

### Environment-Specific Values

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: env-specific-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/username/repo.git
    path: overlays/production
    targetRevision: main
    kustomize:
      version: v4.5.4
      commonLabels:
        environment: production
      commonAnnotations:
        deployer: argocd
  destination:
    server: https://kubernetes.default.svc
    namespace: production
```

### Application Set for Multiple Apps

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: multi-app
  namespace: argocd
spec:
  generators:
  - list:
      elements:
      - cluster: production
        url: https://prod-api:6443
      - cluster: staging
        url: https://staging-api:6443
  template:
    metadata:
      name: app-{{cluster}}
    spec:
      project: default
      source:
        repoURL: https://github.com/username/repo.git
        path: k8s/{{cluster}}/
        targetRevision: main
      destination:
        server: '{{url}}'
        namespace: default
      syncPolicy:
        automated:
          prune: true
          selfHeal: true
```

---

## 10. RBAC and Security

### Create AppProject

```bash
# Create project for team
argocd proj create team-project \
  --destinations https://kubernetes.default.svc,team-ns \
  --sources https://github.com/username/repo.git \
  --allow-cluster-resource '*/*' \
  --allow-namespace-resource '*/*' \
  --deny-cluster-resource 'Secrets/*' 'Namespace/*'
```

### AppProject YAML

```yaml
apiVersion: argoproj.io/v1alpha1
kind: AppProject
metadata:
  name: team-project
  namespace: argocd
spec:
  sourceRepos:
  - 'https://github.com/username/repo.git'
  destinations:
  - server: https://kubernetes.default.svc
    namespace: team-ns
    name: team-cluster
  clusterResourceBlacklist:
  - group: ''
    kind: ResourceQuota
  - group: ''
    kind: NetworkPolicy
  clusterResourceWhitelist:
  - group: '*'
    kind: '*'
  namespaceResourceBlacklist:
  - group: ''
    kind: ResourceQuota
  namespaceResourceWhitelist:
  - group: '*'
    kind: '*'
```

### RBAC Policy

```bash
# Edit RBAC ConfigMap
kubectl edit configmap argocd-rbac-cm -n argocd
```

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-rbac-cm
  namespace: argocd
data:
  policy.default: role:readonly
  policy.csv: |
    p, role:org-admin, applications, *, */*, allow
    p, role:org-admin, repositories, *, *, allow
    p, role:org-admin, clusters, *, *, allow
    p, role:org-admin, accounts, *, *, allow
    p, role:org-admin, appprojects, *, *, allow
    
    p, role:team-lead, applications, get, team-project/*, allow
    p, role:team-lead, applications, sync, team-project/*, allow
    
    p, role:developer, applications, get, team-project/*, allow
    
    g, john@example.com, role:org-admin
    g, team, role:team-lead
```

---

## 11. Monitoring and Health

### Application Health Status

```bash
# Check application health
argocd app get my-app

# Watch application status
argocd app wait my-app --sync

# Check resource health
kubectl get applications -n argocd -o wide
```

### Health Check Configuration

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-app-health-assessment-rules-cm
  namespace: argocd
data:
  resource.customresourcedefinition.apiextensions.k8s.io_health.lua: |
    hs = {}
    if obj.status ~= nil then
      if obj.status.conditions ~= nil then
        for i, condition in ipairs(obj.status.conditions) do
          if condition.type == "Established" and condition.status == "False" then
            hs.status = "Progressing"
            hs.message = condition.message
            return hs
          end
        end
      end
    end
    hs.status = "Healthy"
    return hs
```

### Prometheus Metrics

```bash
# Access ArgoCD metrics
kubectl port-forward svc/argocd-metrics -n argocd 8082:8082

# Key metrics:
# argocd_app_info
# argocd_app_sync_total
# argocd_app_sync_duration_seconds
# argocd_server_request_count
```

---

## 12. Advanced Features

### Helm Repositories

```bash
# Add Helm repository
argocd repo add https://charts.bitnami.com/bitnami \
  --type helm

# List Helm repositories
argocd repo list --type helm
```

### Custom Tools

```yaml
# Enable custom tool for Git integration
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cm
  namespace: argocd
data:
  tool.custom.plugin: |
    command: [./custom-plugin.sh]
    args:
      - -c
      - kustomize
```

### Diff Customization

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cm
  namespace: argocd
data:
  diff.ignoreAggregatedRoles: 'true'
```

### Webhook Authorization

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: github-webhook-secret
  namespace: argocd
type: Opaque
stringData:
  webhook.github.secret: your-webhook-secret
```

---

## 13. Best Practices

### Use GitOps Workflow

```bash
# 1. Make changes in Git
git commit -m "Update deployment image"

# 2. ArgoCD detects and syncs automatically
# 3. Monitor in ArgoCD UI
argocd app get my-app

# 4. Review diff before sync
argocd app diff my-app

# 5. Sync if satisfied
argocd app sync my-app
```

### Organize Repositories

```
repo/
├── base/
│   └── my-app/
│       ├── deployment.yaml
│       ├── service.yaml
│       └── kustomization.yaml
├── overlays/
│   ├── development/
│   ├── staging/
│   └── production/
└── applications/
    ├── app1.yaml
    └── app2.yaml
```

### Namespace Isolation

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: production
---
apiVersion: v1
kind: ResourceQuota
metadata:
  name: production-quota
  namespace: production
spec:
  hard:
    requests.cpu: "100"
    requests.memory: 200Gi
    limits.cpu: "200"
    limits.memory: 400Gi
```

### Version Control

```bash
# Use semantic versioning for releases
git tag -a v1.0.0 -m "Production release"
git push origin v1.0.0

# Reference specific versions in ArgoCD
argocd app set my-app --revision v1.0.0
```

### Sync Strategy

```yaml
# Use automated sync with pruning for safe deployments
syncPolicy:
  automated:
    prune: true    # Remove deleted resources
    selfHeal: true # Fix drift automatically
  syncOptions:
  - CreateNamespace=true
  retry:
    limit: 5
```

### Monitoring and Alerting

```bash
# Set up Prometheus scraping
kubectl apply -f - <<EOF
apiVersion: v1
kind: Service
metadata:
  name: argocd-metrics
  namespace: argocd
  labels:
    app.kubernetes.io/name: argocd-metrics
  annotations:
    prometheus.io/scrape: 'true'
    prometheus.io/port: '8082'
EOF
```

---

## 14. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://argo-cd.readthedocs.io/en/stable/ |
| **Installation Guide** | https://argo-cd.readthedocs.io/en/stable/operator-manual/installation/ |
| **Application CRD** | https://argo-cd.readthedocs.io/en/stable/user-guide/application.yaml |
| **User Guide** | https://argo-cd.readthedocs.io/en/stable/user-guide/ |
| **CLI Commands** | https://argo-cd.readthedocs.io/en/stable/user-guide/commands/argocd/ |
| **Notifications** | https://argocd-notifications.readthedocs.io/en/stable/ |
| **ApplicationSet** | https://argo-cd.readthedocs.io/en/stable/user-guide/application-set.yaml |
| **RBAC** | https://argo-cd.readthedocs.io/en/stable/operator-manual/rbac/ |
| **Metrics** | https://argo-cd.readthedocs.io/en/stable/operator-manual/metrics/ |
| **Architecture** | https://argo-cd.readthedocs.io/en/stable/operator-manual/architecture/ |

### Community and Support

| Resource | URL |
|----------|-----|
| **GitHub Repository** | https://github.com/argoproj/argo-cd |
| **GitHub Discussions** | https://github.com/argoproj/argo-cd/discussions |
| **Slack Channel** | https://argoproj.github.io/community/join-slack |
| **Community Meetings** | https://github.com/argoproj/argo-cd/wiki/Community-Meetings |
| **CNCF ArgoCD Project** | https://www.cncf.io/projects/argo/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **ArgoCD Tutorial** | https://argo-cd.readthedocs.io/en/stable/getting_started/ |
| **Example Applications** | https://github.com/argoproj/argocd-example-apps |
| **Best Practices** | https://argo-cd.readthedocs.io/en/stable/user-guide/best_practices/ |
| **Troubleshooting** | https://argo-cd.readthedocs.io/en/stable/faq/ |

### Related Tools

| Tool | URL |
|------|-----|
| **Argo Workflows** | https://argoproj.github.io/argo-workflows/ |
| **Argo Events** | https://argoproj.github.io/argo-events/ |
| **Argo Rollouts** | https://argoproj.github.io/argo-rollouts/ |
| **Flux CD** | https://fluxcd.io/ |

---

## Quick Reference: Common Commands

```bash
# Login
argocd login argocd.example.com

# Repository management
argocd repo add <repo-url>
argocd repo list
argocd repo rm <repo-url>

# Application management
argocd app create <name> --repo <url> --path <path> --dest-server <server> --dest-namespace <ns>
argocd app list
argocd app get <name>
argocd app delete <name>

# Synchronization
argocd app sync <name>
argocd app sync <name> --revision <revision>
argocd app wait <name>
argocd app rollback <name>

# Monitoring
argocd app logs <name>
argocd app diff <name>
argocd app history <name>

# Project management
argocd proj create <name>
argocd proj list
argocd proj delete <name>
```

---

**Last Updated:** 2024
**ArgoCD Version:** 2.x+
**License:** Apache 2.0

For the latest information, visit the official [ArgoCD Documentation](https://argo-cd.readthedocs.io/en/stable/)