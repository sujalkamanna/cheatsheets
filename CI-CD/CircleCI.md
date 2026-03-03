# CircleCI CI/CD Cheatsheet

![CircleCI Logo](https://circleci.com/docs/assets/img/circle-logo.svg)

---

## Table of Contents
1. [What is CircleCI?](#1-what-is-circleci)
2. [Fundamental Terminology](#2-fundamental-terminology)
3. [Initial Setup and Configuration](#3-initial-setup-and-configuration)
4. [Configuration File Foundation](#4-configuration-file-foundation)
5. [Execution Environments (Executors)](#5-execution-environments-executors)
6. [Job Definition and Structure](#6-job-definition-and-structure)
7. [Operations and Directives](#7-operations-and-directives)
8. [Configuration Reusability](#8-configuration-reusability)
9. [Pipeline Organization and Execution Flow](#9-pipeline-organization-and-execution-flow)
10. [Data Optimization (Caching and Artifacts)](#10-data-optimization-caching-and-artifacts)
11. [Variable Management and Secrets](#11-variable-management-and-secrets)
12. [Enhanced Capabilities (Orbs)](#12-enhanced-capabilities-orbs)
13. [Problem Diagnosis and Observation](#13-problem-diagnosis-and-observation)
14. [Industry Standards and Recommendations](#14-industry-standards-and-recommendations)
15. [Additional Resources](#15-additional-resources)

---

## 1. What is CircleCI?

CircleCI is an automated continuous integration and continuous deployment platform designed to streamline software development workflows by automating testing, building, and releasing processes.

**Key Benefits:**
- Automate repetitive development tasks
- Detect integration issues early in the development cycle
- Speed up deployment processes
- Improve code quality and reliability

**Official Documentation:** [CircleCI Getting Started](https://circleci.com/docs/getting-started/)

---

## 2. Fundamental Terminology

| Term | Description |
|------|-------------|
| **Job** | A compilation of executable tasks that run sequentially within a pipeline |
| **Task/Step** | An atomic operation or command executed as part of a job |
| **Pipeline** | A structured sequence of jobs orchestrated to achieve a development goal |
| **Execution Environment** | The computational platform where jobs operate (Docker, VM, or macOS) |
| **Reusable Package (Orb)** | Pre-configured sets of CircleCI instructions for integrating external services |
| **Workflow** | A set of rules defining how jobs run and interact with each other |
| **Context** | A mechanism for securely sharing environment variables across projects |

**Reference:** [CircleCI Concepts](https://circleci.com/docs/guides/about-circleci/concepts/)

---

## 3. Initial Setup and Configuration

### Step-by-Step Setup:

1. **Create CircleCI Account**
   - Visit [circleci.com](https://circleci.com)
   - Sign up using GitHub, Bitbucket, or GitLab credentials
   - Authorize CircleCI to access your repositories

2. **Connect Your Repository**
   - Navigate to the Projects dashboard
   - Click "Set Up Project" next to your repository
   - Select your repository language

3. **Create Configuration File**
   - Create `.circleci/config.yml` in your repository root
   - Add your automation configuration
   - Commit and push to trigger your first build

**Documentation:** [CircleCI Setup](https://circleci.com/docs/first-steps/)

---

## 4. Configuration File Foundation

### Basic Structure:

```yaml
version: 2.1

jobs:
  compile:
    docker:
      - image: circleci/node:14
    steps:
      - checkout
      - run: npm install
      - run: npm test

workflows:
  version: 2
  automation_pipeline:
    jobs:
      - compile
```

### File Structure Breakdown:

| Section | Purpose |
|---------|---------|
| `version` | Specifies CircleCI configuration version (use 2.1 for latest features) |
| `jobs` | Defines individual automation tasks |
| `workflows` | Orchestrates job execution order and dependencies |
| `orbs` | Imports reusable configuration packages |
| `executors` | Defines reusable execution environments |
| `commands` | Creates reusable step sequences |

**Reference:** [CircleCI Configuration Reference](https://circleci.com/docs/configuration-reference/)

---

## 5. Execution Environments (Executors)

### Docker Executor (Recommended)

Deploy and run jobs inside isolated Docker containers for consistency and portability.

```yaml
docker:
  - image: circleci/node:14
    auth:
      username: $DOCKER_USER
      password: $DOCKER_PASS
  - image: circleci/postgres:12
    environment:
      POSTGRES_DB: test_db
```

**Supported Images:**
- Node.js: `circleci/node:*`
- Python: `circleci/python:*`
- Ruby: `circleci/ruby:*`
- Go: `circleci/golang:*`
- Java: `circleci/openjdk:*`
- PHP: `circleci/php:*`

**Documentation:** [CircleCI Docker Images](https://circleci.com/docs/circleci-images/)

### Machine Executor (Linux VM)

Execute jobs on a dedicated Linux virtual machine environment with full system access.

```yaml
machine:
  image: ubuntu-2004:202101-01
  docker_layer_caching: true
```

**Available Images:**
- Ubuntu 20.04 LTS
- Ubuntu 18.04 LTS
- Ubuntu 16.04 LTS

**Use Cases:**
- Testing Docker daemon operations
- Running system-level commands
- GPU-based workloads

**Documentation:** [CircleCI Machine Executor](https://circleci.com/docs/using-machine/)

### macOS Executor

Deploy jobs on macOS infrastructure for iOS, tvOS, watchOS, or macOS application development.

```yaml
macos:
  xcode: "13.4.1"
  resource_class: macos.x86_64.medium.gen2
```

**Available Xcode Versions:**
- 13.4.1, 13.3.1, 13.2.1, 13.1, 12.5.1, and more

**Documentation:** [CircleCI macOS Executor](https://circleci.com/docs/using-macos/)

### Windows Executor

Execute jobs on Windows operating system for .NET development.

```yaml
windows:
  image: windows-server-2019-vs2019:stable
  shell: powershell.exe
```

**Available Images:**
- Windows Server 2019 with Visual Studio 2019
- Windows Server 2022 with Visual Studio 2022

**Documentation:** [CircleCI Windows Executor](https://circleci.com/docs/using-windows/)

---

## 6. Job Definition and Structure

### Basic Job Template:

```yaml
jobs:
  compile_code:
    docker:
      - image: circleci/node:14
    working_directory: ~/project
    steps:
      - checkout
      - run: npm install
      - run: npm test
```

### Detailed Job with Named Operations:

```yaml
jobs:
  application_build:
    docker:
      - image: circleci/node:14
    working_directory: ~/app
    environment:
      NODE_ENV: test
    steps:
      - checkout
      
      - run:
          name: Fetch dependencies
          command: npm ci
          
      - run:
          name: Execute test suite
          command: npm test
          
      - run:
          name: Generate coverage report
          command: npm run coverage
          
      - run:
          name: Compile application
          command: npm run build
```

### Job Configuration Options:

```yaml
jobs:
  my_job:
    docker:
      - image: circleci/node:14
    
    # Working directory for all steps
    working_directory: ~/repo
    
    # Job-specific environment variables
    environment:
      NODE_ENV: production
      DEBUG: true
    
    # Resource allocation
    resource_class: large
    
    # Parallelism for test splitting
    parallelism: 4
    
    # Step definitions
    steps:
      - checkout
      - run: npm install
```

**Reference:** [CircleCI Job Configuration](https://circleci.com/docs/jobs-steps/)

---

## 7. Operations and Directives

### Source Code Retrieval

Obtain your repository code at the beginning of a job.

```yaml
steps:
  - checkout:
      path: ~/custom-path
```

**Documentation:** [Checkout Command](https://circleci.com/docs/configuration-reference/#checkout)

### Execute Shell Instructions

Run arbitrary shell commands with various options.

```yaml
steps:
  # Simple command
  - run: npm install
  
  # Named command with timeout
  - run:
      name: Run application tests
      command: npm test
      timeout: 300
      
  # Command with environment variables
  - run:
      name: Deploy application
      command: ./scripts/deploy.sh
      environment:
        DEPLOY_ENV: staging
        
  # Background command
  - run:
      name: Start development server
      command: npm start
      background: true
      
  # Conditional execution
  - run:
      name: Deploy on success
      command: ./deploy.sh
      when: on_success
```

**When Conditions:**
- `on_success` - Run if previous steps succeeded
- `on_fail` - Run if any previous step failed
- `always` - Always run regardless of previous status

**Reference:** [Run Step](https://circleci.com/docs/configuration-reference/#run)

### Security Key Management

Add SSH keys for secure authentication with external services.

```yaml
steps:
  - add_ssh_keys:
      fingerprints:
        - "SO:ME:FIN:GER:PR:IN:T"
        - "AN:OT:HE:R:FI:NG:ER:PR:IN:T"
```

**Documentation:** [Add SSH Keys](https://circleci.com/docs/add-ssh-key/)

### Data Persistence Between Jobs

Transfer files and directories between jobs in a workflow.

```yaml
# In first job - save workspace
- persist_to_workspace:
    root: /tmp/workspace
    paths:
      - build/
      - dist/
      - coverage/

# In second job - load workspace
- attach_workspace:
    at: /tmp/workspace
```

**Reference:** [Workspaces](https://circleci.com/docs/workspaces/)

### Store Test Results

Collect and display test results in CircleCI UI.

```yaml
steps:
  - run: npm test -- --reporters=junit
  - store_test_results:
      path: reports/junit.xml
```

**Supported Formats:**
- JUnit XML
- Mocha JSON
- RSpec XML

**Documentation:** [Store Test Results](https://circleci.com/docs/configuration-reference/#store_test_results)

### Store Build Artifacts

Save build outputs and logs for access and analysis.

```yaml
steps:
  - run: npm run build
  - store_artifacts:
      path: ./dist
      destination: build-output
```

**Documentation:** [Store Artifacts](https://circleci.com/docs/artifacts/)

---

## 8. Configuration Reusability

### Reusable Step Sequences (Commands)

Define common sequences to eliminate redundancy across jobs.

```yaml
commands:
  initialize_workspace:
    description: "Initialize development environment and install dependencies"
    parameters:
      package_manager:
        type: enum
        enum: ["npm", "yarn", "pnpm"]
        default: npm
    steps:
      - checkout
      - run:
          name: Install dependencies
          command: |
            if [ "<< parameters.package_manager >>" = "yarn" ]; then
              yarn install
            elif [ "<< parameters.package_manager >>" = "pnpm" ]; then
              pnpm install
            else
              npm ci
            fi

jobs:
  build_application:
    docker:
      - image: circleci/node:14
    steps:
      - initialize_workspace:
          package_manager: yarn
      - run: npm test
```

**Reference:** [Reusable Commands](https://circleci.com/docs/reusing-config/#commands)

### Reusable Environments (Executors)

Define execution environments once and reference multiple times.

```yaml
executors:
  nodejs_env:
    description: "Node.js environment with standard settings"
    docker:
      - image: circleci/node:14
    working_directory: ~/app
    environment:
      NODE_ENV: test

  python_env:
    docker:
      - image: circleci/python:3.9
      - image: circleci/postgres:12
    working_directory: ~/project

jobs:
  build_nodejs:
    executor: nodejs_env
    steps:
      - checkout
      - run: npm install
      
  build_python:
    executor: python_env
    steps:
      - checkout
      - run: pip install -r requirements.txt
```

**Reference:** [Reusable Executors](https://circleci.com/docs/reusing-config/#executors)

### Dynamic Configuration (Parameters)

Use variables to parameterize jobs and commands.

```yaml
commands:
  display_message:
    description: "Display custom message"
    parameters:
      message:
        type: string
        description: Message to display
      color:
        type: string
        default: "blue"
    steps:
      - run: 
          name: Display message
          command: echo "Color: << parameters.color >> | Message: << parameters.message >>"

  run_tests:
    description: "Run tests with custom settings"
    parameters:
      test_type:
        type: enum
        enum: ["unit", "integration", "e2e"]
      timeout:
        type: integer
        default: 300
    steps:
      - run:
          name: Run << parameters.test_type >> tests
          command: npm run test:<< parameters.test_type >>
          timeout: << parameters.timeout >>

jobs:
  test_job:
    docker:
      - image: circleci/node:14
    steps:
      - checkout
      - display_message:
          message: "Starting tests"
          color: "green"
      - run_tests:
          test_type: unit
          timeout: 600
```

**Parameter Types:**
- `string` - Text parameter
- `integer` - Numeric parameter
- `enum` - Multiple choice parameter
- `boolean` - True/False parameter

**Reference:** [Parameterized Jobs](https://circleci.com/docs/reusing-config/#parameterized-jobs)

---

## 9. Pipeline Organization and Execution Flow

### Simple Pipeline

Basic workflow with independent jobs.

```yaml
workflows:
  version: 2
  simple_workflow:
    jobs:
      - build
      - test
      - lint
```

### Linear Execution (Sequential)

Jobs run one after another with dependencies.

```yaml
workflows:
  version: 2
  sequential_pipeline:
    jobs:
      - build
      
      - test:
          requires:
            - build
      
      - security_scan:
          requires:
            - test
      
      - deploy:
          requires:
            - security_scan
          filters:
            branches:
              only: main
```

### Concurrent Execution (Parallel)

Multiple jobs run simultaneously to reduce pipeline duration.

```yaml
workflows:
  version: 2
  concurrent_pipeline:
    jobs:
      # These jobs run in parallel
      - unit_tests
      - integration_tests
      - code_quality_analysis
      - security_scan
      
      # This job waits for all above
      - build:
          requires:
            - unit_tests
            - integration_tests
            - code_quality_analysis
            - security_scan
```

### Filtered Execution (Branch-Specific)

Run jobs only on specific branches.

```yaml
workflows:
  version: 2
  deployment_pipeline:
    jobs:
      - build
      
      - test:
          requires:
            - build
      
      - deploy_staging:
          requires:
            - test
          filters:
            branches:
              only: develop
      
      - deploy_production:
          requires:
            - test
          filters:
            branches:
              only: main
            tags:
              only: /^v\d+\.\d+\.\d+$/
```

### Complex Workflow Example

```yaml
workflows:
  version: 2
  complete_pipeline:
    jobs:
      # Build stage
      - build:
          filters:
            branches:
              ignore: gh-pages
      
      # Test stage (parallel)
      - unit_test:
          requires: [build]
      - integration_test:
          requires: [build]
      - e2e_test:
          requires: [build]
      
      # Analysis stage (parallel)
      - lint:
          requires: [build]
      - security_scan:
          requires: [build]
      
      # Staging deployment (requires all tests)
      - deploy_staging:
          requires: [unit_test, integration_test, e2e_test, lint, security_scan]
          filters:
            branches:
              only: develop
      
      # Production deployment (manual approval)
      - approval:
          type: approval
          requires: [deploy_staging]
      
      - deploy_production:
          requires: [approval]
```

**Reference:** [Workflows](https://circleci.com/docs/workflows/)

---

## 10. Data Optimization (Caching and Artifacts)

### Preserve Dependencies

Store downloaded packages to accelerate future builds.

```yaml
steps:
  - checkout
  
  # Restore from cache if exists
  - restore_cache:
      keys:
        - v1-npm-dependencies-{{ checksum "package-lock.json" }}
        - v1-npm-dependencies-
        - v1-npm-
  
  # Install dependencies
  - run:
      name: Install dependencies
      command: npm ci
  
  # Save to cache for future builds
  - save_cache:
      paths:
        - node_modules
      key: v1-npm-dependencies-{{ checksum "package-lock.json" }}
```

### Cache Best Practices

```yaml
steps:
  # Multiple caching strategies
  - restore_cache:
      keys:
        # Most specific - exact dependency match
        - v2-{{ .Environment.CIRCLE_JOB }}-{{ checksum "package-lock.json" }}-{{ checksum "yarn.lock" }}
        # Medium specificity - any package lock
        - v2-{{ .Environment.CIRCLE_JOB }}-{{ checksum "package-lock.json" }}
        # Less specific - any v2 cache
        - v2-{{ .Environment.CIRCLE_JOB }}-
        - v2-
  
  - run: npm ci
  
  - save_cache:
      paths:
        - node_modules
        - ~/.cache/npm
      key: v2-{{ .Environment.CIRCLE_JOB }}-{{ checksum "package-lock.json" }}-{{ checksum "yarn.lock" }}
```

### Cache Keys and Checksums

```yaml
# Use checksum for automatic cache invalidation
- save_cache:
    key: v1-npm-{{ checksum "package-lock.json" }}
    paths: [node_modules]

# Use timestamp for refreshing cache periodically
- save_cache:
    key: v1-npm-{{ .BuildNum }}
    paths: [node_modules]

# Combine multiple files
- save_cache:
    key: dependencies-{{ checksum "package-lock.json" }}-{{ checksum "Gemfile.lock" }}
    paths: [node_modules, vendor/bundle]
```

### Preserve Build Outputs

Store generated files for access and analysis.

```yaml
jobs:
  build:
    docker:
      - image: circleci/node:14
    steps:
      - checkout
      - run: npm install
      - run: npm run build
      
      # Store built artifacts
      - store_artifacts:
          path: ./dist
          destination: compiled-output
      
      # Store documentation
      - store_artifacts:
          path: ./docs
          destination: api-docs
      
      # Store coverage reports
      - store_artifacts:
          path: ./coverage
          destination: code-coverage
```

### Persist Data Between Jobs

```yaml
# Job 1 - Save workspace
jobs:
  build:
    steps:
      - checkout
      - run: npm run build
      - persist_to_workspace:
          root: /tmp/workspace
          paths:
            - dist/
            - coverage/

# Job 2 - Load workspace
  test:
    steps:
      - checkout
      - attach_workspace:
          at: /tmp/workspace
      - run: npm run test:performance -- dist/
```

**Reference:** [Caching and Artifacts](https://circleci.com/docs/caching/)

---

## 11. Variable Management and Secrets

### Project-Level Environment Variables

Set variables in CircleCI project settings (visible to all workflows).

**Steps:**
1. Navigate to Project Settings
2. Click "Environment Variables"
3. Click "Add Environment Variable"
4. Enter name and value

```yaml
steps:
  - run: echo "API_KEY is $API_KEY"
  - run: npm run deploy
```

### Organization-Level Context Variables

Create shared variable collections across multiple projects.

```yaml
# Create context in CircleCI UI, then use in workflow

workflows:
  version: 2
  build_and_deploy:
    jobs:
      - build:
          context: 
            - build_env  # Custom context

      - deploy:
          context:
            - build_env
            - production_secrets  # Multiple contexts
          requires:
            - build
```

### Job-Specific Environment Variables

Define variables scoped to a specific job.

```yaml
jobs:
  staging_deploy:
    docker:
      - image: circleci/node:14
    environment:
      ENVIRONMENT: staging
      SERVICE_URL: https://staging-api.example.com
      NODE_ENV: production
      LOG_LEVEL: debug
    steps:
      - checkout
      - run: echo "Deploying to $ENVIRONMENT"
      - run: ./scripts/deploy.sh
```

### Step-Level Environment Variables

Define variables for specific steps.

```yaml
steps:
  - run:
      name: Deploy to production
      command: ./deploy.sh
      environment:
        DEPLOY_ENV: production
        DATABASE_URL: ${{ secrets.PROD_DB_URL }}
```

### Using Built-in Variables

CircleCI provides automatically populated variables.

```yaml
steps:
  - run:
      name: Display build information
      command: |
        echo "Repository: $CIRCLE_REPOSITORY_URL"
        echo "Branch: $CIRCLE_BRANCH"
        echo "Build Number: $CIRCLE_BUILD_NUM"
        echo "Build URL: $CIRCLE_BUILD_URL"
        echo "Job Name: $CIRCLE_JOB"
        echo "Commit SHA: $CIRCLE_SHA1"
        echo "Tag: $CIRCLE_TAG"
```

**Complete List:** [Built-in Environment Variables](https://circleci.com/docs/env-vars/#built-in-environment-variables)

### Secure Credential Management

```yaml
version: 2.1

workflows:
  deploy:
    jobs:
      - deploy:
          context: aws-credentials

jobs:
  deploy:
    docker:
      - image: circleci/aws-cli:latest
    steps:
      - checkout
      - run:
          name: Deploy to AWS
          command: |
            aws s3 sync ./build s3://$AWS_BUCKET_NAME
            # Variables from context are automatically available
            # AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY used by aws-cli
```

**Best Practices:**
- Never hardcode credentials in config.yml
- Use CircleCI Contexts for sensitive data
- Rotate credentials regularly
- Use IAM roles when possible
- Restrict context access by organization

**Reference:** [Environment Variables](https://circleci.com/docs/env-vars/)

---

## 12. Enhanced Capabilities (Orbs)

### What are Orbs?

Orbs are reusable, shareable packages of CircleCI configuration that bundle jobs, commands, and executors for common tools and platforms.

**Benefits:**
- Reduce configuration complexity
- Ensure best practices
- Standardize tool integration
- Enable community contributions

### Using Orbs

```yaml
version: 2.1

# Import orbs
orbs:
  node: circleci/node@5.0.0
  aws-s3: circleci/aws-s3@4.2.0
  slack: circleci/slack@4.0.0

jobs:
  build:
    executor: node/default
    steps:
      - checkout
      - node/install-packages:
          pkg-manager: npm
          cache-only-deps: true
      - run: npm test

  deploy:
    executor: node/default
    steps:
      - checkout
      - aws-s3/copy:
          from: "build/"
          to: "s3://my-bucket/app/"
      - slack/notify:
          event: pass
          template: success_tagged_deploy_1
```

### Popular Orbs

#### Node.js

```yaml
orbs:
  node: circleci/node@5.0.0

jobs:
  test:
    executor: node/default
    steps:
      - checkout
      - node/install-packages
      - run: npm test
```

**Reference:** [Node.js Orb](https://circleci.com/developer/orbs/orb/circleci/node)

#### Python

```yaml
orbs:
  python: circleci/python@2.0.0

jobs:
  test:
    executor: python/default
    steps:
      - checkout
      - python/install-packages:
          pip-dependency-file: requirements.txt
      - run: pytest
```

**Reference:** [Python Orb](https://circleci.com/developer/orbs/orb/circleci/python)

#### Docker

```yaml
orbs:
  docker: circleci/docker@2.0.0

jobs:
  build:
    executor: docker/docker
    steps:
      - setup_remote_docker
      - checkout
      - docker/build:
          image: my-app
          tag: latest
      - docker/push:
          image: my-app
          tag: latest
```

**Reference:** [Docker Orb](https://circleci.com/developer/orbs/orb/circleci/docker)

#### AWS CLI

```yaml
orbs:
  aws-cli: circleci/aws-cli@2.0.0

jobs:
  deploy:
    executor: aws-cli/default
    steps:
      - checkout
      - aws-cli/setup:
          role-arn: arn:aws:iam::123456789:role/my-role
      - run: aws s3 sync build/ s3://bucket/
```

**Reference:** [AWS CLI Orb](https://circleci.com/developer/orbs/orb/circleci/aws-cli)

#### Slack

```yaml
orbs:
  slack: circleci/slack@4.0.0

jobs:
  notify:
    steps:
      - slack/notify:
          event: pass
          channel: deployments
          template: success_tagged_deploy_1
      - slack/notify:
          event: fail
          channel: alerts
```

**Reference:** [Slack Orb](https://circleci.com/developer/orbs/orb/circleci/slack)

#### GitHub

```yaml
orbs:
  github-release: h-matsuo/github-release@0.1.3

jobs:
  release:
    steps:
      - checkout
      - github-release/create:
          tag: v1.0.0
          title: Version 1.0.0
          body: Release notes here
```

### Find More Orbs

Visit the [CircleCI Orbs Registry](https://circleci.com/developer/orbs) to discover and search for orbs.

**Reference:** [Orbs Documentation](https://circleci.com/docs/orb-intro/)

---

## 13. Problem Diagnosis and Observation

### SSH into Build

Connect via SSH to a running build for real-time troubleshooting.

**Steps:**
1. Run a build and wait for it to hang or fail
2. Click "Rerun Job with SSH" in CircleCI UI
3. Look for SSH command in build output
4. Connect: `ssh -i ~/.ssh/id_rsa_xxx ubuntu@xx.xx.xxx.xxx`

```bash
# Once connected, explore the environment
pwd                    # Current directory
ls -la                 # List files
cat /etc/os-release   # System info
docker images         # Available Docker images
npm --version         # Tool versions
env | sort            # All environment variables
```

**Documentation:** [SSH Debugging](https://circleci.com/docs/ssh-access-jobs/)

### View Build Logs

Access detailed execution logs for debugging.

```yaml
steps:
  - run:
      name: Debug information
      command: |
        echo "Build Number: $CIRCLE_BUILD_NUM"
        echo "Branch: $CIRCLE_BRANCH"
        echo "Working Directory:"
        pwd
        echo "Environment Variables:"
        env | sort
        echo "Disk Space:"
        df -h
      when: always  # Run even if previous steps failed
```

### Pipeline Analytics

Monitor build metrics through CircleCI dashboard.

**Available Metrics:**
- Build duration trends
- Job success/failure rates
- Branch performance comparison
- Workflow execution patterns
- Test execution times

**Access:** Project Settings → Insights

### Conditional Debugging

```yaml
steps:
  - run:
      name: Run tests
      command: npm test
      
  - run:
      name: Debug on failure
      command: |
        echo "Build failed. Collecting debug info..."
        npm list
        npm outdated
        npm audit
      when: on_fail
```

### Notifications

#### Email Notifications

Configured in CircleCI project settings.

#### Slack Integration

```yaml
version: 2.1

orbs:
  slack: circleci/slack@4.0.0

workflows:
  test:
    jobs:
      - test:
          post-steps:
            - slack/notify:
                event: pass
                channel: test-results
                template: pass_template
            - slack/notify:
                event: fail
                channel: alerts
                template: fail_template
```

#### Custom Webhooks

```yaml
steps:
  - run:
      name: Notify deployment system
      command: |
        curl -X POST https://api.example.com/deploy \
          -H "Authorization: Bearer $WEBHOOK_TOKEN" \
          -d "{\"status\": \"success\"}"
```

**Reference:** [Notifications](https://circleci.com/docs/notifications/)

---

## 14. Industry Standards and Recommendations

### Image Version Specification

Use precise version numbers instead of generic `latest` tags.

```yaml
# ✓ Good - Specific version
docker:
  - image: circleci/node:14.21.1
  - image: circleci/python:3.9.15

# ✗ Avoid - Latest tag
docker:
  - image: circleci/node:latest
```

**Benefits:**
- Reproducible builds across time
- Predictable behavior
- Security control
- Easier debugging

### Smart Dependency Caching

```yaml
version: 2.1

executors:
  node-executor:
    docker:
      - image: circleci/node:14.21.1
    working_directory: ~/repo

commands:
  install-dependencies:
    steps:
      - restore_cache:
          keys:
            # Most specific - exact dependency match
            - node-deps-v1-{{ checksum "package-lock.json" }}
            # Fallback - any v1 cache
            - node-deps-v1-
            
      - run:
          name: Install dependencies
          command: npm ci
          
      - save_cache:
          key: node-deps-v1-{{ checksum "package-lock.json" }}
          paths:
            - node_modules
            - ~/.npm

jobs:
  build:
    executor: node-executor
    steps:
      - checkout
      - install-dependencies
      - run: npm test
```

### Single Responsibility Principle

Designate separate jobs for different concerns.

```yaml
workflows:
  version: 2
  complete_pipeline:
    jobs:
      # Build job - only compilation
      - build
      
      # Test jobs - specific test types
      - unit-test:
          requires: [build]
      - integration-test:
          requires: [build]
      - e2e-test:
          requires: [build]
      
      # Analysis jobs - code quality
      - lint:
          requires: [build]
      - security-scan:
          requires: [build]
      - coverage-report:
          requires: [unit-test]
      
      # Deployment jobs
      - deploy-staging:
          requires: [unit-test, integration-test, lint, security-scan]
      - deploy-production:
          requires: [unit-test, integration-test, e2e-test]
```

### Credential Protection

Utilize CircleCI's variable system for all sensitive authentication details.

```yaml
# ✓ Good - Use environment variables
steps:
  - run:
      name: Deploy
      command: |
        npm run build
        ./scripts/deploy.sh
      environment:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}

# ✗ Avoid - Hardcoded credentials
steps:
  - run: aws configure set aws_access_key_id AKIAIOSFODNN7EXAMPLE
```

### Parallel Task Execution

Distribute test execution across multiple workers.

```yaml
jobs:
  test:
    docker:
      - image: circleci/node:14
    parallelism: 8  # Split tests across 8 containers
    
    steps:
      - checkout
      - restore_cache:
          keys:
            - v1-npm-{{ checksum "package-lock.json" }}
      
      - run:
          name: Run tests
          command: npm test -- --maxWorkers=4
```

### Clear Pipeline Structure

Use self-documenting job names and logical dependency chains.

```yaml
version: 2.1

# Define reusable components
commands:
  report-status:
    parameters:
      status:
        type: string
    steps:
      - run: echo "Job status: << parameters.status >>"

# Clear workflow definition
workflows:
  version: 2
  production-deployment:
    jobs:
      # Phase 1: Validation
      - validate-code-formatting
      - validate-dependencies
      
      # Phase 2: Testing
      - run-unit-tests:
          requires: [validate-code-formatting]
      - run-integration-tests:
          requires: [validate-code-formatting]
      
      # Phase 3: Build
      - build-application:
          requires: [run-unit-tests, run-integration-tests]
      
      # Phase 4: Pre-deployment checks
      - security-scan:
          requires: [build-application]
      - performance-baseline:
          requires: [build-application]
      
      # Phase 5: Deployment
      - deploy-to-staging:
          requires: [security-scan, performance-baseline]
      - approval-for-production:
          type: approval
          requires: [deploy-to-staging]
      - deploy-to-production:
          requires: [approval-for-production]
```

### Orb Version Pinning

Lock orb versions to guarantee consistent automation behavior.

```yaml
# ✓ Good - Specific version
version: 2.1

orbs:
  node: circleci/node@5.0.0
  aws-s3: circleci/aws-s3@4.2.0
  slack: circleci/slack@4.1.0

# ✗ Avoid - Latest version
orbs:
  node: circleci/node@latest
```

### Job Timeout Configuration

Set appropriate timeouts to prevent stuck builds.

```yaml
jobs:
  long-running-tests:
    docker:
      - image: circleci/node:14
    steps:
      - checkout
      - run:
          name: Run comprehensive tests
          command: npm run test:comprehensive
          timeout: 3600  # 1 hour timeout
```

### Workflow Concurrency Control

Prevent multiple deployments running simultaneously.

```yaml
workflows:
  version: 2
  deployment:
    jobs:
      - build
      - deploy:
          requires: [build]
          # Only one deploy job at a time
          concurrency: 1
```

---

## 15. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started Guide** | [https://circleci.com/docs/getting-started/](https://circleci.com/docs/getting-started/) |
| **Configuration Reference** | [https://circleci.com/docs/configuration-reference/](https://circleci.com/docs/configuration-reference/) |
| **Concepts & Terminology** | [https://circleci.com/docs/concepts/](https://circleci.com/docs/concepts/) |
| **Orbs Registry** | [https://circleci.com/developer/orbs](https://circleci.com/developer/orbs) |
| **API Documentation** | [https://circleci.com/docs/api/v2/](https://circleci.com/docs/api/v2/) |
| **Build Insights** | [https://circleci.com/docs/insights/](https://circleci.com/docs/insights/) |
| **Environment Variables** | [https://circleci.com/docs/env-vars/](https://circleci.com/docs/env-vars/) |
| **Workflows** | [https://circleci.com/docs/workflows/](https://circleci.com/docs/workflows/) |
| **Caching** | [https://circleci.com/docs/caching/](https://circleci.com/docs/caching/) |
| **SSH Debugging** | [https://circleci.com/docs/ssh-access-jobs/](https://circleci.com/docs/ssh-access-jobs/) |

### Community and Support

| Resource | URL |
|----------|-----|
| **CircleCI Community Forum** | [https://discuss.circleci.com/](https://discuss.circleci.com/) |
| **GitHub Issues** | [https://github.com/CircleCI-Public/](https://github.com/CircleCI-Public/) |
| **CircleCI Blog** | [https://circleci.com/blog/](https://circleci.com/blog/) |
| **Twitter** | [@circleci](https://twitter.com/circleci) |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Sample Configuration** | [https://github.com/CircleCI-Public/circleci-docs/tree/master/jekyll/_cci2](https://github.com/CircleCI-Public/circleci-docs/tree/master/jekyll/_cci2) |
| **YouTube Channel** | [https://www.youtube.com/circleci](https://www.youtube.com/circleci) |
| **CircleCI Academy** | [https://academy.circleci.com/](https://academy.circleci.com/) |
| **Best Practices** | [https://circleci.com/docs/best-practices/](https://circleci.com/docs/best-practices/) |

### Troubleshooting

| Issue | Reference |
|-------|-----------|
| **Build Failures** | [https://circleci.com/docs/troubleshoot-build-failures/](https://circleci.com/docs/troubleshoot-build-failures/) |
| **Configuration Issues** | [https://circleci.com/docs/config-intro/](https://circleci.com/docs/config-intro/) |
| **Caching Problems** | [https://circleci.com/docs/caching/](https://circleci.com/docs/caching/) |
| **Permission Issues** | [https://circleci.com/docs/github-apps-integration/](https://circleci.com/docs/github-apps-integration/) |

### Configuration Validation Tools

| Tool | URL |
|------|-----|
| **CircleCI CLI** | [https://circleci.com/docs/local-cli/](https://circleci.com/docs/local-cli/) |
| **Config Validator** | [https://circleci.com/docs/local-cli/#validate-a-circleci-config](https://circleci.com/docs/local-cli/#validate-a-circleci-config) |
| **Orb Validator** | [https://circleci.com/docs/orb-author-validate/](https://circleci.com/docs/orb-author-validate/) |

---

## Quick Reference: Common Tasks

### Setup a New Project

```bash
# 1. Create config directory
mkdir -p .circleci

# 2. Create basic config file
cat > .circleci/config.yml << EOF
version: 2.1

jobs:
  build:
    docker:
      - image: circleci/node:14
    steps:
      - checkout
      - run: npm install
      - run: npm test

workflows:
  version: 2
  build:
    jobs:
      - build
EOF

# 3. Commit and push
git add .circleci/config.yml
git commit -m "Add CircleCI configuration"
git push origin main
```

### Deploy on Push to Main

```yaml
workflows:
  version: 2
  deploy-on-main:
    jobs:
      - build:
          filters:
            branches:
              only: main
      - deploy:
          requires: [build]
          filters:
            branches:
              only: main
```

### Run Tests in Parallel

```yaml
jobs:
  test:
    parallelism: 4
    steps:
      - checkout
      - run: npm test
```

### Cache Node Modules

```yaml
steps:
  - restore_cache:
      key: node-v1-{{ checksum "package-lock.json" }}
  - run: npm ci
  - save_cache:
      key: node-v1-{{ checksum "package-lock.json" }}
      paths: [node_modules]
```

---

**Last Updated:** 2024
**CircleCI Version:** 2.1
**License:** MIT

For the latest information, always refer to the official [CircleCI Documentation](https://circleci.com/docs/).