# Nagios Cheatsheet

![Nagios Logo](https://www.nagios.com/files/uploads/2020/04/nagios-core-logo.png)

---

## Table of Contents
1. [What is Nagios?](#1-what-is-nagios)
2. [Core Concepts](#2-core-concepts)
3. [Installation & Setup](#3-installation--setup)
4. [Configuration](#4-configuration)
5. [Hosts & Services](#5-hosts--services)
6. [Monitoring Checks](#6-monitoring-checks)
7. [Alerts & Notifications](#7-alerts--notifications)
8. [Web Interface](#8-web-interface)
9. [Best Practices](#9-best-practices)
10. [Troubleshooting](#10-troubleshooting)
11. [Additional Resources](#11-additional-resources)

---

## 1. What is Nagios?

Nagios is an enterprise-grade open-source monitoring system that enables organizations to identify and resolve IT infrastructure problems before they impact critical business processes. It monitors hosts, services, and applications across distributed environments.

**Key Benefits:**
- Host and service monitoring
- Alert notifications
- Event logging and reporting
- Extensible plugin architecture
- Performance graphs
- Distributed monitoring
- Web-based interface
- Cost-effective solution
- Flexible alerting rules

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Host** | Physical/virtual server or network device |
| **Service** | Process/check running on a host (CPU, disk, etc.) |
| **Plugin** | Executable script checking host/service status |
| **Check** | Execution of a plugin to determine state |
| **State** | Status of host/service (OK, WARNING, CRITICAL, UNKNOWN) |
| **Notification** | Alert sent when state changes |
| **Contact** | Person/group receiving notifications |
| **Command** | Definition of check plugin execution |
| **Time Period** | When notifications are sent |
| **Dependency** | Relationship between hosts/services |

---

## 3. Installation & Setup

### Linux Installation (Ubuntu/Debian)

```bash
# Update system
sudo apt-get update
sudo apt-get upgrade -y

# Install dependencies
sudo apt-get install -y build-essential libgd-dev libmysqlclient-dev \
  libssl-dev libmcrypt-dev libpng-dev apache2 php php-gd

# Create nagios user
sudo useradd nagios
sudo usermod -a -G nagios www-data

# Download Nagios Core
cd /tmp
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.10.tar.gz
tar xzf nagios-4.4.10.tar.gz
cd nagios-4.4.10

# Configure
./configure --with-command-group=nagios

# Build and install
make all
sudo make install
sudo make install-commandmode
sudo make install-init
sudo make install-config
sudo make install-webconf

# Set Apache user
sudo htpasswd -bc /usr/local/nagios/etc/htpasswd.users nagiosadmin admin123

# Start services
sudo systemctl start nagios
sudo systemctl start apache2
sudo systemctl enable nagios
sudo systemctl enable apache2

# Verify
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

### macOS Installation

```bash
# Using Homebrew
brew install nagios

# Or download from
# https://www.nagios.com/downloads/nagios-core/

# Start service
brew services start nagios

# Verify
brew services list | grep nagios
```

### Docker Installation

```bash
# Run Nagios container
docker run -d \
  --name nagios \
  -p 80:80 \
  -p 5667:5667 \
  -e NAGIOS_PASSWD=admin123 \
  jasonrivers/nagios:latest

# Access web interface
# http://localhost/nagios
# Username: nagiosadmin
# Password: admin123
```

### Docker Compose Setup

```yaml
version: '3.8'

services:
  nagios:
    image: jasonrivers/nagios:latest
    container_name: nagios
    ports:
      - "80:80"
      - "5667:5667"
    environment:
      - NAGIOS_PASSWD=admin123
    volumes:
      - ./nagios/etc:/opt/nagios/etc
      - ./nagios/var:/opt/nagios/var
      - ./nagios/plugins:/opt/Custom-Nagios-Plugins
    networks:
      - monitoring

  nrpe:
    image: jasonrivers/nagios-nrpe-server:latest
    container_name: nrpe
    ports:
      - "5666:5666"
    networks:
      - monitoring

volumes:
  nagios-etc:
  nagios-var:

networks:
  monitoring:
```

### Important Ports & Credentials

| Item | Details |
|------|---------|
| **Web UI** | http://localhost/nagios |
| **HTTP Port** | 80 |
| **HTTPS Port** | 443 |
| **NRPE Port** | 5666 |
| **Default User** | nagiosadmin |
| **Default Password** | (set during install) |

---

## 4. Configuration

### Main Configuration File

```bash
# Location
/usr/local/nagios/etc/nagios.cfg

# Verify configuration
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

# Reload configuration
sudo systemctl reload nagios
```

### nagios.cfg Overview

```ini
# /usr/local/nagios/etc/nagios.cfg

# Location of object configuration files
cfg_dir=/usr/local/nagios/etc/objects

# Resource file with macros
resource_file=/usr/local/nagios/etc/resource.cfg

# Status log file
status_file=/usr/local/nagios/var/status.dat

# Check result directory
checkresult_directory=/usr/local/nagios/var/spool/checkresults

# Logging
log_file=/usr/local/nagios/var/nagios.log
log_retention_days=30

# Status update interval (seconds)
status_update_interval=10
refresh_rate=90

# Enable notifications
enable_notifications=1
enable_service_checks=1
enable_host_checks=1

# Retention data
retain_status_information=1
retain_nonstatus_information=1

# Flap detection
enable_flap_detection=1
low_service_flap_threshold=5.0
high_service_flap_threshold=20.0

# Event handling
enable_event_handlers=1

# Performance data
process_performance_data=1

# Obsess over services
obsess_over_services=0
```

### Object Configuration Directory

```bash
# Location
/usr/local/nagios/etc/objects/

# Files
contacts.cfg          # Contact definitions
commands.cfg          # Command definitions
hosts.cfg             # Host definitions
services.cfg          # Service definitions
timeperiods.cfg       # Time period definitions
templates.cfg         # Template definitions

# Include all
cfg_dir=/usr/local/nagios/etc/objects
```

---

## 5. Hosts & Services

### Define Hosts

```bash
# /usr/local/nagios/etc/objects/hosts.cfg

define host{
    use                     linux-server        # Template to use
    host_name               web-server-01
    alias                   Web Server 1
    address                 192.168.1.10
    hostgroups              linux-servers
    parents                 core-router
    check_interval          5
    retry_interval          1
    max_check_attempts      3
}

define host{
    use                     linux-server
    host_name               db-server-01
    alias                   Database Server 1
    address                 192.168.1.20
    hostgroups              databases
}

define host{
    use                     windows-server
    host_name               win-server-01
    alias                   Windows Server 1
    address                 192.168.1.30
}

# Define host group
define hostgroup{
    hostgroup_name          linux-servers
    alias                   Linux Servers
    members                 web-server-01,db-server-01
}

define hostgroup{
    hostgroup_name          databases
    alias                   Database Servers
    members                 db-server-01
}
```

### Define Services

```bash
# /usr/local/nagios/etc/objects/services.cfg

define service{
    use                     local-service
    host_name               web-server-01
    service_description     CPU Load
    check_command           check_local_load!5.0,4.0!10.0,6.0
}

define service{
    use                     local-service
    host_name               web-server-01
    service_description     Disk Usage
    check_command           check_local_disk!20%!10%!/
}

define service{
    use                     local-service
    host_name               web-server-01
    service_description     Memory Usage
    check_command           check_local_swap!20!10
}

define service{
    use                     local-service
    host_name               web-server-01
    service_description     HTTP
    check_command           check_http
    check_interval          5
    notification_interval   30
}

define service{
    use                     local-service
    host_name               db-server-01
    service_description     MySQL
    check_command           check_mysql
    servicegroups           databases
}

# Define service group
define servicegroup{
    servicegroup_name       web-services
    alias                   Web Services
    members                 web-server-01,HTTP
}
```

### Host Templates

```bash
define host{
    name                    linux-server
    use                     generic-host
    check_period            24x7
    check_interval          5
    retry_interval          1
    max_check_attempts      3
    check_command           check-host-alive
    notification_period     24x7
    notification_interval   30
    notification_options    d,r,f,s
    contact_groups          admins
    register                0              # Don't register template
}

define host{
    name                    windows-server
    use                     generic-host
    check_command           check-host-alive
    register                0
}

define host{
    name                    generic-host
    notification_period     24x7
    notification_options    d,r,f
    contact_groups          admins
    register                0
}
```

### Service Templates

```bash
define service{
    name                    local-service
    use                     generic-service
    max_check_attempts      4
    check_interval          5
    retry_interval          1
    check_period            24x7
    notification_period     24x7
    notification_interval   30
    notification_options    w,c,r,f,s
    register                0
}

define service{
    name                    generic-service
    notification_options    w,c,r
    is_volatile             0
    check_freshness         0
    flap_detection_enabled  1
    low_flap_threshold      25.0
    high_flap_threshold     50.0
    register                0
}
```

---

## 6. Monitoring Checks

### Check Commands

```bash
# /usr/local/nagios/etc/objects/commands.cfg

# HTTP check
define command{
    command_name    check_http
    command_line    $USER1$/check_http -I $HOSTADDRESS$ -w 10 -c 30
}

# TCP port check
define command{
    command_name    check_tcp
    command_line    $USER1$/check_tcp -H $HOSTADDRESS$ -p $ARG1$
}

# SSH check
define command{
    command_name    check_ssh
    command_line    $USER1$/check_ssh $HOSTADDRESS$
}

# SMTP check
define command{
    command_name    check_smtp
    command_line    $USER1$/check_smtp -H $HOSTADDRESS$
}

# MySQL check
define command{
    command_name    check_mysql
    command_line    $USER1$/check_mysql -H $HOSTADDRESS$ -u $ARG1$ -p $ARG2$
}

# SNMP check
define command{
    command_name    check_snmp
    command_line    $USER1$/check_snmp -H $HOSTADDRESS$ -C $ARG1$ -o $ARG2$
}

# Remote NRPE check
define command{
    command_name    check_nrpe
    command_line    $USER1$/check_nrpe -H $HOSTADDRESS$ -c $ARG1$
}

# Ping check
define command{
    command_name    check-host-alive
    command_line    $USER1$/check_ping -H $HOSTADDRESS$ -w 3000.0,80% -c 5000.0,100% -p 5
}
```

### Common Plugins

```bash
# Check CPU load
check_local_load -w 5.0,4.0,3.0 -c 10.0,6.0,4.0

# Check disk space
check_local_disk -w 20% -c 10% -p /

# Check memory
check_local_swap -w 20 -c 10

# Check HTTP
check_http -H example.com -w 5 -c 10

# Check TCP port
check_tcp -H 192.168.1.1 -p 3306

# Check DNS
check_dns -H example.com -s 8.8.8.8

# Check SNMP
check_snmp -H 192.168.1.1 -C public -o 1.3.6.1.2.1.1.1.0

# Check NRPE
check_nrpe -H 192.168.1.10 -c check_disk

# Check process
check_procs -w 50:100 -c 100:

# Check connections
check_tcp_conn_stat
```

### NRPE Configuration

```bash
# /usr/local/nagios/etc/nrpe.cfg

# Allowed hosts
allowed_hosts=127.0.0.1,192.168.1.10

# Enable SSL
ssl_version=TLSv1.2
ssl_verify_certificate=1
ssl_cert_file=/etc/nagios/nrpe_local.crt
ssl_key_file=/etc/nagios/nrpe_local.key

# Commands (on remote host)
command[check_users]=/usr/lib/nagios/plugins/check_users -w 5 -c 10
command[check_load]=/usr/lib/nagios/plugins/check_load -r -w .15,.10,.05 -c .30,.25,.20
command[check_disk]=/usr/lib/nagios/plugins/check_disk -w 20% -c 10%
command[check_memory]=/usr/lib/nagios/plugins/check_memory -w 80% -c 90%
command[check_swap]=/usr/lib/nagios/plugins/check_swap -w 20% -c 10%
command[check_zombie_procs]=/usr/lib/nagios/plugins/check_procs -w 5 -c 10 -s Z
command[check_total_procs]=/usr/lib/nagios/plugins/check_procs -w 250 -c 400
```

---

## 7. Alerts & Notifications

### Define Contacts

```bash
# /usr/local/nagios/etc/objects/contacts.cfg

define contact{
    contact_name                    john
    use                             generic-contact
    alias                           John Doe
    email                           john@example.com
    pager                           5551234567@sms.example.com
    can_submit_commands             1
}

define contact{
    contact_name                    jane
    use                             generic-contact
    alias                           Jane Smith
    email                           jane@example.com
    can_submit_commands             1
}

define contactgroup{
    contactgroup_name               admins
    alias                           Nagios Administrators
    members                         john,jane
}

define contactgroup{
    contactgroup_name               oncall
    alias                           On-Call Staff
    members                         john
}
```

### Notification Configuration

```bash
define host{
    host_name               web-server-01
    ...
    contact_groups          admins
    notification_enabled    1
    notification_period     24x7
    notification_interval   30          # Minutes between notifications
    notification_options    d,r,u,f,s   # Down, Recovery, Unreachable, Flapping, Scheduled
    first_notification_delay 0          # Minutes before first notification
}

define service{
    host_name               web-server-01
    service_description     HTTP
    ...
    contact_groups          admins
    notification_enabled    1
    notification_period     business-hours
    notification_interval   30
    notification_options    w,c,r,u,f   # Warning, Critical, Recovery, Unknown, Flapping
}
```

### Define Time Periods

```bash
define timeperiod{
    timeperiod_name     24x7
    alias               24 Hours a Day, 7 Days a Week
    sunday              00:00-24:00
    monday              00:00-24:00
    tuesday             00:00-24:00
    wednesday           00:00-24:00
    thursday            00:00-24:00
    friday              00:00-24:00
    saturday            00:00-24:00
}

define timeperiod{
    timeperiod_name     business-hours
    alias               Business Hours
    monday              09:00-17:00
    tuesday             09:00-17:00
    wednesday           09:00-17:00
    thursday            09:00-17:00
    friday              09:00-17:00
}

define timeperiod{
    timeperiod_name     business-hours-and-weekend
    alias               Business Hours and Weekend
    monday              09:00-17:00
    tuesday             09:00-17:00
    wednesday           09:00-17:00
    thursday            09:00-17:00
    friday              09:00-17:00
    saturday            00:00-24:00
    sunday              00:00-24:00
}
```

### Notification Commands

```bash
define command{
    command_name    notify-host-by-email
    command_line    /usr/bin/printf "%b" "***** Nagios *****\n\nNotification Type: $NOTIFICATIONTYPE$\nHost: $HOSTNAME$\nState: $HOSTSTATE$\nAddress: $HOSTADDRESS$\nInfo: $HOSTOUTPUT$\n\nDate/Time: $LONGDATETIME$\n" | /usr/bin/mail -s "Host $HOSTSTATE$ alert for $HOSTNAME$!" $CONTACTEMAIL$
}

define command{
    command_name    notify-service-by-email
    command_line    /usr/bin/printf "%b" "***** Nagios *****\n\nNotification Type: $NOTIFICATIONTYPE$\nService: $SERVICEDESC$\nHost: $HOSTNAME$\nAddress: $HOSTADDRESS$\nState: $SERVICESTATE$\nInfo: $SERVICEOUTPUT$\n\nDate/Time: $LONGDATETIME$\n" | /usr/bin/mail -s "** $NOTIFICATIONTYPE$ Service Alert: $HOSTNAME$/$SERVICEDESC$ is $SERVICESTATE$ **" $CONTACTEMAIL$
}
```

---

## 8. Web Interface

### Access Web UI

```
URL: http://localhost/nagios
Username: nagiosadmin
Password: (set during installation)

Default paths:
Main page:      /nagios/
Hosts:          /nagios/?hostgroup=all
Services:       /nagios/?servicegroup=all
Alerts:         /nagios/?alert_type=2
Reports:        /nagios/side.html
```

### Web Interface Sections

```
1. Tactical Overview
   - Current status of all hosts/services
   - Problem summary
   - Network outages

2. Service Details
   - Service status
   - Check details
   - Performance graphs

3. Host Details
   - Host status
   - Services on host
   - Host information

4. Alerts
   - Recent alerts
   - Alert history
   - Notifications

5. Reports
   - Availability reports
   - Alert reports
   - Service reports

6. Configuration
   - Host configuration
   - Service configuration
   - Contact configuration
```

### GUI Actions

```
Available Commands:
- Acknowledge alert
- Submit passive check
- Schedule downtime
- Add comment
- Send notification
- Enable/disable checks
- Enable/disable notifications
- Reset performance data
```

---

## 9. Best Practices

### Naming Convention

```bash
# Hosts
web-prod-01, web-prod-02
db-prod-01, db-prod-02
app-staging-01

# Services
CPU Load
Disk Usage
Memory
HTTP
MySQL
SSH
```

### Check Configuration

```bash
# Appropriate intervals
Critical services:    check_interval=1   (1 minute)
Important services:   check_interval=5   (5 minutes)
Standard services:    check_interval=15  (15 minutes)
Non-critical:         check_interval=30  (30 minutes)

# Retry strategy
retry_interval=1
max_check_attempts=3

# Smart notifications
notification_interval=30    # Minutes between notifications
first_notification_delay=0  # Notify immediately
```

### Performance Optimization

```bash
# Caching
cached_service_check_horizon=15

# Service dependencies
enable_predictive_dependency_checks=1

# Optimization
enable_environment_macros=0    # If not using
enable_flap_detection=1
enable_event_handlers=1
```

### Monitoring Strategy

```
1. Monitor critical systems first
   - Web servers
   - Databases
   - Load balancers
   - Core routers

2. Essential checks
   - Host reachability (ping)
   - Service availability
   - Resource utilization (CPU, memory, disk)
   - Application health

3. Escalation policy
   - Initial: Email to on-call
   - 30 minutes: SMS alert
   - 1 hour: Page manager

4. Notification rules
   - Only alert on state changes
   - Avoid alert fatigue
   - Use time-based notifications
```

---

## 10. Troubleshooting

### Configuration Issues

```bash
# Verify configuration
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

# Check syntax of specific file
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/objects/hosts.cfg

# Check logs
tail -f /usr/local/nagios/var/nagios.log

# Check for errors in web interface
Status → Nagios Information → Configuration
```

### Service Check Issues

```bash
# Test check manually
/usr/local/nagios/libexec/check_http -H example.com

# Test NRPE
/usr/local/nagios/libexec/check_nrpe -H 192.168.1.10 -c check_disk

# Check command definitions
grep "command_name check_http" /usr/local/nagios/etc/objects/commands.cfg

# Verify plugins exist
ls -la /usr/local/nagios/libexec/
```

### Notification Problems

```bash
# Check mail configuration
sudo systemctl status postfix
sudo tail -f /var/log/mail.log

# Test email command
echo "Test email" | mail -s "Test" nagiosadmin@localhost

# Check contact email
grep "email" /usr/local/nagios/etc/objects/contacts.cfg

# Enable debug logging
nagios -d /usr/local/nagios/etc/nagios.cfg

# Check notification queue
ls -la /usr/local/nagios/var/spool/checkresults/
```

### Web Interface Issues

```bash
# Restart Apache
sudo systemctl restart apache2

# Check Apache status
sudo systemctl status apache2

# Check Apache error log
sudo tail -f /var/log/apache2/error.log

# Check Nagios process
ps aux | grep nagios

# Restart Nagios
sudo systemctl restart nagios

# Change htpasswd
sudo htpasswd /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

### Performance Issues

```bash
# Reduce check frequency for non-critical services
check_interval=30  # Increase from 5

# Disable flap detection if not needed
enable_flap_detection=0

# Check Nagios process load
top | grep nagios

# Review check results
ls -la /usr/local/nagios/var/spool/checkresults/

# Optimize queries
# Reduce number of services per host
# Use service dependencies
```

---

## 11. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Getting Started** | https://www.nagios.com/downloads/nagios-core/ |
| **Documentation** | https://www.nagios.org/documentation/ |
| **Plugins** | https://www.nagios-plugins.org/ |
| **Exchange** | https://exchange.nagios.org/ |
| **API Guide** | https://www.nagios.org/documentation/usersguide/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Official Site** | https://www.nagios.com/ |
| **Community** | https://support.nagios.com/ |
| **GitHub** | https://github.com/NagiosEnterprises/ |
| **Tutorials** | https://nagios.org/documentation/usersguide/ |

---

## Quick Reference

```bash
# Start/Stop Nagios
sudo systemctl start nagios
sudo systemctl stop nagios
sudo systemctl restart nagios
sudo systemctl status nagios

# Verify configuration
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

# Reload configuration
sudo systemctl reload nagios

# View logs
tail -f /usr/local/nagios/var/nagios.log

# Check specific plugin
/usr/local/nagios/libexec/check_http -H example.com

# Test NRPE
/usr/local/nagios/libexec/check_nrpe -H 192.168.1.10

# Web interface
# http://localhost/nagios

# Change admin password
sudo htpasswd /usr/local/nagios/etc/htpasswd.users nagiosadmin

# Install plugin
cd /tmp
wget https://github.com/nagios-plugins/nagios-plugins/releases/download/release-2.4.0/nagios-plugins-2.4.0.tar.gz
tar xzf nagios-plugins-2.4.0.tar.gz
cd nagios-plugins-2.4.0
./configure
make
sudo make install

# Check plugin
/usr/local/nagios/libexec/check_http -h

# Enable notifications
systemctl reload nagios

# Check running processes
ps aux | grep nagios

# Docker
docker run -d -p 80:80 jasonrivers/nagios:latest
docker logs -f nagios
docker exec nagios /bin/bash

# Reload config in Docker
docker exec nagios /etc/init.d/nagios reload
```

---

## File Locations

| Item | Path |
|------|------|
| **Config File** | /usr/local/nagios/etc/nagios.cfg |
| **Objects Dir** | /usr/local/nagios/etc/objects/ |
| **Plugins** | /usr/local/nagios/libexec/ |
| **Resource File** | /usr/local/nagios/etc/resource.cfg |
| **Log File** | /usr/local/nagios/var/nagios.log |
| **Status Data** | /usr/local/nagios/var/status.dat |
| **Web Dir** | /usr/local/nagios/share/ |
| **CGI Dir** | /usr/local/nagios/sbin/ |
| **NRPE Config** | /usr/local/nagios/etc/nrpe.cfg |

---

**Last Updated:** 2026
**Nagios Version:** 4.4.x+
**License:** GPL v2

For latest information, visit [Nagios Official Site](https://www.nagios.com/)