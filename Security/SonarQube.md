# 🔍 SonarQube Cheatsheet

![SonarQube Logo](https://www.sonarsource.com/static/images/products/sonarqube/sonarqube-logo.svg)

---

# Table of Contents

1. [What is SonarQube?](#1-what-is-sonarqube)
2. [Why SonarQube?](#2-why-sonarqube)
3. [Static Code Analysis](#3-static-code-analysis)
4. [SonarQube Architecture](#4-sonarqube-architecture)
5. [Core Concepts](#5-core-concepts)
6. [SonarQube Components](#6-sonarqube-components)
7. [SonarQube Editions](#7-sonarqube-editions)
8. [SonarQube Terminology](#8-sonarqube-terminology)
9. [Projects](#9-projects)
10. [Quality Profiles](#10-quality-profiles)
11. [Quality Gates](#11-quality-gates)
12. [Rules](#12-rules)
13. [Issues](#13-issues)
14. [Technical Debt](#14-technical-debt)
15. [Code Smells](#15-code-smells)
16. [Bugs](#16-bugs)
17. [Vulnerabilities](#17-vulnerabilities)
18. [Security Hotspots](#18-security-hotspots)
19. [Installation](#19-installation)
20. [Initial Setup](#20-initial-setup)

---

# 1. What is SonarQube?

SonarQube is a code quality and security analysis platform used to inspect source code for bugs, vulnerabilities, code smells, security issues, and maintainability problems.

It helps teams continuously improve code quality throughout the software development lifecycle.

---

## SonarQube Provides

```text id="x7m4wp"
Code Quality Analysis
+
Security Analysis
+
Code Coverage Tracking
+
Technical Debt Measurement
+
Quality Gates
+
CI/CD Integration
```

---

## Purpose

```text id="m5v8xt"
Write Better Code
+
Detect Problems Early
+
Reduce Technical Debt
+
Improve Security
```

---

## High Level Workflow

```text id="r8v2qt"
Developer
     |
     v
Write Code
     |
     v
SonarQube Analysis
     |
     v
Quality Report
     |
     v
Fix Issues
```

---

## Common Use Cases

```text id="p4m7wr"
Code Reviews
+
CI/CD Pipelines
+
Security Validation
+
Code Quality Governance
+
Compliance
```

---

# 2. Why SonarQube?

Poor code quality leads to:

```text id="x6v3qt"
Bugs
+
Security Risks
+
Maintenance Problems
+
Production Failures
```

---

## Traditional Approach

```text id="m8v4xp"
Write Code
      |
      v
Deploy
      |
      v
Find Problems Later
```

---

## SonarQube Approach

```text id="r7m2wt"
Write Code
      |
      v
Analyze Code
      |
      v
Fix Problems Early
      |
      v
Deploy
```

---

## Benefits

```text id="n5v9wr"
Better Code Quality
+
Improved Security
+
Lower Technical Debt
+
Faster Reviews
+
Reduced Production Issues
```

---

## Why Organizations Use SonarQube

```text id="q4m8xt"
Automated Quality Checks
+
Security Validation
+
Developer Productivity
+
Compliance Support
```

---

# 3. Static Code Analysis

Static Code Analysis means examining source code without executing it.

---

## Traditional Testing

```text id="x8m2wp"
Run Application
      |
      v
Observe Behavior
```

---

## Static Analysis

```text id="r5v7qt"
Source Code
      |
      v
Analyze Code Structure
      |
      v
Detect Problems
```

---

## What Static Analysis Finds

```text id="m4v8wr"
Code Smells
+
Bugs
+
Vulnerabilities
+
Duplicated Code
+
Complex Logic
```

---

## Example

Bad Code:

```java
String password = "admin123";
```

SonarQube can identify:

```text id="p7m3xt"
Hardcoded Secret
+
Security Risk
```

---

## Benefits

```text id="n3v8wp"
Fast Feedback
+
Early Detection
+
Improved Security
```

---

# 4. SonarQube Architecture

SonarQube follows a client-server architecture.

---

## High-Level Architecture

```text id="x5m9qt"
Developer
      |
      v
Scanner
      |
      v
SonarQube Server
      |
      v
Database
```

---

## Components

```text id="r8v4wr"
Sonar Scanner
+
SonarQube Server
+
Database
+
Web UI
```

---

## Analysis Workflow

```text id="m2v7xt"
Source Code
      |
      v
Scanner
      |
      v
SonarQube Server
      |
      v
Results Dashboard
```

---

## Architecture Flow

```text id="q6m8wp"
Code
 |
 v
Scanner
 |
 v
Analysis Engine
 |
 v
Database
 |
 v
Dashboard
```

---

# 5. Core Concepts

| Concept         | Description                |
| --------------- | -------------------------- |
| Project         | Application being analyzed |
| Scanner         | Performs analysis          |
| Quality Gate    | Pass/Fail criteria         |
| Quality Profile | Collection of rules        |
| Rule            | Coding standard            |
| Issue           | Problem detected           |
| Bug             | Logic error                |
| Vulnerability   | Security weakness          |
| Code Smell      | Maintainability issue      |
| Technical Debt  | Cost of fixing issues      |

---

## Relationship

```text id="n7m4qt"
Project
   |
   +--- Quality Profile
   |
   +--- Rules
   |
   +--- Analysis
   |
   +--- Issues
```

---

# 6. SonarQube Components

Several components work together.

---

## Main Components

```text id="x4v8wr"
Scanner
+
Server
+
Database
+
Web Interface
```

---

## Scanner

Responsible for:

```text id="m8v3xt"
Reading Source Code
+
Analyzing Files
+
Sending Results
```

---

## Server

Responsible for:

```text id="q5m7wp"
Processing Results
+
Applying Rules
+
Generating Reports
```

---

## Database

Stores:

```text id="r9v2qt"
Projects
+
Issues
+
History
+
Quality Metrics
```

---

## Web Interface

Provides:

```text id="p4m8wr"
Dashboards
+
Reports
+
Administration
```

---

# 7. SonarQube Editions

SonarQube is available in multiple editions.

---

## Community Edition

```text id="n6v4xt"
Free
+
Open Source
```

---

### Features

```text id="x7m2qp"
Basic Analysis
+
Quality Gates
+
Quality Profiles
```

---

## Developer Edition

```text id="r5v8wp"
Branch Analysis
+
Pull Request Analysis
```

---

## Enterprise Edition

```text id="m4v7qt"
Advanced Security
+
Portfolio Management
+
Enterprise Features
```

---

## Data Center Edition

```text id="q8m3wr"
High Availability
+
Large Scale Deployments
```

---

## Edition Comparison

| Feature               | Community | Developer | Enterprise |
| --------------------- | --------- | --------- | ---------- |
| Basic Analysis        | Yes       | Yes       | Yes        |
| Branch Analysis       | No        | Yes       | Yes        |
| Pull Request Analysis | No        | Yes       | Yes        |
| Advanced Security     | No        | Limited   | Yes        |

---

# 8. SonarQube Terminology

Understanding common terminology is important.

---

## Key Terms

```text id="x5v8qt"
Issue
+
Bug
+
Code Smell
+
Vulnerability
+
Hotspot
```

---

## Analysis

```text id="r4m9wp"
Code Inspection Process
```

---

## Measure

```text id="m8v2xt"
Numerical Metric
```

Examples:

```text id="n7m4wr"
Coverage
+
Duplications
+
Complexity
```

---

## Leak Period

```text id="p5v8qt"
Focus On New Code
```

instead of old issues.

---

# 9. Projects

A Project is the primary unit analyzed by SonarQube.

---

## Example

```text id="x8m3wp"
E-Commerce Application
```

---

## Project Contains

```text id="r7v4xt"
Source Code
+
Quality Metrics
+
Issues
+
History
```

---

## Workflow

```text id="m4v8qt"
Project
    |
    v
Analysis
    |
    v
Report
```

---

## Benefits

```text id="q6m2wr"
Visibility
+
Quality Tracking
+
Security Tracking
```

---

# 10. Quality Profiles

A Quality Profile is a collection of coding rules.

Think of it as a rulebook.

---

## Purpose

```text id="n8v4xp"
Define Coding Standards
```

---

## Architecture

```text id="r5m7qt"
Quality Profile
       |
       +---- Rule 1
       |
       +---- Rule 2
       |
       +---- Rule 3
```

---

## Example Rules

```text id="m3v8wr"
No Hardcoded Passwords
+
Avoid Empty Catch Blocks
+
Use Secure APIs
```

---

## Benefits

```text id="x4m9qt"
Consistency
+
Code Quality
+
Security
```

---

## Profile Assignment

```text id="q7v2wp"
Project
     |
     v
Quality Profile
```

---

# 11. Quality Gates

Quality Gates determine whether code passes quality requirements.

---

## Purpose

```text id="n5m8xt"
Pass
Or
Fail
Analysis
```

---

## Example

```text id="r8v3wp"
Coverage > 80%

No Critical Vulnerabilities

No Blocker Bugs
```

---

## Workflow

```text id="m4v7qt"
Analysis
      |
      v
Quality Gate
      |
      +---- Pass
      |
      +---- Fail
```

---

## CI/CD Usage

```text id="x2m8wr"
Pipeline
      |
      v
Quality Gate Check
      |
      v
Deploy
```

---

## Benefits

```text id="q4v9xt"
Automated Quality Enforcement
+
Deployment Protection
```

---

# 12. Rules

Rules define what SonarQube checks.

---

## Examples

```text id="n7m4wp"
Avoid Duplicate Code
+
Avoid Hardcoded Credentials
+
Reduce Complexity
```

---

## Rule Categories

```text id="r6v8qt"
Bug Rules
+
Security Rules
+
Maintainability Rules
```

---

## Workflow

```text id="m5v2xt"
Rule
    |
    v
Analysis
    |
    v
Issue
```

---

## Benefits

```text id="p8m4wr"
Consistency
+
Quality Standards
```

---

# 13. Issues

Issues are problems detected during analysis.

---

## Examples

```text id="x5m7qt"
Unused Variables
+
Hardcoded Passwords
+
Duplicate Logic
```

---

## Severity Levels

```text id="n4v8wr"
Blocker
Critical
Major
Minor
Info
```

---

## Issue Workflow

```text id="r8m2xt"
Issue Found
      |
      v
Developer Fixes
      |
      v
Issue Closed
```

---

## Benefits

```text id="m7v3wp"
Actionable Feedback
+
Continuous Improvement
```

---

# 14. Technical Debt

Technical Debt represents future effort required to fix code quality issues.

---

## Example

```text id="q6m8xt"
Quick Fix Today

Or

Expensive Fix Later
```

---

## Sources

```text id="x3v7wr"
Code Smells
+
Duplications
+
Complexity
```

---

## Technical Debt Model

```text id="r4m9qt"
Issue
   |
   v
Estimated Fix Time
```

---

## Benefits

```text id="n8m4wp"
Prioritization
+
Maintainability
+
Long-Term Planning
```

---

# 15. Code Smells

Code Smells are maintainability issues.

They may not break functionality but make code harder to maintain.

---

## Examples

```text id="m5v8xt"
Long Methods
+
Duplicate Code
+
Unused Variables
+
Complex Logic
```

---

## Example

Bad:

```java
method() {
  // 500 lines
}
```

---

## Impact

```text id="q8v4wr"
Difficult Maintenance
+
Increased Complexity
```

---

## Goal

```text id="x7m2qt"
Cleaner
Readable
Maintainable
Code
```

---

# 16. Bugs

Bugs are logic or implementation errors that may cause incorrect behavior.

---

## Examples

```text id="r5v9wp"
Null Pointer Access
+
Incorrect Conditions
+
Resource Leaks
```

---

## Workflow

```text id="m4v7xt"
Bug Detected
      |
      v
Fix
      |
      v
Retest
```

---

## Impact

```text id="p8m3wr"
Application Failure
+
Unexpected Behavior
```

---

# 17. Vulnerabilities

Vulnerabilities are security weaknesses.

---

## Examples

```text id="x6m8qt"
SQL Injection
+
Cross Site Scripting
+
Hardcoded Secrets
+
Weak Encryption
```

---

## Security Workflow

```text id="r7v2wp"
Vulnerability
      |
      v
Risk Assessment
      |
      v
Fix
```

---

## Benefits

```text id="m3v9xt"
Improved Security
+
Reduced Risk
```

---

# 18. Security Hotspots

Security Hotspots are areas requiring manual review.

They are not necessarily vulnerabilities.

---

## Example

```java
Cipher.getInstance("AES");
```

May require review.

---

## Workflow

```text id="q5m8wr"
Hotspot
     |
     v
Security Review
     |
     +---- Safe
     |
     +---- Needs Fix
```

---

## Purpose

```text id="n4v7xt"
Developer Awareness
+
Security Validation
```

---

# 19. Installation

## Docker Installation

```bash id="x8m2wp"
docker run -d \
  --name sonarqube \
  -p 9000:9000 \
  sonarqube:lts-community
```

---

## Verify Access

```text id="r5m7qt"
http://localhost:9000
```

---

## Kubernetes Installation

```bash id="m7v4wr"
helm repo add sonarqube https://SonarSource.github.io/helm-chart-sonarqube

helm repo update

helm install sonarqube sonarqube/sonarqube
```

---

## Deployment Options

```text id="q4v8xt"
Docker
+
Kubernetes
+
Virtual Machine
```

---

# 20. Initial Setup

---

## Default Credentials

```text id="n7m3wp"
Username: admin

Password: admin
```

---

## First Login

```text id="r8v2qt"
Login
    |
    v
Change Password
```

---

## Create Project

```text id="m5v9xt"
Projects
    |
    v
Create Project
```

---

## Generate Token

```text id="x4m8wr"
My Account
      |
      v
Security
      |
      v
Generate Token
```

---

## Setup Workflow

```text id="q6v3wp"
Install
   |
   v
Login
   |
   v
Create Project
   |
   v
Generate Token
   |
   v
Run Analysis
```

---

# 21. Sonar Scanner

Sonar Scanner is the component responsible for analyzing source code and sending results to the SonarQube server.

Without a scanner, SonarQube cannot analyze code.

---

## Purpose

```text id="m8v4wp"
Read Source Code
+
Analyze Files
+
Collect Metrics
+
Send Results
```

---

## Architecture

```text id="x5m7qt"
Source Code
      |
      v
Sonar Scanner
      |
      v
SonarQube Server
      |
      v
Dashboard
```

---

## Supported Scanners

```text id="r4v8xn"
SonarScanner CLI
+
Maven Scanner
+
Gradle Scanner
+
.NET Scanner
+
NPM Scanner
```

---

## Workflow

```text id="n7m3wr"
Source Code
      |
      v
Scanner
      |
      v
Analysis Report
      |
      v
SonarQube
```

---

## Benefits

```text id="q8v2xt"
Automation
+
CI/CD Integration
+
Consistent Analysis
```

---

# 22. Analysis Workflow

Every SonarQube scan follows a defined workflow.

---

## Complete Flow

```text id="m4v9qp"
Developer
      |
      v
Commit Code
      |
      v
CI/CD Pipeline
      |
      v
Sonar Scanner
      |
      v
SonarQube Server
      |
      v
Quality Gate
      |
      v
Result
```

---

## Analysis Steps

```text id="x6m8wr"
Read Source Code
+
Apply Rules
+
Calculate Metrics
+
Detect Issues
+
Evaluate Quality Gate
```

---

## Final Output

```text id="r9v4xt"
Bugs
+
Code Smells
+
Vulnerabilities
+
Coverage
+
Technical Debt
```

---

# 23. Project Analysis

Project Analysis is the process of evaluating an application's code quality.

---

## Example Project

```text id="n3m7qt"
E-Commerce Application
```

---

## What Gets Analyzed?

```text id="p5v8wr"
Source Code
+
Tests
+
Coverage Reports
+
Dependencies
```

---

## Analysis Result

```text id="m7x4qt"
Code Quality Score
+
Security Findings
+
Coverage Metrics
+
Technical Debt
```

---

## Benefits

```text id="q4v9wp"
Continuous Improvement
+
Security Visibility
+
Code Governance
```

---

# 24. SonarScanner CLI

The most commonly used scanner.

Works independently of build tools.

---

## Installation

Download from:

```text id="r8m3xt"
SonarSource Website
```

---

## Verify Installation

```bash id="x2m8qt"
sonar-scanner --version
```

---

## Basic Command

```bash id="p9v4wr"
sonar-scanner
```

---

## Example

```bash id="m4v8xt"
sonar-scanner \
-Dsonar.projectKey=myapp \
-Dsonar.sources=. \
-Dsonar.host.url=http://localhost:9000 \
-Dsonar.token=TOKEN
```

---

## Workflow

```text id="q7m3wp"
Source Code
      |
      v
SonarScanner CLI
      |
      v
SonarQube
```

---

## Use Cases

```text id="r5v8qt"
Custom Projects
+
Shell Pipelines
+
CI/CD Systems
```

---

# 25. Maven Integration

SonarQube integrates directly with Maven.

---

## Architecture

```text id="n8m2wr"
Maven Project
      |
      v
Maven Sonar Plugin
      |
      v
SonarQube
```

---

## Scan Command

```bash id="x4v7xt"
mvn sonar:sonar
```

---

## Example

```bash id="p6m9wp"
mvn clean verify sonar:sonar
```

---

## Workflow

```text id="m3v8qt"
Compile
      |
      v
Test
      |
      v
Analyze
```

---

## Benefits

```text id="q9v4wr"
Simple Integration
+
Java Support
+
Build Automation
```

---

# 26. Gradle Integration

Used for Gradle-based projects.

---

## Architecture

```text id="r7m2xt"
Gradle Project
      |
      v
Sonar Plugin
      |
      v
SonarQube
```

---

## Plugin Example

```gradle id="x5m8wr"
plugins {
    id "org.sonarqube"
}
```

---

## Analysis Command

```bash id="n4v9qt"
./gradlew sonarqube
```

---

## Benefits

```text id="p8m4wp"
Native Gradle Support
+
Automation
```

---

# 27. NPM / NodeJS Integration

SonarQube supports JavaScript and TypeScript applications.

---

## Workflow

```text id="m7v3xt"
NodeJS Application
        |
        v
Sonar Scanner
        |
        v
SonarQube
```

---

## Example

```bash id="q4m8wr"
npm install

sonar-scanner
```

---

## Common Languages

```text id="r8v2qt"
JavaScript
+
TypeScript
+
NodeJS
```

---

## Benefits

```text id="x6m4wp"
Frontend Analysis
+
Backend Analysis
+
Security Validation
```

---

# 28. .NET Integration

SonarQube supports .NET Framework and .NET Core applications.

---

## Workflow

```text id="n5v9xt"
.NET Project
      |
      v
SonarScanner for .NET
      |
      v
SonarQube
```

---

## Begin Analysis

```bash id="p7m3wr"
dotnet sonarscanner begin
```

---

## Build Project

```bash id="m4v8qt"
dotnet build
```

---

## End Analysis

```bash id="q5v7xt"
dotnet sonarscanner end
```

---

## Benefits

```text id="r4m9wp"
Native .NET Support
+
CI/CD Integration
```

---

# 29. GitLab Integration

One of the most common DevOps integrations.

---

## Architecture

```text id="n7m4wr"
GitLab
   |
   v
Pipeline
   |
   v
Sonar Scanner
   |
   v
SonarQube
```

---

## Workflow

```text id="x4m8wr"
Push Code
      |
      v
GitLab Pipeline
      |
      v
Sonar Analysis
      |
      v
Quality Gate
```

---

## Example

```yaml id="q6v3wp"
sonarqube-check:
  script:
    - sonar-scanner
```

---

## Benefits

```text id="r5v8qt"
Automated Analysis
+
Pull Request Checks
+
Quality Enforcement
```

---

# 30. Jenkins Integration

A classic SonarQube integration.

---

## Architecture

```text id="n8m4xp"
Jenkins
    |
    v
Sonar Scanner
    |
    v
SonarQube
```

---

## Pipeline Flow

```text id="m3v7wr"
Checkout
    |
    v
Build
    |
    v
Sonar Analysis
    |
    v
Quality Gate
```

---

## Jenkins Pipeline Example

```groovy id="x7m2qt"
stage('SonarQube') {
    sh 'sonar-scanner'
}
```

---

## Benefits

```text id="q4v8wr"
CI/CD Automation
+
Quality Validation
```

---

# 31. GitHub Actions Integration

GitHub Actions can run SonarQube scans.

---

## Architecture

```text id="r8m3wp"
GitHub
    |
    v
GitHub Actions
    |
    v
SonarQube
```

---

## Workflow

```text id="m6v4xt"
Push Code
      |
      v
GitHub Workflow
      |
      v
Sonar Analysis
```

---

## Benefits

```text id="p9m2wr"
Automation
+
Pull Request Analysis
```

---

# 32. Azure DevOps Integration

SonarQube integrates with Azure DevOps pipelines.

---

## Architecture

```text id="x5v8qt"
Azure DevOps
      |
      v
Build Pipeline
      |
      v
SonarQube
```

---

## Workflow

```text id="r7m4wp"
Build
  |
  v
Analyze
  |
  v
Quality Gate
```

---

## Benefits

```text id="n4v8wr"
Enterprise CI/CD
+
Quality Governance
```

---

# 33. Pull Request Analysis

One of SonarQube's most powerful features.

Analyzes only new changes in a Pull Request (PR) or Merge Request (MR).

---

## Traditional Analysis

```text id="m8v3xt"
Entire Codebase
```

---

## PR Analysis

```text id="q5m8wr"
Only New Changes
```

---

## Workflow

```text id="r9v2qt"
Developer Creates PR
         |
         v
Sonar Analysis
         |
         v
Comments Added
         |
         v
Review
```

---

## Benefits

```text id="p4m8wr"
Fast Feedback
+
Reduced Noise
+
Better Reviews
```

---

# 34. Branch Analysis

Analyzes individual branches separately.

---

## Example

```text id="x7m4qt"
main

feature-login

feature-payment
```

---

## Workflow

```text id="n5v8xp"
Branch
   |
   v
Analysis
   |
   v
Branch Report
```

---

## Benefits

```text id="r3m9wp"
Independent Analysis
+
Feature Validation
```

---

# 35. New Code Analysis

Focuses on recently added or modified code.

---

## Why?

Most teams cannot fix years of existing issues immediately.

---

## Strategy

```text id="m4v8qt"
Old Code
      |
      v
Ignore Temporarily

-----------------

New Code
      |
      v
Must Meet Standards
```

---

## Benefits

```text id="q8m2wr"
Gradual Improvement
+
Practical Adoption
+
Developer Accountability
```

---

# 36. Reports

SonarQube generates detailed reports after analysis.

---

## Report Contents

```text id="x6v4wp"
Bugs
+
Code Smells
+
Vulnerabilities
+
Coverage
+
Technical Debt
```

---

## Example Metrics

```text id="r5m9xt"
Coverage = 85%

Code Smells = 12

Vulnerabilities = 0

Bugs = 1
```

---

## Benefits

```text id="n7v3wr"
Visibility
+
Trend Analysis
+
Quality Tracking
```

---

# 37. Dashboards

Dashboards provide a centralized view of project quality.

---

## Dashboard Displays

```text id="m8v4qt"
Quality Gate Status
+
Coverage
+
Technical Debt
+
Security Rating
+
Reliability Rating
```

---

## Example

```text id="q4m8wr"
Project

Quality Gate: Passed

Coverage: 87%

Bugs: 0

Vulnerabilities: 0
```

---

## Benefits

```text id="r8v2qt"
Executive Visibility
+
Developer Visibility
+
Continuous Monitoring
```

---

# 38. Quality Gates

Quality Gates are one of the most important features in SonarQube.

They determine whether code meets predefined quality standards before moving forward in the development lifecycle.

Think of a Quality Gate as an automated approval checkpoint.

---

## Purpose

```text id="x7m4wp"
Pass
Or
Fail
Code Quality
```

---

## Why Quality Gates?

Without Quality Gates:

```text id="m5v8xt"
Developer
      |
      v
Commit Code
      |
      v
Deploy
```

Problems may reach production.

---

With Quality Gates:

```text id="r8v2qt"
Developer
      |
      v
Commit Code
      |
      v
Analysis
      |
      v
Quality Gate
      |
      +---- Pass
      |
      +---- Fail
```

---

## Common Conditions

```text id="p4m7wr"
No Critical Bugs
+
No Critical Vulnerabilities
+
Coverage > 80%
+
Duplications < 3%
```

---

## Example Gate

```text id="x6v3qt"
Coverage >= 80%

AND

Critical Vulnerabilities = 0

AND

Blocker Bugs = 0
```

---

## CI/CD Integration

```text id="m8v4xp"
Build
   |
   v
Sonar Analysis
   |
   v
Quality Gate
   |
   v
Deploy
```

---

## Benefits

```text id="r7m2wt"
Automated Governance
+
Better Quality
+
Safer Releases
```

---

# 39. Quality Profiles

Quality Profiles define which rules are used during analysis.

Think of them as rule collections.

---

## Architecture

```text id="n5v9wr"
Quality Profile
       |
       +---- Rule 1
       |
       +---- Rule 2
       |
       +---- Rule 3
```

---

## Purpose

```text id="q4m8xt"
Define Coding Standards
```

for specific programming languages.

---

## Example

Java Profile:

```text id="x8m2wp"
Avoid Empty Catch Blocks
+
Avoid Hardcoded Passwords
+
Avoid Dead Code
```

---

## Profile Assignment

```text id="r5v7qt"
Project
     |
     v
Quality Profile
```

---

## Default Profiles

```text id="m4v8wr"
Java
+
JavaScript
+
Python
+
C#
+
Go
```

---

## Benefits

```text id="p7m3xt"
Consistency
+
Standardization
+
Quality Enforcement
```

---

# 40. Code Coverage

Code Coverage measures how much source code is executed by tests.

---

## Purpose

```text id="n3v8wp"
Measure Test Effectiveness
```

---

## Example

```text id="x5m9qt"
Application

100 Lines Of Code

80 Lines Tested
```

Coverage:

```text id="r8v4wr"
80%
```

---

## Formula

```text id="m2v7xt"
Covered Lines
-----------------
Total Lines

× 100
```

---

## Coverage Levels

```text id="q6m8wp"
90%+  Excellent

80%+  Good

70%+  Acceptable

<70%  Needs Improvement
```

---

## Benefits

```text id="n7m4qt"
Better Testing
+
Reduced Defects
+
Improved Reliability
```

---

# 41. Unit Test Coverage

Unit Test Coverage specifically measures how much code is tested through unit tests.

---

## Example

```text id="x4v8wr"
Login Service

10 Functions

9 Tested
```

Coverage:

```text id="m8v3xt"
90%
```

---

## Workflow

```text id="q5m7wp"
Write Unit Tests
       |
       v
Run Tests
       |
       v
Generate Coverage Report
       |
       v
SonarQube
```

---

## Popular Coverage Tools

```text id="r9v2qt"
JaCoCo
+
Cobertura
+
Istanbul
+
Coverage.py
```

---

## Benefits

```text id="p4m8wr"
Confidence
+
Reliability
+
Quality Assurance
```

---

# 42. Test Reports

SonarQube can import test execution reports.

---

## Sources

```text id="x7m4qt"
JUnit
+
TestNG
+
NUnit
+
PyTest
```

---

## Workflow

```text id="n5v8xp"
Tests
   |
   v
Report
   |
   v
SonarQube
```

---

## Example Metrics

```text id="r3m9wp"
Passed Tests
+
Failed Tests
+
Skipped Tests
```

---

## Benefits

```text id="m4v8qt"
Visibility
+
Trend Analysis
+
Quality Tracking
```

---

# 43. Code Smells

Code Smells are maintainability problems.

They usually don't break functionality but indicate poor design.

---

## Examples

```text id="q8m2wr"
Long Methods
+
Duplicate Code
+
Dead Code
+
Complex Conditions
```

---

## Example

Bad:

```java id="u7e2mn"
if(a){
 if(b){
  if(c){
   if(d){
   }
  }
 }
}
```

---

## Impact

```text id="x6v4wp"
Difficult Maintenance
+
Higher Complexity
+
Future Bugs
```

---

## Goal

```text id="r5m9xt"
Readable Code
+
Maintainable Code
```

---

# 44. Bugs

Bugs indicate logic or implementation problems.

---

## Examples

```text id="n7v3wr"
Null Pointer Access
+
Memory Leak
+
Incorrect Conditions
+
Resource Leak
```

---

## Example

```java id="x4k8tw"
String s = null;

System.out.println(s.length());
```

---

## Impact

```text id="m8v4qt"
Application Failure
+
Unexpected Behavior
```

---

## Reliability Rating

Bugs directly affect:

```text id="q4m8wr"
Reliability
```

---

# 45. Vulnerabilities

Vulnerabilities are security weaknesses that attackers may exploit.

---

## Examples

```text id="r8v2qt"
SQL Injection
+
Cross Site Scripting
+
Hardcoded Credentials
+
Weak Cryptography
```

---

## Security Workflow

```text id="x6m4wp"
Vulnerability
       |
       v
Assessment
       |
       v
Fix
```

---

## Impact

```text id="n5v9xt"
Data Breach
+
Unauthorized Access
+
Compliance Violations
```

---

## Security Rating

Vulnerabilities affect:

```text id="p7m3wr"
Security Rating
```

---

# 46. Security Hotspots

Security Hotspots require human review.

Unlike vulnerabilities, they are not automatically considered security flaws.

---

## Example

```java id="v6p2wd"
Cipher.getInstance("AES");
```

May be safe or unsafe depending on implementation.

---

## Workflow

```text id="m4v8qt"
Hotspot
     |
     v
Review
     |
     +---- Safe
     |
     +---- Needs Fix
```

---

## Purpose

```text id="q5v7xt"
Developer Awareness
+
Security Validation
```

---

## Benefits

```text id="r4m9wp"
Better Security Reviews
+
Reduced Risk
```

---

# 47. OWASP Support

SonarQube helps identify issues related to OWASP security risks.

---

## Common OWASP Categories

```text id="n7m4wr"
Injection
+
Broken Authentication
+
Sensitive Data Exposure
+
Security Misconfiguration
+
Vulnerable Components
```

---

## Example

```text id="x4m8wr"
SQL Injection
```

Maps to:

```text id="q6v3wp"
OWASP Top 10
```

---

## Benefits

```text id="r5v8qt"
Security Compliance
+
Risk Reduction
+
Developer Awareness
```

---

# 48. Secure Coding

SonarQube promotes secure coding practices.

---

## Secure Coding Goals

```text id="n8m4xp"
Prevent Vulnerabilities
+
Protect Data
+
Reduce Attack Surface
```

---

## Recommended Practices

```text id="m3v7wr"
Input Validation
+
Output Encoding
+
Strong Authentication
+
Secure Encryption
```

---

## Secure Development Flow

```text id="x7m2qt"
Write Code
     |
     v
Analyze
     |
     v
Fix Security Issues
```

---

## Benefits

```text id="q4v8wr"
Safer Applications
+
Improved Security Posture
```

---

# 49. Technical Debt

Technical Debt estimates future effort needed to resolve quality issues.

---

## Concept

```text id="r8m3wp"
Quick Shortcut Today

Becomes

Extra Work Tomorrow
```

---

## Sources

```text id="m6v4xt"
Code Smells
+
Duplications
+
Complexity
+
Poor Design
```

---

## Example

```text id="x5v8qt"
20 Issues

Estimated Fix Time

4 Hours
```

---

## Debt Calculation

```text id="r7m4wp"
Issue
    |
    v
Estimated Remediation Time
```

---

## Benefits

```text id="n4v8wr"
Prioritization
+
Planning
+
Maintainability
```

---

# 50. Maintainability Rating

Measures how maintainable code is.

---

## Based On

```text id="m8v3xt"
Technical Debt
+
Code Smells
+
Complexity
```

---

## Ratings

```text id="q5m8wr"
A = Excellent

B = Good

C = Moderate

D = Poor

E = Very Poor
```

---

## Goal

```text id="r9v2qt"
Maintainability Rating A
```

---

## Benefits

```text id="p4m8wr"
Cleaner Code
+
Lower Maintenance Cost
```

---

# 51. Reliability Rating

Measures the likelihood of failures caused by bugs.

---

## Based On

```text id="x7m4qt"
Bugs
+
Defects
+
Logic Errors
```

---

## Ratings

```text id="n5v8xp"
A = Excellent

B = Good

C = Moderate

D = Poor

E = Critical
```

---

## Goal

```text id="r3m9wp"
Reliability Rating A
```

---

## Benefits

```text id="m4v8qt"
Stable Applications
+
Fewer Production Issues
```

---

# 52. Security Rating

Measures overall security posture.

---

## Based On

```text id="q8m2wr"
Vulnerabilities
+
Security Findings
```

---

## Ratings

```text id="x6v4wp"
A = Excellent

B = Good

C = Moderate

D = Poor

E = Critical
```

---

## Goal

```text id="r5m9xt"
Security Rating A
```

---

## Benefits

```text id="n7v3wr"
Reduced Risk
+
Improved Compliance
+
Better Security
```

---

# 53. Monitoring

Monitoring helps ensure SonarQube remains healthy, responsive, and available.

In production environments, SonarQube is a critical component of the CI/CD pipeline.

---

## What Should Be Monitored?

```text id="x7m4wp"
Server Health
+
Database Health
+
Analysis Performance
+
Queue Processing
+
Disk Usage
```

---

## Monitoring Architecture

```text id="m5v8xt"
SonarQube
     |
     v
Metrics
     |
     v
Prometheus
     |
     v
Grafana
```

---

## Key Metrics

```text id="r8v2qt"
CPU Usage
+
Memory Usage
+
Database Connections
+
Response Time
+
Analysis Duration
```

---

## Application Metrics

```text id="p4m7wr"
Project Count
+
Analysis Count
+
Quality Gate Status
```

---

## Benefits

```text id="x6v3qt"
Availability
+
Performance Visibility
+
Capacity Planning
```

---

# 54. Performance Optimization

Large projects and enterprise deployments require optimization.

---

## Common Bottlenecks

```text id="m8v4xp"
Large Codebases
+
Slow Database
+
Insufficient Memory
+
Heavy Concurrent Analysis
```

---

## Performance Flow

```text id="r7m2wt"
Source Code
      |
      v
Scanner
      |
      v
Server
      |
      v
Database
```

---

## Database Optimization

Use:

```text id="n5v9wr"
PostgreSQL
```

Recommended for production deployments.

---

## JVM Optimization

Adjust memory settings.

Example:

```bash id="q4m8xt"
SONAR_WEB_JAVAOPTS="-Xms1G -Xmx2G"
```

---

## Reduce Analysis Time

```text id="x8m2wp"
Analyze Only Needed Files
+
Use Incremental Analysis
+
Optimize CI/CD
```

---

## Benefits

```text id="r5v7qt"
Faster Scans
+
Better User Experience
+
Improved Scalability
```

---

# 55. Best Practices

Following best practices ensures successful SonarQube adoption.

---

## Focus On New Code

Instead of fixing everything immediately:

```text id="m4v8wr"
Old Code
      |
      v
Improve Gradually

-----------------

New Code
      |
      v
Must Meet Standards
```

---

## Enforce Quality Gates

```text id="p7m3xt"
No Critical Bugs
+
No Critical Vulnerabilities
+
Coverage Requirements
```

---

## Integrate With CI/CD

```text id="n3v8wp"
GitLab
+
Jenkins
+
GitHub Actions
+
Azure DevOps
```

---

## Review Security Findings

Pay attention to:

```text id="x5m9qt"
Vulnerabilities
+
Security Hotspots
```

---

## Maintain Quality Profiles

Regularly review:

```text id="r8v4wr"
Rules
+
Coding Standards
+
Security Policies
```

---

## Best Practice Summary

```text id="m2v7xt"
Analyze Continuously
+
Fix Early
+
Automate Enforcement
```

---

# 56. Common Mistakes

Many teams misuse SonarQube during implementation.

---

## Mistake 1

Ignoring Quality Gates.

```text id="q6m8wp"
Quality Gate Failed

But Deployment Continues
```

---

## Mistake 2

Focusing Only On Coverage.

Coverage alone does not guarantee quality.

---

## Mistake 3

Ignoring Security Findings.

```text id="n7m4qt"
Vulnerabilities
+
Hotspots
```

must be reviewed.

---

## Mistake 4

Disabling Too Many Rules.

Result:

```text id="x4v8wr"
Reduced Analysis Effectiveness
```

---

## Mistake 5

Treating SonarQube As A One-Time Activity.

Correct approach:

```text id="m8v3xt"
Continuous Analysis
```

---

# 57. Troubleshooting

A systematic troubleshooting process helps identify issues quickly.

---

## Troubleshooting Flow

```text id="q5m7wp"
Scanner
    |
    v
Network
    |
    v
Server
    |
    v
Database
```

---

## Scanner Cannot Connect

Error:

```text id="r9v2qt"
Connection Refused
```

Check:

```text id="p4m8wr"
Server Status
+
Firewall
+
URL Configuration
```

---

## Analysis Fails

Possible Causes:

```text id="x7m4qt"
Invalid Token
+
Incorrect Project Key
+
Configuration Error
```

---

## Quality Gate Not Updating

Verify:

```text id="n5v8xp"
Analysis Completed
+
Background Tasks Healthy
```

---

## Slow Analysis

Check:

```text id="r3m9wp"
Large Files
+
Server Resources
+
Database Performance
```

---

## Database Problems

Symptoms:

```text id="m4v8qt"
Slow Dashboard
+
Failed Analysis
+
Timeouts
```

---

## Useful Logs

```text id="q8m2wr"
sonar.log

web.log

ce.log

es.log
```

---

## Log Location

```text id="x6v4wp"
SONARQUBE_HOME/logs
```

---

## Common Problems

| Problem               | Cause                   |
| --------------------- | ----------------------- |
| Scanner Failed        | Configuration Issue     |
| Authentication Failed | Invalid Token           |
| Slow Analysis         | Resource Constraints    |
| Failed Startup        | Database Problem        |
| Quality Gate Stuck    | Background Task Failure |

---

# 58. Common Interview Questions

## What is SonarQube?

```text id="r5m9xt"
Code Quality
And
Security Analysis Platform
```

---

## What is Static Code Analysis?

```text id="n7v3wr"
Analyzing Source Code

Without Executing It
```

---

## What is a Quality Gate?

```text id="x4m8wr"
Pass/Fail Criteria

For Code Quality
```

---

## What is a Quality Profile?

```text id="m8v4qt"
Collection Of Rules

Applied During Analysis
```

---

## Difference Between Bug and Code Smell?

```text id="q4m8wr"
Bug
=
Incorrect Behavior

Code Smell
=
Maintainability Problem
```

---

## What is Technical Debt?

```text id="r8v2qt"
Estimated Effort

Required To Fix Issues
```

---

## What is a Security Hotspot?

```text id="x6m4wp"
Code Requiring

Manual Security Review
```

---

## What is Sonar Scanner?

```text id="n5v9xt"
Tool That Analyzes Code

And Sends Results To SonarQube
```

---

## What is New Code Analysis?

```text id="p7m3wr"
Focuses On

Recently Added Or Modified Code
```

---

## Why Integrate SonarQube With CI/CD?

```text id="m4v8qt"
Automated Quality Checks

Before Deployment
```

---

# 59. Additional Resources

## Official Documentation

| Resource                    | URL                                                                                                                                            |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| SonarQube Documentation     | [https://docs.sonarsource.com/sonarqube](https://docs.sonarsource.com/sonarqube)                                                               |
| Sonar Scanner Documentation | [https://docs.sonarsource.com/sonarqube/analyzing-source-code/scanners](https://docs.sonarsource.com/sonarqube/analyzing-source-code/scanners) |
| SonarSource                 | [https://www.sonarsource.com](https://www.sonarsource.com)                                                                                     |
| OWASP                       | [https://owasp.org](https://owasp.org)                                                                                                         |
| Jenkins                     | [https://www.jenkins.io](https://www.jenkins.io)                                                                                               |
| GitLab                      | [https://docs.gitlab.com](https://docs.gitlab.com)                                                                                             |

---

# 📎 Official Documentation

* [https://docs.sonarsource.com/sonarqube](https://docs.sonarsource.com/sonarqube)
* [https://docs.sonarsource.com/sonarqube/analyzing-source-code/scanners](https://docs.sonarsource.com/sonarqube/analyzing-source-code/scanners)
* [https://www.sonarsource.com](https://www.sonarsource.com)
* [https://owasp.org](https://owasp.org)
* [https://www.jenkins.io](https://www.jenkins.io)
* [https://docs.gitlab.com](https://docs.gitlab.com)

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, software engineering, DevOps engineering, DevSecOps, platform engineering, CI/CD automation, and secure software development.

Primary references include:

* SonarQube Documentation
* Sonar Scanner Documentation
* SonarSource Documentation
* OWASP Documentation

SonarQube is a code quality and security platform developed by [SonarSource](https://www.sonarsource.com/?utm_source=chatgpt.com).

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

```text id="x8m4qt"
Static Code Analysis
+
Code Quality
+
Security Analysis
+
Technical Debt Tracking
+
Quality Gates
+
CI/CD Integration
```

SonarQube enables organizations to continuously improve software quality and security by identifying bugs, vulnerabilities, code smells, and maintainability issues early in the development lifecycle.

---

```md id="n7v3wr"
**Last Updated:** 2026
**Platform:** SonarQube
**Version:** SonarQube 10.x+
**License:** Community Edition (LGPL) / Commercial Editions

For latest information, visit https://docs.sonarsource.com/sonarqube
```