# SSH Cheatsheet

![SSH Logo](https://www.openssh.com/images/openssh.gif)

---

## Table of Contents
1. [What is SSH?](#1-what-is-ssh)
2. [Core Concepts](#2-core-concepts)
3. [Installation & Setup](#3-installation--setup)
4. [Basic SSH Connection](#4-basic-ssh-connection)
5. [SSH Keys & Authentication](#5-ssh-keys--authentication)
6. [SSH Configuration](#6-ssh-configuration)
7. [File Transfer (SCP/SFTP)](#7-file-transfer-scpsftp)
8. [Advanced Features](#8-advanced-features)
9. [Security Best Practices](#9-security-best-practices)
10. [Troubleshooting](#10-troubleshooting)
11. [Additional Resources](#11-additional-resources)

---

## 1. What is SSH?

SSH (Secure Shell) is a cryptographic network protocol for secure remote access to servers. It encrypts all data transmitted between client and server, making it safe for authentication and file transfer over untrusted networks.

**Key Benefits:**
- Encrypted communication
- Secure authentication
- Remote command execution
- File transfer (SCP/SFTP)
- Port forwarding
- No plaintext passwords over network
- Industry standard
- Works on all major platforms

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **SSH Client** | User's machine connecting to server |
| **SSH Server** | Remote machine accepting connections |
| **Public Key** | Shared key for encryption/verification |
| **Private Key** | Secret key for decryption/signing |
| **Host Key** | Server's public key identifying itself |
| **Known Hosts** | File storing trusted server keys |
| **Port** | Network port (default: 22) |
| **Cipher** | Encryption algorithm (AES, ChaCha20) |
| **Key Fingerprint** | Unique identifier for public key |
| **Passphrase** | Password protecting private key |

---

## 3. Installation & Setup

### Linux Installation

```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install -y openssh-client openssh-server

# Start SSH server
sudo systemctl start ssh
sudo systemctl enable ssh
sudo systemctl status ssh

# Check SSH listening
sudo ss -tlnp | grep ssh
sudo netstat -tlnp | grep ssh

# CentOS/RedHat
sudo yum install -y openssh-clients openssh-server openssh-askpass

# Start service
sudo systemctl start sshd
sudo systemctl enable sshd
```

### macOS Installation

```bash
# macOS comes with SSH pre-installed
ssh -V                    # Check version

# Enable SSH server
sudo systemsetup -setremotelogin on

# Disable SSH server
sudo systemsetup -setremotelogin off
```

### Windows Installation

```bash
# Option 1: Windows 10/11 built-in
# Settings → System → Optional features → OpenSSH Client

# Option 2: PuTTY
# Download from https://www.putty.org/

# Option 3: Git Bash / WSL
# Already includes SSH client
```

### SSH Configuration File

```bash
# Location
~/.ssh/config                    # Client config (user-specific)
/etc/ssh/sshd_config             # Server config (system-wide)

# SSH directory permissions (important!)
mkdir -p ~/.ssh
chmod 700 ~/.ssh                 # rwx------
chmod 600 ~/.ssh/config          # rw-------
chmod 600 ~/.ssh/authorized_keys # rw-------
chmod 600 ~/.ssh/id_rsa          # rw------- (private key)
chmod 644 ~/.ssh/id_rsa.pub      # rw-r--r-- (public key)
```

### Important Port

| Service | Port | Protocol |
|---------|------|----------|
| **SSH** | 22 | TCP |
| **SSH Alternate** | 2222 | TCP |
| **SSH Secure** | 22/TCP | With key auth |

---

## 4. Basic SSH Connection

### Simple Connection

```bash
# Basic SSH connection
ssh username@hostname
ssh username@192.168.1.10

# With custom port
ssh -p 2222 username@hostname
ssh -p 2222 username@hostname -l username  # Alternative

# Connect to different user
ssh -l different_user hostname

# Run single command
ssh username@hostname "ls -la"
ssh username@hostname "pwd && whoami"

# Run with output
ssh username@hostname 'echo "Hello from remote"'

# Interactive shell
ssh username@hostname
# Then type commands normally
# Type 'exit' to disconnect
```

### Typical SSH Session

```bash
$ ssh user@example.com
The authenticity of host 'example.com (192.168.1.1)' can't be established.
ECDSA key fingerprint is SHA256:abcd1234...
Are you sure you want to continue connecting (yes/no)? yes

user@example.com's password: [type password]

# Now connected to remote system
remote$ ls -la
remote$ pwd
remote$ exit
# Disconnected
```

### Connection Options

```bash
# X11 forwarding (GUI applications)
ssh -X username@hostname
ssh -Y username@hostname              # Trust X11

# Verbose mode (debugging)
ssh -v username@hostname
ssh -vv username@hostname             # Very verbose
ssh -vvv username@hostname            # Most verbose

# Compression
ssh -C username@hostname              # Enable compression

# IPv4 only
ssh -4 username@hostname

# IPv6 only
ssh -6 username@hostname

# No terminal allocation
ssh -T username@hostname              # Non-interactive

# Multiple commands
ssh user@host 'cd /path && ./script.sh && ls'
```

---

## 5. SSH Keys & Authentication

### Generate SSH Keys

```bash
# Generate RSA key pair (most compatible)
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa

# Generate ED25519 key (modern, fast, secure)
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519

# Generate with comment
ssh-keygen -t ed25519 -C "user@example.com" -f ~/.ssh/id_ed25519

# Interactive generation (detailed)
ssh-keygen
# Prompts for:
# 1. File location (~/.ssh/id_rsa)
# 2. Passphrase (optional password)

# Key options:
# -t: Key type (rsa, ed25519, ecdsa)
# -b: Key bits (4096 for RSA, 256 for ED25519)
# -f: File path
# -C: Comment/identifier
# -N: Passphrase (non-interactive)
```

### Verify Generated Keys

```bash
# Check key files
ls -la ~/.ssh/

# View public key
cat ~/.ssh/id_rsa.pub

# View key fingerprint
ssh-keygen -l -f ~/.ssh/id_rsa

# View key fingerprint (hash)
ssh-keygen -l -f ~/.ssh/id_rsa.pub

# View key details
ssh-keygen -y -f ~/.ssh/id_rsa      # Convert private to public
```

### Add Public Key to Server

```bash
# Method 1: Using ssh-copy-id (recommended)
ssh-copy-id -i ~/.ssh/id_rsa.pub user@hostname
ssh-copy-id -i ~/.ssh/id_rsa.pub -p 2222 user@hostname

# Method 2: Manual copy
cat ~/.ssh/id_rsa.pub | ssh user@hostname "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

# Method 3: Manual edit (if ssh already configured)
ssh user@hostname
# On remote:
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
# Paste public key content
# Save and exit

# Verify permissions on remote
# ~/.ssh directory: 700 (rwx------)
# ~/.ssh/authorized_keys: 600 (rw-------)
```

### Test Key-Based Authentication

```bash
# SSH with explicit key file
ssh -i ~/.ssh/id_rsa user@hostname

# SSH without prompt (if key has no passphrase)
ssh -i ~/.ssh/id_ed25519 user@hostname

# Test connection
ssh -i ~/.ssh/id_rsa -T git@github.com      # GitHub test
ssh -i ~/.ssh/id_rsa -T user@hostname 'echo success'

# Once configured, just use:
ssh user@hostname                           # Default keys tried
```

### Passphrase Management

```bash
# Add passphrase to existing key
ssh-keygen -p -f ~/.ssh/id_rsa

# Remove passphrase
ssh-keygen -p -f ~/.ssh/id_rsa
# Enter old passphrase
# Leave new passphrase blank

# SSH agent (avoid typing passphrase repeatedly)
eval $(ssh-agent -s)              # Start agent
ssh-add ~/.ssh/id_rsa             # Add key to agent
ssh-add ~/.ssh/id_ed25519         # Add another key
ssh-add -l                         # List loaded keys
ssh-add -D                         # Delete all keys from agent

# Auto-add keys on login (add to ~/.bashrc)
if [ -z "$SSH_AUTH_SOCK" ]; then
    eval $(ssh-agent -s)
    ssh-add ~/.ssh/id_rsa
fi
```

### Multiple SSH Keys

```bash
# Generate multiple keys
ssh-keygen -t ed25519 -f ~/.ssh/github_key -C "github@example.com"
ssh-keygen -t ed25519 -f ~/.ssh/work_key -C "work@example.com"

# Configure ~/.ssh/config to use specific keys
Host github.com
    IdentityFile ~/.ssh/github_key
    User git

Host work.company.com
    IdentityFile ~/.ssh/work_key
    User username

# Then connections automatically use correct key
ssh git@github.com
ssh user@work.company.com
```

---

## 6. SSH Configuration

### Client Config (~/.ssh/config)

```bash
# Create/edit config file
nano ~/.ssh/config

# Basic example
Host example
    HostName example.com
    User username
    Port 22
    IdentityFile ~/.ssh/id_rsa

# Multiple hosts
Host github
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_key

Host work
    HostName work.company.com
    User workuser
    Port 2222
    IdentityFile ~/.ssh/work_key
    ForwardAgent yes
    ForwardX11 yes

Host *
    AddKeysToAgent yes
    IdentitiesOnly yes
    StrictHostKeyChecking accept-new
    UserKnownHostsFile ~/.ssh/known_hosts
    ControlMaster auto
    ControlPath ~/.ssh/control-%h-%p-%r
    ControlPersist 600
```

### Usage with Config

```bash
# Instead of long command
ssh -i ~/.ssh/id_rsa -p 2222 workuser@work.company.com

# Just use alias
ssh work

# List defined hosts
grep "^Host " ~/.ssh/config

# Test SSH connection
ssh -v work               # Verbose to see which config used
```

### Server Config (sshd_config)

```bash
# Location
/etc/ssh/sshd_config

# Important settings
Port 22                                  # SSH port
AddressFamily inet                       # IPv4 only
ListenAddress 0.0.0.0                   # Listen all IPs
ListenAddress ::                         # IPv6

# Authentication
PermitRootLogin no                       # Disable root login
PubkeyAuthentication yes                 # Enable key auth
PasswordAuthentication no                # Disable password auth
PermitEmptyPasswords no                  # No empty passwords

# Security
StrictModes yes                          # Check file permissions
MaxAuthTries 3                           # Max auth attempts
MaxSessions 10                           # Max sessions per connection

# Logging
SyslogFacility AUTH
LogLevel INFO

# X11 forwarding
X11Forwarding no                         # Disable if not needed
X11DisplayOffset 10

# Banner
Banner /etc/ssh/banner                   # Login banner

# Subsystem
Subsystem sftp /usr/lib/openssh/sftp-server

# Reload after changes
sudo systemctl reload ssh
# Or
sudo sshd -t                             # Test config
sudo systemctl restart ssh               # Restart if OK
```

---

## 7. File Transfer (SCP/SFTP)

### SCP (Secure Copy)

```bash
# Copy file to remote
scp local_file.txt user@hostname:/path/to/

# Copy file from remote
scp user@hostname:/path/to/file.txt ./

# Copy with custom port
scp -P 2222 file.txt user@hostname:/path/

# Copy directory recursively
scp -r local_dir/ user@hostname:/path/to/

# Copy from remote to local recursively
scp -r user@hostname:/remote/dir ./local_dir/

# Multiple files
scp file1.txt file2.txt user@hostname:/path/

# Preserve permissions/timestamps
scp -p file.txt user@hostname:/path/

# Verbose mode
scp -v file.txt user@hostname:/path/
```

### SFTP (Secure File Transfer Protocol)

```bash
# Connect to SFTP
sftp user@hostname
sftp -P 2222 user@hostname        # Custom port

# SFTP commands (interactive)
ls                                 # List remote files
lls                                # List local files
pwd                                # Remote working directory
lpwd                               # Local working directory

cd /path/on/remote                 # Change remote directory
lcd /path/on/local                 # Change local directory

get remote_file.txt                # Download single file
get remote_file.txt local_name.txt # Download with rename
mget *.txt                         # Download multiple

put local_file.txt                 # Upload single file
put local_file.txt remote_name.txt # Upload with rename
mput *.txt                         # Upload multiple

mkdir new_dir                      # Create remote directory
lmkdir new_dir                     # Create local directory

rm remote_file.txt                 # Delete remote file
rmdir empty_dir                    # Remove empty directory

exit                               # Quit SFTP
```

### Batch File Transfer

```bash
# SCP multiple files efficiently
scp user@host:/path/{file1,file2,file3} ./

# SFTP batch operations
sftp user@host << EOF
cd /remote/path
ls
get file1.txt
get file2.txt
put local_file.txt
quit
EOF

# Rsync over SSH (better for sync)
rsync -avz -e ssh user@host:/remote/path/ ./local/
rsync -avz -e 'ssh -p 2222' user@host:/path/ ./local/
rsync -avz --delete -e ssh user@host:/path/ ./local/
```

---

## 8. Advanced Features

### Port Forwarding (Tunneling)

```bash
# Local port forwarding (access remote service via localhost)
ssh -L 8080:remote-service.com:80 user@bastion-host
# Then access: http://localhost:8080

# Remote port forwarding (expose local service to remote)
ssh -R 8080:localhost:3000 user@remote-host
# Remote can access localhost:8080 to reach local:3000

# Dynamic port forwarding (SOCKS proxy)
ssh -D 1080 user@proxy-host
# Configure browser to use SOCKS5 proxy on localhost:1080

# Multiple forwards
ssh -L 8080:service1:80 -L 9000:service2:3000 user@host
```

### Jump Host / Bastion

```bash
# Connect through jump host
ssh -J jumphost user@target-host

# With custom ports
ssh -J jumphost:2222 user@target-host:2222

# Multiple hops
ssh -J host1,host2 user@final-host

# Configure in ~/.ssh/config
Host jump
    HostName jump.example.com
    User jumpuser

Host target
    HostName target.example.com
    ProxyJump jump
    User targetuser

# Then just use
ssh target
```

### SSH Agent & Key Forwarding

```bash
# Start SSH agent
eval $(ssh-agent -s)

# Add keys
ssh-add ~/.ssh/id_rsa
ssh-add -l                         # List loaded keys

# Forward agent to remote
ssh -A user@host

# This allows remote to use local keys for further connections
# E.g., clone git repos on remote using local keys

# Configure in ~/.ssh/config
Host *
    AddKeysToAgent yes
    ForwardAgent yes
```

### Session Management

```bash
# Keep connection alive
ssh -o ServerAliveInterval=60 user@host

# Increase timeout
ssh -o ConnectTimeout=30 user@host

# Connection multiplexing (reuse connections)
ssh -o ControlMaster=auto \
    -o ControlPath=~/.ssh/control-%h-%p-%r \
    -o ControlPersist=600 user@host

# Add to ~/.ssh/config
Host *
    ControlMaster auto
    ControlPath ~/.ssh/control-%h-%p-%r
    ControlPersist 600
    ServerAliveInterval 60
    ServerAliveCountMax 10
```

---

## 9. Security Best Practices

### Client Security

```bash
# 1. Use key-based authentication (not passwords)
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519
ssh-copy-id user@host

# 2. Protect private keys with passphrase
ssh-keygen -p -f ~/.ssh/id_ed25519
# Enter passphrase when prompted

# 3. Disable password authentication on server
# In /etc/ssh/sshd_config:
PasswordAuthentication no
PubkeyAuthentication yes

# 4. Use SSH config for security defaults
cat >> ~/.ssh/config << EOF
Host *
    AddKeysToAgent yes
    IdentitiesOnly yes
    StrictHostKeyChecking accept-new
    UserKnownHostsFile ~/.ssh/known_hosts
    IdentityFile ~/.ssh/id_ed25519
EOF

# 5. Check known_hosts fingerprints
ssh-keygen -l -f ~/.ssh/known_hosts

# 6. Disable unused authentication methods
# In /etc/ssh/sshd_config:
PubkeyAuthentication yes
PasswordAuthentication no
PermitEmptyPasswords no
```

### Server Security

```bash
# 1. Change default SSH port
sudo nano /etc/ssh/sshd_config
# Port 2222
sudo systemctl reload ssh

# 2. Disable root login
PermitRootLogin no
# Use 'sudo' for elevated privileges

# 3. Limit authentication attempts
MaxAuthTries 3
MaxSessions 10

# 4. Disable X11 forwarding (if not needed)
X11Forwarding no

# 5. Restrict user access
# Allow specific users
AllowUsers user1 user2 user3

# 6. Use strong ciphers/algorithms
KexAlgorithms curve25519-sha256,diffie-hellman-group16-sha512
Ciphers chacha20-poly1305@openssh.com,aes-256-gcm@openssh.com
MACs hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com

# 7. Enable logging
SyslogFacility AUTH
LogLevel INFO

# 8. Banner
Banner /etc/ssh/banner

# 9. Reload configuration
sudo sshd -t                 # Test syntax
sudo systemctl reload ssh    # Apply changes
```

### Key Management

```bash
# 1. Regular key rotation
# Generate new key annually
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_$(date +%Y)

# 2. Remove old keys safely
# Replace in authorized_keys on servers
ssh-copy-id -i ~/.ssh/id_ed25519_new.pub user@host

# 3. Audit key usage
last -a                      # Login history
sudo journalctl -u ssh       # SSH logs

# 4. Revoke compromised keys
# Remove from ~/.ssh/authorized_keys on remote
# Regenerate and redistribute new key

# 5. Backup keys securely
tar czf ~/.ssh/backup.tar.gz ~/.ssh/
# Store in secure location (encrypted, offline)
```

---

## 10. Troubleshooting

### Connection Issues

```bash
# Test SSH connectivity
ssh -v user@hostname          # Verbose output
ssh -vv user@hostname         # More verbose
ssh -vvv user@hostname        # Most verbose

# Check SSH server status
sudo systemctl status ssh
sudo ss -tlnp | grep ssh

# Test from client side
ssh -T -v user@hostname 2>&1 | grep -i "authentication"

# Common issues:
# 1. Connection refused → SSH not running
sudo systemctl start ssh

# 2. Timeout → Firewall blocking
# Check port: sudo ufw allow 22
# Or: sudo iptables -I INPUT -p tcp --dport 22 -j ACCEPT

# 3. Wrong password → Check caps lock
# Or: Use key-based auth instead

# 4. Permission denied → Wrong key or user
ssh -i ~/.ssh/id_rsa user@hostname
```

### Key Authentication Issues

```bash
# SSH not finding key automatically
ssh -i ~/.ssh/id_rsa user@hostname  # Explicit key

# Too many authentication failures
# Check ~/.ssh/config for correct settings
grep -A5 "Host hostname" ~/.ssh/config

# Verify key permissions
ls -la ~/.ssh/
# Should be: 700 for ~/.ssh, 600 for keys, 644 for .pub

# Fix permissions
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_*
chmod 644 ~/.ssh/id_*.pub
chmod 644 ~/.ssh/authorized_keys

# SSH agent issues
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_rsa
ssh-add -l

# Debug authentication
ssh -v user@hostname 2>&1 | grep -i "key\|auth"
```

### Server Configuration Issues

```bash
# Test sshd configuration
sudo sshd -t                    # Syntax check
sudo sshd -T | grep port        # Check specific setting

# Reload configuration
sudo systemctl reload ssh

# View logs
sudo journalctl -u ssh -n 20    # Last 20 lines
sudo tail -f /var/log/auth.log

# Check listening on port
sudo lsof -i :22
sudo netstat -tlnp | grep ssh

# Firewall issues
sudo ufw status
sudo ufw allow 22/tcp
```

### Performance Issues

```bash
# Slow connection
ssh -v hostname 2>&1 | grep -i "auth\|cipher"

# Increase timeout
ssh -o ConnectTimeout=10 user@hostname

# Disable X11 forwarding if not needed
ssh -x user@hostname

# Enable connection multiplexing
ssh -o ControlMaster=auto -o ControlPath=~/.ssh/control-%h-%p-%r user@hostname

# Test bandwidth
scp -l 1000 large_file user@hostname:/path/  # Limit to 1Mbps for testing
```

---

## 11. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **OpenSSH** | https://www.openssh.com/ |
| **Man Pages** | https://man.openbsd.org/ssh |
| **Config Examples** | https://man.openbsd.org/ssh_config |
| **Server Config** | https://man.openbsd.org/sshd_config |

### Learning Resources

| Resource | URL |
|----------|-----|
| **SSH Fundamentals** | https://www.ssh.com/ssh/ |
| **GitHub SSH Guide** | https://docs.github.com/en/authentication/connecting-to-github-with-ssh |
| **DigitalOcean SSH** | https://www.digitalocean.com/community/tutorials/ssh-essentials |

---

## Quick Reference

```bash
# Basic connection
ssh user@host
ssh -p 2222 user@host

# Key generation
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519

# Add key to server
ssh-copy-id user@host

# File transfer
scp file.txt user@host:/path/
scp user@host:/path/file.txt ./

# Port forwarding
ssh -L 8080:localhost:3000 user@host

# Jump host
ssh -J jumphost user@target

# Config file
nano ~/.ssh/config

# Test key auth
ssh -i ~/.ssh/id_ed25519 user@host

# SSH agent
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_ed25519

# Check fingerprint
ssh-keygen -l -f ~/.ssh/id_ed25519.pub

# Server logs
sudo journalctl -u ssh -f

# Reload server config
sudo sshd -t && sudo systemctl reload ssh
```

---

**Last Updated:** 2026
**OpenSSH Version:** 8.x+
**License:** BSD License

For detailed help: `man ssh`, `man ssh-keygen`, `man sshd_config`

---

**Security Tips:**
- Always use key-based authentication
- Protect private keys with passphrase
- Regularly audit SSH logs
- Use strong passphrases
- Keep SSH software updated
- Disable password authentication on servers
- Use non-standard ports for additional obscurity
- Monitor failed login attempts