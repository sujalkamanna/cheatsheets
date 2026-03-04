# CloudWatch Cheatsheet

![AWS CloudWatch Logo](https://a0.awsstatic.com/libra-css/images/logos/aws_logo_smile_1200x627.png)

---

## Table of Contents
1. [What is CloudWatch?](#1-what-is-cloudwatch)
2. [Core Concepts](#2-core-concepts)
3. [Installation & Setup](#3-installation--setup)
4. [Metrics](#4-metrics)
5. [Logs](#5-logs)
6. [Alarms](#6-alarms)
7. [Dashboards](#7-dashboards)
8. [Events & Rules](#8-events--rules)
9. [Log Insights](#9-log-insights)
10. [Monitoring Best Practices](#10-monitoring-best-practices)
11. [Troubleshooting](#11-troubleshooting)
12. [Additional Resources](#12-additional-resources)

---

## 1. What is CloudWatch?

CloudWatch is a comprehensive monitoring and observability service provided by AWS. It collects, monitors, and analyzes metrics, logs, and event data from AWS resources and on-premises applications, enabling real-time monitoring and alerting.

**Key Benefits:**
- Real-time monitoring of AWS resources
- Centralized log management and analysis
- Custom metrics collection
- Automated alerting and notifications
- Dashboard visualization
- Event-driven automation
- Cost optimization insights
- Application performance monitoring

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Metrics** | Time-series data points representing measurements (e.g., CPU usage) |
| **Dimensions** | Name-value pairs that identify metrics (e.g., Instance ID) |
| **Namespaces** | Containers for metrics (e.g., AWS/EC2, AWS/RDS) |
| **Logs** | Text data from applications and systems |
| **Log Groups** | Container for log streams with the same retention policy |
| **Log Streams** | Sequence of log events from a single source |
| **Alarms** | Trigger actions based on metric thresholds |
| **Dashboards** | Visual representation of metrics and logs |
| **Events** | Changes in AWS resources or custom events |
| **Insights** | Query and analyze logs using SQL-like queries |

---

## 3. Installation & Setup

### AWS CLI Installation

```bash
# macOS
brew install awscli

# Linux
sudo apt-get install awscli

# Windows (PowerShell)
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi

# Verify installation
aws --version
```

### AWS CLI Configuration

```bash
# Configure credentials
aws configure

# Enter:
# AWS Access Key ID: [your-access-key]
# AWS Secret Access Key: [your-secret-key]
# Default region: us-east-1
# Default output format: json

# Verify configuration
aws sts get-caller-identity

# Configure profile
aws configure --profile dev
```

### CloudWatch Agent Installation

```bash
# Download agent
wget https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm

# Install
sudo rpm -U ./amazon-cloudwatch-agent.rpm

# Verify
/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -m ec2 -a status

# Create configuration
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
```

### CloudWatch Agent Configuration

```json
{
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/application.log",
            "log_group_name": "/aws/ec2/application",
            "log_stream_name": "{instance_id}"
          }
        ]
      }
    }
  },
  "metrics": {
    "metrics_collected": {
      "cpu": {
        "measurement": [
          {
            "name": "cpu_usage_idle",
            "rename": "CPU_IDLE",
            "unit": "Percent"
          }
        ],
        "metrics_collection_interval": 60
      },
      "mem": {
        "measurement": [
          {
            "name": "mem_used_percent",
            "rename": "MEM_USED",
            "unit": "Percent"
          }
        ],
        "metrics_collection_interval": 60
      },
      "disk": {
        "measurement": [
          {
            "name": "used_percent",
            "rename": "DISK_USED",
            "unit": "Percent"
          }
        ],
        "metrics_collection_interval": 60,
        "resources": [
          "/"
        ]
      }
    }
  }
}
```

### Start CloudWatch Agent

```bash
# Start agent
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -a fetch-config \
  -m ec2 \
  -s \
  -c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json

# Check status
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -m ec2 -a query

# Stop agent
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -m ec2 -a stop
```

---

## 4. Metrics

### List Metrics

```bash
# List all metrics
aws cloudwatch list-metrics

# List metrics for EC2
aws cloudwatch list-metrics \
  --namespace AWS/EC2

# List metrics with specific dimension
aws cloudwatch list-metrics \
  --namespace AWS/EC2 \
  --dimensions Name=InstanceId,Value=i-1234567890abcdef0

# List metrics for specific dimension name
aws cloudwatch list-metrics \
  --namespace AWS/RDS \
  --metric-name DatabaseConnections
```

### Get Metric Statistics

```bash
# Get CPU utilization for last hour
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 \
  --metric-name CPUUtilization \
  --dimensions Name=InstanceId,Value=i-1234567890abcdef0 \
  --start-time 2024-01-01T00:00:00Z \
  --end-time 2024-01-01T01:00:00Z \
  --period 300 \
  --statistics Average,Maximum,Minimum

# Get RDS metrics
aws cloudwatch get-metric-statistics \
  --namespace AWS/RDS \
  --metric-name DBConnections \
  --dimensions Name=DBInstanceIdentifier,Value=mydb \
  --start-time 2024-01-01T00:00:00Z \
  --end-time 2024-01-01T01:00:00Z \
  --period 60 \
  --statistics Sum,Average
```

### Custom Metrics

```bash
# Put custom metric
aws cloudwatch put-metric-data \
  --namespace MyApplication \
  --metric-name RequestCount \
  --value 100 \
  --unit Count \
  --dimensions Name=Environment,Value=production Name=Service,Value=api

# Put metric with timestamp
aws cloudwatch put-metric-data \
  --namespace MyApplication \
  --metric-name ResponseTime \
  --value 250 \
  --unit Milliseconds \
  --timestamp 2024-01-01T12:00:00Z

# Put multiple metrics
aws cloudwatch put-metric-data \
  --namespace MyApplication \
  --metric-data \
  file://metrics.json
```

### metrics.json Example

```json
[
  {
    "MetricName": "RequestCount",
    "Value": 100,
    "Unit": "Count",
    "Dimensions": [
      {
        "Name": "Environment",
        "Value": "production"
      },
      {
        "Name": "Service",
        "Value": "api"
      }
    ]
  },
  {
    "MetricName": "ErrorCount",
    "Value": 5,
    "Unit": "Count",
    "Dimensions": [
      {
        "Name": "Environment",
        "Value": "production"
      }
    ]
  }
]
```

### AWS Common Namespaces

```
AWS/EC2             - EC2 instances
AWS/RDS             - RDS databases
AWS/ELB             - Elastic Load Balancer
AWS/ALB             - Application Load Balancer
AWS/Lambda          - Lambda functions
AWS/DynamoDB        - DynamoDB tables
AWS/S3              - S3 buckets
AWS/SQS             - SQS queues
AWS/SNS             - SNS topics
AWS/CloudFront      - CloudFront distributions
AWS/API Gateway     - API Gateway
AWS/Kinesis         - Kinesis streams
AWS/ElastiCache     - ElastiCache clusters
```

---

## 5. Logs

### Create Log Group

```bash
# Create log group
aws logs create-log-group \
  --log-group-name /aws/lambda/my-function

# Create log group with retention
aws logs create-log-group \
  --log-group-name /aws/ec2/application

# Set retention policy
aws logs put-retention-policy \
  --log-group-name /aws/ec2/application \
  --retention-in-days 7

# List log groups
aws logs describe-log-groups

# List log groups with filter
aws logs describe-log-groups \
  --log-group-name-prefix /aws/lambda
```

### Create Log Stream

```bash
# Create log stream
aws logs create-log-stream \
  --log-group-name /aws/ec2/application \
  --log-stream-name instance-i-1234567890abcdef0

# List log streams
aws logs describe-log-streams \
  --log-group-name /aws/ec2/application

# List streams with filter
aws logs describe-log-streams \
  --log-group-name /aws/ec2/application \
  --log-stream-name-prefix instance-
```

### Put Log Events

```bash
# Put log event
aws logs put-log-events \
  --log-group-name /aws/ec2/application \
  --log-stream-name instance-i-1234567890abcdef0 \
  --log-events timestamp=1609459200000,message="Application started"

# Put multiple events
aws logs put-log-events \
  --log-group-name /aws/ec2/application \
  --log-stream-name instance-i-1234567890abcdef0 \
  --log-events \
    timestamp=1609459200000,message="Event 1" \
    timestamp=1609459201000,message="Event 2"
```

### Get Log Events

```bash
# Get log events
aws logs get-log-events \
  --log-group-name /aws/ec2/application \
  --log-stream-name instance-i-1234567890abcdef0

# Get events with time range
aws logs get-log-events \
  --log-group-name /aws/ec2/application \
  --log-stream-name instance-i-1234567890abcdef0 \
  --start-time 1609459200000 \
  --end-time 1609545600000

# Get events with limit
aws logs get-log-events \
  --log-group-name /aws/ec2/application \
  --log-stream-name instance-i-1234567890abcdef0 \
  --limit 50
```

### Log Subscription Filter

```bash
# Create subscription filter to Lambda
aws logs put-subscription-filter \
  --log-group-name /aws/ec2/application \
  --filter-name my-filter \
  --filter-pattern "[ERROR]" \
  --destination-arn arn:aws:lambda:us-east-1:123456789012:function:my-function

# Create subscription to Kinesis
aws logs put-subscription-filter \
  --log-group-name /aws/ec2/application \
  --filter-name my-filter \
  --filter-pattern "" \
  --destination-arn arn:aws:kinesis:us-east-1:123456789012:stream/my-stream

# List subscription filters
aws logs describe-subscription-filters \
  --log-group-name /aws/ec2/application

# Delete subscription filter
aws logs delete-subscription-filter \
  --log-group-name /aws/ec2/application \
  --filter-name my-filter
```

### Log Retention Policies

```bash
# Set retention in days
aws logs put-retention-policy \
  --log-group-name /aws/ec2/application \
  --retention-in-days 7

# Common retention values
# 1, 3, 5, 7, 14, 30, 60, 90, 120, 150, 180, 365, 400, 545, 731, 1827, 3653

# Delete retention policy (never expires)
aws logs delete-retention-policy \
  --log-group-name /aws/ec2/application
```

---

## 6. Alarms

### Create Alarms

```bash
# Create alarm for EC2 CPU
aws cloudwatch put-metric-alarm \
  --alarm-name high-cpu \
  --alarm-description "Alert when CPU > 80%" \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 2 \
  --dimensions Name=InstanceId,Value=i-1234567890abcdef0 \
  --alarm-actions arn:aws:sns:us-east-1:123456789012:my-topic

# Create alarm for RDS connections
aws cloudwatch put-metric-alarm \
  --alarm-name high-db-connections \
  --alarm-description "Alert when DB connections > 100" \
  --metric-name DatabaseConnections \
  --namespace AWS/RDS \
  --statistic Average \
  --period 60 \
  --threshold 100 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 1 \
  --dimensions Name=DBInstanceIdentifier,Value=mydb \
  --alarm-actions arn:aws:sns:us-east-1:123456789012:my-topic

# Create alarm with insufficient data handling
aws cloudwatch put-metric-alarm \
  --alarm-name low-traffic \
  --alarm-description "Alert when traffic is low" \
  --metric-name RequestCount \
  --namespace AWS/ApplicationELB \
  --statistic Sum \
  --period 300 \
  --threshold 100 \
  --comparison-operator LessThanThreshold \
  --evaluation-periods 3 \
  --treat-missing-data notBreaching \
  --dimensions Name=TargetGroup,Value=targetgroup/my-targets
```

### Alarm Actions

```bash
# Create alarm with multiple actions
aws cloudwatch put-metric-alarm \
  --alarm-name critical-alert \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 90 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 1 \
  --dimensions Name=InstanceId,Value=i-1234567890abcdef0 \
  --alarm-actions \
    arn:aws:sns:us-east-1:123456789012:my-topic \
    arn:aws:autoscaling:us-east-1:123456789012:scalingPolicy:12345678-1234-1234-1234-123456789012
```

### Manage Alarms

```bash
# List alarms
aws cloudwatch describe-alarms

# List alarms by state
aws cloudwatch describe-alarms --state-value ALARM

# List alarms for specific metric
aws cloudwatch describe-alarms \
  --metric-name CPUUtilization \
  --namespace AWS/EC2

# Get alarm details
aws cloudwatch describe-alarms \
  --alarm-names high-cpu

# Set alarm state
aws cloudwatch set-alarm-state \
  --alarm-name high-cpu \
  --state-value OK \
  --state-reason "Manual reset"

# Delete alarm
aws cloudwatch delete-alarms --alarm-names high-cpu

# Enable alarm actions
aws cloudwatch enable-alarm-actions \
  --alarm-names high-cpu

# Disable alarm actions
aws cloudwatch disable-alarm-actions \
  --alarm-names high-cpu
```

### Composite Alarms

```bash
# Create composite alarm
aws cloudwatch put-composite-alarm \
  --alarm-name production-critical \
  --alarm-description "Alert if any critical service fails" \
  --alarm-rule "ALARM(high-cpu) OR ALARM(high-memory) OR ALARM(high-disk)" \
  --actions-enabled \
  --alarm-actions arn:aws:sns:us-east-1:123456789012:critical-topic

# Complex composite alarm
aws cloudwatch put-composite-alarm \
  --alarm-name app-health \
  --alarm-rule "(ALARM(api-cpu) AND ALARM(api-memory)) OR ALARM(database-cpu)" \
  --actions-enabled \
  --alarm-actions arn:aws:sns:us-east-1:123456789012:alerts
```

### Anomaly Detection Alarms

```bash
# Create anomaly detector
aws cloudwatch put-metric-alarm \
  --alarm-name cpu-anomaly \
  --comparison-operator LessThanLowerOrGreaterThanUpperThreshold \
  --evaluation-periods 2 \
  --metrics \
    file://anomaly-metric.json \
  --threshold-metric-id e1 \
  --alarm-actions arn:aws:sns:us-east-1:123456789012:my-topic
```

### anomaly-metric.json

```json
[
  {
    "Id": "m1",
    "ReturnData": false,
    "MetricStat": {
      "Metric": {
        "Namespace": "AWS/EC2",
        "MetricName": "CPUUtilization",
        "Dimensions": [
          {
            "Name": "InstanceId",
            "Value": "i-1234567890abcdef0"
          }
        ]
      },
      "Period": 300,
      "Stat": "Average"
    }
  },
  {
    "Id": "e1",
    "Expression": "ANOMALY_DETECTION_BAND(m1, 2)",
    "ReturnData": true
  }
]
```

---

## 7. Dashboards

### Create Dashboard

```bash
# Create dashboard with metrics
aws cloudwatch put-dashboard \
  --dashboard-name my-dashboard \
  --dashboard-body file://dashboard.json

# List dashboards
aws cloudwatch list-dashboards

# Get dashboard
aws cloudwatch get-dashboard \
  --dashboard-name my-dashboard

# Delete dashboard
aws cloudwatch delete-dashboards \
  --dashboard-names my-dashboard
```

### dashboard.json Example

```json
{
  "widgets": [
    {
      "type": "metric",
      "properties": {
        "metrics": [
          [ "AWS/EC2", "CPUUtilization", { "stat": "Average" } ],
          [ ".", "NetworkIn", { "stat": "Sum" } ]
        ],
        "period": 300,
        "stat": "Average",
        "region": "us-east-1",
        "title": "EC2 Metrics"
      }
    },
    {
      "type": "log",
      "properties": {
        "query": "fields @timestamp, @message | stats count() by @logStream",
        "region": "us-east-1",
        "title": "Log Insights Query"
      }
    },
    {
      "type": "metric",
      "properties": {
        "metrics": [
          [ "AWS/RDS", "DatabaseConnections", { "dimensions": { "DBInstanceIdentifier": "mydb" } } ]
        ],
        "period": 60,
        "stat": "Average",
        "region": "us-east-1",
        "title": "RDS Metrics"
      }
    }
  ]
}
```

### Dashboard with Alarms

```json
{
  "widgets": [
    {
      "type": "metric",
      "properties": {
        "metrics": [
          [ "AWS/EC2", "CPUUtilization", { "dimensions": { "InstanceId": "i-1234567890abcdef0" } } ]
        ],
        "annotations": {
          "alarms": [
            "arn:aws:cloudwatch:us-east-1:123456789012:alarm:high-cpu"
          ]
        },
        "period": 300,
        "stat": "Average",
        "region": "us-east-1",
        "title": "CPU with Alarm Threshold"
      }
    },
    {
      "type": "metric",
      "properties": {
        "metrics": [
          [ "AWS/ApplicationELB", "TargetResponseTime" ],
          [ ".", "RequestCount" ]
        ],
        "period": 60,
        "stat": "Average",
        "region": "us-east-1",
        "yAxis": {
          "left": { "min": 0, "max": 1000 },
          "right": { "min": 0 }
        },
        "title": "Load Balancer Metrics"
      }
    }
  ]
}
```

---

## 8. Events & Rules

### Create Event Rule

```bash
# Create rule for EC2 state change
aws events put-rule \
  --name ec2-state-change \
  --event-pattern '{
    "source": ["aws.ec2"],
    "detail-type": ["EC2 Instance State-change Notification"],
    "detail": {
      "state": ["running", "stopped"]
    }
  }' \
  --state ENABLED

# Create rule for scheduled event
aws events put-rule \
  --name daily-backup \
  --schedule-expression "cron(0 2 * * ? *)" \
  --state ENABLED

# Create rule with description
aws events put-rule \
  --name sns-trigger \
  --event-pattern '{
    "source": ["custom.app"],
    "detail-type": ["Alert"]
  }' \
  --description "Trigger SNS notification on custom alerts" \
  --state ENABLED
```

### Add Target to Rule

```bash
# Add Lambda target
aws events put-targets \
  --rule ec2-state-change \
  --targets "Id"="1","Arn"="arn:aws:lambda:us-east-1:123456789012:function:my-function"

# Add SNS target
aws events put-targets \
  --rule daily-backup \
  --targets "Id"="1","Arn"="arn:aws:sns:us-east-1:123456789012:my-topic"

# Add SQS target
aws events put-targets \
  --rule sns-trigger \
  --targets "Id"="1","Arn"="arn:aws:sqs:us-east-1:123456789012:my-queue"

# Add AutoScaling target
aws events put-targets \
  --rule ec2-maintenance \
  --targets "Id"="1","Arn"="arn:aws:autoscaling:us-east-1:123456789012:autoScalingGroup:12345678:autoScalingGroupName/my-asg"
```

### Manage Event Rules

```bash
# List rules
aws events list-rules

# List rules by state
aws events list-rules --state ENABLED

# Get rule details
aws events describe-rule --name ec2-state-change

# List targets for rule
aws events list-targets-by-rule --rule ec2-state-change

# Update rule
aws events put-rule \
  --name ec2-state-change \
  --event-pattern '{"source": ["aws.ec2"]}' \
  --state DISABLED

# Remove target from rule
aws events remove-targets \
  --rule ec2-state-change \
  --ids "1"

# Delete rule
aws events delete-rule --name ec2-state-change
```

### EventBridge Patterns

```json
{
  "detail": {
    "state": [ "running", "stopped" ]
  }
}
```

```json
{
  "source": [ "aws.ec2" ],
  "detail-type": [ "EC2 Instance State-change Notification" ],
  "detail": {
    "instance-id": [ "i-1234567890abcdef0" ]
  }
}
```

```json
{
  "source": [ "aws.health" ],
  "detail-type": [ "AWS Health Event" ],
  "detail": {
    "eventTypeCategory": [ "issue", "maintenance" ]
  }
}
```

---

## 9. Log Insights

### Run Log Insights Queries

```bash
# Basic query
aws logs start-query \
  --log-group-name /aws/ec2/application \
  --start-time 1609459200 \
  --end-time 1609545600 \
  --query-string 'fields @timestamp, @message | stats count()'

# Get query results
aws logs get-query-results \
  --query-id "query-id-from-previous-command"

# Search for errors
aws logs start-query \
  --log-group-name /aws/ec2/application \
  --start-time 1609459200 \
  --end-time 1609545600 \
  --query-string 'fields @timestamp, @message | filter @message like /ERROR/ | stats count() by @logStream'
```

### Common Log Insights Queries

```sql
-- Count log events
fields @timestamp, @message 
| stats count()

-- Filter by log level
fields @timestamp, @message, @level 
| filter @level = "ERROR"

-- Count errors by log stream
fields @logStream, @message 
| filter @message like /ERROR/ 
| stats count() by @logStream

-- Get latest errors
fields @timestamp, @message, @logStream 
| filter @message like /ERROR/ 
| sort @timestamp desc 
| limit 100

-- Calculate response time percentiles
fields @duration 
| stats pct(@duration, 50), pct(@duration, 90), pct(@duration, 99)

-- Count requests by status code
fields @status 
| stats count() as requests by @status

-- Find slowest requests
fields @duration, @requestId, @path 
| sort @duration desc 
| limit 20

-- Group by hour
fields @timestamp, @duration 
| stats avg(@duration) as avg_duration by datefloor(@timestamp, 1h)

-- Multiple filters
fields @timestamp, @message 
| filter @message like /ERROR/ and @status >= 500 
| stats count()

-- Extract field with regex
fields @message 
| filter @message like /user_id=/ 
| stats count() by @message

-- Calculate average by path
fields @path, @duration 
| stats avg(@duration) by @path
```

### Advanced Queries

```sql
-- Count by multiple dimensions
fields @timestamp, @service, @environment, @status 
| stats count() as requests by @service, @environment, @status

-- Calculate metrics
fields @duration, @bytes 
| stats avg(@duration), max(@duration), pct(@duration, 99), sum(@bytes)

-- Identify outliers
fields @duration 
| stats avg(@duration) as avg_dur, pct(@duration, 99) as p99_dur 
| filter @duration > p99_dur

-- Time series analysis
fields @timestamp, @errors 
| stats sum(@errors) as error_count by datefloor(@timestamp, 5m) 
| sort @timestamp desc

-- Compare periods
fields @timestamp, @requests 
| stats sum(@requests) as daily_requests by datefloor(@timestamp, 1d)

-- Top N analysis
fields @error, @count 
| stats count() as occurrence by @error 
| sort occurrence desc 
| limit 10
```

---

## 10. Monitoring Best Practices

### Metric Best Practices

```bash
# 1. Use meaningful metric names
aws cloudwatch put-metric-data \
  --namespace MyApp \
  --metric-name OrderProcessingTime \
  --value 250 \
  --unit Milliseconds

# 2. Add relevant dimensions
aws cloudwatch put-metric-data \
  --namespace MyApp \
  --metric-name RequestCount \
  --value 100 \
  --dimensions \
    Name=Service,Value=api \
    Name=Environment,Value=production \
    Name=Region,Value=us-east-1

# 3. Set appropriate periods
# Use 60 seconds for detailed monitoring
# Use 300 seconds for standard monitoring
# Use 3600 seconds for long-term trends
```

### Alarm Strategy

```bash
# 1. Create actionable alarms
aws cloudwatch put-metric-alarm \
  --alarm-name database-connection-limit \
  --metric-name DatabaseConnections \
  --namespace AWS/RDS \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 2 \
  --alarm-description "Alert when approaching connection limit"

# 2. Use composite alarms
aws cloudwatch put-composite-alarm \
  --alarm-name app-degradation \
  --alarm-rule "ALARM(high-latency) OR ALARM(high-error-rate)" \
  --alarm-actions arn:aws:sns:us-east-1:123456789012:ops-team

# 3. Avoid alarm fatigue
# - Set thresholds based on baselines
# - Use appropriate evaluation periods
# - Group related alarms
```

### Log Best Practices

```bash
# 1. Structure logs consistently
# Include: timestamp, level, service, request_id, message

# 2. Use log levels
# INFO - normal operations
# WARN - potential issues
# ERROR - errors that need attention
# DEBUG - detailed diagnostic information

# 3. Add context to logs
# Include user ID, request ID, service name, environment

# 4. Set appropriate retention
aws logs put-retention-policy \
  --log-group-name /aws/application/prod \
  --retention-in-days 30

# 5. Use log filtering
aws logs put-subscription-filter \
  --log-group-name /aws/application/prod \
  --filter-name errors \
  --filter-pattern "[ERROR]" \
  --destination-arn arn:aws:sns:us-east-1:123456789012:alerts
```

### Dashboard Best Practices

```json
{
  "widgets": [
    {
      "type": "metric",
      "properties": {
        "title": "Application Health",
        "metrics": [
          [ "MyApp", "RequestCount", { "stat": "Sum" } ],
          [ ".", "ErrorCount", { "stat": "Sum" } ],
          [ ".", "LatencyP99", { "stat": "Average" } ]
        ],
        "period": 300,
        "region": "us-east-1",
        "yAxis": {
          "left": { "min": 0 }
        }
      }
    }
  ]
}
```

### Cost Optimization

```bash
# 1. Use metric math to reduce custom metrics
aws cloudwatch put-metric-data \
  --namespace MyApp \
  --metric-data file://metrics.json

# 2. Adjust log retention
aws logs put-retention-policy \
  --log-group-name /aws/application \
  --retention-in-days 7

# 3. Use metric filters instead of dashboards
aws logs put-metric-filter \
  --log-group-name /aws/application \
  --filter-name error-count \
  --filter-pattern "[ERROR]" \
  --metric-transformations \
    metricName=ErrorCount,metricNamespace=MyApp,metricValue=1

# 4. Consolidate dashboards
# Use shared dashboards instead of per-team dashboards
```

---

## 11. Troubleshooting

### Common Issues

```bash
# Issue: Missing metrics
# Solution: Check CloudWatch agent status
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -m ec2 -a query

# Issue: Logs not appearing
# Solution: Check IAM permissions
aws iam get-role-policy \
  --role-name my-role \
  --policy-name cloudwatch-logs

# Issue: Alarm not triggering
# Solution: Check alarm configuration
aws cloudwatch describe-alarms \
  --alarm-names my-alarm

# Issue: High costs
# Solution: Review log retention and metric generation
aws logs describe-log-groups
```

### Debugging Queries

```bash
# Enable debug logging
export AWS_DEBUG=true
aws cloudwatch describe-alarms

# Test alarm configuration
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 \
  --metric-name CPUUtilization \
  --dimensions Name=InstanceId,Value=i-1234567890abcdef0 \
  --start-time 2024-01-01T00:00:00Z \
  --end-time 2024-01-01T01:00:00Z \
  --period 300 \
  --statistics Average

# Validate event rules
aws events describe-rule --name my-rule
aws events list-targets-by-rule --rule my-rule
```

### Performance Optimization

```bash
# 1. Adjust metric collection intervals
# Higher intervals = lower cost, less data
# Lower intervals = higher cost, more data

# 2. Filter logs at source
# Use subscription filters to reduce data

# 3. Use metric math
# Combine multiple metrics into one calculation

# 4. Batch API calls
# Use put-metric-data with multiple metrics
```

---

## 12. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **CloudWatch Guide** | https://docs.aws.amazon.com/cloudwatch/ |
| **CloudWatch Agent** | https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html |
| **Log Insights Queries** | https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html |
| **Metrics** | https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html |
| **Alarms** | https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html |
| **EventBridge** | https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html |
| **Pricing** | https://aws.amazon.com/cloudwatch/pricing/ |

### AWS Services Integration

| Service | Integration |
|---------|-------------|
| **EC2** | Native metrics (CPU, Network, Disk) |
| **RDS** | Database metrics and logs |
| **Lambda** | Function invocations and errors |
| **ELB/ALB** | Load balancer metrics |
| **DynamoDB** | Table and index metrics |
| **S3** | Request metrics |
| **SNS** | Topic metrics |
| **SQS** | Queue metrics |
| **API Gateway** | API metrics and logs |

### Learning Resources

| Resource | URL |
|----------|-----|
| **AWS Training** | https://www.aws.training/ |
| **CloudWatch Workshop** | https://cloudwatchworkshop.com/ |
| **Stack Overflow** | https://stackoverflow.com/questions/tagged/amazon-cloudwatch |
| **AWS Blog** | https://aws.amazon.com/blogs/aws/ |
| **GitHub Examples** | https://github.com/aws-samples |

---

## Quick Reference

```bash
# List all metrics
aws cloudwatch list-metrics

# Create log group
aws logs create-log-group --log-group-name /app/logs

# Create alarm
aws cloudwatch put-metric-alarm \
  --alarm-name my-alarm \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 2

# Put custom metric
aws cloudwatch put-metric-data \
  --namespace MyApp \
  --metric-name RequestCount \
  --value 100

# Run Log Insights query
aws logs start-query \
  --log-group-name /app/logs \
  --start-time 1609459200 \
  --end-time 1609545600 \
  --query-string 'fields @timestamp, @message | stats count()'

# Create dashboard
aws cloudwatch put-dashboard \
  --dashboard-name my-dashboard \
  --dashboard-body file://dashboard.json

# Create event rule
aws events put-rule \
  --name my-rule \
  --event-pattern '{"source": ["aws.ec2"]}'

# Get metric statistics
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 \
  --metric-name CPUUtilization \
  --dimensions Name=InstanceId,Value=i-1234567890abcdef0 \
  --start-time 2024-01-01T00:00:00Z \
  --end-time 2024-01-01T01:00:00Z \
  --period 300 \
  --statistics Average
```

---

**Last Updated:** 2026
**CloudWatch API Version:** Latest
**License:** AWS License

For latest information, visit [AWS CloudWatch Documentation](https://docs.aws.amazon.com/cloudwatch/)