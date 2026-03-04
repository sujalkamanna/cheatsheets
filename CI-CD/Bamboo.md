# Bamboo CI/CD Cheatsheet

![Bamboo Logo](https://wac-cdn.atlassian.com/dam/jcr:5ea5a0a1-c46c-40d1-b48a-50aa47f2a889/bamboo-icon-gradient-blue.svg)

---

## Table of Contents
1. [What is Bamboo?](#1-what-is-bamboo)
2. [Core Concepts](#2-core-concepts)
3. [Getting Started](#3-getting-started)
4. [Plan Configuration](#4-plan-configuration)
5. [Jobs and Tasks](#5-jobs-and-tasks)
6. [Agents and Capabilities](#6-agents-and-capabilities)
7. [Variables and Secrets](#7-variables-and-secrets)
8. [Artifacts and Testing](#8-artifacts-and-testing)
9. [Deployment Projects](#9-deployment-projects)
10. [Best Practices](#10-best-practices)
11. [Additional Resources](#11-additional-resources)

---

## 1. What is Bamboo?

Bamboo is a continuous integration and continuous deployment server by Atlassian. It automates parts of software development like compilation, testing, and deployment, with seamless integration with Jira and Bitbucket.

**Key Benefits:**
- Integrated with Jira and Bitbucket
- Agent-based distributed builds
- Deployment projects for release management
- Visual plan configuration
- Flexible task-based architecture

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Plan** | Build configuration containing jobs and tasks |
| **Job** | Collection of tasks executed sequentially |
| **Task** | Individual action (compile, test, deploy) |
| **Stage** | Logical grouping of jobs (optional) |
| **Agent** | Machine that executes build jobs |
| **Repository** | Source code connection (Git, SVN, etc.) |
| **Artifact** | Output files from build for download |
| **Deployment Project** | Release pipeline for environments |

---

## 3. Getting Started

### Install Bamboo

```bash
# Download Bamboo
wget https://www.atlassian.com/software/bamboo/downloads/binary/atlassian-bamboo-9.0.0.tar.gz

# Extract
tar -xzf atlassian-bamboo-9.0.0.tar.gz

# Start
./atlassian-bamboo-9.0.0/bin/start-bamboo.sh

# Access at http://localhost:8085
```

### Docker Installation

```bash
docker run -d --name bamboo \
  -p 8085:8085 \
  -p 7990:7990 \
  -v bamboo_home:/var/atlassian/application-data/bamboo \
  atlassian/bamboo:latest

# Access at http://localhost:8085
```

### Initial Setup

1. Navigate to http://localhost:8085
2. Follow setup wizard
3. Configure license
4. Set up database
5. Create admin account

---

## 4. Plan Configuration

### Basic Plan YAML

```yaml
version: 2
name: My App CI
key: MYAPP
description: Continuous integration for My App

repositories:
  - name: my-repo
    type: bitbucket
    url: https://bitbucket.org/username/my-repo.git
    branch: main

triggers:
  - type: bitbucket
    name: Repository trigger

stages:
  - name: Build Stage
    manual: false
    jobs:
      - name: Build Job
        key: BUILD
        tasks:
          - type: checkout
            name: Checkout
            repository: my-repo
          
          - type: script
            name: Install dependencies
            script: npm install
          
          - type: script
            name: Build
            script: npm run build
          
          - type: script
            name: Run tests
            script: npm test

  - name: Deploy Stage
    manual: true
    jobs:
      - name: Deploy Job
        key: DEPLOY
        tasks:
          - type: checkout
            name: Checkout
            repository: my-repo
          
          - type: script
            name: Deploy
            script: npm run deploy
```

### Plan via UI

```
Bamboo → Create → Create a new plan
├── Plan name: My App CI
├── Project: MY
├── Repository: my-repo (Bitbucket)
├── Trigger: Repository push
└── Jobs and Tasks
    ├── Job 1: Build
    │   ├── Task 1: Checkout
    │   ├── Task 2: Install
    │   └── Task 3: Build
    └── Job 2: Test
        ├── Task 1: Checkout
        └── Task 2: Test
```

---

## 5. Jobs and Tasks

### Multiple Jobs

```yaml
stages:
  - name: Build
    jobs:
      - name: Compile
        key: COMPILE
        tasks:
          - type: script
            script: npm run build

      - name: Unit Tests
        key: TEST
        tasks:
          - type: script
            script: npm run test:unit

      - name: Integration Tests
        key: INTEGRATION
        tasks:
          - type: script
            script: npm run test:integration
```

### Job Dependencies

```yaml
jobs:
  - name: Build
    key: BUILD
    tasks:
      - type: script
        script: npm run build

  - name: Deploy
    key: DEPLOY
    requires:
      - BUILD  # Wait for BUILD job
    tasks:
      - type: script
        script: npm run deploy
```

### Common Task Types

```yaml
tasks:
  # Script task
  - type: script
    name: Run build
    script: npm run build

  # Checkout
  - type: checkout
    name: Checkout source
    repository: my-repo

  # Maven
  - type: maven3
    name: Maven build
    goal: clean install

  # Docker
  - type: script
    script: docker build -t myapp:latest .
    script: docker push myapp:latest

  # Test parsing
  - type: test
    name: Parse test results
    testResultsPattern: '**/target/surefire-reports/*.xml'

  # Inject variables
  - type: inject-variables
    name: Inject build info
    file: build.properties
```

### Job Variables

```yaml
jobs:
  - name: Build
    key: BUILD
    variables:
      ENVIRONMENT: production
      LOG_LEVEL: debug
    tasks:
      - type: script
        script: |
          echo "Environment: $ENVIRONMENT"
          npm run build -- --env=$ENVIRONMENT
```

---

## 6. Agents and Capabilities

### Local Agent

```bash
# Bamboo server runs builds locally
# Navigate to Agents section in UI
# Enable/disable as needed
```

### Remote Agents

```bash
# Download agent
wget https://www.atlassian.com/software/bamboo/downloads/binary/atlassian-bamboo-agent-installer-9.0.0.jar

# Install
java -jar atlassian-bamboo-agent-installer-9.0.0.jar https://bamboo-server:8085/

# Start
cd atlassian-bamboo-agent-home
./bin/start-agent.sh
```

### Agent Capabilities

```yaml
# Agent automatically detects capabilities
# Or manually configure in agent setup

# View agent capabilities:
# Agents → Agent Details → Capabilities

# Examples:
# - Java 11
# - Node.js 16
# - Docker
# - Maven 3.8
# - Python 3.9
```

### Job Requirements

```yaml
jobs:
  - name: Build
    key: BUILD
    requirements:
      - type: jdk
        version: "11"
      - type: nodejs
        version: "16"
      - type: docker
    tasks:
      - type: script
        script: java -version && node -v && docker -v
```

---

## 7. Variables and Secrets

### Plan Variables

```yaml
variables:
  REGISTRY: docker.io
  IMAGE_NAME: myapp
  NODE_ENV: production

jobs:
  - name: Build
    variables:
      BUILD_NUMBER: ${bamboo.buildNumber}
      COMMIT_SHA: ${bamboo.repository.revision}
    tasks:
      - type: script
        script: |
          echo "Build: ${BUILD_NUMBER}"
          echo "Commit: ${COMMIT_SHA}"
          npm run build
```

### Global Variables

```
Bamboo UI → Settings → Global Variables
├── Variable name: DOCKER_REGISTRY
├── Variable value: docker.io
└── Shared: Yes
```

### Secrets (Secured Variables)

```yaml
# In Bamboo UI: Administration → Security → Secured Variables

# Usage in plan
tasks:
  - type: script
    script: |
      docker login -u ${bamboo.docker_username} -p ${bamboo.docker_password}
      docker push ${REGISTRY}/${IMAGE_NAME}:latest
```

### Built-in Variables

```yaml
tasks:
  - type: script
    script: |
      echo "Build number: ${bamboo.buildNumber}"
      echo "Plan key: ${bamboo.planKey}"
      echo "Job key: ${bamboo.jobKey}"
      echo "Build dir: ${bamboo.build.working.directory}"
      echo "Artifact dir: ${bamboo.build.dir}"
      echo "Source dir: ${bamboo.checkout.dir}"
      echo "Branch: ${bamboo.repository.branch.name}"
```

---

## 8. Artifacts and Testing

### Publish Artifacts

```yaml
jobs:
  - name: Build
    key: BUILD
    tasks:
      - type: script
        script: npm run build

artifact-definitions:
  - name: Build Output
    pattern: dist/**/*
    shared: true
    required: false

  - name: Reports
    pattern: reports/**/*.xml
    shared: true
```

### Download Artifacts

```yaml
jobs:
  - name: Deploy
    key: DEPLOY
    requires:
      - BUILD
    tasks:
      - type: script
        script: |
          # Artifacts from BUILD job auto-downloaded
          ls dist/
          npm run deploy
```

### Publish Test Results

```yaml
jobs:
  - name: Test
    key: TEST
    tasks:
      - type: script
        script: npm test -- --reporters=junit

test-result-parsers:
  - type: junit
    pattern: '**/reports/**/*.xml'
    
  - type: mocha
    pattern: '**/test-results.json'
```

### Publish Code Coverage

```yaml
jobs:
  - name: Coverage
    key: COVERAGE
    tasks:
      - type: script
        script: npm run coverage

artifact-definitions:
  - name: Coverage Report
    pattern: coverage/**/*
    shared: true
```

---

## 9. Deployment Projects

### Create Deployment Project

```
Bamboo → Create → Create a new deployment project
├── Name: My App Deployment
├── Plan: MYAPP-BUILD
└── Environments
    ├── Development
    ├── Staging
    └── Production
```

### Deployment Configuration

```yaml
deployment:
  environments:
    - name: Development
      triggers:
        - type: automatic
          plan: MYAPP-BUILD
      tasks:
        - type: checkout
        - type: script
          script: npm run deploy:dev

    - name: Staging
      triggers:
        - type: manual
      tasks:
        - type: checkout
        - type: script
          script: npm run deploy:staging

    - name: Production
      triggers:
        - type: manual
        - name: approval
          approvers: admin
      tasks:
        - type: checkout
        - type: script
          script: npm run deploy:prod
```

### Deployment Release Notes

```
Deployment UI → Release Management
├── View release history
├── Add release notes
├── Track deployment status
└── Rollback to previous version
```

---

## 10. Best Practices

### Organize Plans

```yaml
version: 2
name: My App CI
key: MYAPP

triggers:
  - type: bitbucket

stages:
  - name: Build & Test
    jobs:
      - name: Build
        key: BUILD
        tasks:
          - type: checkout
          - type: script
            script: npm ci && npm run build

      - name: Test
        key: TEST
        tasks:
          - type: script
            script: npm test

  - name: Security
    jobs:
      - name: Security Scan
        key: SECURITY
        tasks:
          - type: script
            script: npm audit
```

### Use Templates

```yaml
# Common tasks as reusable templates
# Create templates in Bamboo UI
# Settings → Bamboo Specifications → Shared Specifications

# Usage:
jobs:
  - name: Build
    key: BUILD
    shared-specs:
      - node-build-spec
    tasks:
      - type: script
        script: npm run build
```

### Error Handling

```yaml
tasks:
  - type: script
    script: npm run build
    
  - type: script
    script: npm test
    ignore-errors: false  # Fail if test fails

  - type: script
    script: npm run deploy
    if-passed: true  # Only run if previous passed
```

### Agent Configuration

```yaml
jobs:
  - name: Docker Build
    key: DOCKER
    requirements:
      - type: docker
    tasks:
      - type: script
        script: |
          docker build -t myapp:${bamboo.buildNumber} .
          docker push myapp:${bamboo.buildNumber}

  - name: Deploy
    key: DEPLOY
    requirements:
      - type: kubernetes
    tasks:
      - type: script
        script: kubectl apply -f deployment.yaml
```

### Notifications

```
Plan → Notifications
├── Failed build notification
├── Email: team@example.com
├── Jira: Create issue
└── Slack: Send message
```

---

## 11. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://confluence.atlassian.com/bamboo/getting-started-289276866.html |
| **Plan Configuration** | https://confluence.atlassian.com/bamboo/plan-configuration-289276964.html |
| **Tasks** | https://confluence.atlassian.com/bamboo/tasks-289276891.html |
| **Variables** | https://confluence.atlassian.com/bamboo/bamboo-variables-289276798.html |
| **Agents** | https://confluence.atlassian.com/bamboo/agents-289276853.html |
| **Deployment** | https://confluence.atlassian.com/bamboo/deployment-projects-289276904.html |
| **REST API** | https://docs.atlassian.com/bamboo/latest/Bamboo+REST+APIs |
| **YAML Specs** | https://confluence.atlassian.com/bamboo/bamboo-specs-289276983.html |

### Community and Support

| Resource | URL |
|----------|-----|
| **Community Forum** | https://community.atlassian.com/t5/Bamboo/ct-p/bamboo |
| **Issue Tracker** | https://jira.atlassian.com/browse/BAM |
| **Support Portal** | https://support.atlassian.com/bamboo/ |
| **Atlassian Marketplace** | https://marketplace.atlassian.com/apps/search?product=bamboo |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Tutorial** | https://confluence.atlassian.com/bamboo/getting-started-289276866.html |
| **Administration** | https://confluence.atlassian.com/bamboo/administration-289277015.html |
| **API Docs** | https://docs.atlassian.com/bamboo/latest/Bamboo+REST+APIs |
| **Atlassian University** | https://university.atlassian.com/ |

### Related Tools

| Tool | URL |
|------|-----|
| **Bitbucket** | https://bitbucket.org/ |
| **Jira** | https://www.atlassian.com/software/jira |
| **Confluence** | https://www.atlassian.com/software/confluence |
| **OpsGenie** | https://www.atlassian.com/software/opsgenie |

---

## Quick Reference

```yaml
version: 2
name: My App CI
key: MYAPP

repositories:
  - name: my-repo
    type: bitbucket
    url: https://bitbucket.org/username/my-repo.git

triggers:
  - type: bitbucket

stages:
  - name: Build
    jobs:
      - name: Build Job
        key: BUILD
        tasks:
          - type: checkout
            repository: my-repo
          - type: script
            script: npm install && npm run build
          - type: script
            script: npm test

artifact-definitions:
  - name: Build Output
    pattern: dist/**/*

test-result-parsers:
  - type: junit
    pattern: '**/reports/**/*.xml'
```

---

**Last Updated:** 2026
**Bamboo Version:** 9.x+
**License:** Commercial/Free Trial

For latest information, visit [Bamboo Documentation](https://confluence.atlassian.com/bamboo/)