# 🛡️ OWASP Cheatsheet

![OWASP Logo](https://owasp.org/assets/images/logo.png)

---

# Table of Contents

1. [What is OWASP?](#1-what-is-owasp)
2. [Why OWASP?](#2-why-owasp)
3. [Application Security Basics](#3-application-security-basics)
4. [Security Terminology](#4-security-terminology)
5. [OWASP Projects Overview](#5-owasp-projects-overview)
6. [OWASP Top 10 Introduction](#6-owasp-top-10-introduction)
7. [CIA Triad](#7-cia-triad)
8. [Threat Modeling Basics](#8-threat-modeling-basics)
9. [Risk Assessment](#9-risk-assessment)
10. [Secure SDLC](#10-secure-sdlc)
11. [Shift Left Security](#11-shift-left-security)
12. [Defense In Depth](#12-defense-in-depth)
13. [Security Testing Types](#13-security-testing-types)
14. [Authentication](#14-authentication)
15. [Authorization](#15-authorization)
16. [Session Management](#16-session-management)
17. [Cryptography Basics](#17-cryptography-basics)
18. [Secure Coding Principles](#18-secure-coding-principles)

---

# 1. What is OWASP?

OWASP stands for **Open Worldwide Application Security Project**.

It is a non-profit organization focused on improving software security through education, standards, tools, documentation, and community-driven projects.

---

## OWASP Mission

```text id="x7m4wp"
Make Software Security
Visible
+
Understandable
+
Achievable
```

---

## What OWASP Provides

```text id="m5v8xt"
Security Standards
+
Best Practices
+
Security Tools
+
Cheat Sheets
+
Top 10 Risks
+
Training Resources
```

---

## Why OWASP Matters

Most cyberattacks target applications.

OWASP helps developers and organizations identify and mitigate security risks before software reaches production.

---

## Common Users

```text id="r8v2qt"
Developers
+
DevOps Engineers
+
DevSecOps Engineers
+
Security Engineers
+
Architects
+
Auditors
```

---

# 2. Why OWASP?

Modern applications face constant security threats.

---

## Common Risks

```text id="p4m7wr"
SQL Injection
+
Broken Authentication
+
Access Control Issues
+
Sensitive Data Exposure
+
Misconfigurations
```

---

## Without Security

```text id="x6v3qt"
Application
      |
      v
Vulnerability
      |
      v
Attack
      |
      v
Data Breach
```

---

## With OWASP Practices

```text id="m8v4xp"
Application
      |
      v
Security Controls
      |
      v
Reduced Risk
```

---

## Benefits

```text id="r7m2wt"
Improved Security
+
Compliance Support
+
Reduced Risk
+
Developer Awareness
+
Safer Applications
```

---

# 3. Application Security Basics

Application Security (AppSec) protects software from security threats throughout its lifecycle.

---

## Security Goals

```text id="n5v9wr"
Prevent Attacks
+
Protect Data
+
Maintain Trust
+
Ensure Compliance
```

---

## Security Lifecycle

```text id="q4m8xt"
Design
   |
   v
Develop
   |
   v
Test
   |
   v
Deploy
   |
   v
Monitor
```

---

## Security Areas

```text id="x8m2wp"
Authentication
+
Authorization
+
Encryption
+
Input Validation
+
Logging
```

---

## Goal

```text id="r5v7qt"
Build Secure Software
By Design
```

---

# 4. Security Terminology

Understanding security terminology is essential.

---

## Asset

Anything valuable.

Examples:

```text id="m4v8wr"
Database
+
Application
+
Customer Data
```

---

## Threat

Potential danger.

```text id="p7m3xt"
Malware
+
Attacker
+
Insider Threat
```

---

## Vulnerability

Weakness that can be exploited.

```text id="n3v8wp"
SQL Injection
+
Weak Password Policy
```

---

## Risk

Probability and impact of exploitation.

---

## Exploit

Method used to abuse a vulnerability.

---

## Security Relationship

```text id="x5m9qt"
Threat
   |
   v
Vulnerability
   |
   v
Exploit
   |
   v
Impact
```

---

# 5. OWASP Projects Overview

OWASP maintains many security projects.

---

## Popular Projects

```text id="r8v4wr"
OWASP Top 10
+
OWASP API Top 10
+
ASVS
+
ZAP
+
Cheat Sheets
```

---

## OWASP Top 10

Most common web application risks.

---

## OWASP ZAP

Free penetration testing tool.

---

## OWASP ASVS

Application Security Verification Standard.

---

## Cheat Sheets

Practical security implementation guides.

---

## Why These Matter

```text id="m2v7xt"
Security Standards
+
Developer Guidance
+
Industry Adoption
```

---

# 6. OWASP Top 10 Introduction

OWASP Top 10 is the most widely referenced application security awareness document.

---

## Purpose

Identify the most critical web application security risks.

---

## Why Important?

```text id="q6m8wp"
Industry Standard
+
Interview Favorite
+
Compliance Reference
```

---

## Current Categories

```text id="n7m4qt"
Broken Access Control
+
Cryptographic Failures
+
Injection
+
Insecure Design
+
Security Misconfiguration
+
Vulnerable Components
+
Authentication Failures
+
Software Integrity Failures
+
Logging Failures
+
SSRF
```

---

## Security Goal

```text id="x4v8wr"
Identify
+
Prevent
+
Mitigate
```

---

# 7. CIA Triad

The CIA Triad is the foundation of information security.

---

## CIA

```text id="m8v3xt"
Confidentiality
+
Integrity
+
Availability
```

---

## Confidentiality

Protect data from unauthorized access.

Examples:

```text id="q5m7wp"
Encryption
+
Access Control
```

---

## Integrity

Ensure data remains accurate and unchanged.

Examples:

```text id="r9v2qt"
Checksums
+
Digital Signatures
```

---

## Availability

Ensure systems remain accessible.

Examples:

```text id="p4m8wr"
Load Balancing
+
Redundancy
+
Backups
```

---

## CIA Model

```text id="x7m4qt"
Confidentiality
       +
Integrity
       +
Availability
       =
Security
```

---

# 8. Threat Modeling Basics

Threat Modeling identifies potential security threats before development is completed.

---

## Purpose

```text id="n5v8xp"
Identify Risks Early
```

---

## Process

```text id="r3m9wp"
System Design
      |
      v
Identify Assets
      |
      v
Identify Threats
      |
      v
Apply Controls
```

---

## Common Questions

```text id="m4v8qt"
What Can Go Wrong?
+
How Can It Be Exploited?
+
How Can We Prevent It?
```

---

## Benefits

```text id="q8m2wr"
Proactive Security
+
Lower Costs
+
Reduced Risk
```

---

# 9. Risk Assessment

Risk Assessment evaluates security threats and their impact.

---

## Formula

```text id="x6v4wp"
Risk

=

Likelihood

×

Impact
```

---

## Example

```text id="r5m9xt"
Weak Password Policy
      |
      v
High Likelihood
+
High Impact
```

---

## Risk Levels

```text id="n7v3wr"
Low
Medium
High
Critical
```

---

## Goal

```text id="x4m8wr"
Prioritize Security Efforts
```

---

# 10. Secure SDLC

Secure Software Development Life Cycle integrates security into every development phase.

---

## Traditional SDLC

```text id="m8v4qt"
Design
+
Develop
+
Test
+
Deploy
```

---

## Secure SDLC

```text id="q4m8wr"
Design
+
Threat Modeling
+
Secure Coding
+
Security Testing
+
Deployment Security
```

---

## Benefits

```text id="r8v2qt"
Early Detection
+
Reduced Cost
+
Improved Security
```

---

# 11. Shift Left Security

Shift Left means moving security earlier in development.

---

## Traditional Model

```text id="x6m4wp"
Development
      |
      v
Deployment
      |
      v
Security Testing
```

---

## Shift Left Model

```text id="n5v9xt"
Development
      |
      v
Security Testing
      |
      v
Deployment
```

---

## Benefits

```text id="p7m3wr"
Faster Fixes
+
Lower Cost
+
Reduced Risk
```

---

## Common Tools

```text id="m4v8qt"
SonarQube
+
SAST
+
Dependency Scanning
```

---

# 12. Defense In Depth

Defense In Depth uses multiple security layers.

---

## Concept

```text id="q5v7xt"
One Control Fails

Another Protects
```

---

## Layers

```text id="r4m9wp"
Firewall
+
Authentication
+
Authorization
+
Encryption
+
Monitoring
```

---

## Example

```text id="n7m4wr"
Internet
    |
Firewall
    |
WAF
    |
Application
    |
Database
```

---

## Benefits

```text id="x4m8wr"
Improved Protection
+
Reduced Risk
```

---

# 13. Security Testing Types

Multiple testing approaches are used.

---

## SAST

```text id="q6v3wp"
Static Analysis

Without Running Code
```

---

## DAST

```text id="r5v8qt"
Testing Running Application
```

---

## IAST

```text id="n8m4xp"
Interactive Testing
```

---

## Penetration Testing

```text id="m3v7wr"
Simulated Attack
```

---

## Security Testing Flow

```text id="x7m2qt"
Code
  |
  +---- SAST

Application
  |
  +---- DAST

Production
  |
  +---- Pen Test
```

---

# 14. Authentication

Authentication verifies identity.

---

## Question Answered

```text id="q4v8wr"
Who Are You?
```

---

## Examples

```text id="r8m3wp"
Username Password
+
MFA
+
Biometrics
+
SSO
```

---

## Authentication Flow

```text id="m6v4xt"
User
  |
  v
Credentials
  |
  v
Verification
  |
  v
Access
```

---

## Best Practices

```text id="x5v8qt"
Strong Passwords
+
MFA
+
Password Hashing
```

---

# 15. Authorization

Authorization determines what users can do.

---

## Question Answered

```text id="r7m4wp"
What Are You Allowed To Do?
```

---

## Example

```text id="n4v8wr"
Admin
→ Full Access

User
→ Limited Access
```

---

## Authorization Flow

```text id="m8v3xt"
Authenticated User
        |
        v
Permission Check
        |
        v
Allow Or Deny
```

---

## Common Models

```text id="q5m8wr"
RBAC
(Role Based Access Control)

ABAC
(Attribute Based Access Control)
```

---

# 16. Session Management

After authentication, applications use sessions to track users.

---

## Workflow

```text id="r9v2qt"
Login
   |
   v
Session Created
   |
   v
User Activity
   |
   v
Logout
```

---

## Session Components

```text id="p4m8wr"
Session ID
+
Cookie
+
Expiration
```

---

## Risks

```text id="x7m4qt"
Session Hijacking
+
Session Fixation
+
Session Theft
```

---

## Best Practices

```text id="n5v8xp"
Secure Cookies
+
HTTPS
+
Session Timeout
```

---

# 17. Cryptography Basics

Cryptography protects sensitive information.

---

## Goals

```text id="r3m9wp"
Confidentiality
+
Integrity
+
Authentication
```

---

## Encryption Types

### Symmetric Encryption

```text id="m4v8qt"
Same Key

Encrypt
+
Decrypt
```

Examples:

```text id="q8m2wr"
AES
+
ChaCha20
```

---

### Asymmetric Encryption

```text id="x6v4wp"
Public Key

+

Private Key
```

Examples:

```text id="r5m9xt"
RSA
+
ECC
```

---

## Hashing

```text id="n7v3wr"
One-Way Function
```

Examples:

```text id="x4m8wr"
SHA-256
+
SHA-512
+
bcrypt
```

---

## Best Practices

```text id="m8v4qt"
Use Modern Algorithms
+
Avoid MD5
+
Avoid SHA1
```

---

# 18. Secure Coding Principles

Secure coding reduces vulnerabilities before deployment.

---

## Core Principles

```text id="q4m8wr"
Validate Input
+
Encode Output
+
Use Least Privilege
+
Fail Securely
+
Secure Defaults
```

---

## Input Validation

```text id="r8v2qt"
Trust Nothing
From Users
```

---

## Principle of Least Privilege

```text id="x6m4wp"
Give Only Required Access
```

---

## Secure Development Flow

```text id="n5v9xt"
Write Code
      |
      v
Review
      |
      v
Analyze
      |
      v
Test
      |
      v
Deploy
```

---

## Benefits

```text id="p7m3wr"
Reduced Vulnerabilities
+
Improved Security
+
Safer Applications
```

---

# 19. OWASP Top 10 Overview

OWASP Top 10 is the industry's most recognized list of critical web application security risks.

It serves as a security awareness guide for developers, DevOps engineers, security teams, and organizations.

---

## Purpose

```text id="x7m4wp"
Identify
+
Understand
+
Prevent

Common Security Risks
```

---

## Why OWASP Top 10?

Most successful attacks exploit common vulnerabilities.

OWASP helps organizations focus on the risks that matter most.

---

## Current OWASP Top 10 (2021)

```text id="m5v8xt"
A01 Broken Access Control

A02 Cryptographic Failures

A03 Injection

A04 Insecure Design

A05 Security Misconfiguration

A06 Vulnerable Components

A07 Identification &
Authentication Failures

A08 Software &
Data Integrity Failures

A09 Security Logging &
Monitoring Failures

A10 SSRF
```

---

## Security Goal

```text id="r8v2qt"
Prevent Attacks

Before

Production
```

---

## Security Lifecycle

```text id="p4m7wr"
Design
  |
  v
Develop
  |
  v
Test
  |
  v
Deploy
  |
  v
Monitor
```

OWASP Top 10 should be considered during every phase.

---

## Why Developers Must Know It

```text id="x6v3qt"
Interview Questions
+
Real World Attacks
+
Compliance Requirements
+
Secure Development
```

---

# 20. Broken Access Control (A01)

Broken Access Control occurs when users can access resources or perform actions they should not be allowed to.

This is currently the #1 OWASP risk.

---

## What Is Access Control?

Access Control determines:

```text id="m8v4xp"
Who Can Access?
+
What Can They Access?
+
What Can They Do?
```

---

## Example

User:

```text id="r7m2wt"
Role = Customer
```

Allowed:

```text id="n5v9wr"
View Orders
+
Update Profile
```

Not Allowed:

```text id="q4m8xt"
Delete Users
+
Access Admin Panel
```

---

## Vulnerable Example

```text id="x8m2wp"
https://app.com/admin
```

Normal user changes URL manually and gains admin access.

---

## Attack Flow

```text id="r5v7qt"
User
 |
 v
Modify Request
 |
 v
Unauthorized Access
```

---

## Common Examples

```text id="m4v8wr"
Privilege Escalation
+
IDOR
+
Directory Traversal
+
Unauthorized API Access
```

---

## Prevention

```text id="p7m3xt"
Server Side Validation
+
RBAC
+
Least Privilege
+
Deny By Default
```

---

## Real World Impact

```text id="n3v8wp"
Data Theft
+
Account Takeover
+
Admin Access
```

---

# 21. Cryptographic Failures (A02)

Previously called Sensitive Data Exposure.

Occurs when sensitive information is not properly protected.

---

## Examples Of Sensitive Data

```text id="x5m9qt"
Passwords
+
Credit Cards
+
Personal Information
+
Medical Records
```

---

## Vulnerable Example

```java id="j2n8xa"
String password = "admin123";
```

---

## Another Example

Using HTTP instead of HTTPS.

```text id="r8v4wr"
User
 |
 v
HTTP
 |
 v
Attacker Reads Data
```

---

## Common Problems

```text id="m2v7xt"
Weak Encryption
+
No Encryption
+
Hardcoded Secrets
+
Weak TLS Configuration
```

---

## Strong Algorithms

```text id="q6m8wp"
AES-256
+
RSA
+
ECC
+
bcrypt
+
Argon2
```

---

## Weak Algorithms

```text id="n7m4qt"
MD5
+
SHA1
+
DES
```

---

## Prevention

```text id="x4v8wr"
Use HTTPS
+
Encrypt Sensitive Data
+
Hash Passwords
+
Protect Keys
```

---

# 22. Injection (A03)

Injection occurs when untrusted input is interpreted as commands or queries.

One of the most dangerous vulnerabilities.

---

## Common Injection Types

```text id="m8v3xt"
SQL Injection
+
Command Injection
+
LDAP Injection
+
NoSQL Injection
```

---

## SQL Injection Example

Application Query:

```sql id="a8v2wp"
SELECT * FROM users
WHERE username='admin'
AND password='123'
```

---

Attacker Input:

```sql id="m5v8qt"
' OR '1'='1
```

---

Result:

```text id="q5m7wp"
Authentication Bypass
```

---

## Attack Flow

```text id="r9v2qt"
User Input
      |
      v
Application
      |
      v
Database Query
      |
      v
Compromised Data
```

---

## Prevention

```text id="p4m8wr"
Parameterized Queries
+
Prepared Statements
+
Input Validation
+
Least Privilege
```

---

## Secure Example

```java id="x7m4qt"
PreparedStatement
```

instead of string concatenation.

---

## Impact

```text id="n5v8xp"
Database Theft
+
Data Modification
+
Remote Access
```

---

# 23. Insecure Design (A04)

Insecure Design refers to flaws in application architecture and design.

These issues exist before code is even written.

---

## Example

Application allows:

```text id="r3m9wp"
Unlimited Password Attempts
```

---

Result:

```text id="m4v8qt"
Brute Force Attack
```

---

## Design Problem

```text id="q8m2wr"
No Security Requirements
```

during planning.

---

## Examples

```text id="x6v4wp"
No MFA
+
No Rate Limiting
+
Weak Recovery Process
+
Weak Authorization Model
```

---

## Prevention

```text id="r5m9xt"
Threat Modeling
+
Security Requirements
+
Secure Architecture Reviews
```

---

## Secure Design Flow

```text id="n7v3wr"
Requirements
      |
      v
Threat Modeling
      |
      v
Secure Design
      |
      v
Implementation
```

---

# 24. Security Misconfiguration (A05)

One of the most common vulnerabilities.

Occurs when systems are insecurely configured.

---

## Examples

```text id="x4m8wr"
Default Passwords
+
Open Ports
+
Debug Mode Enabled
+
Excessive Permissions
```

---

## Common Cloud Examples

```text id="m8v4qt"
Public S3 Buckets
+
Open Security Groups
+
Weak IAM Policies
```

---

## Example

```text id="q4m8wr"
Admin/Admin
```

Default credentials still enabled.

---

## Attack Flow

```text id="r8v2qt"
Misconfiguration
       |
       v
Attacker Discovery
       |
       v
Compromise
```

---

## Prevention

```text id="x6m4wp"
Harden Systems
+
Remove Defaults
+
Patch Regularly
+
Automate Configuration
```

---

## DevOps Relevance

```text id="n5v9xt"
Docker
+
Kubernetes
+
Cloud
+
CI/CD
```

are common targets.

---

# 25. Vulnerable And Outdated Components (A06)

Applications often rely on third-party libraries.

These components may contain vulnerabilities.

---

## Example

```text id="p7m3wr"
Application
     |
     +--- Spring
     |
     +--- Log4j
     |
     +--- Jackson
```

---

## Famous Example

```text id="m4v8qt"
Log4Shell

(CVE-2021-44228)
```

---

## Attack Flow

```text id="q5v7xt"
Old Library
      |
      v
Known Vulnerability
      |
      v
Exploit
```

---

## Prevention

```text id="r4m9wp"
Dependency Scanning
+
Regular Updates
+
Patch Management
+
SBOM
```

---

## Tools

```text id="n7m4wr"
OWASP Dependency Check
+
Snyk
+
Trivy
+
GitLab Security
```

---

## Impact

```text id="x4m8wr"
Remote Code Execution
+
Data Theft
+
System Compromise
```

---

# 26. Identification & Authentication Failures (A07)

Authentication weaknesses allow attackers to impersonate users.

---

## Examples

```text id="q6v3wp"
Weak Passwords
+
Credential Stuffing
+
No MFA
+
Session Problems
```

---

## Weak Password Example

```text id="r5v8qt"
password123
```

---

## Attack Flow

```text id="n8m4xp"
Attacker
     |
     v
Credential Theft
     |
     v
Login
     |
     v
Account Access
```

---

## Prevention

```text id="m3v7wr"
Strong Passwords
+
MFA
+
Rate Limiting
+
Session Security
```

---

## Best Practices

```text id="x7m2qt"
bcrypt
+
Argon2
+
MFA
+
Account Lockout
```

---

# 27. Software & Data Integrity Failures (A08)

Software and Data Integrity Failures occur when applications trust software updates, plugins, CI/CD pipelines, packages, or data without verifying integrity.

This category was introduced to address modern software supply chain attacks.

---

## What Is Integrity?

Integrity means ensuring software and data have not been modified by unauthorized parties.

---

## Examples

```text id="x7m4wp"
Compromised Software Updates
+
Malicious Packages
+
CI/CD Pipeline Tampering
+
Untrusted Plugins
```

---

## Supply Chain Attack

```text id="m5v8xt"
Developer
     |
     v
Dependency
     |
     v
Compromised Package
     |
     v
Application
```

---

## Real World Example

```text id="r8v2qt"
SolarWinds Attack

2020
```

Attackers inserted malicious code into software updates.

---

## CI/CD Example

```text id="p4m7wr"
Developer
      |
      v
Pipeline
      |
      v
Attacker Modifies Build
      |
      v
Compromised Application
```

---

## Prevention

```text id="x6v3qt"
Code Signing
+
Artifact Verification
+
SBOM
+
Secure CI/CD
+
Dependency Validation
```

---

## Common DevSecOps Controls

```text id="m8v4xp"
Git Signing
+
Image Signing
+
Sigstore
+
Cosign
```

---

## Impact

```text id="r7m2wt"
Malware Distribution
+
Supply Chain Attacks
+
System Compromise
```

---

# 28. Security Logging & Monitoring Failures (A09)

Organizations often fail to detect attacks because logging and monitoring are inadequate.

---

## Why Logging Matters

Without logs:

```text id="n5v9wr"
Attack Occurs
      |
      v
Nobody Knows
```

---

With logging:

```text id="q4m8xt"
Attack Occurs
      |
      v
Logs Generated
      |
      v
Alert Triggered
      |
      v
Response Initiated
```

---

## Examples

```text id="x8m2wp"
Failed Login Attempts
+
Privilege Escalation
+
Unauthorized Access
+
Data Exfiltration
```

---

## Poor Logging Example

```text id="r5v7qt"
Login Failed

No Log Generated
```

---

## Good Logging Example

```text id="m4v8wr"
Timestamp
+
User
+
IP Address
+
Action
+
Result
```

---

## Monitoring Architecture

```text id="p7m3xt"
Application
      |
      v
Logs
      |
      v
SIEM
      |
      v
Alerts
```

---

## Common Tools

```text id="n3v8wp"
ELK Stack
+
Splunk
+
Grafana Loki
+
Wazuh
+
Microsoft Sentinel
```

---

## Prevention

```text id="x5m9qt"
Centralized Logging
+
Alerting
+
Log Retention
+
Incident Response
```

---

## Impact

```text id="r8v4wr"
Delayed Detection
+
Longer Attacks
+
Compliance Failures
```

---

# 29. SSRF (Server-Side Request Forgery) (A10)

SSRF occurs when an attacker tricks a server into making requests on their behalf.

---

## What Happens?

Normally:

```text id="m2v7xt"
User
  |
  v
Application
  |
  v
External Resource
```

---

With SSRF:

```text id="q6m8wp"
Attacker
    |
    v
Application
    |
    v
Internal Resource
```

---

## Vulnerable Example

Application accepts:

```text id="n7m4qt"
https://example.com/image.jpg
```

---

Attacker sends:

```text id="x4v8wr"
http://localhost/admin

or

http://169.254.169.254
```

---

## Cloud Example

```text id="m8v3xt"
AWS Metadata Service
```

Accessing:

```text id="q5m7wp"
169.254.169.254
```

may expose credentials.

---

## Attack Flow

```text id="r9v2qt"
Attacker
      |
      v
Malicious URL
      |
      v
Application Server
      |
      v
Internal Network
```

---

## Prevention

```text id="p4m8wr"
Allow Lists
+
Block Internal IPs
+
Network Segmentation
+
Input Validation
```

---

## Impact

```text id="x7m4qt"
Internal Reconnaissance
+
Credential Theft
+
Cloud Compromise
```

---

# 30. OWASP Top 10 Attack Examples

Understanding attacks is important for interviews and real-world security.

---

## Broken Access Control

```text id="n5v8xp"
User Changes URL

/admin

And Gains Access
```

---

## Cryptographic Failure

```text id="r3m9wp"
Sensitive Data

Sent Over HTTP
```

---

## Injection

```text id="m4v8qt"
' OR '1'='1
```

bypasses authentication.

---

## Security Misconfiguration

```text id="q8m2wr"
Default Passwords

Still Enabled
```

---

## Vulnerable Components

```text id="x6v4wp"
Using Old Log4j Version
```

---

## Authentication Failure

```text id="r5m9xt"
No MFA

Weak Passwords
```

---

## SSRF

```text id="n7v3wr"
Access Internal Services

Using Application Server
```

---

## Common Attack Targets

```text id="x4m8wr"
Web Applications
+
APIs
+
Containers
+
Cloud Services
+
CI/CD Pipelines
```

---

# 31. OWASP Prevention Techniques

The ultimate goal of OWASP is prevention.

---

## Core Security Principles

```text id="m8v4qt"
Prevent
+
Detect
+
Respond
```

---

## Secure Authentication

```text id="q4m8wr"
Strong Passwords
+
MFA
+
Account Lockout
```

---

## Secure Authorization

```text id="r8v2qt"
RBAC
+
Least Privilege
+
Server Side Validation
```

---

## Input Validation

```text id="x6m4wp"
Validate Everything
```

Never trust user input.

---

## Secure Cryptography

```text id="n5v9xt"
AES-256
+
bcrypt
+
Argon2
+
TLS 1.2+
```

---

## Dependency Management

```text id="p7m3wr"
Patch Regularly
+
Scan Dependencies
+
Remove Unused Libraries
```

---

## Logging & Monitoring

```text id="m4v8qt"
Centralized Logs
+
Alerting
+
Incident Detection
```

---

## Secure CI/CD

```text id="q5v7xt"
Secret Management
+
Image Scanning
+
Dependency Scanning
+
Code Review
```

---

## Defense In Depth

```text id="r4m9wp"
Firewall
+
WAF
+
Authentication
+
Authorization
+
Monitoring
```

---

## OWASP Security Checklist

```text id="n7m4wr"
Use MFA
+
Use HTTPS
+
Validate Inputs
+
Patch Systems
+
Scan Dependencies
+
Review Logs
+
Apply Least Privilege
+
Perform Security Testing
```

---

## Secure Development Lifecycle

```text id="x4m8wr"
Requirements
      |
      v
Threat Modeling
      |
      v
Secure Design
      |
      v
Secure Coding
      |
      v
Security Testing
      |
      v
Deployment
      |
      v
Monitoring
```

---

# 32. SAST (Static Application Security Testing)

SAST analyzes source code without executing the application.

It helps identify vulnerabilities early during development.

---

## Purpose

```text id="x7m4wp"
Find Security Issues

Before Deployment
```

---

## When Does SAST Run?

```text id="m5v8xt"
Developer Commit
      |
      v
CI/CD Pipeline
      |
      v
SAST Scan
```

---

## What SAST Detects

```text id="r8v2qt"
SQL Injection
+
Hardcoded Secrets
+
Weak Cryptography
+
Authentication Issues
+
Code Vulnerabilities
```

---

## Workflow

```text id="p4m7wr"
Source Code
      |
      v
SAST Scanner
      |
      v
Security Report
```

---

## Popular SAST Tools

```text id="x6v3qt"
SonarQube
+
Semgrep
+
Checkmarx
+
Fortify
+
Veracode
```

---

## Advantages

```text id="m8v4xp"
Early Detection
+
Fast Feedback
+
Developer Friendly
```

---

## Limitations

```text id="r7m2wt"
Cannot Detect
Runtime Issues
```

---

# 33. DAST (Dynamic Application Security Testing)

DAST tests a running application from the outside.

It behaves like an attacker attempting to find vulnerabilities.

---

## Purpose

```text id="n5v9wr"
Find Runtime Vulnerabilities
```

---

## Workflow

```text id="q4m8xt"
Running Application
       |
       v
DAST Tool
       |
       v
Security Findings
```

---

## What DAST Detects

```text id="x8m2wp"
SQL Injection
+
XSS
+
Authentication Issues
+
Misconfigurations
```

---

## Popular DAST Tools

```text id="r5v7qt"
OWASP ZAP
+
Burp Suite
+
Acunetix
+
Invicti
```

---

## Advantages

```text id="m4v8wr"
Real World Testing
+
No Source Code Needed
```

---

## Limitations

```text id="p7m3xt"
Requires Running Application
```

---

## SAST vs DAST

| Feature              | SAST | DAST |
| -------------------- | ---- | ---- |
| Source Code Required | Yes  | No   |
| Running App Required | No   | Yes  |
| Early Detection      | Yes  | No   |
| Runtime Validation   | No   | Yes  |

---

# 34. IAST (Interactive Application Security Testing)

IAST combines SAST and DAST approaches.

It monitors applications during execution.

---

## Architecture

```text id="n3v8wp"
Application
      |
      v
IAST Agent
      |
      v
Security Analysis
```

---

## How It Works

```text id="x5m9qt"
Run Application
      |
      v
Monitor Internally
      |
      v
Detect Vulnerabilities
```

---

## Benefits

```text id="r8v4wr"
More Accurate Results
+
Lower False Positives
+
Runtime Visibility
```

---

## Common Findings

```text id="m2v7xt"
SQL Injection
+
XSS
+
Authentication Issues
```

---

## Examples

```text id="q6m8wp"
Contrast Security
+
HCL AppScan
```

---

# 35. SCA (Software Composition Analysis)

SCA analyzes third-party dependencies and libraries.

Modern applications often contain more open-source code than custom code.

---

## Why SCA?

```text id="n7m4qt"
Application
     |
     +--- Frameworks

     +--- Libraries

     +--- Packages
```

---

## Common Risks

```text id="x4v8wr"
Known CVEs
+
Outdated Libraries
+
Supply Chain Attacks
```

---

## Workflow

```text id="m8v3xt"
Dependencies
       |
       v
SCA Tool
       |
       v
Vulnerability Report
```

---

## Popular Tools

```text id="q5m7wp"
OWASP Dependency Check
+
Snyk
+
Trivy
+
Dependabot
```

---

## Benefits

```text id="r9v2qt"
Supply Chain Visibility
+
Risk Reduction
+
Compliance
```

---

# 36. Penetration Testing

Penetration Testing (Pen Testing) simulates real-world attacks.

The objective is to identify vulnerabilities before attackers do.

---

## Pen Test Process

```text id="p4m8wr"
Reconnaissance
      |
      v
Scanning
      |
      v
Exploitation
      |
      v
Reporting
```

---

## Types

```text id="x7m4qt"
Black Box
+
Gray Box
+
White Box
```

---

## Black Box

```text id="n5v8xp"
No Internal Knowledge
```

---

## White Box

```text id="r3m9wp"
Full Access
+
Source Code
+
Architecture
```

---

## Common Tools

```text id="m4v8qt"
Burp Suite
+
OWASP ZAP
+
Nmap
+
Metasploit
```

---

## Goal

```text id="q8m2wr"
Identify Real Attack Paths
```

---

# 37. Dependency Scanning

Dependency Scanning identifies vulnerable third-party packages.

---

## Example

```text id="x6v4wp"
Application
     |
     +--- Spring

     +--- Log4j

     +--- Jackson
```

---

## Famous Example

```text id="r5m9xt"
Log4Shell

CVE-2021-44228
```

---

## Workflow

```text id="n7v3wr"
Dependency
      |
      v
Vulnerability Database
      |
      v
Risk Report
```

---

## Benefits

```text id="x4m8wr"
Supply Chain Security
+
Reduced Risk
```

---

## Common Tools

```text id="m8v4qt"
Dependency Check
+
Trivy
+
Snyk
+
GitLab Security
```

---

# 38. Container Security

Containers are a major attack surface in modern applications.

---

## Container Risks

```text id="q4m8wr"
Vulnerable Images
+
Exposed Secrets
+
Privilege Escalation
+
Misconfigurations
```

---

## Container Security Workflow

```text id="r8v2qt"
Container Image
       |
       v
Security Scan
       |
       v
Deploy
```

---

## Security Layers

```text id="x6m4wp"
Image Security
+
Runtime Security
+
Registry Security
```

---

## Best Practices

```text id="n5v9xt"
Minimal Images
+
Image Scanning
+
No Root User
+
Signed Images
```

---

## Common Tools

```text id="p7m3wr"
Trivy
+
Clair
+
Anchore
+
Falco
```

---

# 39. Kubernetes Security

Kubernetes introduces additional security challenges.

---

## Common Risks

```text id="m4v8qt"
Privileged Containers
+
Open Dashboards
+
RBAC Misconfiguration
+
Secrets Exposure
```

---

## Kubernetes Security Layers

```text id="q5v7xt"
Cluster
+
Node
+
Pod
+
Network
+
Application
```

---

## Security Controls

```text id="r4m9wp"
RBAC
+
Network Policies
+
Pod Security
+
Image Scanning
```

---

## Best Practices

```text id="n7m4wr"
Least Privilege
+
Namespace Isolation
+
Secrets Management
+
Audit Logging
```

---

## Common Tools

```text id="x4m8wr"
Falco
+
Kyverno
+
OPA Gatekeeper
+
Trivy
```

---

# 40. API Security

APIs are one of the most targeted attack surfaces.

---

## Why APIs Are Targeted

```text id="q6v3wp"
Direct Data Access
+
Business Logic Exposure
+
Automation Friendly
```

---

## API Security Risks

```text id="r5v8qt"
Broken Authentication
+
Broken Authorization
+
Data Exposure
+
Rate Limit Bypass
```

---

## API Security Workflow

```text id="n8m4xp"
Client
  |
  v
API Gateway
  |
  v
Application
```

---

## Best Practices

```text id="m3v7wr"
Authentication
+
Authorization
+
Rate Limiting
+
Input Validation
```

---

## Common Security Mechanisms

```text id="x7m2qt"
OAuth2
+
JWT
+
API Keys
+
mTLS
```

---

# 41. OWASP API Top 10

OWASP maintains a separate Top 10 for APIs.

---

## Common API Risks

```text id="p8m4wr"
Broken Object Level Authorization

Broken Authentication

Excessive Data Exposure

Mass Assignment

Security Misconfiguration

Injection

Improper Asset Management
```

---

## Example

```text id="r7v3xt"
GET /api/users/123

User Changes

123 → 124
```

Result:

```text id="n4m8wp"
Unauthorized Data Access
```

---

## Why Important?

```text id="x5v7qt"
Most Modern Applications
Are API Driven
```

---

# 42. Secrets Management

Secrets must never be hardcoded into applications.

---

## Examples Of Secrets

```text id="m8v2wr"
Passwords
+
API Keys
+
Database Credentials
+
Certificates
+
Tokens
```

---

## Bad Practice

```java id="v3m7ka"
password="admin123"
```

---

## Better Approach

```text id="r5v8wp"
Secrets Manager
```

---

## Popular Solutions

```text id="n7m4xt"
HashiCorp Vault
+
AWS Secrets Manager
+
Azure Key Vault
+
GCP Secret Manager
```

---

## Benefits

```text id="p4m9wr"
Improved Security
+
Centralized Management
+
Rotation Support
```

---

# 43. Supply Chain Security

Modern attacks increasingly target software supply chains.

---

## Supply Chain Components

```text id="x8m3qt"
Source Code
+
Dependencies
+
Build System
+
Container Images
+
CI/CD
```

---

## Example Attack

```text id="m5v8wr"
Compromised Dependency
      |
      v
Application
      |
      v
Production
```

---

## Security Controls

```text id="r4v9xp"
Dependency Scanning
+
Image Signing
+
Artifact Verification
+
SBOM
```

---

## Goal

```text id="n6m4qt"
Trust But Verify
```

---

# 44. SBOM (Software Bill of Materials)

SBOM is an inventory of software components used in an application.

---

## Similar To

```text id="p7v3wr"
Ingredients List
For Software
```

---

## Example

```text id="x5m8qt"
Application
     |
     +--- Spring Boot

     +--- Log4j

     +--- Jackson
```

---

## Why Important?

```text id="r8m2wp"
Vulnerability Tracking
+
Compliance
+
Supply Chain Security
```

---

## Popular Formats

```text id="n4v8xt"
CycloneDX
+
SPDX
```

---

## Benefits

```text id="m7v3qt"
Transparency
+
Risk Visibility
```

---

# 45. CI/CD Security

CI/CD pipelines are high-value targets.

---

## Risks

```text id="q8m4wr"
Secrets Exposure
+
Pipeline Tampering
+
Malicious Code Injection
```

---

## Secure Pipeline

```text id="r5v9xt"
Code
 |
 v
Scan
 |
 v
Build
 |
 v
Deploy
```

---

## Security Controls

```text id="n7v2wp"
SAST
+
SCA
+
Container Scanning
+
Secrets Detection
```

---

## Best Practices

```text id="x4m8qt"
Protected Branches
+
Signed Commits
+
Least Privilege
```

---

# 46. SonarQube and OWASP

SonarQube helps identify many OWASP-related issues.

---

## Examples

```text id="m5v7wr"
Injection Risks
+
Hardcoded Secrets
+
Weak Cryptography
+
Authentication Problems
```

---

## Workflow

```text id="p8m4xt"
Developer
      |
      v
SonarQube Scan
      |
      v
OWASP Findings
```

---

## Benefits

```text id="r6m9wp"
Shift Left Security
+
Developer Awareness
```

---

# 47. GitLab Security Scanning

GitLab provides built-in security scanning aligned with OWASP recommendations.

---

## Security Features

```text id="x7m2qt"
SAST
+
DAST
+
Dependency Scanning
+
Container Scanning
+
Secret Detection
```

---

## Workflow

```text id="n5v8wr"
Commit
  |
  v
Pipeline
  |
  v
Security Scan
```

---

## Benefits

```text id="m4v7xt"
Integrated DevSecOps
+
Automated Security
```

---

# 48. Jenkins Security Practices

Jenkins is powerful but requires careful security configuration.

---

## Common Risks

```text id="q6m8wp"
Outdated Plugins
+
Excessive Permissions
+
Secret Exposure
```

---

## Security Controls

```text id="r8v4xt"
RBAC
+
Credential Store
+
Plugin Updates
+
Pipeline Security
```

---

## Best Practices

```text id="x5m9wr"
Use HTTPS
+
Secure Agents
+
Rotate Credentials
+
Limit Admin Access
```

---

## Goal

```text id="n7m3qt"
Secure CI/CD Pipeline
```

---

# 49. Security Monitoring

Security Monitoring helps detect attacks, suspicious activity, policy violations, and security incidents.

Monitoring is a critical component of a secure production environment.

---

## Why Security Monitoring?

Without monitoring:

```text id="x7m4wp"
Attack Occurs
      |
      v
Nobody Notices
```

---

With monitoring:

```text id="m5v8xt"
Attack Occurs
      |
      v
Detection
      |
      v
Alert
      |
      v
Response
```

---

## What Should Be Monitored?

```text id="r8v2qt"
Authentication Events
+
Authorization Failures
+
Network Activity
+
Application Logs
+
API Requests
```

---

## Monitoring Architecture

```text id="p4m7wr"
Application
      |
      v
Logs
      |
      v
SIEM
      |
      v
Alerts
```

---

## Common Tools

```text id="x6v3qt"
Splunk
+
ELK Stack
+
Wazuh
+
Microsoft Sentinel
+
Grafana Loki
```

---

## Benefits

```text id="m8v4xp"
Attack Detection
+
Incident Visibility
+
Compliance
```

---

# 50. Incident Response

Incident Response is the process of handling security incidents.

---

## Goal

```text id="r7m2wt"
Detect
+
Contain
+
Eradicate
+
Recover
```

---

## Incident Response Lifecycle

```text id="n5v9wr"
Preparation
      |
      v
Detection
      |
      v
Containment
      |
      v
Eradication
      |
      v
Recovery
      |
      v
Lessons Learned
```

---

## Example Incident

```text id="q4m8xt"
Compromised Account
```

---

## Response Actions

```text id="x8m2wp"
Disable Account
+
Investigate Logs
+
Rotate Credentials
+
Restore Service
```

---

## Benefits

```text id="r5v7qt"
Reduced Damage
+
Faster Recovery
+
Improved Security
```

---

# 51. Security Best Practices

Security should be implemented throughout the software lifecycle.

---

## Core Principles

```text id="m4v8wr"
Least Privilege
+
Defense In Depth
+
Zero Trust
+
Secure Defaults
```

---

## Development Best Practices

```text id="p7m3xt"
Input Validation
+
Output Encoding
+
Secure Coding
+
Code Reviews
```

---

## Infrastructure Best Practices

```text id="n3v8wp"
Patch Systems
+
Use MFA
+
Encrypt Data
+
Network Segmentation
```

---

## DevSecOps Best Practices

```text id="x5m9qt"
SAST
+
DAST
+
Dependency Scanning
+
Container Scanning
```

---

## Security Checklist

```text id="r8v4wr"
HTTPS Everywhere
+
Strong Authentication
+
Least Privilege
+
Continuous Monitoring
```

---

# 52. Common Security Mistakes

Many security incidents result from basic mistakes.

---

## Mistake 1

```text id="m2v7xt"
Hardcoded Credentials
```

---

## Mistake 2

```text id="q6m8wp"
Trusting User Input
```

---

## Mistake 3

```text id="n7m4qt"
Using Outdated Libraries
```

---

## Mistake 4

```text id="x4v8wr"
No MFA
```

---

## Mistake 5

```text id="m8v3xt"
Excessive Permissions
```

---

## Mistake 6

```text id="q5m7wp"
Ignoring Security Alerts
```

---

## Result

```text id="r9v2qt"
Data Breaches
+
Unauthorized Access
+
Financial Loss
```

---

# 53. OWASP Cheat Sheets

OWASP Cheat Sheets provide practical implementation guidance.

They are among the most useful resources for developers.

---

## Popular Cheat Sheets

```text id="p4m8wr"
Authentication
+
Authorization
+
Password Storage
+
Input Validation
+
Session Management
+
Cryptography
```

---

## Purpose

```text id="x7m4qt"
Security Best Practices

In Practical Format
```

---

## Benefits

```text id="n5v8xp"
Developer Friendly
+
Easy To Implement
+
Industry Trusted
```

---

## Common Usage

```text id="r3m9wp"
Design
+
Development
+
Code Reviews
```

---

# 54. Security Headers

Security Headers instruct browsers to enforce security controls.

---

## Why Important?

```text id="m4v8qt"
Additional Browser Protection
```

---

## Common Headers

```text id="q8m2wr"
Content-Security-Policy

Strict-Transport-Security

X-Frame-Options

X-Content-Type-Options

Referrer-Policy
```

---

## Example

```http id="x6v4wp"
Strict-Transport-Security:
max-age=31536000
```

---

## Benefits

```text id="r5m9xt"
Mitigate XSS
+
Prevent Clickjacking
+
Enforce HTTPS
```

---

## Security Header Flow

```text id="n7v3wr"
Browser
    |
    v
Security Headers
    |
    v
Additional Protection
```

---

# 55. Secure Authentication

Authentication verifies user identity.

---

## Secure Authentication Components

```text id="x4m8wr"
Strong Passwords
+
MFA
+
Password Hashing
+
Account Lockout
```

---

## Authentication Flow

```text id="m8v4qt"
User
  |
  v
Credentials
  |
  v
Verification
  |
  v
Access
```

---

## Password Storage

Never store:

```text id="q4m8wr"
Plain Text Passwords
```

---

Use:

```text id="r8v2qt"
bcrypt
+
Argon2
+
PBKDF2
```

---

## MFA

```text id="x6m4wp"
Password
+
Second Factor
```

---

## Benefits

```text id="n5v9xt"
Reduced Account Takeover
+
Improved Security
```

---

# 56. Secure Authorization

Authorization determines what authenticated users can access.

---

## Principle

```text id="p7m3wr"
Least Privilege
```

---

## Common Models

### RBAC

```text id="m4v8qt"
Role Based Access Control
```

---

Example:

```text id="q5v7xt"
Admin
+
Developer
+
User
```

---

### ABAC

```text id="r4m9wp"
Attribute Based Access Control
```

---

Uses:

```text id="n7m4wr"
Role
+
Department
+
Location
+
Time
```

---

## Authorization Flow

```text id="x4m8wr"
Authenticated User
        |
        v
Permission Check
        |
        +---- Allow

        +---- Deny
```

---

## Best Practices

```text id="q6v3wp"
Server Side Validation
+
Least Privilege
+
Default Deny
```

---

# 57. Common Interview Questions

## What is OWASP?

```text id="r5v8qt"
Open Worldwide
Application Security Project
```

---

## What is OWASP Top 10?

```text id="n8m4xp"
Most Critical
Web Application Security Risks
```

---

## What is Broken Access Control?

```text id="m3v7wr"
Users Accessing Resources

They Should Not Access
```

---

## What is SQL Injection?

```text id="x7m2qt"
Malicious Input

Modifies SQL Queries
```

---

## What is SSRF?

```text id="p8m4wr"
Server Forced To Make

Unexpected Requests
```

---

## Difference Between Authentication And Authorization?

```text id="r7v3xt"
Authentication
=
Who Are You?

Authorization
=
What Can You Do?
```

---

## What is SAST?

```text id="n4m8wp"
Static Analysis

Without Running Code
```

---

## What is DAST?

```text id="x5v7qt"
Testing A Running Application
```

---

## What is Defense In Depth?

```text id="m8v2wr"
Multiple Security Layers
```

---

## What is Least Privilege?

```text id="r5v8wp"
Only Minimum Required Access
```

---

# 58. Additional Resources

## Official Resources

| Resource           | URL                                                                                                                                                  |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| OWASP              | [https://owasp.org](https://owasp.org)                                                                                                               |
| OWASP Top 10       | [https://owasp.org/www-project-top-ten](https://owasp.org/www-project-top-ten)                                                                       |
| OWASP API Top 10   | [https://owasp.org/www-project-api-security](https://owasp.org/www-project-api-security)                                                             |
| OWASP Cheat Sheets | [https://cheatsheetseries.owasp.org](https://cheatsheetseries.owasp.org)                                                                             |
| OWASP ASVS         | [https://owasp.org/www-project-application-security-verification-standard](https://owasp.org/www-project-application-security-verification-standard) |
| OWASP ZAP          | [https://www.zaproxy.org](https://www.zaproxy.org)                                                                                                   |

---

# 📎 Official Documentation

* [https://owasp.org](https://owasp.org)
* [https://owasp.org/www-project-top-ten](https://owasp.org/www-project-top-ten)
* [https://owasp.org/www-project-api-security](https://owasp.org/www-project-api-security)
* [https://cheatsheetseries.owasp.org](https://cheatsheetseries.owasp.org)
* [https://owasp.org/www-project-application-security-verification-standard](https://owasp.org/www-project-application-security-verification-standard)
* [https://www.zaproxy.org](https://www.zaproxy.org)

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, software engineering, DevOps engineering, DevSecOps, cloud security, application security, platform engineering, and secure software development.

Primary references include:

* OWASP Top 10
* OWASP API Top 10
* OWASP ASVS
* OWASP Cheat Sheet Series
* OWASP ZAP Documentation

OWASP is a non-profit foundation dedicated to improving software security through community-driven projects, standards, tools, and education.

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

```text id="x8m4qt"
Application Security
+
OWASP Top 10
+
Secure Coding
+
DevSecOps
+
Security Testing
+
Risk Reduction
```

OWASP provides the foundation for modern application security by helping organizations identify, understand, prevent, and mitigate the most critical software security risks throughout the software development lifecycle.

---

```md id="n7v3wr"
**Last Updated:** 2026
**Platform:** OWASP
**Version:** OWASP Top 10 (2021)
**License:** Open Community Project

For latest information, visit https://owasp.org
```