# NPM Cheatsheet

![NPM Logo](https://www.npmjs.com/npm-logo.png)

---

## Table of Contents
1. [What is NPM?](#1-what-is-npm)
2. [Installation & Setup](#2-installation--setup)
3. [Project Initialization](#3-project-initialization)
4. [Package Management](#4-package-management)
5. [Dependencies vs DevDependencies](#5-dependencies-vs-devdependencies)
6. [package.json Structure](#6-packagejson-structure)
7. [npm Scripts](#7-npm-scripts)
8. [Version Management](#8-version-management)
9. [Global Packages](#9-global-packages)
10. [Publishing Packages](#10-publishing-packages)
11. [Best Practices](#11-best-practices)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. What is NPM?

NPM (Node Package Manager) is the default package manager for Node.js. It allows developers to install, manage, and publish reusable code packages from the npm registry.

**Key Features:**
- Package dependency management
- Version control for packages
- Script automation
- Publishing and sharing code
- Package discovery and search
- Security vulnerability scanning
- Workspaces for monorepos
- Offline capabilities

---

## 2. Installation & Setup

### Install Node.js and NPM

```bash
# macOS with Homebrew
brew install node

# Ubuntu/Debian
sudo apt-get update
sudo apt-get install nodejs npm

# Windows
# Download from https://nodejs.org/

# Verify installation
node --version
npm --version

# Update npm to latest
npm install -g npm@latest

# Update Node.js
npm install -g n  # Node version manager
n latest
```

### Configuration

```bash
# View npm config
npm config list

# Set npm registry
npm config set registry https://registry.npmjs.org/

# Set default author
npm config set init-author-name "Your Name"
npm config set init-author-email "you@example.com"

# View config file
cat ~/.npmrc

# Set auth token
npm config set //registry.npmjs.org/:_authToken=YOUR_TOKEN
```

---

## 3. Project Initialization

### Create New Project

```bash
# Initialize with defaults
npm init

# Initialize with all defaults (no prompts)
npm init -y

# Initialize with specific template
npm init -y

# Verify package.json created
cat package.json
```

### Basic package.json

```json
{
  "name": "my-awesome-app",
  "version": "1.0.0",
  "description": "My awesome application",
  "main": "index.js",
  "type": "module",
  "scripts": {
    "start": "node index.js",
    "test": "jest",
    "build": "webpack",
    "dev": "nodemon index.js"
  },
  "keywords": ["node", "express"],
  "author": "Your Name",
  "license": "MIT",
  "dependencies": {},
  "devDependencies": {}
}
```

---

## 4. Package Management

### Install Packages

```bash
# Install all dependencies from package.json
npm install

# Short form
npm i

# Install specific package
npm install express

# Install specific version
npm install express@4.18.0

# Install latest version
npm install express@latest

# Install package and save to dependencies
npm install --save lodash

# Install as dev dependency
npm install --save-dev webpack

# Short flags
npm i -S lodash        # --save
npm i -D webpack       # --save-dev

# Install globally
npm install -g nodemon

# Install from GitHub
npm install username/repo-name

# Install from local file
npm install ../local-package
```

### Update Packages

```bash
# Check for outdated packages
npm outdated

# Update single package
npm update express

# Update all packages
npm update

# Upgrade to major version
npm install express@latest

# View package info
npm view express

# View installed version
npm list

# View only top-level packages
npm list --depth=0
```

### Remove Packages

```bash
# Remove package
npm uninstall express

# Remove from devDependencies
npm uninstall --save-dev webpack

# Remove globally
npm uninstall -g nodemon

# Remove and prune unused
npm prune
```

### Search and Info

```bash
# Search npm registry
npm search express

# View package details
npm view express

# View specific version info
npm view express@4.18.0

# View package homepage
npm view express homepage

# View package repository
npm view express repository.url
```

---

## 5. Dependencies vs DevDependencies

### Understanding Scopes

```json
{
  "dependencies": {
    "express": "^4.18.0",
    "lodash": "^4.17.21"
  },
  "devDependencies": {
    "jest": "^29.0.0",
    "webpack": "^5.0.0",
    "eslint": "^8.0.0"
  }
}
```

### Installation Commands

```bash
# Install as production dependency
npm install express
npm install --save express
npm i -S express

# Install as development dependency
npm install --save-dev jest
npm install -D jest

# Install as optional dependency
npm install --save-optional dotenv

# Install as peer dependency (usually for libraries)
# Manually add to pom.json under peerDependencies
```

### Dependency Comparison

| Type | Use Case | Production Bundle | Installation |
|------|----------|-------------------|--------------|
| **dependencies** | Runtime libraries | Included | `npm i` |
| **devDependencies** | Build tools, testing | Not included | `npm i` or `npm i --only=dev` |
| **peerDependencies** | Plugin compatibility | Not installed | Warn if missing |
| **optionalDependencies** | Optional features | Not required | Install if available |

---

## 6. package.json Structure

### Complete Example

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "My application",
  "main": "dist/index.js",
  "module": "dist/index.esm.js",
  "types": "dist/index.d.ts",
  "type": "module",
  "bin": {
    "my-cli": "./bin/cli.js"
  },
  "files": [
    "dist",
    "README.md"
  ],
  "engines": {
    "node": ">=14.0.0",
    "npm": ">=6.0.0"
  },
  "author": "Your Name <you@example.com>",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/username/my-app.git"
  },
  "homepage": "https://github.com/username/my-app#readme",
  "bugs": {
    "url": "https://github.com/username/my-app/issues"
  },
  "keywords": [
    "node",
    "express",
    "api"
  ],
  "scripts": {
    "start": "node dist/index.js",
    "dev": "nodemon src/index.js",
    "build": "webpack --mode production",
    "test": "jest",
    "test:watch": "jest --watch",
    "lint": "eslint src/",
    "format": "prettier --write src/",
    "prepublishOnly": "npm run build && npm run test"
  },
  "dependencies": {
    "express": "^4.18.0",
    "axios": "^1.0.0"
  },
  "devDependencies": {
    "jest": "^29.0.0",
    "webpack": "^5.0.0",
    "eslint": "^8.0.0",
    "prettier": "^2.8.0"
  },
  "peerDependencies": {
    "react": ">=16.0.0"
  },
  "optionalDependencies": {
    "fsevents": "^2.3.0"
  }
}
```

### Field Descriptions

| Field | Description |
|-------|-------------|
| **name** | Package name (lowercase, no spaces) |
| **version** | Semantic version (MAJOR.MINOR.PATCH) |
| **description** | Short package description |
| **main** | Entry point for CommonJS |
| **module** | Entry point for ES modules |
| **types** | TypeScript definitions file |
| **bin** | CLI commands to install globally |
| **files** | Files included when published |
| **engines** | Required Node/npm versions |
| **scripts** | Custom npm commands |
| **dependencies** | Production dependencies |
| **devDependencies** | Development dependencies |

---

## 7. npm Scripts

### Define Scripts

```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "build": "webpack --mode production",
    "build:dev": "webpack --mode development",
    "test": "jest --coverage",
    "test:watch": "jest --watch",
    "lint": "eslint src/",
    "lint:fix": "eslint src/ --fix",
    "format": "prettier --write .",
    "serve": "http-server dist/",
    "clean": "rm -rf dist/",
    "prebuild": "npm run clean && npm run lint",
    "postbuild": "echo 'Build complete!'",
    "deploy": "npm run build && npm run test && git push"
  }
}
```

### Run Scripts

```bash
# Run main scripts (no 'run' needed)
npm start
npm test

# Run custom scripts (requires 'run')
npm run build
npm run dev
npm run lint
npm run lint:fix
npm run format

# Run script with arguments
npm run build -- --watch
npm test -- --coverage

# Run multiple scripts
npm run lint && npm run build
npm run build && npm run test && npm run deploy

# Run script in sequence
npm run clean && npm run build

# Background execution
npm run dev &
```

### Pre and Post Hooks

```json
{
  "scripts": {
    "prebuild": "npm run clean && npm run lint",
    "build": "webpack",
    "postbuild": "echo 'Build finished!'",
    "pretest": "npm run lint",
    "test": "jest",
    "posttest": "npm run coverage"
  }
}
```

---

## 8. Version Management

### Semantic Versioning

```
MAJOR.MINOR.PATCH
1.    2.      3

MAJOR - Breaking changes
MINOR - New features (backward compatible)
PATCH - Bug fixes (backward compatible)
```

### Version Operators

```json
{
  "dependencies": {
    "express": "4.18.0",           // Exact version
    "express": "^4.18.0",          // Compatible with version (allows minor/patch)
    "express": "~4.18.0",          // Approximately equivalent (patch updates only)
    "express": "4.18.x",           // Any patch version
    "express": "4.x",              // Any minor and patch version
    "express": "*",                // Any version
    "express": ">=4.18.0",         // Greater than or equal
    "express": ">4.18.0",          // Greater than
    "express": "<=4.18.0",         // Less than or equal
    "express": "<4.18.0",          // Less than
    "express": ">=4.18.0 <5.0.0"   // Range
  }
}
```

### Manage Versions

```bash
# View current version
npm view . version

# Bump patch version (1.0.0 -> 1.0.1)
npm version patch

# Bump minor version (1.0.0 -> 1.1.0)
npm version minor

# Bump major version (1.0.0 -> 2.0.0)
npm version major

# Set specific version
npm version 2.0.0

# Create git tag
npm version patch  # Automatically creates git tag v1.0.1

# Don't create git tag
npm version patch --no-git-tag-version
```

### Lock Files

```bash
# Generate package-lock.json
npm install

# Update lock file
npm install

# Install exact versions from lock file
npm ci  # (clean install)

# View lock file
cat package-lock.json
```

---

## 9. Global Packages

### Install Globally

```bash
# Install CLI tool globally
npm install -g nodemon
npm install -g webpack-cli
npm install -g create-react-app
npm install -g typescript

# Short form
npm i -g nodemon

# Install specific version
npm install -g @latest/package@2.0.0
```

### Manage Global Packages

```bash
# List global packages
npm list -g

# List only top-level global packages
npm list -g --depth=0

# Update global package
npm update -g nodemon

# Uninstall global package
npm uninstall -g nodemon

# View global installation path
npm config get prefix

# Change global installation path (macOS/Linux)
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
export PATH=~/.npm-global/bin:$PATH
```

### Create CLI Tool

```bash
# Create bin directory
mkdir bin

# Create CLI entry point
cat > bin/my-cli.js << 'EOF'
#!/usr/bin/env node
console.log('Hello from my CLI!');
EOF

# Make executable
chmod +x bin/my-cli.js

# Add to package.json
# "bin": { "my-cli": "./bin/my-cli.js" }

# Install locally to test
npm install -g .

# Use command
my-cli
```

---

## 10. Publishing Packages

### Prepare Package

```bash
# Create npm account
# https://www.npmjs.com/signup

# Login to npm
npm login

# Verify login
npm whoami
```

### Publish Package

```bash
# Check package.json is complete
npm pack  # Creates tarball preview

# Publish to npm
npm publish

# Publish with tag
npm publish --tag beta

# Publish scoped package
npm publish

# Publish from package.json with prepublishOnly script
```

### package.json for Publishing

```json
{
  "name": "@username/my-package",
  "version": "1.0.0",
  "description": "My awesome package",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "files": [
    "dist",
    "README.md",
    "LICENSE"
  ],
  "repository": {
    "type": "git",
    "url": "https://github.com/username/my-package.git"
  },
  "keywords": [
    "useful",
    "keywords"
  ],
  "author": "Your Name",
  "license": "MIT",
  "scripts": {
    "build": "tsc",
    "test": "jest",
    "prepublishOnly": "npm run build && npm run test"
  }
}
```

### Manage Published Package

```bash
# Update version and publish
npm version patch
npm publish

# View published package
npm view @username/my-package

# Add user as maintainer
npm owner add username @username/my-package

# Deprecate version
npm deprecate @username/my-package@1.0.0 "Use 2.0.0 instead"

# Unpublish (only within 72 hours)
npm unpublish @username/my-package@1.0.0
```

---

## 11. Best Practices

### Project Setup

```bash
# Use .gitignore
echo "node_modules/" >> .gitignore
echo "dist/" >> .gitignore
echo ".env" >> .gitignore

# Use .npmignore for publishing
echo "src/" >> .npmignore
echo "tests/" >> .npmignore
echo ".eslintrc" >> .npmignore

# Commit lock files
git add package-lock.json
git commit -m "Add package lock file"
```

### Dependency Management

```json
{
  "scripts": {
    "audit": "npm audit",
    "audit:fix": "npm audit fix"
  }
}
```

```bash
# Check for vulnerabilities
npm audit

# Fix vulnerabilities
npm audit fix

# Force fix breaking changes
npm audit fix --force

# Review before installing
npm list --all

# Regularly update dependencies
npm update
npm outdated

# Clean cache
npm cache clean --force
```

### Version Strategy

```json
{
  "version": "1.0.0-alpha.1",
  "description": "Semantic versioning with prerelease"
}
```

```bash
# Pre-release versions
# 1.0.0-alpha
# 1.0.0-beta
# 1.0.0-rc.1
# 1.0.0

# Install pre-release
npm install express@next

# List all versions
npm view express versions
```

### Scripts Organization

```json
{
  "scripts": {
    "start": "node dist/index.js",
    "dev": "nodemon src/index.js",
    "build": "webpack --mode production",
    "build:watch": "webpack --watch",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "lint": "eslint src/",
    "lint:fix": "eslint src/ --fix",
    "format": "prettier --write .",
    "clean": "rm -rf dist/",
    "prebuild": "npm run clean",
    "postbuild": "npm run test"
  }
}
```

---

## 12. Troubleshooting

### Common Issues

```bash
# Clear npm cache
npm cache clean --force

# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install

# Fix peer dependency warnings
npm install --legacy-peer-deps

# Update npm itself
npm install -g npm@latest

# Check npm config
npm config list

# Reset npm config to defaults
npm config set registry https://registry.npmjs.org/

# Verify installation
npm doctor
```

### Permission Issues (macOS/Linux)

```bash
# Fix npm permissions
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
export PATH=~/.npm-global/bin:$PATH

# Or use sudo (not recommended)
sudo npm install -g package-name
```

### Network Issues

```bash
# Check registry status
npm config get registry

# Use different registry
npm config set registry https://registry.taobao.org

# Use npm via proxy
npm config set proxy http://proxy:8080
npm config set https-proxy http://proxy:8080

# Offline mode
npm install --prefer-offline

# Force online
npm install --prefer-online
```

### Dependency Conflicts

```bash
# Check dependency tree
npm list

# Check for circular dependencies
npm ls

# Identify duplicate packages
npm list | grep "deduped"

# Force resolution
npm install --legacy-peer-deps

# Manually override versions
npm install express@4.18.0 --save-exact
```

### Quick Debugging

```bash
# Verbose output
npm install --verbose

# Debug mode
npm install --loglevel verbose

# Check what would be installed
npm install --dry-run

# Audit before install
npm install --audit
```

---

## Quick Reference Commands

```bash
# Initialization
npm init -y                        # Quick project setup
npm install                        # Install all dependencies

# Package Installation
npm install <package>              # Install package
npm install -D <package>           # Install dev dependency
npm install -g <package>           # Install globally

# Package Management
npm list                           # List installed packages
npm outdated                       # Check outdated packages
npm update                         # Update all packages
npm uninstall <package>            # Remove package

# Script Execution
npm start                          # Run start script
npm test                           # Run test script
npm run <script>                   # Run custom script
npm run build                      # Build project

# Version Management
npm version patch                  # Patch version bump
npm version minor                  # Minor version bump
npm version major                  # Major version bump

# Publishing
npm login                          # Login to npm
npm publish                        # Publish package
npm view <package>                 # View package info

# Troubleshooting
npm audit                          # Check vulnerabilities
npm audit fix                      # Fix vulnerabilities
npm cache clean --force            # Clear cache
npm ci                             # Clean install (use lock file)
```

---

## Essential npm Configuration

### .npmrc File

```ini
# ~/.npmrc or ./.npmrc in project root

# Registry
registry=https://registry.npmjs.org/

# Authentication
//registry.npmjs.org/:_authToken=your_token_here

# Package defaults
init-author-name=Your Name
init-author-email=you@example.com
init-license=MIT

# Installation
save-exact=false
save-prefix=^
engine-strict=false

# Security
audit=true
audit-level=moderate
```

---

## Resources

| Resource | URL |
|----------|-----|
| **Official Documentation** | https://docs.npmjs.com/ |
| **npm Registry** | https://www.npmjs.com/ |
| **Package Search** | https://www.npmjs.com/search |
| **npm CLI Commands** | https://docs.npmjs.com/cli/ |
| **Semantic Versioning** | https://semver.org/ |
| **Node.js** | https://nodejs.org/ |

---

**Last Updated:** 2024
**NPM Version:** 8.x+
**Node.js Version:** 14.x+

For latest information, visit [NPM Official Documentation](https://docs.npmjs.com/)