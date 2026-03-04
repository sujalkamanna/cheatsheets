# Datadog Cheatsheet

![Datadog Logo](https://imgix.datadoghq.com/img/about/presskit/logo-v/dd_vertical_purple.png)

---

## Table of Contents
1. [What is Datadog?](#1-what-is-datadog)
2. [Core Concepts](#2-core-concepts)
3. [Installation & Setup](#3-installation--setup)
4. [Agents & Integration](#4-agents--integration)
5. [Metrics & Monitoring](#5-metrics--monitoring)
6. [Logs Management](#6-logs-management)
7. [Dashboards](#7-dashboards)
8. [Alerts & Monitors](#8-alerts--monitors)
9. [Best Practices](#9-best-practices)
10. [Additional Resources](#10-additional-resources)

---

## 1. What is Datadog?

Datadog is a cloud-based monitoring and analytics platform that provides infrastructure monitoring, application performance monitoring (APM), log management, and user experience monitoring across your entire tech stack.

**Key Benefits:**
- Infrastructure and application monitoring
- Distributed tracing and APM
- Log aggregation and analysis
- Real-time alerting
- Custom dashboards and visualizations
- Integration with 500+ services
- Multi-cloud support (AWS, Azure, GCP)
- Cost optimization insights

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Agent** | Lightweight daemon collecting metrics and logs from hosts |
| **Metrics** | Time-series data representing measurements |
| **Tags** | Key-value pairs for organizing and filtering data |
| **Dashboards** | Visual representation of metrics and logs |
| **Monitors** | Alerting rules based on metric thresholds |
| **APM** | Application Performance Monitoring for tracing |
| **Logs** | Centralized log collection and analysis |
| **Service** | Application or component being monitored |
| **Environment** | Deployment environment (prod, staging, dev) |
| **Integration** | Connection to external services/platforms |

---

## 3. Installation & Setup

### Datadog Agent Installation

```bash
# macOS
DD_AGENT_MAJOR_VERSION=7 DD_API_KEY=YOUR_API_KEY \
  DD_SITE="datadoghq.com" bash -c "$(curl -L https://s3.amazonaws.com/dd-agent/scripts/install_mac_os.sh)"

# Linux (Debian/Ubuntu)
sudo apt-get update
sudo apt-get install -y datadog-agent

DD_API_KEY=YOUR_API_KEY DD_SITE="datadoghq.com" \
  DD_AGENT_MAJOR_VERSION=7 \
  bash -c "$(curl -L https://s3.amazonaws.com/dd-agent/scripts/install_script.sh)"

# Linux (RedHat/CentOS)
sudo yum install -y datadog-agent

# Windows (PowerShell)
$DDAPIKey="YOUR_API_KEY"
$DDSite="datadoghq.com"
$DDAgentVersion="7"

(New-Object System.Net.WebClient).DownloadFile('https://s3.amazonaws.com/ddagent-windows/ddagent-cli-latest.exe', 'ddagent-cli-latest.exe')
.\ddagent-cli-latest.exe --api-key=$DDAPIKey --site=$DDSite
```

### Agent Configuration

```yaml
# /etc/datadog-agent/datadog.yaml
api_key: YOUR_API_KEY
site: datadoghq.com
hostname: my-host
env: production
tags:
  - service:api
  - team:backend
  - region:us-east-1

# Logging
logs_enabled: true
log_level: info

# APM
apm_enabled: true
apm_non_local_traffic: false

# Process monitoring
process_config:
  enabled: true

# System probe
system_probe_config:
  enabled: true
```

### Verify Agent Status

```bash
# Linux/macOS
sudo datadog-agent status

# Windows
& "C:\Program Files\Datadog\Datadog Agent\bin\agent.exe" status

# Check logs
tail -f /var/log/datadog/agent.log

# Test connectivity
datadog-agent configcheck
```

---

## 4. Agents & Integration

### Install Integrations

```bash
# Install integration via Agent
sudo datadog-agent integration install datadog-<integration-name>==<version>

# Install PostgreSQL integration
sudo datadog-agent integration install datadog-postgres==<version>

# Install MySQL integration
sudo datadog-agent integration install datadog-mysql==<version>

# Restart agent
sudo systemctl restart datadog-agent
```

### Integration Configuration

```yaml
# /etc/datadog-agent/conf.d/mysql.d/conf.yaml
init_config:

instances:
  - server: 127.0.0.1
    user: datadog
    password: password
    port: 3306
    tags:
      - service:mysql
      - env:prod
```

### Common Integrations

```yaml
# PostgreSQL
# /etc/datadog-agent/conf.d/postgres.d/conf.yaml
init_config:

instances:
  - host: localhost
    port: 5432
    username: datadog
    password: password
    dbname: postgres
    ssl: false

# Docker
# /etc/datadog-agent/conf.d/docker.d/conf.yaml
init_config:

instances:
  - url: unix:///var/run/docker.sock
    collect_container_size: true

# Kubernetes
# /etc/datadog-agent/conf.d/kubernetes.d/conf.yaml
init_config:

instances:
  - kubernetes_namespace_name: default
```

### Custom Metrics via StatsD

```python
# Python example
from datadog import initialize, api, statsd

# Send metric
statsd.gauge('my.custom.metric', 123, tags=['env:prod'])
statsd.increment('requests.count', 1, tags=['service:api'])
statsd.timing('request.duration', 250, tags=['endpoint:/api/users'])

# Flush
statsd.flush()
```

```bash
# Bash example
echo "custom.metric:100|g|#env:prod" | nc -u -w0 127.0.0.1 8125
echo "request.count:1|c|#service:api" | nc -u -w0 127.0.0.1 8125
```

---

## 5. Metrics & Monitoring

### Query Metrics

```
avg:system.cpu{env:prod}
max:system.memory{service:api}
sum:requests.total{*}
avg:response.time{host:web-*.prod}
```

### Metric Query Examples

```
# Average CPU across all hosts
avg:system.cpu.user{*}

# Max memory per host
max:system.mem.used{*} by {host}

# Request count per service
sum:http.requests{*} by {service}

# 95th percentile response time
pcts:app.response.time{env:prod}{75,95,99}

# Rate of change
rate(sum:errors.total{*})

# Moving average
moveavg(avg:system.cpu{*}, 5)

# Derivative
derivative(sum:requests.total{*})
```

### Custom Metrics API

```bash
# Send metric via API
curl -X POST "https://api.datadoghq.com/api/v1/series" \
  -H "Content-Type: application/json" \
  -d '{
    "series": [
      {
        "metric": "custom.metric.name",
        "points": [['"$(date +%s)"', 100]],
        "type": "gauge",
        "tags": ["env:prod", "service:api"]
      }
    ]
  }' \
  -H "DD-API-KEY: YOUR_API_KEY"
```

### Metric Tagging Strategy

```yaml
# Recommended tag structure
# env:production
# service:api
# team:backend
# version:1.2.3
# region:us-east-1
# datacenter:dc1

tags:
  - env:prod
  - service:payment
  - team:fintech
  - region:us-east-1
```

---

## 6. Logs Management

### Enable Log Collection

```yaml
# /etc/datadog-agent/datadog.yaml
logs_enabled: true

# /etc/datadog-agent/conf.d/apache.d/conf.yaml
logs:
  - type: file
    path: /var/log/apache2/access.log
    service: apache
    source: apache
    sourcecategory: web_access
    tags:
      - env:prod

  - type: file
    path: /var/log/apache2/error.log
    service: apache
    source: apache
    sourcecategory: web_error
    tags:
      - env:prod
```

### Log Collection

```yaml
# Application logs
logs:
  - type: file
    path: /var/log/myapp/application.log
    service: myapp
    source: custom
    tags:
      - env:prod

# Docker logs
logs:
  - type: docker
    service: myapp
    source: docker
    tags:
      - env:prod

# Syslog
logs:
  - type: syslog
    port: 514
    service: system
    source: syslog
```

### Log Processing Rules

```yaml
# /etc/datadog-agent/conf.d/myapp.d/conf.yaml
logs:
  - type: file
    path: /var/log/app.log
    service: myapp
    source: custom
    
    # Parse JSON logs
    parser:
      type: json

    # Extract fields
    processors:
      - type: attribute-remapper
        sources: [msg]
        sourceType: attribute
        target: message
        targetType: attribute
        preserveSource: false

      - type: category-processor
        target: severity
        categories:
          - name: error
            match: "ERROR"
          - name: warning
            match: "WARN"
          - name: info
            match: "INFO"
```

### Log Queries

```
service:api status:error
env:prod team:backend error_code:500
host:web-01 "user_id:*"
source:docker container_id:abc123
```

---

## 7. Dashboards

### Create Dashboard via API

```bash
# Create dashboard
curl -X POST "https://api.datadoghq.com/api/v1/dashboard" \
  -H "Content-Type: application/json" \
  -H "DD-API-KEY: YOUR_API_KEY" \
  -d @dashboard.json
```

### dashboard.json Example

```json
{
  "title": "Application Performance",
  "layout_type": "ordered",
  "widgets": [
    {
      "id": 1,
      "definition": {
        "type": "timeseries",
        "requests": [
          {
            "q": "avg:system.cpu{env:prod}",
            "display_type": "line"
          },
          {
            "q": "max:system.mem.used{env:prod}",
            "display_type": "area",
            "on_right_yaxis": true
          }
        ],
        "title": "CPU and Memory"
      }
    },
    {
      "id": 2,
      "definition": {
        "type": "query_value",
        "requests": [
          {
            "q": "sum:http.requests{*}",
            "aggregator": "sum"
          }
        ],
        "title": "Total Requests",
        "precision": 0
      }
    },
    {
      "id": 3,
      "definition": {
        "type": "log_stream",
        "query": "service:api status:error",
        "title": "Error Logs"
      }
    },
    {
      "id": 4,
      "definition": {
        "type": "heatmap",
        "requests": [
          {
            "q": "avg:app.response_time{*}"
          }
        ],
        "title": "Response Time Distribution"
      }
    }
  ]
}
```

### Dashboard Types

```json
{
  "widgets": [
    {
      "definition": {
        "type": "timeseries"
      }
    },
    {
      "definition": {
        "type": "query_value"
      }
    },
    {
      "definition": {
        "type": "log_stream"
      }
    },
    {
      "definition": {
        "type": "heatmap"
      }
    },
    {
      "definition": {
        "type": "distribution"
      }
    },
    {
      "definition": {
        "type": "table"
      }
    }
  ]
}
```

---

## 8. Alerts & Monitors

### Create Monitor via API

```bash
# Metric monitor
curl -X POST "https://api.datadoghq.com/api/v1/monitor" \
  -H "Content-Type: application/json" \
  -H "DD-API-KEY: YOUR_API_KEY" \
  -d '{
    "type": "metric alert",
    "query": "avg(last_5m):avg:system.cpu{env:prod} > 80",
    "name": "High CPU Alert",
    "message": "CPU is high on {{host.name}}. @pagerduty @slack-alerts",
    "tags": ["team:backend"],
    "thresholds": {
      "critical": 80,
      "warning": 70
    }
  }'

# Log monitor
curl -X POST "https://api.datadoghq.com/api/v1/monitor" \
  -H "Content-Type: application/json" \
  -H "DD-API-KEY: YOUR_API_KEY" \
  -d '{
    "type": "log alert",
    "query": "logs(\"service:api status:error\").index(\"*\").rollup(\"count\").last(\"5m\") > 10",
    "name": "High Error Rate",
    "message": "Error rate is high. Check logs at @slack-alerts",
    "tags": ["service:api"]
  }'
```

### Monitor Types

```bash
# Metric Alert
avg(last_5m):avg:system.cpu{*} > 80

# Threshold Alert
avg(last_10m):avg:system.mem.used{*} > 90

# Anomaly Detection
avg:system.cpu{*}.anomalies(linear, 2) >= 1

# Forecast
avg:system.cpu{*}.forecast(linear, last_4w, interval='1d', algorithm='trend') > 80

# Composite Monitor
avg:system.cpu{*} > 80 || avg:system.mem.used{*} > 90
```

### Notification Integrations

```
@slack-alerts
@pagerduty
@email
@webhook
@sns
@jira
@opsgenie
@victorops
```

### Monitor Configuration

```bash
# Mute monitor
curl -X POST "https://api.datadoghq.com/api/v1/monitor/12345/mute" \
  -H "DD-API-KEY: YOUR_API_KEY" \
  -d '{
    "scope": "host:my-host",
    "end": 1234567890
  }'

# Unmute monitor
curl -X POST "https://api.datadoghq.com/api/v1/monitor/12345/unmute" \
  -H "DD-API-KEY: YOUR_API_KEY"

# List monitors
curl -X GET "https://api.datadoghq.com/api/v1/monitor" \
  -H "DD-API-KEY: YOUR_API_KEY"
```

---

## 9. Best Practices

### Monitoring Strategy

```yaml
# Golden signals monitoring
# Latency
avg:app.latency{service:api} > 500ms

# Traffic
sum:requests.count{service:api}

# Errors
sum:errors.count{service:api}

# Saturation
avg:system.cpu{env:prod} > 80%
```

### Tagging Best Practices

```yaml
# Mandatory tags
- env:prod|staging|dev
- service:api|web|backend
- team:backend|frontend|platform

# Optional but recommended
- version:1.2.3
- region:us-east-1
- datacenter:dc1
- owner:john.doe
```

### Alert Guidelines

```bash
# Clear alert message
"High CPU on {{host.name}}: {{value}}% (threshold: 80%)"

# Include runbook
"Check wiki: https://wiki.company.com/cpu-high"

# Include context
"Service: {{service.name}}, Environment: {{env}}"

# Avoid alert fatigue
# - Set thresholds based on baselines
# - Use appropriate evaluation windows
# - Implement escalation policies
```

### Log Processing

```yaml
# Structure logs with JSON
{
  "timestamp": "2024-01-01T12:00:00Z",
  "level": "ERROR",
  "service": "api",
  "request_id": "123-456",
  "user_id": "user123",
  "message": "Database connection failed",
  "duration_ms": 150,
  "status_code": 500
}

# This enables better parsing and filtering
```

### Cost Optimization

```bash
# 1. Reduce metric cardinality
# Avoid high-cardinality tags (user_id, request_id)

# 2. Use log sampling
# Only send representative sample of logs

# 3. Adjust retention
# Keep detailed logs for 7 days, summary for 30 days

# 4. Filter unnecessary logs
# Skip health checks, verbose debug logs
```

---

## 10. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Agent Setup** | https://docs.datadoghq.com/agent/ |
| **Integrations** | https://docs.datadoghq.com/integrations/ |
| **APM** | https://docs.datadoghq.com/tracing/ |
| **Logs** | https://docs.datadoghq.com/logs/ |
| **API Reference** | https://docs.datadoghq.com/api/latest/ |
| **Dashboards** | https://docs.datadoghq.com/dashboards/ |
| **Monitors** | https://docs.datadoghq.com/monitors/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Getting Started** | https://docs.datadoghq.com/getting_started/ |
| **Best Practices** | https://docs.datadoghq.com/best_practices/ |
| **Community** | https://www.datadoghq.com/blog/ |
| **Training** | https://www.datadoghq.com/blog/datadog-diver-series/ |

---

## Quick Reference

```bash
# Install agent
DD_API_KEY=YOUR_API_KEY bash -c "$(curl -L https://s3.amazonaws.com/dd-agent/scripts/install_script.sh)"

# Start agent
sudo systemctl start datadog-agent

# Check status
sudo datadog-agent status

# View logs
tail -f /var/log/datadog/agent.log

# Test metric
echo "custom.metric:100|g|#env:prod" | nc -u -w0 127.0.0.1 8125

# Create monitor
curl -X POST "https://api.datadoghq.com/api/v1/monitor" \
  -H "DD-API-KEY: YOUR_API_KEY" \
  -d @monitor.json

# List monitors
curl -X GET "https://api.datadoghq.com/api/v1/monitor" \
  -H "DD-API-KEY: YOUR_API_KEY"

# Create dashboard
curl -X POST "https://api.datadoghq.com/api/v1/dashboard" \
  -H "DD-API-KEY: YOUR_API_KEY" \
  -d @dashboard.json
```

---

**Last Updated:** 2026
**Agent Version:** 7.x
**License:** Datadog License

For latest information, visit [Datadog Documentation](https://docs.datadoghq.com/)