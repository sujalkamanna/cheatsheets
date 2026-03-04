# Jenkins CI/CD Cheatsheet

![Jenkins Logo](https://www.jenkins.io/images/logos/jenkins/jenkins.png)

---

## Table of Contents
1. [What is Jenkins?](#1-what-is-jenkins)
2. [Core Concepts](#2-core-concepts)
3. [Installation](#3-installation)
4. [Declarative Pipeline](#4-declarative-pipeline)
5. [Scripted Pipeline](#5-scripted-pipeline)
6. [Pipeline Stages](#6-pipeline-stages)
7. [Agents and Nodes](#7-agents-and-nodes)
8. [Credentials Management](#8-credentials-management)
9. [Build Triggers](#9-build-triggers)
10. [Plugins and Integration](#10-plugins-and-integration)
11. [Best Practices](#11-best-practices)
12. [Additional Resources](#12-additional-resources)

---

## 1. What is Jenkins?

Jenkins is an open-source automation server used to build, test, and deploy software. It supports distributed builds using master-agent architecture and integrates with various tools through plugins.

**Key Benefits:**
- Pipeline as Code (Jenkinsfile)
- Distributed builds across multiple agents
- Extensive plugin ecosystem
- Easy integration with Git, Docker, Kubernetes
- Declarative and scripted pipelines

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Job/Project** | Basic unit of work in Jenkins |
| **Pipeline** | Complex build process defined as code |
| **Stage** | Logical section of pipeline (build, test, deploy) |
| **Step** | Single task within a stage |
| **Agent** | Machine where jobs run |
| **Master** | Jenkins server that schedules jobs |
| **Workspace** | Directory where job executes |
| **Artifact** | Files produced by job execution |

---

## 3. Installation

### Docker Installation

```bash
# Run Jenkins in Docker
docker run -d --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:latest

# Get initial admin password
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword

# Access at http://localhost:8080
```

### Kubernetes Installation

```bash
# Using Helm
helm repo add jenkinsci https://charts.jenkins.io
helm repo update

helm install jenkins jenkinsci/jenkins \
  --namespace jenkins \
  --create-namespace \
  --values values.yaml

# Port forward
kubectl port-forward -n jenkins svc/jenkins 8080:8080
```

### Linux/macOS Installation

```bash
# macOS
brew install jenkins-lts

# Ubuntu/Debian
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins

# Start Jenkins
sudo systemctl start jenkins
```

---

## 4. Declarative Pipeline

### Basic Declarative Pipeline

```groovy
pipeline {
    agent any
    
    options {
        timestamps()
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh './deploy.sh'
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
```

### Parameters

```groovy
pipeline {
    agent any
    
    parameters {
        string(
            name: 'VERSION',
            defaultValue: '1.0.0',
            description: 'Release version'
        )
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'staging', 'production'],
            description: 'Target environment'
        )
        booleanParam(
            name: 'RUN_TESTS',
            defaultValue: true,
            description: 'Run tests?'
        )
    }
    
    stages {
        stage('Build') {
            steps {
                echo "Building version ${params.VERSION}"
                sh "npm run build -- --version=${params.VERSION}"
            }
        }
    }
}
```

### Conditional Execution

```groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            when {
                branch 'main'
            }
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Deploy Dev') {
            when {
                branch 'develop'
            }
            steps {
                sh './deploy-dev.sh'
            }
        }
        
        stage('Deploy Prod') {
            when {
                allOf {
                    branch 'main'
                    expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
                }
            }
            steps {
                sh './deploy-prod.sh'
            }
        }
    }
}
```

### Parallel Stages

```groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Parallel Tests') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh 'npm run test:unit'
                    }
                }
                stage('Integration Tests') {
                    steps {
                        sh 'npm run test:integration'
                    }
                }
                stage('E2E Tests') {
                    steps {
                        sh 'npm run test:e2e'
                    }
                }
            }
        }
    }
}
```

---

## 5. Scripted Pipeline

### Basic Scripted Pipeline

```groovy
node {
    def workspace = pwd()
    
    try {
        stage('Checkout') {
            checkout scm
        }
        
        stage('Build') {
            sh 'npm install'
            sh 'npm run build'
        }
        
        stage('Test') {
            sh 'npm test'
        }
        
        if (env.BRANCH_NAME == 'main') {
            stage('Deploy') {
                sh './deploy.sh'
            }
        }
        
        currentBuild.result = 'SUCCESS'
    } catch (Exception e) {
        currentBuild.result = 'FAILURE'
        echo "Pipeline failed: ${e.message}"
        throw e
    } finally {
        cleanWs()
    }
}
```

### With Variables and Loops

```groovy
node {
    def nodeVersion = '16'
    def environments = ['dev', 'staging', 'production']
    
    stage('Setup') {
        sh "nvm use ${nodeVersion}"
    }
    
    stage('Deploy to Environments') {
        for (env in environments) {
            stage("Deploy to ${env}") {
                sh "./deploy.sh ${env}"
            }
        }
    }
}
```

---

## 6. Pipeline Stages

### Build Stage

```groovy
stage('Build') {
    steps {
        script {
            echo "Building application..."
            sh '''
                npm install
                npm run build
                echo "Build completed"
            '''
        }
    }
}
```

### Test Stage

```groovy
stage('Test') {
    steps {
        script {
            sh '''
                npm run test:unit
                npm run test:integration
                npm run coverage
            '''
        }
    }
    post {
        always {
            junit 'reports/**/*.xml'
            publishHTML([
                reportDir: 'coverage',
                reportFiles: 'index.html',
                reportName: 'Coverage Report'
            ])
        }
    }
}
```

### Security Stage

```groovy
stage('Security Scan') {
    steps {
        script {
            sh '''
                npm audit
                # or use sonarqube
                sonar-scanner \
                  -Dsonar.projectKey=my-app \
                  -Dsonar.sources=src \
                  -Dsonar.host.url=http://sonarqube:9000 \
                  -Dsonar.login=${SONAR_TOKEN}
            '''
        }
    }
}
```

### Docker Build Stage

```groovy
stage('Build Docker Image') {
    steps {
        script {
            def app = docker.build("myapp:${BUILD_NUMBER}")
            app.push()
            app.push('latest')
        }
    }
}
```

### Deploy Stage

```groovy
stage('Deploy to Kubernetes') {
    steps {
        script {
            withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                sh '''
                    kubectl set image deployment/my-app \
                      my-app=myapp:${BUILD_NUMBER} \
                      -n production
                    kubectl rollout status deployment/my-app -n production
                '''
            }
        }
    }
}
```

---

## 7. Agents and Nodes

### Agent Configuration

```groovy
pipeline {
    // Run on any available agent
    agent any
    
    // Run on specific agent
    // agent { label 'linux' }
    
    // Run in Docker container
    // agent { docker { image 'node:16' } }
    
    stages {
        stage('Build') {
            agent { docker { image 'node:16' } }
            steps {
                sh 'npm --version'
            }
        }
    }
}
```

### Docker Agent

```groovy
pipeline {
    agent {
        docker {
            image 'node:16-alpine'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
            reuseNode true
        }
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'npm install && npm run build'
            }
        }
    }
}
```

### Kubernetes Agent

```groovy
pipeline {
    agent {
        kubernetes {
            yaml '''
                apiVersion: v1
                kind: Pod
                metadata:
                  labels:
                    jenkins: agent
                spec:
                  serviceAccountName: jenkins
                  containers:
                  - name: docker
                    image: docker:20.10
                    command:
                    - cat
                    tty: true
                    volumeMounts:
                    - name: docker-sock
                      mountPath: /var/run/docker.sock
                  volumes:
                  - name: docker-sock
                    hostPath:
                      path: /var/run/docker.sock
            '''
        }
    }
    
    stages {
        stage('Build') {
            steps {
                container('docker') {
                    sh 'docker build -t myapp:latest .'
                }
            }
        }
    }
}
```

---

## 8. Credentials Management

### Using Credentials

```groovy
pipeline {
    agent any
    
    stages {
        stage('Deploy') {
            steps {
                script {
                    // Username and password
                    withCredentials([
                        usernamePassword(
                            credentialsId: 'docker-credentials',
                            usernameVariable: 'DOCKER_USER',
                            passwordVariable: 'DOCKER_PASS'
                        )
                    ]) {
                        sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                        sh 'docker push myapp:latest'
                    }
                    
                    // SSH key
                    withCredentials([
                        sshUserPrivateKey(
                            credentialsId: 'github-ssh-key',
                            keyFileVariable: 'SSH_KEY'
                        )
                    ]) {
                        sh 'git clone git@github.com:username/repo.git'
                    }
                    
                    // Secret text
                    withCredentials([
                        string(
                            credentialsId: 'api-token',
                            variable: 'API_TOKEN'
                        )
                    ]) {
                        sh 'curl -H "Authorization: Bearer $API_TOKEN" https://api.example.com'
                    }
                }
            }
        }
    }
}
```

### Environment Variables

```groovy
pipeline {
    agent any
    
    environment {
        REGISTRY = 'docker.io'
        IMAGE_NAME = 'myapp'
        IMAGE_TAG = "${BUILD_NUMBER}"
        FULL_IMAGE = "${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}"
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t ${FULL_IMAGE} .'
                sh 'docker push ${FULL_IMAGE}'
            }
        }
    }
}
```

---

## 9. Build Triggers

### Poll SCM

```groovy
pipeline {
    agent any
    
    triggers {
        // Poll every 15 minutes
        pollSCM('H/15 * * * *')
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
    }
}
```

### GitHub Webhook

```groovy
pipeline {
    agent any
    
    triggers {
        githubPush()
        // or
        // pollSCM('H * * * *')
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
    }
}
```

### Scheduled Build

```groovy
pipeline {
    agent any
    
    triggers {
        // Every day at 2 AM
        cron('0 2 * * *')
    }
    
    stages {
        stage('Nightly Build') {
            steps {
                sh 'npm run build'
            }
        }
    }
}
```

### Multiple Triggers

```groovy
pipeline {
    agent any
    
    triggers {
        githubPush()
        cron('0 2 * * *')
        pollSCM('H/30 * * * *')
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
    }
}
```

---

## 10. Plugins and Integration

### Common Plugins

```groovy
// Docker plugin
pipeline {
    agent any
    
    stages {
        stage('Build Docker') {
            steps {
                script {
                    def app = docker.build("myapp:${BUILD_NUMBER}")
                    app.push()
                }
            }
        }
    }
}

// Kubernetes plugin
stage('Deploy to K8s') {
    steps {
        step([$class: 'KubernetesEngineBuilder',
              projectId: 'my-project',
              clusterName: 'my-cluster',
              zone: 'us-central1-a',
              manifestPattern: 'k8s/deployment.yaml',
              verifyDeployments: true])
    }
}

// SonarQube plugin
stage('SonarQube Analysis') {
    environment {
        SONAR_TOKEN = credentials('sonarqube-token')
    }
    steps {
        sh '''
            sonar-scanner \
              -Dsonar.projectKey=my-app \
              -Dsonar.sources=src \
              -Dsonar.host.url=http://sonarqube:9000 \
              -Dsonar.login=${SONAR_TOKEN}
        '''
    }
}
```

### Slack Notifications

```groovy
pipeline {
    agent any
    
    post {
        success {
            slackSend(
                color: 'good',
                message: 'Build succeeded: ${JOB_NAME} #${BUILD_NUMBER}'
            )
        }
        failure {
            slackSend(
                color: 'danger',
                message: 'Build failed: ${JOB_NAME} #${BUILD_NUMBER}'
            )
        }
    }
}
```

### Email Notifications

```groovy
pipeline {
    agent any
    
    post {
        failure {
            emailext(
                subject: 'Build Failed: ${JOB_NAME} #${BUILD_NUMBER}',
                body: '''Build failed
                    Check console output: ${BUILD_URL}
                ''',
                to: '${DEFAULT_RECIPIENTS}'
            )
        }
    }
}
```

---

## 11. Best Practices

### Organize Jenkinsfile

```groovy
pipeline {
    agent any
    
    options {
        timestamps()
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }
    
    parameters {
        string(name: 'VERSION', defaultValue: '1.0.0')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'prod'])
    }
    
    environment {
        REGISTRY = 'docker.io'
        BUILD_VERSION = "${VERSION}-${BUILD_NUMBER}"
    }
    
    triggers {
        githubPush()
        pollSCM('H/15 * * * *')
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    }
    
    post {
        always {
            junit 'reports/**/*.xml'
            cleanWs()
        }
    }
}
```

### Use Shared Libraries

```groovy
@Library('shared-library') _

pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    buildApp()
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    deployApp(env.ENVIRONMENT)
                }
            }
        }
    }
}
```

### Error Handling

```groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    try {
                        sh 'npm run build'
                    } catch (Exception e) {
                        echo "Build failed: ${e.message}"
                        currentBuild.result = 'FAILURE'
                        error("Build step failed")
                    }
                }
            }
        }
    }
}
```

### Artifact Management

```groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: 'dist/**/*', allowEmptyArchive: true
            archiveArtifacts artifacts: 'reports/**/*.xml', allowEmptyArchive: true
        }
    }
}
```

---

## 12. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://www.jenkins.io/doc/book/getting-started/ |
| **Pipeline Documentation** | https://www.jenkins.io/doc/book/pipeline/ |
| **Declarative Pipeline** | https://www.jenkins.io/doc/book/pipeline/syntax/#declarative-pipeline |
| **Scripted Pipeline** | https://www.jenkins.io/doc/book/pipeline/scripted-pipeline/ |
| **Shared Libraries** | https://www.jenkins.io/doc/book/pipeline/shared-libraries/ |
| **Plugin Index** | https://plugins.jenkins.io/ |
| **Best Practices** | https://www.jenkins.io/doc/book/pipeline/pipeline-best-practices/ |
| **Kubernetes Plugin** | https://plugins.jenkins.io/kubernetes/ |

### Community and Support

| Resource | URL |
|----------|-----|
| **GitHub Repository** | https://github.com/jenkinsci/jenkins |
| **Jenkins Community** | https://www.jenkins.io/participate/ |
| **Issue Tracker** | https://issues.jenkins.io/ |
| **Mailing Lists** | https://www.jenkins.io/mailing-lists/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Tutorial** | https://www.jenkins.io/doc/tutorials/ |
| **Jenkins User Handbook** | https://www.jenkins.io/doc/book/ |
| **Pipeline Examples** | https://github.com/jenkinsci/pipeline-examples |
| **BlueOcean UI** | https://www.jenkins.io/doc/book/blueocean/ |

### Related Tools

| Tool | URL |
|------|-----|
| **Jenkins X** | https://jenkins-x.io/ |
| **Jenkins Operator** | https://jenkinsci.github.io/kubernetes-operator/ |
| **Cloudbees** | https://www.cloudbees.com/jenkins/ |

---

## Quick Reference Commands

```bash
# Access Jenkins CLI
java -jar jenkins-cli.jar -s http://localhost:8080/

# Build job
java -jar jenkins-cli.jar -s http://localhost:8080/ build my-job

# Get build info
java -jar jenkins-cli.jar -s http://localhost:8080/ get-job my-job

# List jobs
java -jar jenkins-cli.jar -s http://localhost:8080/ list-jobs

# Groovy console for debugging
# Access at: http://localhost:8080/script
```

---

**Last Updated:** 2026
**Jenkins Version:** 2.x+
**License:** MIT

For the latest information, visit [Jenkins Documentation](https://www.jenkins.io/doc/)