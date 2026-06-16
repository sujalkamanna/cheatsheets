# 📨 Apache Kafka Cheatsheet

![Apache Kafka Logo](https://kafka.apache.org/images/apache-kafka.png)

---

# Table of Contents

1. [What is Apache Kafka?](#1-what-is-apache-kafka)
2. [Why Kafka?](#2-why-kafka)
3. [Messaging Systems](#3-messaging-systems)
4. [Kafka Architecture](#4-kafka-architecture)
5. [Core Components](#5-core-components)
6. [Producers](#6-producers)
7. [Consumers](#7-consumers)
8. [Topics](#8-topics)
9. [Partitions](#9-partitions)
10. [Offsets](#10-offsets)
11. [Brokers](#11-brokers)
12. [Clusters](#12-clusters)
13. [Events](#13-events)
14. [Records](#14-records)
15. [Message Flow](#15-message-flow)
16. [Kafka Use Cases](#16-kafka-use-cases)
17. [Installation](#17-installation)
18. [Kafka CLI](#18-kafka-cli)
19. [Kafka UI Tools](#19-kafka-ui-tools)
20. [Basic Architecture](#20-basic-architecture)

---

# 1. What is Apache Kafka?

Apache Kafka is a distributed event streaming and messaging platform used for building real-time data pipelines and event-driven applications.

---

## Key Characteristics

```text id="x7m4wp"
Distributed
+
Fault Tolerant
+
High Throughput
+
Scalable
+
Real Time
```

---

## Kafka Acts As

```text id="m5v8xt"
Message Broker
+
Event Streaming Platform
+
Data Integration Platform
```

---

## Common Use Cases

```text id="r8v2qt"
Log Collection
+
Event Streaming
+
Microservices
+
Analytics Pipelines
+
IoT Systems
```

---

## Core Idea

```text id="p4m7wr"
Produce Events

Store Events

Consume Events
```

---

# 2. Why Kafka?

Traditional messaging systems often struggle with scale and real-time processing.

Kafka solves these challenges.

---

## Benefits

```text id="x6v3qt"
High Throughput
+
Low Latency
+
Durable Storage
+
Horizontal Scaling
```

---

## Why Organizations Use Kafka

```text id="m8v4xp"
Event Driven Architecture
+
Microservices Integration
+
Real Time Analytics
```

---

## Advantages

```text id="r7m2wt"
Replication
+
Fault Tolerance
+
Distributed Design
```

---

## Kafka Enables

```text id="n5v9wr"
Data In Motion
```

instead of traditional batch processing.

---

# 3. Messaging Systems

Kafka belongs to the messaging and event streaming category.

---

## Traditional Messaging

```text id="q4m8xt"
Producer
     |
     v
Queue
     |
     v
Consumer
```

---

## Kafka Messaging

```text id="x8m2wp"
Producer
     |
     v
Topic
     |
     v
Consumer
```

---

## Messaging Goals

```text id="r5v7qt"
Reliable Delivery
+
Decoupled Systems
```

---

## Examples

```text id="m4v8wr"
Kafka
+
RabbitMQ
+
ActiveMQ
```

---

# 4. Kafka Architecture

Kafka is a distributed system composed of multiple brokers.

---

## Architecture

```text id="p7m3xt"
Producer
     |
     v
Kafka Cluster
     |
     v
Consumer
```

---

## Components

```text id="n3v8wp"
Producer
+
Broker
+
Topic
+
Partition
+
Consumer
```

---

## Data Flow

```text id="x5m9qt"
Events
     |
     v
Kafka
     |
     v
Applications
```

---

## Benefits

```text id="r8v4wr"
Scalability
+
Reliability
```

---

# 5. Core Components

Kafka consists of several core building blocks.

---

## Components

```text id="m2v7xt"
Producer
+
Consumer
+
Broker
+
Topic
+
Partition
```

---

## Relationship

```text id="q6m8wp"
Producer
     |
     v
Topic
     |
     v
Consumer
```

---

## Goal

```text id="n7m4qt"
Reliable Event Streaming
```

---

## Foundation

```text id="x4v8wr"
Topics
+
Partitions
+
Offsets
```

---

# 6. Producers

Producers send messages to Kafka topics.

---

## Workflow

```text id="m8v3xt"
Application
      |
      v
Producer
      |
      v
Kafka Topic
```

---

## Example

```text id="q5m7wp"
Order Created Event
```

sent to Kafka.

---

## Responsibilities

```text id="r9v2qt"
Create Events
+
Send Events
```

---

## Benefits

```text id="p4m8wr"
Asynchronous Communication
```

---

# 7. Consumers

Consumers read messages from Kafka topics.

---

## Workflow

```text id="x7m4qt"
Kafka Topic
      |
      v
Consumer
      |
      v
Application
```

---

## Example

```text id="n5v8xp"
Order Processing Service
```

consumes events.

---

## Responsibilities

```text id="r3m9wp"
Read Events
+
Process Events
```

---

## Benefits

```text id="m4v8qt"
Decoupled Applications
```

---

# 8. Topics

Topics are logical channels used to store events.

---

## Example Topics

```text id="q8m2wr"
orders

payments

shipments

users
```

---

## Purpose

```text id="x6v4wp"
Organize Events
```

---

## Workflow

```text id="r5m9xt"
Producer
      |
      v
Topic
      |
      v
Consumer
```

---

## Best Practice

```text id="n7v3wr"
One Business Domain

Per Topic
```

---

# 9. Partitions

Partitions divide a topic into multiple segments.

Partitions enable scalability and parallelism.

---

## Example

```text id="x4m8wr"
orders
   |
   +--- Partition 0

   +--- Partition 1

   +--- Partition 2
```

---

## Benefits

```text id="m8v4qt"
Parallel Processing
+
Scalability
```

---

## Important Concept

```text id="q4m8wr"
Ordering

Guaranteed

Within A Partition
```

---

## Why Partitions Matter

```text id="r8v2qt"
Kafka Scalability
Depends On Partitions
```

---

# 10. Offsets

Offsets uniquely identify messages within partitions.

---

## Example

```text id="x6m4wp"
Partition 0

Offset 0

Offset 1

Offset 2

Offset 3
```

---

## Purpose

```text id="n5v9xt"
Track Consumer Progress
```

---

## Benefits

```text id="p7m3wr"
Replay Messages
+
Resume Processing
```

---

## Important

```text id="m4v8qt"
Offsets Are

Per Partition
```

---

# 11. Brokers

A Broker is a Kafka server.

---

## Responsibilities

```text id="q5v7xt"
Store Messages
+
Serve Consumers
+
Handle Replication
```

---

## Example

```text id="r4m9wp"
Broker 1

Broker 2

Broker 3
```

---

## Benefits

```text id="n7m4wr"
Distributed Storage
```

---

## Cluster Building Block

```text id="x4m8wr"
Broker
```

---

# 12. Clusters

A Kafka Cluster consists of multiple brokers.

---

## Architecture

```text id="q6v3wp"
Broker 1

Broker 2

Broker 3
```

---

## Purpose

```text id="r5v8qt"
Scalability
+
Fault Tolerance
```

---

## Benefits

```text id="n8m4xp"
High Availability
```

---

## Production Recommendation

```text id="m3v7wr"
Minimum

3 Brokers
```

---

# 13. Events

Events represent something that happened.

Everything in Kafka revolves around events.

---

## Examples

```text id="x7m2qt"
Order Created
+
Payment Completed
+
User Registered
```

---

## Structure

```text id="p8m4wr"
Event
   |
   +--- Key

   +--- Value

   +--- Timestamp
```

---

## Purpose

```text id="r7v3xt"
Capture Business Activity
```

---

# 14. Records

A Record is the actual message stored in Kafka.

---

## Structure

```text id="n4m8wp"
Key
+
Value
+
Timestamp
+
Headers
```

---

## Example

```json id="x5v7qt"
{
  "orderId": 1001,
  "status": "created"
}
```

---

## Storage Location

```text id="m8v2wr"
Topic
      |
      v
Partition
      |
      v
Record
```

---

## Purpose

```text id="r5v8wp"
Store Events
```

---

# 15. Message Flow

Kafka follows a producer-topic-consumer model.

---

## Flow

```text id="n7m4xt"
Producer
     |
     v
Topic
     |
     v
Consumer
```

---

## Real Example

```text id="p4m9wr"
E-Commerce App
      |
      v
Order Event
      |
      v
Kafka
      |
      v
Inventory Service
```

---

## Benefits

```text id="x8m3qt"
Loose Coupling
+
Scalability
```

---

# 16. Kafka Use Cases

Kafka is widely used in modern architectures.

---

## Common Use Cases

```text id="m5v8wr"
Microservices
+
Event Streaming
+
Log Aggregation
+
Data Pipelines
+
Analytics
```

---

## Enterprise Use Cases

```text id="r4v9xp"
Fraud Detection
+
Monitoring
+
Real Time Processing
```

---

## Benefits

```text id="n6m4qt"
Real Time Data Movement
```

---

# 17. Installation

Kafka can run on servers, containers, Kubernetes, and cloud platforms.

---

## Deployment Options

```text id="p7v3wr"
Linux
+
Docker
+
Kubernetes
+
Cloud
```

---

## Docker Example

```bash id="x5m8qt"
docker run -d \
  --name kafka \
  apache/kafka
```

---

## Components

```text id="r8m2wp"
Kafka
+
KRaft
```

---

## Recommendation

```text id="n4v8xt"
Use KRaft Mode
For New Deployments
```

---

# 18. Kafka CLI

Kafka provides command-line tools.

---

## Common Commands

```text id="m7v3qt"
Create Topics
+
List Topics
+
Produce Messages
+
Consume Messages
```

---

## List Topics

```bash id="q8m4wr"
kafka-topics.sh \
--list \
--bootstrap-server localhost:9092
```

---

## Benefits

```text id="r5v9xt"
Administration
+
Troubleshooting
```

---

# 19. Kafka UI Tools

UI tools simplify Kafka management.

---

## Popular Tools

```text id="n7v2wp"
AKHQ
+
Kafdrop
+
Conduktor
+
Provectus UI
```

---

## Features

```text id="x4m8qt"
Topic Management
+
Consumer Monitoring
+
Message Inspection
```

---

## Benefits

```text id="m5v7wr"
Operational Visibility
```

---

# 20. Basic Architecture

Kafka enables scalable event streaming.

---

## Architecture

```text id="p8m4xt"
Producer
     |
     v
Kafka Cluster
     |
     v
Consumer
```

---

## Internal Flow

```text id="r6m9wp"
Producer
     |
     v
Topic
     |
     v
Partition
     |
     v
Consumer
```

---

## Core Building Blocks

```text id="x7m2qt"
Topics
+
Partitions
+
Offsets
+
Brokers
+
Consumers
```

---

## Kafka Foundation

```text id="n5v8wr"
Events
+
Topics
+
Partitions
+
Offsets
```

---

# 21. Producer Architecture

Producers are responsible for publishing events to Kafka topics.

---

## Architecture

```text id="x7m4wp"
Application
      |
      v
Producer
      |
      v
Kafka Topic
```

---

## Responsibilities

```text id="m5v8xt"
Create Events
+
Serialize Data
+
Send Messages
```

---

## Workflow

```text id="r8v2qt"
Generate Event
      |
      v
Producer
      |
      v
Partition
```

---

## Goal

```text id="p4m7wr"
Reliable Event Delivery
```

---

# 22. Producer Workflow

Kafka producers follow a predictable workflow.

---

## Flow

```text id="x6v3qt"
Create Event
      |
      v
Select Partition
      |
      v
Send To Broker
      |
      v
Receive ACK
```

---

## Example

```text id="m8v4xp"
Order Created
      |
      v
Producer
      |
      v
orders Topic
```

---

## Benefits

```text id="r7m2wt"
Fast
+
Asynchronous
```

---

# 23. Producer Acknowledgments

ACKs determine how producers verify successful delivery.

---

## ACK Modes

```text id="n5v9wr"
acks=0
acks=1
acks=all
```

---

## acks=0

```text id="q4m8xt"
No Confirmation
```

---

## acks=1

```text id="x8m2wp"
Leader Confirmation
```

---

## acks=all

```text id="r5v7qt"
Leader
+
All Replicas
Confirm
```

---

## Recommended

```text id="m4v8wr"
acks=all
```

for production.

---

# 24. Consumers

Consumers read events from Kafka topics.

---

## Workflow

```text id="p7m3xt"
Topic
   |
   v
Consumer
   |
   v
Application
```

---

## Responsibilities

```text id="n3v8wp"
Read Events
+
Process Events
```

---

## Example

```text id="x5m9qt"
Order Service
```

consumes order events.

---

## Benefits

```text id="r8v4wr"
Loose Coupling
```

---

# 25. Consumer Groups

Consumer Groups allow multiple consumers to share workload.

---

## Architecture

```text id="m2v7xt"
Topic
  |
  +---- Consumer A

  +---- Consumer B

  +---- Consumer C
```

---

## Purpose

```text id="q6m8wp"
Horizontal Scaling
```

---

## Rule

```text id="n7m4qt"
One Partition

Can Be Read By

Only One Consumer

Within A Group
```

---

## Benefits

```text id="x4v8wr"
Parallel Processing
```

---

# 26. Consumer Rebalancing

Rebalancing redistributes partitions among consumers.

---

## Triggered When

```text id="m8v3xt"
Consumer Added
+
Consumer Removed
+
Consumer Failure
```

---

## Example

Before

```text id="q5m7wp"
Consumer A

Partition 0
Partition 1
```

---

After New Consumer

```text id="r9v2qt"
Consumer A

Partition 0
```

```text id="p4m8wr"
Consumer B

Partition 1
```

---

## Goal

```text id="x7m4qt"
Balanced Workload
```

---

# 27. Offset Management

Offsets track consumer progress.

---

## Example

```text id="n5v8xp"
Partition 0

Offset 0
Offset 1
Offset 2
Offset 3
```

---

## Consumer State

```text id="r3m9wp"
Last Read Offset
```

---

## Benefits

```text id="m4v8qt"
Resume Processing
+
Replay Messages
```

---

## Importance

```text id="q8m2wr"
Critical For Reliability
```

---

# 28. Auto Commit

Kafka automatically commits offsets.

---

## Configuration

```properties id="x6v4wp"
enable.auto.commit=true
```

---

## Workflow

```text id="r5m9xt"
Read Message
      |
      v
Auto Commit Offset
```

---

## Advantage

```text id="n7v3wr"
Simple Configuration
```

---

## Drawback

```text id="x4m8wr"
Potential Message Loss
```

---

# 29. Manual Commit

Consumers explicitly commit offsets.

---

## Workflow

```text id="m8v4qt"
Read Message
      |
      v
Process Message
      |
      v
Commit Offset
```

---

## Benefits

```text id="q4m8wr"
Greater Reliability
```

---

## Common Usage

```text id="r8v2qt"
Production Systems
```

---

## Recommendation

```text id="x6m4wp"
Manual Commit
```

for critical workloads.

---

# 30. Delivery Semantics

Delivery semantics define message processing guarantees.

---

## Types

```text id="n5v9xt"
At Most Once
+
At Least Once
+
Exactly Once
```

---

## Goal

```text id="p7m3wr"
Reliable Processing
```

---

## Importance

```text id="m4v8qt"
Data Consistency
```

---

# 31. At Most Once

Message may be lost but never duplicated.

---

## Workflow

```text id="q5v7xt"
Commit Offset
      |
      v
Process Message
```

---

## Risk

```text id="r4m9wp"
Message Loss
```

---

## Benefit

```text id="n7m4wr"
Highest Performance
```

---

## Use Cases

```text id="x4m8wr"
Metrics
+
Logs
```

---

# 32. At Least Once

Message is never lost but may be duplicated.

---

## Workflow

```text id="q6v3wp"
Process Message
      |
      v
Commit Offset
```

---

## Benefits

```text id="r5v8qt"
Reliable Delivery
```

---

## Risk

```text id="n8m4xp"
Duplicate Processing
```

---

## Most Common

```text id="m3v7wr"
At Least Once
```

---

# 33. Exactly Once

Kafka guarantees no duplicates and no loss.

---

## Benefits

```text id="x7m2qt"
Highest Reliability
```

---

## Requirements

```text id="p8m4wr"
Idempotent Producer
+
Transactions
```

---

## Use Cases

```text id="r7v3xt"
Banking
+
Payments
```

---

## Tradeoff

```text id="n4m8wp"
Higher Complexity
```

---

# 34. Retention Policies

Kafka retains messages for a configurable period.

---

## Example

```text id="x5v7qt"
7 Days
30 Days
90 Days
```

---

## Purpose

```text id="m8v2wr"
Replay Events
```

---

## Configuration

```properties id="r5v8wp"
retention.ms=604800000
```

---

## Benefits

```text id="n7m4xt"
Data Recovery
```

---

# 35. Log Compaction

Log Compaction retains only the latest value per key.

---

## Example

```text id="p4m9wr"
user123=v1

user123=v2

user123=v3
```

---

After Compaction

```text id="x8m3qt"
user123=v3
```

---

## Benefits

```text id="m5v8wr"
Reduced Storage
```

---

## Common Usage

```text id="r4v9xp"
State Stores
+
CDC Systems
```

---

# 36. Dead Letter Topics (DLT)

Dead Letter Topics store failed messages.

---

## Workflow

```text id="n6m4qt"
Message Fails
      |
      v
Dead Letter Topic
```

---

## Benefits

```text id="p7v3wr"
Failure Isolation
```

---

## Common Usage

```text id="x5m8qt"
Error Handling
```

---

## Example

```text id="r8m2wp"
orders-dlt
```

---

# 37. Message Ordering

Kafka guarantees ordering within a partition.

---

## Guaranteed

```text id="n4v8xt"
Partition 0

1
2
3
4
5
```

---

## Not Guaranteed

```text id="m7v3qt"
Across Multiple Partitions
```

---

## Important Rule

```text id="q8m4wr"
Ordering

Per Partition
```

---

## Design Tip

```text id="r5v9xt"
Use Same Key
For Related Events
```

---

# 38. Partition Strategies

Partitioning determines event placement.

---

## Round Robin

```text id="n7v2wp"
Even Distribution
```

---

## Key Based

```text id="x4m8qt"
Same Key

Same Partition
```

---

## Custom Partitioner

```text id="m5v7wr"
Application Logic
```

---

## Goal

```text id="p8m4xt"
Balanced Load
```

---

# 39. Performance Tuning

Kafka performance depends heavily on configuration.

---

## Important Areas

```text id="r6m9wp"
Partitions
+
Batching
+
Compression
+
Replication
```

---

## Producer Tuning

```text id="x7m2qt"
batch.size
+
linger.ms
+
compression.type
```

---

## Consumer Tuning

```text id="n5v8wr"
fetch.min.bytes
+
max.poll.records
```

---

## Goal

```text id="m4v7xt"
High Throughput
+
Low Latency
```

---

# 40. Best Practices

Production Kafka requires careful design.

---

## Recommendations

```text id="q6m8wp"
Use Consumer Groups
+
Enable Replication
+
Use Monitoring
+
Use TLS
```

---

## Topic Design

```text id="r8v4xt"
Meaningful Names
+
Proper Partition Count
```

---

## Reliability

```text id="x5m9wr"
acks=all
+
Replication Factor >= 3
```

---

## Summary

```text id="n7v3wr"
Producers
+
Consumers
+
Offsets
+
Consumer Groups
+
Delivery Guarantees
+
Retention
```

---

# 41. Kafka Storage Model

Kafka stores messages as an append-only log.

Messages are written sequentially to disk.

---

## Architecture

```text id="x7m4wp"
Topic
   |
   v
Partition
   |
   v
Log File
```

---

## Characteristics

```text id="m5v8xt"
Append Only
+
Immutable Messages
+
Sequential Writes
```

---

## Benefits

```text id="r8v2qt"
High Throughput
+
Fast Disk Access
```

---

## Core Principle

```text id="p4m7wr"
Write Once

Read Many Times
```

---

# 42. Commit Logs

Kafka topics are implemented as commit logs.

---

## Structure

```text id="x6v3qt"
Offset 0
Offset 1
Offset 2
Offset 3
```

---

## Characteristics

```text id="m8v4xp"
Ordered
+
Persistent
+
Durable
```

---

## Example

```text id="r7m2wt"
Order Created

Order Paid

Order Shipped
```

---

## Benefits

```text id="n5v9wr"
Event Replay
+
Audit Trail
```

---

# 43. Segment Files

Partitions are divided into segment files.

---

## Example

```text id="q4m8xt"
Partition 0

segment-1.log

segment-2.log

segment-3.log
```

---

## Why Segments?

```text id="x8m2wp"
Efficient Storage
+
Retention Management
```

---

## Benefits

```text id="r5v7qt"
Easy Cleanup
+
Fast Access
```

---

## Kafka Internals

```text id="m4v8wr"
Partition

=

Multiple Segments
```

---

# 44. Replication

Replication protects Kafka against broker failures.

---

## Architecture

```text id="p7m3xt"
Leader
   |
   +---- Replica

   +---- Replica
```

---

## Purpose

```text id="n3v8wp"
Fault Tolerance
```

---

## Benefits

```text id="x5m9qt"
High Availability
+
Data Durability
```

---

## Production Recommendation

```text id="r8v4wr"
Replication Factor

>= 3
```

---

# 45. ISR (In-Sync Replicas)

ISR is the set of replicas synchronized with the leader.

---

## Example

```text id="m2v7xt"
Leader
+
Replica A
+
Replica B
```

---

## Purpose

```text id="q6m8wp"
Reliable Replication
```

---

## Kafka Writes

```text id="n7m4qt"
Leader

Waits For ISR
```

when using:

```text id="x4v8wr"
acks=all
```

---

## Benefits

```text id="m8v3xt"
Stronger Durability
```

---

# 46. Leaders

Each partition has one leader.

All reads and writes go through the leader.

---

## Architecture

```text id="q5m7wp"
Partition
      |
      v
Leader
```

---

## Responsibilities

```text id="r9v2qt"
Handle Reads
+
Handle Writes
```

---

## Example

```text id="p4m8wr"
Partition 0

Leader = Broker 1
```

---

## Importance

```text id="x7m4qt"
Core Kafka Component
```

---

# 47. Followers

Followers replicate data from leaders.

---

## Workflow

```text id="n5v8xp"
Leader
    |
    v
Follower
```

---

## Responsibilities

```text id="r3m9wp"
Replicate Data
+
Support Failover
```

---

## Example

```text id="m4v8qt"
Broker 2

Follower
```

---

## Benefits

```text id="q8m2wr"
Redundancy
```

---

# 48. Leader Election

Leader Election selects a new leader if the current leader fails.

---

## Failure Scenario

```text id="x6v4wp"
Leader Down
```

---

## Workflow

```text id="r5m9xt"
Leader Failure
      |
      v
New Leader Selected
      |
      v
Service Continues
```

---

## Benefits

```text id="n7v3wr"
High Availability
```

---

## Goal

```text id="x4m8wr"
Minimal Downtime
```

---

# 49. High Availability

Kafka is designed for continuous operation.

---

## Components

```text id="m8v4qt"
Replication
+
ISR
+
Leader Election
```

---

## Architecture

```text id="q4m8wr"
Broker Failure
      |
      v
Replica Promotion
```

---

## Benefits

```text id="r8v2qt"
Resilience
+
Reliability
```

---

## Production Goal

```text id="x6m4wp"
No Single Point Of Failure
```

---

# 50. Fault Tolerance

Kafka tolerates broker failures without data loss.

---

## Enabled By

```text id="n5v9xt"
Replication
+
Leader Election
+
ISR
```

---

## Failure Example

```text id="p7m3wr"
Broker Down

↓

Cluster Continues Running
```

---

## Benefits

```text id="m4v8qt"
Reliability
```

---

## Core Principle

```text id="q5v7xt"
Expect Failures
```

---

# 51. ZooKeeper

Historically Kafka relied on ZooKeeper.

---

## Responsibilities

```text id="r4m9wp"
Cluster Metadata
+
Leader Election
+
Configuration Management
```

---

## Architecture

```text id="n7m4wr"
Kafka
   |
   v
ZooKeeper
```

---

## Limitations

```text id="x4m8wr"
Additional Complexity
```

---

## Modern Status

```text id="q6v3wp"
Being Replaced By KRaft
```

---

# 52. KRaft Mode

KRaft removes ZooKeeper from Kafka.

KRaft = Kafka Raft Metadata Mode

---

## Architecture

```text id="r5v8qt"
Kafka
   |
   v
Built-In Metadata Management
```

---

## Benefits

```text id="n8m4xp"
Simpler Architecture
+
Better Scalability
+
Fewer Components
```

---

## Modern Recommendation

```text id="m3v7wr"
Use KRaft
For New Clusters
```

---

## Future

```text id="x7m2qt"
ZooKeeper Free Kafka
```

---

# 53. Metadata Management

Kafka stores metadata about the cluster.

---

## Metadata Includes

```text id="p8m4wr"
Topics
+
Partitions
+
Brokers
+
Consumer Groups
```

---

## Managed By

```text id="r7v3xt"
KRaft Controller
```

---

## Purpose

```text id="n4m8wp"
Cluster Coordination
```

---

## Importance

```text id="x5v7qt"
Essential For Operations
```

---

# 54. Controller Node

The Controller manages cluster-wide decisions.

---

## Responsibilities

```text id="m8v2wr"
Leader Elections
+
Metadata Updates
+
Broker Coordination
```

---

## Architecture

```text id="r5v8wp"
Controller
      |
      v
Kafka Cluster
```

---

## Benefits

```text id="n7m4xt"
Centralized Coordination
```

---

## KRaft Role

```text id="p4m9wr"
Controller Quorum
```

---

# 55. Rack Awareness

Rack Awareness distributes replicas across failure domains.

---

## Without Rack Awareness

```text id="x8m3qt"
Rack A

Broker 1
Broker 2
Broker 3
```

---

## Risk

```text id="m5v8wr"
Single Rack Failure
=
Data Loss Risk
```

---

## With Rack Awareness

```text id="r4v9xp"
Rack A -> Broker 1

Rack B -> Broker 2

Rack C -> Broker 3
```

---

## Benefits

```text id="n6m4qt"
Better Availability
+
Disaster Resilience
```

---

# 56. Kafka Internals Summary

Kafka achieves reliability through distributed architecture.

---

## Storage Layer

```text id="p7v3wr"
Commit Logs
+
Segments
+
Partitions
```

---

## Reliability Layer

```text id="x5m8qt"
Replication
+
ISR
+
Leader Election
```

---

## Modern Architecture

```text id="r8m2wp"
KRaft
+
Controller
+
Metadata Management
```

---

## Core Kafka Internals

```text id="n4v8xt"
Leaders
+
Followers
+
Replication
+
KRaft
+
Fault Tolerance
```

---

# 57. Kafka Connect

Kafka Connect is a framework for moving data between Kafka and external systems.

It eliminates the need to write custom integration code.

---

## Purpose

```text id="x7m4wp"
Data Integration
+
Data Movement
+
Automation
```

---

## Architecture

```text id="m5v8xt"
Source System
      |
      v
Source Connector
      |
      v
Kafka
      |
      v
Sink Connector
      |
      v
Target System
```

---

## Benefits

```text id="r8v2qt"
Scalable
+
Fault Tolerant
+
Easy Integration
```

---

## Common Systems

```text id="p4m7wr"
Databases
+
S3
+
Elasticsearch
+
JDBC
```

---

# 58. Source Connectors

Source Connectors import data into Kafka.

---

## Workflow

```text id="x6v3qt"
Database
     |
     v
Source Connector
     |
     v
Kafka Topic
```

---

## Examples

```text id="m8v4xp"
MySQL
+
PostgreSQL
+
MongoDB
+
Oracle
```

---

## Benefits

```text id="r7m2wt"
Automated Data Ingestion
```

---

## Common Use Cases

```text id="n5v9wr"
CDC
+
Analytics
+
Data Pipelines
```

---

# 59. Sink Connectors

Sink Connectors export Kafka data to external systems.

---

## Workflow

```text id="q4m8xt"
Kafka Topic
      |
      v
Sink Connector
      |
      v
Target System
```

---

## Examples

```text id="x8m2wp"
Elasticsearch
+
S3
+
Snowflake
+
Databases
```

---

## Benefits

```text id="r5v7qt"
Automated Data Delivery
```

---

## Use Cases

```text id="m4v8wr"
Reporting
+
Analytics
+
Archiving
```

---

# 60. Kafka Streams

Kafka Streams is Kafka's stream processing library.

---

## Purpose

```text id="p7m3xt"
Process Data
In Real Time
```

---

## Architecture

```text id="n3v8wp"
Kafka Topic
      |
      v
Kafka Streams
      |
      v
Output Topic
```

---

## Operations

```text id="x5m9qt"
Filter
+
Transform
+
Aggregate
+
Join
```

---

## Benefits

```text id="r8v4wr"
Real-Time Processing
```

---

# 61. Stream Processing

Stream Processing analyzes data continuously.

---

## Traditional Processing

```text id="m2v7xt"
Data
      |
      v
Batch Job
      |
      v
Results
```

---

## Stream Processing

```text id="q6m8wp"
Event
      |
      v
Immediate Processing
      |
      v
Result
```

---

## Benefits

```text id="n7m4qt"
Low Latency
+
Real Time Analytics
```

---

## Use Cases

```text id="x4v8wr"
Fraud Detection
+
Monitoring
+
IoT
```

---

# 62. KSQLDB

KSQLDB allows stream processing using SQL.

---

## Purpose

```text id="m8v3xt"
SQL For Kafka
```

---

## Example

```sql id="q5m7wp"
SELECT *
FROM orders
EMIT CHANGES;
```

---

## Benefits

```text id="r9v2qt"
No Java Coding
+
Easy Stream Processing
```

---

## Use Cases

```text id="p4m8wr"
Real-Time Dashboards
+
Monitoring
```

---

# 63. Schema Registry

Schema Registry manages message schemas.

One of the most important Kafka ecosystem components.

---

## Purpose

```text id="x7m4qt"
Data Governance
+
Schema Validation
```

---

## Architecture

```text id="n5v8xp"
Producer
    |
    v
Schema Registry
    |
    v
Kafka
```

---

## Benefits

```text id="r3m9wp"
Data Consistency
+
Compatibility Checks
```

---

## Common Formats

```text id="m4v8qt"
Avro
+
Protobuf
+
JSON Schema
```

---

# 64. Avro

Avro is the most commonly used Kafka serialization format.

---

## Example Schema

```json id="q8m2wr"
{
  "type": "record",
  "name": "Order",
  "fields": [
    {"name":"id","type":"string"}
  ]
}
```

---

## Benefits

```text id="x6v4wp"
Compact
+
Fast
+
Schema Evolution
```

---

## Common Usage

```text id="r5m9xt"
Enterprise Kafka
```

---

# 65. Protobuf

Protocol Buffers are a Google-developed serialization format.

---

## Benefits

```text id="n7v3wr"
Small Messages
+
Fast Serialization
```

---

## Common Usage

```text id="x4m8wr"
Microservices
+
gRPC
```

---

## Advantages

```text id="m8v4qt"
Language Neutral
```

---

## Alternative To

```text id="q4m8wr"
Avro
```

---

# 66. JSON Schema

JSON Schema validates JSON messages.

---

## Example

```json id="r8v2qt"
{
  "type":"object",
  "properties":{
    "id":{
      "type":"string"
    }
  }
}
```

---

## Benefits

```text id="x6m4wp"
Human Readable
```

---

## Drawback

```text id="n5v9xt"
Larger Payloads
```

---

## Common Usage

```text id="p7m3wr"
REST APIs
```

---

# 67. MirrorMaker

MirrorMaker replicates Kafka data between clusters.

---

## Architecture

```text id="m4v8qt"
Cluster A
     |
     v
MirrorMaker
     |
     v
Cluster B
```

---

## Benefits

```text id="q5v7xt"
Disaster Recovery
+
Multi Region Replication
```

---

## Use Cases

```text id="r4m9wp"
Backup Clusters
+
Migration
```

---

## Goal

```text id="n7m4wr"
Cross Cluster Replication
```

---

# 68. Event Driven Architecture

EDA is one of Kafka's primary use cases.

---

## Architecture

```text id="x4m8wr"
Producer
     |
     v
Event
     |
     v
Consumer
```

---

## Benefits

```text id="q6v3wp"
Loose Coupling
+
Scalability
```

---

## Example

```text id="r5v8qt"
Order Created
      |
      v
Payment Service
      |
      v
Inventory Service
```

---

## Modern Usage

```text id="n8m4xp"
Microservices
```

---

# 69. Event Sourcing

Event Sourcing stores changes as events.

---

## Traditional Model

```text id="m3v7wr"
Current State Only
```

---

## Event Sourcing

```text id="x7m2qt"
Event 1
Event 2
Event 3
Event 4
```

---

## Benefits

```text id="p8m4wr"
Audit Trail
+
Replay Capability
```

---

## Common Usage

```text id="r7v3xt"
Financial Systems
+
Banking
```

---

# 70. CQRS

CQRS = Command Query Responsibility Segregation.

---

## Concept

```text id="n4m8wp"
Writes
    |
    v
Commands

Reads
    |
    v
Queries
```

---

## Architecture

```text id="x5v7qt"
Command Side
      |
      v
Kafka
      |
      v
Query Side
```

---

## Benefits

```text id="m8v2wr"
Independent Scaling
+
Better Performance
```

---

## Common Usage

```text id="r5v8wp"
Event Driven Systems
+
Microservices
```

---

# Kafka Ecosystem Summary

```text id="n7m4xt"
Kafka Connect
+
Kafka Streams
+
KSQLDB
+
Schema Registry
+
MirrorMaker
```

---

# Event Streaming Summary

```text id="p4m9wr"
Event Driven Architecture
+
Event Sourcing
+
CQRS
+
Real Time Processing
```

---

# 71. Kafka Security

Kafka security protects clusters, topics, consumers, and producers from unauthorized access.

---

## Security Goals

```text id="x7m4wp"
Authentication
+
Authorization
+
Encryption
+
Auditing
```

---

## Security Layers

```text id="m5v8xt"
Client Security
+
Broker Security
+
Network Security
+
Data Security
```

---

## Production Requirement

```text id="r8v2qt"
Never Run Kafka
Without Security
```

---

## Security Architecture

```text id="p4m7wr"
Producer
      |
      v
Authentication
      |
      v
Authorization
      |
      v
Kafka
```

---

# 72. Authentication

Authentication verifies client identity.

---

## Purpose

```text id="x6v3qt"
Who Are You?
```

---

## Supported Methods

```text id="m8v4xp"
SASL
+
TLS Certificates
+
Kerberos
+
OAuth
```

---

## Workflow

```text id="r7m2wt"
Client
      |
      v
Authentication
      |
      v
Kafka
```

---

## Goal

```text id="n5v9wr"
Verify Identity
```

---

# 73. Authorization

Authorization controls access to Kafka resources.

---

## Purpose

```text id="q4m8xt"
What Can You Do?
```

---

## Resources

```text id="x8m2wp"
Topics
+
Consumer Groups
+
Clusters
```

---

## Example

```text id="r5v7qt"
Read Topic

Allowed

Write Topic

Denied
```

---

## Goal

```text id="m4v8wr"
Least Privilege
```

---

# 74. ACLs

ACL = Access Control List.

Kafka uses ACLs for authorization.

---

## Example

```text id="p7m3xt"
User A

Read Orders Topic
```

---

## Permissions

```text id="n3v8wp"
Read
+
Write
+
Create
+
Delete
+
Alter
```

---

## Workflow

```text id="x5m9qt"
User
   |
   v
ACL Check
   |
   v
Allow / Deny
```

---

## Benefits

```text id="r8v4wr"
Fine Grained Access Control
```

---

# 75. TLS Encryption

TLS encrypts network communication.

---

## Purpose

```text id="m2v7xt"
Protect Data

In Transit
```

---

## Without TLS

```text id="q6m8wp"
Producer
   |
   v
Plain Text
   |
   v
Broker
```

---

## With TLS

```text id="n7m4qt"
Producer
   |
   v
Encrypted Traffic
   |
   v
Broker
```

---

## Benefits

```text id="x4v8wr"
Confidentiality
+
Integrity
```

---

## Recommendation

```text id="m8v3xt"
Always Enable TLS
```

---

# 76. SASL

SASL provides authentication mechanisms for Kafka.

---

## Common SASL Methods

```text id="q5m7wp"
PLAIN
+
SCRAM
+
GSSAPI
+
OAuth
```

---

## Recommended

```text id="r9v2qt"
SCRAM-SHA-256

SCRAM-SHA-512
```

---

## Benefits

```text id="p4m8wr"
Strong Authentication
```

---

## Common Usage

```text id="x7m4qt"
Enterprise Kafka
```

---

# 77. Monitoring

Monitoring ensures Kafka health and performance.

---

## Important Metrics

```text id="n5v8xp"
Broker Health
+
Topic Throughput
+
Consumer Lag
+
Disk Usage
+
Network Usage
```

---

## Architecture

```text id="r3m9wp"
Kafka
   |
   v
Metrics
   |
   v
Dashboard
```

---

## Popular Tools

```text id="m4v8qt"
Prometheus
+
Grafana
+
Datadog
```

---

## Goal

```text id="q8m2wr"
Early Problem Detection
```

---

# 78. Logging

Kafka logs provide operational visibility.

---

## Log Types

```text id="x6v4wp"
Broker Logs
+
Controller Logs
+
Application Logs
```

---

## Common Log Usage

```text id="r5m9xt"
Troubleshooting
+
Auditing
```

---

## Log Locations

```text id="n7v3wr"
Server Logs
Application Logs
```

---

## Benefits

```text id="x4m8wr"
Root Cause Analysis
```

---

# 79. Troubleshooting

Troubleshooting helps identify operational issues.

---

## Common Problems

```text id="m8v4qt"
Consumer Lag
+
Broker Failure
+
Partition Imbalance
+
High Latency
```

---

## Investigation Workflow

```text id="q4m8wr"
Issue
  |
  v
Logs
  |
  v
Metrics
  |
  v
Root Cause
```

---

## Common Checks

```text id="r8v2qt"
Broker Status
+
Consumer Lag
+
Disk Space
```

---

## Goal

```text id="x6m4wp"
Resolve
+
Prevent Recurrence
```

---

# 80. Kafka on Kubernetes

Kafka can run natively on Kubernetes.

---

## Architecture

```text id="n5v9xt"
Kubernetes
      |
      v
Kafka Cluster
      |
      v
Persistent Volumes
```

---

## Benefits

```text id="p7m3wr"
Automation
+
Self Healing
```

---

## Requirements

```text id="m4v8qt"
Persistent Storage
+
Monitoring
```

---

## Common Usage

```text id="q5v7xt"
Platform Engineering
+
Cloud Native Deployments
```

---

# 81. Strimzi

Strimzi is the most popular Kafka Operator for Kubernetes.

---

## Responsibilities

```text id="r4m9wp"
Deployment
+
Scaling
+
Upgrades
+
Management
```

---

## Architecture

```text id="n7m4wr"
Strimzi
      |
      v
Kafka Cluster
```

---

## Benefits

```text id="x4m8wr"
Automation
+
Simplified Operations
```

---

## Common Usage

```text id="q6v3wp"
Production Kubernetes
```

---

# 82. Operators

Operators automate Kafka lifecycle management.

---

## Functions

```text id="r5v8qt"
Provisioning
+
Scaling
+
Recovery
+
Maintenance
```

---

## Popular Operators

```text id="n8m4xp"
Strimzi
+
Confluent Operator
```

---

## Benefits

```text id="m3v7wr"
Reduced Manual Work
```

---

## Goal

```text id="x7m2qt"
Fully Automated Operations
```

---

# 83. Performance Optimization

Performance depends on proper cluster sizing and configuration.

---

## Optimization Areas

```text id="p8m4wr"
Partitions
+
Replication
+
Compression
+
Batching
```

---

## Producer Optimization

```text id="r7v3xt"
batch.size
+
linger.ms
+
compression.type
```

---

## Consumer Optimization

```text id="n4m8wp"
fetch.min.bytes
+
max.poll.records
```

---

## Goal

```text id="x5v7qt"
High Throughput
+
Low Latency
```

---

# 84. Common Mistakes

Many Kafka issues result from poor design.

---

## Common Mistakes

```text id="m8v2wr"
Too Few Partitions
+
No Monitoring
+
No Replication
+
No Security
+
Poor Topic Design
```

---

## Impact

```text id="r5v8wp"
Performance Issues
+
Downtime
+
Data Loss Risk
```

---

## Prevention

```text id="n7m4xt"
Capacity Planning
+
Monitoring
+
Automation
```

---

# 85. Common Interview Questions

## What is Kafka?

```text id="p4m9wr"
Distributed Event Streaming Platform
```

---

## What is a Topic?

```text id="x8m3qt"
Logical Channel
For Events
```

---

## What is a Partition?

```text id="m5v8wr"
Unit Of Parallelism
```

---

## What is an Offset?

```text id="r4v9xp"
Unique Position
Inside A Partition
```

---

## What is a Consumer Group?

```text id="n6m4qt"
Group Of Consumers
Sharing Workload
```

---

## What is ISR?

```text id="p7v3wr"
In Sync Replicas
```

---

## What is KRaft?

```text id="x5m8qt"
ZooKeeper-Free Kafka
Metadata Management
```

---

## Difference Between Kafka and RabbitMQ?

```text id="r8m2wp"
Kafka

Event Streaming

----------------

RabbitMQ

Traditional Messaging
```

---

## What Is Log Compaction?

```text id="n4v8xt"
Keep Latest Value
Per Key
```

---

## What Is Consumer Lag?

```text id="m7v3qt"
Difference Between

Latest Offset

And

Consumer Offset
```

---

# 86. Additional Resources

| Resource           | URL                                                                                                |
| ------------------ | -------------------------------------------------------------------------------------------------- |
| Apache Kafka Docs  | [https://kafka.apache.org/documentation](https://kafka.apache.org/documentation)                   |
| Confluent Docs     | [https://docs.confluent.io](https://docs.confluent.io)                                             |
| Strimzi Docs       | [https://strimzi.io](https://strimzi.io)                                                           |
| KSQLDB Docs        | [https://docs.ksqldb.io](https://docs.ksqldb.io)                                                   |
| Kafka Connect Docs | [https://kafka.apache.org/documentation/#connect](https://kafka.apache.org/documentation/#connect) |

---

# 📎 Official Documentation

* [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
* [Confluent Documentation](https://docs.confluent.io/)
* [Strimzi Documentation](https://strimzi.io/)
* [KSQLDB Documentation](https://docs.ksqldb.io/)
* [Kafka Connect Documentation](https://kafka.apache.org/documentation/#connect)

---

# 📎 Disclaimer & Attribution

This document is an unofficial educational cheatsheet created for learning, revision, interview preparation, DevOps engineering, platform engineering, cloud engineering, site reliability engineering, software engineering, event-driven architecture, and distributed systems.

Primary references include:

* Apache Kafka Documentation
* Confluent Documentation
* Strimzi Documentation
* Kafka Connect Documentation
* KSQLDB Documentation

Apache Kafka is an open-source distributed event streaming platform maintained by the [Apache Software Foundation](https://www.apache.org?utm_source=chatgpt.com).

All trademarks, product names, and service names belong to their respective owners.

For authoritative information, always refer to official documentation.

---

# 🎉 Conclusion

```text id="x8m4qt"
Topics
+
Partitions
+
Offsets
+
Producers
+
Consumers
+
Consumer Groups
+
Replication
+
Kafka Connect
+
Schema Registry
+
KRaft
+
Event Streaming
+
Security
```

Apache Kafka provides a highly scalable, fault-tolerant, distributed event streaming platform that powers modern microservices, real-time analytics, event-driven architectures, data pipelines, and cloud-native systems.

---

```md id="n7v3wr"
**Last Updated:** 2026
**Platform:** Apache Kafka
**Version:** Kafka 4.x+
**License:** Apache License 2.0

For latest information, visit https://kafka.apache.org/documentation/
```