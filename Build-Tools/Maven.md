# Maven Cheatsheet

![Maven Logo](https://maven.apache.org/images/maven-logo-black-on-white.png)

---

## Table of Contents
1. [What is Maven?](#1-what-is-maven)
2. [Core Concepts](#2-core-concepts)
3. [Installation](#3-installation)
4. [Project Structure](#4-project-structure)
5. [POM Configuration](#5-pom-configuration)
6. [Dependencies](#6-dependencies)
7. [Build Lifecycle](#7-build-lifecycle)
8. [Plugins](#8-plugins)
9. [Testing](#9-testing)
10. [Deployment](#10-deployment)
11. [Best Practices](#11-best-practices)
12. [Additional Resources](#12-additional-resources)

---

## 1. What is Maven?

Maven is a build automation tool primarily used for Java projects. It manages project dependencies, compiles code, runs tests, and packages applications using a declarative configuration file (pom.xml).

**Key Benefits:**
- Dependency management
- Project standardization
- Build automation
- Plugin extensibility
- Multi-module projects
- Repository management
- Consistent project structure

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **POM** | Project Object Model - configuration file (pom.xml) |
| **GroupId** | Organization identifier (e.g., com.example) |
| **ArtifactId** | Project identifier (e.g., my-app) |
| **Version** | Project version (e.g., 1.0.0) |
| **Dependency** | External library required by project |
| **Plugin** | Tool that extends Maven functionality |
| **Repository** | Storage for artifacts (local or remote) |
| **Lifecycle** | Predefined build process phases |

---

## 3. Installation

### macOS Installation

```bash
# Using Homebrew
brew install maven

# Verify
mvn --version

# Java requirement
java -version
```

### Linux Installation

```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install maven

# Or download manually
wget https://archive.apache.org/dist/maven/maven-3/3.8.x/binaries/apache-maven-3.8.x-bin.tar.gz
tar -xzf apache-maven-3.8.x-bin.tar.gz
sudo mv apache-maven-3.8.x /opt/maven

# Set PATH
export PATH=$PATH:/opt/maven/bin
```

### Windows Installation

```bash
# Download from https://maven.apache.org/download.cgi
# Extract to C:\Program Files\maven
# Add to PATH: C:\Program Files\maven\bin

# Verify
mvn --version
```

### Configuration

```bash
# Maven home
echo $MAVEN_HOME

# Settings file
~/.m2/settings.xml

# Local repository
~/.m2/repository
```

---

## 4. Project Structure

### Standard Maven Structure

```
my-app/
├── pom.xml                          # Project configuration
├── src/
│   ├── main/
│   │   ├── java/                    # Source code
│   │   │   └── com/example/App.java
│   │   ├── resources/               # Configuration files
│   │   │   └── application.properties
│   │   └── webapp/                  # Web resources (if web project)
│   │       └── index.html
│   └── test/
│       ├── java/                    # Test code
│       │   └── com/example/AppTest.java
│       └── resources/               # Test resources
├── target/                          # Build output
│   ├── classes/
│   ├── test-classes/
│   └── my-app-1.0.0.jar
├── .gitignore
└── README.md
```

### Create Project

```bash
# Create from archetype
mvn archetype:generate \
  -DgroupId=com.example \
  -DartifactId=my-app \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DinteractiveMode=false

# Create web project
mvn archetype:generate \
  -DgroupId=com.example \
  -DartifactId=my-web-app \
  -DarchetypeArtifactId=maven-archetype-webapp \
  -DinteractiveMode=false
```

---

## 5. POM Configuration

### Basic POM

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
  
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.example</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0.0</version>
  <packaging>jar</packaging>

  <name>My Application</name>
  <description>Example Maven Project</description>
  <url>https://github.com/username/my-app</url>

  <properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <!-- Dependencies here -->
  </dependencies>

  <build>
    <plugins>
      <!-- Plugins here -->
    </plugins>
  </build>

</project>
```

### POM with Parent

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.7.0</version>
  <relativePath/> <!-- lookup parent from repository -->
</parent>

<groupId>com.example</groupId>
<artifactId>my-app</artifactId>
<version>1.0.0</version>
<name>My Application</name>

<properties>
  <java.version>11</java.version>
</properties>

<dependencies>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
</dependencies>
```

### Properties

```xml
<properties>
  <maven.compiler.source>11</maven.compiler.source>
  <maven.compiler.target>11</maven.compiler.target>
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  
  <!-- Custom properties -->
  <spring.version>5.3.0</spring.version>
  <junit.version>4.13.2</junit.version>
  
  <!-- Reference: ${spring.version} -->
</properties>
```

---

## 6. Dependencies

### Adding Dependencies

```xml
<dependencies>
  <!-- JUnit for testing -->
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
  </dependency>

  <!-- Spring Framework -->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.3.0</version>
  </dependency>

  <!-- JSON processing -->
  <dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.9</version>
  </dependency>

  <!-- Logging -->
  <dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.32</version>
  </dependency>
</dependencies>
```

### Dependency Scopes

```xml
<!-- compile (default) - included in build -->
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.11.0</version>
  <scope>compile</scope>
</dependency>

<!-- test - only for testing -->
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.13.2</version>
  <scope>test</scope>
</dependency>

<!-- provided - compiled but not packaged -->
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>4.0.1</version>
  <scope>provided</scope>
</dependency>

<!-- runtime - not compiled -->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.28</version>
  <scope>runtime</scope>
</dependency>
```

### Dependency Management

```xml
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-framework-bom</artifactId>
      <version>5.3.0</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>

<!-- Later, use without version -->
<dependencies>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
  </dependency>
</dependencies>
```

### Exclusions

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-core</artifactId>
  <version>5.3.0</version>
  <exclusions>
    <exclusion>
      <groupId>commons-logging</groupId>
      <artifactId>commons-logging</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

---

## 7. Build Lifecycle

### Lifecycle Phases

```
validate    → Validate project structure
compile     → Compile source code
test        → Run unit tests
package     → Package compiled code (JAR/WAR)
integration-test → Run integration tests
verify      → Verify package integrity
install     → Install to local repository
deploy      → Deploy to remote repository
```

### Running Lifecycle

```bash
# Compile
mvn compile

# Test
mvn test

# Package
mvn package

# Install to local repository
mvn install

# Deploy to remote repository
mvn deploy

# Full build
mvn clean install

# Skip tests
mvn clean install -DskipTests

# Skip test compilation
mvn clean install -Dmaven.test.skip=true
```

### Custom Lifecycle

```bash
# Run specific phase and all before it
mvn compile      # compile + validate
mvn test         # compile, test-compile + validate
mvn package      # all phases up to package

# Run specific goals
mvn clean
mvn compile
mvn test
mvn package
```

---

## 8. Plugins

### Common Plugins

```xml
<plugins>
  <!-- Compiler Plugin -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.8.1</version>
    <configuration>
      <source>11</source>
      <target>11</target>
    </configuration>
  </plugin>

  <!-- Surefire Plugin (Testing) -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>2.22.2</version>
    <configuration>
      <includes>
        <include>**/*Test.java</include>
        <include>**/*Tests.java</include>
      </includes>
    </configuration>
  </plugin>

  <!-- Jar Plugin -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.2.0</version>
    <configuration>
      <archive>
        <manifest>
          <mainClass>com.example.Main</mainClass>
        </manifest>
      </archive>
    </configuration>
  </plugin>

  <!-- Assembly Plugin -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-assembly-plugin</artifactId>
    <version>3.3.0</version>
    <executions>
      <execution>
        <phase>package</phase>
        <goals>
          <goal>single</goal>
        </goals>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </execution>
    </executions>
  </plugin>
</plugins>
```

### Spring Boot Plugin

```xml
<plugin>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-maven-plugin</artifactId>
  <version>2.7.0</version>
  <executions>
    <execution>
      <goals>
        <goal>repackage</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

### Docker Plugin

```xml
<plugin>
  <groupId>io.fabric8</groupId>
  <artifactId>docker-maven-plugin</artifactId>
  <version>0.38.0</version>
  <configuration>
    <images>
      <image>
        <name>myapp:${project.version}</name>
        <build>
          <from>openjdk:11-jre-slim</from>
          <assembly>
            <basedir>/app</basedir>
            <descriptorRef>artifact</descriptorRef>
          </assembly>
          <cmd>
            <shell>java -jar /app/${project.build.finalName}.jar</shell>
          </cmd>
        </build>
      </image>
    </images>
  </configuration>
</plugin>
```

---

## 9. Testing

### Add Test Dependencies

```xml
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.13.2</version>
  <scope>test</scope>
</dependency>

<dependency>
  <groupId>org.mockito</groupId>
  <artifactId>mockito-core</artifactId>
  <version>4.0.0</version>
  <scope>test</scope>
</dependency>
```

### Run Tests

```bash
# Run all tests
mvn test

# Run specific test class
mvn test -Dtest=AppTest

# Run specific test method
mvn test -Dtest=AppTest#testAdd

# Skip tests during build
mvn package -DskipTests

# Run tests with specific pattern
mvn test -Dtest=*Service
```

### Test Configuration

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-surefire-plugin</artifactId>
  <version>2.22.2</version>
  <configuration>
    <includes>
      <include>**/*Test.java</include>
      <include>**/*Tests.java</include>
    </includes>
    <parallel>methods</parallel>
    <threadCount>4</threadCount>
  </configuration>
</plugin>
```

### Code Coverage

```xml
<plugin>
  <groupId>org.jacoco</groupId>
  <artifactId>jacoco-maven-plugin</artifactId>
  <version>0.8.7</version>
  <executions>
    <execution>
      <goals>
        <goal>prepare-agent</goal>
      </goals>
    </execution>
    <execution>
      <id>report</id>
      <phase>test</phase>
      <goals>
        <goal>report</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

---

## 10. Deployment

### Deploy to Repository

```xml
<distributionManagement>
  <repository>
    <id>nexus-releases</id>
    <name>Nexus Release Repository</name>
    <url>http://nexus:8081/repository/releases/</url>
  </repository>
  <snapshotRepository>
    <id>nexus-snapshots</id>
    <name>Nexus Snapshot Repository</name>
    <url>http://nexus:8081/repository/snapshots/</url>
  </snapshotRepository>
</distributionManagement>
```

### settings.xml Configuration

```xml
<servers>
  <server>
    <id>nexus-releases</id>
    <username>admin</username>
    <password>{encrypted-password}</password>
  </server>
  <server>
    <id>nexus-snapshots</id>
    <username>admin</username>
    <password>{encrypted-password}</password>
  </server>
</servers>
```

### Deploy Command

```bash
# Deploy to repository
mvn deploy

# Deploy single artifact
mvn deploy:deploy-file \
  -DgroupId=com.example \
  -DartifactId=my-app \
  -Dversion=1.0.0 \
  -Dfile=my-app.jar \
  -Durl=http://nexus:8081/repository/releases/ \
  -DrepositoryId=nexus-releases
```

---

## 11. Best Practices

### Version Management

```xml
<!-- SNAPSHOT for development -->
<version>1.0.0-SNAPSHOT</version>

<!-- RELEASE for production -->
<version>1.0.0</version>

<!-- Semantic versioning: MAJOR.MINOR.PATCH -->
<!-- 1.0.0 = Major version 1, Minor 0, Patch 0 -->
```

### Module Structure

```xml
<!-- Parent POM -->
<packaging>pom</packaging>

<modules>
  <module>common</module>
  <module>api</module>
  <module>web</module>
</modules>
```

### Properties Usage

```xml
<properties>
  <java.version>11</java.version>
  <maven.compiler.source>${java.version}</maven.compiler.source>
  <maven.compiler.target>${java.version}</maven.compiler.target>
  <spring.version>5.3.0</spring.version>
</properties>

<!-- Reference properties -->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-core</artifactId>
  <version>${spring.version}</version>
</dependency>
```

### Build Profiles

```xml
<profiles>
  <profile>
    <id>dev</id>
    <properties>
      <maven.test.skip>false</maven.test.skip>
    </properties>
  </profile>
  
  <profile>
    <id>prod</id>
    <properties>
      <maven.test.skip>true</maven.test.skip>
    </properties>
  </profile>
</profiles>

<!-- Run with profile -->
<!-- mvn clean install -Pprod -->
```

### CI/CD Integration

```yaml
# GitHub Actions example
- name: Build with Maven
  run: mvn clean install

- name: Deploy
  run: mvn deploy -DskipTests
  env:
    NEXUS_USERNAME: ${{ secrets.NEXUS_USERNAME }}
    NEXUS_PASSWORD: ${{ secrets.NEXUS_PASSWORD }}
```

---

## 12. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://maven.apache.org/guides/getting-started/ |
| **POM Reference** | https://maven.apache.org/pom.html |
| **Plugin List** | https://maven.apache.org/plugins/ |
| **Lifecycle Reference** | https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html |
| **Dependency Management** | https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html |
| **Repository** | https://mvnrepository.com/ |
| **Central Repository** | https://repo1.maven.org/maven2/ |

### Community and Support

| Resource | URL |
|----------|-----|
| **GitHub Repository** | https://github.com/apache/maven |
| **Mailing Lists** | https://maven.apache.org/mailing-lists.html |
| **Stack Overflow** | https://stackoverflow.com/questions/tagged/maven |
| **Issue Tracker** | https://issues.apache.org/jira/browse/MNG |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Tutorial** | https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html |
| **POM Guide** | https://maven.apache.org/guides/introduction/introduction-to-the-pom.html |
| **Best Practices** | https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html |
| **Archetype List** | https://maven.apache.org/archetypes/ |

---

## Quick Reference

```bash
# Create project
mvn archetype:generate -DgroupId=com.example -DartifactId=my-app

# Build phases
mvn clean          # Clean target directory
mvn compile        # Compile source
mvn test          # Run tests
mvn package       # Create JAR/WAR
mvn install       # Install to local repo
mvn deploy        # Deploy to remote repo

# Full build
mvn clean install

# Skip tests
mvn clean install -DskipTests

# Build specific profile
mvn clean install -Pprod

# View dependencies
mvn dependency:tree

# Update dependencies
mvn dependency:sources
mvn dependency:resolve

# Run specific test
mvn test -Dtest=AppTest

# Force update snapshots
mvn clean install -U
```

---

**Last Updated:** 2024
**Maven Version:** 3.8.x+
**License:** Apache License 2.0

For latest information, visit [Maven Official Site](https://maven.apache.org/)