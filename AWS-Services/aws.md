# ☁️ AWS Services Reference

A categorized reference guide covering AWS services, their purpose, and common use cases for Cloud Engineers, DevOps Engineers, Platform Engineers, SREs, Solution Architects, and Developers.

---

# Table of Contents

- [Compute](#compute)
- [Containers](#containers)
- [Storage](#storage)
- [Database](#database)
- [Migration & Transfer](#migration--transfer)
- [Networking & Content Delivery](#networking--content-delivery)
- [Developer Tools](#developer-tools)
- [Management & Governance](#management--governance)
- [Media Services](#media-services)
- [Machine Learning](#machine-learning)
- [Analytics](#analytics)
- [Security, Identity & Compliance](#security-identity--compliance)
- [Cloud Financial Management](#cloud-financial-management)
- [Front-End Web & Mobile](#front-end-web--mobile)
- [Application Integration](#application-integration)
- [Business Applications](#business-applications)
- [End User Computing](#end-user-computing)
- [Internet of Things](#internet-of-things)
- [Blockchain](#blockchain)
- [Satellite](#satellite)
- [Quantum Technologies](#quantum-technologies)
- [Game Development](#game-development)

---

# Compute

### Amazon EC2

Scalable virtual servers in the cloud that provide complete control over operating systems, applications, storage, and networking.

### Amazon Lightsail

Simplified VPS service with predictable pricing for websites, blogs, development environments, and small business applications.

### AWS Lambda

Serverless compute service that executes code in response to events without provisioning or managing servers.

### AWS Batch

Managed batch processing service that automatically provisions compute resources for large-scale workloads.

### AWS Elastic Beanstalk

Platform as a Service (PaaS) that simplifies deployment, scaling, and management of web applications.

### AWS Serverless Application Repository

Repository for discovering, sharing, and deploying reusable serverless applications and components.

### AWS Outposts

Brings AWS infrastructure, services, and APIs to on-premises data centers for hybrid cloud deployments.

### EC2 Image Builder

Automates creation, testing, patching, and maintenance of secure and standardized Amazon Machine Images (AMIs).

### AWS App Runner

Fully managed service for deploying containerized web applications and APIs directly from source code or container images.

### AWS Parallel Computing Service

Managed High Performance Computing (HPC) service for scientific research, simulations, and large-scale computational workloads.

### AWS Global View

Provides centralized visibility and management across multiple AWS Regions and accounts.

---

# Containers

### Amazon ECS (Elastic Container Service)

Fully managed container orchestration service for running and scaling Docker containers on AWS.

### Amazon EKS (Elastic Kubernetes Service)

Managed Kubernetes service that simplifies deployment, operation, and scaling of Kubernetes clusters.

### Red Hat OpenShift Service on AWS (ROSA)

Fully managed OpenShift platform jointly operated by AWS and Red Hat for enterprise container applications.

### Amazon ECR (Elastic Container Registry)

Secure container registry for storing, managing, scanning, and distributing Docker and OCI container images.

---

# Storage

### Amazon S3 (Simple Storage Service)

Highly durable object storage service used for backups, static websites, data lakes, application storage, and archives.

### Amazon EFS (Elastic File System)

Elastic shared file storage accessible from multiple EC2 instances simultaneously using standard NFS protocols.

### Amazon FSx

Managed high-performance file systems including Windows File Server, Lustre, NetApp ONTAP, and OpenZFS.

### Amazon S3 Glacier

Low-cost archival storage designed for long-term retention, compliance, and backup requirements.

### AWS Storage Gateway

Hybrid storage service connecting on-premises environments with AWS cloud storage services.

### AWS Backup

Centralized backup management service that automates backups across AWS resources and accounts.

### Recycle Bin

Retention service that enables recovery of deleted supported AWS resources such as EBS snapshots.

### AWS Elastic Disaster Recovery

Disaster recovery service that minimizes downtime and data loss by continuously replicating workloads to AWS.

---

# Database

### Amazon Aurora

Cloud-native relational database compatible with MySQL and PostgreSQL that delivers high performance and availability.

### Amazon RDS (Relational Database Service)

Managed relational database service supporting MySQL, PostgreSQL, MariaDB, Oracle, and SQL Server.

### Amazon ElastiCache

Managed Redis and Memcached service that improves application performance through in-memory caching.

### Amazon Neptune

Fully managed graph database service designed for highly connected datasets and relationship analysis.

### Amazon DocumentDB

MongoDB-compatible document database service for storing and querying JSON-based application data.

### Amazon Keyspaces

Serverless Apache Cassandra-compatible database service for large-scale NoSQL workloads.

### Amazon Timestream

Purpose-built time-series database optimized for IoT applications, operational monitoring, and metrics collection.

### Amazon DynamoDB

Serverless NoSQL key-value and document database delivering single-digit millisecond performance at any scale.

### Aurora DSQL

Distributed SQL database designed for globally distributed, highly available transactional workloads.

### Amazon MemoryDB

Durable Redis-compatible in-memory database providing ultra-low latency and high throughput.

### Oracle Database@AWS

Oracle database services integrated with AWS infrastructure, networking, and operational capabilities.

---

# Migration & Transfer

### AWS Migration Hub

Provides a centralized dashboard to track and manage application, server, and database migrations across multiple AWS migration services.

### AWS Application Migration Service (MGN)

Simplifies lift-and-shift migrations by replicating physical, virtual, and cloud servers into AWS with minimal downtime.

### AWS Application Discovery Service

Collects information about on-premises servers, applications, and dependencies to assist migration planning.

### AWS Database Migration Service (DMS)

Migrates databases to AWS securely with minimal downtime while supporting homogeneous and heterogeneous migrations.

### AWS Transfer Family

Managed file transfer service supporting SFTP, FTPS, FTP, and AS2 protocols directly with AWS storage services.

### AWS Snow Family

Physical devices used to transfer large amounts of data to AWS when network connectivity is limited or expensive.

### AWS DataSync

Automates and accelerates online data transfers between on-premises storage, AWS services, and cloud providers.

### AWS Transform

Uses AI-assisted modernization capabilities to accelerate migration and application transformation projects.

### AWS Mainframe Modernization

Helps migrate, modernize, and run mainframe applications using cloud-native AWS services.

### Amazon Elastic VMware Service

Runs VMware workloads on AWS infrastructure while maintaining familiar VMware tools and operational processes.

---

# Networking & Content Delivery

### Amazon VPC

Provides logically isolated virtual networks in AWS with complete control over IP addressing, routing, and security.

### Amazon CloudFront

Global Content Delivery Network (CDN) that delivers content with low latency using AWS edge locations worldwide.

### Amazon API Gateway

Creates, publishes, secures, and manages REST, HTTP, and WebSocket APIs at any scale.

### AWS Direct Connect

Provides dedicated private network connectivity between on-premises infrastructure and AWS.

### AWS App Mesh

Service mesh that standardizes communication, monitoring, and security between microservices.

### AWS Global Accelerator

Improves application availability and performance using AWS global network infrastructure.

### Amazon Route 53

Highly available DNS service for domain registration, DNS routing, health checks, and traffic management.

### AWS Data Transfer Terminal

Physical locations that enable customers to transfer large datasets directly into AWS infrastructure.

### Amazon Route 53 Global Resolver

Provides centralized DNS resolution across hybrid cloud and multi-region environments.

### AWS Cloud Map

Service discovery solution that helps applications dynamically discover resources and services.

### RTB Fabric

AWS networking fabric service used for advanced routing and large-scale network connectivity management.

### AWS Application Recovery Controller

Improves application resilience by coordinating failover and recovery across multiple AWS Regions.

---

# Developer Tools

### AWS CodeCommit

Managed Git repository service for secure source code storage and version control.

### AWS CodeBuild

Fully managed build service that compiles source code, runs tests, and produces deployment artifacts.

### AWS CodeDeploy

Automates deployments to EC2, Lambda, ECS, and on-premises servers while reducing deployment risks.

### AWS CodePipeline

Continuous Integration and Continuous Delivery (CI/CD) service for automating software release pipelines.

### AWS Cloud9

Browser-based IDE that allows developers to write, run, and debug code directly from the cloud.

### AWS CloudShell

Browser-based shell environment preconfigured with AWS CLI and common development tools.

### AWS X-Ray

Distributed tracing service used to analyze application performance and troubleshoot microservices.

### AWS Fault Injection Service (FIS)

Chaos engineering service that helps test application resilience by injecting controlled failures.

### AWS Infrastructure Composer

Visual infrastructure design tool for creating and managing serverless application architectures.

### AWS App Studio

Low-code application development platform that helps build business applications quickly.

### AWS DevOps Agent

AI-powered assistant that helps automate operational tasks, troubleshooting, and DevOps workflows.

### AWS AppConfig

Safely deploys application configurations and feature flags without requiring code deployments.

### AWS CodeArtifact

Managed artifact repository for storing and sharing software packages and dependencies.

### Amazon Q Developer

Generative AI assistant that helps developers write, debug, explain, and optimize code.

### Amazon CodeCatalyst

Integrated software development service that combines planning, collaboration, CI/CD, and automation.

### Kiro

AI-assisted development environment designed to accelerate software engineering workflows.

---

# Management & Governance

### AWS Organizations

Centralized account management service that helps create, organize, and govern multiple AWS accounts from a single location.

### Amazon CloudWatch

Monitoring and observability service that collects metrics, logs, events, and alarms across AWS resources and applications.

### AWS Auto Scaling

Automatically adjusts compute resources based on demand to improve availability and optimize costs.

### AWS CloudFormation

Infrastructure as Code (IaC) service that provisions and manages AWS resources using templates.

### AWS Config

Tracks AWS resource configurations and records changes to support auditing, compliance, and governance.

### AWS Service Catalog

Allows organizations to create and manage approved catalogs of AWS resources and products.

### AWS Systems Manager

Operational management service used for patching, automation, remote access, configuration management, and inventory tracking.

### AWS Trusted Advisor

Provides recommendations to improve cost optimization, security, performance, reliability, and service limits.

### AWS Control Tower

Automates the setup and governance of secure multi-account AWS environments using best practices.

### AWS Well-Architected Tool

Reviews workloads against AWS Well-Architected Framework best practices and improvement recommendations.

### Amazon Q Developer in Chat Applications (AWS Chatbot)

Enables interaction with AWS services through chat platforms such as Slack and Microsoft Teams.

### AWS Launch Wizard

Guides deployment of enterprise applications like SAP, SQL Server, and Microsoft workloads on AWS.

### AWS Compute Optimizer

Analyzes resource utilization and recommends right-sizing opportunities to reduce costs and improve performance.

### AWS Resource Groups & Tag Editor

Organizes AWS resources using tags and simplifies bulk resource management operations.

### Amazon Managed Grafana

Fully managed Grafana service used to visualize metrics, logs, traces, and observability data.

### Amazon Managed Service for Prometheus

Managed Prometheus service for collecting, storing, and querying cloud-native metrics at scale.

### AWS Resilience Hub

Evaluates and improves application resilience against failures, outages, and disasters.

### AWS Incident Manager

Helps prepare for, respond to, and recover from operational incidents using automated response plans.

### AWS for SAP

Provides services, tools, and best practices for deploying and operating SAP workloads on AWS.

### AWS Telco Network Builder

Automates planning and deployment of telecommunications network infrastructure on AWS.

### AWS Health Dashboard

Provides personalized visibility into AWS service events, maintenance, and resource health issues.

### AWS Proton

Platform engineering service that automates infrastructure provisioning and application deployments.

### AWS Sustainability

Provides insights and reporting tools to measure and reduce cloud infrastructure carbon footprint.

### AWS User Notifications

Centralized notification service that delivers AWS events and alerts to users and teams.

### AWS Partner Central

Portal that helps AWS Partners manage customer engagements, opportunities, and AWS collaboration.

### AWS CloudTrail

Records AWS API activity and account actions for auditing, security monitoring, and compliance.

### AWS License Manager

Tracks and manages software licenses across AWS and hybrid environments.

### AWS Resource Explorer

Search service that helps discover AWS resources across accounts and Regions.

### AWS Service Quotas

Tracks, monitors, and requests increases for AWS service limits and quotas.

---

# Customer Enablement

### AWS IQ

Marketplace that connects customers with AWS-certified experts for consulting and implementation services.

### AWS Managed Services

Managed operations service that helps organizations run AWS environments following AWS best practices.

### AWS Activate for Startups

Program that provides startups with AWS credits, training, technical support, and business resources.

### AWS re:Post Private

Private knowledge-sharing platform that enables organizations to collaborate with AWS experts.

### AWS Support

Provides technical guidance, troubleshooting, architectural recommendations, and operational assistance.

---

# Blockchain

### Amazon Managed Blockchain

Managed blockchain service that simplifies creation and management of blockchain networks using Hyperledger Fabric and Ethereum-compatible technologies.

---

# Satellite

### AWS Ground Station

Managed service that enables communication, control, and data processing for satellite operations.

---

# Quantum Technologies

### Amazon Braket

Quantum computing service that provides access to quantum hardware, simulators, and development tools.

---

# Media Services

### Amazon Kinesis Video Streams

Securely streams, stores, and processes video data from connected devices for analytics, machine learning, and media applications.

### AWS Elemental MediaConvert

File-based video transcoding service that converts media into formats suitable for broadcast and streaming platforms.

### AWS Elemental MediaLive

Live video processing service used to create high-quality broadcast streams for television and online delivery.

### AWS Elemental MediaPackage

Prepares and delivers video content securely to multiple devices using adaptive bitrate streaming.

### AWS Elemental MediaStore

Optimized storage service designed specifically for media workflows requiring low-latency access.

### AWS Elemental MediaTailor

Inserts personalized advertisements into video streams for monetization and audience targeting.

### AWS Elemental Appliances & Software

On-premises media processing solutions for video encoding, packaging, and delivery.

### Amazon Interactive Video Service (IVS)

Managed live-streaming service that enables developers to build interactive video experiences.

### AWS Elemental Inference

Video analytics solution that applies machine learning models to live video streams.

### AWS Deadline Cloud

Managed render farm service for visual effects, animation, and media production workloads.

### AWS Elemental MediaConnect

Reliable transport service for live video feeds between broadcasters, partners, and AWS services.

---

# Machine Learning

### Amazon SageMaker AI

End-to-end machine learning platform used to build, train, deploy, and manage ML models at scale.

### Amazon Augmented AI (A2I)

Adds human review workflows into machine learning predictions to improve model accuracy.

### Amazon CodeGuru

Machine learning-powered code review and application performance optimization service.

### Amazon DevOps Guru

Uses machine learning to detect operational issues and performance anomalies automatically.

### Amazon Comprehend

Natural Language Processing (NLP) service that extracts insights, entities, sentiment, and key phrases from text.

### Amazon Forecast

Machine learning forecasting service used to predict future business outcomes such as demand and sales.

### Amazon Fraud Detector

Detects fraudulent activities using machine learning models built specifically for fraud prevention.

### Amazon Kendra

Enterprise search service powered by machine learning for intelligent document and knowledge retrieval.

### Amazon Personalize

Builds personalized recommendations, product suggestions, and customer experiences.

### Amazon Polly

Converts text into natural-sounding speech using advanced speech synthesis technology.

### Amazon Rekognition

Analyzes images and videos to identify objects, faces, activities, text, and unsafe content.

### Amazon Textract

Extracts text, forms, tables, and structured information from scanned documents and PDFs.

### Amazon Transcribe

Automatically converts spoken audio into text for transcription and speech analytics.

### Amazon Translate

Neural machine translation service supporting multiple languages for global applications.

### AWS Panorama

Brings computer vision capabilities to on-premises cameras and edge devices.

### Amazon Monitron

Uses machine learning and sensors to detect equipment issues and predict maintenance requirements.

### AWS HealthLake

Healthcare-focused data lake service that stores and analyzes healthcare information using AI.

### Amazon Lookout for Equipment

Predictive maintenance service that detects abnormal equipment behavior using machine learning.

### Amazon Q Business

Generative AI assistant that enables employees to search, analyze, and interact with organizational data.

### Amazon Bedrock (Model Endpoint)

Provides managed access to foundation models through secure API endpoints.

### Claude Platform on AWS

Anthropic Claude models available through AWS infrastructure for enterprise AI applications.

### AWS HealthOmics

Managed service for storing, processing, and analyzing genomics and biological data.

### Amazon Nova Act

AI-powered automation service designed for intelligent workflow execution and task orchestration.

### Amazon Bedrock

Fully managed service for building generative AI applications using foundation models from leading AI providers.

### Amazon Bedrock AgentCore

Provides infrastructure for creating, deploying, and managing AI agents within Amazon Bedrock.

### Amazon Q

Generative AI assistant for developers, administrators, business users, and cloud operations teams.

### Amazon Comprehend Medical

Healthcare-focused NLP service that extracts medical insights from clinical text.

### Amazon Lex

Build conversational chatbots and virtual assistants using automatic speech recognition and NLP.

### Amazon Bio Discovery

AI-powered service supporting biological research, drug discovery, and scientific analysis.

### AWS HealthImaging

Cloud-native medical imaging service optimized for storing and analyzing healthcare imaging data.

---

# Analytics

### Amazon Athena

Serverless interactive query service that allows users to analyze data stored in Amazon S3 using standard SQL.

### Amazon Redshift

Fully managed cloud data warehouse designed for large-scale analytics, business intelligence, and reporting.

### Amazon CloudSearch

Managed search service that adds search functionality to websites and applications.

### Amazon OpenSearch Service

Managed search and analytics service used for log analytics, application monitoring, security analytics, and full-text search.

### Amazon Kinesis

Real-time data streaming platform for collecting, processing, and analyzing streaming data at scale.

### Amazon QuickSight

Business intelligence service that creates dashboards, reports, and visualizations from multiple data sources.

### AWS Data Exchange

Provides access to third-party datasets that can be integrated into analytics and business applications.

### AWS Lake Formation

Simplifies building, securing, and managing data lakes on AWS.

### Amazon MSK (Managed Streaming for Apache Kafka)

Fully managed Apache Kafka service used for event streaming and real-time data pipelines.

### AWS Glue DataBrew

Visual data preparation service that helps clean and transform datasets without writing code.

### Amazon FinSpace

Analytics platform designed for financial services organizations to process and analyze financial datasets.

### Managed Apache Flink

Managed stream processing service for building real-time analytics applications using Apache Flink.

### Amazon EMR

Managed big data platform for running Hadoop, Spark, Hive, HBase, and other analytics frameworks.

### AWS Clean Rooms

Privacy-focused analytics service that enables organizations to collaborate on data without sharing raw datasets.

### Amazon SageMaker

Machine learning platform that can also be used for advanced analytics and predictive modeling.

### AWS Entity Resolution

Matches and links related records across datasets to create unified customer or business profiles.

### AWS Glue

Serverless ETL service used for discovering, preparing, moving, and transforming data.

### Amazon Data Firehose

Fully managed service that delivers streaming data into AWS storage, analytics, and observability services.

### Amazon DataZone

Data governance and catalog service that simplifies data discovery, sharing, and access management.

### Amazon Quick

AI-powered business productivity and data interaction service integrated with AWS analytics capabilities.

---

# Security, Identity & Compliance

### AWS Resource Access Manager (RAM)

Allows secure sharing of AWS resources across multiple AWS accounts and organizations.

### Amazon Cognito

Provides user authentication, authorization, and identity management for web and mobile applications.

### AWS Secrets Manager

Securely stores, rotates, and manages database credentials, API keys, tokens, and secrets.

### Amazon GuardDuty

Threat detection service that continuously monitors AWS accounts, workloads, and data for malicious activity.

### Amazon Inspector

Automated vulnerability assessment service for EC2 instances, containers, and application workloads.

### Amazon Macie

Data security service that discovers, classifies, and protects sensitive data stored in Amazon S3.

### AWS IAM Identity Center

Centralized identity management service providing Single Sign-On (SSO) access to AWS accounts and applications.

### AWS Certificate Manager (ACM)

Provisions, manages, and renews SSL/TLS certificates for AWS services and applications.

### AWS Key Management Service (KMS)

Managed encryption service used to create, manage, and control cryptographic keys.

### AWS CloudHSM

Dedicated hardware security modules that provide secure key storage and cryptographic operations.

### AWS Directory Service

Managed Microsoft Active Directory and LDAP-compatible directory services for enterprise applications.

### AWS Firewall Manager

Centralized firewall management service that applies security policies across AWS accounts.

### AWS Artifact

Provides access to AWS compliance reports, certifications, and security documentation.

### Amazon Detective

Security investigation service that helps analyze and identify the root cause of suspicious activities.

### AWS Signer

Code-signing service that ensures software authenticity and integrity before deployment.

### Amazon Security Lake

Centralized security data lake that aggregates logs and security findings from multiple sources.

### AWS Security Agent

AI-powered security assistant that helps automate security operations and investigations.

### Amazon Verified Permissions

Fine-grained authorization service used to manage application permissions and access control.

### AWS Audit Manager

Automates evidence collection and compliance assessments for audits and regulatory requirements.

### AWS Security Hub CSPM

Cloud Security Posture Management solution that continuously evaluates security best practices.

### AWS IAM

Identity and Access Management service used to securely control access to AWS resources.

### AWS WAF

Web Application Firewall that protects applications against common web exploits and attacks.

### AWS Shield

Managed DDoS protection service that safeguards applications from distributed denial-of-service attacks.

### AWS Security Hub

Centralized security management service that aggregates findings from AWS security services.

### AWS Private Certificate Authority

Managed private CA service used to issue and manage internal SSL/TLS certificates.

### AWS Payment Cryptography

Managed cryptographic service designed for payment processing and financial applications.

### AWS Security Incident Response

Managed incident response service that helps organizations detect, investigate, and recover from security incidents.

---

# Cloud Financial Management

### AWS Marketplace

Digital catalog where customers can discover, purchase, and deploy third-party software and services.

### AWS Billing Conductor

Custom billing service that creates billing groups and pricing models for organizations.

### AWS FinOps Agent

AI-powered assistant that helps monitor, optimize, and manage cloud spending.

### Billing and Cost Management

Provides billing dashboards, cost reports, budgets, forecasts, and spending analysis across AWS environments.

---

# Application Integration

### AWS Step Functions

Serverless workflow orchestration service that coordinates multiple AWS services into automated business processes and application workflows.

### Amazon AppFlow

Fully managed integration service that securely transfers data between AWS services and SaaS applications.

### Amazon MQ

Managed message broker service supporting Apache ActiveMQ and RabbitMQ for enterprise messaging workloads.

### Amazon Simple Notification Service (SNS)

Pub/Sub messaging service used for notifications, alerts, event distribution, and application communication.

### Amazon Simple Queue Service (SQS)

Fully managed message queue service that decouples applications and enables reliable asynchronous processing.

### Amazon Simple Workflow Service (SWF)

Workflow coordination service for distributed applications requiring task tracking and state management.

### Amazon Managed Workflows for Apache Airflow (MWAA)

Managed Apache Airflow service used to build, schedule, and monitor complex data pipelines.

### AWS B2B Data Interchange

Managed service that automates Electronic Data Interchange (EDI) transactions with business partners.

### Amazon EventBridge

Serverless event bus that enables event-driven architectures and application integrations.

---

# Front-End Web & Mobile

### AWS Amplify

Full-stack development platform for building, deploying, and hosting modern web and mobile applications.

### AWS AppSync

Managed GraphQL service that simplifies real-time application development and API management.

### AWS Device Farm

Application testing service that enables testing on real Android and iOS devices at scale.

### Amazon Location Service

Provides maps, geocoding, routing, tracking, and location-based capabilities for applications.

---

# Business Applications

### Amazon Connect Customer

Cloud-based contact center service used to build customer support and engagement solutions.

### Amazon Chime

Business communication platform supporting video conferencing, chat, meetings, and collaboration.

### Amazon Simple Email Service (SES)

Scalable email service used for transactional emails, notifications, and marketing campaigns.

### Amazon WorkDocs

Secure document storage, sharing, and collaboration service for enterprise teams.

### Amazon WorkMail

Managed business email and calendar service integrated with desktop and mobile clients.

### Amazon Connect Health

Healthcare-focused customer engagement solution built on Amazon Connect capabilities.

### Amazon Connect Decisions

Provides intelligent routing and decision-making capabilities for customer service interactions.

### Amazon Pinpoint

Customer engagement service used for targeted messaging, analytics, campaigns, and notifications.

### AWS Wickr

Secure messaging and collaboration platform designed for sensitive communications.

### AWS AppFabric

Integrates SaaS applications to improve security visibility and operational efficiency.

### AWS End User Messaging

Service for sending SMS, push notifications, and customer communications at scale.

### Amazon Chime SDK

Developer toolkit for embedding voice, video, messaging, and collaboration features into applications.

---

# End User Computing

### Amazon WorkSpaces

Fully managed Desktop-as-a-Service (DaaS) solution that provides secure cloud desktops.

### Amazon WorkSpaces Applications

Application streaming service that delivers desktop applications without full virtual desktops.

### Amazon WorkSpaces Thin Client

Managed thin-client solution optimized for accessing virtual desktops and cloud applications.

### Amazon WorkSpaces Secure Browser

Secure browser service that isolates web access and protects corporate data.

---

# Internet of Things (IoT)

### AWS IoT Device Defender

Security service that audits, monitors, and protects IoT device fleets from threats.

### AWS IoT Device Management

Provides onboarding, organization, monitoring, and lifecycle management for IoT devices.

### AWS IoT Greengrass

Extends AWS capabilities to edge devices, enabling local processing and offline operation.

### AWS IoT SiteWise

Collects, organizes, and analyzes industrial equipment and operational data.

### AWS IoT Core

Managed platform that securely connects, manages, and processes data from IoT devices.

### AWS IoT TwinMaker

Creates digital twins of real-world systems by combining data from multiple sources.

### AWS IoT Events

Detects and responds to events from IoT sensors and industrial equipment.

### AWS IoT FleetWise

Collects, transforms, and transfers vehicle telemetry data for automotive analytics.

---

# Game Development

### Amazon GameLift Servers

Managed service for deploying, operating, and scaling multiplayer game servers globally.

### Amazon GameLift Streams

Streams games directly from AWS infrastructure to player devices without local installation.

---

# Part 6 Summary

### Categories Covered

- Application Integration
- Front-End Web & Mobile
- Business Applications
- End User Computing
- Internet of Things
- Game Development

### Services Covered

- 9 Application Integration Services
- 4 Front-End Services
- 12 Business Application Services
- 4 End User Computing Services
- 8 IoT Services
- 2 Game Development Services

**Total Services Covered: 39**

---

# 📎 Official Documentation

- AWS Documentation
- AWS Service Catalog
- AWS Well-Architected Framework
- AWS Pricing Calculator
- AWS Skill Builder
- AWS Architecture Center

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational reference created for learning, certification preparation, cloud engineering, DevOps engineering, platform engineering, site reliability engineering, solution architecture, and software development.

Primary references include:

- AWS Documentation
- AWS Architecture Center
- AWS Well-Architected Framework
- AWS Service User Guides
- AWS Service FAQs

Amazon Web Services (AWS) and all AWS service names are trademarks of Amazon Web Services, Inc.

Always refer to official AWS documentation for the latest service features, limits, availability, and pricing information.

---

# 🎉 Conclusion

AWS provides one of the most comprehensive cloud platforms in the world, offering services across compute, storage, databases, networking, security, analytics, machine learning, DevOps, application integration, IoT, media, and enterprise workloads.

Understanding AWS services by category helps cloud engineers, DevOps engineers, platform engineers, architects, and developers select the right service for the right use case while building secure, scalable, resilient, and cost-effective cloud solutions.

---

```text
Last Updated: 2026
Platform: Amazon Web Services (AWS)
Coverage: AWS Service Portfolio
Level: Beginner to Advanced
Use Cases: Cloud, DevOps, SRE, Architecture, Certifications
```