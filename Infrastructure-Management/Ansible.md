# Ansible Cheatsheet

![Ansible Logo](https://www.ansible.com/hubfs/2016_images/assets/social-share-ansible.png)

---

## Table of Contents
1. [What is Ansible?](#1-what-is-ansible)
2. [Installation & Setup](#2-installation--setup)
3. [Inventory Management](#3-inventory-management)
4. [Playbooks](#4-playbooks)
5. [Modules](#5-modules)
6. [Variables & Facts](#6-variables--facts)
7. [Handlers & Notifications](#7-handlers--notifications)
8. [Roles](#8-roles)
9. [Ad-hoc Commands](#9-ad-hoc-commands)
10. [Advanced Features](#10-advanced-features)
11. [Best Practices](#11-best-practices)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. What is Ansible?

Ansible is an agentless infrastructure automation and configuration management tool. It uses SSH to execute commands on remote servers without requiring agents to be installed.

**Key Features:**
- Agentless architecture (SSH-based)
- Simple YAML syntax for playbooks
- Configuration management
- Application deployment
- Orchestration
- Multi-node orchestration
- Idempotent operations
- Extensive module library
- Role-based organization

---

## 2. Installation & Setup

### Install Ansible

```bash
# macOS with Homebrew
brew install ansible

# Ubuntu/Debian
sudo apt-get update
sudo apt-get install ansible

# CentOS/RHEL
sudo yum install ansible

# Using pip (Python package manager)
pip install ansible
pip install ansible==2.10.0  # Specific version

# Verify installation
ansible --version
ansible-inventory --version
ansible-playbook --version
```

### SSH Key Setup

```bash
# Generate SSH key pair
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa

# Copy public key to remote hosts
ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote_host

# Test SSH connection
ssh -i ~/.ssh/id_rsa user@remote_host

# Add key to SSH agent
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_rsa

# Configure SSH (optional)
cat > ~/.ssh/config << 'EOF'
Host web*
    User ubuntu
    IdentityFile ~/.ssh/id_rsa
    StrictHostKeyChecking no
EOF
```

### Directory Structure

```
ansible-project/
├── ansible.cfg                 # Ansible configuration
├── inventory/
│   ├── hosts.ini              # Inventory file
│   └── aws_ec2.yml            # Dynamic inventory
├── playbooks/
│   ├── site.yml               # Main playbook
│   ├── deploy.yml             # Deployment playbook
│   └── configure.yml          # Configuration playbook
├── roles/
│   ├── common/                # Role for common tasks
│   ├── webserver/             # Webserver role
│   └── database/              # Database role
├── group_vars/
│   ├── all.yml                # Variables for all hosts
│   ├── webservers.yml         # Variables for webservers
│   └── databases.yml          # Variables for databases
├── host_vars/
│   ├── web1.yml               # Variables for specific host
│   └── db1.yml                # Variables for specific host
├── templates/                 # Jinja2 templates
├── files/                     # Static files
└── library/                   # Custom modules
```

### ansible.cfg Configuration

```ini
[defaults]
# Inventory file location
inventory = ./inventory/hosts.ini

# Remote user
remote_user = ubuntu

# Private key file
private_key_file = ~/.ssh/id_rsa

# Disable host key checking
host_key_checking = False

# Parallel execution
forks = 10

# Plugin paths
library = ./library
roles_path = ./roles

# Become settings
become = True
become_method = sudo
become_user = root

# Logging
log_path = ./ansible.log

# Retry settings
retry_files_enabled = True
retry_files_save_path = ./retry_files/

# Callback plugin
stdout_callback = yaml
```

---

## 3. Inventory Management

### Static Inventory (hosts.ini)

```ini
# Simple inventory
[webservers]
web1.example.com
web2.example.com
web3.example.com

[databases]
db1.example.com
db2.example.com

[all:vars]
ansible_user=ubuntu
ansible_private_key_file=~/.ssh/id_rsa

# With IP addresses
[webservers]
web1 ansible_host=192.168.1.10
web2 ansible_host=192.168.1.11

# With ports
[webservers]
web1 ansible_host=192.168.1.10 ansible_port=2222
web2 ansible_host=192.168.1.11

# With variables
[webservers]
web1 ansible_host=192.168.1.10 app_port=8080
web2 ansible_host=192.168.1.11 app_port=8080

# Group hierarchy
[webservers:children]
frontend
backend

[frontend]
web1
web2

[backend]
web3
web4
```

### YAML Inventory (hosts.yml)

```yaml
---
all:
  hosts:
    localhost:
      ansible_connection: local
  
  children:
    webservers:
      hosts:
        web1:
          ansible_host: 192.168.1.10
          app_port: 8080
        web2:
          ansible_host: 192.168.1.11
          app_port: 8080
      vars:
        ansible_user: ubuntu
        environment: production
    
    databases:
      hosts:
        db1:
          ansible_host: 192.168.1.20
        db2:
          ansible_host: 192.168.1.21
      vars:
        ansible_user: ubuntu
        db_port: 5432
```

### Validate Inventory

```bash
# List all hosts
ansible-inventory -i inventory/hosts.ini --list

# List specific group
ansible-inventory -i inventory/hosts.ini --list --host webservers

# List in JSON format
ansible-inventory -i inventory/hosts.ini --list --format json

# Check connectivity
ansible all -i inventory/hosts.ini -m ping

# Get host info
ansible webservers -i inventory/hosts.ini -m setup
```

---

## 4. Playbooks

### Basic Playbook Structure

```yaml
---
- name: Configure web servers
  hosts: webservers
  become: yes
  gather_facts: yes
  vars:
    app_port: 8080
    environment: production
  
  pre_tasks:
    - name: Update package manager
      apt:
        update_cache: yes
  
  tasks:
    - name: Install packages
      apt:
        name: "{{ item }}"
        state: present
      loop:
        - nginx
        - curl
        - git
    
    - name: Start nginx service
      service:
        name: nginx
        state: started
        enabled: yes
  
  post_tasks:
    - name: Verify service
      uri:
        url: "http://localhost:{{ app_port }}"
        status_code: 200
  
  handlers:
    - name: Restart nginx
      service:
        name: nginx
        state: restarted
```

### Multiple Plays

```yaml
---
- name: Configure databases
  hosts: databases
  become: yes
  tasks:
    - name: Install PostgreSQL
      apt:
        name: postgresql
        state: present
    
    - name: Start PostgreSQL
      service:
        name: postgresql
        state: started

- name: Configure web servers
  hosts: webservers
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
    
    - name: Start Nginx
      service:
        name: nginx
        state: started

- name: Setup monitoring
  hosts: all
  tasks:
    - name: Install monitoring agent
      apt:
        name: collectd
        state: present
```

### Playbook with Conditions

```yaml
---
- name: Deploy application
  hosts: webservers
  vars:
    deploy_version: "2.0.0"
  
  tasks:
    - name: Install Java (Ubuntu)
      apt:
        name: openjdk-11-jdk
      when: ansible_os_family == "Debian"
    
    - name: Install Java (CentOS)
      yum:
        name: java-11-openjdk
      when: ansible_os_family == "RedHat"
    
    - name: Deploy app if version changed
      copy:
        src: app-{{ deploy_version }}.jar
        dest: /opt/app.jar
      notify: Restart application
      when: deploy_version != current_version | default("1.0.0")
```

### Run Playbooks

```bash
# Run playbook
ansible-playbook playbooks/site.yml

# Run with inventory
ansible-playbook -i inventory/hosts.ini playbooks/site.yml

# Run specific hosts
ansible-playbook playbooks/site.yml -l webservers

# Run with extra variables
ansible-playbook playbooks/site.yml -e "app_port=9000"

# Run in check mode (dry-run)
ansible-playbook playbooks/site.yml --check

# Run with verbose output
ansible-playbook playbooks/site.yml -v
ansible-playbook playbooks/site.yml -vv
ansible-playbook playbooks/site.yml -vvv

# Run specific tags
ansible-playbook playbooks/site.yml --tags "deploy"

# Skip specific tags
ansible-playbook playbooks/site.yml --skip-tags "testing"

# Run with syntax check
ansible-playbook playbooks/site.yml --syntax-check

# Step through playbook
ansible-playbook playbooks/site.yml --step
```

---

## 5. Modules

### Common Modules

```yaml
---
- name: Common modules examples
  hosts: all
  tasks:
    # Package management
    - name: Install package
      apt:
        name: nginx
        state: present
    
    - name: Remove package
      apt:
        name: apache2
        state: absent
    
    - name: Update packages
      apt:
        update_cache: yes
        upgrade: dist
    
    # Service management
    - name: Start service
      service:
        name: nginx
        state: started
        enabled: yes
    
    - name: Restart service
      systemd:
        name: nginx
        state: restarted
        daemon_reload: yes
    
    # File operations
    - name: Copy file
      copy:
        src: local/file.conf
        dest: /etc/nginx/file.conf
        owner: root
        group: root
        mode: '0644'
    
    - name: Create directory
      file:
        path: /var/www/html
        state: directory
        owner: www-data
        group: www-data
        mode: '0755'
    
    - name: Delete file
      file:
        path: /tmp/oldfile
        state: absent
    
    # Command execution
    - name: Run shell command
      shell: |
        #!/bin/bash
        echo "Running script"
        systemctl status nginx
    
    - name: Run command
      command: /usr/bin/whoami
    
    # Template rendering
    - name: Deploy config from template
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
        owner: root
        mode: '0644'
      notify: Restart nginx
    
    # User management
    - name: Create user
      user:
        name: appuser
        state: present
        shell: /bin/bash
        createhome: yes
    
    - name: Add user to group
      user:
        name: appuser
        groups: sudo
        append: yes
    
    # Git operations
    - name: Clone repository
      git:
        repo: https://github.com/user/repo.git
        dest: /opt/app
        version: main
    
    # HTTP requests
    - name: Check web service
      uri:
        url: http://localhost:80
        method: GET
        status_code: 200
    
    # Debug
    - name: Print message
      debug:
        msg: "Debug message: {{ variable }}"
```

### Module Documentation

```bash
# List available modules
ansible-doc -l

# Get module documentation
ansible-doc apt
ansible-doc service
ansible-doc file

# Get module examples
ansible-doc -s apt

# Search for modules
ansible-doc -l | grep nginx
```

---

## 6. Variables & Facts

### Define Variables

```yaml
---
- name: Variables example
  hosts: webservers
  
  # Play variables
  vars:
    app_name: myapp
    app_port: 8080
    app_user: appuser
    environment: production
    packages:
      - nginx
      - curl
      - git
    server_config:
      hostname: web1
      ip: 192.168.1.10
  
  # Variable files
  vars_files:
    - vars/common.yml
    - vars/{{ environment }}.yml
  
  tasks:
    - name: Use variables
      debug:
        msg: "App: {{ app_name }} on port {{ app_port }}"
    
    - name: Use list variables
      debug:
        msg: "Installing {{ item }}"
      loop: "{{ packages }}"
    
    - name: Use dict variables
      debug:
        msg: "Host {{ server_config.hostname }} has IP {{ server_config.ip }}"
    
    # Register variables
    - name: Get service status
      shell: systemctl is-active nginx
      register: nginx_status
      changed_when: false
    
    - name: Print service status
      debug:
        msg: "Nginx status: {{ nginx_status.stdout }}"
    
    # Set facts
    - name: Set custom fact
      set_fact:
        custom_variable: "custom_value"
    
    - name: Use custom fact
      debug:
        msg: "Custom fact: {{ custom_variable }}"
```

### Group and Host Variables

```yaml
# group_vars/webservers.yml
---
app_port: 8080
environment: production
packages:
  - nginx
  - curl

# group_vars/databases.yml
---
db_port: 5432
db_name: myapp_db

# host_vars/web1.yml
---
app_port: 8080
server_name: web1.example.com

# host_vars/db1.yml
---
db_port: 5432
db_replication: true
```

### Facts and Discovery

```yaml
---
- name: Working with facts
  hosts: all
  gather_facts: yes
  
  tasks:
    - name: Print all facts
      debug:
        var: ansible_facts
    
    - name: Print specific facts
      debug:
        msg: |
          OS: {{ ansible_distribution }}
          Kernel: {{ ansible_kernel }}
          IP: {{ ansible_default_ipv4.address }}
          Hostname: {{ ansible_hostname }}
          CPU cores: {{ ansible_processor_vcpus }}
```

---

## 7. Handlers & Notifications

### Handlers Example

```yaml
---
- name: Web server configuration
  hosts: webservers
  become: yes
  
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
    
    - name: Copy nginx config
      copy:
        src: nginx.conf
        dest: /etc/nginx/nginx.conf
        owner: root
        group: root
        mode: '0644'
      notify: Restart nginx
    
    - name: Copy site config
      template:
        src: site.conf.j2
        dest: "/etc/nginx/sites-available/{{ app_name }}.conf"
      notify: Reload nginx
    
    - name: Enable site
      file:
        src: "/etc/nginx/sites-available/{{ app_name }}.conf"
        dest: "/etc/nginx/sites-enabled/{{ app_name }}.conf"
        state: link
      notify: Restart nginx
  
  handlers:
    - name: Restart nginx
      service:
        name: nginx
        state: restarted
    
    - name: Reload nginx
      service:
        name: nginx
        state: reloaded
```

---

## 8. Roles

### Role Structure

```
roles/
└── webserver/
    ├── tasks/
    │   ├── main.yml          # Main task file
    │   └── install.yml       # Install tasks
    ├── handlers/
    │   └── main.yml          # Handlers
    ├── templates/
    │   ├── nginx.conf.j2
    │   └── app.conf.j2
    ├── files/
    │   ├── app.jar
    │   └── config.ini
    ├── vars/
    │   └── main.yml          # Role variables
    ├── defaults/
    │   └── main.yml          # Default variables
    ├── meta/
    │   └── main.yml          # Role dependencies
    └── README.md
```

### Role Implementation

```yaml
# roles/webserver/defaults/main.yml
---
nginx_port: 80
nginx_user: www-data
nginx_worker_processes: auto

# roles/webserver/vars/main.yml
---
nginx_package: nginx
nginx_service: nginx

# roles/webserver/tasks/main.yml
---
- name: Install Nginx
  apt:
    name: "{{ nginx_package }}"
    state: present

- name: Create Nginx config
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
  notify: Restart Nginx

- name: Start Nginx
  service:
    name: "{{ nginx_service }}"
    state: started
    enabled: yes

# roles/webserver/handlers/main.yml
---
- name: Restart Nginx
  service:
    name: "{{ nginx_service }}"
    state: restarted

# roles/webserver/meta/main.yml
---
dependencies:
  - role: common
  - role: firewall

# Playbook using role
---
- name: Deploy webservers
  hosts: webservers
  roles:
    - common
    - webserver
    - monitoring
  vars:
    nginx_port: 8080
```

---

## 9. Ad-hoc Commands

### Basic Ad-hoc Commands

```bash
# Ping hosts
ansible all -m ping
ansible webservers -m ping

# Run shell command
ansible webservers -m shell -a "systemctl status nginx"

# Run command
ansible webservers -m command -a "whoami"

# Execute script
ansible webservers -m script -a "scripts/deploy.sh"

# Copy file
ansible webservers -m copy -a "src=file.txt dest=/tmp/file.txt"

# Create user
ansible webservers -m user -a "name=newuser state=present"

# Install package
ansible webservers -m apt -a "name=curl state=present"

# Manage service
ansible webservers -m service -a "name=nginx state=started"

# Get facts
ansible webservers -m setup
ansible webservers -m setup -a "filter=ansible_os_family"

# Check connectivity
ansible all -m ping -i inventory/hosts.ini

# With become (sudo)
ansible webservers -m shell -a "systemctl restart nginx" -b

# With specific user
ansible webservers -m command -a "whoami" -u ubuntu

# With custom inventory
ansible-inventory all -i custom_inventory.yml -m ping
```

### Ad-hoc Loops and Conditions

```bash
# Loop through hosts
ansible webservers -m shell -a "echo {{ item }}" -e "item=hello"

# Get multiple facts
ansible all -m setup -a "filter=ansible_*_mb"

# Run with variables
ansible webservers -m debug -a "msg={{ custom_var }}" -e "custom_var=hello"

# Limit execution
ansible all -m ping --limit "web1,web2"

# Parallelize execution
ansible all -m ping -f 10  # 10 parallel forks
```

---

## 10. Advanced Features

### Loops and Iteration

```yaml
---
- name: Loop examples
  hosts: all
  tasks:
    # Simple loop
    - name: Install packages
      apt:
        name: "{{ item }}"
        state: present
      loop:
        - nginx
        - curl
        - git
    
    # Loop with list variable
    - name: Create users
      user:
        name: "{{ item }}"
        state: present
      loop: "{{ users }}"
    
    # Loop with dict
    - name: Create directories
      file:
        path: "{{ item.path }}"
        state: directory
        mode: "{{ item.mode }}"
      loop:
        - { path: "/var/www", mode: "0755" }
        - { path: "/var/log/app", mode: "0700" }
    
    # Loop with sequence
    - name: Create numbered files
      file:
        path: "/tmp/file{{ item }}.txt"
        state: touch
      loop: "{{ range(1, 5) | list }}"
```

### Conditionals

```yaml
---
- name: Conditional examples
  hosts: all
  tasks:
    - name: Install package on specific OS
      apt:
        name: nginx
      when: ansible_os_family == "Debian"
    
    - name: Install on multiple conditions
      apt:
        name: nginx
      when:
        - ansible_os_family == "Debian"
        - ansible_distribution_version | version_compare('18.04', '>=')
    
    - name: Use registered variable
      debug:
        msg: "Service is running"
      when: service_status.rc == 0
    
    - name: Check file exists
      debug:
        msg: "Config file exists"
      when: config_file.stat.exists
    
    # Block conditionals
    - block:
        - name: Task 1
          debug:
            msg: "Running task 1"
        - name: Task 2
          debug:
            msg: "Running task 2"
      when: ansible_os_family == "Debian"
```

### Error Handling

```yaml
---
- name: Error handling
  hosts: all
  tasks:
    - name: Run command, ignore errors
      shell: /usr/bin/false
      ignore_errors: yes
    
    - name: Fail with custom message
      fail:
        msg: "Deployment failed"
      when: deploy_status != "success"
    
    - name: Run command with error handler
      shell: systemctl start nginx
      register: result
      failed_when: "'error' in result.stderr"
    
    - name: Task with rescue
      block:
        - name: Main task
          shell: /usr/bin/command
      rescue:
        - name: Rescue task
          debug:
            msg: "Task failed, running rescue"
      always:
        - name: Always run
          debug:
            msg: "This always runs"
```

### Delegation and Async

```yaml
---
- name: Advanced execution
  hosts: all
  tasks:
    # Delegate task to specific host
    - name: Run on different host
      debug:
        msg: "Running on delegated host"
      delegate_to: "{{ groups['monitoring'][0] }}"
    
    # Run locally
    - name: Run locally
      debug:
        msg: "Running on control machine"
      delegate_to: localhost
    
    # Async execution
    - name: Long running task
      shell: /usr/bin/long_script.sh
      async: 300          # Timeout after 300 seconds
      poll: 0             # Don't wait for result
      register: long_task
    
    - name: Check async status
      async_status:
        jid: "{{ long_task.ansible_job_id }}"
      register: job_result
      until: job_result.finished
      retries: 30
```

---

## 11. Best Practices

### Playbook Organization

```yaml
---
# Good structure - site.yml
- name: Configure all infrastructure
  hosts: all
  pre_tasks:
    - name: Update systems
      apt:
        update_cache: yes
  roles:
    - common

- name: Configure webservers
  hosts: webservers
  roles:
    - webserver
    - monitoring
    - logging

- name: Configure databases
  hosts: databases
  roles:
    - database
    - backup

- name: Run tests
  hosts: all
  tasks:
    - name: Run health checks
      uri:
        url: "http://{{ ansible_host }}"
        status_code: 200
```

### Idempotency

```yaml
---
- name: Idempotent tasks
  hosts: all
  tasks:
    # Good - idempotent
    - name: Install package
      apt:
        name: nginx
        state: present
    
    # Bad - not idempotent
    - name: Install package (not idempotent)
      shell: apt-get install nginx
    
    # Good - use changed_when
    - name: Get status
      shell: systemctl is-active nginx
      register: status
      changed_when: false
    
    # Good - use when to control changes
    - name: Deploy only if version changed
      copy:
        src: app.jar
        dest: /opt/app.jar
      when: new_version != current_version
      notify: Restart application
```

### Secure Practices

```yaml
---
# Use vault for secrets
# ansible-vault create secrets.yml

- name: Secure playbook
  hosts: all
  vars_files:
    - secrets.yml  # Encrypted file
  
  tasks:
    - name: Use secret variable
      debug:
        msg: "Password: {{ db_password }}"
      no_log: yes  # Don't log passwords
    
    - name: Run with masked output
      shell: mysql -u root -p{{ db_password }} -e "SHOW DATABASES"
      no_log: yes
    
    # Use become for privilege escalation
    - name: Restart service
      service:
        name: nginx
        state: restarted
      become: yes
      become_method: sudo

# Run with vault
# ansible-playbook site.yml --vault-password-file=.vault_pass
```

### Documentation

```yaml
---
- name: Well-documented playbook
  hosts: webservers
  description: |
    Deploys web application to production servers
    Requirements:
      - Ansible 2.9+
      - SSH access to webservers
  
  tasks:
    - name: Install Nginx web server
      description: Install Nginx from default repository
      apt:
        name: nginx
        state: present
    
    - name: Deploy application code
      description: |
        Copy application files to web root.
        This task triggers the restart handler.
      copy:
        src: app/
        dest: /var/www/html/
      notify: Restart Nginx
```

---

## 12. Troubleshooting

### Debug and Logging

```bash
# Verbose output levels
ansible-playbook site.yml -v      # Level 1
ansible-playbook site.yml -vv     # Level 2
ansible-playbook site.yml -vvv    # Level 3
ansible-playbook site.yml -vvvv   # Level 4 (connection debugging)

# Check syntax
ansible-playbook site.yml --syntax-check

# Dry-run mode
ansible-playbook site.yml --check

# Step through execution
ansible-playbook site.yml --step

# Gather facts with verbose
ansible webservers -m setup -v

# Debug specific task
ansible-playbook site.yml -v -t "specific_tag"

# Check inventory
ansible-inventory -i inventory/hosts.ini --list
ansible-inventory -i inventory/hosts.ini --host web1
```

### Common Issues

```bash
# Connection refused
ansible webservers -m ping -vvv

# SSH key issues
ansible webservers -m ping -i inventory/hosts.ini -e "ansible_private_key_file=/path/to/key"

# Privilege escalation issues
ansible webservers -m command -a "whoami" -b -K  # -K prompts for sudo password

# Timeout issues
ansible-playbook site.yml -e "ansible_connection_timeout=60"

# Module not found
ansible-doc -l | grep module_name

# Verify playbook
ansible-playbook site.yml --syntax-check

# Check host reachability
ansible all -m ping -i inventory/hosts.ini
```

### Debug Tasks

```yaml
---
- name: Debugging playbook
  hosts: all
  tasks:
    - name: Print debug message
      debug:
        msg: "Debug info: {{ variable }}"
    
    - name: Print all variables
      debug:
        var: ansible_facts
    
    - name: Print registered variable
      debug:
        msg: "Result: {{ task_result }}"
      vars:
        task_result: "{{ result.stdout }}"
    
    - name: Assert condition
      assert:
        that:
          - result is succeeded
          - result.rc == 0
        fail_msg: "Task failed"
```

---

## Quick Reference Commands

```bash
# Playbook Execution
ansible-playbook site.yml                          # Run playbook
ansible-playbook site.yml -i inventory/hosts.ini   # Custom inventory
ansible-playbook site.yml -l webservers            # Limit hosts
ansible-playbook site.yml --check                  # Dry-run
ansible-playbook site.yml -v                       # Verbose output

# Ad-hoc Commands
ansible all -m ping                                # Ping all hosts
ansible webservers -m shell -a "ls -la"            # Run shell command
ansible webservers -m copy -a "src=file dest=/tmp" # Copy file
ansible webservers -m service -a "name=nginx state=started"  # Manage service

# Inventory
ansible-inventory -i hosts.ini --list              # List hosts
ansible-inventory -i hosts.ini --host web1         # Host details
ansible webservers -m setup -a "filter=ansible_os_family"  # Get facts

# Debugging
ansible-playbook site.yml --syntax-check           # Check syntax
ansible-playbook site.yml --step                   # Step through
ansible-playbook site.yml -vvv                     # Debug output

# Vault (Secrets)
ansible-vault create secrets.yml                   # Create vault file
ansible-vault edit secrets.yml                     # Edit vault file
ansible-playbook site.yml --vault-password-file=.vault_pass
```

---

## Essential Ansible Configuration

### ansible.cfg

```ini
[defaults]
inventory = ./inventory/hosts.ini
remote_user = ubuntu
private_key_file = ~/.ssh/id_rsa
host_key_checking = False
forks = 10
log_path = ./ansible.log
stdout_callback = yaml

[privilege_escalation]
become = True
become_method = sudo
become_user = root
become_ask_pass = False

[ssh_connection]
pipelining = True
ssh_args = -C -o ControlMaster=auto -o ControlPersist=60s
```

---

## Resources

| Resource | URL |
|----------|-----|
| **Official Documentation** | https://docs.ansible.com/ |
| **Playbook Guide** | https://docs.ansible.com/ansible/latest/user_guide/ |
| **Module Index** | https://docs.ansible.com/ansible/latest/modules/modules_by_category.html |
| **Best Practices** | https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html |
| **Galaxy (Roles)** | https://galaxy.ansible.com/ |
| **Community** | https://www.ansible.com/community |

---

**Last Updated:** 2026
**Ansible Version:** 2.9+
**Python Version:** 3.6+

For latest information, visit [Ansible Official Documentation](https://docs.ansible.com/)