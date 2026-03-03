# Flux CD Cheatsheet

![Flux Logo](https://fluxcd.io/img/flux-horizontal-color.png)

---

## Table of Contents
1. [What is Flux?](#1-what-is-flux)
2. [Core Concepts](#2-core-concepts)
3. [Installation](#3-installation)
4. [Git Repository Setup](#4-git-repository-setup)
5. [Source Configuration](#5-source-configuration)
6. [Kustomization](#6-kustomization)
7. [Helm Integration](#7-helm-integration)
8. [Multi-Cluster Setup](#8-multi-cluster-setup)
9. [Notifications](#9-notifications)
10. [Best Practices](#10-best-practices)
11. [Additional Resources](#11-additional-resources)

---

## 1. What is Flux?

Flux is a GitOps continuous delivery tool that automatically ensures your cluster state matches your Git repository. It synchronizes Kubernetes manifests from Git and manages Helm releases declaratively.

**Key Benefits:**
- Automated deployment from Git
- Declarative configuration management
- Multi-cluster support
- Helm chart management
- Built-in notifications and alerts

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Source** | Git repository or Helm repository |
| **GitRepository** | Configuration for syncing from Git |
| **HelmRepository** | Configuration for syncing Helm charts |
| **Kustomization** | Declarative patching and templating |
| **HelmRelease** | Helm chart deployment configuration |
| **Alert** | Notification configuration for events |
| **Provider** | External service for notifications (Slack, GitHub) |

---

## 3. Installation

### Install Flux CLI

```bash
# macOS
brew install fluxcd/tap/flux

# Linux
curl -s https://fluxcd.io/install.sh | sudo bash

# Verify installation
flux --version
```

### Bootstrap Flux on Cluster

```bash
# GitHub
flux bootstrap github \
  --owner=username \
  --repo=flux-config \
  --branch=main \
  --path=./clusters/production \
  --personal=true

# GitLab
flux bootstrap gitlab \
  --owner=username \
  --repo=flux-config \
  --branch=main \
  --path=./clusters/production

# Generic Git
flux bootstrap git \
  --url=ssh://git@github.com/username/flux-config.git \
  --branch=main \
  --path=./clusters/production
```

### Verify Installation

```bash
# Check Flux components
kubectl get pods -n flux-system

# Verify flux-system namespace
kubectl get all -n flux-system

# Check CRDs
kubectl get crd | grep fluxcd
```

---

## 4. Git Repository Setup

### GitRepository Resource

```yaml
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: flux-config
  namespace: flux-system
spec:
  interval: 1m
  url: https://github.com/username/flux-config.git
  ref:
    branch: main
  secretRef:
    name: flux-git-credentials
```

### Private Repository with SSH

```bash
# Generate SSH key
ssh-keygen -t ed25519 -f flux-ssh-key -N ""

# Create secret
kubectl create secret generic flux-git-credentials \
  --from-file=identity=flux-ssh-key \
  --from-file=identity.pub=flux-ssh-key.pub \
  --from-file=known_hosts=known_hosts \
  -n flux-system

# Add public key to GitHub
cat flux-ssh-key.pub
```

### Private Repository with HTTPS Token

```bash
# Create secret with token
kubectl create secret generic flux-git-credentials \
  --from-literal=username=git-user \
  --from-literal=password=git-token \
  -n flux-system
```

### CLI Commands

```bash
# Create GitRepository
flux create source git flux-config \
  --url=https://github.com/username/flux-config.git \
  --branch=main \
  --interval=1m \
  --export > gitrepository.yaml

# List sources
flux get sources git

# Reconcile source
flux reconcile source git flux-config

# Delete source
flux delete source git flux-config
```

---

## 5. Source Configuration

### Update Strategy

```yaml
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: my-repo
  namespace: flux-system
spec:
  interval: 5m
  url: https://github.com/username/repo.git
  ref:
    branch: main
  # Alternative: use tags or commits
  # ref:
  #   tag: v1.0.0
  # ref:
  #   commit: abc1234567890
```

### Commit Verification (GPG)

```yaml
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: signed-repo
  namespace: flux-system
spec:
  interval: 1m
  url: https://github.com/username/repo.git
  ref:
    branch: main
  verify:
    mode: HEAD
    secretRef:
      name: gpg-public-keys
```

### Ignore Paths

```yaml
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: my-repo
  namespace: flux-system
spec:
  interval: 1m
  url: https://github.com/username/repo.git
  ref:
    branch: main
  ignore: |
    *.md
    .git/
    docs/
    .circleci/
```

---

## 6. Kustomization

### Basic Kustomization

```yaml
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: my-app
  namespace: flux-system
spec:
  interval: 10m
  sourceRef:
    kind: GitRepository
    name: flux-config
  path: ./k8s/my-app
  prune: true
  wait: true
  timeout: 5m
```

### With Substitution

```yaml
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: my-app
  namespace: flux-system
spec:
  interval: 10m
  sourceRef:
    kind: GitRepository
    name: flux-config
  path: ./k8s/my-app
  postBuild:
    substituteFrom:
    - kind: ConfigMap
      name: cluster-vars
```

```yaml
# ConfigMap with variables
apiVersion: v1
kind: ConfigMap
metadata:
  name: cluster-vars
  namespace: flux-system
data:
  ENVIRONMENT: production
  DOMAIN: example.com
  REPLICAS: "3"
```

### Kustomization Dependencies

```yaml
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: app
  namespace: flux-system
spec:
  interval: 10m
  sourceRef:
    kind: GitRepository
    name: flux-config
  path: ./k8s/app
  dependsOn:
  - name: database
  - name: cache
```

### CLI Commands

```bash
# Create Kustomization
flux create kustomization my-app \
  --source=GitRepository/flux-config \
  --path=./k8s/my-app \
  --prune=true \
  --interval=10m \
  --export > kustomization.yaml

# List Kustomizations
flux get kustomizations

# Reconcile Kustomization
flux reconcile kustomization my-app

# Get status
flux get kustomizations -A
```

---

## 7. Helm Integration

### HelmRepository

```yaml
apiVersion: source.toolkit.fluxcd.io/v1beta2
kind: HelmRepository
metadata:
  name: bitnami
  namespace: flux-system
spec:
  interval: 1h
  url: https://charts.bitnami.com/bitnami
```

### HelmRelease Basic

```yaml
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: nginx
  namespace: default
spec:
  interval: 5m
  chart:
    spec:
      chart: nginx
      sourceRef:
        kind: HelmRepository
        name: bitnami
        namespace: flux-system
      version: "13.x"
  values:
    replicaCount: 3
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
```

### HelmRelease with Values File

```yaml
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: my-app
  namespace: production
spec:
  interval: 5m
  chart:
    spec:
      chart: my-chart
      sourceRef:
        kind: HelmRepository
        name: my-repo
      version: ">=1.0.0"
  values:
    image:
      tag: v1.0.0
    autoscaling:
      enabled: true
      minReplicas: 2
      maxReplicas: 10
  valuesFrom:
  - kind: ConfigMap
    name: helm-values
```

### HelmRelease Upgrade Strategy

```yaml
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: my-app
  namespace: production
spec:
  interval: 5m
  chart:
    spec:
      chart: my-chart
      sourceRef:
        kind: HelmRepository
        name: my-repo
  install:
    remediation:
      retries: 3
  upgrade:
    remediation:
      retries: 3
  rollback:
    recreate: true
    cleanupOnFail: true
```

### CLI Commands

```bash
# Add Helm repository
flux create source helm bitnami \
  --url=https://charts.bitnami.com/bitnami \
  --interval=1h \
  --export > helmrepository.yaml

# Create HelmRelease
flux create helmrelease nginx \
  --source=HelmRepository/bitnami \
  --chart=nginx \
  --version="13.x" \
  --interval=5m \
  --export > helmrelease.yaml

# Get HelmReleases
flux get helmreleases -A

# Reconcile HelmRelease
flux reconcile helmrelease nginx
```

---

## 8. Multi-Cluster Setup

### Cluster Structure

```
flux-config/
├── clusters/
│   ├── production/
│   │   ├── flux-system/
│   │   │   └── gotk-components.yaml
│   │   └── apps/
│   │       ├── kustomization.yaml
│   │       └── my-app/
│   ├── staging/
│   │   ├── flux-system/
│   │   │   └── gotk-components.yaml
│   │   └── apps/
│   └── development/
└── shared/
    └── base/
```

### Multi-Cluster Kustomization

```yaml
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: apps
  namespace: flux-system
spec:
  interval: 10m
  sourceRef:
    kind: GitRepository
    name: flux-config
  path: ./apps
  patches:
  - target:
      kind: Deployment
    patch: |-
      - op: replace
        path: /spec/replicas
        value: 1
```

### Cross-Cluster Secrets

```bash
# Create secret in one cluster
kubectl create secret generic app-secret \
  --from-literal=api-key=secret-value \
  -n production

# Reference in HelmRelease
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: my-app
spec:
  values:
    secretRef: app-secret
```

---

## 9. Notifications

### Alert Configuration

```yaml
apiVersion: notification.toolkit.fluxcd.io/v1beta1
kind: Alert
metadata:
  name: on-deployment-failure
  namespace: flux-system
spec:
  providerRef:
    name: slack
  eventSeverity: error
  eventSources:
  - kind: Kustomization
    name: '*'
  - kind: HelmRelease
    name: '*'
  suspend: false
```

### Slack Provider

```yaml
apiVersion: notification.toolkit.fluxcd.io/v1beta1
kind: Provider
metadata:
  name: slack
  namespace: flux-system
spec:
  type: slack
  channel: '#deployments'
  address: https://hooks.slack.com/services/YOUR/WEBHOOK/URL
```

### GitHub Provider

```yaml
apiVersion: notification.toolkit.fluxcd.io/v1beta1
kind: Provider
metadata:
  name: github
  namespace: flux-system
spec:
  type: github
  address: https://github.com/username/repo
  secretRef:
    name: github-token
```

### CLI Commands

```bash
# Create Alert
flux create alert github-alert \
  --provider-ref=github \
  --event-sources=Kustomization/* \
  --event-severity=error \
  --export > alert.yaml

# Get Alerts
flux get alerts

# Suspend Alert
flux suspend alert github-alert
```

---

## 10. Best Practices

### Repository Structure

```
repo/
├── clusters/
│   └── production/
│       ├── flux-system/
│       │   ├── gotk-components.yaml
│       │   ├── gotk-sync.yaml
│       │   └── kustomization.yaml
│       └── apps/
│           ├── kustomization.yaml
│           └── my-app/
├── apps/
│   └── my-app/
│       ├── base/
│       │   ├── deployment.yaml
│       │   ├── service.yaml
│       │   └── kustomization.yaml
│       └── overlays/
│           ├── dev/
│           └── prod/
└── README.md
```

### Version Management

```yaml
# Pin specific versions
spec:
  chart:
    spec:
      version: "1.2.3"  # Exact version
      # version: "1.2.x"   # Patch updates
      # version: ">=1.2.0" # Semantic versioning
```

### Sync Strategy

```yaml
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: my-app
spec:
  interval: 10m
  prune: true      # Remove resources deleted from Git
  wait: true       # Wait for resources to be ready
  timeout: 5m      # Timeout for sync operation
```

### Secrets Management

```bash
# Create sealed secret
echo -n "my-secret" | kubectl create secret generic my-secret \
  --dry-run=client \
  --from-file=/dev/stdin \
  -o yaml | kubeseal -o yaml > sealedsecret.yaml

# Reference in HelmRelease
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: my-app
spec:
  valuesFrom:
  - kind: Secret
    name: my-secret
```

### Health Checks

```yaml
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: my-app
spec:
  wait: true
  healthChecks:
  - apiVersion: apps/v1
    kind: Deployment
    name: my-app
    namespace: default
```

### GitOps Workflow

```bash
# 1. Make changes locally
git clone https://github.com/username/flux-config.git
cd flux-config

# 2. Update manifests
vim clusters/production/apps/my-app/values.yaml

# 3. Commit and push
git add .
git commit -m "Update app version"
git push origin main

# 4. Flux automatically syncs (within interval)
# 5. Monitor deployment
flux get kustomizations --watch
```

---

## 11. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://fluxcd.io/flux/get-started/ |
| **Installation** | https://fluxcd.io/flux/installation/ |
| **Git Sync** | https://fluxcd.io/flux/components/source/gitrepository/ |
| **Helm Integration** | https://fluxcd.io/flux/components/helm/ |
| **Kustomization** | https://fluxcd.io/flux/components/kustomize/kustomization/ |
| **Notifications** | https://fluxcd.io/flux/components/notification/ |
| **CLI Reference** | https://fluxcd.io/flux/cmd/ |
| **Best Practices** | https://fluxcd.io/flux/guides/best-practices/ |
| **Troubleshooting** | https://fluxcd.io/flux/troubleshooting/ |
| **FAQ** | https://fluxcd.io/flux/faq/ |

### Community and Support

| Resource | URL |
|----------|-----|
| **GitHub Repository** | https://github.com/fluxcd/flux2 |
| **GitHub Discussions** | https://github.com/fluxcd/flux2/discussions |
| **Slack Channel** | https://app.slack.com/client/T08PSQ7BQ/C02D91A0W0P |
| **CNCF Flux Project** | https://www.cncf.io/projects/flux/ |
| **Community Events** | https://fluxcd.io/community/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Tutorial** | https://fluxcd.io/flux/get-started/ |
| **Examples** | https://github.com/fluxcd/flux2-kustomize-helm-example |
| **Multi-Tenancy Guide** | https://fluxcd.io/flux/guides/multi-tenancy/ |
| **Sealed Secrets** | https://fluxcd.io/flux/guides/sealed-secrets/ |

### Quick Reference Commands

```bash
# Bootstrap
flux bootstrap github --owner=<org> --repo=<repo> --personal=true

# Git Repository
flux create source git my-repo --url=<url> --branch=<branch>
flux get sources git
flux reconcile source git my-repo

# Kustomization
flux create kustomization my-app --source=<source> --path=<path>
flux get kustomizations
flux reconcile kustomization my-app

# Helm
flux create source helm my-helm --url=<url>
flux create helmrelease my-release --source=<source> --chart=<chart>
flux get helmreleases -A

# Notifications
flux create alert my-alert --provider-ref=<provider> --event-sources=<sources>
flux get alerts

# Status
flux get all
flux logs --all-namespaces
```

---

**Last Updated:** 2024
**Flux Version:** 2.x+
**License:** Apache 2.0

For latest information, visit [Flux Documentation](https://fluxcd.io/)