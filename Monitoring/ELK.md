The **ELK Stack** is a popular set of tools used in DevOps and cloud infrastructure for:

* **Collecting logs**
* **Searching logs**
* **Analyzing logs**
* **Visualizing logs and metrics**

ELK stands for:

* **E** = Elasticsearch
* **L** = Logstash
* **K** = Kibana

Today, people also often use **Elastic Stack**, which additionally includes:

* Beats
* Elastic Agent
* Fleet
* APM
* Security tools

Official site: [Elastic Stack](https://www.elastic.co/elastic-stack)

---

# 1. Why ELK Stack Exists

In DevOps, applications run on:

* servers
* containers
* Kubernetes
* cloud VMs
* microservices

Every system generates logs like:

```txt
INFO User logged in
ERROR Database timeout
WARN CPU usage high
```

Without ELK:

* logs are scattered everywhere
* debugging becomes hard
* monitoring production is painful

ELK centralizes everything into **one searchable system**.

---

# 2. Real DevOps Problem ELK Solves

Imagine:

* 50 microservices
* 200 containers
* thousands of requests per second

A customer says:

> "Payment failed at 2:03 PM"

Without ELK:

* SSH into servers
* manually grep logs
* waste hours

With ELK:

* search instantly
* filter by timestamp
* visualize failures
* detect patterns

This is why ELK is heavily used in:

* DevOps
* SRE
* Cloud Operations
* Security Monitoring
* Observability

---

# 3. High-Level Architecture

Basic flow:

```txt
Applications/Servers
        ↓
    Beats/Filebeat
        ↓
     Logstash
        ↓
 Elasticsearch
        ↓
      Kibana
```

Think of it like:

| Component     | Simple Meaning      |
| ------------- | ------------------- |
| Beats         | Collect logs        |
| Logstash      | Process logs        |
| Elasticsearch | Store + search logs |
| Kibana        | Show dashboards     |

---

# 4. Elasticsearch (Core Engine)

## What It Is

Elasticsearch is the heart of ELK.

It is:

* a distributed search engine
* document database
* analytics engine

Built on:

* Apache Lucene

Official site: [Elasticsearch Docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

---

## Simple Analogy

Imagine Google Search for your logs.

Instead of searching websites:

* Elasticsearch searches logs and events.

---

## How Data Is Stored

Data stored as JSON documents:

```json
{
  "timestamp": "2026-05-22T10:00:00",
  "level": "ERROR",
  "service": "payment-service",
  "message": "Database timeout"
}
```

---

## Important Concepts

---

### a) Index

Like a database.

Example:

```txt
logs-2026
payments-logs
nginx-logs
```

---

### b) Document

One JSON record.

Example:

```json
{
  "user": "john",
  "action": "login"
}
```

---

### c) Shards

Elasticsearch splits data into pieces.

Why?

* scalability
* parallel processing
* high availability

Example:

```txt
1 TB logs
→ split into 5 shards
```

---

### d) Replicas

Copies of shards.

Purpose:

* fault tolerance
* high availability

---

### e) Cluster

Group of Elasticsearch nodes.

```txt
Node1
Node2
Node3
```

Together = cluster.

---

## Why Elasticsearch Is Powerful

Features:

* near real-time search
* distributed
* scalable
* full-text search
* aggregation
* analytics

Example queries:

* errors in last 5 minutes
* top failing APIs
* CPU spikes
* slow database calls

---

# 5. Logstash (Data Processing Pipeline)

## What It Does

Logstash collects and transforms data.

Official docs: [Logstash Docs](https://www.elastic.co/guide/en/logstash/current/index.html)

---

## Think of Logstash As

A factory conveyor belt.

It:

1. receives logs
2. cleans logs
3. transforms logs
4. forwards logs

---

## Logstash Pipeline

Three stages:

```txt
INPUT → FILTER → OUTPUT
```

---

## a) Input

Where logs come from.

Examples:

* files
* Kafka
* syslog
* Redis
* Beats

Example:

```txt
/var/log/nginx/access.log
```

---

## b) Filter

Transforms data.

Examples:

* parse logs
* remove unwanted fields
* convert timestamps
* enrich data

Popular filters:

* grok
* mutate
* date
* geoip

---

## c) Output

Where logs go.

Usually:

* Elasticsearch

Can also go to:

* Kafka
* file
* S3

---

## Example Pipeline

```txt
Nginx logs
    ↓
Logstash parses IP/time/status
    ↓
Elasticsearch stores structured data
```

---

# 6. Beats (Log Collectors)

Beats are lightweight agents installed on servers.

Official docs: [Beats Platform Docs](https://www.elastic.co/beats)

---

## Why Beats?

Instead of heavy Logstash on every server:

* use lightweight Beats

---

## Types of Beats

| Beat       | Purpose             |
| ---------- | ------------------- |
| Filebeat   | Collect log files   |
| Metricbeat | Collect metrics     |
| Packetbeat | Network traffic     |
| Winlogbeat | Windows logs        |
| Auditbeat  | Security audit logs |
| Heartbeat  | Uptime monitoring   |

---

## Most Common: Filebeat

Reads log files continuously.

Example:

```txt
/var/log/nginx/access.log
```

Then sends data to:

* Logstash
* Elasticsearch

---

## Filebeat Flow

```txt
Server Logs
    ↓
Filebeat
    ↓
Logstash/Elasticsearch
```

---

# 7. Kibana (Visualization Layer)

Kibana is the UI/dashboard part.

Official docs: [Kibana Docs](https://www.elastic.co/guide/en/kibana/current/index.html)

---

## What Kibana Does

It helps you:

* search logs
* build dashboards
* create charts
* monitor systems
* create alerts

---

## Common Dashboards

Examples:

* CPU usage
* Kubernetes pod failures
* API response times
* Login failures
* Nginx traffic
* Error rates

---

## Kibana Features

### Discover

Search raw logs.

### Dashboards

Visual charts.

### Visualizations

Graphs, pie charts, heatmaps.

### Alerts

Notify when:

* CPU > 90%
* errors spike
* disk full

### Dev Tools

Run Elasticsearch queries.

---

# 8. End-to-End Example

Suppose:

* user opens website
* API fails

Flow:

```txt
Nginx logs error
    ↓
Filebeat reads log
    ↓
Logstash parses log
    ↓
Elasticsearch indexes log
    ↓
Kibana dashboard shows spike
```

DevOps engineer:

* sees error instantly
* filters logs
* identifies root cause

---

# 9. ELK in Kubernetes

Very common setup in DevOps.

Architecture:

```txt
Pods
 ↓
Filebeat DaemonSet
 ↓
Elasticsearch Cluster
 ↓
Kibana
```

---

## Why Important?

Containers are ephemeral:

* logs disappear quickly

ELK centralizes logs permanently.

---

# 10. ELK vs Observability

Modern DevOps uses:

* logs
* metrics
* traces

ELK started with logs.

Now Elastic Stack also supports:

* metrics
* APM tracing
* security analytics

---

# 11. ELK vs EFK

You may hear:

## ELK

Elasticsearch + Logstash + Kibana

## EFK

Elasticsearch + Fluentd + Kibana

Why Fluentd?

* lighter than Logstash
* Kubernetes-friendly

---

# 12. ELK vs Splunk

Common interview topic.

| ELK                 | Splunk           |
| ------------------- | ---------------- |
| Open ecosystem      | Commercial       |
| More setup effort   | Easier initially |
| Cost effective      | Expensive        |
| Highly customizable | Enterprise-ready |

---

# 13. Important DevOps Use Cases

## a) Centralized Logging

All logs in one place.

---

## b) Monitoring

Track:

* CPU
* memory
* errors
* latency

---

## c) Security Analytics

Detect:

* suspicious logins
* attacks
* failed SSH attempts

---

## d) Troubleshooting

Find production issues quickly.

---

## e) Kubernetes Monitoring

Monitor:

* pods
* nodes
* containers

---

# 14. Important Elasticsearch Theory

---

## Inverted Index

Core search mechanism.

Instead of:

```txt
document → words
```

Elasticsearch stores:

```txt
word → documents
```

This makes searching extremely fast.

---

## Mapping

Defines schema/types.

Example:

```json
"user": "text",
"age": "integer"
```

---

## Aggregation

Used for analytics.

Examples:

* count errors
* average response time
* top IP addresses

---

## Query DSL

Elasticsearch query language.

Example:

```json
{
  "query": {
    "match": {
      "message": "timeout"
    }
  }
}
```

---
# ELK Stack Default Ports
---

| Component                   | Default Port | Purpose                             |
| --------------------------- | ------------ | ----------------------------------- |
| Elasticsearch               | `9200`       | REST API / HTTP access              |
| Elasticsearch               | `9300`       | Internal cluster node communication |
| Logstash                    | `5044`       | Beats input (very common)           |
| Logstash                    | `9600`       | Logstash monitoring API             |
| Kibana                      | `5601`       | Kibana web UI                       |
| Beats (Filebeat → Logstash) | `5044`       | Send logs to Logstash               |
| Filebeat → Elasticsearch    | `9200`       | Direct Elasticsearch connection     |

---

# Component-wise Explanation

---

# 1. Elasticsearch Ports

## Port 9200

Used for:

* REST API
* curl commands
* Kibana connection
* external applications

Example:

```bash id="3oqrjk"
curl http://localhost:9200
```

You’ll see cluster info.

---

## Port 9300

Used internally between Elasticsearch nodes.

Purpose:

* cluster communication
* shard replication
* node discovery

Example:

```txt id="k99lq9"
Node1 ↔ Node2 ↔ Node3
```

Important:

* Usually NOT exposed publicly.

---

# 2. Logstash Ports

## Port 5044

Most common Logstash input port.

Used by:

* Filebeat
* Beats agents

Example flow:

```txt id="14f0nc"
Filebeat → Logstash:5044
```

Typical Logstash config:

```conf id="md5jmb"
input {
  beats {
    port => 5044
  }
}
```

---

## Port 9600

Used for:

* Logstash monitoring API
* pipeline stats
* JVM metrics

Example:

```bash id="f37m9i"
curl http://localhost:9600
```

---

# 3. Kibana Port

## Port 5601

Default Kibana dashboard UI.

Access in browser:

```txt id="n26s9r"
http://localhost:5601
```

Used for:

* dashboards
* Discover logs
* visualizations
* Dev Tools

---

# 4. Beats Ports

## Filebeat

Filebeat itself usually does NOT expose ports.

Instead it sends data TO:

* Logstash:5044
* Elasticsearch:9200

---

# 5. Common DevOps Flow with Ports

```txt id="fjdj5c"
Application Logs
      ↓
Filebeat
      ↓ 5044
Logstash
      ↓ 9200
Elasticsearch
      ↓ 5601
Kibana
```

---

# 6. In Kubernetes

Example services:

| Service               | Port |
| --------------------- | ---- |
| Elasticsearch Service | 9200 |
| Kibana Service        | 5601 |
| Logstash Service      | 5044 |

---

# 7. Security Best Practice

In production:

## Publicly Accessible

* Kibana (5601) sometimes behind auth/proxy

## Internal Only

* 9200
* 9300
* 5044

Usually protected using:

* VPN
* firewall
* reverse proxy
* TLS/SSL
* authentication

---

# 8. Quick Memory Trick

```txt id="w2e3ej"
9200 → Elasticsearch API
9300 → Elasticsearch cluster
5044 → Beats → Logstash
5601 → Kibana UI
9600 → Logstash monitoring
```

---

# 9. Important Interview Tip

Very commonly asked:

> Difference between 9200 and 9300?

Answer:

| Port | Usage                                       |
| ---- | ------------------------------------------- |
| 9200 | REST API communication                      |
| 9300 | Internal node-to-node cluster communication |

---

# 15. Important Logstash Theory

---

## Grok Parsing

Converts raw logs into structured data.

Raw log:

```txt
192.168.1.1 GET /home 200
```

Structured:

```json
{
 "ip":"192.168.1.1",
 "method":"GET",
 "status":"200"
}
```

---

## Pipelines

Multiple pipelines can run simultaneously.

---

## Queueing

Helps avoid data loss during spikes.

---

# 16. Scaling ELK

As traffic grows:

## Elasticsearch Scaling

Add more nodes.

## Logstash Scaling

Multiple pipeline workers.

## Kafka Integration

Used as buffer.

Architecture:

```txt
Apps → Kafka → Logstash → Elasticsearch
```

Very common in enterprise DevOps.

---

# 17. ELK Challenges

ELK is powerful but has issues:

| Problem           | Explanation                |
| ----------------- | -------------------------- |
| High memory usage | Elasticsearch is heavy     |
| Complex scaling   | Large clusters need tuning |
| Storage cost      | Logs grow fast             |
| Mapping conflicts | Bad schemas cause issues   |

---

# 18. Best Practices in DevOps

## Use Filebeat Instead of Heavy Agents

Lightweight.

---

## Use ILM (Index Lifecycle Management)

Automatically:

* rotate logs
* archive old data
* delete expired logs

---

## Separate Hot/Warm Storage

Recent logs:

* fast SSD

Old logs:

* cheaper storage

---

## Monitor Elasticsearch Health

Check:

* cluster status
* heap memory
* shard allocation

---

# 19. ELK Interview Questions

Common ones:

1. What is ELK Stack?
2. Difference between ELK and EFK?
3. What are shards and replicas?
4. How does Elasticsearch search work?
5. What is Grok?
6. Explain Filebeat.
7. What happens if Elasticsearch node fails?
8. How do you scale ELK?
9. Explain inverted index.
10. Why use centralized logging?

---

# 20. Simple Mental Model

Remember this:

```txt
Beats      → Collect data
Logstash   → Process data
Elasticsearch → Store/Search
Kibana     → Visualize
```

---

# 21. Real Production Architecture

Enterprise setup:

```txt
Applications
    ↓
Filebeat
    ↓
Kafka
    ↓
Logstash
    ↓
Elasticsearch Cluster
    ↓
Kibana
    ↓
Alerts/Monitoring
```

This is very common in:

* Kubernetes
* AWS
* Azure
* GCP
* Microservices platforms

---

# 22. Final Summary

ELK Stack is mainly used in DevOps for:

* centralized logging
* monitoring
* troubleshooting
* observability
* analytics
* security monitoring

Core idea:

```txt
Collect → Process → Store → Search → Visualize
```

Where:

* Beats collect
* Logstash processes
* Elasticsearch stores/searches
* Kibana visualizes

That’s the complete ELK ecosystem in simple DevOps terms.

---

Here are the official Elastic Stack (ELK Stack) documentation and reference links for DevOps and administration:

---

# Official Elastic Documentation

## Elasticsearch

### Elasticsearch Main Documentation

[Elasticsearch Official Docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

### Elasticsearch Networking Settings

(ports, bind address, cluster communication, HTTP settings)

[Elasticsearch Networking Settings](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-network.html)

### Elasticsearch Cluster Administration

[Elasticsearch Cluster Docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster.html)

### Elasticsearch Index Management

[Elasticsearch Index Management Docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-mgmt.html)

---

## Logstash

### Logstash Official Documentation

[Logstash Official Docs](https://www.elastic.co/guide/en/logstash/current/index.html)

### Beats Input Plugin

(Filebeat → Logstash communication on port 5044)

[Logstash Beats Input Plugin](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-beats.html)

### Logstash Grok Filter

(used for log parsing)

[Logstash Grok Filter Docs](https://www.elastic.co/guide/en/logstash/current/plugins-filters-grok.html)

### Logstash Pipeline Configuration

[Logstash Pipeline Docs](https://www.elastic.co/guide/en/logstash/current/configuration.html)

---

## Kibana

### Kibana Official Documentation

[Kibana Official Docs](https://www.elastic.co/guide/en/kibana/current/index.html)

### Kibana Settings

(configuration, server.port, Elasticsearch connection)

[Kibana Settings Docs](https://www.elastic.co/guide/en/kibana/current/settings.html)

### Kibana Dashboards

[Kibana Dashboard Guide](https://www.elastic.co/guide/en/kibana/current/dashboard.html)

---

## Beats

### Beats Platform Documentation

[Beats Official Docs](https://www.elastic.co/guide/en/beats/libbeat/current/index.html)

### Filebeat Documentation

[Filebeat Official Docs](https://www.elastic.co/guide/en/beats/filebeat/current/index.html)

### Metricbeat Documentation

[Metricbeat Official Docs](https://www.elastic.co/guide/en/beats/metricbeat/current/index.html)

---

# Important DevOps References

## Elastic Stack Installation

[Elastic Stack Installation Guide](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html)

## Docker Deployment

[Elastic Docker Docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)

## Kubernetes Deployment (ECK)

Elastic Cloud on Kubernetes

[Elastic Cloud on Kubernetes Docs](https://www.elastic.co/guide/en/cloud-on-k8s/current/index.html)

## Index Lifecycle Management (ILM)

[ILM Documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-lifecycle-management.html)

## Security Configuration

TLS, users, roles, RBAC

[Elastic Security Docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/configuring-stack-security.html)

---

# Most Important Docs for Beginners

If you're starting ELK for DevOps, read in this order:

1. [Elastic Stack Overview](https://www.elastic.co/elastic-stack)
2. [Elasticsearch Docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
3. [Filebeat Docs](https://www.elastic.co/guide/en/beats/filebeat/current/index.html)
4. [Logstash Docs](https://www.elastic.co/guide/en/logstash/current/index.html)
5. [Kibana Docs](https://www.elastic.co/guide/en/kibana/current/index.html)
6. [Elastic Kubernetes Docs (ECK)](https://www.elastic.co/guide/en/cloud-on-k8s/current/index.html)

These cover almost everything needed for:

* DevOps
* centralized logging
* Kubernetes logging
* observability
* production ELK setup
* troubleshooting
* monitoring
* scaling ELK clusters