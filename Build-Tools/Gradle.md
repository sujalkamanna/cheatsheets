# Gradle Cheatsheet

![Gradle Logo](https://gradle.org/images/gradle-logo.png)

---

## Table of Contents
1. [What is Gradle?](#1-what-is-gradle)
2. [Core Concepts](#2-core-concepts)
3. [Installation](#3-installation)
4. [Project Structure](#4-project-structure)
5. [Build Script Basics](#5-build-script-basics)
6. [Dependencies](#6-dependencies)
7. [Tasks](#7-tasks)
8. [Plugins](#8-plugins)
9. [Testing](#9-testing)
10. [Deployment](#10-deployment)
11. [Best Practices](#11-best-practices)
12. [Additional Resources](#12-additional-resources)

---

## 1. What is Gradle?

Gradle is a flexible, open-source build automation tool that uses Groovy or Kotlin DSL for configuration. It's faster than Maven, supports incremental builds, and provides powerful dependency management with plugin extensibility.

**Key Benefits:**
- Faster builds (incremental compilation)
- Flexible build automation
- Groovy/Kotlin DSL (more readable than XML)
- Powerful dependency management
- Plugin-based architecture
- Multi-project builds
- Gradle Wrapper for consistency

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Build Script** | build.gradle file with build configuration |
| **Task** | Unit of work (compile, test, package) |
| **Project** | Module or component being built |
| **Plugin** | Extends Gradle functionality |
| **Dependency** | External library required by project |
| **Repository** | Storage for artifacts (local or remote) |
| **Configuration** | Named set of dependencies |
| **Artifact** | Output of build process |

---

## 3. Installation

### macOS Installation

```bash
# Using Homebrew
brew install gradle

# Verify
gradle --version

# Java requirement
java -version
```

### Linux Installation

```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install gradle

# Or download manually
wget https://services.gradle.org/distributions/gradle-7.6-bin.zip
unzip gradle-7.6-bin.zip
sudo mv gradle-7.6 /opt/gradle

# Set PATH
export PATH=$PATH:/opt/gradle/bin
```

### Windows Installation

```bash
# Download from https://gradle.org/releases/
# Extract to C:\Program Files\gradle
# Add to PATH: C:\Program Files\gradle\bin

# Verify
gradle --version
```

### Using Gradle Wrapper

```bash
# Initialize wrapper (recommended)
gradle wrapper --gradle-version=7.6

# Use wrapper instead of gradle
./gradlew build      # macOS/Linux
gradlew.bat build    # Windows

# Benefits: consistent Gradle version across team
```

---

## 4. Project Structure

### Standard Gradle Structure

```
my-app/
├── build.gradle                 # Build configuration (Groovy)
├── build.gradle.kts            # Build configuration (Kotlin)
├── settings.gradle             # Multi-project configuration
├── gradle/
│   └── wrapper/
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── src/
│   ├── main/
│   │   ├── java/               # Source code
│   │   │   └── com/example/App.java
│   │   ├── kotlin/             # Kotlin source
│   │   └── resources/          # Configuration files
│   └── test/
│       ├── java/               # Test code
│       ├── kotlin/
│       └── resources/          # Test resources
├── build/                      # Build output
│   ├── classes/
│   ├── libs/
│   └── reports/
├── .gitignore
└── README.md
```

### Create Project

```bash
# Initialize Gradle project
gradle init

# Choose build script DSL:
# 1. Groovy (build.gradle)
# 2. Kotlin (build.gradle.kts)

# Choose project type:
# 1. Basic
# 2. Application
# 3. Library
# 4. Gradle plugin
```

---

## 5. Build Script Basics

### Basic build.gradle (Groovy)

```groovy
plugins {
    id 'java'
    id 'application'
}

group = 'com.example'
version = '1.0.0'

repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'junit:junit:4.13.2'
}

application {
    mainClass = 'com.example.App'
}
```

### Basic build.gradle.kts (Kotlin)

```kotlin
plugins {
    java
    application
}

group = "com.example"
version = "1.0.0"

repositories {
    mavenCentral()
}

dependencies {
    testImplementation("junit:junit:4.13.2")
}

application {
    mainClass.set("com.example.App")
}
```

### Project Properties

```groovy
// build.gradle
project.group = 'com.example'
project.version = '1.0.0'
project.description = 'My Application'

// Or
group 'com.example'
version '1.0.0'
description 'My Application'

// Custom properties
ext {
    javaVersion = 11
    springVersion = '5.3.0'
}

// Usage: $javaVersion, $springVersion
```

### Settings Configuration

```groovy
// settings.gradle
rootProject.name = 'my-app'

// Multi-project
include 'common'
include 'api'
include 'web'

// Project properties
gradle.ext.javaVersion = 11
```

---

## 6. Dependencies

### Add Dependencies

```groovy
dependencies {
    // Implementation dependencies (compile and runtime)
    implementation 'org.springframework:spring-core:5.3.0'
    implementation 'com.google.code.gson:gson:2.8.9'
    
    // Test dependencies
    testImplementation 'junit:junit:4.13.2'
    testImplementation 'org.mockito:mockito-core:4.0.0'
    
    // Runtime only
    runtimeOnly 'mysql:mysql-connector-java:8.0.28'
    
    // Compile only (not runtime)
    compileOnly 'javax.servlet:javax.servlet-api:4.0.1'
}
```

### Dependency Configurations

```groovy
// Groovy
configurations {
    // Custom configuration
    custom
}

dependencies {
    custom 'org.example:lib:1.0.0'
}

// Kotlin
val custom by configurations.creating

dependencies {
    custom("org.example:lib:1.0.0")
}
```

### Version Management

```groovy
// Define versions
def versions = [
    spring: '5.3.0',
    junit: '4.13.2',
    gson: '2.8.9'
]

dependencies {
    implementation "org.springframework:spring-core:${versions.spring}"
    testImplementation "junit:junit:${versions.junit}"
    implementation "com.google.code.gson:gson:${versions.gson}"
}
```

### Bill of Materials (BOM)

```groovy
dependencies {
    // Import Spring Framework BOM
    implementation platform('org.springframework:spring-framework-bom:5.3.0')
    
    // No version needed
    implementation 'org.springframework:spring-core'
    implementation 'org.springframework:spring-web'
}
```

### Exclusions

```groovy
dependencies {
    implementation('org.springframework:spring-core:5.3.0') {
        exclude group: 'commons-logging', module: 'commons-logging'
    }
}
```

---

## 7. Tasks

### Define Tasks

```groovy
// Simple task
task hello {
    doLast {
        println 'Hello Gradle!'
    }
}

// Task with description
task myTask {
    description = 'My custom task'
    group = 'Custom'
    doLast {
        println 'Running custom task'
    }
}

// Task with inputs/outputs
task copyFiles {
    inputs.dir 'src'
    outputs.dir 'build'
    doLast {
        copy {
            from 'src'
            into 'build'
        }
    }
}

// Task dependencies
task build {
    dependsOn 'compile'
}

task compile {
    doLast {
        println 'Compiling...'
    }
}
```

### Built-in Tasks

```bash
# Common tasks
gradle build          # Build project
gradle clean          # Remove build directory
gradle test           # Run tests
gradle assemble       # Build artifacts
gradle check          # Run checks (tests, linting)

# Java plugin tasks
gradle compileJava    # Compile Java
gradle processResources
gradle jar            # Create JAR

# Application plugin tasks
gradle run            # Run application
gradle installDist    # Create distribution

# View all tasks
gradle tasks
gradle tasks --all
```

### Task Configuration

```groovy
tasks {
    // Configure existing task
    test {
        // Skip tests
        enabled = false
        
        // Parallel execution
        maxParallelForks = Runtime.runtime.availableProcessors()
        
        // Test report
        reports {
            html.enabled = true
            junitXml.enabled = true
        }
    }
    
    // Custom task
    register('myTask') {
        doLast {
            println 'Custom task'
        }
    }
}
```

---

## 8. Plugins

### Apply Plugins

```groovy
plugins {
    id 'java'                    // Java plugin
    id 'application'             // Application plugin
    id 'org.springframework.boot' version '2.7.0'  // Spring Boot
    id 'maven-publish'           // Publishing
    id 'jacoco'                  // Code coverage
}
```

### Java Plugin

```groovy
plugins {
    id 'java'
}

java {
    sourceCompatibility = JavaVersion.VERSION_11
    targetCompatibility = JavaVersion.VERSION_11
}

// Configure source/target version
tasks.withType(JavaCompile) {
    options.release = 11
}
```

### Application Plugin

```groovy
plugins {
    id 'application'
}

application {
    mainClass = 'com.example.App'
    applicationDefaultJvmArgs = ['-Xmx512m']
}

// Creates 'run' task
```

### Spring Boot Plugin

```groovy
plugins {
    id 'org.springframework.boot' version '2.7.0'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id 'java'
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

### Publishing Plugin

```groovy
plugins {
    id 'maven-publish'
}

publishing {
    repositories {
        maven {
            url = 'http://nexus:8081/repository/releases/'
            credentials {
                username = 'admin'
                password = 'password'
            }
        }
    }
    
    publications {
        mavenJava(MavenPublication) {
            from components.java
        }
    }
}

// Publish: gradle publish
```

### Docker Plugin

```groovy
plugins {
    id 'com.palantir.docker' version '0.35.0'
}

docker {
    name = 'myapp:latest'
    files jar.outputs.files
    buildArgs(['JAR_FILE': jar.archiveFileName.get()])
}

// Build image: gradle docker
```

---

## 9. Testing

### Testing Configuration

```groovy
dependencies {
    // JUnit
    testImplementation 'junit:junit:4.13.2'
    
    // Mockito
    testImplementation 'org.mockito:mockito-core:4.0.0'
    
    // AssertJ
    testImplementation 'org.assertj:assertj-core:3.21.0'
}

test {
    // Use JUnit platform
    useJUnitPlatform()
    
    // Show test output
    testLogging {
        events 'passed', 'failed', 'skipped'
    }
    
    // Parallel execution
    maxParallelForks = Runtime.runtime.availableProcessors()
}
```

### Run Tests

```bash
# Run all tests
gradle test

# Run specific test class
gradle test --tests AppTest

# Run specific test method
gradle test --tests AppTest.testAdd

# Run matching pattern
gradle test --tests *Service

# Show test output
gradle test --info
```

### Code Coverage (JaCoCo)

```groovy
plugins {
    id 'jacoco'
}

jacoco {
    toolVersion = '0.8.7'
}

jacocoTestReport {
    dependsOn test
    reports {
        xml.enabled = true
        html.enabled = true
        csv.enabled = false
    }
}

// Generate report: gradle jacocoTestReport
```

---

## 10. Deployment

### Build JAR

```groovy
jar {
    manifest {
        attributes 'Main-Class': 'com.example.App'
    }
}

// Creates executable JAR
```

### Publish to Repository

```groovy
plugins {
    id 'maven-publish'
}

publishing {
    repositories {
        maven {
            url = 'http://nexus:8081/repository/releases/'
            credentials {
                username = 'admin'
                password = 'password'
            }
        }
    }
    
    publications {
        mavenJava(MavenPublication) {
            from components.java
            
            pom {
                name = 'My App'
                description = 'My application'
                url = 'https://github.com/username/my-app'
            }
        }
    }
}

// Publish: gradle publish
```

### Build and Deploy

```bash
# Build and package
gradle clean build

# Publish to repository
gradle publish

# Create distribution
gradle installDist

# Create ZIP/TAR
gradle distZip
gradle distTar
```

---

## 11. Best Practices

### Multi-Project Build

```groovy
// settings.gradle
rootProject.name = 'my-app'

include 'common'
include 'api'
include 'web'
```

```groovy
// common/build.gradle
plugins {
    id 'java-library'
}

dependencies {
    api 'commons-io:commons-io:2.11.0'
}

// api/build.gradle
dependencies {
    implementation project(':common')
    implementation 'org.springframework:spring-core:5.3.0'
}

// web/build.gradle
dependencies {
    implementation project(':common')
    implementation project(':api')
    implementation 'org.springframework.boot:spring-boot-starter-web:2.7.0'
}
```

### Task Organization

```groovy
// Custom task groups
task build {
    group = 'Build'
    description = 'Build application'
    dependsOn 'compile', 'test'
}

task deploy {
    group = 'Deployment'
    description = 'Deploy application'
    dependsOn 'build'
    doLast {
        println 'Deploying...'
    }
}
```

### Build Optimization

```groovy
// Enable parallel builds
org.gradle.parallel=true

// Enable build cache
org.gradle.caching=true

// Daemon settings
org.gradle.daemon=true
org.gradle.daemon.idletimeout=3600000

// Configuration in gradle.properties
```

### CI/CD Integration

```yaml
# GitHub Actions
- name: Build with Gradle
  run: ./gradlew build

- name: Publish
  run: ./gradlew publish
  env:
    NEXUS_USERNAME: ${{ secrets.NEXUS_USERNAME }}
    NEXUS_PASSWORD: ${{ secrets.NEXUS_PASSWORD }}
```

---

## 12. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://gradle.org/guides/getting-started/ |
| **User Manual** | https://docs.gradle.org/current/userguide/userguide.html |
| **DSL Reference** | https://docs.gradle.org/current/dsl/index.html |
| **Plugin Registry** | https://plugins.gradle.org/ |
| **Build Cache** | https://docs.gradle.org/current/userguide/build_cache.html |
| **Testing** | https://docs.gradle.org/current/userguide/testing_guide.html |
| **Publishing** | https://docs.gradle.org/current/userguide/publishing_overview.html |

### Community and Support

| Resource | URL |
|----------|-----|
| **GitHub Repository** | https://github.com/gradle/gradle |
| **Forums** | https://discuss.gradle.org/ |
| **Slack Channel** | https://gradle-community.slack.com/ |
| **Stack Overflow** | https://stackoverflow.com/questions/tagged/gradle |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Tutorials** | https://gradle.org/guides/ |
| **YouTube Channel** | https://www.youtube.com/c/GradleBuild |
| **Blog** | https://gradle.org/blog/ |
| **Examples** | https://github.com/gradle/gradle/tree/master/subprojects/docs/src/samples |

### Popular Plugins

| Plugin | URL |
|--------|-----|
| **Spring Boot** | https://plugins.gradle.org/plugin/org.springframework.boot |
| **Docker** | https://plugins.gradle.org/plugin/com.palantir.docker |
| **Kotlin** | https://plugins.gradle.org/plugin/org.jetbrains.kotlin.jvm |
| **JaCoCo** | Built-in plugin (id 'jacoco') |

---

## Quick Reference

```bash
# Create project
gradle init

# Build project
gradle build

# Build tasks
gradle clean          # Clean build
gradle compileJava    # Compile Java
gradle test          # Run tests
gradle jar           # Create JAR
gradle assemble      # Build all artifacts

# Run application
gradle run

# Publish
gradle publish

# View tasks
gradle tasks

# View dependencies
gradle dependencies

# Check dependency tree
gradle dependencyInsight

# Force refresh
gradle build --refresh-dependencies

# Parallel build
gradle build -x test

# Skip tests
gradle build -x test

# Build with Wrapper
./gradlew build      # macOS/Linux
gradlew.bat build    # Windows
```

---

**Last Updated:** 2024
**Gradle Version:** 7.6+
**License:** Apache License 2.0

For latest information, visit [Gradle Official Site](https://gradle.org/)