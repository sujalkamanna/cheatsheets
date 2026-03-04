# SaltStack Cheatsheet

![SaltStack Logo](https://saltproject.io/images/SaltProject-Logo.png)

---

## Table of Contents
1. [What is SaltStack?](#1-what-is-saltstack)
2. [Installation & Setup](#2-installation--setup)
3. [Architecture](#3-architecture)
4. [Salt Commands](#4-salt-commands)
5. [States](#5-states)
6. [Modules](#6-modules)
7. [Grains & Pillars](#7-grains--pillars)
8. [Formulas](#8-formulas)
9. [Renderers & Templates](#9-renderers--templates)
10. [Orchestration](#10-orchestration)
11. [Best Practices](#11-best-practices)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. What is SaltStack?

SaltStack is an open-source infrastructure automation and event-driven IT orchestration platform. It uses remote execution and configuration management to automate infrastructure at scale with high speed and efficiency.

**Key Features:**
- Agent-based and agentless modes
- Remote execution (commands)
- Configuration management (states)
- Event-driven automation
- High-speed message bus
- Data management (grains, pillars)
- Orchestration
- Scaling to 10,000+ minions
- Python-based modules
- Real-time communication

---

## 2. Installation & Setup

### Install Salt Master

```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install -y salt-master

# CentOS/RHEL
sudo yum install -y salt-master

# macOS
brew install saltstack

# Start master
sudo systemctl start salt-master
sudo systemctl enable salt-master

# Verify
sudo systemctl status salt-master
salt-master --version
```

### Install Salt Minion

```bash
# Ubuntu/Debian
sudo apt-get install -y salt-minion

# CentOS/RHEL
sudo yum install -y salt-minion

# macOS
brew install salt

# Configure minion
sudo nano /etc/salt/minion
# Add: master: 192.168.1.100

# Start minion
sudo systemctl start salt-minion
sudo systemctl enable salt-minion

# Verify
sudo systemctl status salt-minion
```

### Salt Configuration

```yaml
# /etc/salt/master
# Master configuration

# Interface to bind to
interface: 0.0.0.0

# Port
port: 4506

# File directory
file_roots:
  base:
    - /srv/salt/states

# Pillar directory
pillar_roots:
  base:
    - /srv/salt/pillars

# Formulas directory
extension_modules: /srv/salt/formulas

# Log level
log_level: info

# /etc/salt/minion
# Minion configuration

# Master hostname/IP
master: salt.example.com

# Minion ID
id: web1

# Log level
log_level: info

# Module directory
module_dirs:
  - /srv/salt/modules
```

### Directory Structure

```
/srv/salt/
├── states/
│   ├── top.sls                 # State declaration
│   ├── webserver.sls           # Webserver state
│   ├── database.sls            # Database state
│   └── common/
│       └── packages.sls        # Common packages
├── pillars/
│   ├── top.sls                 # Pillar declaration
│   ├── webserver.sls           # Webserver pillar
│   └── secrets.sls             # Secret data
├── formulas/                   # Reusable formulas
│   ├── nginx/
│   └── postgresql/
└── modules/                    # Custom modules
    ├── _states/
    └── _modules/
```

---

## 3. Architecture

### Salt Architecture

```
┌─────────────────────────────────────┐
│      Salt Master                    │
│  - Stores state files               │
│  - Manages minions                  │
│  - Executes commands                │
│  - ZeroMQ message bus               │
└──────────────┬──────────────────────┘
               │ (TCP 4506, 4505)
       ┌───────┼────────┐
       │       │        │
       ▼       ▼        ▼
   ┌────────┐ ┌────────┐ ┌────────┐
   │Minion  │ │Minion  │ │Minion  │
   │(Agent) │ │(Agent) │ │(Agent) │
   └────────┘ └────────┘ └────────┘
```

### Communication Flow

```
1. Minion sends request to Master
2. Master publishes command/state
3. All minions receive via ZeroMQ
4. Minions execute in parallel
5. Minions report results
6. Master aggregates results
```

---

## 4. Salt Commands

### Remote Execution

```bash
# Execute command on all minions
sudo salt '*' cmd.run 'ls -la /tmp'

# Execute on specific minion
sudo salt 'web1' cmd.run 'systemctl status nginx'

# Execute on minion pattern
sudo salt 'web*' cmd.run 'uptime'

# Execute with grain matching
sudo salt -G 'os:Ubuntu' cmd.run 'apt-get update'

# Execute with list
sudo salt -L 'web1,web2,db1' cmd.run 'free -h'

# Execute with compound query
sudo salt -C 'G@os:Ubuntu and L@web1,web2' cmd.run 'df -h'

# Execute with exclusion
sudo salt '*' -E 'db.*' cmd.run 'hostname'  # Execute on non-db minions

# Get return format
sudo salt '*' cmd.run 'uptime' --out=json
sudo salt '*' cmd.run 'uptime' --out=yaml
sudo salt '*' cmd.run 'uptime' --out=table

# Asynchronous execution
sudo salt '*' cmd.run 'long_running_command' --async

# Check job status
sudo salt-run jobs.active
sudo salt-run jobs.list_jobs
sudo salt-run jobs.lookup_jid <job_id>

# Set timeout
sudo salt '*' cmd.run 'command' -t 60

# Verbose output
sudo salt '*' cmd.run 'command' -v
```

### Common Modules

```bash
# pkg - Package management
sudo salt '*' pkg.list_pkgs
sudo salt '*' pkg.install 'nginx'
sudo salt '*' pkg.remove 'apache2'
sudo salt '*' pkg.upgrade

# service - Service management
sudo salt '*' service.get_all
sudo salt '*' service.status nginx
sudo salt '*' service.start nginx
sudo salt '*' service.stop nginx
sudo salt '*' service.restart nginx
sudo salt '*' service.enable nginx

# file - File operations
sudo salt '*' file.check_hash '/etc/hosts' md5
sudo salt '*' file.directory_exists '/var/www'
sudo salt '*' file.file_exists '/etc/nginx/nginx.conf'

# grains - Get system information
sudo salt '*' grains.items
sudo salt '*' grains.get 'os'
sudo salt '*' grains.get 'ipv4'

# pillar - Get pillar data
sudo salt '*' pillar.items
sudo salt '*' pillar.get 'webserver:port'

# network - Network information
sudo salt '*' network.ip_addrs
sudo salt '*' network.ifacerestart 'eth0'

# user - User management
sudo salt '*' user.list_users
sudo salt '*' user.add 'username' 1001

# disk - Disk operations
sudo salt '*' disk.usage
sudo salt '*' disk.inodes

# cron - Cron job management
sudo salt '*' cron.ls
sudo salt '*' cron.list_tab root
```

---

## 5. States

### Basic State File

```yaml
# /srv/salt/states/webserver.sls

# Install packages
nginx-package:
  pkg.installed:
    - name: nginx
    - require:
      - pkg: essential-packages

essential-packages:
  pkg.installed:
    - pkgs:
      - curl
      - git
      - build-essential

# Create directory
www-directory:
  file.directory:
    - name: /var/www/html
    - user: www-data
    - group: www-data
    - mode: 755

# Deploy configuration file
nginx-config:
  file.managed:
    - name: /etc/nginx/nginx.conf
    - source: salt://webserver/files/nginx.conf
    - user: root
    - group: root
    - mode: 644
    - require:
      - pkg: nginx-package

# Manage service
nginx-service:
  service.running:
    - name: nginx
    - enable: True
    - require:
      - pkg: nginx-package
    - watch:
      - file: nginx-config

# Execute command
compile-source:
  cmd.run:
    - name: cd /opt/src && ./configure && make && make install
    - creates: /usr/local/bin/myapp
```

### State with Conditions

```yaml
# /srv/salt/states/conditional.sls

install-nginx-ubuntu:
  pkg.installed:
    - name: nginx
    - onlyif: test $(lsb_release -cs) = focal

install-nginx-centos:
  pkg.installed:
    - name: nginx
    - onfail:
      - pkg: install-nginx-ubuntu

enable-firewall:
  cmd.run:
    - name: ufw enable
    - unless: ufw status | grep -q "Status: active"

create-user:
  user.present:
    - name: appuser
    - uid: 1001
    - home: /home/appuser
    - shell: /bin/bash

add-to-sudo:
  group.present:
    - name: sudo
    - members:
      - appuser
    - require:
      - user: create-user
```

### Top File (State Declaration)

```yaml
# /srv/salt/states/top.sls
base:
  '*':
    - common.packages
    - common.users
  
  'web*':
    - webserver
    - monitoring
  
  'db*':
    - database
    - backup
  
  'ubuntu*':
    - debian-specific
  
  'centos*':
    - redhat-specific

dev:
  'dev*':
    - development-tools

prod:
  'prod*':
    - production-hardening
```

### Apply States

```bash
# Apply all states (top.sls)
sudo salt '*' state.apply

# Apply specific state
sudo salt '*' state.apply webserver

# Apply state on specific minions
sudo salt 'web1,web2' state.apply

# Apply state with grain matching
sudo salt -G 'os:Ubuntu' state.apply

# Test state (dry-run)
sudo salt '*' state.apply test=True

# Get state info
sudo salt '*' state.show_top
sudo salt '*' state.show_sls webserver

# View state compilation
sudo salt '*' state.compile webserver

# State return format
sudo salt '*' state.apply --out=json
sudo salt '*' state.apply --out=yaml
```

---

## 6. Modules

### Create Custom Module

```python
# /srv/salt/modules/_modules/mymodule.py
'''
Custom module for SaltStack
'''

def hello(name):
    '''
    Return a greeting
    
    CLI Example::
    
        salt 'minion' mymodule.hello John
    '''
    return f'Hello, {name}!'

def get_app_version():
    '''
    Get application version
    '''
    import subprocess
    result = subprocess.run(['/opt/app/bin/app', '--version'], 
                          capture_output=True, text=True)
    return result.stdout.strip()

def is_service_running(service_name):
    '''
    Check if service is running
    '''
    import salt.utils.decorators as decorators
    __salt__ = salt.client.get_local_client()
    
    result = __salt__['service.status'](service_name)
    return result

def deploy_app(app_name, version):
    '''
    Deploy application
    '''
    return {
        'status': 'deployed',
        'app': app_name,
        'version': version
    }
```

### Use Custom Module

```bash
# Call custom module
sudo salt '*' mymodule.hello John
sudo salt '*' mymodule.get_app_version
sudo salt '*' mymodule.is_service_running nginx
sudo salt '*' mymodule.deploy_app 'myapp' '1.0.0'
```

### Custom State Module

```python
# /srv/salt/modules/_states/mystate.py
'''
Custom state module
'''

def app_deployed(name, version):
    '''
    Ensure application is deployed
    
    Usage::
    
        deploy-myapp:
          mystate.app_deployed:
            - version: 1.0.0
    '''
    ret = {'name': name, 'changes': {}, 'result': False, 'comment': ''}
    
    # Check if already deployed
    current_version = __salt__['mymodule.get_app_version']()
    
    if current_version == version:
        ret['result'] = True
        ret['comment'] = f'{name} already at version {version}'
        return ret
    
    # Deploy application
    result = __salt__['mymodule.deploy_app'](name, version)
    
    if result['status'] == 'deployed':
        ret['result'] = True
        ret['changes'] = {
            'old_version': current_version,
            'new_version': version
        }
        ret['comment'] = f'{name} deployed to {version}'
    else:
        ret['result'] = False
        ret['comment'] = f'Failed to deploy {name}'
    
    return ret
```

---

## 7. Grains & Pillars

### Grains (System Information)

```bash
# List all grains
sudo salt '*' grains.items

# Get specific grain
sudo salt '*' grains.get 'os'
sudo salt '*' grains.get 'ipv4'
sudo salt '*' grains.get 'cpu_model'

# Common grains
sudo salt '*' grains.get 'hostname'
sudo salt '*' grains.get 'domain'
sudo salt '*' grains.get 'fqdn'
sudo salt '*' grains.get 'oscodename'
sudo salt '*' grains.get 'mem_total'
sudo salt '*' grains.get 'num_cpus'
sudo salt '*' grains.get 'virtual'
```

### Custom Grains

```yaml
# /etc/salt/minion.d/custom_grains.conf
grains:
  environment: production
  role: webserver
  data_center: us-east-1
  custom_app: myapp

# Or in grains file
# /etc/salt/grains
environment: production
role: webserver
data_center: us-east-1
```

### Pillars (Configuration Data)

```yaml
# /srv/salt/pillars/top.sls
base:
  '*':
    - common
  
  'web*':
    - webserver
  
  'db*':
    - database

# /srv/salt/pillars/common.sls
---
ntp_servers:
  - 0.pool.ntp.org
  - 1.pool.ntp.org

# /srv/salt/pillars/webserver.sls
---
webserver:
  port: 80
  workers: 4
  user: www-data
  group: www-data

# /srv/salt/pillars/database.sls
---
database:
  engine: postgresql
  version: '12'
  backup: true
  replication: false
```

### Use Grains and Pillars

```yaml
# /srv/salt/states/config.sls

# Use grains
install-for-ubuntu:
  pkg.installed:
    - name: nginx
    - onlyif: test {{ grains.os }} = Ubuntu

hostname-config:
  file.managed:
    - name: /etc/hostname
    - contents: {{ grains.fqdn }}

# Use pillars
nginx-config:
  file.managed:
    - name: /etc/nginx/nginx.conf
    - source: salt://webserver/nginx.conf.j2
    - template: jinja
    - context:
        port: {{ pillar['webserver']['port'] }}
        workers: {{ pillar['webserver']['workers'] }}

database-setup:
  cmd.run:
    - name: |
        psql -U postgres -c "CREATE DATABASE {{ pillar['database']['name'] }};"
    - unless: psql -U postgres -lqt | cut -d \| -f 1 | grep -w {{ pillar['database']['name'] }}
```

### Refresh Grains and Pillars

```bash
# Refresh grains on minion
sudo salt '*' grains.ls
sudo salt '*' grains.delkey key_name

# Refresh pillars on minion
sudo salt '*' pillar.items
sudo salt '*' pillar.refresh_pillar

# Force refresh
sudo salt '*' saltutil.refresh_grains
sudo salt '*' saltutil.refresh_pillar_and_modules
```

---

## 8. Formulas

### Formula Structure

```
nginx-formula/
├── nginx/
│   ├── init.sls
│   ├── install.sls
│   ├── config.sls
│   └── service.sls
├── files/
│   ├── nginx.conf
│   └── default.conf
├── templates/
│   └── nginx.conf.j2
├── README.md
├── LICENSE
└── metadata.yaml
```

### Create Formula

```yaml
# nginx-formula/nginx/init.sls
include:
  - nginx.install
  - nginx.config
  - nginx.service

# nginx-formula/nginx/install.sls
nginx:
  pkg.installed: []

# nginx-formula/nginx/config.sls
/etc/nginx/nginx.conf:
  file.managed:
    - source: salt://nginx/files/nginx.conf
    - user: root
    - group: root
    - mode: 644
    - require:
      - pkg: nginx

# nginx-formula/nginx/service.sls
nginx-service:
  service.running:
    - name: nginx
    - enable: True
    - watch:
      - file: /etc/nginx/nginx.conf
```

### Use Formulas

```bash
# Clone formula
cd /srv/salt/formulas
git clone https://github.com/saltstack-formulas/nginx-formula.git

# Apply formula
sudo salt '*' state.apply formulas.nginx-formula.nginx

# Use with custom data
# /srv/salt/states/top.sls
base:
  'web*':
    - formulas.nginx-formula.nginx
```

---

## 9. Renderers & Templates

### Jinja Templates

```yaml
# /srv/salt/states/template_example.sls

# Simple variable substitution
nginx-config:
  file.managed:
    - name: /etc/nginx/nginx.conf
    - source: salt://webserver/nginx.conf.j2
    - template: jinja
    - context:
        port: 80
        workers: 4

# /srv/salt/webserver/nginx.conf.j2
user www-data;
worker_processes {{ workers }};
error_log /var/log/nginx/error.log;

events {
    worker_connections 1024;
}

http {
    server {
        listen {{ port }};
        server_name _;
        
        location / {
            root /var/www/html;
            index index.html;
        }
    }
}
```

### Jinja with Conditions

```yaml
# /srv/salt/states/conditional_template.sls

app-config:
  file.managed:
    - name: /etc/app/config.yml
    - source: salt://app/config.yml.j2
    - template: jinja
    - context:
        environment: {{ salt['grains.get']('environment') }}
        debug: {{ pillar.get('app:debug', False) }}

# /srv/salt/app/config.yml.j2
---
environment: {{ environment }}
debug: {{ debug }}

{% if environment == 'production' %}
database:
  host: db.prod.internal
  pool_size: 50
{% else %}
database:
  host: localhost
  pool_size: 5
{% endif %}

{% for server in servers %}
server_{{ loop.index }}:
  address: {{ server }}
{% endfor %}
```

### Mako Templates

```yaml
# /srv/salt/states/mako_template.sls

config-file:
  file.managed:
    - name: /etc/myapp/config
    - source: salt://myapp/config.mako
    - template: mako
    - context:
        port: 8080

# /srv/salt/myapp/config.mako
<%
servers = ['server1', 'server2', 'server3']
%>

<%def name="render_servers(servers)">
% for server in servers:
  - ${server}
% endfor
</%def>

port: ${port}

servers:
${render_servers(servers)}
```

---

## 10. Orchestration

### Salt Orchestration

```yaml
# /srv/salt/orchestrate/site.yaml

# Orchestration runner
upgrade_infrastructure:
  salt.state:
    - tgt: 'web*'
    - sls: webserver.upgrade
    - require_in:
      - salt: verify_deployment

verify_deployment:
  salt.function:
    - name: cmd.run
    - tgt: 'web*'
    - arg:
      - curl -f http://localhost

notify_slack:
  salt.function:
    - name: http.query
    - tgt: 'salt-master'
    - kwarg:
        url: https://hooks.slack.com/...
        method: POST

# Sequential execution
deploy_app:
  salt.state:
    - tgt: '*'
    - sls: app.deploy
    - order: 1

start_services:
  salt.state:
    - tgt: '*'
    - sls: app.start
    - order: 2
    - require:
      - salt: deploy_app

# Require other minions
web_first:
  salt.state:
    - tgt: 'web1'
    - sls: common

web_rest:
  salt.state:
    - tgt: 'web2,web3,web4'
    - sls: common
    - require:
      - salt: web_first
```

### Execute Orchestration

```bash
# Run orchestration
sudo salt-run state.orchestrate site

# Run with specific pillar
sudo salt-run state.orchestrate site pillar='{"env":"prod"}'

# Async execution
sudo salt-run -a state.orchestrate site

# Get job status
sudo salt-run jobs.active
```

---

## 11. Best Practices

### State Organization

```yaml
# Top-level organization
# /srv/salt/states/top.sls
base:
  '*':
    - common.packages
    - common.users
    - common.security
  
  'web*':
    - webserver.packages
    - webserver.config
    - webserver.service
  
  'db*':
    - database.setup
    - database.backup

# Modular states
# /srv/salt/states/webserver/init.sls
include:
  - webserver.packages
  - webserver.config
  - webserver.service

# /srv/salt/states/webserver/packages.sls
nginx:
  pkg.installed: []

# /srv/salt/states/webserver/config.sls
/etc/nginx/nginx.conf:
  file.managed:
    - source: salt://webserver/nginx.conf
    - require:
      - pkg: nginx

# /srv/salt/states/webserver/service.sls
nginx-service:
  service.running:
    - name: nginx
    - enable: True
    - watch:
      - file: /etc/nginx/nginx.conf
```

### Pillar Security

```yaml
# /srv/salt/pillars/top.sls
base:
  '*':
    - common
  
  'prod*':
    - prod.secrets

dev:
  '*':
    - dev.defaults

# /srv/salt/pillars/prod/secrets.sls
# Encrypt sensitive data
db_password: |
  -----BEGIN ENCRYPTED DATA-----
  ...encrypted content...
  -----END ENCRYPTED DATA-----

api_key: |
  -----BEGIN ENCRYPTED DATA-----
  ...encrypted content...
  -----END ENCRYPTED DATA-----
```

### Error Handling

```yaml
# /srv/salt/states/error_handling.sls

graceful-deploy:
  cmd.run:
    - name: /opt/deploy.sh
    - onfail:
      - cmd: rollback_deploy

rollback_deploy:
  cmd.run:
    - name: /opt/rollback.sh
    - require:
      - cmd: graceful-deploy

restart-with-check:
  service.running:
    - name: nginx
    - require:
      - cmd: verify_config

verify_config:
  cmd.run:
    - name: nginx -t
    - unless: nginx -t > /dev/null 2>&1
```

---

## 12. Troubleshooting

### Debug Salt

```bash
# Verbose output
sudo salt '*' cmd.run 'uptime' -v

# Debug level
sudo salt '*' cmd.run 'uptime' --log-level=debug

# Trace execution
sudo salt -l debug '*' state.apply

# Show state tree
sudo salt '*' state.show_top
sudo salt '*' state.show_sls webserver

# Compile states
sudo salt '*' state.compile webserver

# Test runner
sudo salt-run state.orch test_runner

# Check minion connectivity
sudo salt-run manage.up
sudo salt-run manage.down

# Get minion grains
sudo salt '*' grains.items

# Check pillar data
sudo salt '*' pillar.items
```

### Common Issues

```bash
# Minion not responding
# Check minion is running
sudo systemctl status salt-minion

# Check certificates
sudo salt-key -l
sudo salt-key -a minion_name

# Test connectivity
sudo salt 'minion' test.ping

# View minion logs
sudo tail -f /var/log/salt/minion

# Restart minion
sudo systemctl restart salt-minion

# Regenerate keys
sudo rm -f /etc/salt/pki/minion/{minion.pub,minion.key}
sudo systemctl restart salt-minion

# Permission issues
# Check file permissions
sudo chown -R root:root /srv/salt
sudo chmod -R 755 /srv/salt

# Module not found
# Refresh modules
sudo salt '*' saltutil.refresh_modules

# State syntax errors
# Validate YAML
python -m yaml /srv/salt/states/webserver.sls

# Test state locally
sudo salt-call state.apply webserver test=True
```

### Performance Tuning

```yaml
# /etc/salt/master
# Increase performance

# Worker threads
worker_threads: 4

# Job cache
job_cache: True
cache: localfs

# Timeout
timeout: 30

# Master minion port
# /etc/salt/minion
# Increase performance

# Cachedir
cachedir: /var/cache/salt

# Module cache
module_refresh_interval: 60

# Max open files (in system)
# /etc/security/limits.conf
* soft nofile 65535
* hard nofile 65535
```

---

## Quick Reference Commands

```bash
# Remote Execution
sudo salt '*' cmd.run 'uptime'                    # Execute command
sudo salt 'web*' pkg.install 'nginx'              # Install package
sudo salt '*' service.status 'nginx'              # Service status
sudo salt -G 'os:Ubuntu' cmd.run 'uname -a'       # Execute by grain

# States
sudo salt '*' state.apply                         # Apply all states
sudo salt '*' state.apply webserver               # Apply specific state
sudo salt '*' state.apply test=True               # Dry-run
sudo salt '*' state.show_top                      # Show top file

# Grains & Pillars
sudo salt '*' grains.items                        # List grains
sudo salt '*' grains.get 'os'                     # Get grain
sudo salt '*' pillar.items                        # List pillars
sudo salt '*' pillar.get 'webserver:port'         # Get pillar value

# Job Management
sudo salt-run jobs.active                         # Active jobs
sudo salt-run jobs.list_jobs                      # All jobs
sudo salt-run jobs.lookup_jid <id>                # Job details

# Key Management
sudo salt-key -l                                  # List keys
sudo salt-key -a minion                           # Accept key
sudo salt-key -r minion                           # Reject key
sudo salt-key -d minion                           # Delete key

# Debugging
sudo salt '*' test.ping                           # Test connectivity
sudo salt-run manage.up                           # Check active minions
sudo salt '*' grains.ls                           # List grain names
salt-call state.apply --local                     # Local execution
```

---

## Essential SaltStack Concepts

### Master
Central server that manages minions and delivers configurations

### Minion
Client node that receives commands and states from master

### States
Declarative configuration files (YAML) defining desired state

### Grains
Static system information collected from minions

### Pillars
Dynamic data/configuration stored on master for minions

### Formulas
Reusable Salt states and templates for common tasks

### Renderers
Template engines (Jinja2, Mako) for generating configurations

### Modules
Python modules extending Salt functionality

---

## Resources

| Resource | URL |
|----------|-----|
| **Official Documentation** | https://docs.saltproject.io/ |
| **Installation Guide** | https://docs.saltproject.io/en/latest/topics/installation/ |
| **State Module Docs** | https://docs.saltproject.io/en/latest/ref/states/all/ |
| **Execution Modules** | https://docs.saltproject.io/en/latest/ref/modules/all/ |
| **Best Practices** | https://docs.saltproject.io/en/latest/topics/best_practices.html |
| **Community** | https://saltproject.io/community/ |

---

**Last Updated:** 2026
**SaltStack Version:** 3.x+
**Python Version:** 3.6+

For latest information, visit [SaltStack Official Documentation](https://docs.saltproject.io/)