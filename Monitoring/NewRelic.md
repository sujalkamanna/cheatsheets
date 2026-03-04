# New Relic Cheatsheet

![New Relic Logo](https://newrelic.com/assets/newrelic/source/NewRelic-logo-square.png)

---

## Table of Contents
1. [What is New Relic?](#1-what-is-new-relic)
2. [Core Concepts](#2-core-concepts)
3. [Installation & Setup](#3-installation--setup)
4. [Agents & Instrumentation](#4-agents--instrumentation)
5. [Dashboards & Queries](#5-dashboards--queries)
6. [APM Monitoring](#6-apm-monitoring)
7. [Alerts & Notifications](#7-alerts--notifications)
8. [Infrastructure Monitoring](#8-infrastructure-monitoring)
9. [Best Practices](#9-best-practices)
10. [Troubleshooting](#10-troubleshooting)
11. [Additional Resources](#11-additional-resources)

---

## 1. What is New Relic?

New Relic is a comprehensive observability platform that provides full-stack monitoring, APM (Application Performance Monitoring), infrastructure monitoring, log management, and synthetic monitoring for applications and infrastructure across cloud and on-premises environments.

**Key Benefits:**
- Full-stack observability
- Application Performance Monitoring (APM)
- Infrastructure and server monitoring
- Log aggregation and analysis
- Synthetic monitoring
- Real User Monitoring (RUM)
- AI-powered insights
- Multi-cloud support (AWS, Azure, GCP)
- Custom dashboards and alerts

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Agent** | Software installed to collect metrics and logs |
| **Entity** | Monitored application, host, service, or container |
| **Metrics** | Quantitative measurements (CPU, memory, latency) |
| **Events** | Recorded transactions and occurrences |
| **Traces** | Path of requests through distributed systems |
| **Logs** | Application and system log data |
| **Service Map** | Visual relationship between applications |
| **Dashboard** | Custom visualization of data |
| **Alert Policy** | Rules triggering notifications |
| **NRQL** | New Relic Query Language for data analysis |

---

## 3. Installation & Setup

### Create New Relic Account

```
Steps:
1. Go to https://newrelic.com/signup
2. Create account with email
3. Verify email
4. Set organization name
5. Get License Key (found in Account Settings)
```

### Get License Key

```
GUI Steps:
1. Click "Account settings" (bottom left)
2. Click "API keys"
3. Copy License key
4. Or click "Create key" for new key
```

### Java Agent Installation

```bash
# Download agent
wget https://download.newrelic.com/newrelic/java-agent/newrelic-java/current/newrelic-java.zip

# Extract
unzip newrelic-java.zip
cd newrelic

# Edit newrelic.yml
nano newrelic.yml

# Set license key
license_key: YOUR_LICENSE_KEY
app_name: MyApplication
```

### Java Application Setup

```bash
# Add to JVM startup
java -javaagent:/path/to/newrelic/newrelic.jar \
  -Dnewrelic.environment=production \
  -Dnewrelic.config.file=/path/to/newrelic/newrelic.yml \
  -jar application.jar

# Or in environment
export JAVA_OPTS="-javaagent:/opt/newrelic/newrelic.jar"
java $JAVA_OPTS -jar application.jar
```

### Node.js Agent Installation

```bash
# Install package
npm install newrelic

# Create newrelic.js configuration
cat > newrelic.js << 'EOF'
exports.config = {
  app_name: ['MyNodeApp'],
  license_key: 'YOUR_LICENSE_KEY',
  logging: {
    level: 'info'
  },
  allow_all_headers: true,
  attributes: {
    exclude: [
      'request.headers.cookie',
      'request.headers.authorization',
      'request.headers.x-api-key',
      'request.headers.x-access-token'
    ]
  }
};
EOF

# Require at top of application.js
require('newrelic');
```

### Python Agent Installation

```bash
# Install package
pip install newrelic

# Create configuration file
newrelic-admin generate-config YOUR_LICENSE_KEY newrelic.ini

# Run with agent
NEW_RELIC_CONFIG_FILE=newrelic.ini newrelic-admin run-python your_application.py

# Or run server
gunicorn --bind 0.0.0.0:8000 \
  --workers 4 \
  --worker-class sync \
  --env NEW_RELIC_CONFIG_FILE=newrelic.ini \
  myapp:app
```

### Docker Installation

```dockerfile
# Dockerfile with New Relic
FROM openjdk:11-jre-slim

RUN curl -s https://download.newrelic.com/newrelic/java-agent/newrelic-java/current/newrelic-java.zip \
    | unzip -d / - && \
    mv /newrelic /opt/newrelic

ENV JAVA_OPTS="-javaagent:/opt/newrelic/newrelic.jar"
ENV NEW_RELIC_LICENSE_KEY=YOUR_LICENSE_KEY
ENV NEW_RELIC_APP_NAME=MyApp

COPY application.jar /app.jar
CMD java $JAVA_OPTS -jar /app.jar
```

### Docker Compose Setup

```yaml
version: '3.8'

services:
  app:
    image: myapp:latest
    environment:
      - NEW_RELIC_LICENSE_KEY=YOUR_LICENSE_KEY
      - NEW_RELIC_APP_NAME=MyApplication
      - NEW_RELIC_LOG=stdout
    ports:
      - "8080:8080"

  newrelic-infrastructure:
    image: newrelic/infrastructure:latest
    environment:
      - NRIA_LICENSE_KEY=YOUR_LICENSE_KEY
      - NRIA_VERBOSE=0
    volumes:
      - /:/rootfs:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /sys:/sys:ro
      - /proc:/proc:ro
    cap_add:
      - SYS_PTRACE
```

### Kubernetes Installation

```yaml
# Helm chart installation
helm repo add newrelic https://helm-charts.newrelic.com
helm repo update

helm install newrelic-bundle newrelic/nri-bundle \
  --set newrelic.enabled=true \
  --set newrelic.licenseKey=YOUR_LICENSE_KEY \
  --set newrelic.cluster=production-us-east \
  -f values.yaml

# Verify installation
kubectl get pods -n newrelic
kubectl logs -n newrelic deployment/newrelic-infrastructure
```

### Important Ports & Endpoints

| Service | Port/Endpoint | Purpose |
|---------|--------------|---------|
| **New Relic API** | https://api.newrelic.com | REST API |
| **NerdGraph API** | https://api.newrelic.com/graphql | GraphQL API |
| **Collector** | https://collector.newrelic.com | Agent data collection |
| **EU Collector** | https://collector.eu01.nr-data.net | EU data collection |

---

## 4. Agents & Instrumentation

### Edit Agent Configuration

```yaml
# newrelic.yml (Java)
license_key: YOUR_LICENSE_KEY
app_name: MyApplication
environment: production

# Agent settings
agent:
  enabled: true
  capture_params: false
  
# Transaction settings
transaction_tracer:
  enabled: true
  transaction_threshold: apdex_f
  record_sql: obfuscated
  log_sql: false
  
# Error collection
error_collector:
  enabled: true
  capture_events: true
  max_event_samples_stored: 100

# Custom instrumentation
custom_instrumentation_editor:
  enabled: true

# Distributed tracing
distributed_tracing:
  enabled: true
```

### Java Agent Configuration

```yaml
# Common settings
app_name: MyApp
license_key: YOUR_LICENSE_KEY
log_level: info

# JVM settings
jvm:
  redirect_stderr: false
  
# Thread profiler
thread_profiler:
  enabled: true
  
# Attributes
attributes:
  enabled: true
  exclude: [request.headers.cookie, request.headers.authorization]

# Custom metrics
custom_metrics:
  - name: Custom/MyMetric
    value: 100
```

### Node.js Agent Configuration

```javascript
exports.config = {
  app_name: ['MyNodeApp'],
  license_key: process.env.NEW_RELIC_LICENSE_KEY,
  logging: {
    level: 'info',
    filepath: 'stdout'
  },
  distributed_tracing: {
    enabled: true
  },
  attributes: {
    enabled: true,
    exclude: ['request.headers.cookie', 'request.parameters.password']
  }
};
```

### Custom Instrumentation

```java
// Java - Custom metric
import com.newrelic.api.agent.NewRelic;

public class CustomMetrics {
  public void recordMetric() {
    // Record custom metric
    NewRelic.recordMetric("Custom/MyMetric", 100);
    
    // Record custom event
    NewRelic.recordCustomEvent("MyEvent", 
      ImmutableMap.of("key", "value", "count", 42));
    
    // Add custom attribute
    NewRelic.addCustomParameter("user_id", "12345");
    
    // Track transaction
    NewRelic.setTransactionName("Custom", "MyTransaction");
  }
}
```

```javascript
// Node.js - Custom instrumentation
const newrelic = require('newrelic');

// Custom metric
newrelic.recordMetric('Custom/MyMetric', 100);

// Custom event
newrelic.recordCustomEvent('MyEvent', {
  userId: 12345,
  action: 'purchase',
  amount: 99.99
});

// Custom attribute
newrelic.addCustomAttribute('user_id', '12345');

// Track transaction
newrelic.setTransactionName('Custom/Transaction');
```

### Agent Health Check

```bash
# Check agent status in logs
tail -f /var/log/newrelic/newrelic_agent.log

# Check Java agent
grep "Agent started" newrelic_agent.log

# Verify data collection
# Go to New Relic UI → APM → Your App
# Check for recent data points
```

---

## 5. Dashboards & Queries

### Access Dashboard

```
GUI Steps:
1. Log in to New Relic (newrelic.com)
2. Click "Dashboards" (top menu)
3. Click "Create dashboard"
4. Choose "Create a blank dashboard"
5. Add widgets
6. Click "Save dashboard"
```

### Create Custom Dashboard

```
Steps:
1. Dashboards → Create dashboard
2. Click "Add a chart"
3. Select chart type:
   - Line chart
   - Bar chart
   - Pie chart
   - Table
   - Number
   - Heatmap
4. Write NRQL query
5. Click "Save"
```

### NRQL Query Basics

```nrql
-- Simple query
SELECT count(*) FROM Transaction

-- With time range
SELECT count(*) FROM Transaction 
SINCE 1 hour ago

-- Group by
SELECT count(*) FROM Transaction 
GROUP BY appName

-- Average response time
SELECT average(duration) FROM Transaction 
WHERE response.status = '200'

-- Error rate
SELECT count(*) FROM Transaction 
WHERE error = true FACET error.class

-- Top 5 endpoints
SELECT count(*) FROM Transaction 
FACET request.uri 
LIMIT 5

-- Response time percentiles
SELECT percentile(duration, 50, 95, 99) 
FROM Transaction

-- Throughput (requests per minute)
SELECT rate(count(*), 1 minute) 
FROM Transaction

-- Database query performance
SELECT average(duration) FROM Span 
WHERE span.kind = 'internal' 
FACET db.statement
```

### NRQL Functions

```nrql
-- Aggregation functions
SELECT 
  count(*) as total,
  average(duration) as avg_duration,
  max(duration) as max_duration,
  min(duration) as min_duration,
  stddev(duration) as stddev,
  sum(value) as total_value
FROM Transaction

-- Percentile
SELECT percentile(duration, 50, 75, 90, 95, 99) 
FROM Transaction

-- Rate of change
SELECT rate(count(*), 1 minute) FROM Transaction

-- Unique count
SELECT uniqueCount(user_id) FROM PageView

-- Filtering
SELECT count(*) FROM Transaction 
WHERE duration > 1
  AND appName = 'MyApp'
  AND http.statusCode != 404

-- Conditional
SELECT count(*) FROM Transaction 
FACET CASE WHEN duration < 0.5 THEN 'Fast'
           WHEN duration < 1.0 THEN 'Medium'
           ELSE 'Slow' END
```

### Facet Queries

```nrql
-- Group by single dimension
SELECT count(*) FROM Transaction 
FACET appName

-- Group by multiple dimensions
SELECT count(*) FROM Transaction 
FACET appName, host

-- Time-based facet
SELECT count(*) FROM Transaction 
FACET appName TIMESERIES 1 minute

-- Limit results
SELECT count(*) FROM Transaction 
FACET appName LIMIT 10

-- Order results
SELECT count(*) FROM Transaction 
FACET appName ORDER BY count(*) DESC
```

---

## 6. APM Monitoring

### View APM Overview

```
GUI Steps:
1. Click "APM & Services" (top menu)
2. Select your application
3. View key metrics:
   - Throughput (requests/min)
   - Response time
   - Error rate
   - Apdex score
4. Click tabs:
   - Transactions
   - Databases
   - External services
   - Traces
```

### Application Performance Metrics

```
Key Metrics:
- Throughput: Requests per minute
- Response Time: Average, median, percentiles
- Error Rate: % of failed requests
- Apdex Score: User satisfaction (0-1)
  - 0.93-1.0: Excellent
  - 0.85-0.92: Good
  - 0.70-0.84: Fair
  - Below 0.70: Poor
```

### Transaction Traces

```
GUI Steps:
1. APM → Your Application
2. Click "Transactions"
3. Select transaction
4. Click "Trace details"
5. View:
   - Timeline of operations
   - Database queries
   - External API calls
   - Stack traces
   - Slow segments
```

### Distributed Tracing

```
GUI Steps:
1. APM → Service map
2. View service relationships
3. Click "Distributed traces"
4. Select trace
5. View end-to-end request flow:
   - Timeline
   - Service calls
   - Latency breakdown
   - Errors in pipeline
```

### Database Performance

```nrql
-- Slow database queries
SELECT 
  average(duration) as avg_duration,
  count(*) as query_count
FROM Span 
WHERE span.kind = 'internal' 
FACET db.statement 
LIMIT 10

-- Database time by service
SELECT sum(duration) FROM Span 
WHERE db.system IS NOT NULL 
FACET service.name

-- Database connection pool
SELECT average(db.connection_pool_open) 
FROM Span 
FACET host
```

---

## 7. Alerts & Notifications

### Create Alert Policy

```
GUI Steps:
1. Click "Alerts & AI" (top menu)
2. Click "Alert policies"
3. Click "Create policy"
4. Set:
   - Policy name: "Production Alerts"
   - Incident preference: "Per policy"
5. Click "Create"
```

### Add Alert Condition

```
Steps:
1. In Alert Policy
2. Click "Add condition"
3. Choose condition type:
   - APM
   - Infrastructure
   - Custom NRQL
   - Synthetic
4. Configure:
   - Entity: Select app/host
   - Metric: CPU, Memory, Response Time, etc.
   - Threshold: Critical value
   - Duration: Time above threshold
5. Click "Create"
```

### NRQL Alert Example

```
Condition Type: NRQL query
Query:
SELECT count(*) FROM Transaction 
WHERE error = true

Threshold:
- Critical when: Value > 10
- For at least: 5 minutes

Evaluation:
- Frequency: Every 1 minute
- Window: 5 minutes
```

### Add Notification Channel

```
Steps:
1. In Alert Policy
2. Click "Notification channels"
3. Click "Add notification channel"
4. Select channel:
   - Email
   - Slack
   - PagerDuty
   - Webhook
   - OpsGenie
   - Teams
5. Configure:
   - Channel name
   - API key / Webhook URL
   - Test notification
6. Save
```

### Slack Integration

```
Setup:
1. In Slack workspace: Create app
2. Under "Incoming Webhooks" → Add New Webhook
3. Copy Webhook URL
4. In New Relic:
   - Add notification channel
   - Type: Slack
   - Webhook URL: [paste URL]
   - Channel: #alerts
   - Test notification
   - Save
```

### Alert Notification Template

```
Alert: {{ conditionName }}
Status: {{ status }}
Time: {{ timestamp }}
Value: {{ triggerValue }}
Threshold: {{ thresholdValue }}
Entity: {{ entityName }}
Incident ID: {{ incidentId }}
URL: {{ incidentUrl }}
```

---

## 8. Infrastructure Monitoring

### Infrastructure Agent Installation

```bash
# Download and install (Ubuntu/Debian)
curl -sSL https://raw.githubusercontent.com/newrelic/infrastructure-agent/master/assets/examples/install.sh | sudo bash

# Or manual installation
sudo apt-get update
sudo apt-get install newrelic-infra -y

# Configure license key
echo "license_key: YOUR_LICENSE_KEY" | \
  sudo tee /etc/newrelic-infra.yml

# Start service
sudo systemctl start newrelic-infra
sudo systemctl enable newrelic-infra

# Check status
sudo systemctl status newrelic-infra
sudo tail -f /var/log/newrelic-infra/newrelic-infra.log
```

### Infrastructure Integrations

```bash
# MySQL integration
sudo yum install nri-mysql

# PostgreSQL integration
sudo yum install nri-postgresql

# Redis integration
sudo yum install nri-redis

# Nginx integration
sudo yum install nri-nginx

# Configure integration
sudo nano /etc/newrelic-infra/integrations.d/mysql-config.yml

# Restart agent
sudo systemctl restart newrelic-infra
```

### Monitor Hosts

```
GUI Steps:
1. Click "Infrastructure" (top menu)
2. Click "Hosts"
3. View:
   - Host inventory
   - CPU usage
   - Memory usage
   - Disk usage
   - Network I/O
4. Click specific host for details
```

### Custom Infrastructure Metrics

```yaml
# /etc/newrelic-infra/integrations.d/custom-integration.yml
integrations:
  - name: nri-custom
    command: custom-integration
    interval: 30s
    labels:
      environment: production
    config:
      metric_path: /usr/local/bin/custom-metric.sh
```

---

## 9. Best Practices

### Application Instrumentation

```
1. Deploy Agent Early
   - Install during development
   - Test in staging
   - Deploy to production

2. Configure Properly
   - Set meaningful app name
   - Add environment labels
   - Enable distributed tracing
   - Exclude sensitive data

3. Custom Events
   - Track business metrics
   - Monitor user actions
   - Log significant events

4. Error Tracking
   - Catch and report errors
   - Add context to errors
   - Track error trends

5. Performance Tuning
   - Monitor response times
   - Optimize slow queries
   - Reduce external calls
```

### Dashboard Best Practices

```nrql
1. Clear naming
   - "API Response Time"
   - "Database Query Performance"

2. Appropriate time range
   - Real-time: Last 1-6 hours
   - Trends: Last 7-30 days

3. Key metrics only
   - Avoid cluttered dashboards
   - Focus on actionable data

4. Query optimization
   - Use appropriate SINCE clause
   - Filter unnecessary data
   - Use LIMIT for large datasets

Example:
SELECT average(duration) FROM Transaction 
WHERE appName = 'MyApp'
  AND transaction.name NOT LIKE '%health%'
SINCE 1 hour ago
TIMESERIES
```

### Alert Strategy

```
1. Meaningful Alerts
   - Base on actual business impact
   - Avoid alert fatigue
   - Actionable thresholds

2. Escalation
   - Warning level first
   - Critical for urgent issues
   - Include runbook URL

3. Testing
   - Test alerts in staging
   - Verify notification delivery
   - Review alert response

4. Tuning
   - Adjust thresholds over time
   - Monitor alert patterns
   - Update based on baselines
```

### Data Retention

```
Free Tier:
- Metrics: 7 days
- Events: 1 day
- Logs: 1 day
- Traces: 1 day

Pro Tier:
- Metrics: 395 days (13 months)
- Events: 395 days
- Logs: 30 days
- Traces: 7 days (configurable)
```

---

## 10. Troubleshooting

### Agent Connection Issues

```bash
# Check agent logs
tail -f /var/log/newrelic/newrelic_agent.log

# Verify license key
grep "license_key" /etc/newrelic-infra.yml

# Test connectivity
curl -I https://collector.newrelic.com

# Check firewall
sudo ufw allow outbound 443/tcp

# Check DNS
nslookup collector.newrelic.com

# Restart agent
sudo systemctl restart newrelic-infra
```

### No Data Appearing

```bash
# Check agent status
New Relic UI → Infrastructure → Hosts

# Verify configuration
cat /etc/newrelic-infra.yml

# Check permissions
ls -la /var/log/newrelic-infra/

# Check network connectivity
telnet collector.newrelic.com 443

# View agent version
newrelic-infra -version

# Debug mode
export NRIA_VERBOSE=1
newrelic-infra -d
```

### High Agent Overhead

```
Optimization:
1. Reduce sampling rate
2. Disable unnecessary integrations
3. Increase collection interval
4. Filter unnecessary metrics

Configuration:
sample_rate: 10000  # Default sampling
custom_attributes:
  enabled: true
```

### Common Issues & Solutions

```bash
# Issue: "License key invalid"
# Solution: Verify license key in config

# Issue: Agent not starting
# Solution: Check syntax in YAML file
yamllint /etc/newrelic-infra.yml

# Issue: High memory usage
# Solution: Reduce collection frequency

# Issue: Missing metrics
# Solution: Check if integration installed and running

# Debug command
sudo newrelic-infra -debug -logfile=/tmp/debug.log

# Check recent log errors
grep ERROR /var/log/newrelic-infra/newrelic-infra.log | tail -20
```

---

## 11. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://docs.newrelic.com/docs/new-relic-solutions/get-started/intro-new-relic/ |
| **APM Docs** | https://docs.newrelic.com/docs/apm/ |
| **Infrastructure** | https://docs.newrelic.com/docs/infrastructure/ |
| **NRQL Guide** | https://docs.newrelic.com/docs/nrql/ |
| **API Reference** | https://docs.newrelic.com/docs/apis/ |
| **Alerts** | https://docs.newrelic.com/docs/alerts-applied-intelligence/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **New Relic University** | https://learn.newrelic.com/ |
| **Blog** | https://newrelic.com/blog |
| **Community** | https://discuss.newrelic.com/ |
| **GitHub** | https://github.com/newrelic |

---

## Quick Reference

```bash
# Java agent setup
java -javaagent:/path/to/newrelic.jar \
  -Dnewrelic.config.file=newrelic.yml \
  -jar app.jar

# Node.js agent
require('newrelic');
npm start

# Python agent
NEW_RELIC_CONFIG_FILE=newrelic.ini \
  newrelic-admin run-program python app.py

# Infrastructure agent
sudo systemctl start newrelic-infra
sudo systemctl status newrelic-infra
tail -f /var/log/newrelic-infra/newrelic-infra.log

# Docker container
docker run -e NEW_RELIC_LICENSE_KEY=YOUR_KEY \
  -e NEW_RELIC_APP_NAME=MyApp \
  myapp:latest

# Check agent health
curl https://api.newrelic.com/v2/applications.json \
  -H "X-Api-Key: YOUR_API_KEY"

# NRQL query example
SELECT count(*) FROM Transaction 
WHERE appName = 'MyApp'
SINCE 1 hour ago
TIMESERIES

# View logs
New Relic UI → Logs
Filter: service_name = 'MyApp'

# Alert policy
Alerts & AI → Alert policies → Create policy
```

---

## File Locations

| Item | Path |
|------|------|
| **Java Config** | newrelic.yml |
| **Node Config** | newrelic.js |
| **Infra Config** | /etc/newrelic-infra.yml |
| **Infra Integrations** | /etc/newrelic-infra/integrations.d/ |
| **Logs** | /var/log/newrelic-infra/ |
| **License Key** | Dashboard → Account settings → API keys |

---

**Last Updated:** 2026
**New Relic Version:** Latest
**License:** Commercial & Free Tier Available

For latest information, visit [New Relic Documentation](https://docs.newrelic.com/)