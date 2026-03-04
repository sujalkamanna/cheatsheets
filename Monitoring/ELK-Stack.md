# ELK Stack Cheatsheet

![ELK Stack Logo](https://www.elastic.co/content/images/merch/elastic-logo-social-400x400.png)

---

## Table of Contents
1. [What is ELK Stack?](#1-what-is-elk-stack)
2. [Core Concepts](#2-core-concepts)
3. [Installation & Setup](#3-installation--setup)
4. [Elasticsearch](#4-elasticsearch)
5. [Logstash](#5-logstash)
6. [Kibana](#6-kibana)
7. [Beats](#7-beats)
8. [Best Practices](#8-best-practices)
9. [Troubleshooting](#9-troubleshooting)
10. [Additional Resources](#10-additional-resources)

---

## 1. What is ELK Stack?

ELK Stack is a collection of three open-source products: Elasticsearch, Logstash, and Kibana. It provides a powerful platform for searching, analyzing, and visualizing data in real-time from various sources.

**Key Benefits:**
- Centralized log management
- Real-time data analysis
- Full-text search capabilities
- Custom dashboards and visualizations
- Scalable architecture
- Cost-effective solution
- Easy integration with multiple data sources
- Powerful query language (Query DSL)

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Elasticsearch** | Search and analytics engine storing indexed data |
| **Logstash** | Data processing pipeline for ingesting and transforming logs |
| **Kibana** | Visualization platform for exploring and analyzing data |
| **Beats** | Lightweight data shippers for various data types |
| **Index** | Collection of documents with similar characteristics |
| **Document** | Single data record in JSON format |
| **Shard** | Subdivision of an index for distributed storage |
| **Replica** | Copy of a shard for redundancy |
| **Node** | Single Elasticsearch instance |
| **Cluster** | Group of interconnected nodes |

---

## 3. Installation & Setup

### Elasticsearch Installation

```bash
# macOS (Homebrew)
brew tap elastic/tap
brew install elastic/tap/elasticsearch-full
brew services start elastic/tap/elasticsearch-full

# Linux (Ubuntu/Debian)
sudo apt-get update
sudo apt-get install -y curl gnupg2 lsb-release ubuntu-keyring

curl https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | \
  sudo tee /etc/apt/sources.list.d/elastic-8.x.list

sudo apt-get update
sudo apt-get install -y elasticsearch

# Start service
sudo systemctl start elasticsearch
sudo systemctl enable elasticsearch

# Windows (Download)
# Download from https://www.elastic.co/downloads/elasticsearch
# Extract and run: bin\elasticsearch.bat
```

### Logstash Installation

```bash
# macOS
brew install logstash

# Linux (Ubuntu/Debian)
sudo apt-get install -y logstash

# Start service
sudo systemctl start logstash
sudo systemctl enable logstash
```

### Kibana Installation

```bash
# macOS
brew install kibana

# Linux (Ubuntu/Debian)
sudo apt-get install -y kibana

# Start service
sudo systemctl start kibana
sudo systemctl enable kibana

# Access Kibana
# http://localhost:5601
```

### Docker Compose Setup

```yaml
version: '3.8'

services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.0.0
    environment:
      - discovery.type=single-node
      - xpack.security.enabled=false
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ports:
      - "9200:9200"
    volumes:
      - es_data:/usr/share/elasticsearch/data

  logstash:
    image: docker.elastic.co/logstash/logstash:8.0.0
    volumes:
      - ./logstash.conf:/usr/share/logstash/pipeline/logstash.conf
    ports:
      - "5000:5000"
      - "9600:9600"
    depends_on:
      - elasticsearch

  kibana:
    image: docker.elastic.co/kibana/kibana:8.0.0
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch

volumes:
  es_data:
```

### Run with Docker Compose

```bash
docker-compose up -d
```

---

## 4. Elasticsearch

### Basic Operations

```bash
# Check cluster health
curl -X GET "localhost:9200/_cluster/health?pretty"

# Get cluster info
curl -X GET "localhost:9200/_cat/nodes?v"

# List indices
curl -X GET "localhost:9200/_cat/indices?v"

# Create index
curl -X PUT "localhost:9200/my-index-2024.01.01"

# Delete index
curl -X DELETE "localhost:9200/my-index-2024.01.01"

# Get index settings
curl -X GET "localhost:9200/my-index-2024.01.01/_settings"
```

### Index Configuration

```bash
# Create index with settings
curl -X PUT "localhost:9200/my-index" \
  -H "Content-Type: application/json" \
  -d '{
    "settings": {
      "number_of_shards": 3,
      "number_of_replicas": 1,
      "analysis": {
        "analyzer": {
          "my_analyzer": {
            "type": "standard"
          }
        }
      }
    },
    "mappings": {
      "properties": {
        "timestamp": { "type": "date" },
        "level": { "type": "keyword" },
        "message": { "type": "text" },
        "host": { "type": "keyword" },
        "service": { "type": "keyword" }
      }
    }
  }'
```

### Document Operations

```bash
# Index document
curl -X POST "localhost:9200/my-index/_doc" \
  -H "Content-Type: application/json" \
  -d '{
    "timestamp": "2024-01-01T12:00:00Z",
    "level": "ERROR",
    "message": "Database connection failed",
    "host": "web-01",
    "service": "api"
  }'

# Get document
curl -X GET "localhost:9200/my-index/_doc/1"

# Update document
curl -X POST "localhost:9200/my-index/_update/1" \
  -H "Content-Type: application/json" \
  -d '{
    "doc": {
      "level": "RESOLVED"
    }
  }'

# Delete document
curl -X DELETE "localhost:9200/my-index/_doc/1"

# Bulk operations
curl -X POST "localhost:9200/_bulk" \
  -H "Content-Type: application/json" \
  -d @bulk.json
```

### bulk.json Example

```json
{ "index" : { "_index" : "my-index", "_id" : "1" } }
{ "timestamp": "2024-01-01T12:00:00Z", "level": "ERROR", "message": "Error 1" }
{ "index" : { "_index" : "my-index", "_id" : "2" } }
{ "timestamp": "2024-01-01T12:01:00Z", "level": "WARN", "message": "Warning 1" }
```

### Search Queries

```bash
# Simple search
curl -X GET "localhost:9200/my-index/_search" \
  -H "Content-Type: application/json" \
  -d '{
    "query": {
      "match": {
        "message": "error"
      }
    }
  }'

# Filter query
curl -X GET "localhost:9200/my-index/_search" \
  -H "Content-Type: application/json" \
  -d '{
    "query": {
      "bool": {
        "must": [
          { "match": { "level": "ERROR" } }
        ],
        "filter": [
          { "range": { "timestamp": { "gte": "2024-01-01" } } }
        ]
      }
    }
  }'

# Aggregation
curl -X GET "localhost:9200/my-index/_search" \
  -H "Content-Type: application/json" \
  -d '{
    "aggs": {
      "log_levels": {
        "terms": {
          "field": "level"
        }
      }
    }
  }'

# Range query
curl -X GET "localhost:9200/my-index/_search" \
  -H "Content-Type: application/json" \
  -d '{
    "query": {
      "range": {
        "timestamp": {
          "gte": "2024-01-01T00:00:00Z",
          "lte": "2024-01-31T23:59:59Z"
        }
      }
    }
  }'
```

### Index Lifecycle Management (ILM)

```bash
# Create ILM policy
curl -X PUT "localhost:9200/_ilm/policy/my-policy" \
  -H "Content-Type: application/json" \
  -d '{
    "policy": "my-policy",
    "phases": {
      "hot": {
        "min_age": "0ms",
        "actions": {
          "rollover": {
            "max_primary_store_size": "50gb",
            "max_age": "30d"
          }
        }
      },
      "warm": {
        "min_age": "30d",
        "actions": {
          "set_priority": {
            "priority": 50
          }
        }
      },
      "cold": {
        "min_age": "90d",
        "actions": {
          "set_priority": {
            "priority": 0
          }
        }
      },
      "delete": {
        "min_age": "365d",
        "actions": {
          "delete": {}
        }
      }
    }
  }'
```

---

## 5. Logstash

### Basic Configuration

```ruby
# /etc/logstash/conf.d/logstash.conf

input {
  file {
    path => "/var/log/application.log"
    start_position => "beginning"
    codec => multiline {
      pattern => "^%{TIMESTAMP_ISO8601}"
      negate => true
      what => "previous"
    }
  }
}

filter {
  grok {
    match => { "message" => "%{TIMESTAMP_ISO8601:timestamp} %{LOGLEVEL:level} \[%{DATA:logger}\] %{GREEDYDATA:message}" }
  }
  
  date {
    match => [ "timestamp", "ISO8601" ]
    target => "@timestamp"
  }

  mutate {
    add_field => {
      "service" => "myapp"
      "environment" => "production"
    }
    remove_field => [ "host" ]
  }
}

output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "logs-%{+YYYY.MM.dd}"
  }
  
  stdout {
    codec => rubydebug
  }
}
```

### Input Plugins

```ruby
# TCP input
input {
  tcp {
    port => 5000
    codec => json
  }
}

# Syslog input
input {
  syslog {
    port => 514
    facility_labels => ["auth", "authpriv", "cron"]
  }
}

# Kafka input
input {
  kafka {
    bootstrap_servers => "localhost:9092"
    topics => ["logs"]
    consumer_threads => 4
  }
}

# HTTP input
input {
  http {
    port => 8080
    codec => json
  }
}
```

### Filter Plugins

```ruby
# Grok - parse unstructured data
filter {
  grok {
    match => {
      "message" => "%{COMBINEDAPACHELOG}"
    }
  }
}

# JSON - parse JSON
filter {
  json {
    source => "message"
  }
}

# Mutate - add/remove/rename fields
filter {
  mutate {
    add_field => { "datacenter" => "us-east-1" }
    remove_field => [ "temp_field" ]
    rename => { "old_field" => "new_field" }
    lowercase => [ "level" ]
  }
}

# Date - parse date field
filter {
  date {
    match => [ "timestamp", "dd/MMM/yyyy:HH:mm:ss Z" ]
  }
}

# Conditional
filter {
  if [level] == "ERROR" {
    mutate { add_field => { "alert" => true } }
  }
}
```

### Output Plugins

```ruby
# Elasticsearch output
output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "logs-%{+YYYY.MM.dd}"
    document_type => "_doc"
  }
}

# File output
output {
  file {
    path => "/var/log/logstash/output.log"
    codec => json_lines
  }
}

# Kafka output
output {
  kafka {
    bootstrap_servers => "localhost:9092"
    topic_id => "processed-logs"
  }
}

# S3 output
output {
  s3 {
    bucket => "my-bucket"
    region => "us-east-1"
    prefix => "logs/"
  }
}
```

### Test Configuration

```bash
# Syntax check
logstash -f /etc/logstash/conf.d/logstash.conf --config.test_and_exit

# Run with debug
logstash -f /etc/logstash/conf.d/logstash.conf --log.level=debug

# Sample data
echo '2024-01-01T12:00:00Z ERROR [Service] Database failed' | \
  logstash -f /etc/logstash/conf.d/logstash.conf
```

---

## 6. Kibana

### Discover

```
# Search logs in Discover
level: ERROR
service: api
timestamp: [2024-01-01 TO 2024-01-31]

# Filter
level: ERROR AND service: api
host: (web-01 OR web-02)
message: "connection"
```

### Create Visualizations

```bash
# Line chart
# Data: Timestamp (X-axis), Count of logs (Y-axis)
# Filter: level: ERROR

# Bar chart
# Data: Service (X-axis), Count (Y-axis)
# Breakdown by: level

# Pie chart
# Data: level, Count
```

### Dashboard Creation

```
1. Go to Dashboards
2. Create new dashboard
3. Add visualizations
4. Set time filter
5. Add filters
6. Save dashboard
```

### Alerting

```
1. Go to Stack Management > Alerting
2. Create new alert
3. Define condition: ES Query returns documents
4. Set threshold and time window
5. Configure actions (email, Slack, webhook)
6. Save alert
```

### Canvas

```
# Create reports with Canvas
1. Go to Canvas
2. Create new workpad
3. Add elements:
   - Images
   - Text
   - Data tables
   - Visualizations
4. Configure styling
5. Export as PDF
```

---

## 7. Beats

### Filebeat Installation

```bash
# macOS
brew install filebeat

# Linux
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.0.0-linux-x86_64.tar.gz
tar xzvf filebeat-8.0.0-linux-x86_64.tar.gz
cd filebeat-8.0.0-linux-x86_64

# Windows
# Download from https://www.elastic.co/downloads/beats/filebeat
```

### Filebeat Configuration

```yaml
# /etc/filebeat/filebeat.yml

filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /var/log/application/*.log
  fields:
    service: myapp
    environment: production
  multiline.pattern: '^\['
  multiline.negate: true
  multiline.match: after

- type: log
  enabled: true
  paths:
    - /var/log/nginx/access.log
  fields:
    service: nginx

output.elasticsearch:
  hosts: ["localhost:9200"]
  index: "filebeat-%{+yyyy.MM.dd}"

processors:
  - add_host_metadata: ~
  - add_docker_metadata: ~
```

### Metricbeat Configuration

```yaml
# /etc/metricbeat/metricbeat.yml

metricbeat.modules:
- module: system
  period: 10s
  metricsets:
    - cpu
    - memory
    - filesystem
    - process
  enabled: true

- module: docker
  period: 10s
  enabled: true

- module: mysql
  period: 10s
  hosts: ["root:password@tcp(localhost:3306)/"]
  enabled: true

output.elasticsearch:
  hosts: ["localhost:9200"]
  index: "metricbeat-%{+yyyy.MM.dd}"

processors:
  - add_host_metadata: ~
```

### Start Beats

```bash
# Filebeat
sudo systemctl start filebeat
sudo systemctl enable filebeat

# Metricbeat
sudo systemctl start metricbeat
sudo systemctl enable metricbeat

# Check status
sudo systemctl status filebeat
```

---

## 8. Best Practices

### Index Naming Strategy

```
# Use date-based indices
logs-application-2024.01.01
logs-nginx-2024.01.01
metrics-system-2024.01.01

# Use index templates
PUT _index_template/logs-template
{
  "index_patterns": ["logs-*"],
  "template": {
    "settings": {
      "number_of_shards": 3,
      "number_of_replicas": 1
    }
  }
}
```

### Data Volume Management

```yaml
# Configure retention in Logstash
output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "logs-%{+YYYY.MM.dd}"
  }
}

# Use ILM to delete old indices
# Keep hot data: 7 days
# Warm: 30 days
# Cold/Delete: 90 days
```

### Performance Optimization

```yaml
# Batch size
output {
  elasticsearch {
    hosts => ["localhost:9200"]
    bulk_size => 1000
    flush_interval => 5
  }
}

# Worker threads
output {
  elasticsearch {
    hosts => ["localhost:9200"]
    workers => 4
  }
}

# Heap allocation
# Logstash: -Xms512m -Xmx512m
# Elasticsearch: -Xms1g -Xmx1g
```

### Monitoring ELK Stack

```bash
# Monitor Elasticsearch
curl -X GET "localhost:9200/_cat/health?v"
curl -X GET "localhost:9200/_cat/nodes?v"
curl -X GET "localhost:9200/_cat/indices?v"

# Check Logstash status
curl -X GET "localhost:9600/_node/stats"

# Check Kibana status
curl -X GET "localhost:5601/api/status"
```

---

## 9. Troubleshooting

### Common Issues

```bash
# Issue: High memory usage
# Solution: Increase heap size
export ES_JAVA_OPTS="-Xms2g -Xmx2g"

# Issue: Slow queries
# Solution: Check shard allocation
curl -X GET "localhost:9200/_cat/shards?v"

# Issue: Missing logs
# Solution: Check Logstash pipeline
logstash -f config.conf --config.test_and_exit

# Issue: Disk space full
# Solution: Delete old indices
curl -X DELETE "localhost:9200/logs-2024.01.01"

# Check Elasticsearch logs
tail -f /var/log/elasticsearch/elasticsearch.log

# Check Logstash logs
tail -f /var/log/logstash/logstash-plain.log

# Check Kibana logs
tail -f /var/log/kibana/kibana.log
```

### Performance Monitoring

```bash
# Slow queries
curl -X GET "localhost:9200/_cat/indices?v&h=name,store.size,docs.count"

# Hot threads
curl -X GET "localhost:9200/_nodes/hot_threads"

# Task management
curl -X GET "localhost:9200/_tasks?detailed=true"

# Cluster stats
curl -X GET "localhost:9200/_cluster/stats"
```

---

## 10. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Elasticsearch Docs** | https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html |
| **Logstash Docs** | https://www.elastic.co/guide/en/logstash/current/index.html |
| **Kibana Docs** | https://www.elastic.co/guide/en/kibana/current/index.html |
| **Beats Docs** | https://www.elastic.co/guide/en/beats/libbeat/current/index.html |
| **Query DSL** | https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Getting Started** | https://www.elastic.co/what-is/elasticsearch |
| **Elasticsearch Blog** | https://www.elastic.co/blog |
| **Community** | https://discuss.elastic.co/ |
| **Stack Overflow** | https://stackoverflow.com/questions/tagged/elasticsearch |

---

## Quick Reference

```bash
# Check health
curl -X GET "localhost:9200/_cluster/health?pretty"

# Create index
curl -X PUT "localhost:9200/my-index"

# Index document
curl -X POST "localhost:9200/my-index/_doc" \
  -H "Content-Type: application/json" \
  -d '{"message":"test"}'

# Search
curl -X GET "localhost:9200/my-index/_search" \
  -H "Content-Type: application/json" \
  -d '{"query":{"match":{"message":"test"}}}'

# Start Logstash
logstash -f config.conf

# Test Logstash config
logstash -f config.conf --config.test_and_exit

# Access Kibana
# http://localhost:5601

# Install Filebeat
filebeat setup -e

# Start Filebeat
sudo systemctl start filebeat
```

---

**Last Updated:** 2026
**ELK Version:** 8.x
**License:** Elastic License & SSPL

For latest information, visit [Elastic Documentation](https://www.elastic.co/guide/)