# Nexus Repository Manager Cheatsheet

![Nexus Logo](https://www.sonatype.com/hubfs/Sonatype_August2020/Images/nexus-repo-icon.png)

---

## Table of Contents
1. [What is Nexus?](#1-what-is-nexus)
2. [Core Concepts](#2-core-concepts)
3. [Installation](#3-installation)
4. [Repository Types](#4-repository-types)
5. [Uploading Artifacts](#5-uploading-artifacts)
6. [Downloading Artifacts](#6-downloading-artifacts)
7. [Users and Roles](#7-users-and-roles)
8. [Cleanup and Maintenance](#8-cleanup-and-maintenance)
9. [Security](#9-security)
10. [Best Practices](#10-best-practices)
11. [Additional Resources](#11-additional-resources)

---

## 1. What is Nexus?

Nexus Repository Manager is a universal artifact repository manager by Sonatype. It manages software components, dependencies, and artifacts across Maven, npm, Docker, PyPI, and many other formats.

**Key Benefits:**
- Universal repository for all artifact types
- Proxy external repositories (Maven Central, npm, etc.)
- Private artifact hosting
- Access control and security
- Artifact search and cleanup policies
- Container image management

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Repository** | Storage for artifacts (hosted, proxy, or group) |
| **Hosted Repository** | Stores your own artifacts |
| **Proxy Repository** | Caches external repositories |
| **Group Repository** | Aggregates multiple repositories |
| **Artifact** | Software component (JAR, npm, Docker image, etc.) |
| **Component** | Individual artifact stored in repository |
| **Asset** | File within a component |
| **Blob Store** | Physical storage backend |

---

## 3. Installation

### Docker Installation

```bash
# Run Nexus in Docker
docker run -d --name nexus \
  -p 8081:8081 \
  -p 8082:8082 \
  -p 8083:8083 \
  -v nexus-data:/nexus-data \
  sonatype/nexus3:latest

# Get admin password
docker exec nexus cat /nexus-data/admin.password

# Access at http://localhost:8081
```

### Linux Installation

```bash
# Download
wget https://download.sonatype.com/nexus/3/nexus-3.x.x-unix.tar.gz

# Extract
tar -xzf nexus-3.x.x-unix.tar.gz

# Move to /opt
sudo mv nexus-3.x.x /opt/nexus
sudo mv sonatype-work /opt/sonatype-work

# Create nexus user
sudo useradd nexus
sudo chown -R nexus:nexus /opt/nexus /opt/sonatype-work

# Start Nexus
/opt/nexus/bin/nexus start

# Access at http://localhost:8081
```

### Initial Setup

1. Navigate to http://localhost:8081
2. Login with admin / <password>
3. Change admin password
4. Create repositories
5. Configure blob stores if needed

---

## 4. Repository Types

### Hosted Repository (Maven)

```
Nexus UI → Administration → Repositories → Create Repository
├── Repository Format: maven2
├── Repository Type: Hosted
├── Name: my-releases
├── Version Policy: Release
├── Layout Policy: Strict
└── Deployment Policy: Allow redeploy
```

### Maven Configuration

```xml
<!-- pom.xml -->
<distributionManagement>
  <repository>
    <id>nexus-releases</id>
    <url>http://nexus:8081/repository/my-releases/</url>
  </repository>
  <snapshotRepository>
    <id>nexus-snapshots</id>
    <url>http://nexus:8081/repository/my-snapshots/</url>
  </snapshotRepository>
</distributionManagement>

<!-- settings.xml -->
<servers>
  <server>
    <id>nexus-releases</id>
    <username>admin</username>
    <password>password123</password>
  </server>
  <server>
    <id>nexus-snapshots</id>
    <username>admin</username>
    <password>password123</password>
  </server>
</servers>

<profiles>
  <profile>
    <id>nexus</id>
    <repositories>
      <repository>
        <id>nexus-releases</id>
        <url>http://nexus:8081/repository/my-releases/</url>
        <snapshots>
          <enabled>false</enabled>
        </snapshots>
      </repository>
      <repository>
        <id>nexus-snapshots</id>
        <url>http://nexus:8081/repository/my-snapshots/</url>
        <releases>
          <enabled>false</enabled>
        </releases>
      </repository>
    </repositories>
  </profile>
</profiles>

<activeProfiles>
  <activeProfile>nexus</activeProfile>
</activeProfiles>
```

### npm Repository

```
Nexus UI → Administration → Repositories → Create Repository
├── Repository Format: npm
├── Repository Type: Hosted
├── Name: npm-internal
└── Enable Strict Content Type Validation
```

### npm Configuration

```bash
# .npmrc
registry=http://nexus:8081/repository/npm-internal/

# Or configure via CLI
npm config set registry http://nexus:8081/repository/npm-internal/

# Set credentials
npm login --registry=http://nexus:8081/repository/npm-internal/

# Or in .npmrc
//nexus:8081/repository/npm-internal/:username=admin
//nexus:8081/repository/npm-internal/:_password=base64(password)
//nexus:8081/repository/npm-internal/:email=admin@example.com
```

### Docker Repository

```
Nexus UI → Administration → Repositories → Create Repository
├── Repository Format: docker
├── Repository Type: Hosted
├── Name: docker-internal
├── HTTP Connector Port: 5000
└── Enable Docker API v2
```

### Docker Configuration

```bash
# Configure Docker daemon
# /etc/docker/daemon.json
{
  "insecure-registries": ["nexus:5000"]
}

# Login
docker login nexus:5000 -u admin -p password123

# Tag image
docker tag myapp:latest nexus:5000/myapp:latest

# Push image
docker push nexus:5000/myapp:latest

# Pull image
docker pull nexus:5000/myapp:latest
```

### Proxy Repository

```
Nexus UI → Administration → Repositories → Create Repository
├── Repository Format: maven2 (or npm, docker, etc.)
├── Repository Type: Proxy
├── Name: maven-central-proxy
├── Remote Storage: https://repo1.maven.org/maven2/
└── Enable Negative Cache
```

### Group Repository

```
Nexus UI → Administration → Repositories → Create Repository
├── Repository Format: maven2
├── Repository Type: Group
├── Name: maven-all
├── Member Repositories:
│   ├── maven-internal (hosted)
│   ├── maven-snapshots (hosted)
│   └── maven-central-proxy (proxy)
└── Order: Matters (top to bottom)
```

---

## 5. Uploading Artifacts

### Maven Deploy

```bash
# Deploy release
mvn deploy:deploy-file \
  -DgroupId=com.example \
  -DartifactId=my-app \
  -Dversion=1.0.0 \
  -Dpackaging=jar \
  -Dfile=my-app-1.0.0.jar \
  -Durl=http://nexus:8081/repository/my-releases/ \
  -DrepositoryId=nexus-releases
```

### npm Publish

```bash
# Publish package
npm publish --registry=http://nexus:8081/repository/npm-internal/

# Or in package.json
{
  "name": "@mycompany/my-app",
  "version": "1.0.0",
  "publishConfig": {
    "registry": "http://nexus:8081/repository/npm-internal/"
  }
}

npm publish
```

### Docker Push

```bash
# Tag and push
docker tag myapp:1.0.0 nexus:5000/myapp:1.0.0
docker push nexus:5000/myapp:1.0.0

# Push multiple tags
docker tag myapp:1.0.0 nexus:5000/myapp:latest
docker push nexus:5000/myapp:latest
```

### PyPI Upload

```bash
# Configure in ~/.pypirc
[distutils]
index-servers =
  nexus

[nexus]
repository: http://nexus:8081/repository/pypi-internal/
username: admin
password: password123

# Upload
twine upload -r nexus dist/*
```

### REST API Upload

```bash
# Upload artifact via REST API
curl -v -u admin:password123 \
  --upload-file myapp-1.0.0.jar \
  'http://nexus:8081/repository/my-releases/com/example/myapp/1.0.0/myapp-1.0.0.jar'
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

<!-- Uses configured repository (group) -->
```

### npm Install

```bash
# Install from Nexus
npm install @mycompany/my-app

# From package.json
{
  "dependencies": {
    "@mycompany/my-app": "1.0.0"
  }
}

npm install
```

### Gradle

```gradle
repositories {
  maven {
    url 'http://nexus:8081/repository/maven-all/'
    credentials {
      username 'admin'
      password 'password123'
    }
  }
}

dependencies {
  implementation 'com.example:my-app:1.0.0'
}
```

### Docker Pull

```bash
docker pull nexus:5000/myapp:1.0.0
```

### REST API Download

```bash
# List artifacts
curl -u admin:password123 \
  'http://nexus:8081/service/rest/v1/components?repository=my-releases'

# Download artifact
curl -u admin:password123 \
  -O 'http://nexus:8081/repository/my-releases/com/example/myapp/1.0.0/myapp-1.0.0.jar'
```

---

## 7. Users and Roles

### Create User

```
Nexus UI → Administration → Security → Users → Create User
├── Username: developer1
├── First Name: John
├── Last Name: Doe
├── Email: john@example.com
├── Password: strongpass123
└── Status: Active
```

### Create Role

```
Nexus UI → Administration → Security → Roles → Create Role
├── Role ID: developers
├── Role Name: Developers
├── Privileges:
│   ├── nx-repository-view-*-*-read
│   ├── nx-repository-view-*-*-browse
│   └── nx-repository-view-*-my-releases-*
└── Members: developer1, developer2
```

### Assign Role to User

```
Nexus UI → Administration → Security → Users
├── Select user
├── Roles:
│   ├── Add: developers
│   └── Save
```

---

## 8. Cleanup and Maintenance

### Cleanup Policy

```
Nexus UI → Administration → Repository → Cleanup Policies
├── Name: cleanup-old-artifacts
├── Format: Maven
├── Criteria:
│   ├── Last downloaded: 30 days
│   ├── Last updated: 90 days
│   └── Is pre-release: Include
└── Apply to repositories
```

### Delete Old Artifacts

```bash
# Via CLI
curl -X DELETE -u admin:password123 \
  'http://nexus:8081/service/rest/v1/components?repository=my-releases&lastBlobUpdatedBefore=2023-01-01'

# Via UI: Search for artifacts → Select → Delete
```

### Maintenance Tasks

```
Nexus UI → Administration → System → Tasks
├── Create Task
├── Type: Database Cleanup
├── Recurrence: Daily
└── Run time: 2 AM
```

### Blob Store Cleanup

```
Nexus UI → Administration → System → Tasks
├── Type: Blob store Compact
├── Blob Store: default
└── Schedule: Weekly
```

---

## 9. Security

### HTTPS Configuration

```
Nexus UI → Administration → System → HTTP
├── Enable HTTPS
├── Keystore File: /path/to/keystore.jks
├── Keystore Password: ****
└── Key Password: ****
```

### Anonymous Access

```
Nexus UI → Administration → Security → Settings
├── Allow Anonymous Access: Yes/No
├── Enable Realms:
│   ├── LDAP
│   ├── Local Authenticating Realm
│   └── Local Authorizing Realm
```

### LDAP Integration

```
Nexus UI → Administration → Security → Realms
├── Name: LDAP
├── Protocol: ldap/ldaps
├── Server: ldap.example.com
├── Port: 389
├── Search Base DN: dc=example,dc=com
└── Enable
```

### Repository Permissions

```
Nexus UI → Administration → Security → Realms & Privileges
├── Create Privilege
├── Type: Repository
├── Repository: my-releases
├── Actions: read, browse, edit, add, delete
└── Role assignment
```

---

## 10. Best Practices

### Repository Organization

```
Nexus Repositories:
├── Hosted:
│   ├── my-releases (release artifacts)
│   ├── my-snapshots (snapshot artifacts)
│   ├── npm-internal (npm packages)
│   └── docker-internal (Docker images)
├── Proxy:
│   ├── maven-central
│   ├── npm-central
│   └── docker-hub
└── Group:
    ├── maven-all (combines releases, snapshots, central)
    ├── npm-all (combines internal, npm-central)
    └── docker-all (combines internal, docker-hub)
```

### Version Management

```
Release artifacts: 1.0.0, 1.1.0, 2.0.0
Snapshot artifacts: 1.0.0-SNAPSHOT (temporary)

Nexus configuration:
├── Release repo: Version policy = Release
├── Snapshot repo: Version policy = Snapshot
└── Cleanup: Remove snapshots older than 30 days
```

### Security Best Practices

```
✓ Change default admin password
✓ Use HTTPS/TLS
✓ Create limited-privilege users
✓ Enable LDAP/Active Directory
✓ Audit artifact access
✓ Restrict public repositories
✓ Use strong authentication tokens
✓ Regular backups of repositories
```

### CI/CD Integration

```yaml
# Jenkins example
pipeline {
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean install'
      }
    }
    
    stage('Deploy to Nexus') {
      steps {
        sh 'mvn deploy -DskipTests'
      }
    }
  }
}
```

### Artifact Naming Convention

```
Maven:
├── com/example/my-app/1.0.0/my-app-1.0.0.jar
└── com/example/my-app/1.0.0-SNAPSHOT/my-app-1.0.0-SNAPSHOT.jar

npm:
├── @mycompany/my-app/1.0.0
└── @mycompany/my-app/1.0.0-beta.1

Docker:
├── nexus:5000/myapp:1.0.0
└── nexus:5000/myapp:latest
```

---

## 11. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://help.sonatype.com/repomanager3 |
| **Repository Formats** | https://help.sonatype.com/repomanager3/formats |
| **Maven** | https://help.sonatype.com/repomanager3/formats/maven-repositories |
| **npm** | https://help.sonatype.com/repomanager3/formats/npm-registry |
| **Docker** | https://help.sonatype.com/repomanager3/formats/docker-registry |
| **REST API** | https://help.sonatype.com/repomanager3/integrations/rest-and-integration-api |
| **Security** | https://help.sonatype.com/repomanager3/security |
| **Administration** | https://help.sonatype.com/repomanager3/administration |

### Community and Support

| Resource | URL |
|----------|-----|
| **Community Forum** | https://community.sonatype.com/ |
| **Issues** | https://issues.sonatype.org/ |
| **Gitter Chat** | https://gitter.im/sonatype/nexus-developers |
| **Support** | https://support.sonatype.com/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Tutorial** | https://help.sonatype.com/repomanager3/getting-started |
| **Video Guide** | https://www.sonatype.com/products/repository-oss-videos |
| **Best Practices** | https://help.sonatype.com/repomanager3/best-practices |
| **Docker Hub** | https://hub.docker.com/r/sonatype/nexus3 |

### Related Tools

| Tool | URL |
|------|-----|
| **Sonatype CLI** | https://github.com/sonatype-nexus-community/sonatype-cli |
| **Maven** | https://maven.apache.org/ |
| **Gradle** | https://gradle.org/ |
| **npm** | https://www.npmjs.com/ |

---

## Quick Reference

```bash
# Docker run
docker run -d --name nexus -p 8081:8081 -v nexus-data:/nexus-data sonatype/nexus3

# Maven deploy
mvn deploy:deploy-file -DgroupId=com.example -DartifactId=my-app \
  -Dversion=1.0.0 -Dpackaging=jar -Dfile=my-app.jar \
  -Durl=http://nexus:8081/repository/releases/ -DrepositoryId=nexus

# npm publish
npm publish --registry=http://nexus:8081/repository/npm-internal/

# Docker push
docker tag myapp:1.0.0 nexus:5000/myapp:1.0.0
docker push nexus:5000/myapp:1.0.0

# List artifacts (REST API)
curl -u admin:password http://nexus:8081/service/rest/v1/components?repository=releases
```

---

**Last Updated:** 2026
**Nexus Version:** 3.x+
**License:** Open Source/Pro

For latest information, visit [Nexus Repository Manager](https://help.sonatype.com/repomanager3)