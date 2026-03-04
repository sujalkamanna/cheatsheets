# Prometheus Cheatsheet

![Prometheus Logo](https://prometheus.io/assets/prometheus_logo.png)

---

## Table of Contents
1. [What is Prometheus?](#1-what-is-prometheus)
2. [Core Concepts](#2-core-concepts)
3. [Installation & Setup](#3-installation--setup)
4. [Configuration](#4-configuration)
5. [Scraping & Targets](#5-scraping--targets)
6. [Metrics & Query Language](#6-metrics--query-language)
7. [PromQL Queries](#7-promql-queries)
8. [Alerting](#8-alerting)
9. [Exporters](#9-exporters)
10. [Best Practices](#10-best-practices)
11. [Troubleshooting](#11-troubleshooting)
12. [Additional Resources](#12-additional-resources)

---

## 1. What is Prometheus?

Prometheus is an open-source monitoring and alerting system designed for reliability and scalability. It collects metrics from configured targets at specified intervals, evaluates alerting rules, and displays results through a built-in web UI or external visualization tools like Grafana.

**Key Benefits:**
- Multi-dimensional data model
- Powerful query language (PromQL)
- Pull-based scraping architecture
- Built-in service discovery
- Time-series database (TSDB)
- Efficient storage
- Standalone operation (no external dependencies)
- Active community and ecosystem

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Metrics** | Time-series data with name and labels |
| **Labels** | Key-value pairs identifying metrics |
| **Sample** | Single data point: metric value + timestamp |
| **Target** | Endpoint to scrape metrics from |
| **Job** | Group of targets with same purpose |
| **Instance** | Single target in a job |
| **Scrape** | Pull metrics from target at interval |
| **TSDB** | Time-Series Database storing metrics |
| **Exporter** | Program exposing metrics for scraping |
| **Alertmanager** | Handles alert routing and notifications |

---

## 3. Installation & Setup

### Linux Installation (Ubuntu/Debian)

```bash
# Create user and directories
sudo useradd --no-create-home --shell /bin/false prometheus
sudo useradd --no-create-home --shell /bin/false node_exporter

# Create directories
sudo mkdir -p /etc/prometheus
sudo mkdir -p /var/lib/prometheus

# Download Prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.45.0/prometheus-2.45.0.linux-amd64.tar.gz

# Extract
tar xvfz prometheus-2.45.0.linux-amd64.tar.gz

# Copy files
sudo cp prometheus-2.45.0.linux-amd64/prometheus /usr/local/bin/
sudo cp prometheus-2.45.0.linux-amd64/promtool /usr/local/bin/
sudo cp -r prometheus-2.45.0.linux-amd64/consoles /etc/prometheus/
sudo cp -r prometheus-2.45.0.linux-amd64/console_libraries /etc/prometheus/

# Set ownership
sudo chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus

# Verify installation
prometheus --version
```

### macOS Installation

```bash
# Using Homebrew
brew install prometheus

# Start service
brew services start prometheus

# Verify
brew services list | grep prometheus

# View logs
tail -f /usr/local/var/log/prometheus.log
```

### Docker Installation

```bash
# Run Prometheus container
docker run -d \
  --name prometheus \
  -p 9090:9090 \
  -v /etc/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml \
  -v prometheus-storage:/prometheus \
  prom/prometheus:latest \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/prometheus

# With Docker Compose
cat > docker-compose.yml << 'EOF'
version: '3.8'

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus-storage:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--web.console.libraries=/usr/share/prometheus/console_libraries'
      - '--web.console.templates=/usr/share/prometheus/consoles'
    networks:
      - monitoring

  node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    ports:
      - "9100:9100"
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
    command:
      - '--path.procfs=/host/proc'
      - '--path.sysfs=/host/sys'
      - '--collector.filesystem.mount-points-exclude=^/(sys|proc|dev|host|etc)($$|/)'
    networks:
      - monitoring

volumes:
  prometheus-storage:

networks:
  monitoring:
EOF

docker-compose up -d
```

### Windows Installation

```powershell
# Download from
# https://github.com/prometheus/prometheus/releases

# Extract and navigate to folder
cd prometheus-2.45.0.windows-amd64

# Run Prometheus
.\prometheus.exe --config.file=prometheus.yml

# Or create service (PowerShell Admin)
New-Service -Name "Prometheus" `
  -BinaryPathName "C:\prometheus\prometheus.exe --config.file=C:\prometheus\prometheus.yml" `
  -DisplayName "Prometheus" `
  -StartupType Automatic

Start-Service -Name "Prometheus"
```

### Important Port

| Service | Port | Purpose |
|---------|------|---------|
| **Prometheus UI** | 9090 | Web interface & API |
| **Node Exporter** | 9100 | Host metrics |
| **Alertmanager** | 9093 | Alert management |
| **Pushgateway** | 9091 | Metrics gateway |

### Access Prometheus

```
URL: http://localhost:9090

Sections:
- Graph (query interface)
- Alerts (alert status)
- Status (configuration & targets)
- Help (documentation)
```

---

## 4. Configuration

### Main Configuration File

```bash
# Location
/etc/prometheus/prometheus.yml

# Check syntax
promtool check config /etc/prometheus/prometheus.yml

# Reload configuration (without restart)
curl -X POST http://localhost:9090/-/reload
```

### Basic prometheus.yml

```yaml
# Global settings
global:
  scrape_interval: 15s          # Default scrape interval
  evaluation_interval: 15s      # Alert evaluation interval
  external_labels:
    monitor: 'prometheus-main'
    environment: 'production'
  query_log_file: /var/log/prometheus/query.log

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - localhost:9093

# Load alert rules
rule_files:
  - '/etc/prometheus/rules/*.yml'

# Scrape configurations
scrape_configs:
  # Prometheus itself
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  # Node Exporter
  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']
        labels:
          instance: 'server-01'
      - targets: ['192.168.1.10:9100']
        labels:
          instance: 'server-02'
    relabel_configs:
      - source_labels: [__address__]
        target_label: instance

  # Docker daemon
  - job_name: 'docker'
    static_configs:
      - targets: ['localhost:9323']

  # MySQL
  - job_name: 'mysql'
    static_configs:
      - targets: ['localhost:3306']

  # PostgreSQL
  - job_name: 'postgres'
    static_configs:
      - targets: ['localhost:5432']

  # Application
  - job_name: 'app'
    scrape_interval: 5s
    scrape_timeout: 5s
    metrics_path: '/metrics'
    static_configs:
      - targets: ['localhost:8080']
```

### Service Discovery

```yaml
# Kubernetes Service Discovery
scrape_configs:
  - job_name: 'kubernetes-apiservers'
    kubernetes_sd_configs:
      - role: endpoints
    scheme: https
    tls_config:
      ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt

  - job_name: 'kubernetes-pods'
    kubernetes_sd_configs:
      - role: pod
    relabel_configs:
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
        action: keep
        regex: true
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
        action: replace
        target_label: __metrics_path__
        regex: (.+)

# Consul Service Discovery
scrape_configs:
  - job_name: 'consul'
    consul_sd_configs:
      - server: localhost:8500
    relabel_configs:
      - source_labels: [__meta_consul_service]
        target_label: job

# File-based Service Discovery
scrape_configs:
  - job_name: 'file-sd'
    file_sd_configs:
      - files:
          - '/etc/prometheus/targets/*.yml'
        refresh_interval: 30s
```

### Relabeling

```yaml
scrape_configs:
  - job_name: 'relabel-example'
    static_configs:
      - targets: ['localhost:9100']
    relabel_configs:
      # Keep only targets with label region=us
      - source_labels: [__meta_consul_dc]
        action: keep
        regex: us-.*

      # Drop targets matching pattern
      - source_labels: [__meta_consul_service]
        action: drop
        regex: mysql

      # Replace label value
      - source_labels: [__address__]
        target_label: instance
        replacement: 'server-${1}'

      # Map label
      - source_labels: [__meta_consul_service]
        target_label: job

      # Add new label
      - target_label: environment
        replacement: production
```

### Remote Storage

```yaml
# Write to remote storage
remote_write:
  - url: http://localhost:9009/api/v1/push
    queue_config:
      capacity: 10000
      max_shards: 200
      min_shards: 1
      max_samples_per_send: 500
      batch_send_wait: 5s
      min_backoff: 30ms
      max_backoff: 100ms

# Read from remote storage
remote_read:
  - url: http://localhost:9009/api/v1/read
    read_recent: true
```

---

## 5. Scraping & Targets

### View Active Targets

```
GUI Method:
1. Go to http://localhost:9090
2. Click "Status" → "Targets"
3. View all scraping targets
4. Check health status (UP/DOWN)
5. View last scrape time
```

### Test Target Scrape

```bash
# Check if target is accessible
curl http://localhost:9100/metrics

# Check Prometheus target health via API
curl http://localhost:9090/api/v1/targets

# Get specific target info
curl 'http://localhost:9090/api/v1/targets?state=active'

# Curl response format:
# "state": "up" or "down"
# "discoveredLabels": {...}
# "labels": {...}
# "scrapePool": "node"
# "scrapeUrl": "http://localhost:9100/metrics"
# "globalUrl": "..."
# "lastError": ""
# "lastScrape": "2024-01-01T12:00:00Z"
# "lastScrapeDuration": 0.123
```

### Manual Metrics Check

```bash
# Scrape metrics from target manually
curl http://localhost:9100/metrics | grep -E "^(node_cpu|node_memory)"

# Example output:
# node_cpu_seconds_total{cpu="0",mode="idle"} 12345.67
# node_memory_MemAvailable_bytes 8589934592
# node_memory_MemFree_bytes 2147483648

# Parse specific metric
curl http://localhost:9100/metrics | grep "node_load1"

# Check metric help text
curl http://localhost:9100/metrics | grep "^# HELP"
```

### Dynamic Target Management

```bash
# Reload configuration without downtime
curl -X POST http://localhost:9090/-/reload

# Check current configuration
curl http://localhost:9090/api/v1/query?query=up

# Service discovery list
curl http://localhost:9090/service-discovery

# View YAML configuration
curl http://localhost:9090/api/v1/targets?state=active | jq
```

---

## 6. Metrics & Query Language

### Metric Types

```
1. Counter
   - Always increases or resets
   - Example: http_requests_total

2. Gauge
   - Can go up or down
   - Example: temperature_celsius

3. Histogram
   - Counts observations in buckets
   - Example: http_request_duration_seconds

4. Summary
   - Similar to histogram with quantiles
   - Example: http_request_duration_seconds
```

### Metric Naming

```
Format: <namespace>_<subsystem>_<name>_<unit>

Examples:
- http_requests_total       (counter)
- node_cpu_seconds_total    (counter)
- process_resident_memory_bytes (gauge)
- request_duration_seconds  (histogram)

Best Practices:
- Use underscore separator
- Use base units (seconds, bytes)
- Avoid redundant suffixes
- Keep names meaningful
```

### Label Best Practices

```yaml
# Good labeling
up{job="prometheus", instance="localhost:9090"}
http_requests_total{job="api", method="GET", status="200"}
node_cpu_seconds_total{cpu="0", mode="idle"}

# Avoid high cardinality
# Bad: user_id="user_12345" (too many unique values)
# Good: service="api", endpoint="/users"

# Consistent labels across metrics
# Use same label names
# Use same label values
```

---

## 7. PromQL Queries

### GUI Query Interface

```
Steps:
1. Go to http://localhost:9090
2. Click "Graph" tab
3. Enter query in text box
4. Click "Execute" or press Ctrl+Enter
5. View results:
   - Console tab: Table format
   - Graph tab: Time-series chart
6. Adjust time range (top right)
```

### Instant Queries

```promql
# Get current value
up
up{job="prometheus"}
process_resident_memory_bytes

# With label matching
up{job=~"prometheus|node"}
up{job!="alertmanager"}
node_cpu_seconds_total{mode="idle"}

# Multiple conditions
http_requests_total{job="api", status="200"}
```

### Range Queries

```promql
# Last 5 minutes
up[5m]

# Last hour
node_cpu_seconds_total[1h]

# Supported durations: s, m, h, d, w, y
```

### Functions

```promql
# Rate - per-second average rate
rate(http_requests_total[5m])
rate(node_network_receive_bytes_total[1m])

# Increase - total increase
increase(http_requests_total[1h])

# Irate - instantaneous rate
irate(http_requests_total[5m])

# Delta - change over time
delta(temperature_celsius[1h])

# Derivative - per-second derivative
deriv(temperature_celsius[1h])

# Abs - absolute value
abs(temperature_celsius)

# Ceil - round up
ceil(memory_usage_percent)

# Floor - round down
floor(cpu_usage_percent)

# Round - round to nearest
round(metric_value)

# Histogram quantiles
histogram_quantile(0.95, http_request_duration_seconds_bucket)

# Count distinct
count(up)

# Sum all
sum(up)

# Average
avg(node_cpu_seconds_total)

# Maximum
max(node_memory_MemAvailable_bytes)

# Minimum
min(http_response_time_seconds)

# Stddev - standard deviation
stddev(request_duration_seconds)

# Predicting values
predict_linear(cpu_usage[1h], 3600)
```

### Aggregation

```promql
# By job
sum by (job) (up)

# By multiple labels
sum by (job, instance) (http_requests_total)

# Without label
sum without (mode) (node_cpu_seconds_total)

# Top 5 values
topk(5, node_memory_MemAvailable_bytes)

# Bottom 5 values
bottomk(5, http_response_time_seconds)

# Count group
count by (job) (up)

# Quantile
quantile(0.95, http_request_duration_seconds)
```

### Operators

```promql
# Arithmetic operators
node_memory_MemAvailable_bytes / 1024 / 1024 / 1024  # Convert to GB

rate(http_requests_total[5m]) * 60  # Per-minute rate

# Comparison operators
up == 1
http_response_code != 200
cpu_usage_percent > 80
memory_usage_percent < 50
request_duration_ms >= 100

# Logical operators
(cpu_usage_percent > 80) and (memory_usage_percent > 70)
(disk_usage_percent > 90) or (inode_usage_percent > 85)
not (up == 1)

# Boolean operators
http_requests_total > 1000 bool  # Returns 0 or 1
```

### Common Queries

```promql
# System monitoring
# CPU usage
100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)

# Memory usage percentage
100 * (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes))

# Disk usage
100 * (node_filesystem_size_bytes - node_filesystem_avail_bytes) / node_filesystem_size_bytes

# Network I/O
rate(node_network_transmit_bytes_total[5m])
rate(node_network_receive_bytes_total[5m])

# Application monitoring
# Request rate
rate(http_requests_total[5m])

# Error rate
rate(http_requests_total{status=~"5.."}[5m])

# Error rate percentage
100 * rate(http_requests_total{status=~"5.."}[5m]) / rate(http_requests_total[5m])

# Response time
histogram_quantile(0.95, http_request_duration_seconds_bucket)

# Uptime
up * 100  # Shows 100 if up, 0 if down
```

---

## 8. Alerting

### Alert Rules File

```bash
# Location
/etc/prometheus/rules/alerts.yml

# Include in prometheus.yml
rule_files:
  - '/etc/prometheus/rules/*.yml'
```

### Create Alert Rules

```yaml
# /etc/prometheus/rules/alerts.yml

groups:
  - name: system_alerts
    interval: 15s
    rules:
      - alert: HighCPUUsage
        expr: |
          100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 80
        for: 5m
        labels:
          severity: warning
          team: backend
        annotations:
          summary: "High CPU usage detected on {{ $labels.instance }}"
          description: "CPU usage is {{ $value | humanize }}% on {{ $labels.instance }}"

      - alert: HighMemoryUsage
        expr: |
          100 * (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) > 85
        for: 5m
        labels:
          severity: critical
          team: ops
        annotations:
          summary: "High memory usage on {{ $labels.instance }}"
          description: "Memory is {{ $value | humanize }}% utilized"

      - alert: DiskSpaceLow
        expr: |
          100 * (node_filesystem_size_bytes - node_filesystem_avail_bytes) / node_filesystem_size_bytes > 90
        for: 10m
        labels:
          severity: critical
        annotations:
          summary: "Low disk space on {{ $labels.device }}"
          description: "Disk is {{ $value | humanize }}% full"

      - alert: InstanceDown
        expr: up == 0
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "Instance {{ $labels.instance }} is down"
          description: "{{ $labels.instance }} has been unreachable for more than 5 minutes"

      - alert: HighErrorRate
        expr: |
          rate(http_requests_total{status=~"5.."}[5m]) > 0.05
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High error rate detected"
          description: "Error rate is {{ $value | humanizePercentage }}"

      - alert: SlowResponseTime
        expr: |
          histogram_quantile(0.95, http_request_duration_seconds_bucket) > 1
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Slow response times detected"
          description: "95th percentile response time is {{ $value }}s"
```

### Verify Alert Rules

```bash
# Check rule syntax
promtool check rules /etc/prometheus/rules/alerts.yml

# Check all rules
promtool check rules /etc/prometheus/rules/*.yml

# Reload rules without restart
curl -X POST http://localhost:9090/-/reload
```

### View Alerts in GUI

```
Steps:
1. Go to http://localhost:9090
2. Click "Alerts" tab
3. View alert status:
   - Green: OK
   - Yellow: Warning
   - Red: Critical/Firing
4. Check alert details:
   - Rule name
   - Status
   - Value
   - Labels
   - Annotations
```

### Alertmanager Configuration

```yaml
# /etc/alertmanager/alertmanager.yml

global:
  resolve_timeout: 5m
  slack_api_url: 'YOUR_SLACK_WEBHOOK_URL'

route:
  receiver: 'default'
  group_by: ['alertname', 'cluster', 'service']
  group_wait: 10s
  group_interval: 10s
  repeat_interval: 12h
  routes:
    - match:
        severity: critical
      receiver: 'pagerduty'
      continue: true

    - match:
        severity: warning
      receiver: 'slack'

receivers:
  - name: 'default'
    slack_configs:
      - channel: '#alerts'
        title: 'Alert: {{ .GroupLabels.alertname }}'
        text: '{{ range .Alerts }}{{ .Annotations.description }}{{ end }}'

  - name: 'pagerduty'
    pagerduty_configs:
      - service_key: 'YOUR_PAGERDUTY_KEY'

  - name: 'slack'
    slack_configs:
      - channel: '#warnings'
        title: '{{ .GroupLabels.alertname }}'
```

---

## 9. Exporters

### Node Exporter (Host Metrics)

```bash
# Download and install
wget https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz
tar xvfz node_exporter-1.6.1.linux-amd64.tar.gz

# Copy binary
sudo cp node_exporter-1.6.1.linux-amd64/node_exporter /usr/local/bin/
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter

# Create systemd service
sudo tee /etc/systemd/system/node_exporter.service << EOF
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter \
  --collector.filesystem.mount-points-exclude=^/(sys|proc|dev|host|etc)($$|/) \
  --collector.netdev.device-exclude=^(veth.*)$$

Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF

# Start service
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter

# Verify
curl http://localhost:9100/metrics
```

### MySQL Exporter

```bash
# Download
wget https://github.com/prometheus/mysqld_exporter/releases/download/v0.15.0/mysqld_exporter-0.15.0.linux-amd64.tar.gz

# Extract and install
tar xvfz mysqld_exporter-0.15.0.linux-amd64.tar.gz
sudo cp mysqld_exporter-0.15.0.linux-amd64/mysqld_exporter /usr/local/bin/

# Create DSN file
sudo tee /etc/mysqld_exporter/.my.cnf << EOF
[client]
user=exporter
password=exporter_password
host=localhost
EOF

# Systemd service
sudo tee /etc/systemd/system/mysqld_exporter.service << EOF
[Unit]
Description=MySQL Exporter
After=network.target

[Service]
Type=simple
User=mysql
ExecStart=/usr/local/bin/mysqld_exporter --config.my-cnf=/etc/mysqld_exporter/.my.cnf

Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF

# Start
sudo systemctl enable mysqld_exporter
sudo systemctl start mysqld_exporter

# Verify
curl http://localhost:9104/metrics
```

### PostgreSQL Exporter

```bash
# Download
wget https://github.com/prometheus-community/postgres_exporter/releases/download/v0.13.0/postgres_exporter-0.13.0.linux-amd64.tar.gz

# Install
tar xvfz postgres_exporter-0.13.0.linux-amd64.tar.gz
sudo cp postgres_exporter-0.13.0.linux-amd64/postgres_exporter /usr/local/bin/

# Create .env file
sudo tee /etc/postgres_exporter/.env << EOF
DATA_SOURCE_NAME="postgresql://exporter:password@localhost:5432/postgres?sslmode=disable"
EOF

# Systemd service
sudo tee /etc/systemd/system/postgres_exporter.service << EOF
[Unit]
Description=PostgreSQL Exporter
After=network.target

[Service]
Type=simple
User=postgres
EnvironmentFile=/etc/postgres_exporter/.env
ExecStart=/usr/local/bin/postgres_exporter

Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF

# Start
sudo systemctl enable postgres_exporter
sudo systemctl start postgres_exporter

# Verify
curl http://localhost:9187/metrics
```

### Common Exporter Ports

| Exporter | Port | Metrics |
|----------|------|---------|
| Node Exporter | 9100 | Host/System |
| MySQL | 9104 | MySQL database |
| PostgreSQL | 9187 | PostgreSQL |
| Docker | 9323 | Docker stats |
| Redis | 9121 | Redis info |
| MongoDB | 9216 | MongoDB |
| RabbitMQ | 15692 | RabbitMQ |
| Nginx | 9113 | Nginx stats |

---

## 10. Best Practices

### Metric Design

```
1. Naming Convention
   - Use lowercase
   - Use underscore separator
   - Include unit as suffix
   Example: http_request_duration_seconds

2. Label Strategy
   - Keep cardinality low
   - Use consistent label names
   - Avoid user IDs, request IDs
   - Group by service, job, instance
   Example: {job="api", instance="srv-01", method="GET"}

3. Metric Types
   - Counter: Always increases (requests_total)
   - Gauge: Can go up/down (memory_bytes)
   - Histogram: Distribution (request_duration_seconds)
   - Summary: Quantiles (response_time_seconds)
```

### Storage & Performance

```yaml
# prometheus.yml settings
global:
  scrape_interval: 15s        # Default 15-30 seconds
  scrape_timeout: 10s         # Less than scrape_interval
  evaluation_interval: 15s    # Default 15-30 seconds

# TSDB configuration
storage:
  tsdb:
    retention:
      time: 15d              # Retention period
    max_block_duration: 31d  # Block size
    min_block_duration: 2h
```

### Alerting Best Practices

```yaml
rules:
  - alert: HighCPU
    # Use appropriate evaluation period
    for: 5m                  # Wait 5min before alerting
    
    # Clear annotation
    annotations:
      summary: "CPU high on {{ $labels.instance }}"
      description: "CPU is {{ $value }}%"
    
    # Meaningful labels
    labels:
      severity: warning
      team: backend
      runbook: "https://wiki.company.com/cpu-high"
```

### Scraping Strategy

```yaml
scrape_configs:
  # Fast-changing metrics
  - job_name: 'app'
    scrape_interval: 5s
    scrape_timeout: 5s

  # Stable metrics
  - job_name: 'infrastructure'
    scrape_interval: 30s

  # Heavy queries
  - job_name: 'database'
    scrape_interval: 60s
    scrape_timeout: 30s
```

### Security

```yaml
# Require authentication
scrape_configs:
  - job_name: 'secure'
    scheme: https
    tls_config:
      ca_file: /etc/prometheus/ca.crt
      cert_file: /etc/prometheus/cert.pem
      key_file: /etc/prometheus/key.pem
    basic_auth:
      username: 'user'
      password: 'password'

# Reverse proxy (nginx)
location /prometheus/ {
  proxy_pass http://localhost:9090/;
  auth_basic "Prometheus";
  auth_basic_user_file /etc/nginx/.htpasswd;
}
```

---

## 11. Troubleshooting

### Common Issues

```bash
# Issue: Target down
# Check: Is exporter running?
curl http://target:9100/metrics

# Issue: No metrics appearing
# Check: Scrape interval and evaluation time
curl http://localhost:9090/api/v1/targets

# Check specific target
curl http://localhost:9090/api/v1/targets?state=active | jq '.data.activeTargets[] | select(.labels.job=="node")'

# Issue: High memory usage
# Solution: Reduce retention period
# Edit prometheus.yml:
storage:
  tsdb:
    retention:
      time: 7d  # Reduce from 15d

sudo systemctl restart prometheus

# Check Prometheus logs
tail -f /var/log/prometheus/prometheus.log

# Verify configuration
promtool check config /etc/prometheus/prometheus.yml

# Check rules
promtool check rules /etc/prometheus/rules/*.yml

# Query API for errors
curl http://localhost:9090/api/v1/status/tsdb
```

### Performance Optimization

```bash
# Monitor Prometheus itself
# Query Prometheus metrics about Prometheus
prometheus_tsdb_symbol_table_size_bytes
prometheus_tsdb_index_page_size_bytes
prometheus_http_request_duration_seconds

# Check cardinality
curl http://localhost:9090/api/v1/query?query='count({__name__=~".+"})'

# Check series count
curl http://localhost:9090/api/v1/query?query='count(count by (__name__) ({__name__=~".+"}))'

# Identify high cardinality metrics
promtool query instant 'topk(10, count by (__name__) ({__name__=~".+"}))'
```

### API Debugging

```bash
# Test instant query
curl 'http://localhost:9090/api/v1/query?query=up'

# Test range query
curl 'http://localhost:9090/api/v1/query_range?query=up&start=1609459200&end=1609545600&step=60'

# Get targets
curl http://localhost:9090/api/v1/targets

# Get series
curl 'http://localhost:9090/api/v1/series?match[]=up'

# Query label values
curl 'http://localhost:9090/api/v1/label/job/values'

# Query label names
curl 'http://localhost:9090/api/v1/labels'
```

---

## 12. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://prometheus.io/docs/prometheus/latest/getting_started/ |
| **Configuration** | https://prometheus.io/docs/prometheus/latest/configuration/configuration/ |
| **PromQL** | https://prometheus.io/docs/prometheus/latest/querying/basics/ |
| **API** | https://prometheus.io/docs/prometheus/latest/querying/api/ |
| **Exporters** | https://prometheus.io/docs/instrumenting/exporters/ |
| **Alerting** | https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Official Site** | https://prometheus.io/ |
| **Tutorials** | https://prometheus.io/docs/tutorials/first_steps/ |
| **Community** | https://prometheus.io/community/ |
| **GitHub** | https://github.com/prometheus/prometheus |
| **Stack Overflow** | https://stackoverflow.com/questions/tagged/prometheus |

---

## Quick Reference

```bash
# Start Prometheus
prometheus --config.file=prometheus.yml

# With systemd
sudo systemctl start prometheus
sudo systemctl status prometheus

# Check syntax
promtool check config prometheus.yml
promtool check rules rules.yml

# Reload configuration
curl -X POST http://localhost:9090/-/reload

# Query CLI
promtool query instant 'up'
promtool query range 'rate(http_requests_total[5m])' --start=2024-01-01T00:00:00Z --end=2024-01-02T00:00:00Z

# View targets
curl http://localhost:9090/api/v1/targets

# View alerts
curl http://localhost:9090/api/v1/alerts

# Access UI
# http://localhost:9090

# Check metrics directly
curl http://localhost:9090/metrics

# Docker commands
docker run -p 9090:9090 prom/prometheus
docker logs -f prometheus
docker restart prometheus

# Check listening ports
sudo netstat -tulpn | grep 9090
sudo lsof -i :9090
```

---

## File Locations

| Item | Path |
|------|------|
| **Config** | /etc/prometheus/prometheus.yml |
| **Rules** | /etc/prometheus/rules/*.yml |
| **Data** | /var/lib/prometheus/ |
| **Logs** | /var/log/prometheus/prometheus.log |
| **Systemd** | /etc/systemd/system/prometheus.service |
| **Binary** | /usr/local/bin/prometheus |

---

**Last Updated:** 2026
**Prometheus Version:** 2.45.x+
**License:** Apache 2.0

For latest information, visit [Prometheus Official Site](https://prometheus.io/)