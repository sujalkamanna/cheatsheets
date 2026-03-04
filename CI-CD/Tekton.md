# Tekton Cheatsheet

![Tekton Logo](https://tekton.dev/images/tekton-horizontal-color.png)

---

## Table of Contents
1. [What is Tekton?](#1-what-is-tekton)
2. [Core Concepts](#2-core-concepts)
3. [Installation](#3-installation)
4. [Tasks](#4-tasks)
5. [Pipelines](#5-pipelines)
6. [PipelineRuns](#6-pipelineruns)
7. [Triggers](#7-triggers)
8. [Parameters and Variables](#8-parameters-and-variables)
9. [Results and Workspaces](#9-results-and-workspaces)
10. [Best Practices](#10-best-practices)
11. [Additional Resources](#11-additional-resources)

---

## 1. What is Tekton?

Tekton is a cloud-native continuous integration and continuous delivery (CI/CD) framework that runs on Kubernetes. It provides Kubernetes-native resources for declaring CI/CD pipelines with reusable, loosely coupled tasks.

**Key Benefits:**
- Cloud-native and Kubernetes-native
- Serverless CI/CD (runs in containers)
- Reusable and composable tasks
- Built-in triggers for webhooks
- Scalable and extensible
- Open-source and free
- No agent setup required

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Task** | Single unit of work (build, test, deploy) |
| **Step** | Individual container execution within task |
| **Pipeline** | Sequence of tasks with dependencies |
| **PipelineRun** | Execution instance of a pipeline |
| **TaskRun** | Execution instance of a task |
| **Workspace** | Shared storage between tasks |
| **Parameter** | Input value for task/pipeline |
| **Result** | Output value from task/pipeline |
| **Trigger** | Event that starts pipeline execution |
| **EventListener** | Receives webhook events |

---

## 3. Installation

### Prerequisites

```bash
# Kubernetes cluster (1.17+)
kubectl version

# kubectl installed
kubectl cluster-info
```

### Install Tekton Pipelines

```bash
# Install latest version
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml

# Or specific version
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/v0.50.x/release.yaml

# Verify installation
kubectl get pods -n tekton-pipelines

# Install Tekton Triggers
kubectl apply --filename https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml

# Verify Triggers
kubectl get pods -n tekton-pipelines
```

### Install Tekton Dashboard (Optional)

```bash
# Install Dashboard
kubectl apply --filename https://storage.googleapis.com/tekton-releases/dashboard/latest/release.yaml

# Port forward to access
kubectl port-forward -n tekton-pipelines svc/tekton-dashboard 9097:9097

# Access at http://localhost:9097
```

### Install Tekton CLI

```bash
# macOS
brew install tektoncd-cli

# Linux
curl -LO https://github.com/tektoncd/cli/releases/download/v0.x.x/tkn_0.x.x_Linux_x86_64.tar.gz
tar xzvf tkn_0.x.x_Linux_x86_64.tar.gz
sudo mv tkn /usr/local/bin/

# Verify
tkn version
```

---

## 4. Tasks

### Basic Task

```yaml
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: build-image
spec:
  description: Build Docker image
  params:
    - name: IMAGE
      description: Image name
      type: string
  steps:
    - name: build
      image: docker:latest
      command: ["docker", "build", "-t", "$(params.IMAGE)", "."]
```

### Task with Steps

```yaml
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: build-and-test
spec:
  description: Build and run tests
  workspaces:
    - name: source
  params:
    - name: nodejs-version
      default: "16"
  steps:
    - name: checkout
      image: alpine/git
      workingDir: $(workspaces.source.path)
      script: |
        git clone https://github.com/username/repo.git .
        git checkout $(params.branch)

    - name: install
      image: node:$(params.nodejs-version)
      workingDir: $(workspaces.source.path)
      script: npm ci

    - name: build
      image: node:$(params.nodejs-version)
      workingDir: $(workspaces.source.path)
      script: npm run build

    - name: test
      image: node:$(params.nodejs-version)
      workingDir: $(workspaces.source.path)
      script: npm test
```

### Task with Results

```yaml
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: get-version
spec:
  description: Extract version from package.json
  results:
    - name: version
      description: Application version
  steps:
    - name: read-version
      image: alpine
      script: |
        VERSION=$(grep '"version"' package.json | head -1 | sed 's/[^0-9.]*//g')
        echo $VERSION | tee $(results.version.path)
```

### Task with Environment Variables

```yaml
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: deploy-app
spec:
  params:
    - name: environment
      type: string
  steps:
    - name: deploy
      image: alpine
      env:
        - name: APP_ENV
          value: $(params.environment)
        - name: APP_NAME
          value: "myapp"
      script: |
        echo "Deploying $APP_NAME to $APP_ENV"
        # deployment script here
```

### Task with Volumes

```yaml
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: docker-build
spec:
  steps:
    - name: build
      image: docker:dind
      volumeMounts:
        - name: docker-sock
          mountPath: /var/run/docker.sock
      script: |
        docker build -t myapp:latest .
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
```

---

## 5. Pipelines

### Basic Pipeline

```yaml
apiVersion: tekton.dev/v1
kind: Pipeline
metadata:
  name: build-deploy
spec:
  description: Build and deploy pipeline
  workspaces:
    - name: shared-workspace
  params:
    - name: image-name
      type: string
      default: myapp:latest
  tasks:
    - name: build-task
      taskRef:
        name: build-image
      params:
        - name: IMAGE
          value: $(params.image-name)
      workspaces:
        - name: source
          workspace: shared-workspace

    - name: push-task
      runAfter: [build-task]
      taskRef:
        name: push-image
      params:
        - name: IMAGE
          value: $(params.image-name)
```

### Pipeline with Dependencies

```yaml
apiVersion: tekton.dev/v1
kind: Pipeline
metadata:
  name: complete-pipeline
spec:
  workspaces:
    - name: source
  tasks:
    # Build task
    - name: build
      taskRef:
        name: build-app
      workspaces:
        - name: source
          workspace: source

    # Parallel testing
    - name: unit-test
      runAfter: [build]
      taskRef:
        name: unit-test
      workspaces:
        - name: source
          workspace: source

    - name: integration-test
      runAfter: [build]
      taskRef:
        name: integration-test
      workspaces:
        - name: source
          workspace: source

    # Deploy after all tests
    - name: deploy
      runAfter: [unit-test, integration-test]
      taskRef:
        name: deploy-app
      workspaces:
        - name: source
          workspace: source
```

### Pipeline with When Condition

```yaml
apiVersion: tekton.dev/v1
kind: Pipeline
metadata:
  name: conditional-pipeline
spec:
  params:
    - name: branch
      type: string
  tasks:
    - name: clone
      taskRef:
        name: git-clone
      params:
        - name: url
          value: https://github.com/username/repo.git
        - name: revision
          value: $(params.branch)

    - name: deploy-prod
      when:
        - input: $(params.branch)
          operator: in
          values: ["main", "master"]
      runAfter: [clone]
      taskRef:
        name: deploy-production
```

---

## 6. PipelineRuns

### Create PipelineRun

```yaml
apiVersion: tekton.dev/v1
kind: PipelineRun
metadata:
  name: build-deploy-run-1
spec:
  pipelineRef:
    name: build-deploy
  workspaces:
    - name: shared-workspace
      volumeClaimTemplate:
        spec:
          accessModes:
            - ReadWriteOnce
          resources:
            requests:
              storage: 1Gi
  params:
    - name: image-name
      value: myapp:1.0.0
```

### PipelineRun via CLI

```bash
# Create PipelineRun
tkn pipeline start build-deploy \
  --workspace name=shared-workspace,volumeClaimTemplateFile=pvc-template.yaml \
  --param image-name=myapp:1.0.0 \
  --showlog

# List PipelineRuns
tkn pipelinerun list

# Get PipelineRun details
tkn pipelinerun describe build-deploy-run-1

# View logs
tkn pipelinerun logs build-deploy-run-1 -f

# Delete PipelineRun
tkn pipelinerun delete build-deploy-run-1
```

### TaskRun (Manual Task Execution)

```yaml
apiVersion: tekton.dev/v1
kind: TaskRun
metadata:
  name: build-image-run-1
spec:
  taskRef:
    name: build-image
  params:
    - name: IMAGE
      value: myapp:1.0.0
  workspaces:
    - name: source
      emptyDir: {}
```

### TaskRun via CLI

```bash
# Create TaskRun
tkn task start build-image \
  --param IMAGE=myapp:1.0.0 \
  --showlog

# List TaskRuns
tkn taskrun list

# View TaskRun logs
tkn taskrun logs build-image-run-1
```

---

## 7. Triggers

### EventListener

```yaml
apiVersion: triggers.tekton.dev/v1beta1
kind: EventListener
metadata:
  name: pipeline-trigger
spec:
  serviceAccountName: pipeline-trigger-sa
  triggers:
    - name: github-push
      interceptors:
        - ref:
            name: github
          params:
            - name: secretRef
              value:
                secretKey: github-token
                secretName: webhook-secret
      bindings:
        - ref: github-binding
      template:
        ref: pipeline-template
```

### TriggerBinding

```yaml
apiVersion: triggers.tekton.dev/v1beta1
kind: TriggerBinding
metadata:
  name: github-binding
spec:
  params:
    - name: gitrevision
      value: $(body.head_commit.id)
    - name: gitrepositoryurl
      value: $(body.repository.clone_url)
    - name: gitbranch
      value: $(body.ref)
```

### TriggerTemplate

```yaml
apiVersion: triggers.tekton.dev/v1beta1
kind: TriggerTemplate
metadata:
  name: pipeline-template
spec:
  params:
    - name: gitrevision
    - name: gitrepositoryurl
    - name: gitbranch
  resourcetemplates:
    - apiVersion: tekton.dev/v1
      kind: PipelineRun
      metadata:
        generateName: build-deploy-
      spec:
        pipelineRef:
          name: build-deploy
        workspaces:
          - name: shared-workspace
            volumeClaimTemplate:
              spec:
                accessModes:
                  - ReadWriteOnce
                resources:
                  requests:
                    storage: 1Gi
        params:
          - name: gitrevision
            value: $(tt.params.gitrevision)
          - name: gitrepositoryurl
            value: $(tt.params.gitrepositoryurl)
```

### Expose EventListener

```bash
# Create service
kubectl expose eventlistener pipeline-trigger \
  --type=LoadBalancer \
  -n default

# Get external IP
kubectl get svc el-pipeline-trigger

# Webhook URL
# http://<external-ip>:8080

# Or use port-forward
kubectl port-forward svc/el-pipeline-trigger 8080:8080
```

---

## 8. Parameters and Variables

### Task Parameters

```yaml
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: param-task
spec:
  params:
    - name: message
      type: string
      description: Message to print
      default: "Hello Tekton"
    - name: count
      type: string
      default: "1"
  steps:
    - name: print
      image: alpine
      script: |
        for i in $(seq 1 $(params.count)); do
          echo $(params.message)
        done
```

### Pipeline Parameters

```yaml
apiVersion: tekton.dev/v1
kind: Pipeline
metadata:
  name: param-pipeline
spec:
  params:
    - name: image-registry
      type: string
      default: docker.io
    - name: image-name
      type: string
    - name: image-tag
      type: string
      default: latest
  tasks:
    - name: build
      taskRef:
        name: build-image
      params:
        - name: REGISTRY
          value: $(params.image-registry)
        - name: IMAGE
          value: $(params.image-name)
        - name: TAG
          value: $(params.image-tag)
```

### Variable Substitution

```yaml
steps:
  - name: build
    image: alpine
    script: |
      # Task context variables
      echo "Task Name: $(context.task.name)"
      echo "Task Retry: $(context.task.retry-count)"
      
      # Step context variables
      echo "Step Name: $(context.steps.build.image)"
```

---

## 9. Results and Workspaces

### Task Results

```yaml
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: build-result-task
spec:
  results:
    - name: image-digest
      description: Image digest
  steps:
    - name: build
      image: docker
      script: |
        docker build -t myapp:latest . | tee $(results.image-digest.path)
```

### Workspace Configuration

```yaml
apiVersion: tekton.dev/v1
kind: Pipeline
metadata:
  name: workspace-pipeline
spec:
  workspaces:
    - name: shared-data
    - name: cache
  tasks:
    - name: clone
      taskRef:
        name: git-clone
      workspaces:
        - name: output
          workspace: shared-data

    - name: build
      runAfter: [clone]
      taskRef:
        name: build-app
      workspaces:
        - name: source
          workspace: shared-data
        - name: cache
          workspace: cache
```

### PipelineRun with Workspaces

```yaml
apiVersion: tekton.dev/v1
kind: PipelineRun
metadata:
  name: workspace-run
spec:
  pipelineRef:
    name: workspace-pipeline
  workspaces:
    - name: shared-data
      volumeClaimTemplate:
        spec:
          accessModes:
            - ReadWriteOnce
          resources:
            requests:
              storage: 5Gi
    - name: cache
      emptyDir: {}
```

---

## 10. Best Practices

### Task Organization

```yaml
# Keep tasks small and focused
# One responsibility per task
# Reusable across pipelines

apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: unit-test
spec:
  description: Run unit tests only
  workspaces:
    - name: source
  steps:
    - name: test
      image: node:16
      workingDir: $(workspaces.source.path)
      script: npm test
```

### Security

```yaml
# Use SecurityContext
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: secure-task
spec:
  steps:
    - name: build
      image: alpine
      securityContext:
        runAsNonRoot: true
        runAsUser: 1000
        fsGroup: 2000
      script: |
        echo "Running as non-root"
```

### Resource Limits

```yaml
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: resource-limited-task
spec:
  steps:
    - name: build
      image: alpine
      resources:
        requests:
          memory: "256Mi"
          cpu: "250m"
        limits:
          memory: "512Mi"
          cpu: "500m"
      script: echo "Running with resource limits"
```

### Error Handling

```yaml
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: error-handling
spec:
  steps:
    - name: build
      image: alpine
      onFailure: continue  # Continue even if fails
      script: |
        set -e  # Exit on error
        npm run build || exit 1

    - name: cleanup
      image: alpine
      script: echo "Cleanup steps"
```

### CI/CD Integration

```yaml
# Complete pipeline example
apiVersion: tekton.dev/v1
kind: Pipeline
metadata:
  name: production-pipeline
spec:
  workspaces:
    - name: source
  params:
    - name: git-url
      type: string
    - name: git-branch
      type: string
      default: main
  tasks:
    - name: clone
      taskRef:
        name: git-clone
      params:
        - name: url
          value: $(params.git-url)
        - name: revision
          value: $(params.git-branch)

    - name: build
      runAfter: [clone]
      taskRef:
        name: build-app

    - name: test
      runAfter: [build]
      taskRef:
        name: run-tests

    - name: deploy
      runAfter: [test]
      taskRef:
        name: deploy-to-k8s
      when:
        - input: $(params.git-branch)
          operator: in
          values: ["main"]
```

---

## 11. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://tekton.dev/docs/getting-started/ |
| **Installation** | https://tekton.dev/docs/installation/ |
| **Pipelines** | https://tekton.dev/docs/pipelines/ |
| **Triggers** | https://tekton.dev/docs/triggers/ |
| **Tasks** | https://tekton.dev/docs/pipelines/tasks/ |
| **API Reference** | https://tekton.dev/docs/pipelines/pipelinerun/ |
| **CLI Reference** | https://tekton.dev/docs/cli/ |
| **Best Practices** | https://tekton.dev/docs/pipelines/best-practices/ |

### Community and Support

| Resource | URL |
|----------|-----|
| **GitHub Repository** | https://github.com/tektoncd/pipeline |
| **GitHub Discussions** | https://github.com/tektoncd/pipeline/discussions |
| **Slack Channel** | https://cloud-native.slack.com/messages/tekton |
| **Community Meetings** | https://tekton.dev/docs/community/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Tutorials** | https://tekton.dev/docs/getting-started/ |
| **Examples** | https://github.com/tektoncd/pipeline/tree/main/examples |
| **Blog** | https://tekton.dev/blog/ |
| **Hub** | https://hub.tekton.dev/ |
| **Dashboard** | https://tekton.dev/docs/dashboard/ |

### Task Hub

| Resource | URL |
|----------|-----|
| **Tekton Hub** | https://hub.tekton.dev/ |
| **Popular Tasks** | git-clone, docker, kubernetes, helm |
| **Community Tasks** | Contributed by community |
| **Installation** | `tkn hub install task <task-name>` |

---

## Quick Reference

```bash
# Install Tekton
kubectl apply -f https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
kubectl apply -f https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml

# Create task
kubectl apply -f task.yaml

# Create pipeline
kubectl apply -f pipeline.yaml

# Start pipeline
tkn pipeline start build-deploy --showlog

# List pipelines
tkn pipeline list

# View logs
tkn pipelinerun logs build-deploy-run-1 -f

# Delete PipelineRun
tkn pipelinerun delete build-deploy-run-1

# Install task from Hub
tkn hub install task git-clone

# Get task details
tkn task describe git-clone
```

---

**Last Updated:** 2026
**Tekton Version:** 0.50.x+
**License:** Apache 2.0 (Open Source)

For latest information, visit [Tekton Documentation](https://tekton.dev/docs/)