The **EFK Stack** is a logging and observability stack used heavily in **DevOps** and especially in **Kubernetes environments**.

EFK stands for:

* **E** → Elasticsearch
* **F** → Fluentd
* **K** → Kibana

It is very similar to ELK, but:

* **Logstash is replaced by Fluentd**

Why?
Because Fluentd is:

* lighter
* cloud-native
* Kubernetes-friendly
* easier for container logging

Official overview:
[Elastic Stack Overview](https://www.elastic.co/elastic-stack)

Fluentd official docs:
[Fluentd Official Docs](https://docs.fluentd.org)

---

# 1. Why EFK Stack Exists

In Kubernetes and microservices:

* containers start/stop quickly
* logs disappear
* debugging is difficult

EFK solves this by:

* collecting logs centrally
* processing logs
* storing logs
* visualizing logs

---

# 2. High-Level Architecture

```txt id="m1j8fx"
Applications / Containers
          ↓
       Fluentd
          ↓
    Elasticsearch
          ↓
        Kibana
```

Simple meaning:

| Component     | Purpose                |
| ------------- | ---------------------- |
| Fluentd       | Collect + process logs |
| Elasticsearch | Store + search logs    |
| Kibana        | Visualize logs         |

---

# 3. Why EFK Is Popular in Kubernetes

Kubernetes creates:

* many pods
* dynamic containers
* distributed logs

EFK is ideal because:

* Fluentd works very well with container logs
* Kubernetes officially supports Fluentd integrations
* lighter than Logstash

---

# 4. Elasticsearch (Storage + Search Engine)

Elasticsearch stores and indexes logs.

Official docs:
[Elasticsearch Docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

---

## What Elasticsearch Does

Think of it as:

> Google Search for logs

You can search:

* errors
* timestamps
* API failures
* pod crashes

---

## Example Log

```json id="2o8y1u"
{
  "timestamp": "2026-05-22T10:00:00",
  "pod": "payment-service",
  "level": "ERROR",
  "message": "Database timeout"
}
```

---

## Important Elasticsearch Theory

---

## a) Index

Like a database table.

Example:

```txt id="6rjpxe"
kubernetes-logs
nginx-logs
app-logs
```

---

## b) Document

One JSON log event.

---

## c) Shards

Elasticsearch splits large data into smaller pieces.

Why?

* scalability
* parallel processing

---

## d) Replicas

Copies of shards.

Purpose:

* high availability
* fault tolerance

---

## e) Cluster

Multiple Elasticsearch nodes working together.

---

# 5. Fluentd (Log Collector + Processor)

Fluentd is the core difference between ELK and EFK.

Official docs:
[Fluentd Documentation](https://docs.fluentd.org)

---

# What Fluentd Does

Fluentd:

* collects logs
* parses logs
* transforms logs
* forwards logs

It acts like:

> a smart log router

---

# Fluentd Pipeline

```txt id="xah0wb"
INPUT → PARSE/FILTER → OUTPUT
```

---

# Inputs

Fluentd collects logs from:

* Docker containers
* Kubernetes pods
* files
* syslog
* applications

Example:

```txt id="rn3w8m"
/var/log/containers/
```

---

# Filters

Fluentd can:

* parse JSON
* add metadata
* remove fields
* enrich logs

Example:

* add Kubernetes pod name
* namespace
* labels

---

# Outputs

Usually sends logs to:

* Elasticsearch

Can also send to:

* Kafka
* S3
* Datadog
* Splunk

---

# Why Fluentd Is Better for Kubernetes

Compared to Logstash:

| Fluentd                  | Logstash           |
| ------------------------ | ------------------ |
| Lightweight              | Heavy              |
| Lower memory             | Higher memory      |
| Kubernetes-native        | Generic            |
| Easier container logging | More configuration |

---

# 6. Kibana (Visualization Layer)

Kibana provides dashboards and search UI.

Official docs:
[Kibana Docs](https://www.elastic.co/guide/en/kibana/current/index.html)

---

# What Kibana Does

You can:

* search logs
* build dashboards
* monitor systems
* create alerts

---

# Common Dashboards

Examples:

* pod crashes
* API latency
* container restarts
* failed login attempts
* Kubernetes node health

---

# 7. EFK Stack Workflow

Example:

```txt id="3fbd2f"
Kubernetes Pod
      ↓
Container writes logs
      ↓
Fluentd reads logs
      ↓
Fluentd enriches logs
      ↓
Elasticsearch stores logs
      ↓
Kibana visualizes logs
```

---

# 8. Ports Used in EFK Stack

# Elasticsearch Ports

| Port   | Purpose                            |
| ------ | ---------------------------------- |
| `9200` | REST API / HTTP                    |
| `9300` | Node-to-node cluster communication |

---

# Kibana Port

| Port   | Purpose       |
| ------ | ------------- |
| `5601` | Kibana Web UI |

Access:

```txt id="nryu7l"
http://localhost:5601
```

---

# Fluentd Ports

| Port    | Purpose                 |
| ------- | ----------------------- |
| `24224` | Fluentd forward input   |
| `5140`  | Syslog input (optional) |
| `9880`  | HTTP input plugin       |

---

# Common Kubernetes Flow with Ports

```txt id="o7fl22"
Pods
 ↓
Fluentd :24224
 ↓
Elasticsearch :9200
 ↓
Kibana :5601
```

---

# 9. Fluentd Important Theory

---

# Buffering

Fluentd buffers logs before sending.

Benefits:

* avoids data loss
* handles traffic spikes

---

# Plugins

Fluentd is plugin-based.

Popular plugins:

* input plugins
* parser plugins
* output plugins

---

# Tagging

Logs are tagged for routing.

Example:

```txt id="z7ru1l"
kubernetes.nginx
app.payment
```

---

# Parsing

Converts raw logs into structured JSON.

Raw:

```txt id="eggrd6"
ERROR database timeout
```

Structured:

```json id="v44ahv"
{
 "level":"ERROR",
 "message":"database timeout"
}
```

---

# 10. EFK in Kubernetes

Very common architecture:

```txt id="6h9ih2"
Pods
 ↓
Fluentd DaemonSet
 ↓
Elasticsearch StatefulSet
 ↓
Kibana Deployment
```

---

# Why DaemonSet?

Fluentd runs on every Kubernetes node.

Purpose:

* collect logs from all containers

---

# 11. Fluent Bit vs Fluentd

Modern DevOps often uses:

* Fluent Bit instead of Fluentd

Why?
Because Fluent Bit is:

* ultra lightweight
* faster
* lower CPU usage

---

# Difference

| Fluent Bit         | Fluentd        |
| ------------------ | -------------- |
| Lightweight        | Heavier        |
| Written in C       | Ruby           |
| Edge log collector | Full processor |

Very common architecture:

```txt id="v9n9v6"
Fluent Bit → Fluentd → Elasticsearch
```

---

# 12. EFK vs ELK

| EFK                   | ELK             |
| --------------------- | --------------- |
| Uses Fluentd          | Uses Logstash   |
| Better for Kubernetes | Generic logging |
| Lightweight           | Heavy           |
| Cloud-native          | Traditional     |

---

# 13. Common DevOps Use Cases

---

## Centralized Logging

All logs in one place.

---

## Kubernetes Monitoring

Track:

* pod crashes
* restart loops
* container errors

---

## Troubleshooting

Debug distributed systems quickly.

---

## Security Monitoring

Detect:

* unauthorized access
* suspicious activity
* failed logins

---

## Application Monitoring

Monitor:

* latency
* API failures
* exceptions

---

# 14. Scaling EFK Stack

---

# Elasticsearch Scaling

Add more nodes.

---

# Fluentd Scaling

Multiple replicas.

---

# Kafka Integration

Enterprise architecture:

```txt id="utx5wk"
Applications
    ↓
Fluentd
    ↓
Kafka
    ↓
Elasticsearch
```

---

# 15. Challenges of EFK

| Problem                    | Explanation              |
| -------------------------- | ------------------------ |
| Elasticsearch memory usage | JVM heavy                |
| Storage cost               | Logs grow fast           |
| Complex scaling            | Large clusters difficult |
| Mapping conflicts          | Schema issues            |

---

# 16. Security Best Practices

Production setup usually includes:

* TLS encryption
* RBAC
* authentication
* private networking
* API keys

Do NOT expose:

* 9200 publicly
* 9300 publicly

---

# 17. Quick Memory Trick

```txt id="3j0n80"
Fluentd → Collect/process logs
Elasticsearch → Store/search logs
Kibana → Visualize logs
```

---

# 18. Real Production EFK Architecture

```txt id="udmkrj"
Kubernetes Pods
       ↓
Fluentd DaemonSet
       ↓
Kafka Buffer
       ↓
Elasticsearch Cluster
       ↓
Kibana Dashboards
       ↓
Alerts & Monitoring
```

---

# 19. Most Important Interview Questions

1. Difference between ELK and EFK?
2. Why Fluentd in Kubernetes?
3. What is DaemonSet in EFK?
4. Explain Elasticsearch shards and replicas.
5. What is Fluentd buffering?
6. Difference between Fluentd and Fluent Bit?
7. Why centralized logging?
8. Explain EFK workflow.

---

# 20. Official Documentation Links

## Elasticsearch

[Elasticsearch Docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

## Fluentd

[Fluentd Official Docs](https://docs.fluentd.org)

## Kibana

[Kibana Docs](https://www.elastic.co/guide/en/kibana/current/index.html)

## Fluent Bit

[Fluent Bit Docs](https://docs.fluentbit.io)

## Elastic Kubernetes Operator (ECK)

[Elastic Cloud on Kubernetes Docs](https://www.elastic.co/guide/en/cloud-on-k8s/current/index.html)