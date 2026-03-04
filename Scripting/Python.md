# Python Scripting for DevOps & Cloud Cheatsheet

![Python Logo](https://www.python.org/static/community_logos/python-logo.png)

---

## Table of Contents
1. [What is Python for DevOps?](#1-what-is-python-for-devops)
2. [Script Basics & Setup](#2-script-basics--setup)
3. [Variables & Data Types](#3-variables--data-types)
4. [Operators & Expressions](#4-operators--expressions)
5. [Control Structures](#5-control-structures)
6. [Functions & Modules](#6-functions--modules)
7. [File & System Operations](#7-file--system-operations)
8. [Networking & HTTP](#8-networking--http)
9. [Cloud & Infrastructure](#9-cloud--infrastructure)
10. [Process & Automation](#10-process--automation)
11. [Best Practices](#11-best-practices)
12. [Practical Examples](#12-practical-examples)

---

## 1. What is Python for DevOps?

Python is a high-level programming language widely used in DevOps for automation, infrastructure management, cloud deployment, and system administration. It provides powerful libraries for AWS, Azure, GCP, Docker, Kubernetes, and Linux system operations.

**Key Benefits:**
- Easy to learn and read
- Extensive DevOps libraries
- Cloud platform SDKs (boto3, azure-sdk)
- System automation capabilities
- Cross-platform compatibility
- Rich ecosystem for DevOps tools
- Strong community support
- Powerful data structures

---

## 2. Script Basics & Setup

### Creating & Running Python Scripts

```bash
# Create Python script
nano script.py
touch script.py

# Script with shebang
cat > script.py << 'EOF'
#!/usr/bin/env python3
"""
Script description
Author: DevOps Team
Date: 2024-01-01
"""

print("Hello from Python")
EOF

# Make executable
chmod +x script.py

# Run script
python3 script.py
./script.py                          # With shebang
python script.py
```

### Virtual Environment Setup

```bash
# Create virtual environment
python3 -m venv devops-env
python3 -m venv /opt/app/venv

# Activate environment
source devops-env/bin/activate      # Linux/macOS
devops-env\Scripts\activate         # Windows

# Deactivate
deactivate

# Install packages in venv
pip install boto3 requests paramiko
pip install -r requirements.txt

# Create requirements file
pip freeze > requirements.txt

# Deactivate
deactivate
```

### Basic Script Structure

```python
#!/usr/bin/env python3
"""
Module docstring describing script
Author: Name
Date: 2024-01-01
Version: 1.0
"""

import sys
import logging
from typing import List, Dict

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)

def main() -> int:
    """Main entry point"""
    try:
        logger.info("Starting script")
        # Main logic here
        logger.info("Script completed successfully")
        return 0
    except Exception as e:
        logger.error(f"Error: {e}")
        return 1

if __name__ == "__main__":
    sys.exit(main())
```

---

## 3. Variables & Data Types

### Basic Variable Types

```python
# String
name = "DevOps"
message = "Multi-line\nstring"
raw_string = r"C:\Users\Admin"      # Raw string (no escape)
f-string = f"Hello {name}"          # F-string (formatted)

# Integer
count = 42
negative = -10
big_number = 1_000_000              # Underscore for readability

# Float
price = 19.99
ratio = 3.14

# Boolean
is_active = True
is_production = False

# None
value = None

# Lists (ordered, mutable)
servers = ["web-01", "web-02", "db-01"]
numbers = [1, 2, 3, 4, 5]
mixed = [1, "two", 3.0, True]

# Tuples (ordered, immutable)
coordinates = (10, 20)
rgb = (255, 128, 0)

# Dictionary (key-value)
config = {
    "host": "localhost",
    "port": 8080,
    "debug": True,
    "servers": ["web-01", "web-02"]
}

# Set (unique values)
tags = {"production", "critical", "monitored"}
```

### Type Checking & Conversion

```python
# Check type
type(variable)                      # Returns type
isinstance(value, int)              # Check if int
isinstance(value, (int, float))     # Check multiple

# Type hints (for documentation)
def process_file(filename: str) -> bool:
    """Process file and return success status"""
    return True

def calculate(a: int, b: int) -> int:
    return a + b

# Type conversion
int("42")                           # Convert to int
str(42)                             # Convert to string
float("3.14")                       # Convert to float
bool(1)                             # Convert to bool
list("abc")                         # Convert to list
dict([("a", 1), ("b", 2)])        # Convert to dict
```

### String Operations

```python
# String concatenation
greeting = "Hello" + " " + "World"
greeting = f"Hello {name}"
greeting = "Hello {}".format(name)

# String methods
text = "  Hello World  "
text.strip()                        # Remove whitespace
text.lower()                        # Lowercase
text.upper()                        # Uppercase
text.replace("World", "Python")
text.split(" ")                     # Split into list
",".join(["a", "b", "c"])         # Join list

# String formatting
f"{value:.2f}"                      # 2 decimal places
f"{value:10}"                       # Width 10
f"{value:,}"                        # Thousands separator
f"{value:.0%}"                      # Percentage

# Check string
"world" in "hello world"            # True
text.startswith("Hello")
text.endswith("World")
text.find("o")                      # Find index
```

---

## 4. Operators & Expressions

### Arithmetic Operators

```python
# Basic operations
a = 10
b = 3

a + b                               # 13 (addition)
a - b                               # 7 (subtraction)
a * b                               # 30 (multiplication)
a / b                               # 3.333... (division)
a // b                              # 3 (floor division)
a % b                               # 1 (modulo)
a ** b                              # 1000 (exponent)

# Assignment operators
x = 5
x += 3                              # x = x + 3
x -= 2                              # x = x - 2
x *= 2                              # x = x * 2
x /= 4                              # x = x / 4
```

### Comparison Operators

```python
# Comparison
a = 5
b = 10

a == b                              # False (equal)
a != b                              # True (not equal)
a < b                               # True (less than)
a <= b                              # True (less or equal)
a > b                               # False (greater than)
a >= b                              # False (greater or equal)

# Membership
"hello" in "hello world"            # True
5 in [1, 2, 3, 4, 5]              # True
"name" in {"name": "value"}        # True (key)

# Identity
x = None
x is None                           # True
x is not None                       # False
a is b                              # Same object?
```

### Logical Operators

```python
# AND / OR / NOT
True and True                       # True
True and False                      # False
True or False                       # True
not True                            # False
not False                           # True

# Complex conditions
if age > 18 and verified and country == "US":
    print("Access granted")

if status == "active" or status == "pending":
    process()
```

---

## 5. Control Structures

### If/Elif/Else

```python
# Simple if
if condition:
    print("True")

# If-else
if age >= 18:
    print("Adult")
else:
    print("Minor")

# If-elif-else
if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "F"

# Ternary operator
status = "active" if count > 0 else "inactive"
message = "Valid" if len(password) >= 8 else "Too short"

# One-liner with if-else
print("Adult") if age >= 18 else print("Minor")
```

### For Loops

```python
# Loop through list
servers = ["web-01", "web-02", "db-01"]
for server in servers:
    print(f"Connecting to {server}")

# Loop with range
for i in range(5):                  # 0, 1, 2, 3, 4
    print(i)

for i in range(1, 10, 2):           # 1, 3, 5, 7, 9 (step=2)
    print(i)

# Loop with index
for index, server in enumerate(servers):
    print(f"{index}: {server}")

# Loop through dictionary
config = {"host": "localhost", "port": 8080}
for key, value in config.items():
    print(f"{key}: {value}")

# Loop with range and list
for i in range(len(servers)):
    print(f"{i}: {servers[i]}")
```

### While Loops

```python
# While loop
count = 0
while count < 5:
    print(count)
    count += 1

# While True with break
while True:
    user_input = input("Enter command (q to quit): ")
    if user_input == "q":
        break
    print(f"Processing: {user_input}")

# While with continue
count = 0
while count < 10:
    count += 1
    if count % 2 == 0:
        continue
    print(count)                    # Print odd numbers only
```

### Try/Except (Error Handling)

```python
# Basic try-except
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")

# Multiple exceptions
try:
    file = open("config.yml")
    data = file.read()
except FileNotFoundError:
    print("File not found")
except IOError:
    print("IO Error")

# Catch all exceptions
try:
    # risky code
    pass
except Exception as e:
    print(f"Error: {e}")

# Finally block
try:
    connection = open_database()
except ConnectionError:
    print("Connection failed")
finally:
    if connection:
        connection.close()

# Raise exceptions
if age < 0:
    raise ValueError("Age cannot be negative")

# Custom exception
class ConfigError(Exception):
    pass

raise ConfigError("Invalid configuration")
```

---

## 6. Functions & Modules

### Function Definition

```python
# Basic function
def greet():
    print("Hello")

greet()

# Function with parameters
def greet(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

greet("Alice")
greet("Bob", "Hi")

# Return value
def add(a, b):
    return a + b

result = add(5, 3)

# Multiple return values
def get_status():
    return "active", 200, {"message": "OK"}

status, code, data = get_status()

# Type hints
def process(filename: str, verbose: bool = False) -> bool:
    """Process file and return success status"""
    return True

# Docstring
def calculate(a: int, b: int) -> int:
    """
    Calculate sum of two numbers
    
    Args:
        a: First number
        b: Second number
    
    Returns:
        Sum of a and b
    """
    return a + b
```

### Advanced Functions

```python
# *args (variable positional arguments)
def sum_all(*args):
    return sum(args)

sum_all(1, 2, 3, 4, 5)              # 15

# **kwargs (variable keyword arguments)
def print_config(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_config(host="localhost", port=8080, debug=True)

# Combining args and kwargs
def configure(required, *args, **kwargs):
    print(f"Required: {required}")
    print(f"Args: {args}")
    print(f"Kwargs: {kwargs}")

# Default arguments
def deploy(environment="staging", force=False):
    print(f"Deploying to {environment}")
    if force:
        print("Force deployment enabled")

# Lambda (anonymous function)
square = lambda x: x ** 2
square(5)                           # 25

numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))  # [1, 4, 9, 16, 25]
```

### Modules & Imports

```python
# Import entire module
import os
import sys

os.getcwd()
sys.exit(0)

# Import specific items
from pathlib import Path
from datetime import datetime
from json import loads, dumps

# Import with alias
import yaml as yml
from kubernetes import client as k8s_client

# Import all from module
from os import *                    # Not recommended

# Create your own module
# my_utils.py
def calculate_cpu_percent(used, total):
    return (used / total) * 100

def format_bytes(bytes):
    for unit in ['B', 'KB', 'MB', 'GB']:
        if bytes < 1024:
            return f"{bytes:.2f}{unit}"
        bytes /= 1024

# Use module
from my_utils import calculate_cpu_percent, format_bytes
```

---

## 7. File & System Operations

### File Operations

```python
# Read file
with open("config.txt", "r") as f:
    content = f.read()
    
# Read line by line
with open("config.txt", "r") as f:
    for line in f:
        print(line.strip())

# Read as list
with open("config.txt", "r") as f:
    lines = f.readlines()

# Write file
with open("output.txt", "w") as f:
    f.write("Hello\n")
    f.write("World\n")

# Append file
with open("output.txt", "a") as f:
    f.write("Appended line\n")

# JSON file operations
import json

# Read JSON
with open("config.json", "r") as f:
    config = json.load(f)

# Write JSON
with open("output.json", "w") as f:
    json.dump(config, f, indent=2)

# YAML file operations
import yaml

# Read YAML
with open("config.yml", "r") as f:
    config = yaml.safe_load(f)

# Write YAML
with open("output.yml", "w") as f:
    yaml.dump(config, f, default_flow_style=False)
```

### System Operations

```python
import os
import sys
import subprocess
import shutil
from pathlib import Path

# Path operations
Path.home()                         # Home directory
Path.cwd()                          # Current directory
Path("/path/to/file.txt")

# File/directory existence
Path("file.txt").exists()
Path("file.txt").is_file()
Path("directory").is_dir()

# Create directory
Path("new_dir").mkdir(parents=True, exist_ok=True)

# Remove file/directory
Path("file.txt").unlink()           # Remove file
shutil.rmtree("directory")          # Remove directory

# Copy file
shutil.copy("source.txt", "dest.txt")
shutil.copytree("src_dir", "dest_dir")

# Move file
shutil.move("old.txt", "new.txt")

# List directory
list(Path(".").glob("*.py"))
list(Path(".").rglob("*.py"))       # Recursive

# File size
Path("file.txt").stat().st_size

# Get file info
import os
os.path.getsize("file.txt")
os.path.getmtime("file.txt")
os.path.getctime("file.txt")
```

### Running System Commands

```python
import subprocess

# Run command and capture output
result = subprocess.run(
    ["ls", "-la"],
    capture_output=True,
    text=True
)
print(result.stdout)
print(result.stderr)
print(result.returncode)

# Run with shell
result = subprocess.run(
    "df -h | grep '/dev'",
    shell=True,
    capture_output=True,
    text=True
)

# Run command with timeout
try:
    result = subprocess.run(
        ["ping", "8.8.8.8"],
        timeout=5,
        capture_output=True
    )
except subprocess.TimeoutExpired:
    print("Command timed out")

# Execute and return output directly
output = subprocess.check_output(["whoami"], text=True)

# Get environment variables
import os
os.environ.get("PATH")
os.environ["MY_VAR"] = "value"
os.getenv("HOME")
```

---

## 8. Networking & HTTP

### HTTP Requests

```python
import requests
import json

# GET request
response = requests.get("https://api.example.com/users")
print(response.status_code)
print(response.text)
print(response.json())

# GET with parameters
params = {"limit": 10, "offset": 0}
response = requests.get(
    "https://api.example.com/users",
    params=params
)

# POST request
data = {"name": "John", "email": "john@example.com"}
response = requests.post(
    "https://api.example.com/users",
    json=data,
    headers={"Content-Type": "application/json"}
)

# Headers and authentication
headers = {
    "Authorization": "Bearer token123",
    "User-Agent": "MyApp/1.0"
}
response = requests.get(
    "https://api.example.com/data",
    headers=headers
)

# Basic authentication
response = requests.get(
    "https://api.example.com/secure",
    auth=("username", "password")
)

# Timeout and retries
from requests.adapters import HTTPAdapter
from requests.packages.urllib3.util.retry import Retry

session = requests.Session()
retry = Retry(total=3, backoff_factor=0.5)
adapter = HTTPAdapter(max_retries=retry)
session.mount("http://", adapter)
session.mount("https://", adapter)

response = session.get("https://api.example.com/data", timeout=10)

# Error handling
try:
    response = requests.get("https://api.example.com", timeout=5)
    response.raise_for_status()
except requests.exceptions.Timeout:
    print("Request timed out")
except requests.exceptions.ConnectionError:
    print("Connection error")
except requests.exceptions.HTTPError as e:
    print(f"HTTP error: {e}")
```

### SSH & Remote Execution

```python
import paramiko

# SSH connection
ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect("192.168.1.10", username="admin", password="password")

# Execute command
stdin, stdout, stderr = ssh.exec_command("ls -la")
print(stdout.read().decode())

# Close connection
ssh.close()

# With context manager
with paramiko.SSHClient() as ssh:
    ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    ssh.connect("192.168.1.10", username="admin", password="password")
    stdin, stdout, stderr = ssh.exec_command("whoami")
    print(stdout.read().decode())

# SSH key authentication
private_key = paramiko.RSAKey.from_private_key_file("/home/user/.ssh/id_rsa")
ssh.connect("192.168.1.10", username="admin", pkey=private_key)

# SFTP file transfer
sftp = ssh.open_sftp()
sftp.put("local_file.txt", "/tmp/remote_file.txt")
sftp.get("/tmp/remote_file.txt", "downloaded_file.txt")
sftp.close()
```

---

## 9. Cloud & Infrastructure

### AWS (boto3)

```python
import boto3
import json

# Initialize clients
ec2 = boto3.client("ec2", region_name="us-east-1")
s3 = boto3.client("s3")
rds = boto3.client("rds")

# EC2 Operations
# List instances
response = ec2.describe_instances()
for reservation in response['Reservations']:
    for instance in reservation['Instances']:
        print(f"ID: {instance['InstanceId']}")
        print(f"State: {instance['State']['Name']}")

# Launch instance
response = ec2.run_instances(
    ImageId="ami-0c55b159cbfafe1f0",
    MinCount=1,
    MaxCount=1,
    InstanceType="t2.micro",
    TagSpecifications=[{
        'ResourceType': 'instance',
        'Tags': [{'Key': 'Name', 'Value': 'web-server'}]
    }]
)
instance_id = response['Instances'][0]['InstanceId']

# Stop/Start instance
ec2.stop_instances(InstanceIds=[instance_id])
ec2.start_instances(InstanceIds=[instance_id])

# S3 Operations
# List buckets
response = s3.list_buckets()
for bucket in response['Buckets']:
    print(bucket['Name'])

# Upload file
s3.upload_file("local_file.txt", "my-bucket", "remote_file.txt")

# Download file
s3.download_file("my-bucket", "remote_file.txt", "local_file.txt")

# List objects
response = s3.list_objects_v2(Bucket="my-bucket")
for obj in response.get('Contents', []):
    print(obj['Key'])

# RDS Operations
# List databases
response = rds.describe_db_instances()
for db in response['DBInstances']:
    print(f"DB: {db['DBInstanceIdentifier']}")
```

### Docker

```python
import docker
import json

# Connect to Docker daemon
client = docker.from_env()

# List images
for image in client.images.list():
    print(image.tags)

# Pull image
image = client.images.pull("ubuntu", "latest")

# Run container
container = client.containers.run(
    "ubuntu:latest",
    "echo 'Hello from container'",
    detach=False
)
print(container.logs())

# List running containers
for container in client.containers.list():
    print(f"Container: {container.name}")
    print(f"Status: {container.status}")

# Stop container
container.stop()

# Remove container
container.remove()

# Build image
image, build_logs = client.images.build(
    path="/path/to/dockerfile",
    tag="my-app:latest"
)

# Docker compose
from docker.compose import run, up, down

# Execute command in container
container = client.containers.get("container_name")
result = container.exec_run("ls /app")
print(result.output.decode())
```

### Kubernetes

```python
from kubernetes import client, config, watch

# Load kubeconfig
config.load_kube_config()

# Create client
v1 = client.CoreV1Api()
apps_v1 = client.AppsV1Api()

# List pods
pods = v1.list_pod_for_all_namespaces(watch=False)
for pod in pods.items:
    print(f"Namespace: {pod.metadata.namespace}")
    print(f"Pod: {pod.metadata.name}")

# List services
services = v1.list_service_for_all_namespaces()
for service in services.items:
    print(f"Service: {service.metadata.name}")

# List deployments
deployments = apps_v1.list_namespaced_deployment("default")
for deployment in deployments.items:
    print(f"Deployment: {deployment.metadata.name}")

# Create deployment
deployment_manifest = {
    "apiVersion": "apps/v1",
    "kind": "Deployment",
    "metadata": {"name": "my-app"},
    "spec": {
        "replicas": 3,
        "selector": {"matchLabels": {"app": "my-app"}},
        "template": {
            "metadata": {"labels": {"app": "my-app"}},
            "spec": {
                "containers": [{
                    "name": "app",
                    "image": "my-app:latest"
                }]
            }
        }
    }
}
apps_v1.create_namespaced_deployment("default", deployment_manifest)

# Watch pods
w = watch.Watch()
for event in w.stream(v1.list_pod_for_all_namespaces):
    print(f"Event: {event['type']}")
    print(f"Pod: {event['object'].metadata.name}")
```

---

## 10. Process & Automation

### Configuration Management

```python
import yaml
import json
import configparser

# Parse YAML
with open("config.yml") as f:
    config = yaml.safe_load(f)
    print(config["database"]["host"])

# Parse JSON
with open("config.json") as f:
    config = json.load(f)
    print(config["app"]["debug"])

# Parse INI
config = configparser.ConfigParser()
config.read("config.ini")
print(config.get("database", "host"))

# Environment variables
import os
db_host = os.getenv("DB_HOST", "localhost")
db_port = os.getenv("DB_PORT", 5432)

# Configuration class
class Config:
    def __init__(self):
        self.db_host = os.getenv("DB_HOST", "localhost")
        self.db_port = int(os.getenv("DB_PORT", 5432))
        self.debug = os.getenv("DEBUG", "False") == "True"
    
    def __repr__(self):
        return f"Config(host={self.db_host}, port={self.db_port})"

config = Config()
```

### Logging & Monitoring

```python
import logging
import logging.handlers
from datetime import datetime

# Basic logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

logger = logging.getLogger(__name__)
logger.info("Application started")
logger.warning("Warning message")
logger.error("Error message")
logger.debug("Debug message")

# File logging
handler = logging.FileHandler("app.log")
handler.setLevel(logging.ERROR)
formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')
handler.setFormatter(formatter)
logger.addHandler(handler)

# Rotating file handler
from logging.handlers import RotatingFileHandler
handler = RotatingFileHandler(
    "app.log",
    maxBytes=10*1024*1024,           # 10MB
    backupCount=5
)
logger.addHandler(handler)

# Time-based rotation
from logging.handlers import TimedRotatingFileHandler
handler = TimedRotatingFileHandler(
    "app.log",
    when="midnight",
    interval=1,
    backupCount=7
)
logger.addHandler(handler)

# Custom logging format
logger = logging.getLogger("myapp")
formatter = logging.Formatter(
    '%(asctime)s | %(name)s | %(levelname)s | %(message)s',
    datefmt='%Y-%m-%d %H:%M:%S'
)
```

### Scheduling & Cron

```python
import schedule
import time
from datetime import datetime

# Simple scheduling
def job():
    print(f"Running job at {datetime.now()}")

schedule.every(10).seconds.do(job)
schedule.every(5).minutes.do(job)
schedule.every().hour.do(job)
schedule.every().day.at("10:30").do(job)
schedule.every().wednesday.at("13:15").do(job)

# Run scheduler
while True:
    schedule.run_pending()
    time.sleep(1)

# APScheduler (more advanced)
from apscheduler.schedulers.background import BackgroundScheduler

scheduler = BackgroundScheduler()
scheduler.add_job(job, 'interval', seconds=10)
scheduler.add_job(job, 'cron', hour=10, minute=30)
scheduler.start()

# Keep program running
try:
    while True:
        time.sleep(1)
except KeyboardInterrupt:
    scheduler.shutdown()
```

---

## 11. Best Practices

### Code Style & Standards

```python
# PEP 8 - Python Style Guide

# Good naming
user_count = 10
is_active = True
get_user_by_id()

# Bad naming
x = 10
b = True
fetchuser()

# Line length (max 79 characters)
config = load_config(
    filename="config.yml",
    validate=True
)

# Imports organization
import os
import sys
from datetime import datetime

import requests
import yaml

from my_module import my_function

# Docstrings
def deploy_service(service_name: str, version: str) -> bool:
    """
    Deploy service to production
    
    Args:
        service_name: Name of service to deploy
        version: Version to deploy
    
    Returns:
        True if deployment successful
    
    Raises:
        ServiceNotFound: If service doesn't exist
    """
    pass
```

### Error Handling & Validation

```python
# Input validation
def create_user(email: str, age: int) -> bool:
    if not email or "@" not in email:
        raise ValueError("Invalid email")
    if age < 0 or age > 150:
        raise ValueError("Invalid age")
    return True

# Comprehensive error handling
try:
    response = requests.get(url, timeout=5)
    response.raise_for_status()
    data = response.json()
except requests.exceptions.Timeout:
    logger.error("Request timed out")
    return None
except requests.exceptions.HTTPError as e:
    logger.error(f"HTTP error: {e.response.status_code}")
    return None
except json.JSONDecodeError:
    logger.error("Invalid JSON response")
    return None
except Exception as e:
    logger.error(f"Unexpected error: {e}")
    return None

# Custom exceptions
class ConfigurationError(Exception):
    """Configuration is invalid"""
    pass

class ServiceUnavailable(Exception):
    """Service is not available"""
    pass

# Using context managers for cleanup
class DatabaseConnection:
    def __enter__(self):
        self.conn = connect_to_db()
        return self.conn
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.conn:
            self.conn.close()

with DatabaseConnection() as conn:
    conn.query("SELECT * FROM users")
```

### Testing & Debugging

```python
# Unit testing
import unittest

class TestCalculator(unittest.TestCase):
    def test_add(self):
        self.assertEqual(2 + 2, 4)
    
    def test_divide_by_zero(self):
        with self.assertRaises(ZeroDivisionError):
            1 / 0

# Run tests
if __name__ == "__main__":
    unittest.main()

# Debugging
import pdb

def problematic_function():
    x = 5
    pdb.set_trace()                 # Breakpoint
    y = x * 2
    return y

# Print debugging
print(f"Variable: {var}")
import sys
print(f"Debug info: {info}", file=sys.stderr)
```

---

## 12. Practical Examples

### AWS EC2 Auto-Scaling Script

```python
#!/usr/bin/env python3
"""
AWS EC2 Instance Monitor and Auto-Scaler
Monitors CPU usage and scales instances
"""

import boto3
import logging
from datetime import datetime, timedelta

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class EC2Scaler:
    def __init__(self, region="us-east-1"):
        self.ec2 = boto3.client("ec2", region_name=region)
        self.cloudwatch = boto3.client("cloudwatch", region_name=region)
    
    def get_instances_by_tag(self, tag_key, tag_value):
        """Get instances with specific tag"""
        response = self.ec2.describe_instances(
            Filters=[
                {'Name': f'tag:{tag_key}', 'Values': [tag_value]},
                {'Name': 'instance-state-name', 'Values': ['running']}
            ]
        )
        instances = []
        for reservation in response['Reservations']:
            instances.extend(reservation['Instances'])
        return instances
    
    def get_cpu_utilization(self, instance_id):
        """Get average CPU utilization for last 5 minutes"""
        response = self.cloudwatch.get_metric_statistics(
            Namespace='AWS/EC2',
            MetricName='CPUUtilization',
            Dimensions=[
                {'Name': 'InstanceId', 'Value': instance_id}
            ],
            StartTime=datetime.utcnow() - timedelta(minutes=5),
            EndTime=datetime.utcnow(),
            Period=300,
            Statistics=['Average']
        )
        
        if response['Datapoints']:
            return response['Datapoints'][0]['Average']
        return 0
    
    def scale(self, tag_key="Environment", tag_value="production"):
        """Scale instances based on CPU usage"""
        instances = self.get_instances_by_tag(tag_key, tag_value)
        
        for instance in instances:
            instance_id = instance['InstanceId']
            cpu = self.get_cpu_utilization(instance_id)
            
            logger.info(f"Instance {instance_id}: CPU {cpu:.2f}%")
            
            if cpu > 80:
                logger.warning(f"High CPU on {instance_id}")
                # Scale up logic here
            elif cpu < 20:
                logger.info(f"Low CPU on {instance_id}")
                # Scale down logic here

if __name__ == "__main__":
    scaler = EC2Scaler()
    scaler.scale()
```

### Server Health Check Script

```python
#!/usr/bin/env python3
"""
Server Health Check Script
Monitors multiple servers and reports status
"""

import requests
import json
import logging
from typing import Dict, List
from datetime import datetime

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class HealthCheck:
    def __init__(self, timeout=5):
        self.timeout = timeout
        self.results = []
    
    def check_server(self, url: str, expected_status=200) -> Dict:
        """Check single server health"""
        try:
            response = requests.get(url, timeout=self.timeout)
            status = "UP" if response.status_code == expected_status else "DOWN"
            return {
                "url": url,
                "status": status,
                "status_code": response.status_code,
                "response_time": response.elapsed.total_seconds(),
                "timestamp": datetime.now().isoformat()
            }
        except requests.exceptions.Timeout:
            return {
                "url": url,
                "status": "DOWN",
                "error": "Timeout",
                "timestamp": datetime.now().isoformat()
            }
        except Exception as e:
            return {
                "url": url,
                "status": "DOWN",
                "error": str(e),
                "timestamp": datetime.now().isoformat()
            }
    
    def check_servers(self, urls: List[str]) -> List[Dict]:
        """Check multiple servers"""
        self.results = []
        for url in urls:
            result = self.check_server(url)
            self.results.append(result)
            logger.info(f"{url}: {result['status']}")
        return self.results
    
    def generate_report(self) -> str:
        """Generate health report"""
        up_count = sum(1 for r in self.results if r['status'] == 'UP')
        down_count = len(self.results) - up_count
        
        report = f"""
        Health Check Report
        Time: {datetime.now().isoformat()}
        Total Servers: {len(self.results)}
        Up: {up_count}
        Down: {down_count}
        
        Details:
        """
        for result in self.results:
            report += f"\n  {result['url']}: {result['status']}"
        
        return report

if __name__ == "__main__":
    servers = [
        "http://web-01.example.com",
        "http://web-02.example.com",
        "http://api.example.com",
        "http://db.example.com"
    ]
    
    checker = HealthCheck()
    checker.check_servers(servers)
    print(checker.generate_report())
```

### Configuration Deployment Script

```python
#!/usr/bin/env python3
"""
Configuration Deployment Script
Deploy configurations to multiple servers via SSH
"""

import paramiko
import yaml
import logging
import sys
from pathlib import Path

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class ConfigDeployer:
    def __init__(self, key_file=None):
        self.key_file = key_file
    
    def deploy_config(self, host: str, config_path: str, 
                     remote_path: str, username: str) -> bool:
        """Deploy configuration file to remote host"""
        try:
            ssh = paramiko.SSHClient()
            ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
            
            # Connect with key or password
            if self.key_file:
                key = paramiko.RSAKey.from_private_key_file(self.key_file)
                ssh.connect(host, username=username, pkey=key)
            else:
                ssh.connect(host, username=username)
            
            # Transfer file
            sftp = ssh.open_sftp()
            sftp.put(config_path, remote_path)
            sftp.close()
            
            logger.info(f"Deployed {config_path} to {host}:{remote_path}")
            ssh.close()
            return True
        
        except Exception as e:
            logger.error(f"Failed to deploy to {host}: {e}")
            return False
    
    def deploy_to_all(self, hosts: list, config_path: str, 
                     remote_path: str, username: str):
        """Deploy to all hosts"""
        results = {}
        for host in hosts:
            results[host] = self.deploy_config(
                host, config_path, remote_path, username
            )
        return results

if __name__ == "__main__":
    deployer = ConfigDeployer(key_file="/home/user/.ssh/id_rsa")
    
    hosts = [
        "web-01.example.com",
        "web-02.example.com",
        "api-01.example.com"
    ]
    
    results = deployer.deploy_to_all(
        hosts,
        "./config/nginx.conf",
        "/etc/nginx/nginx.conf",
        "admin"
    )
    
    print("\nDeployment Results:")
    for host, success in results.items():
        status = "SUCCESS" if success else "FAILED"
        print(f"  {host}: {status}")
```

---

## Quick Reference

```python
# File I/O
open(), read(), write(), json.load(), yaml.load()

# System
os.system(), subprocess.run(), Path, shutil

# HTTP
requests.get(), requests.post(), requests.Session()

# SSH
paramiko.SSHClient(), ssh.exec_command()

# AWS
boto3.client(), ec2, s3, rds, cloudwatch

# Docker
docker.from_env(), client.containers, client.images

# Kubernetes
client.CoreV1Api(), v1.list_pod_for_all_namespaces()

# Logging
logging.basicConfig(), logger.info(), logger.error()

# Error Handling
try/except/finally, raise, custom exceptions

# Functions
def, return, *args, **kwargs, type hints

# Data Types
str, int, float, bool, list, dict, tuple, set

# Control
if/elif/else, for, while, break, continue
```

---

**Last Updated:** 2026
**Python Version:** 3.8+
**Popular Libraries:** boto3, requests, paramiko, docker, kubernetes, pyyaml, schedule

---

**Tips:**
- Always use virtual environments
- Implement comprehensive error handling
- Log all important operations
- Use type hints for better code
- Follow PEP 8 style guide
- Write unit tests
- Use configuration files
- Avoid hardcoded values
- Implement retry logic
- Monitor and alert on failures