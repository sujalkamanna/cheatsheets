# Bitbucket Cheatsheet

![Bitbucket Logo](https://wac-cdn.atlassian.com/dam/jcr:e348b920-caf0-4b5a-aab5-eae480af6628/Bitbucket@2x.png)

---

## Table of Contents
1. [What is Bitbucket?](#1-what-is-bitbucket)
2. [Installation & Setup](#2-installation--setup)
3. [Repository Management](#3-repository-management)
4. [Branching Strategy](#4-branching-strategy)
5. [Pull Requests](#5-pull-requests)
6. [Merging & Conflicts](#6-merging--conflicts)
7. [Bitbucket Pipelines](#7-bitbucket-pipelines)
8. [Access Control & Permissions](#8-access-control--permissions)
9. [Webhooks & Integrations](#9-webhooks--integrations)
10. [Best Practices](#10-best-practices)
11. [Advanced Features](#11-advanced-features)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. What is Bitbucket?

Bitbucket is a Git repository management and collaboration platform by Atlassian. It provides version control, source code management, and integrations with CI/CD tools for development teams.

**Key Features:**
- Git & Mercurial repository hosting
- Pull request workflow
- Code review tools
- Branch permissions
- Bitbucket Pipelines (CI/CD)
- Jira integration
- Team collaboration
- API access
- Repository webhooks
- Branch restrictions

---

## 2. Installation & Setup

### Git Installation

```bash
# macOS with Homebrew
brew install git

# Ubuntu/Debian
sudo apt-get update
sudo apt-get install git

# CentOS/RHEL
sudo yum install git

# Windows
# Download from https://git-scm.com/download/win

# Verify installation
git --version
```

### Git Configuration

```bash
# Set global configuration
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default editor
git config --global core.editor "vim"

# Set default branch name
git config --global init.defaultBranch main

# View configuration
git config --list
git config --global --list

# Configure per repository
cd /path/to/repo
git config user.name "Project Name"
git config user.email "project@example.com"

# Store credentials (not recommended)
git config --global credential.helper store

# Use SSH keys (recommended)
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -N ""
ssh-add ~/.ssh/id_rsa
```

### SSH Key Setup

```bash
# Generate SSH key
ssh-keygen -t rsa -b 4096 -C "your.email@example.com" -f ~/.ssh/bitbucket_rsa

# Add to SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/bitbucket_rsa

# Copy public key
cat ~/.ssh/bitbucket_rsa.pub

# Add to Bitbucket
# Bitbucket Settings → Personal Settings → SSH keys → Add key

# Test connection
ssh -T git@bitbucket.org
```

---

## 3. Repository Management

### Clone Repository

```bash
# Clone via HTTPS
git clone https://bitbucket.org/username/repository.git

# Clone via SSH
git clone git@bitbucket.org:username/repository.git

# Clone to specific directory
git clone https://bitbucket.org/username/repository.git my-repo

# Clone specific branch
git clone -b develop https://bitbucket.org/username/repository.git

# Shallow clone (limited history)
git clone --depth 1 https://bitbucket.org/username/repository.git

# Clone with submodules
git clone --recurse-submodules https://bitbucket.org/username/repository.git
```

### Create Repository

```bash
# Initialize local repository
git init

# Add remote origin
git remote add origin git@bitbucket.org:username/repository.git

# Create initial commit
git add .
git commit -m "Initial commit"

# Push to remote
git branch -M main
git push -u origin main

# Via Bitbucket UI
# 1. Click "+" icon
# 2. Select "Create repository"
# 3. Fill details and click "Create repository"
```

### Remote Management

```bash
# View remotes
git remote -v
git remote show origin

# Add remote
git remote add origin https://bitbucket.org/username/repository.git

# Remove remote
git remote remove origin

# Rename remote
git remote rename origin upstream

# Change remote URL
git remote set-url origin https://bitbucket.org/new/repository.git

# Set remote branch tracking
git branch -u origin/main
git branch --set-upstream-to=origin/main main

# Pull latest changes
git pull origin main
git fetch origin
```

---

## 4. Branching Strategy

### Branch Management

```bash
# List branches
git branch                          # Local branches
git branch -a                       # All branches
git branch -r                       # Remote branches

# Create branch
git branch feature/login-page
git checkout -b feature/login-page  # Create and switch

# Switch branch
git checkout main
git switch main                     # Newer syntax

# Create from remote branch
git checkout --track origin/develop
git checkout -b develop origin/develop

# Delete branch
git branch -d feature/login-page    # Safe delete (merged)
git branch -D feature/login-page    # Force delete

# Delete remote branch
git push origin --delete feature/login-page

# Rename branch
git branch -m old-name new-name
git branch -m new-name              # Rename current branch

# List branch info
git branch -v                       # With latest commit
git branch -vv                      # With tracking info
```

### Git Flow Branching

```
main (production)
  ↑
  ├─ release/1.0.0 (release preparation)
  │   ↓
  ├─ hotfix/bug-fix (production bugs)
  │   ↓
develop (integration)
  ↑
  ├─ feature/feature-name (feature development)
  ├─ bugfix/bug-fix (bug fixes)
  └─ chore/maintenance (maintenance)
```

### Branching Workflow

```bash
# Feature branch workflow
git checkout -b feature/user-authentication develop
# Make changes
git add .
git commit -m "Add user authentication"
git push -u origin feature/user-authentication
# Create Pull Request in Bitbucket

# Bugfix workflow
git checkout -b bugfix/login-redirect develop
# Fix bug
git add .
git commit -m "Fix login redirect issue"
git push -u origin bugfix/login-redirect

# Release preparation
git checkout -b release/1.0.0 develop
git commit -m "Bump version to 1.0.0"
git push -u origin release/1.0.0

# Hotfix for production
git checkout -b hotfix/critical-bug main
# Fix critical bug
git add .
git commit -m "Fix critical production bug"
git push -u origin hotfix/critical-bug
```

---

## 5. Pull Requests

### Create Pull Request

```bash
# Push feature branch
git push -u origin feature/user-auth

# Create via CLI (if using hub or gh)
hub pull-request -b develop -h feature/user-auth -m "Add user authentication"

# Create via Bitbucket UI
# 1. Go to Pull requests tab
# 2. Click "Create pull request"
# 3. Select source and destination branches
# 4. Add title and description
# 5. Click "Create pull request"
```

### Pull Request Template

```markdown
<!-- bitbucket/.bitbucket/PULL_REQUEST_TEMPLATE.md -->

## Description
Brief description of changes

## Type of change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Related Issues
Closes #123

## Changes Made
- Change 1
- Change 2
- Change 3

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests passed
- [ ] Manual testing completed

## Screenshots/Videos (if applicable)
[Add screenshots or videos]

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex logic
- [ ] Documentation updated
- [ ] No new warnings generated
- [ ] Tests pass locally

## Reviewers
@reviewer1 @reviewer2
```

### Pull Request Review

```bash
# Review locally
git fetch origin
git checkout feature/user-auth

# Test changes
npm install
npm test
npm run build

# Approve changes
# Bitbucket UI → Approve button

# Request changes
# Bitbucket UI → Request changes with comment

# Comment on PR
# Click line number → Add comment

# View diff
git diff main feature/user-auth

# View commits
git log main..feature/user-auth
git log --oneline main..feature/user-auth
```

### Merge Pull Request

```bash
# Merge via Bitbucket UI
# 1. Click "Merge" button
# 2. Choose merge strategy
# 3. Delete source branch if needed
# 4. Click "Merge"

# Merge strategies
# - Merge commit (default)
# - Squash (single commit)
# - Rebase (linear history)

# Merge locally
git checkout develop
git pull origin develop
git merge feature/user-auth
git push origin develop

# Delete merged branch
git branch -d feature/user-auth
git push origin --delete feature/user-auth
```

---

## 6. Merging & Conflicts

### Merge Operations

```bash
# Merge branch
git merge feature/user-auth

# Merge with commit message
git merge --no-ff feature/user-auth -m "Merge user auth feature"

# Merge specific commits
git merge --no-commit feature/user-auth  # Prepare merge without committing

# Merge specific commit
git cherry-pick commit-hash

# Abort merge
git merge --abort
```

### Conflict Resolution

```bash
# View conflicts
git status                          # Shows conflicted files
git diff                            # Shows all conflicts

# Resolve conflicts manually
# 1. Open conflicted file
# 2. Find conflict markers (<<<<<<, ======, >>>>>>)
# 3. Edit file to resolve
# 4. Save file

# Example conflict
<<<<<<< HEAD (current branch)
Feature from current branch
=======
Feature from merging branch
>>>>>>> feature/user-auth

# Mark as resolved
git add conflicted-file.txt

# Complete merge
git commit -m "Merge feature/user-auth"

# Use tool to resolve
git merge tool                      # Opens merge tool
git mergetool                       # Launch configured tool

# Accept incoming/current changes
git checkout --theirs conflicted-file.txt   # From feature branch
git checkout --ours conflicted-file.txt     # From current branch

# Resolve all with strategy
git merge -X ours feature/user-auth         # Prefer current changes
git merge -X theirs feature/user-auth       # Prefer incoming changes
```

### Rebase Operations

```bash
# Rebase branch onto main
git rebase main
git rebase main feature/user-auth

# Interactive rebase
git rebase -i main                          # Rebase last commits
git rebase -i HEAD~3                        # Rebase last 3 commits

# Rebase commands
# p (pick) - use commit
# r (reword) - use commit but edit message
# s (squash) - use commit but squash into previous
# f (fixup) - like squash but discard message
# d (drop) - remove commit
# x (exec) - run command

# Abort rebase
git rebase --abort

# Continue after conflict
git add .
git rebase --continue

# Force push after rebase (be careful!)
git push origin feature/user-auth --force-with-lease
```

---

## 7. Bitbucket Pipelines

### Pipelines Configuration

```yaml
# bitbucket-pipelines.yml
image: atlassian/default-image:latest

definitions:
  steps:
    - step: &build-test
        name: Build and Test
        script:
          - apt-get update && apt-get install -y python3
          - python3 --version
          - python3 -m pip install -r requirements.txt
          - python3 -m pytest tests/
          - echo "Tests passed!"
        artifacts:
          - build/**

    - step: &deploy-staging
        name: Deploy to Staging
        script:
          - echo "Deploying to staging..."
          - ./scripts/deploy.sh staging
        trigger: manual

    - step: &deploy-production
        name: Deploy to Production
        script:
          - echo "Deploying to production..."
          - ./scripts/deploy.sh production
        trigger: manual

pipelines:
  default:
    - step: *build-test

  branches:
    main:
      - step: *build-test
      - step: *deploy-staging
      - step: *deploy-production

    develop:
      - step: *build-test

    feature/*:
      - step: *build-test

  pull-requests:
    '**':
      - step:
          name: Code Review Checks
          script:
            - python3 -m pip install pylint
            - pylint src/
            - python3 -m pytest tests/ --cov=src

  custom:
    manual-test:
      - step:
          name: Run All Tests
          script:
            - python3 -m pytest tests/ -v
```

### Node.js Pipeline Example

```yaml
# bitbucket-pipelines.yml for Node.js
image: node:18

definitions:
  steps:
    - step: &build-test
        name: Build and Test
        script:
          - npm install
          - npm run build
          - npm test
          - npm run lint
        artifacts:
          - dist/**
          - coverage/**

    - step: &deploy-prod
        name: Deploy to Production
        script:
          - npm install -g aws-cli
          - aws s3 sync dist/ s3://my-bucket/
          - aws cloudfront create-invalidation --distribution-id E1234567 --paths "/*"
        trigger: manual

pipelines:
  default:
    - step: *build-test

  branches:
    main:
      - step: *build-test
      - step: *deploy-prod

  pull-requests:
    '**':
      - step:
          name: Code Quality
          script:
            - npm install
            - npm run lint
            - npm test
```

### Docker Pipeline Example

```yaml
# bitbucket-pipelines.yml with Docker
image: atlassian/default-image:latest

definitions:
  steps:
    - step: &build-docker
        name: Build Docker Image
        script:
          - docker build -t $DOCKER_REGISTRY_HOST/$BITBUCKET_REPO_SLUG:$BITBUCKET_COMMIT .
          - docker push $DOCKER_REGISTRY_HOST/$BITBUCKET_REPO_SLUG:$BITBUCKET_COMMIT
        services:
          - docker

    - step: &deploy-k8s
        name: Deploy to Kubernetes
        script:
          - apt-get update && apt-get install -y kubectl
          - kubectl set image deployment/myapp myapp=$DOCKER_REGISTRY_HOST/$BITBUCKET_REPO_SLUG:$BITBUCKET_COMMIT
        trigger: manual

pipelines:
  branches:
    main:
      - step: *build-docker
      - step: *deploy-k8s
```

### Environment Variables in Pipelines

```bash
# Built-in variables
$BITBUCKET_REPO_SLUG          # Repository name
$BITBUCKET_REPO_FULL_SLUG     # Full repository path
$BITBUCKET_COMMIT             # Current commit hash
$BITBUCKET_BRANCH             # Current branch
$BITBUCKET_PULL_REQUEST_ID    # PR number
$BITBUCKET_BUILD_NUMBER       # Build number

# Custom environment variables
# Set in Bitbucket UI: Repository → Settings → Repository variables

# Secure variables (encrypted)
# Set in Bitbucket UI: Repository → Settings → Secure variables

# Access in pipeline
script:
  - echo $DOCKER_USERNAME
  - echo $DATABASE_URL
```

---

## 8. Access Control & Permissions

### Repository Permissions

```
Permission Level    | Capabilities
─────────────────────────────────────────────
None               | No access
Read (Viewer)      | Clone, read
Write (Contributor)| Clone, push, create PR
Admin (Maintainer) | Full access, settings
```

### Branch Permissions

```
# Bitbucket UI: Repository → Settings → Branch permissions

Types of permissions:
- Require approval
- Require all approvals
- Require status checks
- Restrict to specific users
- Require pull request
- Prevent deletion
- Prevent force push
```

### Managing Users

```bash
# Add user to repository
# Bitbucket UI: Repository → Settings → User and group access

# Grant permissions
# 1. Repository → Settings → User and group access
# 2. Click "Grant access"
# 3. Select user/group
# 4. Choose permission level
# 5. Click "Grant"

# Remove access
# 1. Find user in access list
# 2. Click "X" to remove

# Bulk permissions
# 1. Repository → Settings → Default reviewers
# 2. Add users as default reviewers
```

### Bitbucket Project Roles

```
Role           | Permissions
────────────────────────────────────────
Member         | View, create repos
Contributor    | + Push, create PR
Maintainer     | + Merge, delete branch
Admin          | + Settings, manage users
```

---

## 9. Webhooks & Integrations

### Webhook Configuration

```bash
# Create webhook
# Bitbucket UI: Repository → Settings → Webhooks

# Webhook types
- Repository push
- Pull request created
- Pull request updated
- Pull request merged
- Pull request declined
- Comment created
- Commit comment created
```

### Webhook Payload Example

```json
{
  "eventKey": "repo:push",
  "date": "2024-01-15T10:30:00Z",
  "actor": {
    "name": "John Doe",
    "emailAddress": "john@example.com",
    "id": 1,
    "slug": "johndoe",
    "links": {
      "self": [
        {
          "href": "https://bitbucket.org/johndoe"
        }
      ]
    }
  },
  "repository": {
    "slug": "my-repo",
    "id": 1,
    "name": "my-repo",
    "project": {
      "key": "PROJ",
      "id": 1,
      "name": "My Project",
      "slug": "my-project"
    }
  },
  "push": {
    "changes": [
      {
        "new": {
          "name": "main",
          "rawNode": "abc123def456",
          "type": "branch"
        },
        "old": {
          "name": "main",
          "rawNode": "xyz789abc123",
          "type": "branch"
        },
        "created": false,
        "closed": false,
        "forced": false,
        "commits": [
          {
            "hash": "abc123def456",
            "type": "commit",
            "links": {
              "self": [
                {
                  "href": "https://bitbucket.org/my-repo/commits/abc123def456"
                }
              ]
            }
          }
        ]
      }
    ]
  }
}
```

### Integrations

```yaml
# Popular Bitbucket Integrations

# Jira Integration
# Repository → Settings → Integration → JIRA
# Link issues automatically in commits

# Slack Integration
# Repository → Settings → Webhooks
# Send notifications to Slack channel

# Jenkins Integration
# Repository → Settings → Integration → Jenkins
# Trigger builds on push

# GitHub Actions
# Use Bitbucket to GitHub sync
# Enable Actions on GitHub repo

# CloudBuild Integration
# Connect Google Cloud Build
# Automatic builds on push
```

### Slack Webhook Example

```bash
# Create Slack webhook
# 1. Slack App → Incoming Webhooks
# 2. Create webhook URL
# 3. Copy webhook URL

# Add to Bitbucket
# Repository → Settings → Webhooks
# URL: https://hooks.slack.com/services/YOUR/WEBHOOK/URL
```

---

## 10. Best Practices

### Commit Message Guidelines

```
Format: <type>(<scope>): <subject>

Types:
- feat: new feature
- fix: bug fix
- docs: documentation
- style: code style
- refactor: refactoring
- perf: performance
- test: testing
- chore: maintenance

Examples:
feat(auth): add two-factor authentication
fix(login): resolve session timeout issue
docs: update installation guide
refactor(database): optimize query performance
```

### Commit Best Practices

```bash
# Make small, focused commits
git add src/specific_file.js
git commit -m "feat(auth): add login validation"

# Avoid large commits
# Wrong: Too many changes
git add .
git commit -m "Update everything"

# Right: Focused changes
git add src/user/login.js
git commit -m "feat(auth): add password validation"

# Review before committing
git diff --staged                       # Review staged changes
git status                              # Check status

# Amend last commit
git commit --amend                      # Edit message
git commit --amend --no-edit            # Add forgotten changes

# Interactive staging
git add -p                              # Stage by hunk
```

### Code Review Practices

```markdown
## Review Checklist

### Code Quality
- [ ] Code is readable and maintainable
- [ ] Follows project coding standards
- [ ] No code duplication
- [ ] Proper error handling

### Testing
- [ ] Unit tests added/updated
- [ ] Tests are passing
- [ ] Edge cases covered
- [ ] Integration tests pass

### Performance
- [ ] No performance regressions
- [ ] Efficient algorithms used
- [ ] Database queries optimized

### Security
- [ ] No security vulnerabilities
- [ ] Input validation present
- [ ] Secrets not committed
- [ ] Dependencies secure

### Documentation
- [ ] Code commented where needed
- [ ] README updated if necessary
- [ ] API documentation current
- [ ] Changelog updated
```

### Branch Naming Convention

```
Convention: <type>/<description>

Types:
- feature/        : New features
- bugfix/         : Bug fixes
- hotfix/         : Production hotfixes
- release/        : Release preparation
- chore/          : Maintenance tasks
- docs/           : Documentation

Examples:
feature/user-authentication
bugfix/login-redirect
hotfix/critical-security-patch
release/1.0.0
chore/update-dependencies
docs/api-documentation
```

---

## 11. Advanced Features

### Code Owners

```bash
# Create CODEOWNERS file
# .bitbucket/CODEOWNERS

# Frontend team owns all .js files
*.js @frontend-team

# Backend team owns API routes
src/api/ @backend-team

# DevOps team owns infrastructure
infrastructure/ @devops-team

# Specific files
docker-compose.yml @devops-team
.github/workflows/ @ci-team
```

### Branch Policies

```yaml
# Required branch protections (via Bitbucket UI)

# Main branch protections
- Require at least 2 approvals
- Require all status checks pass
- Dismiss stale reviews
- Require review from code owners
- Require branches to be up to date
- Include administrators
- Restrict who can push
- Restrict who can delete
```

### Repository Insights

```bash
# View analytics
# Bitbucket UI: Repository → Insights

Available metrics:
- Contributors
- Commits
- Pull requests
- Branches
- Forks
- Watchers
- Clone statistics
- Activity graph
```

---

## 12. Troubleshooting

### Common Issues

```bash
# Issue: Authentication failed
# Solution: Verify SSH key or credentials
ssh -T git@bitbucket.org
git config --list | grep credential

# Issue: Remote branch not showing
# Solution: Fetch latest changes
git fetch origin
git branch -r

# Issue: Cannot push (rejected)
# Solution: Pull latest changes first
git pull origin main --rebase
git push origin main

# Issue: Accidental commit on wrong branch
# Solution: Move commits to correct branch
git log main..feature/wrong              # See commits
git cherry-pick commit-hash              # Copy to correct branch
git reset --hard origin/wrong            # Reset wrong branch

# Issue: Large files slow down clone
# Solution: Use Git LFS (Large File Storage)
git lfs install
git lfs track "*.zip"
git add .gitattributes
git add large-file.zip
git commit -m "Add large file with LFS"
git push

# Issue: Merge conflicts
# Solution: Resolve conflicts manually
git status                               # See conflicted files
# Edit files to resolve
git add resolved-file.txt
git commit -m "Resolve merge conflicts"

# Issue: Lost commits
# Solution: Use reflog to recover
git reflog                               # See all commits
git reset --hard commit-hash             # Restore to commit
```

### Performance Optimization

```bash
# Optimize repository
git gc                                  # Garbage collection
git gc --aggressive                     # Aggressive optimization

# Large repository handling
git clone --depth 1 repo.git             # Shallow clone
git fetch --depth=1 origin main          # Shallow fetch

# Remove large files from history
# Use BFG Repo-Cleaner
bfg --delete-files large-file.zip
git reflog expire --expire=now --all
git gc --prune=now --aggressive
git push origin main --force-with-lease

# Cache credentials securely
git config --global credential.helper osxkeychain  # macOS
git config --global credential.helper wincred      # Windows
git config --global credential.helper pass         # Linux
```

### Debug Modes

```bash
# Enable debug logging
GIT_TRACE=1 git clone repo.git
GIT_TRACE_PERFORMANCE=1 git push

# SSH debugging
ssh -vvv git@bitbucket.org

# Test Bitbucket connection
ssh -T git@bitbucket.org

# Show git configuration
git config --list --show-origin

# Verbose push
git push -v origin main
git push -vv origin main
```

---

## Quick Reference Commands

```bash
# Setup & Configuration
git init                                # Initialize repo
git config --global user.name "Name"    # Set username
git clone <url>                         # Clone repo
git remote add origin <url>             # Add remote

# Branching
git branch                              # List branches
git branch <name>                       # Create branch
git checkout -b <name>                  # Create and switch
git switch <branch>                     # Switch branch
git branch -d <name>                    # Delete branch

# Staging & Committing
git status                              # Check status
git add <file>                          # Stage file
git add .                               # Stage all
git commit -m "message"                 # Commit
git push origin <branch>                # Push to remote

# Pulling & Merging
git pull origin <branch>                # Pull and merge
git fetch origin                        # Fetch updates
git merge <branch>                      # Merge branch
git rebase <branch>                     # Rebase

# Pull Requests
git push -u origin <branch>             # Create PR
# Then create PR in Bitbucket UI

# History
git log                                 # Show commits
git log --oneline                       # Compact log
git diff                                # Show changes
git show <commit>                       # Show commit

# Undoing
git reset HEAD~1                        # Undo commit
git revert <commit>                     # Create undo commit
git checkout -- <file>                  # Discard changes
```

---

## Resources

| Resource | URL |
|----------|-----|
| **Bitbucket Documentation** | https://bitbucket.org/product/guides |
| **Git Documentation** | https://git-scm.com/doc |
| **Bitbucket Cloud REST API** | https://developer.atlassian.com/cloud/bitbucket/rest/api-group-repositories/ |
| **Git Workflows** | https://www.atlassian.com/git/tutorials/comparing-workflows |
| **Jira Integration** | https://bitbucket.org/product/features/integrations/jira |
| **Community** | https://community.atlassian.com/t5/Bitbucket/ct-p/bitbucket |

---

**Last Updated:** 2024
**Git Version:** 2.40+
**Bitbucket Cloud**

For latest information, visit [Bitbucket Official Documentation](https://bitbucket.org/product/guides)