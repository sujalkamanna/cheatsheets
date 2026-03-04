# Grafana Cheatsheet

![Grafana Logo](https://grafana.com/static/assets/img/branding/grafana_logo_swirl_fullcolor.png)

---

## Table of Contents
1. [What is Grafana?](#1-what-is-grafana)
2. [Core Concepts](#2-core-concepts)
3. [Installation & Setup](#3-installation--setup)
4. [Configuration](#4-configuration)
5. [Data Sources](#5-data-sources)
6. [Dashboards](#6-dashboards)
7. [Panels & Visualizations](#7-panels--visualizations)
8. [Alerting](#8-alerting)
9. [Users & Permissions](#9-users--permissions)
10. [Best Practices](#10-best-practices)
11. [Troubleshooting](#11-troubleshooting)
12. [Additional Resources](#12-additional-resources)

---

## 1. What is Grafana?

Grafana is an open-source visualization and monitoring platform that integrates with various data sources to create interactive dashboards, perform real-time data analysis, and set up comprehensive alerting systems.

**Key Benefits:**
- Multi-data source support
- Rich visualization options
- Real-time data monitoring
- Flexible alerting system
- User authentication and authorization
- Dashboard templating
- Cost-effective solution
- Active community support

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Dashboard** | Collection of panels displaying metrics and data |
| **Panel** | Individual visualization on a dashboard |
| **Data Source** | Connection to backend database/system (Prometheus, Elasticsearch, etc.) |
| **Query** | Request to retrieve data from data source |
| **Variables** | Dynamic template parameters for dashboards |
| **Annotation** | Event marker on timeline |
| **Alert** | Rule triggering notification on threshold breach |
| **Organization** | Workspace grouping dashboards and users |
| **Team** | Group of users with shared permissions |
| **Playlist** | Sequence of dashboards displaying in rotation |

---

## 3. Installation & Setup

### Linux Installation (Ubuntu/Debian)

```bash
# Add Grafana repository
sudo apt-get install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"

# Add Grafana GPG key
sudo apt-get install -y apt-transport-https
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -

# Update and install
sudo apt-get update
sudo apt-get install -y grafana-server

# Start service
sudo systemctl start grafana-server
sudo systemctl enable grafana-server

# Check status
sudo systemctl status grafana-server
```

### macOS Installation

```bash
# Install via Homebrew
brew install grafana

# Start Grafana
brew services start grafana

# Verify
brew services list | grep grafana
```

### Docker Installation

```bash
# Run Grafana container
docker run -d \
  --name grafana \
  -p 3000:3000 \
  -e GF_SECURITY_ADMIN_PASSWORD=admin \
  -v grafana-storage:/var/lib/grafana \
  grafana/grafana:latest

# With Docker Compose
cat > docker-compose.yml << 'EOF'
version: '3.8'

services:
  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
      - GF_USERS_ALLOW_SIGN_UP=false
    volumes:
      - grafana-storage:/var/lib/grafana
      - ./provisioning:/etc/grafana/provisioning
    networks:
      - monitoring

  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus-storage:/prometheus
    networks:
      - monitoring

volumes:
  grafana-storage:
  prometheus-storage:

networks:
  monitoring:
EOF

docker-compose up -d
```

### Windows Installation

```bash
# Download installer from
# https://grafana.com/grafana/download?platform=windows

# Or use Chocolatey
choco install grafana

# Start service
net start Grafana

# Verify
http://localhost:3000
```

### Default Credentials

```
URL: http://localhost:3000
Username: admin
Password: admin
```

**⚠️ Change default password immediately on first login!**

### Important Ports

| Service | Port | Purpose |
|---------|------|---------|
| **Grafana UI** | 3000 | Web interface |
| **Prometheus** | 9090 | Metrics database |
| **Elasticsearch** | 9200 | Logs/metrics storage |
| **InfluxDB** | 8086 | Time-series database |
| **MySQL** | 3306 | Relational database |
| **PostgreSQL** | 5432 | Relational database |

---

## 4. Configuration

### Main Configuration File

```bash
# Location
/etc/grafana/grafana.ini

# Or via environment variables
export GF_SECURITY_ADMIN_PASSWORD="secure_password"
export GF_INSTALL_PLUGINS="grafana-piechart-panel"
```

### grafana.ini Configuration

```ini
[paths]
data = /var/lib/grafana
logs = /var/log/grafana
plugins = /var/lib/grafana/plugins
provisioning = /etc/grafana/provisioning

[server]
protocol = http
http_addr = 0.0.0.0
http_port = 3000
domain = localhost
root_url = http://localhost:3000

[database]
type = sqlite3
path = /var/lib/grafana/grafana.db

# For MySQL
# type = mysql
# host = localhost:3306
# name = grafana
# user = grafana
# password = password

[security]
admin_user = admin
admin_password = admin
disable_brute_force_login_protection = false
cookie_secure = false
cookie_samesite = lax

[auth]
disable_login_form = false
disable_signout_link = false

[users]
allow_sign_up = false
auto_assign_org = true
auto_assign_org_id = 1
auto_assign_org_role = Viewer

[auth.anonymous]
enabled = false
org_role = Viewer

[log]
level = info
```

### Environment Variables

```bash
# Server settings
export GF_SERVER_HTTP_PORT=3000
export GF_SERVER_DOMAIN=example.com
export GF_SERVER_ROOT_URL=https://example.com/grafana

# Database
export GF_DATABASE_TYPE=mysql
export GF_DATABASE_HOST=localhost:3306
export GF_DATABASE_NAME=grafana
export GF_DATABASE_USER=grafana
export GF_DATABASE_PASSWORD=password

# Security
export GF_SECURITY_ADMIN_USER=admin
export GF_SECURITY_ADMIN_PASSWORD=admin123
export GF_SECURITY_SECRET_KEY=your_secret_key

# SMTP for alerts
export GF_SMTP_ENABLED=true
export GF_SMTP_HOST=smtp.example.com:587
export GF_SMTP_USER=alerts@example.com
export GF_SMTP_PASSWORD=password
export GF_SMTP_FROM_ADDRESS=alerts@example.com

# Start with environment
grafana-server
```

### Enable HTTPS

```ini
[server]
protocol = https
http_addr = 0.0.0.0
http_port = 3000
cert_file = /etc/grafana/certs/cert.pem
cert_key = /etc/grafana/certs/key.pem
```

```bash
# Generate self-signed certificate
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/grafana/certs/key.pem \
  -out /etc/grafana/certs/cert.pem
```

---

## 5. Data Sources

### Add Data Source via GUI

```
Steps:
1. Click "Configuration" (gear icon) → Data Sources
2. Click "Add data source"
3. Select data source type
4. Fill in connection details
5. Click "Save & Test"
```

### Prometheus Data Source

```yaml
# Configuration
Type: Prometheus
URL: http://localhost:9090
Access: Server (default)
Scrape interval: 15s
Query timeout: 60s
HTTP Method: GET
```

### Elasticsearch Data Source

```yaml
Type: Elasticsearch
URL: http://localhost:9200
Access: Server
Index name: logs-*
Time field name: @timestamp
Min time interval: 1m
```

### InfluxDB Data Source

```yaml
Type: InfluxDB
URL: http://localhost:8086
Access: Server
Database: telegraf
User: admin
Password: password
HTTP Method: GET
```

### MySQL Data Source

```yaml
Type: MySQL
Host: localhost:3306
Database: metrics
User: grafana
Password: password
SSL Mode: disable
```

### Add Data Source via CLI

```bash
# Using Grafana CLI
grafana-cli admin data-source-uid-reset

# Via API
curl -X POST http://localhost:3000/api/datasources \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -d '{
    "name": "Prometheus",
    "type": "prometheus",
    "url": "http://localhost:9090",
    "access": "proxy",
    "isDefault": true
  }'
```

---

## 6. Dashboards

### Create Dashboard via GUI

```
Steps:
1. Click "+" → Dashboard
2. Click "Add new panel"
3. Select visualization type
4. Choose data source
5. Write query
6. Configure panel settings
7. Click "Save" button
8. Enter dashboard name
9. Click "Save"
```

### Dashboard JSON Structure

```json
{
  "dashboard": {
    "title": "System Metrics",
    "description": "Overall system health",
    "tags": ["production", "system"],
    "timezone": "UTC",
    "refresh": "30s",
    "time": {
      "from": "now-6h",
      "to": "now"
    },
    "panels": [
      {
        "id": 1,
        "title": "CPU Usage",
        "type": "graph",
        "datasource": "Prometheus",
        "targets": [
          {
            "expr": "avg(rate(cpu_usage[5m]))",
            "legendFormat": "CPU Usage %"
          }
        ]
      }
    ]
  }
}
```

### Import Dashboard

```
GUI Method:
1. Click "+" → Import
2. Enter dashboard ID or JSON
3. Select data source
4. Click "Import"

CLI Method:
grafana-cli dashboards pull <dashboard-id>
```

### Popular Dashboard IDs

```
1860  - Node Exporter for Prometheus
3662  - Prometheus 2.0 Stats
7152  - Kubernetes Cluster Monitoring
11074 - Node Exporter Full
12114 - System Metrics
```

### Provision Dashboards

```yaml
# /etc/grafana/provisioning/dashboards/dashboards.yml
apiVersion: 1

providers:
  - name: 'Default'
    orgId: 1
    folder: ''
    type: file
    disableDeletion: false
    updateIntervalSeconds: 10
    allowUiUpdates: true
    options:
      path: /var/lib/grafana/dashboards
```

```bash
# Place dashboard JSON files in provisioning directory
cp dashboard.json /var/lib/grafana/dashboards/

# Restart Grafana
sudo systemctl restart grafana-server
```

---

## 7. Panels & Visualizations

### Panel Types

| Type | Use Case |
|------|----------|
| **Graph** | Time-series data |
| **Stat** | Single metric value |
| **Table** | Tabular data |
| **Gauge** | Percentage/threshold |
| **Bar Gauge** | Comparison bars |
| **Heatmap** | Density visualization |
| **Pie Chart** | Percentage distribution |
| **Map** | Geographic data |
| **Logs** | Log messages |
| **Alert List** | Alert status |

### Create Graph Panel

```
Steps:
1. Click "Add panel" → Graph
2. Configure:
   - Title: "CPU Usage"
   - Data Source: Prometheus
   - Query: avg(rate(cpu_usage[5m]))
   - Legend: Show/Format
   - Axes: Left Y-axis settings
   - Display: Lines, Points, etc.
3. Click "Apply"
```

### Create Stat Panel

```
Steps:
1. Click "Add panel" → Stat
2. Configure:
   - Title: "Memory Usage"
   - Query: node_memory_MemAvailable_bytes
   - Unit: Bytes (SI)
   - Thresholds: 70 (yellow), 90 (red)
   - Color mode: Value/Background
3. Click "Apply"
```

### Create Table Panel

```
Steps:
1. Click "Add panel" → Table
2. Configure:
   - Title: "Host Status"
   - Query: up{job="prometheus"}
   - Columns: Select fields to display
   - Sorting: Sort by column
   - Pagination: Rows per page
3. Click "Apply"
```

### Variables & Templates

```
Creating Variable:
1. Go to Dashboard Settings
2. Click "Variables"
3. Click "New variable"
4. Configure:
   - Name: environment
   - Type: Query
   - Data source: Prometheus
   - Query: label_values(up, env)
   - Refresh: On Dashboard Load
5. Save

Usage in panel query:
avg(rate(cpu_usage{env="$environment"}[5m]))
```

### Repeating Panels

```
Steps:
1. Edit panel
2. Go to "Panel" options
3. Enable "Repeat by variable"
4. Select variable: environment
5. Click "Apply"

This creates panel for each variable value
```

---

## 8. Alerting

### Create Alert Rule via GUI

```
Steps:
1. Edit Panel → Alert tab
2. Click "Create Alert"
3. Configure:
   - Name: "High CPU"
   - Evaluate: Every 1m for 5m
   - Condition: avg(cpu_usage) > 80
   - No data: No Alert
   - Execution error: Alert
4. Click "Save" → "Save Dashboard"
```

### Alert Notification Channels

```
Configuration Path:
Configuration (gear icon) → Notification channels

Supported Channels:
- Email
- Slack
- PagerDuty
- OpsGenie
- Microsoft Teams
- Webhook
- VictorOps
- Discord
- Telegram
```

### Configure Slack Integration

```
Steps:
1. In Slack workspace: Create incoming webhook
2. Copy webhook URL
3. In Grafana: Go to Notification Channels
4. Click "New Channel"
5. Configure:
   - Name: Slack Alerts
   - Type: Slack
   - Webhook URL: [paste webhook]
   - Channel: #alerts
6. Click "Send Test" → "Save"
```

### Email Alerts

```ini
# In grafana.ini
[smtp]
enabled = true
host = smtp.gmail.com:587
user = alerts@example.com
password = app_password
from_address = alerts@example.com
from_name = Grafana Alerts
startTLS_policy = MandatoryStartTLS
```

### Alert via API

```bash
curl -X POST http://localhost:3000/api/alert-notifications \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -d '{
    "name": "Email Alerts",
    "type": "email",
    "isDefault": false,
    "settings": {
      "addresses": "admin@example.com"
    }
  }'
```

### Alert Notification Template

```
Alert: {{ .AlertRuleName }}
Status: {{ .Status }}
Time: {{ .StartTime }}
Value: {{ .Value }}
URL: {{ .DashboardURL }}
```

---

## 9. Users & Permissions

### User Management via GUI

```
Steps:
1. Configuration (gear) → Users
2. Click "New user"
3. Fill:
   - Name: John Doe
   - Email: john@example.com
   - Username: john
   - Password: secure_password
4. Select Role:
   - Admin: Full access
   - Editor: Can modify dashboards
   - Viewer: Read-only access
5. Click "Create"
```

### User Roles

```
1. Admin
   - Manage users, organizations
   - Configure data sources
   - Manage plugins

2. Editor
   - Create/edit dashboards
   - Create alerts
   - View all dashboards

3. Viewer
   - View dashboards
   - View alerts
   - Read-only access
```

### Create User via API

```bash
curl -X POST http://localhost:3000/api/admin/users \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "login": "john",
    "password": "secure_password",
    "role": "Editor"
  }'
```

### Organization Management

```
Steps:
1. Click on profile icon → Switch org
2. Click "Create new organization"
3. Enter organization name
4. Click "Create"

Teams within organization:
1. Configuration → Teams
2. Click "New team"
3. Add members
4. Set permissions
```

### LDAP Authentication

```ini
# In grafana.ini
[auth.ldap]
enabled = true
config_file = /etc/grafana/ldap.toml
```

```toml
# /etc/grafana/ldap.toml
[[servers]]
host = "ldap.example.com"
port = 389
use_ssl = false
start_tls = true
bind_dn = "cn=admin,dc=example,dc=com"
bind_password = "password"

[servers.attributes]
name = "givenName"
surname = "sn"
username = "uid"
member_of = "memberOf"
email = "mail"
```

---

## 10. Best Practices

### Dashboard Design

```
1. Clear Naming
   - Dashboard: "Production System Metrics"
   - Panels: "CPU Usage", "Memory Utilization"

2. Organized Panels
   - Group related metrics together
   - Use rows for sections
   - Consistent layout

3. Appropriate Visualization
   - Graph: Time-series trends
   - Stat: Current values
   - Table: Detailed data
   - Gauge: Thresholds

4. Readable Labels
   - Use unit suffixes: %, GB, ms
   - Format numbers: 1.2K instead of 1200
   - Add descriptions for clarity

5. Time Range
   - Set appropriate default: now-6h, now-24h
   - Allow user selection
   - Use refresh intervals wisely
```

### Query Optimization

```
Prometheus:
# Bad: Too high cardinality
up{pod=~"pod-.*"}

# Good: Specific label filter
up{job="api", env="prod"}

Elasticsearch:
# Bad: No time filter
GET logs/_search

# Good: With time filter
GET logs/_search
{
  "query": {
    "range": {
      "@timestamp": {
        "gte": "now-1h"
      }
    }
  }
}
```

### Alert Best Practices

```
1. Meaningful Alert Names
   - "High CPU Usage" ✓
   - "Alert1" ✗

2. Clear Thresholds
   - Based on historical data
   - Leave room for spikes
   - Test alert rules

3. Action Plan
   - Include runbook URL in notification
   - Specific remediation steps
   - Escalation procedure

4. Avoid Alert Fatigue
   - Don't alert on every spike
   - Use evaluation windows
   - Adjust thresholds based on patterns
```

### Security

```
1. Change Default Password
   - On first login change admin password

2. Enable Authentication
   - LDAP/OAuth for enterprise
   - Enforce strong passwords

3. API Keys
   - Use for integrations
   - Set expiration dates
   - Restrict permissions

4. HTTPS
   - Enable certificate
   - Use secure cookies
   - Restrict cross-origin

5. Backup
   - Regular database backups
   - Export dashboards
   - Document configurations
```

---

## 11. Troubleshooting

### Common Issues

```bash
# Issue: Cannot connect to Prometheus
# Solution: Check URL and firewall
curl -X GET http://localhost:9090/api/v1/targets

# Issue: Dashboard loading slowly
# Solution: Optimize queries, increase evaluation period
# Reduce number of panels
# Use more specific time ranges

# Issue: High memory usage
# Solution: Clear log files, optimize database
sudo systemctl restart grafana-server

# Issue: Authentication errors
# Check logs
tail -f /var/log/grafana/grafana.log

# Issue: Data not appearing
# Solution: Check data source connection
# Verify queries in Prometheus/Elasticsearch
# Check time ranges match

# Restart Grafana
sudo systemctl restart grafana-server

# Check Grafana logs
sudo journalctl -u grafana-server -f

# Check configuration
sudo grafana-cli admin reset-admin-password newpassword
```

### Performance Optimization

```bash
# Increase memory
export GF_SERVER_MAX_OPEN_FILES=2000

# Database optimization
# Use PostgreSQL/MySQL instead of SQLite for production

# Query optimization
# Reduce metric cardinality
# Use appropriate time intervals
# Add query caching

# Dashboard optimization
# Reduce number of panels per dashboard
# Use aggregation functions
# Increase refresh intervals
```

### API Debugging

```bash
# Get API token
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"user":"admin","password":"admin"}'

# List data sources
curl -X GET http://localhost:3000/api/datasources \
  -H "Authorization: Bearer YOUR_TOKEN"

# List dashboards
curl -X GET http://localhost:3000/api/search \
  -H "Authorization: Bearer YOUR_TOKEN"

# Get specific dashboard
curl -X GET http://localhost:3000/api/dashboards/uid/DASHBOARD_UID \
  -H "Authorization: Bearer YOUR_TOKEN"
```

---

## 12. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://grafana.com/docs/grafana/latest/getting-started/ |
| **Installation** | https://grafana.com/grafana/download |
| **Dashboards** | https://grafana.com/docs/grafana/latest/dashboards/ |
| **Data Sources** | https://grafana.com/docs/grafana/latest/datasources/ |
| **API Docs** | https://grafana.com/docs/grafana/latest/http_api/ |
| **Alert Rules** | https://grafana.com/docs/grafana/latest/alerting/ |
| **Dashboard Gallery** | https://grafana.com/grafana/dashboards |

### Useful Dashboard IDs

| ID | Name | Purpose |
|----|------|---------|
| 1860 | Node Exporter | System metrics |
| 3662 | Prometheus | Prometheus stats |
| 7152 | Kubernetes | K8s monitoring |
| 12114 | System | Full system view |
| 11074 | Node Exporter Full | Detailed host metrics |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Official Blog** | https://grafana.com/blog/ |
| **Community** | https://community.grafana.com/ |
| **Stack Overflow** | https://stackoverflow.com/questions/tagged/grafana |
| **GitHub** | https://github.com/grafana/grafana |

---

## Quick Reference

```bash
# Start Grafana
sudo systemctl start grafana-server
sudo systemctl enable grafana-server

# Stop Grafana
sudo systemctl stop grafana-server

# Check status
sudo systemctl status grafana-server

# View logs
tail -f /var/log/grafana/grafana.log

# Reset admin password
sudo grafana-cli admin reset-admin-password newpassword

# Install plugin
grafana-cli plugins install grafana-piechart-panel
sudo systemctl restart grafana-server

# Access Grafana
# http://localhost:3000

# API: List data sources
curl -X GET http://localhost:3000/api/datasources \
  -H "Authorization: Bearer TOKEN"

# API: Create dashboard
curl -X POST http://localhost:3000/api/dashboards/db \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer TOKEN" \
  -d @dashboard.json

# Docker restart
docker restart grafana

# Docker logs
docker logs -f grafana

# Check open ports
sudo netstat -tulpn | grep 3000
sudo lsof -i :3000
```

---

## File Locations

| Item | Path |
|------|------|
| **Config** | /etc/grafana/grafana.ini |
| **Data** | /var/lib/grafana/ |
| **Logs** | /var/log/grafana/ |
| **Plugins** | /var/lib/grafana/plugins/ |
| **Provisioning** | /etc/grafana/provisioning/ |
| **Dashboards** | /etc/grafana/provisioning/dashboards/ |
| **Data Sources** | /etc/grafana/provisioning/datasources/ |

---

**Last Updated:** 2026
**Grafana Version:** 9.x+
**License:** AGPL v3 / Enterprise

For latest information, visit [Grafana Official Site](https://grafana.com/)