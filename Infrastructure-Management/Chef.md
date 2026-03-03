# Chef Cheatsheet

![Chef Logo](https://www.chef.io/hubfs/Chef-favicon.png)

---

## Table of Contents
1. [What is Chef?](#1-what-is-chef)
2. [Installation & Setup](#2-installation--setup)
3. [Chef Architecture](#3-chef-architecture)
4. [Recipes](#4-recipes)
5. [Resources](#5-resources)
6. [Cookbooks](#6-cookbooks)
7. [Attributes](#7-attributes)
8. [Templates](#8-templates)
9. [Roles & Policies](#9-roles--policies)
10. [Node Management](#10-node-management)
11. [Best Practices](#11-best-practices)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. What is Chef?

Chef is an infrastructure automation platform that uses code to manage infrastructure configuration. It enables you to define infrastructure as code and automate deployment, configuration, and management of servers.

**Key Features:**
- Infrastructure as Code (IaC)
- Configuration management
- Compliance automation
- Agentless or agent-based execution
- Powerful DSL (Ruby-based)
- Centralized server (Chef Server)
- Recipe-based automation
- Idempotent operations
- Cross-platform support

---

## 2. Installation & Setup

### Install Chef Workstation

```bash
# macOS with Homebrew
brew install chef-workstation

# Ubuntu/Debian
curl https://packages.chef.io/files/stable/chef-workstation/22.12.1008/ubuntu/20.04/chef-workstation_22.12.1008-1_amd64.deb -o chef-workstation.deb
sudo dpkg -i chef-workstation.deb

# Windows (via Chocolatey)
choco install chef-workstation

# CentOS/RHEL
curl https://packages.chef.io/files/stable/chef-workstation/22.12.1008/el/8/chef-workstation-22.12.1008-1.el8.x86_64.rpm -o chef-workstation.rpm
sudo rpm -ivh chef-workstation.rpm

# Verify installation
chef --version
kitchen --version
cookstyle --version
```

### Generate SSH Keys

```bash
# Generate RSA key for Chef
ssh-keygen -t rsa -f ~/.ssh/id_rsa -N ""

# Copy to remote hosts
ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote_host

# Test connectivity
ssh user@remote_host "echo 'Connected'"
```

### Chef Configuration

```bash
# Create .chef directory
mkdir -p ~/.chef

# Add to .gitignore
echo "~/.chef/*.pem" >> .gitignore
echo "Berksfile.lock" >> .gitignore
echo "cookbooks/" >> .gitignore
```

### Directory Structure

```
chef-project/
├── cookbooks/                  # Cookbooks directory
│   ├── webserver/
│   ├── database/
│   └── monitoring/
├── roles/                      # Role definitions
│   ├── webserver.json
│   ├── database.json
│   └── base.json
├── data_bags/                  # Data bag definitions
│   └── users/
├── environments/               # Environment definitions
│   ├── dev.json
│   ├── test.json
│   └── prod.json
├── .chef-repo.txt
└── .gitignore
```

---

## 3. Chef Architecture

### Chef Components

```
┌─────────────────────────────────────┐
│      Chef Workstation               │
│  (Development Machine)              │
│  - Write cookbooks                  │
│  - Test recipes                     │
│  - Generate commands                │
└─────────────────┬───────────────────┘
                  │
                  │ (knife command)
                  ▼
┌─────────────────────────────────────┐
│      Chef Server                    │
│  - Central repository               │
│  - Stores cookbooks                 │
│  - Manages nodes                    │
│  - Stores data bags                 │
└─────────────────┬───────────────────┘
                  │
        ┌─────────┼─────────┐
        │         │         │
        ▼         ▼         ▼
    ┌─────┐  ┌─────┐  ┌─────┐
    │Node │  │Node │  │Node │
    │(Chef│  │(Chef│  │(Chef│
    │Client)  Client)  Client)
    └─────┘  └─────┘  └─────┘
```

### Execution Flow

```
1. Write Recipe/Cookbook on Workstation
2. Upload to Chef Server (knife upload)
3. Bootstrap Node (knife bootstrap)
4. Chef Client runs on Node
5. Converge Node to Desired State
6. Report back to Chef Server
```

---

## 4. Recipes

### Basic Recipe Structure

```ruby
# recipes/default.rb
#
# Cookbook:: webserver
# Recipe:: default
#
# All rights reserved - 2024

# Update system packages
execute 'update_packages' do
  command 'apt-get update'
  action :run
end

# Install packages
package 'nginx' do
  action :install
end

# Create directory
directory '/var/www/html' do
  owner 'www-data'
  group 'www-data'
  mode '0755'
  recursive true
  action :create
end

# Deploy configuration file
template '/etc/nginx/nginx.conf' do
  source 'nginx.conf.erb'
  owner 'root'
  group 'root'
  mode '0644'
  notifies :restart, 'service[nginx]'
end

# Manage service
service 'nginx' do
  supports status: true, restart: true, reload: true
  action [:enable, :start]
end

# Copy file
cookbook_file '/etc/nginx/index.html' do
  source 'index.html'
  owner 'www-data'
  group 'www-data'
  mode '0644'
  action :create
end

# Write file
file '/tmp/application.conf' do
  content lazy {
    "APP_PORT=#{node['app']['port']}\n"
  }
  owner 'root'
  group 'root'
  mode '0644'
end

# Execute Ruby code
ruby_block 'configure_app' do
  block do
    puts "Application port: #{node['app']['port']}"
  end
  action :run
end
```

### Recipe with Conditions

```ruby
# recipes/conditionals.rb

# Only run on Ubuntu
if node['platform'] == 'ubuntu'
  package 'build-essential' do
    action :install
  end
end

# Check if file exists
unless ::File.exist?('/etc/app/config.yml')
  template '/etc/app/config.yml' do
    source 'config.yml.erb'
    action :create
  end
end

# Conditional based on attribute
if node['environment'] == 'production'
  node.override['app']['port'] = 443
  node.override['app']['ssl'] = true
else
  node.override['app']['port'] = 8080
  node.override['app']['ssl'] = false
end

# Execute only if condition met
execute 'enable_firewall' do
  command 'ufw enable'
  only_if { node['enable_firewall'] }
end

# Not if condition
execute 'disable_selinux' do
  command 'setenforce 0'
  not_if { node['selinux_enabled'] == false }
end
```

### Recipe Includes

```ruby
# recipes/default.rb

# Include other recipes
include_recipe 'webserver::default'
include_recipe 'webserver::ssl'
include_recipe 'monitoring::agent'

# Conditionally include
include_recipe 'webserver::ssl' if node['environment'] == 'production'
```

---

## 5. Resources

### Common Resources

```ruby
# File Management
file '/tmp/myfile.txt' do
  content 'Hello World'
  owner 'root'
  group 'root'
  mode '0644'
  action :create
end

directory '/var/www/html' do
  owner 'www-data'
  group 'www-data'
  mode '0755'
  recursive true
end

cookbook_file '/etc/config.conf' do
  source 'config.conf'
  owner 'root'
  mode '0644'
  action :create
end

# Templates
template '/etc/nginx/nginx.conf' do
  source 'nginx.conf.erb'
  owner 'root'
  group 'root'
  mode '0644'
  variables(
    port: node['nginx']['port'],
    workers: node['nginx']['workers']
  )
  notifies :restart, 'service[nginx]'
end

# Package Management
package 'nginx' do
  action :install
end

package %w[curl git vim] do
  action :install
end

package 'postgresql-client' do
  version '12.4-1.pgdg100+1'
  action :install
end

# Service Management
service 'nginx' do
  action [:enable, :start]
  supports status: true, restart: true, reload: true
end

service 'postgresql' do
  action :restart
  only_if { node['postgresql']['restart'] }
end

# Execute Commands
execute 'compile_source' do
  command './configure && make && make install'
  cwd '/opt/myapp'
  user 'nobody'
  group 'nogroup'
  action :run
end

bash 'install_rvm' do
  code <<-EOH
    curl -sSL https://get.rvm.io | bash
    source /etc/profile.d/rvm.sh
    rvm install ruby
  EOH
  not_if 'which rvm'
end

# User and Group Management
user 'appuser' do
  uid 1001
  gid 'appgroup'
  home '/home/appuser'
  shell '/bin/bash'
  password '$1$kWrNVAx1$cxO...'
  action :create
end

group 'appgroup' do
  gid 1001
  members ['appuser']
  action :create
end

# Notifications
service 'nginx' do
  action [:enable, :start]
  notifies :restart, 'service[php-fpm]'
  subscribes :restart, 'template[/etc/nginx/nginx.conf]'
end

# Remote File
remote_file '/opt/app.jar' do
  source 'https://releases.example.com/app-1.0.0.jar'
  owner 'appuser'
  group 'appgroup'
  mode '0755'
  action :create
end

# Git Clone
git '/opt/myapp' do
  repository 'https://github.com/user/myapp.git'
  revision 'main'
  action :sync
  notifies :run, 'execute[build_app]'
end

# Ruby Block
ruby_block 'configure_application' do
  block do
    require 'yaml'
    config = YAML.load_file('/etc/app/config.yml')
    config['port'] = node['app']['port']
    File.write('/etc/app/config.yml', YAML.dump(config))
  end
  action :run
end

# Link/Symlink
link '/etc/app.conf' do
  to '/opt/app/app.conf'
  action :create
end

# Cron Job
cron 'backup_database' do
  minute '0'
  hour '2'
  day '*'
  month '*'
  weekday '*'
  command '/usr/local/bin/backup.sh'
  action :create
end
```

---

## 6. Cookbooks

### Create Cookbook

```bash
# Generate cookbook
chef generate cookbook webserver

# Generated structure
webserver/
├── recipes/
│   └── default.rb
├── attributes/
│   └── default.rb
├── templates/
├── files/
├── spec/
├── test/
├── README.md
├── metadata.rb
├── Berksfile
└── .gitignore
```

### Cookbook Metadata

```ruby
# metadata.rb
name 'webserver'
maintainer 'Your Company'
maintainer_email 'you@example.com'
license 'Apache-2.0'
description 'Installs and configures web server'
version '1.0.0'
chef_version '>= 16.0'

supports 'ubuntu', '>= 18.04'
supports 'centos', '>= 7.0'

issues_url 'https://github.com/example/webserver/issues'
source_url 'https://github.com/example/webserver'

depends 'nginx', '~> 11.0'
depends 'ssl', '~> 1.0'
depends 'monitoring', '~> 2.0'
```

### Cookbook Dependencies (Berksfile)

```ruby
# Berksfile
source 'https://supermarket.chef.io'

cookbook 'nginx', '~> 11.0'
cookbook 'postgresql', '~> 8.0'
cookbook 'nodejs', '~> 6.0'

# Local cookbook
cookbook 'webserver', path: './'

# Git source
cookbook 'custom', git: 'https://github.com/user/custom.git'
```

### Upload Cookbook

```bash
# Resolve dependencies
berks install

# Upload to Chef Server
berks upload

# Upload specific cookbook
berks upload webserver

# Force upload
berks upload --force
```

---

## 7. Attributes

### Define Attributes

```ruby
# attributes/default.rb
default['webserver']['port'] = 80
default['webserver']['workers'] = 4
default['webserver']['enable_ssl'] = false
default['webserver']['user'] = 'www-data'
default['webserver']['group'] = 'www-data'

# Platform-specific attributes
case node['platform']
when 'ubuntu'
  default['webserver']['package_name'] = 'nginx'
  default['webserver']['config_file'] = '/etc/nginx/nginx.conf'
when 'centos'
  default['webserver']['package_name'] = 'httpd'
  default['webserver']['config_file'] = '/etc/httpd/conf/httpd.conf'
end

# Nested attributes
default['app']['database']['host'] = 'localhost'
default['app']['database']['port'] = 5432
default['app']['database']['name'] = 'myapp'
```

### Attribute Precedence

```ruby
# attributes/default.rb
default['app']['port'] = 8080

# attributes/production.rb (included based on environment)
override['app']['port'] = 443

# Node-level override
node.override['app']['port'] = 9000
```

### Use Attributes

```ruby
# recipes/default.rb
package node['webserver']['package_name'] do
  action :install
end

service node['webserver']['package_name'] do
  action [:enable, :start]
end

template '/etc/nginx/nginx.conf' do
  source 'nginx.conf.erb'
  variables(
    port: node['webserver']['port'],
    workers: node['webserver']['workers'],
    user: node['webserver']['user']
  )
end
```

### Platform-Specific Recipes

```ruby
# recipes/install.rb
include_recipe "#{cookbook_name}::_install_#{node['platform_family']}"

# recipes/_install_debian.rb
package 'nginx' do
  action :install
end

# recipes/_install_rhel.rb
package 'httpd' do
  action :install
end
```

---

## 8. Templates

### Create Template

```erb
<!-- templates/nginx.conf.erb -->
user <%= @user %>;
worker_processes <%= @workers %>;
error_log /var/log/nginx/error.log;
pid /var/run/nginx.pid;

events {
    worker_connections 1024;
}

http {
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;

    default_type application/octet-stream;

    access_log /var/log/nginx/access.log;

    gzip on;

    server {
        listen <%= @port %>;
        server_name _;

        location / {
            root /var/www/html;
            index index.html;
        }
    }
}
```

### Deploy Template

```ruby
# recipes/default.rb
template '/etc/nginx/nginx.conf' do
  source 'nginx.conf.erb'
  owner 'root'
  group 'root'
  mode '0644'
  variables(
    port: node['nginx']['port'],
    workers: node['nginx']['workers'],
    user: node['nginx']['user']
  )
  notifies :restart, 'service[nginx]'
end
```

### Template with Conditionals

```erb
<!-- templates/app.conf.erb -->
server {
    listen <%= @port %>;
    server_name <%= @server_name %>;

    <% if @enable_ssl %>
    ssl on;
    ssl_certificate /etc/ssl/certs/server.crt;
    ssl_certificate_key /etc/ssl/private/server.key;
    <% end %>

    <% @locations.each do |loc| %>
    location <%= loc['path'] %> {
        proxy_pass <%= loc['proxy'] %>;
    }
    <% end %>
}
```

---

## 9. Roles & Policies

### Create Role

```json
// roles/webserver.json
{
  "name": "webserver",
  "description": "Web server role",
  "json_class": "Chef::Role",
  "chef_type": "role",
  "run_list": [
    "recipe[base::default]",
    "recipe[webserver::default]",
    "recipe[webserver::ssl]",
    "recipe[monitoring::agent]"
  ],
  "default_attributes": {
    "nginx": {
      "port": 80,
      "workers": 4
    }
  },
  "override_attributes": {
    "nginx": {
      "user": "www-data"
    }
  }
}
```

### Create Policy

```ruby
# policies/webserver_policy.rb
name 'webserver_policy'
description 'Production web server policy'

run_list(
  'recipe[base::default]',
  'recipe[webserver::default]'
)

default_attributes(
  nginx: {
    port: 443,
    ssl: true
  }
)
```

### Assign Role to Node

```bash
# Using knife
knife node run_list set node_name "role[webserver]"

# Using JSON
knife node run_list set node_name "role[base],recipe[webserver::default]"
```

---

## 10. Node Management

### Bootstrap Node

```bash
# Bootstrap Linux node
knife bootstrap 192.168.1.100 \
  --ssh-user ubuntu \
  --sudo \
  --identity-file ~/.ssh/id_rsa \
  --node-name web1 \
  --run-list "role[base],role[webserver]"

# Bootstrap with attributes
knife bootstrap 192.168.1.100 \
  --ssh-user ubuntu \
  --sudo \
  --node-name web1 \
  --run-list "role[webserver]" \
  -j '{"nginx": {"port": 8080}}'

# Windows bootstrap
knife bootstrap windows winrm 192.168.1.101 \
  --winrm-user Administrator \
  --winrm-password 'Password123!' \
  --node-name win1 \
  --run-list "recipe[webserver::default]"
```

### Manage Nodes

```bash
# List nodes
knife node list

# Show node details
knife node show web1

# Show node attributes
knife node show web1 -a "run_list"
knife node show web1 -a "automatic.platform"

# Edit node
knife node edit web1

# Delete node
knife node delete web1

# Set node run_list
knife node run_list set web1 "role[webserver],recipe[monitoring::default]"

# Fetch node info
knife node show web1 -Fj > web1.json

# Create node from JSON
knife node from file web1.json
```

### Run Chef on Nodes

```bash
# Manually trigger chef-client
ssh ubuntu@web1 'sudo chef-client'

# Run specific recipe
ssh ubuntu@web1 'sudo chef-client -r recipe[webserver::default]'

# Dry-run (why-run mode)
ssh ubuntu@web1 'sudo chef-client --why-run'

# Run with log level
ssh ubuntu@web1 'sudo chef-client -l debug'

# SSH and run command
knife ssh "role:webserver" "sudo chef-client" -x ubuntu
```

---

## 11. Best Practices

### Cookbook Organization

```ruby
# recipes/default.rb - Include other recipes
include_recipe 'webserver::install'
include_recipe 'webserver::configure'
include_recipe 'webserver::start'

# recipes/install.rb - Install packages
package 'nginx' do
  action :install
end

# recipes/configure.rb - Configure service
template '/etc/nginx/nginx.conf' do
  source 'nginx.conf.erb'
  notifies :restart, 'service[nginx]'
end

# recipes/start.rb - Start service
service 'nginx' do
  action [:enable, :start]
end
```

### Idempotent Recipes

```ruby
# Good - idempotent
execute 'compile_from_source' do
  command 'cd /opt/source && ./configure && make && make install'
  not_if { ::File.exist?('/usr/local/bin/app') }
end

# Good - idempotent
file '/etc/config.conf' do
  content 'CONFIG=value'
  owner 'root'
  mode '0644'
  action :create
end

# Bad - not idempotent
execute 'install_something' do
  command 'apt-get install package && echo "done" > /tmp/installed'
end
```

### Guard Clauses

```ruby
# Skip if condition
execute 'enable_firewall' do
  command 'ufw enable'
  only_if 'test -f /etc/default/ufw'
end

# Skip unless condition
execute 'configure_app' do
  command '/opt/app/configure.sh'
  not_if { ::File.exist?('/etc/app/configured') }
end

# Check with guard clause
execute 'setup_database' do
  command 'mysql -u root < /tmp/setup.sql'
  only_if { system('mysql -u root -e "SHOW DATABASES"') }
end
```

### Error Handling

```ruby
# Ignore errors
execute 'optional_command' do
  command 'might_fail.sh'
  ignore_errors true
end

# Handle specific exit codes
bash 'check_status' do
  code 'systemctl status nginx'
  returns [0, 3]  # Accept exit codes 0 and 3
end

# Retry on failure
execute 'install_package' do
  command 'apt-get install nginx'
  retries 3
  retry_delay 2
end
```

### Documentation

```ruby
# recipes/default.rb
#
# Cookbook:: webserver
# Recipe:: default
#
# Description: Installs and configures Nginx web server
#
# Requirements:
#   - Ubuntu/Debian
#   - 2GB RAM minimum
#
# Usage:
#   - Include in run_list
#   - Set node['nginx']['port'] attribute

package 'nginx' do
  action :install
  # Install from default repository
end
```

---

## 12. Troubleshooting

### Debug Mode

```bash
# Run with debug logging
sudo chef-client -l debug

# Log level options
sudo chef-client -l fatal
sudo chef-client -l error
sudo chef-client -l warn
sudo chef-client -l info
sudo chef-client -l debug
sudo chef-client -l trace

# Why-run mode (preview changes)
sudo chef-client --why-run

# No fork (useful for debugging)
sudo chef-client --no-fork

# Verbose
sudo chef-client -v
sudo chef-client -vv
sudo chef-client -vvv
```

### Common Issues

```ruby
# Issue: Template not found
# Wrong:
template '/etc/app.conf' do
  source 'app.conf'  # Missing .erb extension
end

# Right:
template '/etc/app.conf' do
  source 'app.conf.erb'
end

# Issue: Resource not notified
# Use correct syntax:
notify :restart, 'service[nginx]'  # Correct
notify :restart, service[nginx]    # Wrong

# Issue: Node attribute undefined
# Use safe navigation:
node['app']['port'] rescue 8080

# Issue: Circular dependency
# Avoid having resources notify each other in a circle

# Issue: Converge fails silently
# Use why-run mode to preview
sudo chef-client --why-run
```

### Troubleshooting Commands

```bash
# Check syntax
cookstyle .
knife cookbook test webserver

# Validate cookbook
knife cookbook metadata webserver

# Test recipe locally (requires Test Kitchen)
kitchen test

# Run specific recipe
sudo chef-client -r "recipe[webserver::install]"

# Get node info
knife node show web1 -a "automatic"

# Check Chef Server connection
knife client list

# View node logs
sudo tail -f /var/log/chef/client.log

# Force converge
sudo chef-client -f

# Reset node
knife node delete web1
knife bootstrap 192.168.1.100 --node-name web1
```

### Test Kitchen Setup

```bash
# Install Test Kitchen
chef gem install test-kitchen

# Kitchen init
kitchen init

# Run tests
kitchen list
kitchen create
kitchen converge
kitchen verify
kitchen destroy
kitchen test

# Kitchen config (.kitchen.yml)
---
driver:
  name: vagrant

provisioner:
  name: chef_zero

platforms:
  - name: ubuntu-20.04

suites:
  - name: default
    run_list:
      - recipe[webserver::default]
    verifier:
      inspec_tests:
        - test/integration/default
```

---

## Quick Reference Commands

```bash
# Cookbook Management
chef generate cookbook webserver                    # Create cookbook
berks install                                       # Install dependencies
berks upload                                        # Upload to Chef Server
berks upload --force                                # Force upload

# Node Management
knife node list                                     # List nodes
knife node show web1                                # Node details
knife bootstrap 192.168.1.100 -x ubuntu -N web1    # Bootstrap node
knife ssh "role:webserver" "sudo chef-client"      # Run on multiple nodes

# Knife Commands
knife cookbook list                                 # List cookbooks
knife cookbook upload webserver                     # Upload cookbook
knife role list                                     # List roles
knife role create webserver                         # Create role
knife data bag list                                 # List data bags
knife client list                                   # List clients

# Debugging
sudo chef-client -l debug                           # Run with debug
sudo chef-client --why-run                          # Preview changes
cookstyle .                                         # Check syntax
knife cookbook test webserver                       # Validate cookbook

# Testing
kitchen test                                        # Run all tests
kitchen converge                                    # Apply recipes
kitchen verify                                      # Run tests
kitchen destroy                                     # Cleanup
```

---

## Essential Chef Concepts

### Convergence
Chef runs recipes and converges node state to desired state through idempotent resources.

### Idempotence
Recipes should be safe to run multiple times without changing already correct state.

### Notifications
Resources can notify other resources to take action (restart service, reload config).

### Guards
Control resource execution with `only_if` and `not_if` conditions.

### Attributes
Node configuration managed with default, normal, and override precedence levels.

### Roles
Assign sets of recipes and attributes to nodes for consistent configuration.

---

## Resources

| Resource | URL |
|----------|-----|
| **Official Documentation** | https://docs.chef.io/ |
| **Chef Infra Client** | https://docs.chef.io/chef_infra_client/ |
| **Resource Reference** | https://docs.chef.io/resource/ |
| **Best Practices** | https://docs.chef.io/ruby/ |
| **Supermarket** | https://supermarket.chef.io/ |
| **Community** | https://community.chef.io/ |

---

**Last Updated:** 2024
**Chef Version:** 17.x+
**Ruby Version:** 2.7+

For latest information, visit [Chef Official Documentation](https://docs.chef.io/)