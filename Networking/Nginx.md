# Nginx Cheatsheet

![Nginx Logo](https://nginx.org/nginx.png)

---

## Table of Contents
1. [What is Nginx?](#1-what-is-nginx)
2. [Core Concepts](#2-core-concepts)
3. [Installation & Setup](#3-installation--setup)
4. [Configuration](#4-configuration)
5. [Server Blocks (Virtual Hosts)](#5-server-blocks-virtual-hosts)
6. [Reverse Proxy & Load Balancing](#6-reverse-proxy--load-balancing)
7. [SSL/TLS Configuration](#7-ssltls-configuration)
8. [Performance Optimization](#8-performance-optimization)
9. [Logging & Monitoring](#9-logging--monitoring)
10. [Best Practices](#10-best-practices)
11. [Troubleshooting](#11-troubleshooting)
12. [Additional Resources](#12-additional-resources)

---

## 1. What is Nginx?

Nginx is a lightweight, high-performance web server and reverse proxy that efficiently handles thousands of concurrent connections. It's event-driven architecture makes it ideal for modern web applications, API gateways, and load balancing scenarios.

**Key Benefits:**
- Lightweight and fast
- Event-driven architecture
- Low memory footprint
- High concurrent connections
- Reverse proxy capabilities
- Load balancing
- URL rewriting
- Caching support
- SSL/TLS support
- Easy configuration

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Server Block** | Virtual host configuration (like VirtualHost in Apache) |
| **Upstream** | Group of backend servers for load balancing |
| **Location Block** | URL-based configuration directives |
| **Directive** | Configuration instruction |
| **Worker Process** | Thread handling requests |
| **Master Process** | Main Nginx process |
| **Context** | Scope where directive applies |
| **Proxy** | Forward requests to backend servers |
| **Cache** | Store responses for faster delivery |
| **Rate Limiting** | Restrict request frequency |

---

## 3. Installation & Setup

### Linux Installation (Ubuntu/Debian)

```bash
# Update system
sudo apt-get update
sudo apt-get upgrade -y

# Install Nginx
sudo apt-get install -y nginx

# Start service
sudo systemctl start nginx
sudo systemctl enable nginx

# Verify installation
sudo systemctl status nginx
nginx -v
```

### macOS Installation

```bash
# Using Homebrew
brew install nginx

# Start service
brew services start nginx

# Verify
brew services list | grep nginx
nginx -v
```

### Linux Installation (RedHat/CentOS)

```bash
# Install Nginx
sudo yum install -y nginx

# Start service
sudo systemctl start nginx
sudo systemctl enable nginx

# Verify
sudo systemctl status nginx
nginx -v
```

### Docker Installation

```bash
# Run Nginx container
docker run -d \
  --name nginx \
  -p 80:80 \
  -p 443:443 \
  -v /etc/nginx:/etc/nginx \
  -v /var/www/html:/usr/share/nginx/html \
  nginx:latest

# Verify
docker logs nginx
curl http://localhost
```

### Docker Compose Setup

```yaml
version: '3.8'

services:
  nginx:
    image: nginx:latest
    container_name: nginx
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./conf.d:/etc/nginx/conf.d
      - ./html:/usr/share/nginx/html
      - ./logs:/var/log/nginx
      - ./certs:/etc/nginx/certs
    networks:
      - web

  app:
    image: myapp:latest
    container_name: app
    ports:
      - "8080:8080"
    networks:
      - web

volumes:
  nginx_logs:

networks:
  web:
```

### Important Ports

| Service | Port | Purpose |
|---------|------|---------|
| **HTTP** | 80 | Web traffic |
| **HTTPS** | 443 | Secure web traffic |
| **Backend** | 8080 | Application server |
| **Metrics** | 8888 | Nginx metrics endpoint |

---

## 4. Configuration

### Main Configuration File

```bash
# Location
/etc/nginx/nginx.conf

# Test configuration
sudo nginx -t

# Reload configuration
sudo systemctl reload nginx
sudo nginx -s reload

# Restart service
sudo systemctl restart nginx
```

### Basic nginx.conf

```nginx
# Main context
user www-data;
worker_processes auto;           # Auto-detect CPU cores
pid /run/nginx.pid;
error_log /var/log/nginx/error.log warn;

# Events context
events {
    worker_connections 1024;     # Max connections per worker
    use epoll;                    # Linux event model
    multi_accept on;
}

# HTTP context
http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    # Logging format
    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';

    access_log /var/log/nginx/access.log main;

    # Performance settings
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;
    server_tokens off;

    # Gzip compression
    gzip on;
    gzip_vary on;
    gzip_types text/plain text/css text/xml text/javascript 
               application/x-javascript application/xml+rss 
               application/javascript application/json;

    # Include server blocks
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

### Server Block Configuration

```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name _;
    root /var/www/html;
    index index.html index.htm;

    # Access and error logs
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    # Basic routing
    location / {
        try_files $uri $uri/ =404;
    }

    # Return error page
    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;
}
```

### Location Blocks

```nginx
server {
    server_name example.com;

    # Exact match (highest priority)
    location = / {
        proxy_pass http://backend;
    }

    # Prefix match
    location /api/ {
        proxy_pass http://api_backend;
    }

    # Regex match (case-sensitive)
    location ~ \.php$ {
        fastcgi_pass 127.0.0.1:9000;
    }

    # Regex match (case-insensitive)
    location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
        expires 30d;
        add_header Cache-Control "public, immutable";
    }

    # Priority order:
    # 1. = (exact)
    # 2. ^~ (prefix, stops regex)
    # 3. ~ (regex)
    # 4. ~* (regex case-insensitive)
    # 5. / (default)
}
```

---

## 5. Server Blocks (Virtual Hosts)

### Single Domain Server Block

```nginx
# /etc/nginx/sites-available/example.com

server {
    listen 80;
    listen [::]:80;

    server_name example.com www.example.com;
    root /var/www/example.com;
    index index.html index.php;

    # Logs
    access_log /var/log/nginx/example.com-access.log;
    error_log /var/log/nginx/example.com-error.log;

    # Redirect HTTP to HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    server_name example.com www.example.com;
    root /var/www/example.com;
    index index.html index.php;

    # SSL certificates
    ssl_certificate /etc/ssl/certs/example.com.crt;
    ssl_certificate_key /etc/ssl/private/example.com.key;

    # Logs
    access_log /var/log/nginx/example.com-access.log;
    error_log /var/log/nginx/example.com-error.log;

    # Security headers
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-Frame-Options "DENY" always;
    add_header X-XSS-Protection "1; mode=block" always;

    # Location blocks
    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        include fastcgi_params;
    }
}
```

### Enable/Disable Sites

```bash
# Enable site
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/example.com

# Disable site
sudo rm /etc/nginx/sites-enabled/example.com

# Test configuration
sudo nginx -t

# Reload Nginx
sudo systemctl reload nginx
```

### Multiple Server Blocks

```nginx
# Site 1
server {
    server_name site1.com www.site1.com;
    root /var/www/site1;
    listen 80;
    listen 443 ssl;
    ssl_certificate /etc/ssl/certs/site1.crt;
    ssl_certificate_key /etc/ssl/private/site1.key;
}

# Site 2
server {
    server_name site2.com www.site2.com;
    root /var/www/site2;
    listen 80;
    listen 443 ssl;
    ssl_certificate /etc/ssl/certs/site2.crt;
    ssl_certificate_key /etc/ssl/private/site2.key;
}

# Catch-all (default)
server {
    listen 80 default_server;
    server_name _;
    return 444;  # Close connection
}
```

---

## 6. Reverse Proxy & Load Balancing

### Basic Reverse Proxy

```nginx
server {
    server_name example.com;

    location / {
        proxy_pass http://backend-server:8080;
        
        # Forward headers
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # Timeouts
        proxy_connect_timeout 60s;
        proxy_send_timeout 60s;
        proxy_read_timeout 60s;
    }
}
```

### HTTPS to HTTP Backend

```nginx
server {
    listen 443 ssl http2;
    server_name example.com;

    ssl_certificate /etc/ssl/certs/example.com.crt;
    ssl_certificate_key /etc/ssl/private/example.com.key;

    location / {
        proxy_pass http://backend-server:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
    }
}
```

### Upstream Load Balancing

```nginx
# Define upstream server pool
upstream backend {
    server backend1:8080 weight=5;
    server backend2:8080 weight=3;
    server backend3:8080 weight=2;
    
    # Load balancing method (default: round-robin)
    # least_conn;
    # ip_hash;
    # random;
    # least_time last_byte;
}

server {
    server_name example.com;

    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

### Load Balancing Methods

```nginx
# Round-robin (default)
upstream backend {
    server backend1:8080;
    server backend2:8080;
}

# Least connections
upstream backend {
    least_conn;
    server backend1:8080;
    server backend2:8080;
}

# IP hash (sticky sessions)
upstream backend {
    ip_hash;
    server backend1:8080;
    server backend2:8080;
}

# Random
upstream backend {
    random;
    server backend1:8080;
    server backend2:8080;
}

# Least time (fastest)
upstream backend {
    least_time last_byte;
    server backend1:8080;
    server backend2:8080;
}
```

### Health Checks

```nginx
upstream backend {
    server backend1:8080 max_fails=3 fail_timeout=30s;
    server backend2:8080 max_fails=3 fail_timeout=30s;
    server backend3:8080 backup;  # Fallback server
    
    keepalive 32;
}

server {
    location / {
        proxy_pass http://backend;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
    }
}
```

### Connection Pooling

```nginx
upstream backend {
    server backend1:8080;
    server backend2:8080;
    
    keepalive 32;
    keepalive_requests 100;
    keepalive_timeout 60s;
}

server {
    location / {
        proxy_pass http://backend;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        proxy_set_header Keep-Alive "timeout=5";
    }
}
```

---

## 7. SSL/TLS Configuration

### Generate Self-Signed Certificate

```bash
# Create self-signed certificate
sudo openssl req -x509 -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/server.key \
  -out /etc/ssl/certs/server.crt \
  -nodes

# Verify certificate
openssl x509 -in /etc/ssl/certs/server.crt -text -noout
```

### Let's Encrypt with Certbot

```bash
# Install Certbot
sudo apt-get install -y certbot python3-certbot-nginx

# Generate certificate
sudo certbot certonly --nginx -d example.com -d www.example.com

# Auto-renewal
sudo systemctl enable certbot.timer
sudo systemctl start certbot.timer

# Verify renewal
sudo certbot renew --dry-run
```

### SSL Configuration

```nginx
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name example.com;

    # Certificate files
    ssl_certificate /etc/ssl/certs/example.com.crt;
    ssl_certificate_key /etc/ssl/private/example.com.key;
    ssl_trusted_certificate /etc/ssl/certs/chain.crt;

    # SSL protocols (disable old versions)
    ssl_protocols TLSv1.2 TLSv1.3;

    # Cipher suites
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    # Performance
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    ssl_session_tickets off;

    # OCSP stapling
    ssl_stapling on;
    ssl_stapling_verify on;
    resolver 8.8.8.8 8.8.4.4;

    # Security headers
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-Frame-Options "DENY" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;

    location / {
        proxy_pass http://backend;
    }
}

# Redirect HTTP to HTTPS
server {
    listen 80;
    listen [::]:80;
    server_name example.com www.example.com;
    return 301 https://$server_name$request_uri;
}
```

---

## 8. Performance Optimization

### Compression

```nginx
# Gzip compression
gzip on;
gzip_vary on;
gzip_proxied any;
gzip_comp_level 6;
gzip_types text/plain text/css text/xml text/javascript 
            application/json application/javascript application/xml+rss;
gzip_disable "msie6";
gzip_min_length 1000;

# Brotli compression (if compiled)
# brotli on;
# brotli_comp_level 6;
# brotli_types text/plain text/css text/xml text/javascript;
```

### Caching

```nginx
# Browser cache
location ~* \.(jpg|jpeg|png|gif|ico|css|js|woff|woff2)$ {
    expires 1y;
    add_header Cache-Control "public, immutable";
    add_header ETag "$file_mtime-$file_size";
}

# Cache HTML differently
location ~ \.html?$ {
    expires 7d;
    add_header Cache-Control "public, must-revalidate";
}

# Proxy cache
proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=my_cache:10m max_size=100m inactive=60m;

server {
    location / {
        proxy_cache my_cache;
        proxy_cache_valid 200 60m;
        proxy_cache_valid 404 5m;
        proxy_cache_use_stale error timeout invalid_header updating;
        add_header X-Cache-Status $upstream_cache_status;
    }
}
```

### Rate Limiting

```nginx
# Define rate limit zone
limit_req_zone $binary_remote_addr zone=general:10m rate=10r/s;
limit_req_zone $binary_remote_addr zone=api:10m rate=5r/s;
limit_req_status 429;

server {
    # Apply rate limit
    location / {
        limit_req zone=general burst=20 nodelay;
        proxy_pass http://backend;
    }

    location /api/ {
        limit_req zone=api burst=10 nodelay;
        proxy_pass http://api_backend;
    }
}
```

### Connection Limits

```nginx
# Limit connections per IP
limit_conn_zone $binary_remote_addr zone=addr:10m;

server {
    location / {
        limit_conn addr 10;
        proxy_pass http://backend;
    }
}
```

### Performance Tuning

```nginx
# Worker processes
worker_processes auto;

# Worker connections
events {
    worker_connections 4096;
    use epoll;
    multi_accept on;
}

# Keep-alive
keepalive_timeout 65;
keepalive_requests 100;

# TCP optimization
tcp_nopush on;
tcp_nodelay on;

# Buffering
client_body_buffer_size 128k;
client_max_body_size 10m;
proxy_buffer_size 128k;
proxy_buffers 4 256k;
proxy_busy_buffers_size 256k;
```

---

## 9. Logging & Monitoring

### Custom Log Format

```nginx
# Define custom format
log_format main '$remote_addr - $remote_user [$time_local] '
                '"$request" $status $body_bytes_sent '
                '"$http_referer" "$http_user_agent"';

log_format detailed '$remote_addr - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent" '
                    'rt=$request_time uct="$upstream_connect_time" '
                    'uht="$upstream_header_time" urt="$upstream_response_time"';

# Use format
access_log /var/log/nginx/access.log main;
```

### Server Status

```nginx
server {
    listen 8888;
    server_name localhost;

    location /nginx_status {
        stub_status on;
        access_log off;
        allow 127.0.0.1;
        allow 192.168.1.0/24;
        deny all;
    }
}

# Access: http://localhost:8888/nginx_status
```

### View Logs

```bash
# Real-time access log
tail -f /var/log/nginx/access.log

# Real-time error log
tail -f /var/log/nginx/error.log

# Count requests
wc -l /var/log/nginx/access.log

# Top 10 IPs
cut -d' ' -f1 /var/log/nginx/access.log | sort | uniq -c | sort -rn | head -10

# Top 10 URLs
cut -d' ' -f7 /var/log/nginx/access.log | sort | uniq -c | sort -rn | head -10

# HTTP status codes
cut -d' ' -f9 /var/log/nginx/access.log | sort | uniq -c | sort -rn

# Response times
grep -oP 'rt=\K[^ ]+' /var/log/nginx/access.log | sort -rn | head -10

# Errors in last hour
grep "$(date -d '1 hour ago' '+%d/%b/%Y:%H')" /var/log/nginx/error.log
```

---

## 10. Best Practices

### Security

```nginx
# Hide version
server_tokens off;

# Security headers
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-Frame-Options "DENY" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;

# Disable dangerous methods
if ($request_method !~ ^(GET|HEAD|POST|PUT|DELETE|OPTIONS)$) {
    return 405;
}

# Block common attacks
location ~ ^/(admin|wp-admin|wp-login) {
    deny all;
}

location ~ /\.env {
    deny all;
}
```

### URL Rewriting

```nginx
# Remove .html extension
location / {
    try_files $uri $uri/ $uri.html =404;
}

# Rewrite rules
rewrite ^/old-page$ /new-page permanent;
rewrite ^/user/([0-9]+)$ /profile.php?id=$1 last;

# Trailing slash
rewrite ^([^.]*[^/])$ $1/ permanent;
```

### Conditional Logic

```nginx
# Conditional based on request
if ($request_method = POST) {
    return 405;
}

if ($request_uri ~ "^/admin") {
    return 403;
}

# Set variables
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}

server {
    location / {
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
    }
}
```

### Configuration Organization

```bash
# Organized directory structure
/etc/nginx/
├── nginx.conf
├── mime.types
├── conf.d/
│   ├── upstream.conf
│   ├── rate-limit.conf
│   └── security.conf
├── sites-available/
│   ├── example.com
│   ├── site2.com
│   └── default
├── sites-enabled/
│   ├── example.com -> ../sites-available/example.com
│   └── site2.com -> ../sites-available/site2.com
└── ssl/
    ├── example.com.crt
    └── example.com.key
```

---

## 11. Troubleshooting

### Configuration Issues

```bash
# Test configuration
sudo nginx -t

# Verbose syntax check
sudo nginx -T

# Check specific file
sudo nginx -c /etc/nginx/nginx.conf -t

# View parsed configuration
sudo nginx -T | less
```

### Common Issues

```bash
# Issue: Port already in use
# Solution: Check listening ports
sudo lsof -i :80
sudo netstat -tulpn | grep :80

# Issue: Site not accessible
# Solution: Enable site
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

# Issue: Permission denied
# Solution: Check file permissions
ls -la /var/www/html
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html

# Issue: SSL certificate error
# Solution: Check certificate
sudo openssl x509 -in /etc/ssl/certs/example.com.crt -text -noout

# Issue: Upstream timeout
# Solution: Increase timeout
proxy_connect_timeout 60s;
proxy_send_timeout 60s;
proxy_read_timeout 60s;
```

### Debug Mode

```bash
# Increase error log level
error_log /var/log/nginx/error.log debug;

# Check logs
tail -f /var/log/nginx/error.log

# Restart Nginx
sudo systemctl restart nginx

# Monitor processes
ps aux | grep nginx
watch 'ps aux | grep nginx | wc -l'

# Check connections
ss -ant | grep :80 | wc -l
netstat -an | grep ESTABLISHED | wc -l
```

### Performance Issues

```bash
# Check system resources
top -p $(pgrep -d',' nginx)
free -h
df -h

# Monitor Nginx status
curl http://localhost:8888/nginx_status

# Check slow upstream
grep -oP 'urt=\K[^ ]+' /var/log/nginx/access.log | sort -rn | head -10

# Identify slow requests
tail -f /var/log/nginx/access.log | grep -E '[1-9][0-9]{2}\.[0-9]'
```

---

## 12. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **Nginx Docs** | https://nginx.org/en/docs/ |
| **Directives** | https://nginx.org/en/docs/dirindex.html |
| **FAQ** | https://nginx.org/en/docs/faq.html |
| **Admin Guide** | https://docs.nginx.com/nginx/admin-guide/ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Official Site** | https://nginx.org/ |
| **Getting Started** | https://nginx.org/en/docs/beginners_guide.html |
| **Community** | https://community.nginx.org/ |
| **GitHub** | https://github.com/nginx/nginx |

---

## Quick Reference

```bash
# Start/Stop/Reload
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl reload nginx

# Check status
sudo systemctl status nginx
nginx -v

# Test configuration
sudo nginx -t
sudo nginx -T

# View running processes
ps aux | grep nginx

# Check listening ports
sudo netstat -tulpn | grep nginx
sudo lsof -i :80

# View logs
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log

# Enable/disable site
sudo ln -s /etc/nginx/sites-available/site /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/site
sudo nginx -t && sudo systemctl reload nginx

# Generate certificate
sudo openssl req -x509 -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/server.key \
  -out /etc/ssl/certs/server.crt -nodes

# Certbot
sudo certbot certonly --nginx -d example.com
sudo certbot renew --dry-run

# Docker
docker run -d -p 80:80 -p 443:443 nginx:latest
docker logs -f nginx
docker exec nginx nginx -t

# Compose
docker-compose up -d
docker-compose down

# Test virtual host
curl -H "Host: example.com" http://localhost

# Check SSL
openssl s_client -connect example.com:443

# Monitor status
curl http://localhost:8888/nginx_status
```

---

## File Locations

| Item | Path |
|------|------|
| **Main Config** | /etc/nginx/nginx.conf |
| **Sites Available** | /etc/nginx/sites-available/ |
| **Sites Enabled** | /etc/nginx/sites-enabled/ |
| **Config.d** | /etc/nginx/conf.d/ |
| **MIME Types** | /etc/nginx/mime.types |
| **Document Root** | /var/www/html |
| **Access Log** | /var/log/nginx/access.log |
| **Error Log** | /var/log/nginx/error.log |
| **PID** | /var/run/nginx.pid |
| **Cache** | /var/cache/nginx/ |
| **SSL Certs** | /etc/ssl/certs/ |
| **SSL Keys** | /etc/ssl/private/ |

---

**Last Updated:** 2026
**Nginx Version:** 1.24.x+
**License:** 2-clause BSD License

For latest information, visit [Nginx Official Site](https://nginx.org/)