# GitHub Actions Cheatsheet

![GitHub Actions Logo](https://github.githubassets.com/images/modules/site/features/actions-icon-actions.svg)

---

## Table of Contents
1. [What is GitHub Actions?](#1-what-is-github-actions)
2. [Core Concepts](#2-core-concepts)
3. [Getting Started](#3-getting-started)
4. [Workflow Syntax](#4-workflow-syntax)
5. [Jobs and Steps](#5-jobs-and-steps)
6. [Runners](#6-runners)
7. [Variables and Secrets](#7-variables-and-secrets)
8. [Artifacts and Caching](#8-artifacts-and-caching)
9. [Advanced Features](#9-advanced-features)
10. [Best Practices](#10-best-practices)
11. [Additional Resources](#11-additional-resources)

---

## 1. What is GitHub Actions?

GitHub Actions is GitHub's built-in continuous integration and continuous deployment service. It automates workflows directly from your repository using YAML configuration files in the `.github/workflows/` directory.

**Key Benefits:**
- Native GitHub integration
- Free tier with 2000 minutes/month
- Community-driven actions marketplace
- Matrix builds for multiple configurations
- Self-hosted and cloud runners

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Workflow** | Automated process defined in YAML file |
| **Event** | Trigger that starts workflow (push, PR, schedule) |
| **Job** | Set of steps that execute on same runner |
| **Step** | Individual task within a job |
| **Action** | Reusable unit of code for common tasks |
| **Runner** | Server that executes workflow jobs |
| **Artifact** | Files produced by workflow for download |
| **Secret** | Encrypted variable for sensitive data |

---

## 3. Getting Started

### Create Workflow File

```bash
# Create directory
mkdir -p .github/workflows

# Create workflow file
touch .github/workflows/ci.yml
```

### Basic Workflow

```yaml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - uses: actions/setup-node@v3
        with:
          node-version: '16'
      
      - run: npm install
      - run: npm run build
      - run: npm test
```

### View Workflow Status

```bash
# In GitHub UI: Actions tab
# Or use GitHub CLI:
gh run list
gh run view <run-id>
```

---

## 4. Workflow Syntax

### Complete Workflow Example

```yaml
name: Build and Deploy

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 2 * * *'
  workflow_dispatch:

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  build:
    runs-on: ubuntu-latest
    
    permissions:
      contents: read
      packages: write
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build
        run: npm run build
      
      - name: Run tests
        run: npm test
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage/cobertura-coverage.xml

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to production
        run: npm run deploy
        env:
          DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
```

### Event Triggers

```yaml
# On push to specific branches
on:
  push:
    branches: [main, develop]
    paths:
      - 'src/**'
      - 'package.json'

# On pull request
on:
  pull_request:
    branches: [main]
    types: [opened, synchronize, reopened]

# On schedule (cron)
on:
  schedule:
    - cron: '0 2 * * *'  # Daily at 2 AM UTC

# Manual trigger
on:
  workflow_dispatch:
    inputs:
      environment:
        type: choice
        options: [dev, staging, prod]
        default: dev

# Multiple events
on: [push, pull_request, workflow_dispatch]
```

---

## 5. Jobs and Steps

### Sequential Jobs

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm run build

  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm test

  deploy:
    needs: [build, test]
    runs-on: ubuntu-latest
    steps:
      - run: npm run deploy
```

### Parallel Jobs

```yaml
jobs:
  unit-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm run test:unit

  integration-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm run test:integration

  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm run lint

# All three jobs run in parallel
```

### Matrix Builds

```yaml
jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node-version: [14, 16, 18]
    
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm test
      # 9 jobs total (3 OS × 3 versions)
```

### Conditional Execution

```yaml
steps:
  - name: Deploy to production
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'
    run: npm run deploy:prod

  - name: Run on success
    if: success()
    run: echo "Build succeeded"

  - name: Run on failure
    if: failure()
    run: echo "Build failed"

  - name: Always run
    if: always()
    run: echo "This always runs"
```

### Continue on Error

```yaml
steps:
  - name: Lint
    continue-on-error: true
    run: npm run lint

  - name: Build (fails if lint fails)
    run: npm run build
```

---

## 6. Runners

### GitHub-Hosted Runners

```yaml
jobs:
  build:
    runs-on: ubuntu-latest    # Linux
    # runs-on: windows-latest  # Windows
    # runs-on: macos-latest    # macOS
    steps:
      - run: echo "Running on ${{ runner.os }}"
```

### Self-Hosted Runner

```yaml
jobs:
  build:
    runs-on: self-hosted
    steps:
      - uses: actions/checkout@v3
      - run: npm run build

# Register runner:
# Settings → Actions → Runners → New self-hosted runner
```

### Custom Runner Labels

```yaml
jobs:
  build:
    runs-on: [self-hosted, linux, x64, gpu]
    steps:
      - run: nvidia-smi
```

---

## 7. Variables and Secrets

### Default Variables

```yaml
steps:
  - name: Display variables
    run: |
      echo "Repository: ${{ github.repository }}"
      echo "Branch: ${{ github.ref }}"
      echo "Event: ${{ github.event_name }}"
      echo "Actor: ${{ github.actor }}"
      echo "Commit: ${{ github.sha }}"
      echo "Job: ${{ github.job }}"
      echo "Workspace: ${{ github.workspace }}"
```

### Environment Variables

```yaml
env:
  GLOBAL_VAR: "global value"

jobs:
  build:
    env:
      JOB_VAR: "job value"
    steps:
      - name: Display vars
        env:
          STEP_VAR: "step value"
        run: |
          echo $GLOBAL_VAR
          echo $JOB_VAR
          echo $STEP_VAR
```

### Secrets

```yaml
# Define in repository Settings → Secrets and variables → Actions

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy
        run: npm run deploy
        env:
          API_KEY: ${{ secrets.API_KEY }}
          DB_PASSWORD: ${{ secrets.DB_PASSWORD }}

      - name: Upload artifact
        uses: actions/upload-artifact@v3
        with:
          name: build
          path: dist/
```

### Contexts

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Display context
        run: |
          echo "GitHub context: ${{ toJson(github) }}"
          echo "Env context: ${{ toJson(env) }}"
          echo "Runner context: ${{ toJson(runner) }}"
```

---

## 8. Artifacts and Caching

### Upload Artifacts

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm run build
      
      - name: Upload artifacts
        uses: actions/upload-artifact@v3
        with:
          name: dist
          path: dist/
          retention-days: 5

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Download artifacts
        uses: actions/download-artifact@v3
        with:
          name: dist
      
      - run: npm run deploy
```

### Cache Dependencies

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - uses: actions/setup-node@v3
        with:
          node-version: '16'
          cache: 'npm'
      
      - run: npm ci
      - run: npm run build

# Automatic caching with setup-node
# Or manual caching:

      - uses: actions/cache@v3
        with:
          path: node_modules
          key: ${{ runner.os }}-npm-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-npm-
```

---

## 9. Advanced Features

### Reusable Workflows

```yaml
# .github/workflows/reusable.yml
name: Reusable Build

on:
  workflow_call:
    inputs:
      node-version:
        required: true
        type: string
    secrets:
      npm-token:
        required: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ inputs.node-version }}
      - run: npm run build

# .github/workflows/main.yml
name: Main Workflow

on: [push]

jobs:
  call-build:
    uses: ./.github/workflows/reusable.yml
    with:
      node-version: '16'
    secrets:
      npm-token: ${{ secrets.NPM_TOKEN }}
```

### Docker Container Action

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    container:
      image: node:16-alpine
      options: --cpus 1 --memory 4g
    steps:
      - uses: actions/checkout@v3
      - run: npm run build
```

### Service Containers

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:13
        env:
          POSTGRES_DB: test_db
          POSTGRES_USER: user
          POSTGRES_PASSWORD: password
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432
    steps:
      - uses: actions/checkout@v3
      - run: npm test
```

### Create Issue on Failure

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm run build
      
      - name: Create issue on failure
        if: failure()
        uses: actions/github-script@v6
        with:
          script: |
            github.rest.issues.create({
              owner: context.repo.owner,
              repo: context.repo.repo,
              title: 'Build failed',
              body: 'Build failed in ${{ github.run_number }}'
            })
```

---

## 10. Best Practices

### Use Action Versions

```yaml
# ✓ Good - Specific version
- uses: actions/setup-node@v3

# ✗ Avoid - Latest or no version
- uses: actions/setup-node@latest
```

### Organize Workflows

```yaml
name: Build, Test, Deploy

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  NODE_VERSION: '16'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'
      - run: npm ci && npm run build

  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'
      - run: npm ci && npm test

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3
      - run: npm run deploy
        env:
          DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
```

### Use Caching Effectively

```yaml
- uses: actions/cache@v3
  with:
    path: |
      node_modules
      ~/.cache
    key: ${{ runner.os }}-${{ hashFiles('**/package-lock.json') }}
    restore-keys: ${{ runner.os }}-
```

### Secure Secrets

```yaml
# Never log secrets
- name: Login
  run: npm login --registry=${{ secrets.NPM_REGISTRY }}
  # Secret automatically masked in logs

# Use short-lived tokens
# Rotate secrets regularly
```

### Use Marketplace Actions

```yaml
# Pre-built actions from marketplace
- uses: actions/setup-node@v3      # Setup tools
- uses: codecov/codecov-action@v3  # Upload coverage
- uses: docker/build-push-action@v4 # Build Docker image
- uses: actions/github-script@v6   # Run scripts
```

---

## 11. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://docs.github.com/en/actions/quickstart |
| **Workflow Syntax** | https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions |
| **Events** | https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows |
| **Context & Expressions** | https://docs.github.com/en/actions/learn-github-actions/contexts |
| **Variables** | https://docs.github.com/en/actions/learn-github-actions/variables |
| **Encrypted Secrets** | https://docs.github.com/en/actions/security-guides/encrypted-secrets |
| **Runners** | https://docs.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners |
| **Artifacts** | https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts |
| **Caching** | https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows |

### Community and Support

| Resource | URL |
|----------|-----|
| **GitHub Discussions** | https://github.com/orgs/github/discussions |
| **Actions Repository** | https://github.com/actions |
| **Community Actions** | https://github.com/marketplace?type=actions |
| **Status Page** | https://www.githubstatus.com/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Tutorial** | https://docs.github.com/en/actions/learn-github-actions |
| **Examples** | https://github.com/actions/starter-workflows |
| **Best Practices** | https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions |
| **Advanced** | https://docs.github.com/en/actions/using-workflows/reusing-workflows |

### Popular Actions

| Action | URL |
|--------|-----|
| **Checkout** | https://github.com/actions/checkout |
| **Setup Node** | https://github.com/actions/setup-node |
| **Upload Artifact** | https://github.com/actions/upload-artifact |
| **Deploy Pages** | https://github.com/actions/deploy-pages |

---

## Quick Reference

```yaml
name: CI/CD

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '16'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
      - run: npm test
      - uses: actions/upload-artifact@v3
        if: always()
        with:
          name: dist
          path: dist/
```

---

**Last Updated:** 2026
**GitHub Actions Version:** Latest
**License:** MIT

For latest information, visit [GitHub Actions Documentation](https://docs.github.com/en/actions)