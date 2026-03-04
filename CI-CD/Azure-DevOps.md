# Azure DevOps Cheatsheet

![Azure DevOps Logo](https://learn.microsoft.com/en-us/azure/devops/media/devops-icon-deploy.png)

---

## Table of Contents
1. [What is Azure DevOps?](#1-what-is-azure-devops)
2. [Core Concepts](#2-core-concepts)
3. [Getting Started](#3-getting-started)
4. [Pipeline Structure](#4-pipeline-structure)
5. [Jobs and Tasks](#5-jobs-and-tasks)
6. [Stages and Environments](#6-stages-and-environments)
7. [Variables and Secrets](#7-variables-and-secrets)
8. [Agents and Pools](#8-agents-and-pools)
9. [Artifacts and Testing](#9-artifacts-and-testing)
10. [Best Practices](#10-best-practices)
11. [Additional Resources](#11-additional-resources)

---

## 1. What is Azure DevOps?

Azure DevOps is a suite of development tools provided by Microsoft for planning, developing, testing, and deploying applications. It includes Pipelines for CI/CD automation using YAML or visual editor configuration.

**Key Benefits:**
- Integrated DevOps platform (Boards, Repos, Pipelines, Tests, Artifacts)
- Free tier with 1 job parallel and unlimited private repos
- YAML and visual pipeline editor
- Native integration with Azure services
- Cross-platform support

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Pipeline** | Automated workflow for build and deployment |
| **Stage** | Logical division of pipeline (Build, Test, Deploy) |
| **Job** | Collection of steps executed on agent |
| **Task** | Individual action within a job |
| **Agent** | Machine that runs pipeline jobs |
| **Artifact** | Output files from pipeline for deployment |
| **Trigger** | Event that starts pipeline execution |
| **Variable** | Named value used in pipeline configuration |

---

## 3. Getting Started

### Create Pipeline File

```bash
# Create pipeline configuration
touch azure-pipelines.yml
```

### Basic Pipeline

```yaml
trigger:
  - main
  - develop

pr:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: NodeTool@0
    inputs:
      versionSpec: '16.x'
  
  - script: npm install
    displayName: 'Install dependencies'
  
  - script: npm run build
    displayName: 'Build'
  
  - script: npm test
    displayName: 'Run tests'
```

### View Pipeline Status

```bash
# In Azure DevOps UI: Pipelines → All
# Or use Azure CLI:
az pipelines run --id <pipeline-id>
az pipelines build list
```

---

## 4. Pipeline Structure

### Complete Pipeline Example

```yaml
trigger:
  branches:
    include:
    - main
    - develop
  paths:
    include:
    - src/
    - tests/
    - package.json

pr:
  branches:
    include:
    - main

pool:
  vmImage: 'ubuntu-latest'

variables:
  buildConfiguration: 'Release'
  dotnetVersion: '6.0.x'
  nodeVersion: '16.x'

stages:
- stage: Build
  displayName: 'Build Stage'
  jobs:
  - job: BuildJob
    displayName: 'Build Application'
    steps:
    - task: NodeTool@0
      inputs:
        versionSpec: $(nodeVersion)
    
    - script: npm ci
      displayName: 'Install dependencies'
    
    - script: npm run build
      displayName: 'Build application'
    
    - task: PublishBuildArtifacts@1
      inputs:
        pathToPublish: $(Build.ArtifactStagingDirectory)
        artifactName: drop

- stage: Test
  displayName: 'Test Stage'
  dependsOn: Build
  jobs:
  - job: TestJob
    steps:
    - task: DownloadBuildArtifacts@0
      inputs:
        buildType: 'current'
        downloadType: 'single'
        artifactName: 'drop'
    
    - script: npm test
      displayName: 'Run tests'
    
    - task: PublishTestResults@2
      inputs:
        testResultsFormat: 'JUnit'
        testResultsFiles: '**/junit.xml'

- stage: Deploy
  displayName: 'Deploy Stage'
  dependsOn: Test
  condition: succeeded()
  jobs:
  - deployment: DeployApp
    displayName: 'Deploy to Production'
    environment: 'Production'
    strategy:
      runOnce:
        deploy:
          steps:
          - script: npm run deploy
            displayName: 'Deploy application'
```

### Conditional Execution

```yaml
steps:
- script: echo "Build succeeded"
  condition: succeeded()

- script: echo "Build failed"
  condition: failed()

- script: echo "Always run"
  condition: always()

- script: echo "Run on main branch"
  condition: eq(variables['Build.SourceBranch'], 'refs/heads/main')
```

---

## 5. Jobs and Tasks

### Multiple Jobs

```yaml
jobs:
- job: BuildJob
  displayName: 'Build Application'
  steps:
  - script: npm run build

- job: TestJob
  displayName: 'Run Tests'
  dependsOn: BuildJob
  steps:
  - script: npm test

- job: SecurityJob
  displayName: 'Security Scan'
  dependsOn: BuildJob
  steps:
  - script: npm audit
```

### Parallel Jobs

```yaml
jobs:
- job: UnitTest
  displayName: 'Unit Tests'
  steps:
  - script: npm run test:unit

- job: IntegrationTest
  displayName: 'Integration Tests'
  steps:
  - script: npm run test:integration

- job: E2ETest
  displayName: 'E2E Tests'
  steps:
  - script: npm run test:e2e

# All three run in parallel
```

### Job with Container

```yaml
jobs:
- job: ContainerJob
  displayName: 'Run in Container'
  pool:
    vmImage: 'ubuntu-latest'
  container: node:16-alpine
  steps:
  - script: npm run build
```

### Common Tasks

```yaml
steps:
# Checkout code
- checkout: self

# Use .NET
- task: UseDotNet@2
  inputs:
    version: '6.0.x'

# Use Node.js
- task: NodeTool@0
  inputs:
    versionSpec: '16.x'

# Use Python
- task: UsePythonVersion@0
  inputs:
    versionSpec: '3.9'

# Run command
- script: npm run build
  displayName: 'Build'

# Run PowerShell
- powershell: Get-ChildItem
  displayName: 'List files'

# Copy files
- task: CopyFiles@2
  inputs:
    sourceFolder: '$(Build.SourcesDirectory)'
    contents: '**/*.js'
    targetFolder: '$(Build.ArtifactStagingDirectory)'

# Archive files
- task: ArchiveFiles@2
  inputs:
    rootFolderOrFile: '$(Build.ArtifactStagingDirectory)'
    archiveFile: '$(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip'
```

---

## 6. Stages and Environments

### Multi-Stage Pipeline

```yaml
stages:
- stage: Build
  displayName: 'Build'
  jobs:
  - job: BuildJob
    steps:
    - script: npm run build

- stage: Test
  displayName: 'Test'
  dependsOn: Build
  jobs:
  - job: TestJob
    steps:
    - script: npm test

- stage: Deploy
  displayName: 'Deploy'
  dependsOn: Test
  condition: succeeded()
  jobs:
  - job: DeployJob
    steps:
    - script: npm run deploy
```

### Deployment Job

```yaml
stages:
- stage: Deploy
  displayName: 'Deploy to Azure'
  jobs:
  - deployment: DeployApp
    displayName: 'Deploy Application'
    environment: 'production'
    strategy:
      runOnce:
        preDeploy:
          steps:
          - script: echo "Pre-deployment steps"
        
        deploy:
          steps:
          - task: AzureWebApp@1
            inputs:
              azureSubscription: 'Azure Subscription'
              appType: 'webAppLinux'
              appName: 'my-app'
              package: '$(Pipeline.Workspace)/drop'
        
        postDeploy:
          steps:
          - script: echo "Post-deployment steps"
        
        on:
          failure:
            steps:
            - script: echo "Deployment failed"
```

### Environment Protection

```yaml
# Create approval gates in environment settings
# Deploy stage uses approval before deployment

stages:
- stage: DeployProduction
  displayName: 'Deploy to Production'
  jobs:
  - deployment: DeployProd
    displayName: 'Deploy'
    environment: 'production'
    strategy:
      runOnce:
        deploy:
          steps:
          - script: npm run deploy:prod
```

---

## 7. Variables and Secrets

### Built-in Variables

```yaml
steps:
- script: |
    echo "Build ID: $(Build.BuildId)"
    echo "Build Number: $(Build.BuildNumber)"
    echo "Source Branch: $(Build.SourceBranch)"
    echo "Commit: $(Build.SourceVersion)"
    echo "Agent: $(Agent.Name)"
    echo "Workspace: $(Agent.WorkFolder)"
  displayName: 'Display variables'
```

### Custom Variables

```yaml
variables:
  buildConfiguration: 'Release'
  environment: 'staging'
  registry: 'myregistry.azurecr.io'

stages:
- stage: Build
  variables:
    localVar: 'stage value'
  jobs:
  - job: BuildJob
    variables:
      jobVar: 'job value'
    steps:
    - script: |
        echo "Global: $(buildConfiguration)"
        echo "Stage: $(localVar)"
        echo "Job: $(jobVar)"
```

### Pipeline Variables

```yaml
variables:
  - name: buildVersion
    value: '1.0.0'
  
  - group: 'Common Variables'  # Link variable group
  
  - template: variables/build.yml  # Template variables

steps:
- script: echo "Version: $(buildVersion)"
```

### Secrets

```yaml
# Define in Pipeline settings or variable groups
# Access with $(...) syntax - automatically masked in logs

steps:
- script: |
    npm config set _auth=$(NPM_TOKEN)
    npm publish
  displayName: 'Publish package'
  env:
    NPM_TOKEN: $(npm-token)  # Secret variable

- task: AzureCLI@2
  inputs:
    azureSubscription: 'Azure Subscription'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      az storage blob upload \
        --connection-string $(storage-connection-string) \
        --container-name drop \
        --file '$(Build.ArtifactStagingDirectory)/*'
```

---

## 8. Agents and Pools

### Microsoft-Hosted Agents

```yaml
pool:
  vmImage: 'ubuntu-latest'       # Linux
  # vmImage: 'windows-latest'    # Windows
  # vmImage: 'macos-latest'      # macOS

steps:
- script: echo "Running on $(Agent.OS)"
```

### Self-Hosted Agent Pool

```yaml
pool:
  name: 'MyAgentPool'
  demands:
  - Agent.Version -gtVersion 2.200.0
  - Docker

steps:
- script: docker --version
  displayName: 'Check Docker'
```

### Agent Capabilities

```yaml
pool:
  name: 'Windows Agents'
  demands:
  - msbuild
  - visualstudio
  - npm

steps:
- script: npm --version
```

---

## 9. Artifacts and Testing

### Publish Artifacts

```yaml
steps:
- script: npm run build
  displayName: 'Build'

- task: PublishBuildArtifacts@1
  inputs:
    pathToPublish: '$(Build.ArtifactStagingDirectory)'
    artifactName: 'drop'
    publishLocation: 'Container'
  displayName: 'Publish artifacts'
```

### Download Artifacts

```yaml
- task: DownloadBuildArtifacts@0
  inputs:
    buildType: 'current'
    downloadType: 'single'
    artifactName: 'drop'
    downloadPath: '$(System.ArtifactsDirectory)'

- script: ls $(System.ArtifactsDirectory)/drop
```

### Publish Test Results

```yaml
- script: npm test -- --reporters=junit
  displayName: 'Run tests'

- task: PublishTestResults@2
  condition: succeededOrFailed()
  inputs:
    testResultsFormat: 'JUnit'
    testResultsFiles: '**/junit.xml'
    displayName: 'Publish test results'
```

### Publish Code Coverage

```yaml
- script: npm run coverage
  displayName: 'Generate coverage'

- task: PublishCodeCoverageResults@1
  inputs:
    codeCoverageTool: Cobertura
    summaryFileLocation: '$(System.DefaultWorkingDirectory)/coverage/cobertura-coverage.xml'
```

---

## 10. Best Practices

### Organize Pipeline

```yaml
trigger:
  - main

pr:
  - main

pool:
  vmImage: 'ubuntu-latest'

variables:
  buildConfiguration: 'Release'
  nodeVersion: '16.x'

stages:
- stage: Build
  jobs:
  - job: BuildJob
    steps:
    - task: NodeTool@0
      inputs:
        versionSpec: $(nodeVersion)
    - script: npm ci && npm run build

- stage: Test
  dependsOn: Build
  jobs:
  - job: TestJob
    steps:
    - script: npm test

- stage: Deploy
  dependsOn: Test
  condition: succeeded()
  jobs:
  - deployment: DeployApp
    environment: 'production'
    strategy:
      runOnce:
        deploy:
          steps:
          - script: npm run deploy
```

### Use Variable Groups

```yaml
variables:
  - group: 'BuildVariables'      # Share across pipelines
  - group: 'DatabaseSecrets'
  - name: localVar
    value: 'local'

steps:
- script: echo "Using group variables"
```

### Templates for Reusability

```yaml
# jobs/build.yml
parameters:
  - name: nodeVersion
    default: '16.x'

jobs:
- job: BuildJob
  steps:
  - task: NodeTool@0
    inputs:
      versionSpec: ${{ parameters.nodeVersion }}
  - script: npm run build

# azure-pipelines.yml
stages:
- stage: Build
  jobs:
  - template: jobs/build.yml
    parameters:
      nodeVersion: '18.x'
```

### Error Handling

```yaml
steps:
- script: npm run build
  displayName: 'Build'
  continueOnError: true
  # Pipeline continues even if this fails

- script: npm test
  displayName: 'Test'
  # Pipeline stops if this fails
```

### Secure Credentials

```yaml
steps:
- task: AzureCLI@2
  inputs:
    azureSubscription: 'Service Connection'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      az storage blob upload \
        --connection-string $(connString) \
        --container-name drop \
        --file dist/*
```

---

## 11. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/ |
| **YAML Schema** | https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/ |
| **Tasks** | https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/index |
| **Variables** | https://docs.microsoft.com/en-us/azure/devops/pipelines/build/variables |
| **Agents** | https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/agents |
| **Triggers** | https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers |
| **Environments** | https://docs.microsoft.com/en-us/azure/devops/pipelines/process/environments |
| **Templates** | https://docs.microsoft.com/en-us/azure/devops/pipelines/process/templates |
| **Best Practices** | https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/ |

### Community and Support

| Resource | URL |
|----------|-----|
| **DevOps Discussions** | https://developercommunity.visualstudio.com/ |
| **Stack Overflow** | https://stackoverflow.com/questions/tagged/azure-devops |
| **GitHub Issues** | https://github.com/microsoft/azure-pipelines-tasks/issues |
| **Status Page** | https://status.dev.azure.com/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Microsoft Learn** | https://learn.microsoft.com/en-us/azure/devops/pipelines/ |
| **Tutorial** | https://docs.microsoft.com/en-us/azure/devops/pipelines/create-first-pipeline |
| **Examples** | https://github.com/microsoft/azure-pipelines-yaml |
| **Task Library** | https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/index |

### Popular Extensions

| Extension | URL |
|-----------|-----|
| **SonarCloud** | https://marketplace.visualstudio.com/items?itemName=SonarSource.sonarcloud |
| **Docker** | https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker |
| **Kubernetes** | https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.vss-services-kubernetesdeploy |
| **GitHub** | https://marketplace.visualstudio.com/items?itemName=KalyanAkumarChintada.GitHub |

---

## Quick Reference

```yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

variables:
  buildConfiguration: 'Release'

stages:
- stage: Build
  jobs:
  - job: BuildJob
    steps:
    - task: NodeTool@0
      inputs:
        versionSpec: '16.x'
    - script: npm ci
    - script: npm run build
    - script: npm test
    - task: PublishBuildArtifacts@1
      inputs:
        pathToPublish: $(Build.ArtifactStagingDirectory)

- stage: Deploy
  dependsOn: Build
  jobs:
  - deployment: DeployApp
    environment: 'production'
    strategy:
      runOnce:
        deploy:
          steps:
          - script: npm run deploy
```

---

**Last Updated:** 2026
**Azure DevOps Version:** Latest
**License:** Microsoft

For latest information, visit [Azure DevOps Documentation](https://docs.microsoft.com/en-us/azure/devops/)