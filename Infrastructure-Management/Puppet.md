# Puppet Cheatsheet

![Puppet Logo](https://puppet.com/favicon.ico)

---

## Table of Contents
1. [What is Puppet?](#1-what-is-puppet)
2. [Installation & Setup](#2-installation--setup)
3. [Puppet Architecture](#3-puppet-architecture)
4. [Manifests](#4-manifests)
5. [Resources](#5-resources)
6. [Classes](#6-classes)
7. [Modules](#7-modules)
8. [Variables & Facts](#8-variables--facts)
9. [Conditionals & Loops](#9-conditionals--loops)
10. [Node Management](#10-node-management)
11. [Best Practices](#11-best-practices)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. What is Puppet?

Puppet is an open-source infrastructure automation and configuration management tool. It uses a declarative language to define the desired state of systems and automatically applies those configurations across your infrastructure.

**Key Features:**
- Declarative configuration language
- Agent-based architecture (optional agentless)
- Master-agent model
- Idempotent operations
- Cross-platform support
- Module ecosystem
- Hiera for data management
- Resource abstraction
- Change tracking
- Compliance management

---

## 2. Installation & Setup

### Install Puppet Agent

```bash
# macOS with Homebrew
brew install puppet

# Ubuntu/Debian
wget https://apt.puppetlabs.com/puppet-release-focal.deb
sudo dpkg -i puppet-release-focal.deb
sudo apt-get update
sudo apt-get install -y puppet-agent

# CentOS/RHEL
sudo rpm -Uvh https://yum.puppetlabs.com/puppet-release-el-7.noarch.rpm
sudo yum install -y puppet-agent

# Windows (PowerShell)
choco install puppet-agent

# Verify installation
puppet --version
puppet agent --version
```

### Install Puppet Server

```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install -y puppetserver

# CentOS/RHEL
sudo yum install -y puppetserver

# macOS
brew install puppetserver

# Start server
sudo systemctl start puppetserver
sudo systemctl enable puppetserver

# Verify
sudo systemctl status puppetserver
```

### Initial Setup

```bash
# Generate certificates
sudo puppet cert generate $(hostname)

# List pending certificate requests
sudo puppet cert list

# Sign certificate request
sudo puppet cert sign agent-node.example.com

# View signed certificates
sudo puppet cert list --all
```

---

## 3. Puppet Architecture

### Architecture Components

```
┌─────────────────────────────────────┐
│      Puppet Server (Master)         │
│  - Central authority                │
│  - Stores manifests & modules       │
│  - Compiles catalogs                │
│  - Manages certificates             │
└──────────────┬──────────────────────┘
               │ (TCP 8140)
       ┌───────┼───────┐
       │       │       │
       ▼       ▼       ▼
   ┌────┐  ┌────┐  ┌────┐
   │Node│  │Node│  │Node│
   │(Agent) (Agent) (Agent)
   └────┘  └────┘  └────┘
   (Puppet Client)
```

### Puppet Agent Flow

```
1. Puppet Agent sends facts
2. Puppet Server compiles catalog
3. Server returns catalog
4. Agent applies configuration
5. Agent sends report to server
6. Server stores report
```

### Directory Structure

```
/etc/puppetlabs/code/
├── modules/                    # Modules directory
│   ├── webserver/
│   ├── database/
│   └── monitoring/
├── environments/               # Environment definitions
│   ├── production/
│   │   └── manifests/
│   └── development/
├── manifests/
│   └── site.pp                # Main manifest
└── hiera.yaml                 # Hiera configuration
```

---

## 4. Manifests

### Basic Manifest Structure

```puppet
# /etc/puppetlabs/code/manifests/site.pp

# Single resource
file { '/tmp/test.txt':
  ensure  => present,
  content => 'Hello World',
  owner   => 'root',
  group   => 'root',
  mode    => '0644',
}

# Multiple resources
package { 'nginx':
  ensure => present,
}

service { 'nginx':
  ensure    => running,
  enable    => true,
  require   => Package['nginx'],
}

# Node-specific configuration
node 'web1.example.com' {
  include webserver
  include monitoring
}

node 'db1.example.com' {
  include database
  include backup
}

# Default node
node default {
  include base
}
```

### Manifest with Classes

```puppet
# /etc/puppetlabs/code/manifests/site.pp

node 'web1.example.com' {
  class { 'webserver':
    port   => 8080,
    workers => 4,
  }
}

node 'db1.example.com' {
  class { 'database':
    engine => 'postgresql',
    version => '12',
  }
}

node default {
  include base::system
  include base::users
  include base::monitoring
}
```

---

## 5. Resources

### Common Resource Types

```puppet
# Package management
package { 'nginx':
  ensure => present,
}

package { ['curl', 'git', 'vim']:
  ensure => present,
}

package { 'postgresql-client':
  ensure => '12.4-1',
}

# File management
file { '/etc/nginx/nginx.conf':
  ensure  => present,
  content => 'nginx configuration',
  owner   => 'root',
  group   => 'root',
  mode    => '0644',
}

directory { '/var/www/html':
  ensure => directory,
  owner  => 'www-data',
  group  => 'www-data',
  mode   => '0755',
}

# Service management
service { 'nginx':
  ensure    => running,
  enable    => true,
  provider  => 'systemd',
  subscribe => File['/etc/nginx/nginx.conf'],
}

# Exec/Shell commands
exec { 'update_system':
  command => '/usr/bin/apt-get update',
  onlyif  => '/usr/bin/test -f /etc/debian_version',
}

exec { 'compile_source':
  command => '/bin/bash -c "cd /opt/src && ./configure && make && make install"',
  creates => '/usr/local/bin/myapp',
}

# User and group
user { 'appuser':
  ensure     => present,
  uid        => 1001,
  gid        => 1001,
  home       => '/home/appuser',
  shell      => '/bin/bash',
  managehome => true,
}

group { 'appgroup':
  ensure => present,
  gid    => 1001,
}

# Cron jobs
cron { 'backup_database':
  command => '/usr/local/bin/backup.sh',
  user    => 'root',
  hour    => 2,
  minute  => 0,
}

# Symlinks
file { '/etc/app.conf':
  ensure => link,
  target => '/opt/app/app.conf',
}

# Mount
mount { '/mnt/shared':
  ensure   => mounted,
  device   => '192.168.1.100:/export/shared',
  fstype   => 'nfs',
  options  => 'defaults',
  remounts => true,
}

# Host entry
host { 'web1.example.com':
  ensure       => present,
  ip           => '192.168.1.10',
  host_aliases => ['web1'],
}

# SSH key
ssh_authorized_key { 'deploy@example.com':
  ensure => present,
  user   => 'deploy',
  type   => 'ssh-rsa',
  key    => 'AAAAB3NzaC1yc2E...',
}
```

### Resource Attributes

```puppet
# Common attributes
file { '/tmp/myfile':
  ensure => present,      # present, absent, directory, link
  owner  => 'root',       # User name/UID
  group  => 'root',       # Group name/GID
  mode   => '0644',       # File permissions
  backup => '.backup',    # Backup before changes
  force  => true,         # Force symlink creation
}

# Execution attributes
exec { 'my_command':
  command => '/usr/bin/command',
  user    => 'nobody',      # Run as user
  group   => 'nogroup',     # Run as group
  cwd     => '/opt',        # Change directory
  timeout => 300,           # Timeout in seconds
  creates => '/tmp/file',   # Only run if not exists
  onlyif  => '/usr/bin/test -f /etc/flag',  # Run if condition true
  unless  => '/usr/bin/test -f /tmp/skip',  # Run if condition false
}

# Relationship attributes
package { 'nginx':
  ensure  => present,
  require => Package['build-essential'],  # Require dependency
}

service { 'nginx':
  ensure    => running,
  subscribe => File['/etc/nginx/nginx.conf'],  # Subscribe to changes
  notify    => Service['php-fpm'],              # Notify on change
}

# Array of resources
exec { 'deploy_app':
  command => '/opt/deploy.sh',
  require => [
    Package['git'],
    Package['nodejs'],
    User['appuser'],
  ],
}
```

---

## 6. Classes

### Define Classes

```puppet
# modules/webserver/manifests/init.pp
class webserver (
  Integer $port = 80,
  Integer $workers = 4,
  String $user = 'www-data',
  String $group = 'www-data',
) {
  # Class code
  package { 'nginx':
    ensure => present,
  }

  file { '/etc/nginx/nginx.conf':
    ensure  => present,
    content => template('webserver/nginx.conf.erb'),
    require => Package['nginx'],
    notify  => Service['nginx'],
  }

  service { 'nginx':
    ensure  => running,
    enable  => true,
    require => Package['nginx'],
  }
}
```

### Use Classes

```puppet
# Include class
include webserver

# Include with parameters
class { 'webserver':
  port    => 8080,
  workers => 8,
}

# Include multiple classes
include webserver
include database
include monitoring

# Declare with parameters
class { 'database':
  engine  => 'postgresql',
  version => '12',
  backup  => true,
}
```

### Class Inheritance

```puppet
# Base class
class base {
  package { 'curl':
    ensure => present,
  }
}

# Inherited class
class webserver inherits base {
  package { 'nginx':
    ensure => present,
  }

  service { 'nginx':
    ensure => running,
  }
}
```

---

## 7. Modules

### Module Structure

```
webserver/
├── manifests/
│   ├── init.pp                 # Main class
│   ├── install.pp              # Install class
│   ├── config.pp               # Config class
│   └── service.pp              # Service class
├── templates/
│   ├── nginx.conf.erb          # Template
│   └── site.conf.erb           # Template
├── files/
│   ├── default.html            # Static file
│   └── app.jar                 # Static file
├── spec/
│   └── spec_helper.rb          # Test helper
├── metadata.json               # Module metadata
└── README.md                   # Documentation
```

### Create Module

```bash
# Generate module
puppet module generate username-webserver

# Navigate to module
cd /etc/puppetlabs/code/modules/webserver

# Install from Puppet Forge
puppet module install puppetlabs-nginx
```

### Module Metadata

```json
// metadata.json
{
  "name": "example-webserver",
  "version": "1.0.0",
  "author": "Example Corp",
  "summary": "Puppet module to manage web servers",
  "license": "Apache-2.0",
  "source": "https://github.com/example/puppet-webserver",
  "project_page": "https://github.com/example/puppet-webserver",
  "issues_url": "https://github.com/example/puppet-webserver/issues",
  "dependencies": [
    {
      "name": "puppetlabs/stdlib",
      "version_requirement": ">= 4.6.0"
    },
    {
      "name": "puppetlabs/concat",
      "version_requirement": ">= 2.1.0"
    }
  ],
  "operatingsystem_support": [
    {
      "operatingsystem": "Ubuntu",
      "operatingsystemrelease": ["18.04", "20.04"]
    },
    {
      "operatingsystem": "CentOS",
      "operatingsystemrelease": ["7", "8"]
    }
  ]
}
```

### Module Organization

```puppet
# modules/webserver/manifests/init.pp
class webserver {
  include webserver::install
  include webserver::config
  include webserver::service

  Class['webserver::install']
  -> Class['webserver::config']
  -> Class['webserver::service']
}

# modules/webserver/manifests/install.pp
class webserver::install {
  package { 'nginx':
    ensure => present,
  }
}

# modules/webserver/manifests/config.pp
class webserver::config {
  file { '/etc/nginx/nginx.conf':
    ensure  => present,
    content => template('webserver/nginx.conf.erb'),
    require => Class['webserver::install'],
  }
}

# modules/webserver/manifests/service.pp
class webserver::service {
  service { 'nginx':
    ensure  => running,
    enable  => true,
    require => Class['webserver::config'],
  }
}
```

---

## 8. Variables & Facts

### Define Variables

```puppet
# Variables in manifests
$webserver_port = 80
$workers = 4
$user = 'www-data'

# String interpolation
$message = "Server is listening on port ${webserver_port}"

# Arrays
$packages = ['nginx', 'curl', 'git', 'vim']

# Hashes
$config = {
  'port'   => 80,
  'user'   => 'www-data',
  'group'  => 'www-data',
}

# Conditional variables
$instance_type = $facts['os']['distro']['codename'] ? {
  'focal'  => 't3.large',
  'bionic' => 't3.medium',
  default  => 't3.small',
}

# Use variables
package { $packages:
  ensure => present,
}

file { '/etc/nginx/nginx.conf':
  content => "port = ${webserver_port}\n",
}
```

### Facts

```puppet
# Access facts
$os_family = $facts['os']['family']
$hostname = $facts['networking']['hostname']
$ip_address = $facts['networking']['ip']
$memory_mb = $facts['memory']['system']['total_bytes'] / (1024 * 1024)
$processor_count = $facts['processors']['count']
$kernel = $facts['kernel']
$kernel_version = $facts['kernelversion']
$uptime = $facts['uptime']

# Use facts in conditionals
if $facts['os']['family'] == 'Debian' {
  package { 'nginx':
    ensure => present,
  }
}

# Use facts in resources
file { '/etc/hostname':
  ensure  => present,
  content => $facts['networking']['hostname'],
}

# Custom facts
class { 'webserver':
  environment => $facts['custom_environment'],
}
```

### Hiera Integration

```yaml
# /etc/puppetlabs/code/hiera.yaml
version: 5
defaults:
  datadir: data
  data_hash: yaml_data

hierarchy:
  - name: "Per-node data"
    path: "nodes/%{facts.networking.fqdn}.yaml"
  - name: "Per-role data"
    path: "role/%{facts.custom_role}.yaml"
  - name: "Common data"
    path: "common.yaml"

# common.yaml
---
webserver::port: 80
webserver::workers: 4
database::engine: postgresql
database::version: '12'

# nodes/web1.example.com.yaml
---
webserver::port: 8080
webserver::workers: 8

# Use in manifest
class { 'webserver':
  port    => lookup('webserver::port'),
  workers => lookup('webserver::workers'),
}
```

---

## 9. Conditionals & Loops

### Conditionals

```puppet
# If/elsif/else
if $facts['os']['family'] == 'Debian' {
  package { 'nginx':
    ensure => present,
  }
} elsif $facts['os']['family'] == 'RedHat' {
  package { 'httpd':
    ensure => present,
  }
} else {
  notify { 'unsupported_os': }
}

# Unless (opposite of if)
unless $facts['os']['family'] == 'Windows' {
  service { 'nginx':
    ensure => running,
  }
}

# Case statement
case $facts['os']['distro']['codename'] {
  'focal': {
    $php_version = '7.4'
  }
  'bionic': {
    $php_version = '7.2'
  }
  default: {
    $php_version = '7.0'
  }
}

# Ternary operator
$service_name = $facts['os']['family'] ? {
  'Debian' => 'nginx',
  'RedHat' => 'httpd',
}
```

### Loops

```puppet
# Array iteration
$packages = ['nginx', 'curl', 'git']
package { $packages:
  ensure => present,
}

# Hash iteration
$users = {
  'john' => { uid => 1001, home => '/home/john' },
  'jane' => { uid => 1002, home => '/home/jane' },
}

$users.each |$name, $attrs| {
  user { $name:
    uid        => $attrs['uid'],
    home       => $attrs['home'],
    managehome => true,
  }
}

# Range iteration
$ports = [80, 443, 8080, 8443]
$ports.each |$port| {
  firewall { "allow_port_${port}":
    port   => $port,
    action => 'accept',
  }
}

# Map
$doubled = [1, 2, 3].map |$x| { $x * 2 }
# Result: [2, 4, 6]

# Select
$even = [1, 2, 3, 4, 5].select |$x| { $x % 2 == 0 }
# Result: [2, 4]

# Reduce
$sum = [1, 2, 3, 4, 5].reduce(0) |$memo, $x| { $memo + $x }
# Result: 15
```

---

## 10. Node Management

### Configure Nodes

```puppet
# /etc/puppetlabs/code/manifests/site.pp

# Node-specific configuration
node 'web1.example.com' {
  include base
  include webserver
  
  class { 'webserver':
    port => 8080,
  }
}

node 'db1.example.com' {
  include base
  include database
  
  class { 'database':
    engine => 'postgresql',
  }
}

# Wildcard nodes
node /^web\d+\.example\.com$/ {
  include base
  include webserver
}

# Default node
node default {
  include base
}
```

### Node Definition with Facts

```puppet
node default {
  case $facts['os']['family'] {
    'Debian': {
      include debian_base
    }
    'RedHat': {
      include redhat_base
    }
  }

  # Common classes for all nodes
  include monitoring
  include logging
  include security
}
```

### Manage Nodes with Puppet

```bash
# List certificates
sudo puppet cert list

# List all certificates
sudo puppet cert list --all

# Sign certificate request
sudo puppet cert sign web1.example.com

# Sign all pending requests
sudo puppet cert sign --all

# Revoke certificate
sudo puppet cert revoke web1.example.com

# Clean certificate
sudo puppet cert clean web1.example.com

# Force purge
sudo puppet cert clean --all
```

### Run Puppet Agent

```bash
# Run puppet agent manually
sudo puppet agent --test

# Run with verbose output
sudo puppet agent --test --verbose

# Run without caching
sudo puppet agent --test --no-daemonize

# Run specific manifest
sudo puppet apply /path/to/manifest.pp

# Dry-run (noop)
sudo puppet agent --test --noop

# Set puppet to run in background
sudo puppet agent --enable
sudo puppet agent --disable "Maintenance window"

# View agent status
sudo puppet agent --status
```

---

## 11. Best Practices

### Modular Design

```puppet
# Good structure - separate concerns
class webserver {
  include webserver::install
  include webserver::config
  include webserver::service
}

class webserver::install {
  package { 'nginx':
    ensure => present,
  }
}

class webserver::config {
  file { '/etc/nginx/nginx.conf':
    ensure  => present,
    content => template('webserver/nginx.conf.erb'),
    require => Class['webserver::install'],
    notify  => Class['webserver::service'],
  }
}

class webserver::service {
  service { 'nginx':
    ensure  => running,
    enable  => true,
  }
}
```

### Parameterization

```puppet
# Good - parameterized class
class webserver (
  String $package_name = 'nginx',
  Integer $port = 80,
  String $user = 'www-data',
  String $group = 'www-data',
  Array[String] $packages = ['curl', 'git'],
) {
  package { 'nginx':
    ensure => present,
    name   => $package_name,
  }

  # Use parameters
  file { '/etc/nginx/nginx.conf':
    content => template('webserver/nginx.conf.erb'),
  }
}

# Use with defaults
include webserver

# Use with custom values
class { 'webserver':
  port => 8080,
  user => 'nginx',
}
```

### Dependency Management

```puppet
# Use arrow operators for ordering
Class['webserver::install']
-> Class['webserver::config']
-> Class['webserver::service']

# Use require and notify
package { 'nginx':
  ensure => present,
  notify => Service['nginx'],
}

service { 'nginx':
  ensure  => running,
  require => Package['nginx'],
}

# Use subscribe
service { 'nginx':
  ensure    => running,
  subscribe => File['/etc/nginx/nginx.conf'],
}
```

### Idempotent Resources

```puppet
# Good - creates file only if not exists
file { '/tmp/setup.done':
  ensure  => present,
  content => 'Setup completed',
}

# Good - runs only if condition not met
exec { 'setup_database':
  command => '/usr/local/bin/setup_db.sh',
  unless  => '/usr/local/bin/check_db.sh',
}

# Bad - runs every time
exec { 'run_script':
  command => '/usr/local/bin/script.sh',
}
```

### Documentation

```puppet
# modules/webserver/manifests/init.pp
#
# @summary Installs and manages Nginx web server
#
# @param port
#   The port Nginx listens on
#   Type: Integer
#   Default: 80
#
# @param workers
#   Number of worker processes
#   Type: Integer
#   Default: 4
#
# @example Basic usage
#   include webserver
#
# @example Custom configuration
#   class { 'webserver':
#     port => 8080,
#   }
#
class webserver (
  Integer $port = 80,
  Integer $workers = 4,
) {
  # Implementation
}
```

---

## 12. Troubleshooting

### Debug Mode

```bash
# Run with debug output
sudo puppet agent --test --debug

# Verbose output
sudo puppet agent --test --verbose

# Trace output (very detailed)
sudo puppet agent --test --trace

# Show resource order
sudo puppet agent --test --graph

# Dry-run to see changes
sudo puppet agent --test --noop

# Check for syntax errors
puppet parser validate /etc/puppetlabs/code/manifests/site.pp
```

### Common Issues

```puppet
# Issue: Undefined variable
# Wrong:
file { '/tmp/test.txt':
  content => $undefined_var,
}

# Right:
file { '/tmp/test.txt':
  content => $defined_var,
}

# Issue: Circular dependency
# Wrong - circular reference
Package['nginx'] -> Service['nginx'] -> Package['nginx']

# Issue: Missing requires
# Wrong - service might start before package installed
service { 'nginx':
  ensure => running,
}

# Right - add requires
service { 'nginx':
  ensure  => running,
  require => Package['nginx'],
}

# Issue: Wrong resource name
# Wrong:
subscribe => File['/etc/nginx/nginx.conf']

# Right:
subscribe => File['/etc/nginx/nginx.conf']
```

### Debugging Commands

```bash
# Validate manifest
puppet parser validate manifest.pp

# Show syntax errors
puppet apply --syntax-only manifest.pp

# Test manifest locally
puppet apply manifest.pp --noop

# Get facts
facter
facter networking.ip
facter os.family

# Test resource
puppet resource package nginx
puppet resource file /tmp

# Compile catalog
puppet agent --test --catalog_cache_terminus json

# View log files
sudo tail -f /var/log/puppetlabs/puppet/puppet.log

# Check certificate status
sudo puppet cert list

# Debug Hiera lookups
puppet lookup webserver::port --debug

# Run single class
sudo puppet apply -e "include webserver"
```

### Test Syntax

```bash
# Check all manifests
for f in /etc/puppetlabs/code/manifests/*.pp; do
  puppet parser validate "$f"
done

# Check module syntax
puppet parser validate /etc/puppetlabs/code/modules/webserver/manifests/init.pp

# Lint puppet code
puppet-lint /etc/puppetlabs/code/manifests/site.pp
puppet-lint /etc/puppetlabs/code/modules/webserver/

# Style check
rubocop /etc/puppetlabs/code/modules/webserver/
```

---

## Quick Reference Commands

```bash
# Agent Operations
sudo puppet agent --test                          # Run agent once
sudo puppet agent --test --noop                   # Dry-run
sudo puppet agent --enable                        # Enable agent
sudo puppet agent --disable "reason"              # Disable agent
sudo puppet agent --status                        # Check status

# Certificates
sudo puppet cert list                             # List pending
sudo puppet cert sign node.example.com            # Sign certificate
sudo puppet cert list --all                       # List all certs
sudo puppet cert revoke node.example.com          # Revoke cert

# Manifest Operations
puppet parser validate manifest.pp                # Check syntax
puppet apply manifest.pp                          # Apply manifest
puppet apply manifest.pp --noop                   # Dry-run
puppet apply -e "include class"                   # Apply single class

# Debugging
sudo puppet agent --test --debug                  # Debug output
sudo puppet agent --test --verbose                # Verbose output
facter                                            # Show facts
puppet lookup key --debug                         # Debug Hiera

# Module Management
puppet module list                                # List modules
puppet module search nginx                        # Search Forge
puppet module install puppetlabs/nginx            # Install module
puppet module upgrade puppetlabs/nginx            # Update module
```

---

## Essential Puppet Concepts

### Resources
Atomic units describing system state (packages, services, files, etc.)

### Manifests
Text files containing Puppet code that define resources

### Modules
Organized collections of classes, files, and templates

### Classes
Named blocks of Puppet code that are reusable and parameterizable

### Facts
System information automatically collected by Puppet

### Hiera
Data lookup system for separating code from data

### Catalogs
Compiled configuration sent from master to agent

### Idempotence
Resources safe to apply multiple times without unintended changes

---

## Resources

| Resource | URL |
|----------|-----|
| **Official Documentation** | https://puppet.com/docs/ |
| **Puppet Language Reference** | https://puppet.com/docs/puppet/latest/lang_intro.html |
| **Resource Type Reference** | https://puppet.com/docs/puppet/latest/type.html |
| **Puppet Forge Modules** | https://forge.puppet.com/ |
| **Best Practices** | https://puppet.com/docs/puppet/latest/style_guide.html |
| **Community** | https://puppet.com/community/ |

---

**Last Updated:** 2024
**Puppet Version:** 7.x+
**Ruby Version:** 2.7+

For latest information, visit [Puppet Official Documentation](https://puppet.com/docs/)