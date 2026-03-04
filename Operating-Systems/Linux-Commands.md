# Linux Commands Cheatsheet

![Linux Logo](https://www.linux.com/images/logos/linux-tux-mascot.png)

---

## Table of Contents
1. [File & Directory Management](#1-file--directory-management)
2. [Navigation & Permissions](#2-navigation--permissions)
3. [File Content & Searching](#3-file-content--searching)
4. [Process & System Management](#4-process--system-management)
5. [Networking Commands](#5-networking-commands)
6. [Text Processing](#6-text-processing)
7. [Package Management](#7-package-management)
8. [Disk & Storage](#8-disk--storage)
9. [User & Group Management](#9-user--group-management)
10. [System Information](#10-system-information)
11. [Compression & Archives](#11-compression--archives)
12. [Advanced Tips](#12-advanced-tips)

---

## 1. File & Directory Management

### Basic Commands

```bash
# List files
ls                          # List directory contents
ls -l                       # Long format
ls -la                      # Include hidden files
ls -lh                      # Human-readable sizes
ls -lS                      # Sort by size
ls -lt                      # Sort by modification time
ls -R                       # Recursive listing
ls -d */                    # List directories only

# Directory operations
pwd                         # Print working directory
cd /path/to/dir            # Change directory
cd ~                        # Home directory
cd -                        # Previous directory
cd ..                       # Parent directory
mkdir my_folder            # Create directory
mkdir -p path/to/dir       # Create with parents
rmdir my_folder            # Remove empty directory
rm -rf my_folder           # Remove directory with contents (careful!)

# File operations
touch file.txt             # Create empty file
cp file.txt file_copy.txt  # Copy file
cp -r dir1 dir2            # Copy directory recursively
mv file.txt new_name.txt   # Move/rename file
rm file.txt                # Remove file
rm -f file.txt             # Force remove
```

### Useful File Operations

```bash
# Create files with content
echo "content" > file.txt           # Create/overwrite
echo "content" >> file.txt          # Append
cat > file.txt << EOF              # Multi-line input
line 1
line 2
EOF

# Copy with attributes
cp -p file.txt backup.txt          # Preserve permissions/timestamps
cp -a dir1 dir2                    # Archive copy (best for backups)

# Safe removal
rm -i file.txt                     # Interactive (ask before delete)
rm -v file.txt                     # Verbose (show what's being deleted)

# Find and delete
find . -name "*.tmp" -delete       # Delete all .tmp files
find . -type f -empty -delete      # Delete empty files
```

---

## 2. Navigation & Permissions

### Permissions Management

```bash
# View permissions
ls -l                      # Show permissions (rwxrwxrwx)
stat file.txt              # Detailed file information

# Change permissions (numeric)
chmod 755 file.txt         # rwxr-xr-x (user: 7, group: 5, other: 5)
chmod 644 file.txt         # rw-r--r--
chmod 600 file.txt         # rw-------
chmod 700 dir/             # rwx------

# Change permissions (symbolic)
chmod u+x file.txt         # Add execute for user
chmod g+r file.txt         # Add read for group
chmod o-w file.txt         # Remove write for others
chmod a+r file.txt         # Add read for all
chmod -R 755 dir/          # Recursive

# Change ownership
chown user file.txt        # Change user
chown user:group file.txt  # Change user and group
chown -R user:group dir/   # Recursive
sudo chown root file.txt   # Change to root

# Numeric permission table
# 4 = read (r)
# 2 = write (w)
# 1 = execute (x)
# 7 = rwx, 6 = rw-, 5 = r-x, 4 = r--, 0 = ---
```

### Path & Environment

```bash
# View environment variables
echo $PATH                 # Display PATH
echo $HOME                 # Home directory
echo $USER                 # Current user
env                        # All environment variables
printenv                   # Display environment

# Set variables (temporary)
export VAR_NAME="value"    # Export variable
VAR_NAME="value"           # Local variable

# Working with paths
which command              # Find command location
whereis command            # Comprehensive search
type command               # Command type/info
```

---

## 3. File Content & Searching

### View File Content

```bash
# Display content
cat file.txt               # Display file
cat file1.txt file2.txt    # Display multiple files
less file.txt              # Paginated view (q to quit)
more file.txt              # Similar to less
head file.txt              # First 10 lines
head -n 20 file.txt        # First 20 lines
tail file.txt              # Last 10 lines
tail -n 20 file.txt        # Last 20 lines
tail -f file.txt           # Follow file (updates)
wc file.txt                # Word/line/character count
wc -l file.txt             # Line count only
```

### Search & Find

```bash
# Find files
find . -name "*.txt"                    # Find by name
find . -name "*test*" -type f          # Find files matching pattern
find . -type d -name "logs"            # Find directories
find . -size +100M                     # Files larger than 100MB
find . -mtime -7                       # Modified in last 7 days
find . -user username                  # Files owned by user
find . -perm 644                       # Files with specific permissions
find . -exec rm {} \;                  # Execute command on results

# Search file contents
grep "pattern" file.txt                # Search for pattern
grep -i "pattern" file.txt             # Case-insensitive
grep -r "pattern" ./dir/               # Recursive search
grep -n "pattern" file.txt             # Show line numbers
grep -c "pattern" file.txt             # Count matches
grep -v "pattern" file.txt             # Invert (exclude) match
grep "^pattern" file.txt               # Pattern at line start
grep "pattern$" file.txt               # Pattern at line end

# Advanced grep (regex)
grep -E "[0-9]{3}-[0-9]{4}" file.txt  # Extended regex
grep -P "\d{3}" file.txt               # Perl regex (if enabled)
```

### Text Processing Tools

```bash
# Sort and unique
sort file.txt                          # Sort lines
sort -r file.txt                       # Reverse sort
sort -u file.txt                       # Unique lines
sort -n file.txt                       # Numeric sort
uniq file.txt                          # Remove duplicates (sorted)
uniq -c file.txt                       # Count occurrences

# Cut and awk
cut -d':' -f1 file.txt                # Cut by delimiter (:), field 1
cut -c1-10 file.txt                   # Cut characters 1-10
awk '{print $1, $3}' file.txt         # Print fields 1 and 3
awk -F':' '{print $1}' file.txt       # Use colon as delimiter
awk 'NR>1' file.txt                   # Skip first line

# Sed (stream editor)
sed 's/old/new/' file.txt             # Replace (first occurrence per line)
sed 's/old/new/g' file.txt            # Replace all occurrences
sed '5d' file.txt                     # Delete line 5
sed -i 's/old/new/g' file.txt         # In-place replacement
```

---

## 4. Process & System Management

### Process Management

```bash
# View processes
ps                         # Current shell processes
ps aux                     # All processes (detailed)
ps -ef                     # All processes (alternative format)
ps -C process_name        # Processes by name
top                        # Interactive process monitor
htop                       # Better top (if installed)
pgrep process_name        # Find process ID (PID)

# Kill processes
kill PID                   # Send SIGTERM (graceful)
kill -9 PID               # Send SIGKILL (force kill)
killall process_name      # Kill by process name
pkill process_name        # Kill by pattern

# Background/Foreground
command &                 # Run in background
jobs                      # List background jobs
fg %1                     # Bring job 1 to foreground
bg %1                     # Resume job 1 in background
Ctrl+Z                    # Pause current process
Ctrl+C                    # Terminate current process
```

### System Management

```bash
# Shutdown/Reboot
shutdown -h now            # Shutdown now
shutdown -r now            # Reboot now
shutdown -h +10            # Shutdown in 10 minutes
shutdown -c                # Cancel scheduled shutdown
reboot                     # Reboot (immediate)
halt                       # Halt system

# System information
uname -a                   # Kernel information
hostnamectl                # Hostname/OS info
lsb_release -a             # Linux distribution info
cat /proc/version          # Kernel version

# Services (systemd)
systemctl start service    # Start service
systemctl stop service     # Stop service
systemctl restart service  # Restart service
systemctl reload service   # Reload config
systemctl enable service   # Auto-start at boot
systemctl disable service  # Disable auto-start
systemctl status service   # Service status
systemctl list-units       # List all services
```

### Running Commands

```bash
# Execute commands
bash script.sh             # Run bash script
sh script.sh               # Run sh script
./script.sh                # Execute script (if executable)
source script.sh           # Run in current shell
. script.sh                # Same as source

# Scheduling
cron                       # Background task scheduler
crontab -e                 # Edit cron jobs
crontab -l                 # List cron jobs
# Cron format: minute hour day month weekday command
# 0 2 * * * /path/to/backup.sh  (daily at 2 AM)

at command                 # Schedule one-time execution
at now + 1 hour            # Schedule 1 hour from now
atq                        # View scheduled jobs
```

---

## 5. Networking Commands

### Network Configuration

```bash
# IP Configuration
ip addr show               # Show IP addresses
ip addr add 192.168.1.10/24 dev eth0   # Add IP
ip addr del 192.168.1.10/24 dev eth0   # Remove IP
ip route show              # Show routing table
ip route add default via 192.168.1.1   # Add default gateway
ifconfig                   # Network interface config (deprecated)
iwconfig                   # Wireless config

# DNS Configuration
cat /etc/resolv.conf       # View DNS servers
nslookup example.com       # Query DNS
dig example.com            # DNS lookup (detailed)
ping example.com           # Test connectivity
traceroute example.com     # Trace route to host
mtr example.com            # Interactive traceroute
```

### Network Testing & Troubleshooting

```bash
# Connectivity
ping -c 4 example.com              # Ping with count
ping -i 0.2 example.com            # Set interval
curl http://example.com            # Fetch web content
wget http://example.com/file       # Download file
curl -I http://example.com         # Get headers only
curl -X POST -d "data" http://url  # POST request

# Port checking
netstat -tlnp               # TCP listening ports with PIDs
netstat -an                 # All network connections
ss -tlnp                    # Socket statistics (newer)
lsof -i :8080              # Process using port 8080
telnet example.com 80      # Test connection to port
nc -zv example.com 80      # Netcat port scan

# Network traffic
tcpdump -i eth0            # Capture packets
tcpdump -i eth0 port 80    # Capture port 80 traffic
iftop                      # Bandwidth monitor
nethogs                    # Per-process bandwidth
```

### SSH & Remote Access

```bash
# SSH connection
ssh user@host              # Connect to remote host
ssh -p 2222 user@host      # Custom SSH port
ssh user@host "command"    # Run command remotely
ssh -i /path/to/key user@host   # Use specific key

# SSH key management
ssh-keygen -t rsa -b 4096  # Generate SSH key
ssh-copy-id user@host      # Copy public key to remote
ssh-add ~/.ssh/id_rsa      # Add key to agent

# Secure copy
scp file.txt user@host:/path/       # Copy to remote
scp user@host:/path/file.txt ./     # Copy from remote
scp -r dir/ user@host:/path/        # Copy directory recursively
```

---

## 6. Text Processing

### sed (Stream Editor)

```bash
# Basic replacements
sed 's/old/new/' file.txt           # Replace first occurrence
sed 's/old/new/g' file.txt          # Replace all
sed 's/old/new/2' file.txt          # Replace 2nd occurrence only
sed -i 's/old/new/g' file.txt       # In-place edit
sed 's|/path/old|/path/new|g' file  # Use | as delimiter

# Delete operations
sed '5d' file.txt                   # Delete line 5
sed '1,3d' file.txt                 # Delete lines 1-3
sed '/pattern/d' file.txt           # Delete lines matching pattern
sed '$d' file.txt                   # Delete last line

# Insert/Append
sed '5i\New line' file.txt          # Insert before line 5
sed '5a\New line' file.txt          # Append after line 5
sed '1i\Header' file.txt            # Add header

# Multiple commands
sed -e 's/old/new/g' -e 's/foo/bar/g' file.txt
```

### awk (Text Processing)

```bash
# Basic usage
awk '{print $1}' file.txt           # Print first field
awk '{print $1, $3}' file.txt       # Print fields 1 and 3
awk -F':' '{print $1}' file.txt     # Use : as delimiter
awk '{print NF}' file.txt           # Print number of fields
awk '{print NR}' file.txt           # Print line number

# Conditional processing
awk '$3 > 100' file.txt             # Print lines where field 3 > 100
awk '/pattern/ {print}' file.txt    # Print matching lines
awk '!/pattern/ {print}' file.txt   # Print non-matching lines

# Calculations
awk '{sum += $1} END {print sum}' file.txt  # Sum column
awk '{print $1 * 2}' file.txt               # Multiply values
awk 'NR > 1' file.txt                       # Skip header
```

### Common Patterns

```bash
# Remove duplicates
sort file.txt | uniq
awk '!seen[$0]++' file.txt

# Filter and process
grep "pattern" file.txt | awk '{print $1}'

# Replace in multiple files
find . -name "*.txt" -exec sed -i 's/old/new/g' {} \;

# Count words/lines
wc -w file.txt              # Word count
wc -l file.txt              # Line count
grep -c "pattern" file.txt  # Count matches

# Extract columns
cut -d',' -f1,3 file.csv    # Extract columns 1 and 3 from CSV
awk -F',' '{print $1, $3}' file.csv
```

---

## 7. Package Management

### apt (Debian/Ubuntu)

```bash
# Update package list
sudo apt update              # Update package index
sudo apt upgrade             # Upgrade packages
sudo apt full-upgrade        # Dist upgrade

# Install packages
sudo apt install package-name
sudo apt install -y package-name        # Auto-yes
sudo apt install package1 package2      # Multiple packages

# Remove packages
sudo apt remove package-name            # Remove package
sudo apt purge package-name             # Remove with config
sudo apt autoremove                     # Remove unused dependencies

# Search and info
apt search keyword           # Search packages
apt list --installed         # List installed packages
apt show package-name        # Package information
apt-cache search pattern     # Search cache
```

### yum/dnf (RedHat/CentOS)

```bash
# Update packages
sudo yum check-update        # Check for updates
sudo yum update              # Update all packages
sudo yum update -y           # Auto-yes

# Install packages
sudo yum install package-name
sudo yum install -y package-name

# Remove packages
sudo yum remove package-name
sudo yum autoremove          # Remove unused

# Search and info
yum search keyword
yum info package-name
yum list installed
```

### Snap & Flatpak

```bash
# Snap (Ubuntu)
snap search keyword
snap install package-name
snap list                    # List installed snaps
snap remove package-name

# Flatpak
flatpak search keyword
flatpak install package-name
flatpak list
flatpak remove package-name
```

---

## 8. Disk & Storage

### Disk Usage

```bash
# Disk space
df                          # Disk free
df -h                       # Human-readable
df -T                       # Show filesystem type
du -sh dir/                 # Directory size
du -sh */                   # Size of each directory
du -sh .[^.]* */            # Include hidden files
du --max-depth=2 -h         # Limit depth

# Find large files
find . -type f -size +100M -exec ls -lh {} \;
find . -type f -exec ls -s {} \; | sort -rn | head -10
du -ah /path | sort -rh | head -10
```

### Mount & Unmount

```bash
# View mounts
mount                       # Show mounted filesystems
mount | grep /mnt           # Grep specific mount
df -h                       # Disk usage of mounts

# Mount filesystems
sudo mount /dev/sda1 /mnt/disk      # Mount partition
sudo mount -o remount,rw /          # Remount with RW
sudo mount -t iso9660 -o loop file.iso /mnt/iso

# Unmount
sudo umount /mnt/disk               # Unmount
sudo umount -l /mnt/disk            # Lazy unmount
sudo umount -f /mnt/disk            # Force unmount
```

### Filesystem Tools

```bash
# Check filesystem
sudo fsck /dev/sda1         # Check filesystem
sudo fsck -n /dev/sda1      # Check without repair

# Format filesystem
sudo mkfs.ext4 /dev/sda1    # Create ext4
sudo mkfs.ntfs /dev/sda1    # Create NTFS
sudo mkfs.vfat /dev/sda1    # Create FAT32

# View partition info
lsblk                       # List block devices
fdisk -l                    # List partitions
blkid                       # Show UUIDs
parted -l                   # Partition table info
```

---

## 9. User & Group Management

### User Management

```bash
# Create user
sudo useradd username
sudo useradd -m username            # Create home directory
sudo useradd -s /bin/bash username  # Set shell
sudo useradd -d /custom/home username

# User information
whoami                      # Current user
id                          # User and group IDs
id username                 # Specific user info
w                           # Logged in users
who                         # User login information
last                        # Login history

# Change password
passwd                      # Change own password
sudo passwd username        # Change user password

# Modify user
sudo usermod -l newname oldname      # Rename user
sudo usermod -s /bin/bash username   # Change shell
sudo usermod -aG groupname username  # Add to group
sudo usermod -G group1,group2 user   # Set groups (replace)

# Delete user
sudo userdel username               # Delete user
sudo userdel -r username            # Delete with home directory
```

### Group Management

```bash
# Create group
sudo groupadd groupname
sudo groupadd -g 1000 groupname     # With specific GID

# View groups
groups                      # User's groups
groups username             # User's groups
getent group                # All groups
getent group groupname      # Group info

# Modify groups
sudo groupmod -n newname oldname    # Rename group
sudo groupmod -g 1001 groupname     # Change GID
sudo gpasswd -a user group          # Add user to group
sudo gpasswd -d user group          # Remove user from group

# Delete group
sudo groupdel groupname
```

### Sudo Configuration

```bash
# View sudo config
sudo visudo                 # Safe sudoers edit
sudo cat /etc/sudoers       # View sudoers
sudo -l                     # List sudo privileges

# Edit sudoers (safe way)
sudo visudo
# Add: username ALL=(ALL) ALL
# Add: username ALL=(ALL) NOPASSWD: /bin/restart
```

---

## 10. System Information

### System Details

```bash
# Hardware information
uname -a                    # All kernel information
uname -s                    # Kernel name
uname -r                    # Kernel release
lscpu                       # CPU information
lsmem                       # Memory information
free -h                     # Memory usage

# Detailed info
cat /proc/cpuinfo           # CPU details
cat /proc/meminfo           # Memory details
cat /proc/uptime            # System uptime
uptime                      # Uptime summary
hostname                    # System hostname
hostnamectl                 # Hostname (systemd)

# Disk and hardware
lsblk                       # Block devices
lspci                       # PCI devices
lsusb                       # USB devices
hwinfo                      # Hardware info (if installed)
dmidecode                   # DMI info
```

### System Monitoring

```bash
# Real-time monitoring
top                         # Process monitor
htop                        # Better top
vmstat 1 5                  # Virtual memory (5 samples, 1s interval)
iostat 1 5                  # I/O statistics
sar -u 1 5                  # CPU usage (SAR)

# Logs
journalctl                  # System journal
journalctl -xe              # Recent errors
journalctl -u service-name  # Specific service logs
tail -f /var/log/syslog     # System log
tail -f /var/log/auth.log   # Auth log
```

---

## 11. Compression & Archives

### Compression

```bash
# gzip
gzip file.txt               # Compress (creates .gz)
gunzip file.txt.gz          # Decompress
gzip -d file.txt.gz         # Alternative decompress
gzip -k file.txt            # Keep original

# bzip2
bzip2 file.txt              # Compress (creates .bz2)
bunzip2 file.txt.bz2        # Decompress

# xz
xz file.txt                 # Compress (creates .xz)
unxz file.txt.xz            # Decompress

# zip
zip file.zip file.txt       # Create zip
unzip file.zip              # Extract zip
zip -r archive.zip dir/     # Recursive zip
```

### Tar Archives

```bash
# Create archives
tar -cf archive.tar file.txt         # Create uncompressed
tar -czf archive.tar.gz dir/         # Create gzipped
tar -cjf archive.tar.bz2 dir/        # Create bzipped
tar -cJf archive.tar.xz dir/         # Create xz compressed

# Extract archives
tar -xf archive.tar                  # Extract tar
tar -xzf archive.tar.gz              # Extract gzipped tar
tar -xjf archive.tar.bz2             # Extract bzipped tar
tar -xJf archive.tar.xz              # Extract xz tar
tar -xf archive.tar -C /path/        # Extract to directory

# List contents
tar -tzf archive.tar.gz              # List files in gz archive
tar -tf archive.tar                  # List tar contents

# Useful options
tar -czf archive.tar.gz --exclude='*.log' dir/  # Exclude files
tar -czf archive.tar.gz --wildcards --exclude='*.tmp' dir/
```

---

## 12. Advanced Tips

### Command Piping & Redirection

```bash
# Piping
command1 | command2         # Pipe output to command2
grep "pattern" file | wc -l # Count matches
ps aux | grep process       # Find process

# Redirection
command > file.txt          # Redirect output (overwrite)
command >> file.txt         # Append output
command 2> error.log        # Redirect error
command 2>&1 output.log     # Redirect error to output
command &> output.log       # Both stdout and stderr

# Combining
ls -l > output.txt 2>&1     # Output and error to file
grep "error" *.log 2>/dev/null | head -10
```

### Command Substitution & Variables

```bash
# Command substitution
result=$(command)           # Store output in variable
result=`command`            # Older syntax
echo "Today is $(date)"     # Embed in string

# Variable operations
VAR="value"
echo $VAR                   # Print variable
echo ${VAR}                 # Explicit variable
echo ${VAR:-default}        # Default if unset
echo ${VAR:0:5}             # Substring

# Arrays
arr=(val1 val2 val3)
echo ${arr[0]}              # First element
echo ${arr[@]}              # All elements
echo ${#arr[@]}             # Array length
```

### Useful Aliases & Functions

```bash
# Create aliases (add to ~/.bashrc)
alias ll='ls -la'
alias gs='git status'
alias update='sudo apt update && sudo apt upgrade'
alias mkdir='mkdir -pv'
alias cp='cp -v'
alias rm='rm -i'

# Functions
function mkcd() {
    mkdir -p "$1"
    cd "$1"
}

function backup() {
    cp "$1" "$1.backup.$(date +%Y%m%d-%H%M%S)"
}

# Use: mkcd new_directory
```

### Job Control & Backgrounding

```bash
# Running processes
command &                   # Run in background
jobs                        # List jobs
jobs -l                     # List with PID
fg %1                       # Bring job 1 to foreground
bg %1                       # Resume job 1 in background

# Detach from terminal
nohup command &             # Run immune to hangups
command &                   # Run in background
screen                      # Terminal multiplexer
tmux                        # Another multiplexer
```

### System Optimization

```bash
# Clear cache (safe)
sync                        # Sync filesystems
free && sync && echo 3 > /proc/sys/vm/drop_caches && free

# Disk cleanup
sudo apt autoremove -y      # Remove unused packages
sudo apt autoclean -y       # Remove old package cache
sudo apt clean              # Clean package cache
sudo journalctl --vacuum-time=7d    # Clean old logs

# Find and remove
find /tmp -type f -atime +7 -delete      # Remove old temp files
find /var/log -name "*.gz" -mtime +30 -delete  # Old logs
```

### Useful One-Liners

```bash
# Find and replace in multiple files
find . -name "*.txt" -exec sed -i 's/old/new/g' {} \;

# Count files by type
find . -type f | sed 's/.*\.//' | sort | uniq -c

# Monitor disk usage
watch -n 5 'du -sh .'

# Top memory consumers
ps aux | sort -k 3 -rn | head -10

# Network bandwidth
watch -n 1 'nstat -z; sleep 1; nstat'

# Generate random password
openssl rand -base64 12

# Current connections
netstat -an | grep ESTABLISHED | wc -l

# Find large files
find . -type f -size +1G
```

---

## Quick Reference Summary

```bash
# Essential commands
ls, cd, pwd, mkdir, rm, cp, mv, cat, grep, find, ps, kill, top
sudo, passwd, whoami, ssh, scp, ping, curl, wget, tar, zip, unzip
systemctl, journalctl, apt/yum, chmod, chown, su, df, du, mount

# Most used patterns
Command | Grep               # Pipe to search
Command > File              # Redirect output
Command &                   # Background
sudo Command                # Run as root
Find . -name "pattern"      # Search files
Grep -r "pattern" ./dir     # Search content
ps aux | grep process       # Find process
kill PID                    # Stop process
```

---

**Last Updated:** 2026
**Tested on:** Ubuntu 22.04 LTS, CentOS 8, Debian 11
**License:** Open Source

For detailed man pages: `man command_name`

---

**Tips:**
- Use `Tab` for command completion
- Use `Ctrl+R` for command history search
- Use `man command` for help
- Google is your friend for complex tasks
- Test commands with echo before using with `rm -r`