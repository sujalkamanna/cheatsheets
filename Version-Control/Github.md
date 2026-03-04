# GitHub Cheatsheet

![GitHub Logo](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png)

---

## Table of Contents
1. [What is GitHub?](#1-what-is-github)
2. [Account Setup](#2-account-setup)
3. [Repository Management](#3-repository-management)
4. [Pull Requests & Code Review](#4-pull-requests--code-review)
5. [Issues & Project Management](#5-issues--project-management)
6. [GitHub Actions (CI/CD)](#6-github-actions-cicd)
7. [Collaboration & Teams](#7-collaboration--teams)
8. [Security & Access Control](#8-security--access-control)
9. [GitHub Pages & Wiki](#9-github-pages--wiki)
10. [API & Automation](#10-api--automation)
11. [Best Practices](#11-best-practices)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. What is GitHub?

GitHub is a cloud-based Git repository hosting platform and collaboration tool. It provides version control, project management, CI/CD capabilities, and community features for software development teams.

**Key Features:**
- Git repository hosting
- Pull request workflow
- Code review tools
- Issue tracking
- GitHub Actions (CI/CD)
- Project management
- Team collaboration
- Access control
- GitHub Pages
- API & Webhooks

---

## 2. Account Setup

### Create Account

```
1. Visit https://github.com/signup
2. Enter email, password, username
3. Verify email address
4. Choose plan (Free/Pro/Enterprise)
5. Complete setup
```

### SSH Configuration

```bash
# Generate SSH key
ssh-keygen -t rsa -b 4096 -C "your.email@github.com"
ssh-keygen -t ed25519 -C "your.email@github.com"  # Modern alternative

# Add to SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa

# Copy public key
cat ~/.ssh/id_rsa.pub

# Add to GitHub
# Settings → SSH and GPG keys → New SSH key → Paste key

# Test connection
ssh -T git@github.com
```

### Global Configuration

```bash
# Set Git configuration
git config --global user.name "Your Name"
git config --global user.email "your.email@github.com"

# Set default editor
git config --global core.editor "vim"

# Set default branch
git config --global init.defaultBranch main

# Configure GPG signing
git config --global user.signingkey <key-id>
git config --global commit.gpgsign true
```

### Personal Access Token

```bash
# Create personal access token
# GitHub → Settings → Developer settings → Personal access tokens → Generate new token

# Use token for authentication
git clone https://github.com/username/repo.git
# When prompted for password, use token instead

# Set token in environment
export GITHUB_TOKEN=ghp_xxxxxxxxxxxxx

# Use in scripts
curl -H "Authorization: token $GITHUB_TOKEN" https://api.github.com/user
```

---

## 3. Repository Management

### Create Repository

```
1. Click "+" → New repository
2. Enter repository name
3. Add description (optional)
4. Choose public/private
5. Initialize with README (optional)
6. Choose .gitignore template
7. Choose license
8. Click "Create repository"
```

### Clone & Setup

```bash
# Clone repository
git clone https://github.com/username/repository.git
cd repository

# Via SSH (recommended)
git clone git@github.com:username/repository.git

# Configure remote
git remote -v
git remote add origin https://github.com/username/repo.git
git remote set-url origin git@github.com:username/repo.git
```

### Repository Settings

```
Settings Menu:
├── General
│   ├── Repository name
│   ├── Description
│   ├── Public/Private
│   └── Delete repository
├── Collaborators & teams
├── Security & analysis
├── Branches
├── Pages
├── Webhooks
├── Deploy keys
├── Environments
└── Actions → General
```

### Branch Protection

```
Repository Settings → Branches → Add rule

Protection Options:
├── Require pull request review
├── Require status checks to pass
├── Require branches to be up to date
├── Require code owner review
├── Restrict who can push
├── Include administrators
└── Prevent force push
```

### Repository Topics

```
Add topics to categorize repository:
Repository → About (gear icon) → Topics

Examples:
- python
- machine-learning
- devops
- kubernetes
- docker
- terraform
```

---

## 4. Pull Requests & Code Review

### Create Pull Request

```bash
# Push feature branch
git push -u origin feature/user-auth

# Create via GitHub UI
# 1. Go to repository
# 2. Click "Pull requests" tab
# 3. Click "New pull request"
# 4. Select base and compare branches
# 5. Add title and description
# 6. Click "Create pull request"

# Create with GitHub CLI
gh pr create --title "Add user authentication" --body "Implements login feature"
gh pr create --assignee @me --reviewer reviewer1,reviewer2
```

### PR Template

```markdown
<!-- .github/pull_request_template.md -->

## Description
Brief description of changes

## Type of change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Related issues
Closes #123

## Changes made
- Change 1
- Change 2
- Change 3

## Testing
- [ ] Unit tests added
- [ ] Integration tests passed
- [ ] Manual testing completed

## Screenshots/Videos (if applicable)
[Add screenshots]

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added
- [ ] Documentation updated
- [ ] No new warnings
- [ ] Tests pass

## Reviewers
@reviewer1 @reviewer2
```

### Code Review

```
Review Process:
1. Open pull request
2. Review changes (Files changed tab)
3. Add comments on code
4. Request changes or Approve
5. Author responds to comments
6. Merge when approved and tests pass

Review Features:
├── View diff
├── Comment on lines
├── Suggest changes
├── Approve/Request changes
├── Add review summary
└── Mark as ready for review
```

### Merge Pull Request

```bash
# Merge strategies
1. Create a merge commit (default)
2. Squash and merge (single commit)
3. Rebase and merge (linear history)

# Merge via GitHub UI
# Click "Merge pull request" button

# Merge via CLI
gh pr merge <pr-number>
gh pr merge <pr-number> --squash
gh pr merge <pr-number> --rebase

# Delete branch after merge
git push origin --delete feature/user-auth
# Or check "Delete branch" on GitHub UI
```

---

## 5. Issues & Project Management

### Create Issue

```
1. Go to repository
2. Click "Issues" tab
3. Click "New issue"
4. Add title and description
5. Add labels, assignees, milestone
6. Click "Submit new issue"

Via CLI:
gh issue create --title "Bug: Login not working" --body "Description"
gh issue create --assignee @me --label "bug,urgent"
```

### Issue Template

```markdown
<!-- .github/ISSUE_TEMPLATE/bug_report.md -->

## Bug description
Clear description of the bug

## Steps to reproduce
1. Step 1
2. Step 2
3. Step 3

## Expected behavior
What should happen

## Actual behavior
What actually happens

## Environment
- OS: [macOS/Linux/Windows]
- Browser: [Chrome/Firefox]
- Version: [e.g. 1.0.0]

## Screenshots
[If applicable]

## Additional context
[Any other relevant information]
```

### Issue Management

```bash
# Work with issues via CLI
gh issue list
gh issue list --state closed
gh issue view <issue-number>
gh issue comment <issue-number> -b "Comment text"
gh issue close <issue-number>
gh issue reopen <issue-number>
gh issue edit <issue-number> --add-label bug
```

### Labels

```
Common Labels:
├── bug (red)
├── enhancement (blue)
├── documentation (purple)
├── good first issue (green)
├── help wanted (yellow)
├── wontfix (gray)
├── duplicate (gray)
├── invalid (gray)
├── urgent (red)
└── blocked (red)

Create custom labels:
Repository → Labels → New label
```

### Milestones

```
Create Milestones:
Repository → Milestones → New milestone

Use for:
├── Version releases (v1.0, v1.1)
├── Sprint planning
├── Deadline tracking
└── Feature grouping

Associate issues:
Issue → Milestone dropdown → Select milestone
```

### GitHub Projects

```
Create Project:
Repository → Projects → New project

Board Types:
├── Table (spreadsheet view)
├── Board (kanban view)
└── Roadmap (timeline view)

Workflow:
1. Create columns/statuses
2. Add issues/PRs to project
3. Move through statuses
4. Track progress

Example Status Flow:
Todo → In Progress → In Review → Done
```

---

## 6. GitHub Actions (CI/CD)

### Basic Workflow

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.11'
    
    - name: Install dependencies
      run: |
        pip install -r requirements.txt
    
    - name: Run tests
      run: |
        pytest tests/
    
    - name: Run linting
      run: |
        pylint src/
```

### Node.js Workflow

```yaml
# .github/workflows/nodejs.yml
name: Node.js CI

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [16.x, 18.x, 20.x]
    
    steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
    
    - run: npm ci
    - run: npm run build
    - run: npm test
    - run: npm run lint
```

### Docker Build Workflow

```yaml
# .github/workflows/docker.yml
name: Docker Build

on:
  push:
    branches: [ main ]
    tags: [ 'v*' ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v2
    
    - name: Login to Docker Hub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}
    
    - name: Build and push
      uses: docker/build-push-action@v4
      with:
        context: .
        push: true
        tags: |
          ${{ secrets.DOCKER_USERNAME }}/myapp:latest
          ${{ secrets.DOCKER_USERNAME }}/myapp:${{ github.sha }}
```

### Deployment Workflow

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Deploy to production
      env:
        DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
        DEPLOY_HOST: ${{ secrets.DEPLOY_HOST }}
      run: |
        mkdir -p ~/.ssh
        echo "$DEPLOY_KEY" > ~/.ssh/deploy_key
        chmod 600 ~/.ssh/deploy_key
        ssh-keyscan $DEPLOY_HOST >> ~/.ssh/known_hosts
        ssh -i ~/.ssh/deploy_key user@$DEPLOY_HOST './deploy.sh'
```

### Scheduled Workflows

```yaml
# .github/workflows/scheduled.yml
name: Scheduled Tasks

on:
  schedule:
    - cron: '0 2 * * *'  # Daily at 2 AM UTC
    - cron: '0 0 * * 0'  # Weekly on Sunday

jobs:
  backup:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - run: ./scripts/backup.sh
```

### Using Secrets

```bash
# Add secrets in GitHub UI
# Repository → Settings → Secrets and variables → Actions → New secret

# Use in workflow
env:
  DATABASE_URL: ${{ secrets.DATABASE_URL }}
  API_KEY: ${{ secrets.API_KEY }}

# Or individual
- run: echo ${{ secrets.SECRET_NAME }}
```

---

## 7. Collaboration & Teams

### Adding Collaborators

```
Repository Settings → Collaborators and teams → Add people

Permission Levels:
├── Pull (Read)    - Clone, pull, see discussions
├── Triage (Triage) - Read + manage issues/PRs
├── Push (Write)   - Read + push, merge PRs
├── Maintain       - Write + manage settings
└── Admin          - Full access
```

### Organization Setup

```
Create Organization:
GitHub → Profile → Organizations → New organization

Organization Structure:
├── Teams
├── Repositories
├── Members
├── Settings
└── Projects

Create Team:
Organization → Teams → New team

Assign Permissions:
Team → Repositories → Add repositories
```

### Code Owners

```bash
# Create CODEOWNERS file
cat > CODEOWNERS << 'EOF'
# Default owners for all files
* @default-owner

# Frontend team owns all .js files
*.js @frontend-team

# Backend team owns API routes
src/api/ @backend-team

# DevOps team owns infrastructure
infrastructure/ @devops-team
.github/workflows/ @devops-team

# Specific files
docker-compose.yml @devops-team
package.json @maintainer
EOF

# Push to repository
git add CODEOWNERS
git commit -m "Add code owners"
git push origin main
```

### Discussion Forums

```
Enable Discussions:
Repository Settings → Features → Discussions

Use for:
├── Q&A
├── General discussion
├── Announcements
├── Show and tell
└── Polls
```

---

## 8. Security & Access Control

### Secrets Management

```bash
# Add repository secrets
# Settings → Secrets and variables → Actions → New secret

# Use in workflows
- run: npm install --token=${{ secrets.NPM_TOKEN }}

# Environment-specific secrets
# Settings → Environments → Create environment
# Add environment secrets

# Use environment in workflow
jobs:
  deploy:
    environment: production
    env:
      SECRET: ${{ secrets.DB_PASSWORD }}
```

### Branch Protection Rules

```
Settings → Branches → Add rule

Protections:
├── Require pull request reviews
├── Require status checks to pass
├── Require branches up to date
├── Require code owner review
├── Restrict who can push
├── Include administrators
└── Prevent deletion
```

### Dependabot

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
    commit-message:
      prefix: "chore"
    open-pull-requests-limit: 10

  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"
```

### Security Scanning

```
Enable in Settings:
├── Dependabot alerts
├── Dependabot security updates
├── Secret scanning
├── Code scanning (Advanced)
└── Private vulnerability reporting
```

---

## 9. GitHub Pages & Wiki

### GitHub Pages

```
Enable in Settings:
Repository → Settings → Pages

Options:
├── Deploy from branch
├── GitHub Actions
└── Custom domain

Create site:
1. Create docs/ folder or gh-pages branch
2. Push index.html or markdown files
3. Site available at: username.github.io/repo

Example (Jekyll):
# _config.yml
title: My Project
description: Project description
theme: jekyll-theme-minimal

# index.md
# Welcome
```

### GitHub Wiki

```
Enable in Settings:
Repository → Settings → Features → Wiki

Create wiki:
1. Go to repository → Wiki
2. Click "Create the first page"
3. Add title and content
4. Save page

Wiki structure:
├── Home (default page)
├── [Custom pages]
└── Sidebar (optional)

Clone wiki repository:
git clone https://github.com/username/repo.wiki.git
```

---

## 10. API & Automation

### GitHub CLI

```bash
# Install GitHub CLI
brew install gh
choco install gh           # Windows
sudo apt install gh        # Ubuntu

# Login
gh auth login

# Common commands
gh repo list              # List repositories
gh repo create my-repo    # Create repository
gh repo clone username/repo  # Clone repo
gh issue create           # Create issue
gh pr create              # Create pull request
gh pr list                # List PRs
gh pr view <pr-number>    # View PR
gh release create v1.0.0  # Create release
```

### REST API

```bash
# Get user info
curl https://api.github.com/users/username

# List repositories
curl https://api.github.com/users/username/repos

# Get repository
curl https://api.github.com/repos/username/repo

# Create issue
curl -X POST https://api.github.com/repos/username/repo/issues \
  -H "Authorization: token $GITHUB_TOKEN" \
  -d '{"title":"Bug","body":"Description"}'

# List pull requests
curl https://api.github.com/repos/username/repo/pulls

# Get workflow runs
curl https://api.github.com/repos/username/repo/actions/runs
```

### Webhooks

```
Configure Webhook:
Settings → Webhooks → Add webhook

Webhook Types:
├── Push
├── Pull request
├── Issues
├── Release
├── Workflow run
└── Custom events

Use webhook to trigger CI/CD, notifications, etc.
```

---

## 11. Best Practices

### Repository Organization

```
Repository Structure:
├── .github/
│   ├── workflows/         # CI/CD pipelines
│   ├── ISSUE_TEMPLATE/    # Issue templates
│   └── pull_request_template.md
├── src/                   # Source code
├── tests/                 # Test files
├── docs/                  # Documentation
├── .gitignore
├── README.md
├── LICENSE
└── package.json
```

### README Template

```markdown
# Project Name

Short description

## Features
- Feature 1
- Feature 2
- Feature 3

## Installation
```bash
git clone repo.git
cd repo
npm install
```

## Usage
```bash
npm start
npm test
npm run build
```

## Configuration
Environment variables:
- VAR1: Description
- VAR2: Description

## Contributing
1. Fork repository
2. Create feature branch
3. Make changes
4. Submit pull request

## License
MIT License

## Support
Contact: email@example.com
```

### Commit Best Practices

```
Conventional Commits:
<type>(<scope>): <subject>

Types:
- feat: New feature
- fix: Bug fix
- docs: Documentation
- style: Formatting
- refactor: Refactoring
- perf: Performance
- test: Tests
- chore: Maintenance
- ci: CI/CD

Examples:
feat(auth): add login functionality
fix(api): resolve endpoint timeout
docs: update installation guide
```

### Issue Labeling

```
Essential Labels:
├── bug (red)
├── enhancement (blue)
├── documentation (purple)
├── good first issue (green)
├── help wanted (orange)
├── urgent (red)
├── blocked (red)
└── in progress (yellow)
```

---

## 12. Troubleshooting

### Common Issues

```bash
# Issue: Cannot push to repository
# Solution:
git pull origin main
git push origin main

# Issue: SSH authentication fails
# Solution:
ssh -T git@github.com      # Test connection
ssh-keygen -t rsa -b 4096  # Generate new key
# Add to GitHub Settings

# Issue: Large file rejection
# Solution:
# Use Git LFS for large files
git lfs install
git lfs track "*.zip"
git add .gitattributes
git add large-file.zip
git commit -m "Add large file"
git push

# Issue: Merge conflicts
# Solution:
git status                 # See conflicted files
# Edit files to resolve
git add resolved-files
git commit -m "Resolve conflicts"

# Issue: Accidental commit
# Solution:
git reset --soft HEAD~1    # Undo commit, keep changes
git reset --hard HEAD~1    # Undo, discard changes
```

### Workflow Debugging

```bash
# Check workflow status
gh workflow list
gh run list
gh run view <run-id>

# View workflow logs
gh run view <run-id> --log

# Rerun failed workflow
gh run rerun <run-id>

# Cancel workflow
gh run cancel <run-id>

# Enable debug logging
# Add to workflow:
- name: Enable debug mode
  run: |
    echo "RUNNER_DEBUG=1" >> $GITHUB_ENV
```

---

## Quick Reference

```bash
# Setup
gh auth login                          # Login to GitHub
git clone https://github.com/user/repo  # Clone repo
git config user.name "Name"            # Configure git

# Issues & PRs
gh issue create                        # Create issue
gh pr create                           # Create PR
gh pr list                             # List PRs
gh pr review <pr>                      # Review PR
gh pr merge <pr>                       # Merge PR

# Workflows
gh workflow list                       # List workflows
gh run list                            # List runs
gh run view <run-id>                   # View run

# Collaborators
gh repo collaborators list             # List collaborators
gh repo collaborators add username     # Add collaborator

# Releases
gh release create v1.0.0               # Create release
gh release list                        # List releases
gh release delete v1.0.0               # Delete release

# Secrets
gh secret set SECRET_NAME              # Add secret
gh secret list                         # List secrets
gh secret delete SECRET_NAME           # Delete secret
```

---

## Resources

| Resource | URL |
|----------|-----|
| **Official Docs** | https://docs.github.com |
| **GitHub Guides** | https://guides.github.com |
| **GitHub CLI** | https://cli.github.com |
| **Actions Marketplace** | https://github.com/marketplace |
| **API Documentation** | https://docs.github.com/rest |
| **Community Forum** | https://github.community |

---

**Last Updated:** 2026
**GitHub Product:** Latest
**GitHub CLI Version:** 2.x+

For latest information, visit [GitHub Official Documentation](https://docs.github.com)