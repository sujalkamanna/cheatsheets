# Package Management Cheatsheet (Comprehensive)

![Package Management Logo](https://www.debian.org/logos/openlogo.svg)

---

## Table of Contents
1. [What is Package Management?](#1-what-is-package-management)
2. [Core Concepts](#2-core-concepts)
3. [APT (Debian/Ubuntu) - Complete Guide](#3-apt-debianubuntu---complete-guide)
4. [YUM/DNF (RedHat/CentOS) - Complete Guide](#4-yumdnf-redhatcentos---complete-guide)
5. [Pacman (Arch Linux)](#5-pacman-arch-linux)
6. [Zypper (openSUSE)](#6-zypper-opensuse)
7. [Snap Packages](#7-snap-packages)
8. [Flatpak](#8-flatpak)
9. [Pip (Python)](#9-pip-python)
10. [NPM (Node.js)](#10-npm-nodejs)
11. [Cargo (Rust)](#11-cargo-rust)
12. [Comparison & Troubleshooting](#12-comparison--troubleshooting)

---

## 1. What is Package Management?

Package management is a system for installing, updating, removing, and managing software packages on a Linux system. It automatically handles dependencies, maintains a database of installed software, and simplifies software installation/removal.

**Key Components:**
- Package manager (tool)
- Package repository (server)
- Dependencies (required packages)
- Package cache (local storage)
- Package database (metadata)

**Key Benefits:**
- Automated dependency resolution
- Version management
- Security updates
- Centralized software distribution
- Easy uninstallation
- Consistency across systems
- Rollback capabilities
- System-wide configuration

---

## 2. Core Concepts

### Package Terminology

| Term | Meaning |
|------|---------|
| **Package** | Software bundle (binary + metadata) |
| **Repository** | Server hosting packages |
| **Dependency** | Required package for another to work |
| **Version** | Software release number (1.2.3) |
| **Binary** | Compiled executable program |
| **Source** | Original program code |
| **Archive** | Compressed package file |
| **Metadata** | Package info (name, version, dependencies) |
| **Cache** | Locally stored package data |
| **Index** | Catalog of available packages |

### Package Distribution Families

```
Debian Family
├── Debian
├── Ubuntu (Server/Desktop)
├── Linux Mint
├── Elementary OS
└── Pop!_OS
→ Package Manager: APT
→ Package Format: .deb
→ Package Tool: dpkg

RedHat Family
├── RHEL (Red Hat Enterprise Linux)
├── CentOS / Rocky Linux
├── Fedora
└── AlmaLinux
→ Package Manager: YUM / DNF
→ Package Format: .rpm
→ Package Tool: rpm

Arch Family
├── Arch Linux
├── Manjaro
└── EndeavourOS
→ Package Manager: Pacman
→ Package Format: .pkg.tar.zst
→ AUR: Arch User Repository

openSUSE Family
├── openSUSE Leap
└── openSUSE Tumbleweed
→ Package Manager: Zypper
→ Package Format: .rpm
```

---

## 3. APT (Debian/Ubuntu) - Complete Guide

### Installation Basics

```bash
# Update package list (ALWAYS do this first)
sudo apt update                    # Fetch latest package metadata
sudo apt upgrade                   # Upgrade installed packages
sudo apt full-upgrade              # Distribution upgrade
sudo apt dist-upgrade              # Legacy command (similar to full-upgrade)

# Install single package
sudo apt install package-name

# Install multiple packages
sudo apt install package1 package2 package3

# Install with auto-yes (no prompts)
sudo apt install -y package-name

# Install specific version
sudo apt install package-name=1.2.3
sudo apt install package-name=1.2.*          # Install latest 1.2.x

# Install without recommendations
sudo apt install --no-install-recommends package-name

# Install with recommended dependencies
sudo apt install --with-recommends package-name
```

### Searching & Browsing

```bash
# Search available packages
apt search keyword
apt search package-name
apt search --names-only keyword    # Search only in package names
apt search --full keyword          # Search in full description

# Search by regex
apt search '^keyword$'             # Exact match
apt search 'key.*word'             # Pattern match

# View package information
apt show package-name              # Modern way (apt)
apt-cache show package-name        # Traditional way
apt-cache search keyword           # Search in cache
apt-cache showpkg package-name     # Show package versions

# List available versions
apt-cache policy package-name
apt-cache policy package-name | grep "Release"

# Browse packages
apt list                           # All available packages
apt list --installed               # Installed packages
apt list --upgradable              # Packages with updates
apt list --all-versions            # All versions of all packages
apt list | grep keyword            # Filter packages

# Show package statistics
apt-cache stats
apt-cache search . | wc -l         # Count available packages
dpkg -l | wc -l                    # Count installed packages
```

### Removing Packages

```bash
# Remove package (keep configuration)
sudo apt remove package-name
sudo apt uninstall package-name    # Alternative

# Remove package (delete configuration)
sudo apt purge package-name
sudo apt remove --purge package-name

# Remove package and dependencies
sudo apt autoremove                # Remove unused dependencies
sudo apt remove package-name && sudo apt autoremove

# Remove all dependencies of a package
sudo apt remove --auto-remove package-name

# Clean cache
sudo apt clean                     # Remove all cached package files
sudo apt autoclean                 # Remove outdated cached packages
sudo apt autoclean --purge         # Also remove configuration

# Remove package and all its dependencies
sudo apt remove package-name
sudo apt autoremove --purge        # Remove orphaned packages
```

### Advanced APT Features

```bash
# Hold/Unhold packages (prevent updates)
sudo apt-mark hold package-name
sudo apt-mark unhold package-name
sudo apt-mark showhold             # Show held packages

# Prevent auto-update
echo "package-name hold" | sudo dpkg --set-selections

# Check for broken dependencies
sudo apt check                     # Check for issues
sudo apt install --fix-broken      # Fix broken installations
sudo apt install --fix-missing     # Install missing dependencies

# Simulate operations
apt-cache depends package-name     # Show dependencies
apt-cache rdepends package-name    # Show reverse dependencies
apt install --simulate package-name  # Simulate install (show what would happen)
apt upgrade --simulate             # Simulate upgrade

# Autofix dependencies
sudo apt --fix-broken install
sudo apt install -f                # Shorthand
sudo dpkg --configure -a           # Configure unconfigured packages

# Pinning (force specific version)
echo "package-name 1.0.0-1" | sudo dpkg --set-selections
sudo apt install package-name=1.0.0-1 -o APT::Default-Release=jessie
```

### APT Configuration Files

```bash
# Main configuration
/etc/apt/apt.conf                  # Main config file
/etc/apt/apt.conf.d/               # Additional config files

# Repository configuration
/etc/apt/sources.list              # Main sources list
/etc/apt/sources.list.d/           # Additional sources

# Example sources.list entry
deb [arch=amd64 signed-by=/usr/share/keyrings/ubuntu-archive-keyring.gpg] http://archive.ubuntu.com/ubuntu focal main restricted universe multiverse
deb-src [arch=amd64 signed-by=/usr/share/keyrings/ubuntu-archive-keyring.gpg] http://archive.ubuntu.com/ubuntu focal main restricted universe multiverse

# Preferences/Pinning
/etc/apt/preferences                # Package pinning rules
/etc/apt/preferences.d/             # Additional rules

# Add repository (PPA)
sudo add-apt-repository ppa:username/ppa-name
sudo add-apt-repository --remove ppa:username/ppa-name

# Add GPG key
wget -qO - https://example.com/key.gpg | sudo apt-key add -

# Modern way (Ubuntu 18.04+)
wget -qO - https://example.com/key.gpg | sudo tee /usr/share/keyrings/example-key.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/example-key.gpg] http://example.com/repo focal main" | sudo tee /etc/apt/sources.list.d/example.list
```

### DPKG (Lower-level Tool)

```bash
# Install local .deb file
sudo dpkg -i package.deb

# List installed packages
dpkg -l
dpkg -l | grep keyword
dpkg -l | grep -E "^ii"            # Only installed packages

# Show package info
dpkg -s package-name               # Status of installed package
dpkg -p package-name               # Info about package

# List files in package
dpkg -L package-name               # Installed package files
dpkg -L package-name | grep bin    # Find binaries

# List contents of .deb file
dpkg -c package.deb                # Files in archive
dpkg --contents package.deb

# Find which package owns a file
dpkg -S /usr/bin/command           # Find package for file
which command | xargs dpkg -S      # Combined command

# Remove package
sudo dpkg -r package-name          # Remove (keep config)
sudo dpkg -P package-name          # Purge (remove config)
sudo dpkg --remove-architecture i386  # Remove architecture

# Fix broken installs
sudo dpkg --configure -a           # Configure all
sudo dpkg --repair                 # Repair packages

# Verify package integrity
dpkg -V package-name               # Verify installed
md5sum package.deb                 # Check file integrity
```

### APT Package Management Workflow

```bash
# Daily/Weekly updates
sudo apt update                    # Update list
sudo apt upgrade                   # Install updates
sudo apt autoremove                # Clean up

# Monthly maintenance
sudo apt update
sudo apt full-upgrade              # Dist upgrade if needed
sudo apt autoremove
sudo apt autoclean

# Find and update specific package
apt list --upgradable              # See updates
sudo apt install package-name      # Update specific

# Check system
apt-get check                      # Check for broken dependencies
sudo apt install --fix-broken      # Fix issues

# Clean up old files
sudo apt autoclean
sudo apt clean
sudo apt autoremove --purge
```

---

## 4. YUM/DNF (RedHat/CentOS) - Complete Guide

### YUM Basics (Older Systems)

```bash
# Check for updates
sudo yum check-update              # List available updates
sudo yum check-update -y           # Include obsolete packages

# Update packages
sudo yum update                    # Update all packages
sudo yum update -y                 # Auto-yes
sudo yum update package-name       # Update specific package
sudo yum upgrade                   # Similar to update

# Install packages
sudo yum install package-name
sudo yum install -y package-name   # Auto-yes
sudo yum install package1 package2
sudo yum install package-name.x86_64  # Specify architecture

# Downgrade package
sudo yum downgrade package-name
sudo yum downgrade kernel          # Kernel downgrade

# Remove packages
sudo yum remove package-name
sudo yum erase package-name        # Alternative
sudo yum autoremove                # Remove unused dependencies

# Search
yum search keyword
yum search all keyword             # Search all fields
yum search package-name

# Package information
yum info package-name
yum list package-name
yum list available                 # All available packages
yum list installed                 # All installed packages
yum list updates                   # Packages with updates

# Repo information
yum repolist                       # List configured repos
yum repolist all                   # Include disabled repos
yum repoinfo repo-name             # Details about repo
```

### DNF Basics (Newer Systems)

```bash
# DNF is the newer, faster replacement for YUM
# Most commands are similar

# Check for updates
sudo dnf check-update              # List available updates
sudo dnf upgrade                   # Upgrade system

# Install packages
sudo dnf install package-name
sudo dnf install -y package-name

# Remove packages
sudo dnf remove package-name
sudo dnf autoremove                # Remove unused

# Search and browse
dnf search keyword
dnf info package-name
dnf list available
dnf list installed

# Repo management
dnf repolist
dnf repoinfo repo-name

# Caching and cleanup
sudo dnf clean all                 # Remove all cache
sudo dnf clean packages            # Remove cached packages
sudo dnf clean metadata            # Remove metadata cache
sudo dnf makecache                 # Rebuild cache
```

### YUM Transaction History

```bash
# View transaction history
yum history                        # List transactions
yum history info id                # Details about transaction
yum history list                   # Full history
yum history list | head -10        # Recent transactions

# Undo/Redo transactions
sudo yum history undo id           # Undo specific transaction
sudo yum history undo last         # Undo last transaction
sudo yum history redo id           # Redo transaction

# Resolve with history
sudo yum history resolve id        # Resolve conflicts

# Clear history
sudo yum history clear
sudo yum history delete id
```

### Repository Management

```bash
# List repositories
yum repolist                       # Enabled repos
yum repolist all                   # All repos (enabled and disabled)
yum repolist enabled
yum repolist disabled

# Enable/disable repository
sudo yum-config-manager --enable repo-name
sudo yum-config-manager --disable repo-name

# Add repository
sudo yum-config-manager --add-repo=http://example.com/repo.repo
sudo dnf config-manager --add-repo http://example.com/repo.repo

# Remove repository
sudo rm /etc/yum.repos.d/repo-name.repo
sudo yum-config-manager --remove-repo=repo-id

# View repository configuration
cat /etc/yum.repos.d/*.repo
ls /etc/yum.repos.d/

# Set repository priority
sudo nano /etc/yum.repos.d/repo.repo
# Add: priority=10 (lower number = higher priority)

# Add EPEL (Extra Packages for Enterprise Linux)
sudo yum install epel-release      # CentOS/RHEL
sudo dnf install epel-release      # Fedora

# Add RPMFusion (multimedia, etc.)
sudo yum install https://download1.rpmfusion.org/free/el/rpmfusion-free-release-$(rpm -E %rhel).noarch.rpm
```

### RPM (Lower-level Tool)

```bash
# Install local .rpm file
sudo rpm -i package.rpm            # Install
sudo rpm -U package.rpm            # Upgrade
sudo rpm -F package.rpm            # Freshen (upgrade if installed)
sudo rpm -ivh package.rpm          # Install verbose with progress

# Install from URL
sudo rpm -i http://example.com/package.rpm

# List packages
rpm -qa                            # All installed packages
rpm -qa | grep keyword
rpm -q package-name                # Is package installed?

# Package information
rpm -qi package-name               # Installed package info
rpm -qp package.rpm                # Info about .rpm file
rpm -ql package-name               # List files in package
rpm -qc package-name               # Configuration files
rpm -qd package-name               # Documentation files

# Find which package owns a file
rpm -qf /usr/bin/command           # Find package
rpm -qa | xargs rpm -ql | grep keyword

# Dependencies
rpm -qR package-name               # Requirements
rpm -qa --requires                 # Packages with requirements
rpm -qp --requires package.rpm     # Requirements of .rpm file

# Remove packages
sudo rpm -e package-name           # Erase/remove
sudo rpm -e --nodeps package-name  # Remove without checking dependencies

# Verify packages
rpm -V package-name                # Verify integrity
rpm -Va                            # Verify all packages

# Extract .rpm without installing
rpm2cpio package.rpm | cpio -idmv
```

### YUM/DNF Caching and Performance

```bash
# Clean cache
sudo yum clean all                 # Remove all caches
sudo yum clean packages            # Remove cached packages
sudo yum clean metadata            # Remove metadata
sudo yum clean dbcache             # Remove database cache

# Rebuild cache
sudo yum makecache                 # Build cache
sudo yum makecache fast            # Fast rebuild

# Parallel downloads (YUM)
sudo nano /etc/yum.conf
# Add: max_parallel_downloads=10

# Parallel downloads (DNF)
sudo nano /etc/dnf/dnf.conf
# Add: max_parallel_downloads=10

# Keep only latest packages in cache
keepcache=0                        # Don't keep cached packages
keepcache=1                        # Keep cached packages
```

### YUM Plugins and Utilities

```bash
# Useful plugins
sudo yum install yum-utils         # Additional utilities
sudo yum install yum-plugin-fastestmirror  # Auto-select fastest mirror
sudo yum install yum-plugin-security       # Security updates

# Utilities
repoquery --requires package-name  # Show requirements
repoquery --whatrequires package-name  # What requires this
yum-complete-transaction           # Complete interrupted transactions
yumdownloader package-name         # Download .rpm
yumdownloader --resolve package-name  # Download with dependencies
```

---

## 5. Pacman (Arch Linux)

### Basic Pacman Commands

```bash
# Synchronize and upgrade
sudo pacman -Syu                   # Full system upgrade
sudo pacman -Syy                   # Refresh package database
sudo pacman -Syuu                  # Upgrade, including downgrades
sudo pacman -Syyuu                 # Force refresh and upgrade

# Install packages
sudo pacman -S package-name
sudo pacman -S package1 package2
sudo pacman -S --noconfirm package-name  # No confirmation
sudo pacman -S --asdeps package-name     # Install as dependency

# Search packages
pacman -Ss keyword                 # Search repositories
pacman -Qs keyword                 # Search installed packages
pacman -F keyword                  # Search filenames
pacman -Fl package-name            # List files in package

# Package information
pacman -Si package-name            # Info from repository
pacman -Qi package-name            # Info about installed package
pacman -Qii package-name           # Detailed info about installed
pacman -Ql package-name            # List files in installed package
pacman -Qo /path/to/file           # Find package owning file

# List packages
pacman -Q                          # All installed packages
pacman -Qe                         # Explicitly installed packages
pacman -Qd                         # Packages installed as dependencies
pacman -Qq                         # Just names (no versions)
pacman -Qu                         # Packages with updates available

# Remove packages
sudo pacman -R package-name        # Remove package
sudo pacman -Rs package-name       # Remove with unused dependencies
sudo pacman -Rns package-name      # Remove (delete config files)
sudo pacman -Rs $(pacman -Qdtq)    # Remove all unused packages

# Clean cache
sudo pacman -Sc                    # Clean cache (keep latest versions)
sudo pacman -Scc                   # Clean all cache
sudo pacman -R $(pacman -qdtq)     # Remove orphaned packages
```

### AUR (Arch User Repository)

```bash
# Install AUR helper (yay)
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si

# Or install paru (alternative)
sudo pacman -S paru

# Search AUR
yay -Ss keyword
yay keyword                        # Implicit search
paru -Ss keyword

# Install from AUR
yay -S package-name
yay -S --noconfirm package-name
paru -S package-name

# Update AUR packages
yay -Sua                           # Update AUR only
yay -Syu                           # Update all (official + AUR)
paru -Sua

# AUR statistics
yay -Ps

# View PKGBUILD before installing
yay -S --edit package-name
```

### Pacman Configuration

```bash
# Configuration file
/etc/pacman.conf

# Repository sections
[core]
[extra]
[community]
[multilib]
[custom]

# Add custom repository
echo "[custom]" | sudo tee -a /etc/pacman.conf
echo "Server = http://example.com/repo" | sudo tee -a /etc/pacman.conf
sudo pacman -Sy

# Parallel downloads (Pacman 6.0+)
sudo nano /etc/pacman.conf
# Uncomment: ParallelDownloads = 10

# Keep only recent cache
sudo paccache -r           # Remove uninstalled packages
sudo paccache -rk3         # Keep 3 recent versions
```

---

## 6. Zypper (openSUSE)

### Basic Zypper Commands

```bash
# Refresh repositories
sudo zypper refresh                # Refresh all repos
sudo zypper ref                    # Short form
sudo zypper refresh --force        # Force refresh

# Update packages
sudo zypper update                 # Update packages
sudo zypper up                     # Short form
sudo zypper dist-upgrade           # Distribution upgrade
sudo zypper dup                    # Short form

# Install packages
sudo zypper install package-name
sudo zypper in package-name
sudo zypper install package1 package2

# Search packages
zypper search keyword
zypper se keyword
zypper search --details keyword    # Show details
zypper search -t package keyword   # Search only packages

# Package information
zypper info package-name
zypper if package-name             # Short form

# List packages
zypper packages                    # All packages
zypper packages --installed        # Installed packages
zypper products                    # Products/patterns

# Remove packages
sudo zypper remove package-name
sudo zypper rm package-name
sudo zypper remove-orphaned        # Remove unused packages

# Clean cache
sudo zypper clean                  # Clean all caches
sudo zypper cc                     # Short form
sudo zypper clean --all            # Remove all cached packages
```

### Zypper Repository Management

```bash
# List repositories
zypper repos
zypper lr

# Add repository
sudo zypper addrepo repo-url repo-name
sudo zypper ar repo-url repo-name

# Remove repository
sudo zypper removerepo repo-id
sudo zypper rr repo-id

# Enable/disable repository
sudo zypper modifyrepo --enable repo-name
sudo zypper modifyrepo --disable repo-name

# Set repository priority
sudo zypper modifyrepo --priority 50 repo-name

# Refresh specific repository
sudo zypper refresh repo-name
```

---

## 7. Snap Packages

### Basic Snap Commands

```bash
# Search snap packages
snap search keyword
snap find keyword

# Install snap
sudo snap install package-name
sudo snap install --classic package-name  # Unrestricted (classic)
sudo snap install --edge package-name     # Edge version
sudo snap install --beta package-name     # Beta version
sudo snap install --candidate package-name

# List snaps
snap list
snap list --all                    # Including disabled
snap list | grep keyword

# View snap details
snap info package-name
snap details package-name

# Snap permissions/interfaces
snap connections                   # All connections
snap connections package-name      # Specific snap
snap interface interface-name      # Interface details

# Update snaps
snap refresh                       # Update all snaps
snap refresh package-name          # Update specific snap
snap refresh --beta package-name   # Update to beta version

# Enable/disable snaps
sudo snap disable package-name
sudo snap enable package-name
sudo snap remove package-name      # Uninstall

# Snap configuration
snap set package-name key=value    # Set config
snap get package-name key          # Get config
snap get package-name              # Get all config
```

### Snap Channels and Versions

```bash
# List available channels
snap info package-name             # Show available channels

# Switch channel
sudo snap switch --channel=edge package-name
sudo snap switch --channel=stable package-name
sudo snap switch --channel=beta package-name

# Revert to previous version
sudo snap revert package-name
sudo snap revert package-name --revision=123

# Install specific revision
sudo snap install package-name --revision=123
```

### Snap Confinement and Permissions

```bash
# Connect interface/permission
sudo snap connect package-name:interface
sudo snap connect package-name:interface other-snap:slot

# Disconnect interface
sudo snap disconnect package-name:interface

# View interface documentation
snap interface interface-name

# Grant/revoke permissions via connections
snap connections                   # Current permissions
sudo snap connect snap:interface   # Grant access
sudo snap disconnect snap:interface # Revoke access
```

---

## 8. Flatpak

### Basic Flatpak Commands

```bash
# Add Flathub (main repository)
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# Search packages
flatpak search keyword
flatpak search package-name

# Install applications
flatpak install flathub package-name
flatpak install --user flathub package-name  # User-level
flatpak install --system flathub package-name  # System-level

# List installed apps
flatpak list --app                 # Applications only
flatpak list --runtime             # Runtimes only
flatpak list                       # Everything
flatpak list --user                # User-installed
flatpak list --system              # System-wide

# View app details
flatpak info package-name
flatpak show package-name          # Similar

# Update applications
flatpak update                     # Update all
flatpak update package-name        # Update specific
flatpak update --user              # User installations
flatpak update --system            # System installations

# Run applications
flatpak run package-name
flatpak run --devel package-name   # Development version
```

### Flatpak Permissions

```bash
# View permissions
flatpak info package-name          # All permissions
flatpak override --user --show package-name  # Show overrides

# Grant permissions
sudo flatpak override --user --filesystem=home package-name
sudo flatpak override --user --socket=wayland package-name
sudo flatpak override --user --device=dri package-name

# Revoke permissions
sudo flatpak override --user --unset=filesystem package-name
sudo flatpak override --user --unset=socket package-name

# Common permissions
--filesystem=host                  # Full filesystem
--filesystem=home                  # Home directory only
--socket=wayland                   # Wayland display
--socket=x11                       # X11 display
--device=dri                       # GPU access
--share=network                    # Network
--talk-name=org.freedesktop.*     # D-Bus access
```

### Flatpak Maintenance

```bash
# Clean unused
flatpak uninstall --unused
flatpak remove --unused-runtimes

# Remove application
flatpak uninstall package-name

# Remove with config
flatpak uninstall --delete-data package-name

# Clean cache
flatpak repair                     # Repair installation
flatpak install --reinstall package-name  # Reinstall
```

---

## 9. Pip (Python)

### Python Virtual Environments

```bash
# Create virtual environment
python3 -m venv myenv              # Create
python3 -m venv myenv --system-site-packages  # Include system packages

# Activate virtual environment
source myenv/bin/activate          # Linux/macOS
myenv\Scripts\activate             # Windows
source myenv/bin/activate.fish     # Fish shell

# Deactivate
deactivate

# Check Python version in venv
python --version

# Create with specific Python version
python3.9 -m venv myenv            # Use Python 3.9
```

### Pip Package Management

```bash
# Install packages
pip install package-name
pip3 install package-name
pip install package-name==1.2.3    # Specific version
pip install 'package-name>=1.0,<2.0'  # Version range
pip install package-name[extra]    # With extras/features

# Install from file
pip install -r requirements.txt
pip install -r requirements/base.txt requirements/dev.txt

# Install from git
pip install git+https://github.com/user/repo.git
pip install git+https://github.com/user/repo.git@branch-name
pip install git+ssh://git@github.com/user/repo.git

# Install in editable mode (development)
pip install -e /path/to/package
pip install -e .                   # Current directory

# List installed packages
pip list
pip list --outdated                # Show updates available
pip list -o                        # Short form
pip list --format=json             # JSON output

# Show package information
pip show package-name              # Details
pip index versions package-name    # Available versions

# Search packages (deprecated)
# Use https://pypi.org instead
```

### Pip Requirements Management

```bash
# Export installed packages
pip freeze > requirements.txt
pip freeze --local > requirements.txt  # Exclude venv packages

# Export specific packages
pip freeze | grep -i keyword > requirements.txt

# Create requirements manually
cat > requirements.txt << EOF
Django==3.2.0
requests>=2.25.0,<3.0.0
pytest>=6.0
EOF

# Install from requirements
pip install -r requirements.txt
pip install -r requirements.txt --upgrade

# Compare versions
pip list --outdated
pip index versions package-name

# Update packages
pip install --upgrade package-name
pip install -U package-name
pip install -r requirements.txt --upgrade

# Upgrade pip itself
pip install --upgrade pip
python -m pip install --upgrade pip  # Alternative
```

### Pip Advanced Usage

```bash
# Download packages without installing
pip download package-name -d ./packages
pip download -r requirements.txt -d ./packages

# Install from downloaded packages
pip install --no-index --find-links ./packages package-name

# Install with specific cache
pip install --cache-dir ./cache package-name

# Check for conflicts
pip check

# Uninstall packages
pip uninstall package-name
pip uninstall -r requirements.txt
pip uninstall -y package-name      # No confirmation

# Uninstall with dependencies
pip install pipdeptree
pipdeptree --packages package-name  # Show dependencies
pip uninstall package-name $(pipdeptree -p package-name -f | tail -n +2 | awk '{print $2}' | sed 's/\[.*\]//')

# Install from local wheel
pip install ./package-0.1-py3-none-any.whl

# Build package
pip install build
python -m build

# List package dependencies
pip show package-name
pipdeptree
pipdeptree -p package-name
```

---

## 10. NPM (Node.js)

### NPM Basic Commands

```bash
# Check npm and node versions
npm --version
npm -v
node --version
node -v

# Install packages locally
npm install package-name
npm i package-name                 # Shorthand
npm install package-name@1.2.3     # Specific version
npm install 'package-name@^1.2.3'  # Compatible version
npm install 'package-name@~1.2.3'  # Approximately

# Install as development dependency
npm install --save-dev package-name
npm install -D package-name
npm install --dev package-name

# Install globally
sudo npm install -g package-name
npm install -g --prefix /usr/local package-name

# Install peer dependencies
npm install package-name --force
npm install package-name --legacy-peer-deps  # For old packages

# Install from GitHub
npm install github:user/repo
npm install github:user/repo#branch-name
npm install https://github.com/user/repo.git
```

### NPM Project Management

```bash
# Initialize new project
npm init                           # Interactive
npm init -y                        # Use defaults
npm init --scope=@user             # Scoped package

# Install from package.json
npm install                        # Install all dependencies
npm ci                             # Clean install (from lock file)
npm install --production           # Skip dev dependencies

# package.json structure
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "My application",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "jest",
    "build": "webpack"
  },
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "jest": "^27.0.0"
  }
}

# Run scripts
npm start                          # npm run start
npm test                           # npm run test
npm run build                      # Custom script
npm run                            # List available scripts
```

### NPM Dependency Management

```bash
# List dependencies
npm list                           # All dependencies (tree)
npm list --depth=0                 # Top-level only
npm list --all                     # Include peer dependencies
npm list --global                  # Globally installed

# List outdated packages
npm outdated
npm outdated --global
npm outdated | grep wanted         # Show updates available

# Update packages
npm update                         # Update all
npm update package-name            # Update specific
npm update -g                      # Update global
npm update -g package-name         # Update global specific

# Update to specific version
npm install package-name@latest
npm install package-name@next      # Beta/next version

# Security vulnerabilities
npm audit                          # Check for vulnerabilities
npm audit fix                      # Automatically fix
npm audit fix --force              # Force fix (may break things)

# Remove packages
npm uninstall package-name
npm remove package-name
npm rm package-name
npm uninstall -g package-name      # Global removal

# Remove node_modules
rm -rf node_modules
npm ci                             # Clean reinstall
```

### NPM Advanced Features

```bash
# Lock file management
npm ci                             # Use package-lock.json
npm install --package-lock-only    # Update lock without installing
npm ci --offline                   # Use cache

# Cache management
npm cache verify                   # Verify cache integrity
npm cache clean --force            # Clear cache
npm cache ls                       # List cache contents

# Version management
npm version major                  # Bump major version
npm version minor                  # Bump minor version
npm version patch                  # Bump patch version
npm version 1.2.3                  # Set specific version

# Publish packages
npm login                          # Authenticate
npm publish                        # Publish to NPM
npm unpublish package-name@1.0.0   # Unpublish specific version

# Link packages (development)
npm link                           # Link from current directory
npm link package-name              # Link package for development

# Check for issues
npm ls                             # Show duplicate packages
npm dedupe                         # Remove duplicates
```

---

## 11. Cargo (Rust)

### Cargo Basic Commands

```bash
# Create new project
cargo new my-project
cargo new --bin my-project         # Binary project
cargo new --lib my-project         # Library project
cargo init                         # Initialize existing directory

# Build project
cargo build                        # Debug build
cargo build --release              # Release build (optimized)
cargo build --target x86_64-unknown-linux-gnu

# Run project
cargo run                          # Build and run
cargo run --release
cargo run -- arg1 arg2             # With arguments

# Check project
cargo check                        # Check without building
cargo check --release

# Test project
cargo test                         # Run tests
cargo test test_name               # Run specific test
cargo test -- --show-output        # Show println! output

# Documentation
cargo doc                          # Generate docs
cargo doc --open                   # Generate and open in browser

# Format code
cargo fmt                          # Format using rustfmt
cargo fmt --check                  # Check formatting

# Lint code
cargo clippy                       # Run clippy linter
cargo clippy -- -D warnings        # Deny warnings
```

### Cargo Dependency Management

```bash
# Add dependencies
cargo add package-name
cargo add package-name@1.0
cargo add --dev package-name       # Dev dependency
cargo add --build package-name     # Build dependency

# List dependencies
cargo tree                         # Dependency tree
cargo tree --duplicates            # Show duplicates
cargo tree --depth 2               # Limit depth

# Update dependencies
cargo update                       # Update to latest compatible
cargo update -p package-name       # Update specific package

# Remove dependencies
cargo remove package-name
cargo remove --dev package-name

# Check for security vulnerabilities
cargo audit                        # Check vulnerabilities
cargo audit fix                    # Fix issues
```

### Cargo.toml Management

```toml
[package]
name = "my-project"
version = "0.1.0"
edition = "2021"

[dependencies]
serde = "1.0"
tokio = { version = "1.0", features = ["full"] }
anyhow = "1.0"

[dev-dependencies]
criterion = "0.4"

[build-dependencies]
cc = "1.0"

[[bin]]
name = "my-app"
path = "src/main.rs"

[profile.release]
opt-level = 3
lto = true
codegen-units = 1
```

---

## 12. Comparison & Troubleshooting

### Package Manager Comparison

| Feature | APT | YUM/DNF | Pacman | Snap | Flatpak |
|---------|-----|---------|--------|------|---------|
| **Install** | apt install | yum install | pacman -S | snap install | flatpak install |
| **Update** | apt upgrade | yum update | pacman -Syu | snap refresh | flatpak update |
| **Remove** | apt remove | yum remove | pacman -R | snap remove | flatpak uninstall |
| **Search** | apt search | yum search | pacman -Ss | snap search | flatpak search |
| **Package Type** | .deb | .rpm | .pkg.tar | .snap | .flatpak |
| **Distros** | Debian, Ubuntu | RHEL, CentOS | Arch, Manjaro | Universal | Universal |
| **Isolation** | No | No | No | Yes | Yes |
| **Size** | Small | Medium | Small | Medium-Large | Large |
| **Speed** | Fast | Medium | Very Fast | Medium | Medium |
| **Dependencies** | Automatic | Automatic | Automatic | Bundled | Bundled |

### Common Troubleshooting

```bash
# APT Issues
sudo apt update                    # Fix broken cache
sudo apt --fix-broken install
sudo dpkg --configure -a
sudo rm /var/lib/apt/lists/lock    # Remove lock file

# YUM Issues
sudo yum clean all
sudo yum update
sudo yum history undo last

# Pacman Issues
sudo pacman -Syu
sudo pacman -Syyu                  # Force refresh
sudo rm /var/lib/pacman/sync/*     # Remove cache

# Permission errors
sudo chown -R $USER ~/.npm         # Fix npm permissions
sudo chown -R $USER ~/.pip         # Fix pip permissions

# Lock file conflicts
sudo lsof | grep .lock
killall apt apt-get yum dnf       # Kill package managers
sudo rm /var/lib/apt/lists/lock

# Broken dependencies
# APT
sudo apt install -f
sudo apt --fix-broken install

# YUM
sudo yum install yum-utils
sudo yum-complete-transaction

# Pacman
sudo pacman -Syu
```

### Best Practices Summary

```bash
# Always update first
sudo apt update && sudo apt upgrade         # APT
sudo yum update                             # YUM
sudo pacman -Syu                            # Pacman

# Keep system clean
sudo apt autoremove && sudo apt autoclean   # APT
sudo yum autoremove                         # YUM
sudo pacman -Sc && sudo pacman -Rsn $(pacman -Qdtq)  # Pacman

# Maintain requirements files
pip freeze > requirements.txt
npm ci --package-lock-only

# Regular maintenance
- Update weekly/monthly
- Check for security issues
- Remove unused packages
- Clean cache periodically
- Keep backups of configurations
```

---

## Quick Command Reference

```bash
# Ubuntu/Debian
sudo apt update && sudo apt upgrade
sudo apt install package
sudo apt search keyword
sudo apt remove package
sudo apt autoremove

# CentOS/RHEL
sudo yum update
sudo yum install package
sudo yum search keyword
sudo yum remove package
sudo yum autoremove

# Arch
sudo pacman -Syu
sudo pacman -S package
pacman -Ss keyword
sudo pacman -R package

# Snap
snap install package
snap update
snap list
snap remove package

# Flatpak
flatpak install flathub package
flatpak update
flatpak list
flatpak uninstall package

# Python
pip install package
pip freeze > requirements.txt
pip list --outdated
pip uninstall package

# Node.js
npm install package
npm install -r requirements.txt
npm list --outdated
npm uninstall package
```

---

**Last Updated:** 2026
**Package Managers Covered:** APT, YUM/DNF, Pacman, Zypper, Snap, Flatpak, Pip, NPM, Cargo
**License:** Open Source

---

**Tips:**
- Always run updates before installing packages
- Use virtual environments for Python projects
- Keep lock files for reproducible builds
- Check security vulnerabilities regularly
- Maintain requirements files
- Test updates in staging first
- Document custom repositories
- Keep system packages separate from user packages