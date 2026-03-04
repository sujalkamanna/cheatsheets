# Travis CI Cheatsheet

![Travis CI Logo](https://travis-ci.com/images/logos/TravisCI-Full-Color.png)

---

## Table of Contents
1. [What is Travis CI?](#1-what-is-travis-ci)
2. [Core Concepts](#2-core-concepts)
3. [Getting Started](#3-getting-started)
4. [Configuration Basics](#4-configuration-basics)
5. [Build Lifecycle](#5-build-lifecycle)
6. [Languages and Environments](#6-languages-and-environments)
7. [Testing](#7-testing)
8. [Deployment](#8-deployment)
9. [Cache and Artifacts](#9-cache-and-artifacts)
10. [Security](#10-security)
11. [Best Practices](#11-best-practices)
12. [Additional Resources](#12-additional-resources)

---

## 1. What is Travis CI?

Travis CI is a cloud-based continuous integration service that automatically tests and deploys code. It integrates seamlessly with GitHub repositories and provides hosted CI/CD without requiring infrastructure setup.

**Key Benefits:**
- Easy GitHub integration
- Free for public repositories
- Multiple language support
- Docker support
- Automatic deployments
- Build matrix for multiple configurations
- No infrastructure setup required

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Repository** | GitHub repository connected to Travis CI |
| **Build** | Complete test and deployment process |
| **Stage** | Logical phase of build (test, deploy) |
| **Job** | Individual execution within a build |
| **Workflow** | Orchestrated sequence of jobs/stages |
| **Environment Variable** | Configuration value for builds |
| **Cache** | Stored dependencies between builds |
| **Artifact** | Build output files |

---

## 3. Getting Started

### Connect Repository to Travis CI

1. Go to [travis-ci.com](https://travis-ci.com)
2. Sign in with GitHub
3. Authorize Travis CI
4. Select repository
5. Enable repository
6. Create `.travis.yml` in repository root

### Basic Setup

```bash
# Create .travis.yml
touch .travis.yml

# Commit and push
git add .travis.yml
git commit -m "Add Travis CI configuration"
git push origin main
```

### View Build Status

```
Travis CI UI → Repository → Builds
├── Current build status
├── Build history
├── Build logs
└── Pull request checks
```

---

## 4. Configuration Basics

### Minimal Configuration

```yaml
# .travis.yml
language: node_js
node_js:
  - "16"

script:
  - npm test
```

### Complete Configuration

```yaml
# .travis.yml
language: node_js

node_js:
  - "16"
  - "18"
  - "20"

# Before installing dependencies
before_install:
  - npm install -g npm@latest
  - npm --version

# Install dependencies
install:
  - npm ci

# Run tests
script:
  - npm run build
  - npm test

# After successful build
after_success:
  - npm run coverage

# After failed build
after_failure:
  - echo "Build failed"

# After build completes (success or failure)
after_script:
  - echo "Cleaning up"

# Only build specific branches
branches:
  only:
    - main
    - develop
  except:
    - gh-pages

# Notifications
notifications:
  email:
    on_success: change
    on_failure: always
  slack:
    secure: xxxxxx
```

---

## 5. Build Lifecycle

### Phases

```yaml
# Build lifecycle phases (in order):

1. OPTIONAL: before_install
   - Runs before install phase
   - Can modify system

2. install
   - Install project dependencies
   - Default depends on language

3. OPTIONAL: before_script
   - Run before main script
   - Setup phase

4. script
   - Run tests
   - Main build phase

5. OPTIONAL: after_success
   - Run if build succeeds

6. OPTIONAL: after_failure
   - Run if build fails

7. OPTIONAL: after_script
   - Run after everything
   - Cleanup phase

8. OPTIONAL: before_deploy
   - Run before deployment

9. deploy
   - Deploy application

10. OPTIONAL: after_deploy
    - Run after deployment
```

### Complete Lifecycle Example

```yaml
language: node_js
node_js:
  - "16"

before_install:
  - echo "Setting up"
  - npm install -g npm@latest

install:
  - npm ci
  - npm run build-deps

before_script:
  - npm run lint-setup

script:
  - npm run lint
  - npm run build
  - npm test

after_success:
  - npm run coverage
  - bash <(curl -s https://codecov.io/bash)

after_failure:
  - npm run debug

after_script:
  - echo "Build completed"

before_deploy:
  - echo "Preparing deployment"

deploy:
  provider: heroku
  api_key: $HEROKU_API_KEY
  app: my-app-production
  on:
    branch: main

after_deploy:
  - echo "Deployment successful"

notifications:
  slack:
    secure: xxxxx
```

---

## 6. Languages and Environments

### Node.js

```yaml
language: node_js
node_js:
  - "16"
  - "18"
  - "20"

cache: npm

install:
  - npm ci

script:
  - npm test
```

### Python

```yaml
language: python
python:
  - "3.8"
  - "3.9"
  - "3.10"

install:
  - pip install -r requirements.txt
  - pip install pytest

script:
  - pytest tests/
```

### Java

```yaml
language: java
jdk:
  - openjdk11
  - openjdk17

script:
  - mvn clean install
```

### Ruby

```yaml
language: ruby
ruby:
  - "2.7"
  - "3.0"
  - "3.1"

install:
  - bundle install

script:
  - rspec
```

### Go

```yaml
language: go
go:
  - "1.18"
  - "1.19"

install:
  - go get -d -v ./...

script:
  - go test ./...
```

### Docker

```yaml
services:
  - docker

script:
  - docker build -t myapp:latest .
  - docker run myapp:latest npm test
```

---

## 7. Testing

### Run Tests

```yaml
language: node_js
node_js:
  - "16"

script:
  - npm test
```

### Multiple Test Commands

```yaml
script:
  - npm run lint
  - npm run build
  - npm test
  - npm run integration-test
```

### Test Report Integration

```yaml
script:
  - npm test -- --coverage

after_success:
  # Upload coverage to Codecov
  - bash <(curl -s https://codecov.io/bash)

  # Upload to Coveralls
  - npm install -g coveralls
  - cat ./coverage/lcov.info | coveralls
```

### Matrix Testing

```yaml
language: node_js
node_js:
  - "16"
  - "18"

env:
  - NODE_ENV=test DB=sqlite
  - NODE_ENV=test DB=postgres

# Expands to 4 jobs (2 versions × 2 environments)

# Exclude specific combinations
matrix:
  exclude:
    - node_js: "16"
      env: NODE_ENV=test DB=postgres
```

---

## 8. Deployment

### Heroku Deployment

```yaml
language: node_js
node_js:
  - "16"

script:
  - npm test

deploy:
  provider: heroku
  api_key: $HEROKU_API_KEY
  app: my-app-production
  on:
    branch: main
```

### GitHub Pages Deployment

```yaml
language: node_js
node_js:
  - "16"

script:
  - npm run build

deploy:
  provider: pages
  github_token: $GITHUB_TOKEN
  local_dir: dist/
  skip_cleanup: true
  on:
    branch: main
```

### AWS Deployment

```yaml
language: node_js
node_js:
  - "16"

script:
  - npm run build

deploy:
  provider: aws-lambda
  function_name: my-function
  region: us-east-1
  access_key_id: $AWS_ACCESS_KEY_ID
  secret_access_key: $AWS_SECRET_ACCESS_KEY
  on:
    branch: main
```

### Docker Registry Push

```yaml
language: node_js
node_js:
  - "16"

services:
  - docker

script:
  - npm test
  - docker build -t myapp:$TRAVIS_BUILD_NUMBER .

deploy:
  provider: script
  script: bash scripts/docker-push.sh
  on:
    branch: main
```

### Custom Deployment Script

```yaml
deploy:
  provider: script
  script: bash scripts/deploy.sh
  on:
    branch: main
    tags: true
```

---

## 9. Cache and Artifacts

### Cache Configuration

```yaml
language: node_js
node_js:
  - "16"

cache:
  npm: true
  # or
  directories:
    - node_modules
    - .cache
    - vendor

before_script:
  - npm ci

script:
  - npm test
```

### Caching Multiple Languages

```yaml
cache:
  directories:
    - node_modules      # npm
    - vendor            # PHP composer
    - .m2               # Maven
    - ~/.gradle         # Gradle
```

### Store Artifacts

```yaml
language: node_js
node_js:
  - "16"

script:
  - npm run build
  - npm test

after_success:
  - tar -czf dist.tar.gz dist/

artifacts:
  paths:
    - dist/
    - coverage/
    - reports/
  expire_in: 30 days
```

---

## 10. Security

### Environment Variables

```yaml
# Define in Travis CI UI
# Settings → Environment Variables

language: node_js
node_js:
  - "16"

script:
  - npm test

deploy:
  provider: heroku
  api_key: $HEROKU_API_KEY  # Set in UI
  on:
    branch: main
```

### Encrypted Variables

```bash
# Encrypt sensitive data
travis encrypt HEROKU_API_KEY=xxxx -r username/repo

# Add to .travis.yml
env:
  secure: encrypted_value_here
```

### SSH Key for Deployment

```yaml
# Generate SSH key and add to Travis CI

before_deploy:
  - eval "$(ssh-agent -s)"
  - ssh-add ~/.ssh/id_rsa

deploy:
  provider: script
  script: bash scripts/deploy.sh
```

### Secure Secrets

```yaml
# Use Travis CI encrypted variables
# Never commit sensitive data to repository

language: node_js
node_js:
  - "16"

env:
  global:
    - secure: "encrypted_token_here"

script:
  - npm test
```

---

## 11. Best Practices

### Keep .travis.yml Simple

```yaml
# ✓ Good - Clear and focused
language: node_js
node_js:
  - "16"

cache: npm

install:
  - npm ci

script:
  - npm test

deploy:
  provider: heroku
  api_key: $HEROKU_API_KEY
  on:
    branch: main
```

### Fast Builds

```yaml
# Cache dependencies
cache: npm

# Skip unnecessary installs
install:
  - npm ci

# Run tests in parallel (if supported)
script:
  - npm run test:unit
  - npm run test:integration

# Fail fast
matrix:
  fast_fail: true
```

### Branch Strategy

```yaml
# Only build important branches
branches:
  only:
    - main
    - develop
    - /^release-.*$/  # regex pattern

  except:
    - gh-pages
    - /^feature-.*$/
```

### Notifications

```yaml
notifications:
  email:
    on_success: change  # Email on status change
    on_failure: always  # Always on failure

  slack:
    secure: encrypted_slack_webhook

  webhooks:
    - urls:
        - http://example.com/webhook
      on_success: always
      on_failure: always
```

### Multi-Stage Builds

```yaml
stages:
  - test
  - deploy

jobs:
  include:
    - stage: test
      script: npm test

    - stage: deploy
      script: skip
      deploy:
        provider: heroku
        api_key: $HEROKU_API_KEY
        on:
          branch: main
```

---

## 12. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://docs.travis-ci.com/user/getting-started/ |
| **Configuration Reference** | https://docs.travis-ci.com/user/customizing-the-build/ |
| **Language Guide** | https://docs.travis-ci.com/user/languages/ |
| **Deployment Guide** | https://docs.travis-ci.com/user/deployment/ |
| **Build Stages** | https://docs.travis-ci.com/user/build-stages/ |
| **Caching** | https://docs.travis-ci.com/user/caching/ |
| **Environment Variables** | https://docs.travis-ci.com/user/environment-variables/ |
| **API** | https://docs.travis-ci.com/user/api/ |

### Community and Support

| Resource | URL |
|----------|-----|
| **GitHub Repository** | https://github.com/travis-ci/travis-ci |
| **Community Forum** | https://discourse.travis-ci.com/ |
| **Status Page** | https://www.traviscistatus.com/ |
| **Support Email** | support@travis-ci.com |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Tutorials** | https://docs.travis-ci.com/user/tutorial/ |
| **Examples** | https://github.com/travis-ci/travis-ci/tree/master/examples |
| **Blog** | https://blog.travis-ci.com/ |
| **Webinars** | https://www.youtube.com/travisci |

---

## Quick Reference

```yaml
# Minimal .travis.yml
language: node_js
node_js:
  - "16"
script:
  - npm test

# With deployment
language: node_js
node_js:
  - "16"
cache: npm
script:
  - npm test
deploy:
  provider: heroku
  api_key: $HEROKU_API_KEY
  on:
    branch: main

# Multi-language matrix
language: python
python:
  - "3.8"
  - "3.9"
  - "3.10"
install:
  - pip install -r requirements.txt
script:
  - pytest tests/

# With Docker
services:
  - docker
script:
  - docker build -t myapp .
  - docker run myapp pytest

# With notifications
notifications:
  email:
    on_failure: always
  slack:
    secure: xxxxx
```

---

**Last Updated:** 2026
**Travis CI Version:** Latest
**License:** Proprietary/Freemium

For latest information, visit [Travis CI Documentation](https://docs.travis-ci.com/)