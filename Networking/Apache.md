# Apache Cheatsheet

![Apache Logo](https://www.apache.org/foundation/press/kit/asf_logo.png)

---

## Table of Contents
1. [What is Apache?](#1-what-is-apache)
2. [Core Concepts](#2-core-concepts)
3. [Installation & Setup](#3-installation--setup)
4. [Configuration](#4-configuration)
5. [Virtual Hosts](#5-virtual-hosts)
6. [Modules & Optimization](#6-modules--optimization)
7. [SSL/TLS Configuration](#7-ssltls-configuration)
8. [Reverse Proxy & Load Balancing](#8-reverse-proxy--load-balancing)
9. [Logging & Monitoring](#9-logging--monitoring)
10. [Best Practices](#10-best-practices)
11. [Troubleshooting](#11-troubleshooting)
12. [Additional Resources](#12-additional-resources)

---

## 1. What is Apache?

Apache HTTP Server is a free, open-source web server software that powers a large portion of websites on the internet. It's highly modular, extensible, and widely used for serving web content, running web applications, and acting as a reverse proxy.

**Key Benefits:**
- Open-source and free
- Highly modular architecture
- Cross-platform support
- URL rewriting capabilities
- Virtual hosting support
- SSL/TLS support
- Reverse proxy functionality
- Load balancing
- Dynamic module loading
- Extensive logging

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **VirtualHost** | Multiple websites on single server |
| **Directive** | Configuration instruction |
| **Module** | Apache extension providing functionality |
| **Handler** | Processes requests for specific content |
| **Filter** | Processes response data |
| **Document Root** | Base directory for website files |
| **ServerName** | Primary domain name |
| **ServerAlias** | Additional domain names |
| **Port** | Network port (80 HTTP, 443 HTTPS) |
| **Directory** | Configuration for specific directories |

---

## 3. Installation & Setup

### Linux Installation (Ubuntu/Debian)

```bash
# Update system
sudo apt-get update
sudo apt-get upgrade -y

# Install Apache
sudo apt-get install -y apache2

# Enable required modules
sudo a2enmod rewrite
sudo a2enmod ssl
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod headers
sudo a2enmod deflate

# Start service
sudo systemctl start apache2
sudo systemctl enable apache2

# Verify installation
sudo systemctl status apache2
apache2 -v
```

### macOS Installation

```bash
# Using Homebrew
brew install httpd

# Start service
brew services start httpd

# Verify
brew services list | grep httpd
httpd -v
```

### Linux Installation (RedHat/CentOS)

```bash
# Install Apache
sudo yum install -y httpd

# Enable modules
sudo yum install -y mod_ssl mod_rewrite

# Start service
sudo systemctl start httpd
sudo systemctl enable httpd

# Verify
sudo systemctl status httpd
httpd -v
```

### Docker Installation

```bash
# Run Apache container
docker run -d \
  --name apache \
  -p 80:80 \
  -p 443:443 \
  -v /var/www/html:/usr/local/apache2/htdocs \
  -v /etc/apache2:/usr/local/apache2/conf \
  httpd:latest

# Verify
docker logs apache
curl http://localhost
```

### Docker Compose Setup

```yaml
version: '3.8'

services:
  apache:
    image: httpd:latest
    container_name: apache
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./html:/usr/local/apache2/htdocs
      - ./conf:/usr/local/apache2/conf
      - ./logs:/usr/local/apache2/logs
    networks:
      - web

  php:
    image: php:8.0-fpm
    container_name: php
    volumes:
      - ./html:/var/www/html
    networks:
      - web

volumes:
  apache_logs:

networks:
  web:
```

### Important Ports

| Service | Port | Purpose |
|---------|------|---------|
| **HTTP** | 80 | Web traffic |
| **HTTPS** | 443 | Secure web traffic |
| **Proxy** | 8080 | Proxy requests |
| **Health Check** | 8888 | Server-status page |

---

## 4. Configuration

### Main Configuration File

```bash
# Location
/etc/apache2/apache2.conf        # Ubuntu/Debian
/etc/httpd/conf/httpd.conf       # RedHat/CentOS

# Test configuration
sudo apache2ctl configtest      # Ubuntu/Debian
sudo apachectl configtest       # RedHat/CentOS

# Reload configuration
sudo systemctl reload apache2    # Ubuntu/Debian
sudo systemctl reload httpd      # RedHat/CentOS
```

### Basic apache2.conf

```apache
# Global settings
ServerRoot "/etc/apache2"
Listen 80

# User and group
User www-data
Group www-data

# Admin email
ServerAdmin admin@example.com

# Server hostname
ServerName example.com
ServerSignature Off
ServerTokens Prod

# Document root
DocumentRoot /var/www/html

# Timeout settings
Timeout 300
KeepAlive On
KeepAliveTimeout 5

# Performance tuning
StartServers 8
MinSpareServers 5
MaxSpareServers 20
MaxRequestWorkers 256
MaxConnectionsPerChild 0

# Load configuration files
IncludeOptional mods-enabled/*.load
IncludeOptional mods-enabled/*.conf
IncludeOptional conf-enabled/*.conf
IncludeOptional sites-enabled/*.conf
```

### Directory Configuration

```apache
# Basic directory settings
<Directory /var/www/html>
    # Allow/Deny access
    Require all granted
    
    # Enable .htaccess
    AllowOverride All
    
    # Directory listing
    Options Indexes FollowSymLinks
    
    # Default files
    DirectoryIndex index.html index.php
</Directory>

# Restrict directory
<Directory /var/www/html/admin>
    Require all denied
</Directory>

# Allow specific IP
<Directory /var/www/html/api>
    Require ip 192.168.1.0/24
</Directory>

# Require authentication
<Directory /var/www/html/secure>
    <RequireAll>
        Require method POST
        Require valid-user
    </RequireAll>
</Directory>
```

### File Configuration

```apache
<Files ~ "\.php$">
    Require all granted
</Files>

# Restrict access to sensitive files
<Files "*.conf">
    Require all denied
</Files>

<Files "*.log">
    Require all denied
</Files>

<FilesMatch "(web\.config|\.env)$">
    Require all denied
</FilesMatch>
```

### HTTP/2 Configuration

```apache
# Enable HTTP/2
LoadModule http2_module modules/mod_http2.so

# Use HTTP/2 for HTTPS connections
Protocols h2 h1

# Per virtual host
<VirtualHost *:443>
    Protocols h2 h1
</VirtualHost>
```

---

## 5. Virtual Hosts

### Single Domain VirtualHost

```apache
# /etc/apache2/sites-available/example.com.conf

<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com
    ServerAdmin admin@example.com
    
    DocumentRoot /var/www/example.com/html
    
    <Directory /var/www/example.com/html>
        Require all granted
        AllowOverride All
        Options FollowSymLinks
        DirectoryIndex index.html index.php
    </Directory>
    
    # Logging
    ErrorLog ${APACHE_LOG_DIR}/example.com-error.log
    CustomLog ${APACHE_LOG_DIR}/example.com-access.log combined
    
    # Redirect HTTP to HTTPS
    RewriteEngine On
    RewriteCond %{SERVER_NAME} =example.com
    RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```

### HTTPS VirtualHost

```apache
<VirtualHost *:443>
    ServerName example.com
    ServerAlias www.example.com
    ServerAdmin admin@example.com
    
    DocumentRoot /var/www/example.com/html
    
    # SSL/TLS Configuration
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/example.com.crt
    SSLCertificateKeyFile /etc/ssl/private/example.com.key
    SSLCertificateChainFile /etc/ssl/certs/chain.crt
    
    # SSL protocols and ciphers
    SSLProtocol TLSv1.2 TLSv1.3
    SSLCipherSuite HIGH:!aNULL:!MD5
    
    # Security headers
    Header set Strict-Transport-Security "max-age=31536000"
    Header set X-Content-Type-Options "nosniff"
    Header set X-Frame-Options "DENY"
    Header set X-XSS-Protection "1; mode=block"
    
    <Directory /var/www/example.com/html>
        Require all granted
        AllowOverride All
        Options FollowSymLinks
    </Directory>
    
    ErrorLog ${APACHE_LOG_DIR}/example.com-error.log
    CustomLog ${APACHE_LOG_DIR}/example.com-access.log combined
</VirtualHost>
```

### Enable/Disable Sites

```bash
# Enable site
sudo a2ensite example.com

# Disable site
sudo a2dissite example.com

# List available sites
ls /etc/apache2/sites-available/

# List enabled sites
ls /etc/apache2/sites-enabled/

# Reload Apache
sudo systemctl reload apache2
```

### Multiple VirtualHosts

```apache
# Site 1
<VirtualHost *:80>
    ServerName site1.com
    DocumentRoot /var/www/site1
</VirtualHost>

# Site 2
<VirtualHost *:80>
    ServerName site2.com
    DocumentRoot /var/www/site2
</VirtualHost>

# Site 3
<VirtualHost *:80>
    ServerName site3.com
    DocumentRoot /var/www/site3
</VirtualHost>
```

### Wildcard VirtualHost

```apache
# Catch-all for subdomains
<VirtualHost *:80>
    ServerName example.com
    ServerAlias *.example.com
    DocumentRoot /var/www/example.com
    
    # Dynamic routing
    RewriteEngine On
    RewriteCond %{HTTP_HOST} ^(.+)\.example\.com$ [NC]
    RewriteRule ^ /subdomains/%1/ [L]
</VirtualHost>
```

---

## 6. Modules & Optimization

### Load Modules

```bash
# List available modules
apache2ctl -M

# Ubuntu/Debian
sudo a2enmod module_name     # Enable
sudo a2dismod module_name    # Disable

# RedHat/CentOS
# Edit /etc/httpd/conf.modules.d/*.conf
```

### Common Modules

```bash
# Core modules
mod_rewrite         # URL rewriting
mod_proxy          # Proxy functionality
mod_ssl            # SSL/TLS support
mod_deflate        # Compression
mod_headers        # HTTP headers
mod_expires        # Caching headers
mod_security       # Web Application Firewall
mod_auth_basic     # Basic authentication
mod_auth_digest    # Digest authentication
mod_php            # PHP support
mod_wsgi           # Python WSGI support
mod_perl           # Perl support
```

### Enable Common Modules

```bash
sudo a2enmod rewrite
sudo a2enmod ssl
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod headers
sudo a2enmod deflate
sudo a2enmod expires
sudo a2enmod security
sudo a2enmod http2
sudo a2enmod mpm_prefork   # or mpm_worker, mpm_event
```

### Compression (mod_deflate)

```apache
<IfModule mod_deflate.c>
    AddOutputFilterByType DEFLATE text/html text/plain text/xml
    AddOutputFilterByType DEFLATE text/css
    AddOutputFilterByType DEFLATE text/javascript application/javascript
    AddOutputFilterByType DEFLATE application/json
    
    DeflateCompressionLevel 9
</IfModule>
```

### Caching Headers (mod_expires)

```apache
<IfModule mod_expires.c>
    ExpiresActive On
    
    # Cache HTML for 1 day
    ExpiresByType text/html "access plus 1 day"
    
    # Cache CSS for 1 year
    ExpiresByType text/css "access plus 1 year"
    
    # Cache JavaScript for 1 year
    ExpiresByType application/javascript "access plus 1 year"
    
    # Cache images for 1 year
    ExpiresByType image/jpeg "access plus 1 year"
    ExpiresByType image/png "access plus 1 year"
    ExpiresByType image/gif "access plus 1 year"
    ExpiresByType image/webp "access plus 1 year"
    
    # Default 1 week
    ExpiresDefault "access plus 1 week"
</IfModule>
```

### URL Rewriting (mod_rewrite)

```apache
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    
    # Remove .html extension
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(.*)\.html$ /$1 [L,R=301]
    
    # Remove .php extension
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^([^\.]+)$ $1.php [NC,L]
    
    # Redirect HTTP to HTTPS
    RewriteCond %{HTTPS} off
    RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
    
    # Block access to sensitive files
    RewriteRule ^(\.env|\.git|wp-config\.php|config\.php)$ - [F,L]
</IfModule>
```

### Performance Tuning

```apache
# MPM (Multi-Processing Module)
# Use one at a time

# prefork - one process per connection (stable, resource-heavy)
<IfModule mpm_prefork_module>
    StartServers 8
    MinSpareServers 5
    MaxSpareServers 20
    MaxRequestWorkers 256
    MaxConnectionsPerChild 0
</IfModule>

# worker - threads per process (faster, less stable)
<IfModule mpm_worker_module>
    StartServers 2
    MinSpareServers 25
    MaxSpareServers 75
    ThreadsPerChild 25
    MaxRequestWorkers 256
</IfModule>

# event - optimized for concurrent connections
<IfModule mpm_event_module>
    StartServers 2
    MinSpareServers 25
    MaxSpareServers 75
    ThreadsPerChild 25
    MaxRequestWorkers 256
    MaxConnectionsPerChild 0
</IfModule>
```

---

## 7. SSL/TLS Configuration

### Generate Self-Signed Certificate

```bash
# Create certificate
sudo openssl req -x509 -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/server.key \
  -out /etc/ssl/certs/server.crt \
  -nodes

# Or create certificate signing request
sudo openssl req -new -newkey rsa:2048 \
  -keyout /etc/ssl/private/server.key \
  -out /etc/ssl/certs/server.csr

# Sign with certificate authority
sudo openssl x509 -req -days 365 \
  -in /etc/ssl/certs/server.csr \
  -signkey /etc/ssl/private/server.key \
  -out /etc/ssl/certs/server.crt
```

### Obtain Let's Encrypt Certificate

```bash
# Install Certbot
sudo apt-get install -y certbot python3-certbot-apache

# Generate certificate
sudo certbot certonly --apache -d example.com -d www.example.com

# Auto-renewal
sudo systemctl enable certbot.timer
sudo systemctl start certbot.timer

# Verify renewal
sudo certbot renew --dry-run
```

### SSL/TLS Configuration

```apache
<VirtualHost *:443>
    ServerName example.com
    
    # Enable SSL
    SSLEngine on
    
    # Certificate files
    SSLCertificateFile /etc/ssl/certs/example.com.crt
    SSLCertificateKeyFile /etc/ssl/private/example.com.key
    SSLCertificateChainFile /etc/ssl/certs/chain.crt
    
    # SSL protocol versions (disable old versions)
    SSLProtocol -all +TLSv1.2 +TLSv1.3
    
    # Cipher suites
    SSLCipherSuite HIGH:!aNULL:!MD5:!3DES
    SSLHonorCipherOrder on
    
    # Performance
    SSLSessionCache shmcb:/var/cache/apache2/ssl_scache(512000)
    SSLSessionCacheTimeout 300
    
    # Security headers
    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
    Header always set X-Content-Type-Options "nosniff"
    Header always set X-Frame-Options "DENY"
    Header always set X-XSS-Protection "1; mode=block"
    Header always set Referrer-Policy "no-referrer"
    
    DocumentRoot /var/www/example.com
    
    <Directory /var/www/example.com>
        Require all granted
    </Directory>
</VirtualHost>
```

### HTTP to HTTPS Redirect

```apache
# Redirect all HTTP to HTTPS
<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com
    
    RewriteEngine On
    RewriteCond %{HTTPS} off
    RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
</VirtualHost>
```

---

## 8. Reverse Proxy & Load Balancing

### Basic Reverse Proxy

```apache
# Forward requests to backend server
<VirtualHost *:80>
    ServerName example.com
    
    ProxyPreserveHost On
    ProxyPass / http://backend-server:8080/
    ProxyPassReverse / http://backend-server:8080/
    
    # Headers
    RequestHeader set X-Forwarded-For "%{REMOTE_ADDR}s"
    RequestHeader set X-Forwarded-Proto "http"
</VirtualHost>
```

### HTTPS to HTTP Backend

```apache
<VirtualHost *:443>
    ServerName example.com
    
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/example.com.crt
    SSLCertificateKeyFile /etc/ssl/private/example.com.key
    
    # Proxy to backend
    ProxyPreserveHost On
    ProxyPass / http://backend-server:8080/
    ProxyPassReverse / http://backend-server:8080/
    
    # Forward client info
    RequestHeader set X-Forwarded-For "%{REMOTE_ADDR}s"
    RequestHeader set X-Forwarded-Proto "https"
    RequestHeader set X-Forwarded-Host "%{HTTP_HOST}s"
</VirtualHost>
```

### Load Balancing

```apache
# Define backend pool
<Proxy balancer://mybalancer>
    BalancerMember http://backend1:8080
    BalancerMember http://backend2:8080
    BalancerMember http://backend3:8080
    ProxySet lbmethod=byrequests
</Proxy>

# Use balancer
<VirtualHost *:80>
    ServerName example.com
    
    ProxyPreserveHost On
    ProxyPass / balancer://mybalancer/
    ProxyPassReverse / balancer://mybalancer/
</VirtualHost>
```

### Sticky Sessions

```apache
<Proxy balancer://mybalancer>
    BalancerMember http://backend1:8080 route=1
    BalancerMember http://backend2:8080 route=2
    BalancerMember http://backend3:8080 route=3
    ProxySet stickysession=JSESSIONID scolonpathdelim=on
</Proxy>
```

### Health Check

```apache
<Proxy balancer://mybalancer>
    BalancerMember http://backend1:8080 ping=5
    BalancerMember http://backend2:8080 ping=5
    BalancerMember http://backend3:8080 ping=5
</Proxy>

# Enable manager page
<Location /balancer-manager>
    SetHandler balancer-manager
    Require ip 192.168.1.0/24
</Location>
```

---

## 9. Logging & Monitoring

### Access Log Format

```apache
# Default combined format
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined

# Custom format
LogFormat "%h %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\" %D" custom

# Parameters:
# %h - Remote host
# %u - Remote user
# %t - Time request was received
# %r - Request line
# %>s - Status code
# %b - Size of response
# %D - Time taken (microseconds)
# %T - Time taken (seconds)

CustomLog ${APACHE_LOG_DIR}/access.log combined
ErrorLog ${APACHE_LOG_DIR}/error.log
```

### Server Status

```apache
# Enable status module
<IfModule mod_status.c>
    <Location /server-status>
        SetHandler server-status
        Require ip 127.0.0.1 192.168.1.0/24
    </Location>
</IfModule>

# Access: http://example.com/server-status
# Real-time: http://example.com/server-status?auto
```

### View Logs

```bash
# Real-time access log
tail -f /var/log/apache2/access.log

# Real-time error log
tail -f /var/log/apache2/error.log

# Count requests
wc -l /var/log/apache2/access.log

# Top 10 IPs
cut -d' ' -f1 /var/log/apache2/access.log | sort | uniq -c | sort -rn | head -10

# Top 10 URLs
cut -d' ' -f7 /var/log/apache2/access.log | sort | uniq -c | sort -rn | head -10

# HTTP status codes
cut -d' ' -f9 /var/log/apache2/access.log | sort | uniq -c

# Errors in last hour
grep "$(date -d '1 hour ago' '+%d/%b/%Y:%H')" /var/log/apache2/error.log
```

---

## 10. Best Practices

### Security Headers

```apache
# Prevent clickjacking
Header set X-Frame-Options "SAMEORIGIN"

# Prevent MIME type sniffing
Header set X-Content-Type-Options "nosniff"

# Enable XSS filter
Header set X-XSS-Protection "1; mode=block"

# HSTS for HTTPS
Header set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"

# Content Security Policy
Header set Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'"

# Referrer policy
Header set Referrer-Policy "strict-origin-when-cross-origin"

# Permissions policy
Header set Permissions-Policy "geolocation=(), microphone=(), camera=()"
```

### Performance Optimization

```apache
# Enable compression
<IfModule mod_deflate.c>
    AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript
</IfModule>

# Set caching headers
<IfModule mod_expires.c>
    ExpiresActive On
    ExpiresByType text/html "access plus 1 day"
    ExpiresByType text/css "access plus 1 year"
    ExpiresByType application/javascript "access plus 1 year"
    ExpiresByType image/jpeg "access plus 1 year"
</IfModule>

# HTTP/2 push
<IfModule mod_http2.c>
    Protocols h2 h1
    H2Push on
    H2PushResource /css/style.css
    H2PushResource /js/script.js
</IfModule>

# Keep-alive
KeepAlive On
KeepAliveTimeout 5
```

### Resource Limits

```apache
# Limit request size
LimitRequestBody 10485760  # 10MB

# Timeout
Timeout 300

# Keep-alive connections
MaxKeepAliveRequests 100
KeepAliveTimeout 5

# Connection per IP
<Limit GET POST PUT DELETE>
    Order Allow,Deny
    Allow from all
</Limit>
```

---

## 11. Troubleshooting

### Common Issues

```bash
# Issue: Port already in use
# Solution: Check process using port
sudo lsof -i :80
sudo netstat -tulpn | grep :80

# Issue: Configuration error
# Solution: Test configuration
sudo apache2ctl configtest
sudo apachectl configtest

# Issue: Virtual host not responding
# Solution: Verify enabled
sudo a2ensite example.com
sudo apache2ctl configtest
sudo systemctl reload apache2

# Issue: Permission denied
# Solution: Check file permissions
ls -la /var/www/html
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html

# Issue: SSL certificate error
# Solution: Check certificate
openssl x509 -in /etc/ssl/certs/example.com.crt -text -noout

# Issue: Module not loading
# Solution: Enable module
sudo a2enmod module_name
sudo apache2ctl configtest
sudo systemctl restart apache2
```

### Debug Mode

```bash
# Enable debug logging
sudo sed -i 's/LogLevel warn/LogLevel debug/' /etc/apache2/apache2.conf

# View debug logs
tail -f /var/log/apache2/error.log

# Check loaded modules
apache2ctl -M

# List configuration
apache2ctl -t -D DUMP_CONFIG

# Trace execution
apache2ctl -t -D DUMP_CONFIG | grep -i "virtualhost"
```

### Performance Debugging

```bash
# Check connections
netstat -an | grep ESTABLISHED | wc -l

# Check Apache processes
ps aux | grep apache2

# Monitor in real-time
watch 'ps aux | grep apache2 | wc -l'

# Check system resources
top -p $(pgrep -d',' apache2)

# Analyze slow requests
tail -f /var/log/apache2/access.log | grep -E "[0-9]{6,}"  # requests > 100ms
```

---

## 12. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Apache Docs** | https://httpd.apache.org/docs/ |
| **Configuration** | https://httpd.apache.org/docs/2.4/configuration/ |
| **Modules** | https://httpd.apache.org/docs/2.4/mod/ |
| **FAQ** | https://httpd.apache.org/docs/2.4/faq/ |
| **SSL Guide** | https://httpd.apache.org/docs/2.4/ssl/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Official Site** | https://httpd.apache.org/ |
| **Getting Started** | https://httpd.apache.org/docs/2.4/getting-started.html |
| **Community** | https://httpd.apache.org/lists.html |
| **GitHub** | https://github.com/apache/httpd |

---

## Quick Reference

```bash
# Start/Stop/Restart
sudo systemctl start apache2
sudo systemctl stop apache2
sudo systemctl restart apache2
sudo systemctl reload apache2

# Check status
sudo systemctl status apache2
apache2ctl fullstatus

# Verify configuration
sudo apache2ctl configtest
sudo apache2ctl -t -D DUMP_VHOSTS

# Enable/Disable modules
sudo a2enmod module_name
sudo a2dismod module_name

# List modules
apache2ctl -M

# Enable/Disable sites
sudo a2ensite site_name
sudo a2dissite site_name

# Reload logs
sudo systemctl reload apache2

# View logs
tail -f /var/log/apache2/access.log
tail -f /var/log/apache2/error.log

# Check listening ports
sudo netstat -tulpn | grep apache
sudo lsof -i :80

# Generate certificate
sudo openssl req -x509 -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/server.key \
  -out /etc/ssl/certs/server.crt -nodes

# Docker
docker run -d -p 80:80 -p 443:443 httpd:latest
docker logs -f apache

# Compose
docker-compose up -d
docker-compose down

# Test virtual host
curl -H "Host: example.com" http://localhost

# Check SSL certificate
openssl s_client -connect example.com:443
```

---

## File Locations

| Item | Path |
|------|------|
| **Main Config** | /etc/apache2/apache2.conf |
| **Mods Available** | /etc/apache2/mods-available/ |
| **Mods Enabled** | /etc/apache2/mods-enabled/ |
| **Sites Available** | /etc/apache2/sites-available/ |
| **Sites Enabled** | /etc/apache2/sites-enabled/ |
| **Conf Available** | /etc/apache2/conf-available/ |
| **Conf Enabled** | /etc/apache2/conf-enabled/ |
| **Document Root** | /var/www/html |
| **Access Log** | /var/log/apache2/access.log |
| **Error Log** | /var/log/apache2/error.log |
| **SSL Certs** | /etc/ssl/certs/ |
| **SSL Keys** | /etc/ssl/private/ |

---

**Last Updated:** 2026
**Apache Version:** 2.4.x+
**License:** Apache License 2.0

For latest information, visit [Apache HTTP Server Official Site](https://httpd.apache.org/)