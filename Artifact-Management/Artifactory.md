# Artifactory Cheatsheet

![Artifactory Logo](https://jfrog.com/favicon.ico)

---

## Table of Contents
1. [What is Artifactory?](#1-what-is-artifactory)
2. [Core Concepts](#2-core-concepts)
3. [Installation](#3-installation)
4. [Repository Types](#4-repository-types)
5. [Uploading Artifacts](#5-uploading-artifacts)
6. [Downloading Artifacts](#6-downloading-artifacts)
7. [Users and Permissions](#7-users-and-permissions)
8. [AQL and Search](#8-aql-and-search)
9. [Promotion and Staging](#9-promotion-and-staging)
10. [Build Integration](#10-build-integration)
11. [Best Practices](#11-best-practices)
12. [Additional Resources](#12-additional-resources)

---

## 1. What is Artifactory?

Artifactory is a universal DevOps platform and repository manager by JFrog. It manages software artifacts of any format and size, providing intelligent caching, security scanning, and seamless CI/CD integration.

**Key Benefits:**
- Universal artifact repository for all formats
- Intelligent remote caching and proxy
- Fine-grained access control and permissions
- Advanced search with AQL (Artifactory Query Language)
- Build integration and artifact promotion
- High availability and replication
- REST API for automation
- Vulnerability scanning integration

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Repository** | Storage container for artifacts (local, remote, virtual) |
| **Local Repository** | Stores your organization's artifacts |
| **Remote Repository** | Proxies external repositories (caches) |
| **Virtual Repository** | Aggregates multiple repositories for unified access |
| **Artifact** | Software component (JAR, npm, Docker image, etc.) |
| **Build** | Complete artifact build with metadata |
| **Promotion** | Moving artifacts between repositories/environments |
| **AQL** | Query language for advanced artifact searching |
| **Blob Store** | Physical storage backend (filesystem, cloud) |

---

## 3. Installation

### Docker Installation

```bash
# Run Artifactory OSS (Open Source)
docker run -d --name artifactory \
  -p 8081:8081 \
  -p 8082:8082 \
  -v artifactory-data:/var/opt/jfrog/artifactory \
  releases-docker.jfrog.io/jfrog/artifactory-oss:latest

# Run Artifactory Pro
docker run -d --name artifactory \
  -p 8081:8081 \
  -p 8082:8082 \
  -v artifactory-data:/var/opt/jfrog/artifactory \
  releases-docker.jfrog.io/jfrog/artifactory-pro:latest

# Get admin password
docker logs artifactory | grep password

# Access at http://localhost:8081/artifactory
```

### Linux Installation

```bash
# Download Artifactory
wget https://releases.jfrog.io/artifactory/artifactory-oss/org/artifactory/oss/jfrog-artifactory-oss/7.49.x/jfrog-artifactory-oss-7.49.x-linux.tar.gz

# Extract
tar -xzf jfrog-artifactory-oss-7.49.x-linux.tar.gz
cd artifactory-oss-7.49.x/

# Start Artifactory
./bin/artifactoryctl.sh start

# Logs
./bin/artifactoryctl.sh tail

# Stop
./bin/artifactoryctl.sh stop

# Access at http://localhost:8081/artifactory
# Default: admin / password
```

### Kubernetes Helm Installation

```bash
# Add JFrog Helm repository
helm repo add jfrog https://charts.jfrog.io
helm repo update

# Install Artifactory
helm install artifactory jfrog/artifactory \
  --namespace artifactory \
  --create-namespace \
  --set artifactory.admin.password=mypassword

# Get access info
helm status artifactory -n artifactory
```

### Initial Setup

1. Navigate to http://localhost:8081/artifactory
2. Login with admin / default-password
3. Change admin password
4. Configure base URL
5. Create repositories
6. Configure blob stores if needed

---

## 4. Repository Types

### Create Local Repository (Maven)

```
Artifactory UI → Administration → Repositories → Repositories → Create Repository
├── Select Package Type: Maven
├── Specify Repository Key: my-releases
├── Repository Type: Local
├── Layout: Maven 2 & 3 (m2)
├── Suppress POM Consistency Checks: (optional)
├── Version Policy: Release
├── Checksum Policy: Client checksums
└── Create Local Repository
```

### Create Remote Repository (Proxy)

```
Artifactory UI → Create Repository
├── Package Type: Maven
├── Specify Repository Key: remote-maven-central
├── Repository Type: Remote
├── URL: https://repo1.maven.org/maven2/
├── Credentials: (if needed)
├── Username/Password: (for private repos)
├── Suppress POM Consistency Checks: false
└── Create Remote Repository
```

### Create Virtual Repository (Group)

```
Artifactory UI → Create Repository
├── Package Type: Maven
├── Specify Repository Key: maven-all
├── Repository Type: Virtual
├── Selected Repositories:
│   ├── my-releases (1st priority)
│   ├── my-snapshots
│   └── remote-maven-central
├── Default Deployment Repository: my-releases
└── Create Virtual Repository
```

### Maven Repository Configuration

```yaml
# pom.xml - Distribution Management
<distributionManagement>
  <repository>
    <id>artifactory-releases</id>
    <name>Artifactory Releases</name>
    <url>http://artifactory:8081/artifactory/my-releases</url>
  </repository>
  <snapshotRepository>
    <id>artifactory-snapshots</id>
    <name>Artifactory Snapshots</name>
    <url>http://artifactory:8081/artifactory/my-snapshots</url>
  </snapshotRepository>
</distributionManagement>

# settings.xml - Server Credentials
<servers>
  <server>
    <id>artifactory-releases</id>
    <username>admin</username>
    <password>{encrypted-password}</password>
  </server>
  <server>
    <id>artifactory-snapshots</id>
    <username>admin</username>
    <password>{encrypted-password}</password>
  </server>
</servers>

# Repositories
<repositories>
  <repository>
    <id>maven-all</id>
    <url>http://artifactory:8081/artifactory/maven-all</url>
    <snapshots>
      <enabled>true</enabled>
    </snapshots>
    <releases>
      <enabled>true</enabled>
    </releases>
  </repository>
</repositories>
```

### npm Repository Configuration

```
Artifactory UI → Create Repository
├── Package Type: npm
├── Repository Key: npm-internal
├── Repository Type: Local
├── Deployment: Allow redeploy, Suppress POM Consistency
└── Create
```

```bash
# .npmrc configuration
registry=http://artifactory:8081/artifactory/api/npm/npm-internal/
always-auth=true
_auth=${base64(username:password)}

# Or via npm login
npm login --registry=http://artifactory:8081/artifactory/api/npm/npm-internal/
npm login --scope=@mycompany --registry=http://artifactory:8081/artifactory/api/npm/npm-internal/

# Publish to npm
npm publish --registry=http://artifactory:8081/artifactory/api/npm/npm-internal/

# Install from npm
npm install @mycompany/my-package
```

### Docker Repository Configuration

```
Artifactory UI → Create Repository
├── Package Type: Docker
├── Repository Key: docker-internal
├── Repository Type: Local
├── Enable Docker API: Yes
├── Enable Docker API V2: Yes
├── HTTP Port: 5000
└── Create
```

```bash
# Configure Docker daemon (/etc/docker/daemon.json)
{
  "insecure-registries": ["artifactory:5000"],
  "registry-mirrors": ["http://artifactory:8081/artifactory/docker-remote/"]
}

# Restart Docker
sudo systemctl restart docker

# Login to Artifactory
docker login artifactory:5000 -u admin -p password

# Tag image
docker tag myapp:1.0.0 artifactory:5000/docker-internal/myapp:1.0.0

# Push image
docker push artifactory:5000/docker-internal/myapp:1.0.0

# Pull image
docker pull artifactory:5000/docker-internal/myapp:1.0.0
```

### Python (PyPI) Repository

```
Artifactory UI → Create Repository
├── Package Type: Python
├── Repository Key: pypi-internal
├── Repository Type: Local
├── Deployment: Allow redeploy
└── Create
```

```bash
# setup.py or setup.cfg
# With twine
twine upload -r artifactory dist/*

# pip.conf or .pypirc
[distutils]
index-servers =
  artifactory

[artifactory]
repository: http://artifactory:8081/artifactory/api/pypi/pypi-internal
username: admin
password: password123

# Install packages
pip install -i http://artifactory:8081/artifactory/api/pypi/pypi-internal/simple my-package
```

### Gradle Repository Configuration

```gradle
repositories {
  maven {
    url 'http://artifactory:8081/artifactory/maven-all/'
    credentials {
      username 'admin'
      password 'password123'
    }
  }
}

dependencies {
  implementation 'com.example:my-app:1.0.0'
}

// Publishing
publishing {
  repositories {
    maven {
      url 'http://artifactory:8081/artifactory/my-releases'
      credentials {
        username 'admin'
        password 'password123'
      }
    }
  }
}
```

---

## 5. Uploading Artifacts

### Maven Deploy

```bash
# Deploy via Maven
mvn deploy

# Deploy single artifact
mvn deploy:deploy-file \
  -DgroupId=com.example \
  -DartifactId=my-app \
  -Dversion=1.0.0 \
  -Dpackaging=jar \
  -Dfile=my-app-1.0.0.jar \
  -Durl=http://artifactory:8081/artifactory/my-releases \
  -DrepositoryId=artifactory-releases

# Deploy with sources and javadoc
mvn deploy:deploy-file \
  -DgroupId=com.example \
  -DartifactId=my-app \
  -Dversion=1.0.0 \
  -Dfile=my-app-1.0.0.jar \
  -Dsources=my-app-1.0.0-sources.jar \
  -Djavadoc=my-app-1.0.0-javadoc.jar \
  -Durl=http://artifactory:8081/artifactory/my-releases \
  -DrepositoryId=artifactory-releases
```

### npm Publish

```bash
# Configure registry
npm config set registry http://artifactory:8081/artifactory/api/npm/npm-internal/

# Login
npm login --registry=http://artifactory:8081/artifactory/api/npm/npm-internal/

# Publish
npm publish

# Publish scoped package
npm publish --access restricted
```

### Docker Push

```bash
# Login
docker login artifactory:5000 -u admin -p password123

# Build and tag
docker build -t myapp:1.0.0 .
docker tag myapp:1.0.0 artifactory:5000/docker-internal/myapp:1.0.0

# Push
docker push artifactory:5000/docker-internal/myapp:1.0.0

# Push multiple tags
docker tag myapp:1.0.0 artifactory:5000/docker-internal/myapp:latest
docker push artifactory:5000/docker-internal/myapp:latest
```

### REST API Upload

```bash
# Upload single file
curl -u admin:password123 -T my-app-1.0.0.jar \
  "http://artifactory:8081/artifactory/my-releases/com/example/myapp/1.0.0/my-app-1.0.0.jar"

# Upload with properties
curl -u admin:password123 \
  -H "X-Artifactory-Meta-Build-Name: my-build" \
  -H "X-Artifactory-Meta-Build-Number: 123" \
  -T my-app-1.0.0.jar \
  "http://artifactory:8081/artifactory/my-releases/com/example/myapp/1.0.0/my-app-1.0.0.jar"

# Upload directory
find ./dist -type f | while read file; do
  curl -u admin:password123 -T "$file" \
    "http://artifactory:8081/artifactory/my-releases/${file#./dist/}"
done
```

### Gradle Upload

```gradle
plugins {
  id "com.jfrog.artifactory" version "4.x.x"
}

artifactoryPublish {
  publications("mavenJava")
}

artifactory {
  contextUrl = 'http://artifactory:8081/artifactory'
  publish {
    repository {
      repoKey = 'my-releases'
      username = 'admin'
      password = 'password123'
    }
    defaults {
      publishArtifacts = true
      publishPom = true
    }
  }
}
```

---

## 6. Downloading Artifacts

### Maven Dependency

```xml
<!-- pom.xml -->
<dependency>
  <groupId>com.example</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0.0</version>
</dependency>

<!-- Automatically pulls from configured virtual repository -->
```

### npm Install

```bash
# Install from Artifactory
npm install @mycompany/my-package

# Install specific version
npm install @mycompany/my-package@1.0.0

# Install from package.json
npm install
```

### Docker Pull

```bash
# Pull from Artifactory
docker pull artifactory:5000/docker-internal/myapp:1.0.0

# Pull with tag
docker pull artifactory:5000/docker-internal/myapp:latest
```

### REST API Download

```bash
# Download single artifact
curl -u admin:password123 -O \
  "http://artifactory:8081/artifactory/my-releases/com/example/myapp/1.0.0/my-app-1.0.0.jar"

# Download with output filename
curl -u admin:password123 -o downloaded-file.jar \
  "http://artifactory:8081/artifactory/my-releases/com/example/myapp/1.0.0/my-app-1.0.0.jar"

# List directory
curl -u admin:password123 \
  "http://artifactory:8081/artifactory/api/storage/my-releases/com/example/myapp/1.0.0/"
```

### Gradle Dependencies

```gradle
repositories {
  maven {
    url 'http://artifactory:8081/artifactory/maven-all'
    credentials {
      username 'admin'
      password 'password123'
    }
  }
}

dependencies {
  implementation 'com.example:my-app:1.0.0'
  implementation 'com.example:my-lib:2.0.0'
}
```

---

## 7. Users and Permissions

### Create User

```
Artifactory UI → Administration → Users Management
├── New User
├── Username: developer1
├── Email: developer1@example.com
├── Password: strongpassword123
├── Admin: No
└── Create
```

### User Credentials

```bash
# Encrypt password for use in Maven/Gradle
# Use password encryption tool in Artifactory UI

# View user API key
# User Profile → Change Password → API Key
```

### Create Group

```
Artifactory UI → Administration → Groups
├── New Group
├── Group Name: development
├── Add Members: developer1, developer2
└── Create
```

### Permission Targets

```
Artifactory UI → Administration → Security → Permissions
├── New Permission
├── Name: developers-access
├── Repositories:
│   ├── Build Name: *
│   ├── Include: my-releases, my-snapshots
│   └── Actions: Manage, Manage Release Bundle, Manage Watchers
├── Users/Groups:
│   ├── Add Group: development
│   ├── Permissions: Read, Write, Delete, Manage
│   └── Save
└── Create
```

### Role-Based Access Control

```
Artifactory Roles:
├── Admin: Full access
├── Contributor: Push, deploy artifacts
├── Viewer: Read-only access
├── Deployer: Deploy artifacts to specific repos
└── Custom: Define custom permissions
```

---

## 8. AQL and Search

### Basic AQL Query

```bash
# Query all artifacts in repository
curl -u admin:password123 -X POST \
  "http://artifactory:8081/artifactory/api/search/aql" \
  -d 'items.find({"repo":"my-releases"})'

# Query specific artifact
curl -u admin:password123 -X POST \
  "http://artifactory:8081/artifactory/api/search/aql" \
  -d 'items.find({"repo":"my-releases","name":"my-app*"})'

# Query by properties
curl -u admin:password123 -X POST \
  "http://artifactory:8081/artifactory/api/search/aql" \
  -d 'items.find({"$or":[{"$and":[{"name":"*"},{"property.build_name":"my-build"}]}]})'
```

### Advanced AQL Queries

```bash
# Find artifacts older than specific date
curl -u admin:password123 -X POST \
  "http://artifactory:8081/artifactory/api/search/aql" \
  -d 'items.find({"repo":"my-releases","modified":{"$before":"2023-01-01"}})'

# Find artifacts by size
curl -u admin:password123 -X POST \
  "http://artifactory:8081/artifactory/api/search/aql" \
  -d 'items.find({"size":{"$gt":1000000}})'

# Find artifacts by checksum
curl -u admin:password123 -X POST \
  "http://artifactory:8081/artifactory/api/search/aql" \
  -d 'items.find({"sha1":"abc123def456..."})'

# Find artifacts by path pattern
curl -u admin:password123 -X POST \
  "http://artifactory:8081/artifactory/api/search/aql" \
  -d 'items.find({"path":"/com/example/*"})'

# Query with limit and sorting
curl -u admin:password123 -X POST \
  "http://artifactory:8081/artifactory/api/search/aql" \
  -d 'items.find({"repo":"my-releases"}).limit(10).offset(0).sort({"$desc":["created"]})'
```

### UI Search

```
Artifactory UI → Artifacts → Search
├── Quick Search: Search box (filename, path)
├── Advanced Search:
│   ├── Repository: my-releases
│   ├── Name: my-app*
│   ├── Type: Artifact
│   ├── Created: After 2023-01-01
│   ├── Modified: Before 2026-01-01
│   └── Search
```

---

## 9. Promotion and Staging

### Promote Artifacts

```bash
# Promote via REST API
curl -u admin:password123 -X POST \
  "http://artifactory:8081/artifactory/api/promote/my-app/1.0.0" \
  -H "Content-Type: application/json" \
  -d '{
    "status": "released",
    "comment": "Ready for production",
    "copy": true,
    "artifacts": true,
    "dependencies": false,
    "targetRepo": "production-releases"
  }'

# Promote with dry-run
curl -u admin:password123 -X POST \
  "http://artifactory:8081/artifactory/api/promote/my-app/1.0.0?dryrun=1" \
  -d '{...}'
```

### Promote via UI

```
Artifactory UI → Artifacts → Select artifact
├── More Actions → Promote
├── Target Repository: production-releases
├── Status: released
├── Comment: Ready for production
└── Promote
```

### Build Promotion

```bash
# Promote entire build
curl -u admin:password123 -X POST \
  "http://artifactory:8081/artifactory/api/promote/builds/my-build/1.0.0" \
  -d '{
    "status": "released",
    "targetRepo": "production-releases",
    "comment": "Promoted to production",
    "copy": true,
    "artifacts": true,
    "dependencies": false
  }'
```

### Promotion Policy

```
Artifactory UI → Administration → Advanced Security
├── Configure promotion rules
├── Source Repository: my-snapshots
├── Target Repository: my-releases
├── Automatic Promotion: (optional)
└── Save
```

---

## 10. Build Integration

### Capture Build Info (Maven)

```bash
# Install Maven Artifactory Plugin
mvn org.apache.maven.plugins:maven-help-plugin:describe \
  -Dplugin=org.jfrog.buildinfo:artifactory-maven-plugin

# Configure pom.xml
<plugin>
  <groupId>org.jfrog.buildinfo</groupId>
  <artifactId>artifactory-maven-plugin</artifactId>
  <version>3.x.x</version>
  <executions>
    <execution>
      <id>build-info</id>
      <goals>
        <goal>publish</goal>
      </goals>
    </execution>
  </executions>
</plugin>

# Deploy with build info
mvn clean install artifactory:publish \
  -Dartifactory.publish.contextUrl=http://artifactory:8081/artifactory \
  -Dartifactory.publish.repoKey=my-releases \
  -Dartifactory.publish.username=admin \
  -Dartifactory.publish.password=password123 \
  -Dartifactory.publish.buildName=my-build \
  -Dartifactory.publish.buildNumber=123
```

### Capture Build Info (Gradle)

```gradle
plugins {
  id "com.jfrog.artifactory" version "4.x.x"
  id "com.jfrog.build-info" version "4.x.x"
}

artifactory {
  contextUrl = 'http://artifactory:8081/artifactory'
  
  publish {
    repository {
      repoKey = 'my-releases'
      username = 'admin'
      password = 'password123'
    }
    defaults {
      publishArtifacts = true
      publishBuildInfo = true
      buildName = 'my-build'
      buildNumber = '123'
    }
  }
}

// Run: gradle build artifactoryPublish
```

### Build Information API

```bash
# Get build information
curl -u admin:password123 \
  "http://artifactory:8081/artifactory/api/build/my-build/123"

# Get build statistics
curl -u admin:password123 \
  "http://artifactory:8081/artifactory/api/builds/usage/stats"

# List all builds
curl -u admin:password123 \
  "http://artifactory:8081/artifactory/api/builds"
```

### View Build Info

```
Artifactory UI → Builds
├── Select Build: my-build
├── Build Number: 123
├── View:
│   ├── Summary: Build info, dependencies
│   ├── Dependencies: Used artifacts
│   ├── Artifacts: Generated artifacts
│   └── License: License information
```

---

## 11. Best Practices

### Repository Strategy

```
Artifactory Repository Structure:

Local Repositories:
├── my-releases: Release artifacts (final versions)
├── my-snapshots: Snapshot artifacts (development versions)
├── npm-internal: Internal npm packages
├── docker-internal: Internal Docker images
└── pypi-internal: Internal Python packages

Remote Repositories:
├── remote-maven-central: Maven Central proxy
├── remote-npm: npm registry proxy
├── remote-docker: Docker Hub proxy
└── remote-pypi: PyPI proxy

Virtual Repositories:
├── maven-all: Aggregates releases, snapshots, maven-central
├── npm-all: Aggregates npm-internal, remote-npm
├── docker-all: Aggregates docker-internal, remote-docker
└── pypi-all: Aggregates pypi-internal, remote-pypi
```

### Artifact Cleanup Policies

```
Artifactory UI → Administration → Repositories → Cleanup Policies
├── Policy Name: cleanup-old-snapshots
├── Includes:
│   ├── Repository: my-snapshots
│   ├── Include Patterns: **
│   └── Exclude Patterns: *-SNAPSHOT
├── Exclude:
│   ├── Artifacts not downloaded for 30 days
│   ├── Artifacts not modified for 90 days
│   └── Keep minimum: 10 builds
└── Apply Policy
```

### Security Best Practices

```
✓ Change default admin password
✓ Use strong API keys for automation
✓ Enable HTTPS/TLS
✓ Configure user authentication (LDAP, OAuth, SAML)
✓ Implement permission targets
✓ Audit repository access
✓ Regular backup of repository data
✓ Use service accounts for CI/CD
✓ Enable artifact signing and verification
✓ Scan artifacts for vulnerabilities
```

### Storage Optimization

```
Artifactory UI → Administration → Storage Management
├── Optimize Repository Storage:
│   ├── Remove unused artifacts
│   ├── Compress storage
│   └── Archive old builds
├── Configure Blob Store:
│   ├── Filesystem
│   ├── Database
│   └── Cloud (S3, GCS, Azure)
└── Monitor Disk Usage
```

### Version Management

```
Release Versions:
├── Format: 1.0.0, 1.1.0, 2.0.0 (semantic versioning)
├── Stored in: my-releases
├── Policy: No redeploy (immutable)
└── Retention: Keep indefinitely

Snapshot Versions:
├── Format: 1.0.0-SNAPSHOT
├── Stored in: my-snapshots
├── Policy: Allow redeploy
├── Retention: Keep last 10 builds or 30 days
└── Cleanup: Automatic via policies
```

### CI/CD Integration

```yaml
# Jenkins Pipeline example
pipeline {
  agent any
  
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean install'
      }
    }
    
    stage('Deploy to Artifactory') {
      steps {
        sh '''
          mvn deploy \
            -DaltDeploymentRepository=artifactory::default::http://artifactory:8081/artifactory/my-releases \
            -Dartifactory.publish.artifacts=true \
            -Dartifactory.publish.buildInfo=true
        '''
      }
    }
    
    stage('Promote') {
      steps {
        sh '''
          curl -u admin:${ARTIFACTORY_PASSWORD} -X POST \
            "http://artifactory:8081/artifactory/api/promote/${JOB_NAME}/${BUILD_NUMBER}" \
            -d '{"targetRepo":"production-releases","status":"released"}'
        '''
      }
    }
  }
}
```

---

## 12. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://jfrog.com/artifactory/ |
| **Documentation** | https://www.jfrog.com/confluence/display/JFROG/Artifactory |
| **Installation Guide** | https://www.jfrog.com/confluence/display/JFROG/Installing+Artifactory |
| **REST API** | https://www.jfrog.com/confluence/display/JFROG/Artifactory+REST+API |
| **AQL** | https://www.jfrog.com/confluence/display/JFROG/Artifactory+Query+Language |
| **Administration** | https://www.jfrog.com/confluence/display/JFROG/Administration |
| **Security** | https://www.jfrog.com/confluence/display/JFROG/Security+Configuration |
| **Maven Integration** | https://www.jfrog.com/confluence/display/JFROG/Maven+Repositories |

### Community and Support

| Resource | URL |
|----------|-----|
| **Community Forum** | https://www.jfrog.com/community/forums/ |
| **Slack Channel** | https://jfrog.slack.com/ |
| **GitHub Issues** | https://github.com/jfrog/artifactory-user-plugins/issues |
| **Support Portal** | https://support.jfrog.com/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Video Tutorials** | https://www.jfrog.com/resources/videos/ |
| **Docker Hub** | https://hub.docker.com/r/jfrog/artifactory-oss |
| **Maven Central** | https://mvnrepository.com/ |
| **API Examples** | https://github.com/jfrog/artifactory-examples |
| **Best Practices** | https://www.jfrog.com/confluence/display/JFROG/Best+Practices |

### Related Tools

| Tool | URL |
|------|-----|
| **JFrog CLI** | https://jfrog.com/jfrog-cli/ |
| **Xray** | https://jfrog.com/xray/ |
| **Mission Control** | https://jfrog.com/mission-control/ |
| **Pipelines** | https://jfrog.com/pipelines/ |

---

## Quick Reference

```bash
# Docker run
docker run -d --name artifactory \
  -p 8081:8081 \
  -v artifactory-data:/var/opt/jfrog/artifactory \
  releases-docker.jfrog.io/jfrog/artifactory-oss:latest

# Maven deploy
mvn deploy:deploy-file \
  -DgroupId=com.example \
  -DartifactId=my-app \
  -Dversion=1.0.0 \
  -Dfile=my-app-1.0.0.jar \
  -Durl=http://artifactory:8081/artifactory/my-releases \
  -DrepositoryId=artifactory-releases

# npm publish
npm publish --registry=http://artifactory:8081/artifactory/api/npm/npm-internal/

# Docker push
docker login artifactory:5000 -u admin -p password
docker tag myapp:1.0.0 artifactory:5000/docker-internal/myapp:1.0.0
docker push artifactory:5000/docker-internal/myapp:1.0.0

# REST API search
curl -u admin:password -X POST \
  http://artifactory:8081/artifactory/api/search/aql \
  -d 'items.find({"repo":"my-releases"})'

# Promote artifact
curl -u admin:password -X POST \
  http://artifactory:8081/artifactory/api/promote/my-app/1.0.0 \
  -d '{"targetRepo":"production-releases","status":"released"}'
```

---

**Last Updated:** 2026
**Artifactory Version:** 7.x+
**License:** Commercial / Open Source (OSS)

For latest information, visit [Artifactory Documentation](https://www.jfrog.com/confluence/display/JFROG/Artifactory)