# Loki Cheatsheet

![Grafana Loki Logo](https://grafana.com/static/assets/img/branding/grafana_loki_icon.svg)

---

## Table of Contents
1. [What is Loki?](#1-what-is-loki)
2. [Core Concepts](#2-core-concepts)
3. [Installation & Setup](#3-installation--setup)
4. [Configuration](#4-configuration)
5. [Log Collection (Promtail)](#5-log-collection-promtail)
6. [Queries (LogQL)](#6-queries-logql)
7. [Retention & Storage](#7-retention--storage)
8. [Loki with Grafana](#8-loki-with-grafana)
9. [Best Practices](#9-best-practices)
10. [Troubleshooting](#10-troubleshooting)
11. [Additional Resources](#11-additional-resources)

---

## 1. What is Loki?

Loki is a log aggregation system designed by Grafana Labs that's inspired by Prometheus. It's optimized for efficiency and cost-effectiveness, storing only log labels and indexing them for fast querying without full-text indexing of the entire log content.

**Key Benefits:**
- Efficient resource usage
- Cost-effective log storage
- LogQL query language
- Built-in Grafana integration
- Horizontal scalability
- Multi-tenant support
- Label-based indexing
- No full-text search overhead

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Loki** | Log aggregation backend and database |
| **Promtail** | Log collection agent (like Filebeat for Prometheus) |
| **LogQL** | Query language for logs (Prometheus-inspired) |
| **Stream** | Set of logs identified by labels |
| **Labels** | Key-value pairs identifying log streams |
| **Log Line** | Individual log message |
| **Chunk** | Compressed block of log lines |
| **Ingester** | Component handling incoming log data |
| **Querier** | Component handling log queries |
| **Table** | Time-based storage division |

---

## 3. Installation & Setup

### Linux Installation (Ubuntu/Debian)

```bash
# Create user
sudo useradd --no-create-home --shell /bin/false loki

# Download Loki
wget https://github.com/grafana/loki/releases/download/v2.9.0/loki-linux-amd64.zip

# Extract
unzip loki-linux-amd64.zip
sudo mv loki-linux-amd64 /usr/local/bin/loki
sudo chmod +x /usr/local/bin/loki

# Create config directory
sudo mkdir -p /etc/loki
sudo mkdir -p /var/lib/loki

# Set permissions
sudo chown -R loki:loki /etc/loki /var/lib/loki

# Verify installation
loki --version
```

### macOS Installation

```bash
# Using Homebrew
brew tap grafana/loki
brew install loki

# Or download manually
wget https://github.com/grafana/loki/releases/download/v2.9.0/loki-darwin-amd64.zip
unzip loki-darwin-amd64.zip
chmod +x loki-darwin-amd64

# Start service
brew services start loki

# Verify
brew services list | grep loki
```

### Docker Installation

```bash
# Run Loki container
docker run -d \
  --name loki \
  -p 3100:3100 \
  -v loki-storage:/loki \
  grafana/loki:latest

# Run with config file
docker run -d \
  --name loki \
  -p 3100:3100 \
  -v /etc/loki/loki-config.yml:/etc/loki/local-config.yml \
  -v loki-storage:/loki \
  grafana/loki:latest \
  -config.file=/etc/loki/local-config.yml
```

### Docker Compose Setup

```yaml
version: '3.8'

services:
  loki:
    image: grafana/loki:latest
    container_name: loki
    ports:
      - "3100:3100"
    volumes:
      - ./loki-config.yml:/etc/loki/local-config.yml
      - loki-storage:/loki
    command: -config.file=/etc/loki/local-config.yml
    networks:
      - monitoring

  promtail:
    image: grafana/promtail:latest
    container_name: promtail
    volumes:
      - ./promtail-config.yml:/etc/promtail/config.yml
      - /var/log:/var/log:ro
      - /var/lib/docker/containers:/var/lib/docker/containers:ro
      - /var/run/docker.sock:/var/run/docker.sock
    command: -config.file=/etc/promtail/config.yml
    depends_on:
      - loki
    networks:
      - monitoring

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    depends_on:
      - loki
    networks:
      - monitoring

volumes:
  loki-storage:

networks:
  monitoring:
```

### Systemd Service

```ini
# /etc/systemd/system/loki.service
[Unit]
Description=Loki log aggregation system
After=network.target

[Service]
Type=simple
User=loki
Group=loki
ExecStart=/usr/local/bin/loki -config.file=/etc/loki/loki-config.yml
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

```bash
# Enable and start service
sudo systemctl daemon-reload
sudo systemctl enable loki
sudo systemctl start loki

# Check status
sudo systemctl status loki
```

### Important Ports

| Component | Port | Purpose |
|-----------|------|---------|
| **Loki API** | 3100 | Log ingestion & querying |
| **Loki Distributor** | 3100 | Log entry point |
| **Loki Querier** | 3100 | Query interface |
| **Promtail** | 9080 | Metrics endpoint (optional) |

### Verify Installation

```bash
# Check Loki health
curl -s http://localhost:3100/ready
# Output: ready

# Check Loki metrics
curl -s http://localhost:3100/metrics | head -20

# Get version info
curl -s http://localhost:3100/api/prom/label
```

---

## 4. Configuration

### Basic loki-config.yml

```yaml
auth_enabled: false

ingester:
  chunk_idle_period: 3m
  max_chunk_age: 1h
  max_streams_per_user: 10000
  chunk_retain_period: 1m
  max_chunk_bytes: 1000000
  enforce_rate_limit_no_limits: false
  rate_limit_enabled: true
  rate_limit: 6
  rate_limit_burst: 15

limits_config:
  enforce_metric_name: false
  reject_old_samples: true
  reject_old_samples_max_age: 168h
  ingestion_rate_mb: 10
  ingestion_burst_size_mb: 20
  max_cache_freshness_per_query: 10m
  split_queries_by_interval: 24h
  max_streams_matchers_per_query: 20000
  max_global_streams_per_user: 10000

schema_config:
  configs:
    - from: 2020-10-24
      store: boltdb-shipper
      object_store: filesystem
      schema: v11
      index:
        prefix: index_
        period: 24h

server:
  http_listen_port: 3100
  http_server_read_timeout: 600s
  http_server_write_timeout: 600s
  log_level: info
  log_format: json

storage_config:
  boltdb_shipper:
    shared_store: filesystem
    active_index_directory: /loki/boltdb-shipper-active
    shared_store_type: filesystem
  filesystem:
    directory: /loki/chunks

chunk_store_config:
  max_look_back_period: 0s

table_manager:
  retention_deletes_enabled: true
  retention_period: 744h
  poll_interval: 10m
```

### Multi-Tenant Configuration

```yaml
auth_enabled: true

ingester:
  chunk_idle_period: 3m
  max_chunk_age: 1h
  max_streams_per_user: 10000

limits_config:
  # Per-tenant limits
  ingestion_rate_mb: 10
  ingestion_burst_size_mb: 20
  max_streams_per_user: 1000
  max_global_streams_per_user: 10000
  retention_period: 744h

server:
  http_listen_port: 3100

storage_config:
  boltdb_shipper:
    shared_store: filesystem
  filesystem:
    directory: /loki/chunks

auth:
  type: singletenant
```

### Production Configuration

```yaml
auth_enabled: true

ingester:
  chunk_idle_period: 3m
  max_chunk_age: 1h
  max_streams_per_user: 10000
  chunk_retain_period: 1m
  lifecycler:
    ring:
      kvstore:
        store: consul
      replication_factor: 3

distributor:
  ring:
    kvstore:
      store: consul

limits_config:
  enforce_metric_name: false
  reject_old_samples: true
  reject_old_samples_max_age: 168h
  ingestion_rate_mb: 50
  ingestion_burst_size_mb: 100
  max_streams_per_user: 10000
  retention_period: 2160h  # 90 days

schema_config:
  configs:
    - from: 2020-10-24
      store: cassandra
      object_store: s3
      schema: v11
      index:
        prefix: index_
        period: 24h

server:
  http_listen_port: 3100
  grpc_listen_port: 9096
  http_server_read_timeout: 600s
  http_server_write_timeout: 600s
  log_level: info
  log_format: json

storage_config:
  cassandra:
    addresses: cassandra-1,cassandra-2,cassandra-3
    keyspace: loki
  s3:
    s3: s3://my-bucket
    endpoint: s3.amazonaws.com
    access_key_id: ${AWS_ACCESS_KEY}
    secret_access_key: ${AWS_SECRET_KEY}
```

### Check Configuration

```bash
# Syntax check
loki -config.file=loki-config.yml -verify-config

# Reload configuration (requires HTTP API)
curl -X POST http://localhost:3100/-/reload

# View current config
curl http://localhost:3100/api/prom/config
```

---

## 5. Log Collection (Promtail)

### Download Promtail

```bash
# Download
wget https://github.com/grafana/loki/releases/download/v2.9.0/promtail-linux-amd64.zip

# Extract
unzip promtail-linux-amd64.zip
sudo mv promtail-linux-amd64 /usr/local/bin/promtail
sudo chmod +x /usr/local/bin/promtail

# Verify
promtail --version
```

### Basic promtail-config.yml

```yaml
server:
  http_listen_port: 9080
  grpc_listen_port: 0
  log_level: info

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://localhost:3100/loki/api/v1/push

scrape_configs:
  # Syslog
  - job_name: syslog
    static_configs:
      - targets:
          - localhost
        labels:
          job: syslog
          __path__: /var/log/syslog

  # Application logs
  - job_name: application
    static_configs:
      - targets:
          - localhost
        labels:
          job: myapp
          service: api
          environment: production
          __path__: /var/log/myapp/*.log

  # Nginx access logs
  - job_name: nginx
    static_configs:
      - targets:
          - localhost
        labels:
          job: nginx
          __path__: /var/log/nginx/access.log
    pipeline_stages:
      - regex:
          expression: '^(?P<remote>[^ ]*) (?P<host>[^ ]*) (?P<user>[^ ]*) \[(?P<time>[^\]]*)\] "(?P<method>\S+)(?: +(?P<path>[^\"]*) +\S*)?" (?P<status>[^ ]*) (?P<size>[^ ]*)(?: "(?P<referer>[^\"]*)" "(?P<agent>[^\"]*)")??$'
      - labels:
          status:
          method:

  # Docker logs
  - job_name: docker
    docker_sd_configs:
      - host: unix:///var/run/docker.sock
    relabel_configs:
      - source_labels: ['__meta_docker_container_name']
        target_label: 'container'
      - source_labels: ['__meta_docker_container_label_com_docker_compose_service']
        target_label: 'service'
```

### Advanced Promtail Configuration

```yaml
server:
  http_listen_port: 9080

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://localhost:3100/loki/api/v1/push
    tenant_id: tenant-1
    batchwait: 1s
    batchsize: 10485760  # 10MB

scrape_configs:
  - job_name: application
    static_configs:
      - targets: [localhost]
        labels:
          job: myapp
          __path__: /var/log/app.log

    # Pipeline stages for log processing
    pipeline_stages:
      # Parse JSON logs
      - json:
          expressions:
            level: level
            msg: message
            timestamp: timestamp

      # Add labels from parsed fields
      - labels:
          level:
          msg:

      # Timestamp extraction
      - timestamp:
          source: timestamp
          format: RFC3339

      # Drop logs matching pattern
      - drop:
          expression: '.*health.*'

      # Metric extraction
      - metrics:
          log_lines_total:
            type: Counter
            description: "Total log lines"
            prefix: "app_"
          log_level_total:
            type: Counter
            description: "Log lines by level"
            prefix: "app_"
            labels:
              level:

      # Output
      - output:
          source: msg
```

### Pipeline Stage Examples

```yaml
# 1. JSON parsing
- json:
    expressions:
      timestamp: timestamp
      level: level
      message: message

# 2. Regex parsing
- regex:
    expression: '(?P<level>\w+) (?P<message>.*)'

# 3. Add static label
- static_labels:
    environment: production
    region: us-east-1

# 4. Copy label
- labels:
    level:
    service:

# 5. Replace value
- replace:
    expression: '(?P<message>.*)'
    replace: 'Modified: {{ .message }}'

# 6. Filter lines
- drop:
    expression: '.*health.*'

# 7. Extract timestamp
- timestamp:
    source: timestamp
    format: 'Jan 2 15:04:05'

# 8. Add metric
- metrics:
    log_count:
      type: Counter
      description: "Count of logs"
      action: add
      value: "1"
```

### Systemd Service for Promtail

```ini
# /etc/systemd/system/promtail.service
[Unit]
Description=Promtail log collector
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/promtail -config.file=/etc/loki/promtail-config.yml
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

```bash
# Enable and start
sudo systemctl daemon-reload
sudo systemctl enable promtail
sudo systemctl start promtail

# Check status
sudo systemctl status promtail
tail -f /var/log/syslog | grep promtail
```

---

## 6. Queries (LogQL)

### GUI Query Interface

```
Steps in Grafana:
1. Add data source: Loki (http://localhost:3100)
2. Go to Explore → Select Loki
3. Write LogQL query
4. Click "Run query" (Ctrl+Enter)
5. View results
```

### Basic LogQL Queries

```logql
-- Select all logs from job
{job="myapp"}

-- Select with multiple labels
{job="myapp", environment="prod"}

-- Select with label regex
{job=~"myapp|api"}
{environment!="dev"}

-- Select logs containing text
{job="myapp"} |= "error"

-- Exclude logs containing text
{job="myapp"} != "health"

-- Case-insensitive search
{job="myapp"} |= "(?i)warning"

-- Get log count
count_over_time({job="myapp"}[5m])

-- Get rate
rate({job="myapp"}[5m])

-- Get bytes rate
rate({job="myapp"}[5m] | bytes)
```

### Log Parsing

```logql
-- Parse JSON logs
{job="myapp"} | json

-- Parse JSON and filter
{job="myapp"} | json | level="error"

-- Parse with regex
{job="myapp"} | regex "(?P<method>\w+) (?P<path>/\S+) (?P<status>\d+)"

-- Extract and filter
{job="myapp"} | json level="level" | level="ERROR"

-- Multiple parsing stages
{job="myapp"} 
  | json 
  | timestamp="timestamp" 
  | status="status_code" 
  | status>=500
```

### Filtering Examples

```logql
-- Contains
{job="api"} |= "error"

-- Does not contain
{job="api"} != "health"

-- Regex match
{job="api"} |~ "error.*timeout"

-- Regex not match
{job="api"} !~ "info"

-- Multiple conditions
{job="api"} |= "error" |= "database"

-- IP address
{job="api"} |~ "client_ip=\d+\.\d+\.\d+\.\d+"

-- Numeric comparison
{job="api"} | json | duration > 1000
```

### Metrics from Logs

```logql
-- Count logs (instant)
count({job="myapp"})

-- Count over time
count_over_time({job="myapp"}[5m])

-- Bytes per second
rate({job="myapp"}[5m] | bytes)

-- Rate of errors
rate({job="myapp"} |= "error"[5m])

-- Bytes total
bytes_over_time({job="myapp"}[5m])

-- Last byte offset
last_over_time({job="myapp"}[5m] | bytes)
```

### Label Extraction

```logql
-- Extract from JSON
{job="myapp"} | json user_id="userId", level="level"

-- Extract with regex
{job="myapp"} | regex "user_id=(?P<user>\d+)"

-- Add static label
{job="myapp"} | json | line_format "{{ .message }}" | pattern "<_>"

-- Keep specific labels
{job="myapp"} | keep user_id, level

-- Drop specific labels
{job="myapp"} | drop request_id
```

### Common Query Examples

```logql
-- Error logs in last hour
{job="api", environment="prod"} |= "error" | json
last 1h
LIMIT 100

-- Request latency percentiles
{job="api"} | json duration=duration_ms
| stats 
  p50(duration) as p50,
  p95(duration) as p95,
  p99(duration) as p99

-- Logs with specific status code
{job="nginx"} | regex "status=(?P<status>\d+)" | status="500"

-- Count by level
{job="app"} | json level="level"
| stats count() by level

-- Requests per second by endpoint
{job="api"} | regex "path=(?P<path>\S+)"
| stats rate(count()) by path
```

---

## 7. Retention & Storage

### Set Retention Policy

```yaml
# In loki-config.yml
limits_config:
  retention_period: 744h  # 31 days

table_manager:
  retention_deletes_enabled: true
  retention_period: 744h
  poll_interval: 10m
```

### Storage Backends

```yaml
# Filesystem (default, for testing)
storage_config:
  filesystem:
    directory: /loki/chunks

# S3
storage_config:
  s3:
    s3: s3://my-bucket/loki/
    endpoint: s3.amazonaws.com
    access_key_id: ${AWS_ACCESS_KEY}
    secret_access_key: ${AWS_SECRET_KEY}

# GCS
storage_config:
  gcs:
    bucket_name: my-bucket
    project_id: my-project

# Azure
storage_config:
  azure:
    account_name: myaccount
    account_key: ${AZURE_KEY}
    container_name: logs
```

### Index Storage

```yaml
schema_config:
  configs:
    - from: 2020-10-24
      store: boltdb-shipper
      object_store: filesystem
      schema: v11
      index:
        prefix: index_
        period: 24h
```

### Compression & Size

```yaml
ingester:
  # Chunk configuration
  chunk_idle_period: 3m
  max_chunk_age: 1h
  max_chunks_per_user: 100000
  max_chunk_bytes: 1000000  # 1MB
  chunk_encoding: snappy  # or gzip

limits_config:
  # Ingestion limits
  ingestion_rate_mb: 10
  ingestion_burst_size_mb: 20
```

---

## 8. Loki with Grafana

### Add Loki Data Source

```
GUI Steps:
1. Grafana → Configuration → Data Sources
2. Click "Add data source"
3. Select "Loki"
4. Configure:
   - Name: Loki
   - URL: http://localhost:3100
   - Access: Server (default)
5. Click "Save & Test"
```

### Query in Grafana

```
Steps:
1. Go to Explore
2. Select data source: Loki
3. Select job from dropdown
4. Write LogQL query
5. Click "Run query"
6. View results in Logs tab
```

### Create Loki Dashboard

```
Steps:
1. Create new dashboard
2. Add panel
3. Data source: Loki
4. Query type: Logs
5. Write LogQL query
6. Select visualization:
   - Logs (table format)
   - Stat (count)
   - Graph (metrics from logs)
7. Save panel
```

### Panel Examples

```
1. Error count panel
   Query: count_over_time({job="api"} |= "error"[5m])
   Visualization: Stat
   Units: short

2. Log table
   Query: {job="api"} | json
   Visualization: Logs
   Show: Last 100 lines

3. Request duration graph
   Query: {job="api"} | json duration=duration_ms
   Visualization: Time series
```

### Alert from Loki

```
Steps:
1. Edit panel with Loki query
2. Go to Alert tab
3. Click "Create alert"
4. Set threshold:
   - Evaluate: Every 1m
   - For: 5m
   - Condition: count > 100
5. Add notification
6. Save alert
```

---

## 9. Best Practices

### Label Strategy

```yaml
# Recommended labels
job: myapp              # Application name
instance: server-01     # Server/container
environment: prod       # Environment
service: api           # Service name
level: error           # Log level (from parsing)
namespace: default     # Kubernetes namespace

# Rules:
# 1. Keep cardinality low
# 2. Avoid high cardinality labels (user_id, request_id)
# 3. Use consistent naming
# 4. Group by service/job/instance

# Good
{job="api", instance="prod-01", method="GET"}

# Bad
{job="api", user_id="12345", request_id="xyz"}
```

### Log Format

```json
{
  "timestamp": "2024-01-01T12:00:00Z",
  "level": "ERROR",
  "service": "api",
  "message": "Database connection failed",
  "duration_ms": 250,
  "status_code": 500,
  "user_id": 12345,
  "request_id": "abc-123-def"
}
```

### Performance Optimization

```yaml
# Chunk settings
ingester:
  chunk_idle_period: 3m
  max_chunk_age: 1h
  max_chunk_bytes: 1000000

# Limits
limits_config:
  ingestion_rate_mb: 10
  max_streams_per_user: 1000

# Retention
table_manager:
  retention_period: 744h  # 31 days
```

### Scalability

```yaml
# Horizontal scaling
distributor:
  ring:
    kvstore:
      store: consul

ingester:
  lifecycler:
    ring:
      kvstore:
        store: consul
      replication_factor: 3

# Query optimization
limits_config:
  split_queries_by_interval: 24h
  max_cache_freshness_per_query: 10m
```

---

## 10. Troubleshooting

### Common Issues

```bash
# Issue: Logs not appearing in Loki
# Solution 1: Check Promtail is running
sudo systemctl status promtail
tail -f /var/log/syslog | grep promtail

# Solution 2: Check Promtail config
promtail -config.file=promtail-config.yml -verify-config

# Solution 3: Verify file permissions
ls -la /var/log/myapp/

# Check Loki health
curl -s http://localhost:3100/ready

# Check target labels in Promtail
curl -s http://localhost:9080/api/prom/targets

# Issue: High memory usage
# Solution: Reduce ingestion rate
limits_config:
  ingestion_rate_mb: 5

# Issue: Slow queries
# Solution: Add time range filter
{job="api"} [5m]
```

### Debug Mode

```bash
# Start Loki in debug mode
loki -config.file=loki-config.yml -debug

# Start Promtail in debug mode
promtail -config.file=promtail-config.yml -debug

# Check logs
tail -f /var/log/loki.log
tail -f /var/log/promtail.log
```

### API Debugging

```bash
# Check API endpoints
curl -s http://localhost:3100/loki/api/v1/labels

# Get label values
curl -s 'http://localhost:3100/loki/api/v1/label/job/values'

# Push test log
curl -X POST -H "Content-Type: application/json" \
  -d '{"streams":[{"stream":{"job":"test"},"values":[["'$(date +%s%N)'","test log line"]]}]}' \
  http://localhost:3100/loki/api/v1/push

# Query logs via API
curl -G -s http://localhost:3100/loki/api/v1/query \
  --data-urlencode 'query={job="myapp"}'
```

### Performance Monitoring

```bash
# Check Loki metrics
curl -s http://localhost:3100/metrics | grep -E "loki_ingester|loki_querier"

# Monitor ingestion rate
curl -s http://localhost:3100/metrics | grep loki_ingester_chunks

# Check chunk size
curl -s http://localhost:3100/metrics | grep loki_ingester_chunk_size
```

---

## 11. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Loki Docs** | https://grafana.com/docs/loki/latest/ |
| **Installation** | https://grafana.com/docs/loki/latest/installation/ |
| **Configuration** | https://grafana.com/docs/loki/latest/configuration/ |
| **LogQL** | https://grafana.com/docs/loki/latest/query/ |
| **Promtail** | https://grafana.com/docs/loki/latest/clients/promtail/ |
| **API** | https://grafana.com/docs/loki/latest/api/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Getting Started** | https://grafana.com/docs/loki/latest/getting-started/ |
| **Loki Blog** | https://grafana.com/blog/tags/loki/ |
| **Community** | https://community.grafana.com/ |
| **GitHub** | https://github.com/grafana/loki |

---

## Quick Reference

```bash
# Start Loki
loki -config.file=loki-config.yml

# Start Promtail
promtail -config.file=promtail-config.yml

# With systemd
sudo systemctl start loki
sudo systemctl start promtail

# Check health
curl http://localhost:3100/ready

# Check targets
curl http://localhost:9080/api/prom/targets

# View metrics
curl http://localhost:3100/metrics

# Query logs via API
curl 'http://localhost:3100/loki/api/v1/query?query={job="myapp"}'

# Docker
docker run -p 3100:3100 grafana/loki:latest

# Docker Compose
docker-compose up -d

# Check logs
docker logs -f loki
docker logs -f promtail

# Reload config
curl -X POST http://localhost:3100/-/reload

# Verify config
loki -config.file=loki-config.yml -verify-config
promtail -config.file=promtail-config.yml -verify-config
```

---

## File Locations

| Item | Path |
|------|------|
| **Loki Binary** | /usr/local/bin/loki |
| **Loki Config** | /etc/loki/loki-config.yml |
| **Promtail Binary** | /usr/local/bin/promtail |
| **Promtail Config** | /etc/loki/promtail-config.yml |
| **Positions File** | /tmp/positions.yaml |
| **Storage** | /loki/chunks |
| **Index** | /loki/boltdb-shipper-active |
| **Systemd Services** | /etc/systemd/system/loki.service |
| **Logs** | /var/log/loki/ |

---

**Last Updated:** 2026
**Loki Version:** 2.9.x+
**License:** AGPL v3

For latest information, visit [Grafana Loki Documentation](https://grafana.com/docs/loki/)