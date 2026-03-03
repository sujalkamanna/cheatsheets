# GitLab CI/CD Cheatsheet

![GitLab Logo](https://about.gitlab.com/images/press/logo/png/gitlab-icon-rgb.png)

---

## Table of Contents
1. [What is GitLab CI?](#1-what-is-gitlab-ci)
2. [Core Concepts](#2-core-concepts)
3. [Getting Started](#3-getting-started)
4. [Pipeline Configuration](#4-pipeline-configuration)
5. [Jobs and Stages](#5-jobs-and-stages)
6. [Runners](#6-runners)
7. [Variables and Secrets](#7-variables-and-secrets)
8. [Artifacts and Cache](#8-artifacts-and-cache)
9. [Advanced Features](#9-advanced-features)
10. [Best Practices](#10-best-practices)
11. [Additional Resources](#11-additional-resources)

---

## 1. What is GitLab CI?

GitLab CI is a built-in continuous integration and continuous deployment service that works seamlessly with GitLab repositories. It uses `.gitlab-ci.yml` files to define pipelines that automatically run when code is pushed.

**Key Benefits:**
- Native GitLab integration
- No external CI/CD tool needed
- Container-native with Docker support
- Built-in artifact management
- Scalable runner architecture

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Pipeline** | Complete workflow defined in `.gitlab-ci.yml` |
| **Stage** | Logical grouping of jobs (build, test, deploy) |
| **Job** | Single task/script executed by a runner |
| **Runner** | Agent that executes pipeline jobs |
| **Artifact** | Files produced by jobs for download/use |
| **Cache** | Persistent storage between job runs |
| **Environment** | Deployment target (dev, staging, prod) |
| **Trigger** | Event that starts pipeline execution |

---

## 3. Getting Started

### Enable GitLab CI

```bash
# Create .gitlab-ci.yml in repository root
touch .gitlab-ci.yml

# GitLab automatically detects and runs CI/CD pipeline
```

### Basic Pipeline Structure

```yaml
# .gitlab-ci.yml

stages:
  - build
  - test
  - deploy

variables:
  NODE_ENV: production

build_job:
  stage: build
  script:
    - npm install
    - npm run build
  artifacts:
    paths:
      - dist/

test_job:
  stage: test
  script:
    - npm test

deploy_job:
  stage: deploy
  script:
    - npm run deploy
  only:
    - main
```

### View Pipeline Status

```bash
# In GitLab UI: Project → CI/CD → Pipelines
# Or use GitLab CLI:
gitlab pipeline list
gitlab pipeline get <pipeline-id>
```

---

## 4. Pipeline Configuration

### Complete Pipeline Example

```yaml
stages:
  - build
  - test
  - security
  - deploy

# Global variables
variables:
  REGISTRY: registry.gitlab.com
  IMAGE_NAME: $CI_REGISTRY_IMAGE
  IMAGE_TAG: $CI_COMMIT_SHA

# Default configuration for all jobs
default:
  image: node:16-alpine
  retry:
    max: 2
    when:
      - runner_system_failure
      - stuck_or_timeout_failure
  timeout: 30m

build:
  stage: build
  image: node:16-alpine
  script:
    - npm ci
    - npm run build
  artifacts:
    paths:
      - dist/
      - node_modules/
    expire_in: 1 hour
  cache:
    key: npm-$CI_COMMIT_REF_SLUG
    paths:
      - node_modules/

unit_test:
  stage: test
  script:
    - npm test
  coverage: '/Lines\s*:\s*(\d+.\d+)%/'
  artifacts:
    reports:
      junit: reports/junit.xml
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml

integration_test:
  stage: test
  services:
    - postgres:13
  variables:
    POSTGRES_DB: test_db
    POSTGRES_USER: user
    POSTGRES_PASSWORD: password
  script:
    - npm run test:integration

sonar_scan:
  stage: security
  image: sonarsource/sonar-scanner-cli:latest
  script:
    - sonar-scanner
      -Dsonar.projectKey=my-app
      -Dsonar.sources=src
      -Dsonar.host.url=$SONAR_HOST_URL
      -Dsonar.login=$SONAR_TOKEN

deploy_staging:
  stage: deploy
  environment:
    name: staging
    url: https://staging.example.com
  script:
    - npm run deploy:staging
  only:
    - develop

deploy_production:
  stage: deploy
  environment:
    name: production
    url: https://example.com
  script:
    - npm run deploy:production
  when: manual
  only:
    - main
```

### Conditional Execution

```yaml
build:
  stage: build
  script:
    - npm run build
  # Run on specific branches
  only:
    - main
    - develop
  # or exclude branches
  # except:
  #   - tags

deploy:
  stage: deploy
  script:
    - npm run deploy
  only:
    # Run on tags
    - tags
  when: manual
```

---

## 5. Jobs and Stages

### Parallel Jobs in Stage

```yaml
stages:
  - test

test:unit:
  stage: test
  script:
    - npm run test:unit

test:integration:
  stage: test
  script:
    - npm run test:integration

test:e2e:
  stage: test
  script:
    - npm run test:e2e

# All run in parallel during test stage
```

### Sequential Stages

```yaml
stages:
  - build
  - test
  - security
  - deploy

# Stages run sequentially (build → test → security → deploy)
# Jobs within same stage run in parallel
```

### Job Dependencies

```yaml
build:
  stage: build
  script:
    - npm run build
  artifacts:
    paths:
      - dist/

deploy:
  stage: deploy
  dependencies:
    - build
  script:
    - npm run deploy
  # Only depends on build, not other stage artifacts
```

### Allow Failure

```yaml
lint:
  stage: test
  script:
    - npm run lint
  allow_failure: true
  # Pipeline continues even if this job fails

test:
  stage: test
  script:
    - npm test
  allow_failure: false
  # Pipeline stops if this job fails
```

---

## 6. Runners

### Register Runner

```bash
# Install Runner
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
sudo apt-get install gitlab-runner

# Register runner
sudo gitlab-runner register \
  --url https://gitlab.com/ \
  --registration-token $REGISTRATION_TOKEN \
  --executor docker \
  --docker-image node:16-alpine \
  --description "Docker Runner"

# List runners
sudo gitlab-runner list

# Verify runner
gitlab-runner verify --delete
```

### Docker Executor

```yaml
# Runner configuration
build:
  stage: build
  image: node:16-alpine
  script:
    - npm install
    - npm run build
  tags:
    - docker
```

### Kubernetes Executor

```yaml
build:
  stage: build
  image: node:16-alpine
  tags:
    - kubernetes
  script:
    - npm run build
  resource_limits:
    cpu: "1"
    memory: "1Gi"
```

### Run on Specific Runner

```yaml
build:
  stage: build
  tags:
    - docker
    - linux
  script:
    - npm run build
  # Runs on runner with both 'docker' and 'linux' tags
```

---

## 7. Variables and Secrets

### Environment Variables

```yaml
# Predefined variables
build:
  script:
    - echo $CI_PROJECT_NAME        # Project name
    - echo $CI_COMMIT_SHA          # Commit hash
    - echo $CI_COMMIT_BRANCH       # Branch name
    - echo $CI_COMMIT_MESSAGE      # Commit message
    - echo $CI_BUILD_ID            # Build ID
    - echo $CI_REGISTRY_IMAGE      # Registry image name
    - echo $GITLAB_USER_LOGIN      # User login

# Custom variables
variables:
  ENVIRONMENT: production
  DOCKER_REGISTRY: registry.gitlab.com

deploy:
  stage: deploy
  variables:
    DATABASE_URL: postgres://prod-db:5432/db
  script:
    - npm run deploy -- --env $ENVIRONMENT
```

### Protected Variables

```yaml
# In GitLab UI: Settings → CI/CD → Variables
# Create protected variable only accessible on protected branches/tags

deploy:
  stage: deploy
  environment:
    name: production
  only:
    - tags
  script:
    - ./deploy.sh
  # Can access $PROD_API_KEY (protected variable)
```

### File Variables

```yaml
# In GitLab UI: Settings → CI/CD → Variables
# Create variable with file content

deploy:
  stage: deploy
  script:
    - cat $KUBECONFIG_FILE > ~/.kube/config
    - kubectl apply -f deployment.yaml
```

### Using Secrets in Script

```yaml
deploy:
  stage: deploy
  script:
    - echo "API_KEY=$API_KEY" > .env
    - npm run deploy
  # Access via $API_KEY if defined as CI variable
```

---

## 8. Artifacts and Cache

### Save Artifacts

```yaml
build:
  stage: build
  script:
    - npm run build
  artifacts:
    paths:
      - dist/
      - build/
    expire_in: 1 week
    reports:
      junit: reports/junit.xml
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml
```

### Download Artifacts

```yaml
deploy:
  stage: deploy
  dependencies:
    - build
  script:
    - ls dist/
    - npm run deploy
  # Automatically downloads build artifacts
```

### Cache Dependencies

```yaml
stages:
  - build
  - test

build:
  stage: build
  script:
    - npm ci
  cache:
    key: npm-$CI_COMMIT_REF_SLUG
    paths:
      - node_modules/
    policy: pull-push

test:
  stage: test
  script:
    - npm test
  cache:
    key: npm-$CI_COMMIT_REF_SLUG
    paths:
      - node_modules/
    policy: pull
  # Only pulls cache, doesn't save
```

### Cache Policy

```yaml
build:
  cache:
    paths:
      - node_modules/
    policy: pull-push  # Default: pull and push

install:
  cache:
    paths:
      - node_modules/
    policy: pull  # Only pull from cache

save:
  cache:
    paths:
      - node_modules/
    policy: push  # Only push to cache
```

---

## 9. Advanced Features

### Docker Image Building

```yaml
stages:
  - build
  - push

build_image:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - docker tag $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA $CI_REGISTRY_IMAGE:latest

push_image:
  stage: push
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - docker push $CI_REGISTRY_IMAGE:latest
```

### Multi-Project Pipeline

```yaml
trigger_downstream:
  stage: deploy
  trigger:
    project: group/downstream-project
    branch: main
  only:
    - main
```

### Manual Approval

```yaml
deploy_production:
  stage: deploy
  environment:
    name: production
  script:
    - npm run deploy:prod
  when: manual
  # Requires manual click in UI to start
```

### Scheduled Pipeline

```yaml
# In GitLab UI: CI/CD → Schedules
# Create schedule to run pipeline at specific times

nightly_build:
  stage: build
  script:
    - npm run build:nightly
  only:
    - schedules
```

### Rules (Advanced Conditions)

```yaml
build:
  stage: build
  script:
    - npm run build
  rules:
    - if: '$CI_MERGE_REQUEST_IID'
      when: always
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: always
    - when: manual
```

---

## 10. Best Practices

### Organize Pipeline

```yaml
stages:
  - validate
  - build
  - test
  - security
  - deploy

# Clear stage naming for understanding pipeline flow
```

### Use Includes

```yaml
# main.yml
include:
  - local: 'ci/build.yml'
  - local: 'ci/test.yml'
  - local: 'ci/deploy.yml'

stages:
  - build
  - test
  - deploy
```

### Reusable Job Templates

```yaml
.docker_image_build:
  image: docker:latest
  services:
    - docker:dind
  before_script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY

build_app:
  extends: .docker_image_build
  stage: build
  script:
    - docker build -t $CI_REGISTRY_IMAGE:latest .
```

### Proper Caching

```yaml
build:
  cache:
    key:
      files:
        - package-lock.json
      prefix: ${CI_COMMIT_REF_SLUG}
    paths:
      - node_modules/
  script:
    - npm ci
```

### Error Handling

```yaml
deploy:
  stage: deploy
  script:
    - npm run deploy || exit 1
  retry:
    max: 2
    when:
      - runner_system_failure
      - stuck_or_timeout_failure
```

### Documentation

```yaml
# Add descriptions to jobs
build:
  stage: build
  description: "Build application for all platforms"
  script:
    - npm run build
```

---

## 11. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://docs.gitlab.com/ee/ci/quick_start/ |
| **Pipeline Configuration** | https://docs.gitlab.com/ee/ci/yaml/ |
| **CI/CD Variables** | https://docs.gitlab.com/ee/ci/variables/ |
| **Runners** | https://docs.gitlab.com/runner/ |
| **Artifacts** | https://docs.gitlab.com/ee/ci/artifacts/ |
| **Cache** | https://docs.gitlab.com/ee/ci/caching/ |
| **Environments** | https://docs.gitlab.com/ee/ci/environments/ |
| **Advanced** | https://docs.gitlab.com/ee/ci/directed_acyclic_graph/ |
| **Best Practices** | https://docs.gitlab.com/ee/ci/cloud_services/ |
| **Examples** | https://docs.gitlab.com/ee/ci/examples/ |

### Community and Support

| Resource | URL |
|----------|-----|
| **GitLab Community** | https://forum.gitlab.com/ |
| **Issue Tracker** | https://gitlab.com/gitlab-org/gitlab/-/issues |
| **Slack** | https://slack.gitlab.com/ |
| **Status Page** | https://status.gitlab.com/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Tutorial** | https://docs.gitlab.com/ee/ci/quick_start/ |
| **Video Tutorials** | https://about.gitlab.com/learn/ |
| **GitLab Handbook** | https://about.gitlab.com/handbook/ |
| **Templates** | https://gitlab.com/gitlab-examples |

### Related Tools

| Tool | URL |
|------|-----|
| **GitLab Runner** | https://docs.gitlab.com/runner/ |
| **GitLab CLI** | https://docs.gitlab.com/ee/editor_extensions/gitlab_cli/ |
| **Auto DevOps** | https://docs.gitlab.com/ee/topics/autodevops/ |

---

## Quick Reference

```yaml
# Minimal .gitlab-ci.yml
image: node:16-alpine

stages:
  - build
  - test
  - deploy

build:
  stage: build
  script:
    - npm install
    - npm run build

test:
  stage: test
  script:
    - npm test

deploy:
  stage: deploy
  script:
    - npm run deploy
  only:
    - main
```

---

**Last Updated:** 2024
**GitLab Version:** 14.x+
**License:** SSPL/Open Source

For latest information, visit [GitLab CI/CD Documentation](https://docs.gitlab.com/ee/ci/)