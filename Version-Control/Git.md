# Git Cheatsheet

![Git Logo](https://git-scm.com/images/logos/downloads/Git-Icon-1788C.png)

---

## Table of Contents
1. [What is Git?](#1-what-is-git)
2. [Installation & Configuration](#2-installation--configuration)
3. [Basic Commands](#3-basic-commands)
4. [Branching](#4-branching)
5. [Merging & Rebasing](#5-merging--rebasing)
6. [Stashing & Undoing](#6-stashing--undoing)
7. [Remote Operations](#7-remote-operations)
8. [History & Inspection](#8-history--inspection)
9. [Advanced Workflows](#9-advanced-workflows)
10. [Git Hooks](#10-git-hooks)
11. [Best Practices](#11-best-practices)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. What is Git?

Git is a distributed version control system designed for tracking changes in source code during software development. It enables collaboration, manages project history, and allows multiple developers to work simultaneously.

**Key Features:**
- Distributed architecture
- Fast performance
- Branching & merging
- Complete history tracking
- Staging area
- Integrity checking
- Offline capabilities
- Collaboration tools
- Tag management
- Workflow flexibility

---

## 2. Installation & Configuration

### Install Git

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

### Initial Configuration

```bash
# Set global user configuration
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default editor
git config --global core.editor "vim"

# Set default branch name
git config --global init.defaultBranch main

# View configuration
git config --list
git config --global --list
git config --global user.name  # View specific setting

# Local configuration (per repository)
cd /path/to/repo
git config user.name "Project Specific Name"
git config user.email "project@example.com"

# Remove configuration
git config --global --unset user.name

# Edit configuration file directly
git config --global --edit
```

### SSH Key Setup

```bash
# Generate SSH key
ssh-keygen -t rsa -b 4096 -C "your.email@example.com"
# Or with custom filename
ssh-keygen -t rsa -b 4096 -C "your.email@example.com" -f ~/.ssh/github_rsa

# Add to SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa

# Add to ssh config for multiple keys
cat > ~/.ssh/config << EOF
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/github_rsa

Host gitlab.com
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/gitlab_rsa
EOF

# Set permissions
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub

# Test connection
ssh -T git@github.com
ssh -T git@gitlab.com
```

### Git Aliases

```bash
# Create command aliases
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual 'log --graph --oneline --all'
git config --global alias.amend 'commit --amend --no-edit'

# Usage
git co main          # Instead of git checkout main
git br -a            # Instead of git branch -a
git st               # Instead of git status
git visual           # Instead of git log --graph --oneline --all
```

---

## 3. Basic Commands

### Initialize Repository

```bash
# Initialize new repository
git init

# Initialize with default branch name
git init -b main

# Clone existing repository
git clone https://github.com/user/repository.git
git clone git@github.com:user/repository.git
git clone https://github.com/user/repository.git my-folder  # Custom folder
git clone --depth 1 https://github.com/user/repository.git   # Shallow clone
git clone --bare https://github.com/user/repository.git      # Bare repository
```

### Status & Inspection

```bash
# Check repository status
git status
git status -s        # Short format
git status -v        # Verbose

# Show working tree changes
git diff             # Unstaged changes
git diff --staged    # Staged changes
git diff HEAD        # All changes vs latest commit
git diff main feature/login  # Compare branches

# Show specific file changes
git diff README.md
git diff --stat      # Show statistics only
```

### Staging & Committing

```bash
# Stage files
git add file.txt     # Stage specific file
git add .            # Stage all changes
git add src/         # Stage directory
git add -A           # Stage all (including deletions)
git add -p           # Interactive staging (by hunk)

# Unstage files
git reset HEAD file.txt    # Unstage specific file
git reset HEAD~1           # Undo last commit (keep changes)

# Commit changes
git commit -m "Commit message"
git commit -m "Title" -m "Description"
git commit --amend           # Modify last commit
git commit --amend --no-edit # Add to last commit without changing message
git commit -am "Message"     # Stage and commit tracked files

# Commit with signing
git commit -S -m "Signed commit"
```

### File Management

```bash
# Remove files
git rm file.txt           # Remove and stage deletion
git rm --cached file.txt  # Remove from staging only
git rm -r directory/      # Remove directory

# Move/Rename files
git mv old_name.txt new_name.txt
git mv file.txt path/to/file.txt

# Check file status
git ls-files           # List tracked files
git ls-files --others  # List untracked files
git status -u          # Show untracked files
```

---

## 4. Branching

### Branch Operations

```bash
# List branches
git branch                    # Local branches
git branch -a                 # All branches (local + remote)
git branch -r                 # Remote branches only
git branch -v                 # With last commit info
git branch -vv                # With tracking info

# Create branch
git branch feature/login      # Create locally
git branch -b feature/login   # Create and switch (shorthand)

# Switch branch
git checkout feature/login    # Traditional method
git switch feature/login      # Newer syntax (Git 2.23+)
git checkout -b feature/login # Create and switch

# Create from remote branch
git checkout --track origin/develop
git checkout -b develop origin/develop
git switch -c develop origin/develop

# Rename branch
git branch -m old-name new-name      # Rename current
git branch -m feature/old feature/new # Rename specific

# Delete branch
git branch -d feature/login   # Safe delete (merged only)
git branch -D feature/login   # Force delete
git push origin --delete feature/login  # Delete remote branch

# Delete all local branches except main
git branch | grep -v "main" | xargs git branch -d

# Set upstream branch
git branch -u origin/main
git branch --set-upstream-to=origin/main
git push -u origin feature/login
```

### Branch Information

```bash
# Show merged branches
git branch --merged        # Merged into current branch
git branch -r --merged     # Remote branches

# Show unmerged branches
git branch --no-merged

# List branches with creation date
git branch -v
git for-each-ref --sort=-creatordate --format='%(refname:short) %(creatordate:short)' refs/heads

# Show branch protections
git config --get-all branch.main.merge
```

---

## 5. Merging & Rebasing

### Merge Operations

```bash
# Merge branch
git merge feature/login       # Merge into current branch
git merge --no-ff feature/login  # Create merge commit
git merge --squash feature/login # Squash before merge
git merge --no-commit feature/login  # Prepare merge without committing

# Merge specific commit
git cherry-pick commit-hash

# Abort merge
git merge --abort

# View merge status
git status         # Shows merge conflicts
git diff           # Shows unmerged paths
```

### Conflict Resolution

```bash
# View conflicts
git status
git diff
git diff --ours    # Show current branch changes
git diff --theirs  # Show incoming changes

# Resolve manually
# 1. Open conflicted file
# 2. Find conflict markers: <<<<<<< ======= >>>>>>>
# 3. Edit to resolve
# 4. Save file

# Mark as resolved
git add resolved-file.txt

# Complete merge
git commit -m "Resolve merge conflicts"

# Use merge tool
git mergetool      # Launch configured tool
git difftool       # View differences with tool

# Accept changes
git checkout --ours file.txt     # Keep current branch
git checkout --theirs file.txt   # Accept incoming changes

# Resolve with strategy
git merge -X ours feature/login     # Prefer current changes
git merge -X theirs feature/login   # Prefer incoming changes
```

### Rebase Operations

```bash
# Rebase onto another branch
git rebase main
git rebase main feature/login

# Interactive rebase
git rebase -i main                   # Rebase onto main
git rebase -i HEAD~3                 # Last 3 commits
git rebase -i --root                 # All commits

# Interactive rebase commands
# p (pick)   - use commit
# r (reword) - edit commit message
# s (squash) - combine with previous
# f (fixup)  - like squash but discard message
# d (drop)   - remove commit
# x (exec)   - run shell command
# b (break)  - pause rebasing
# m (merge)  - create merge commit

# Continue after conflict
git rebase --continue

# Abort rebase
git rebase --abort

# Skip problematic commit
git rebase --skip

# Force push after rebase (use with care!)
git push origin feature/login --force-with-lease
git push --force-with-lease origin feature/login
```

### Rebase vs Merge

```
# Merge (keeps history)
git merge feature/login

# Creates: linear history with merge commit
main ─────────────────── merge commit
       │                 │
       └─ feature commits ─┘

# Rebase (clean linear history)
git rebase main
git merge --ff main

# Creates: linear history without merge commit
main ──── feature commits (replayed)
```

---

## 6. Stashing & Undoing

### Stashing Work

```bash
# Stash changes
git stash              # Stash all changes
git stash save "message"  # With message
git stash -u           # Include untracked files
git stash -k           # Keep staged changes

# List stashes
git stash list
git stash list --stat

# View stash content
git stash show
git stash show stash@{0}
git stash show -p      # Show full diff

# Apply stash
git stash apply        # Apply latest stash
git stash apply stash@{0}
git stash pop          # Apply and remove

# Delete stash
git stash drop stash@{0}  # Delete specific
git stash clear           # Delete all

# Create branch from stash
git stash branch new-branch
git stash branch new-branch stash@{0}
```

### Undoing Changes

```bash
# Discard uncommitted changes
git checkout -- file.txt           # Discard in working tree
git restore file.txt               # Newer syntax
git restore --staged file.txt      # Unstage file

# Undo last commit (keep changes)
git reset --soft HEAD~1            # Undo, keep staged
git reset --mixed HEAD~1           # Undo, keep untracked
git reset --hard HEAD~1            # Undo, discard all

# Undo specific file in commit
git checkout HEAD -- file.txt

# Create undo commit (safe)
git revert commit-hash

# Reset to specific commit
git reset --hard commit-hash       # Dangerous!
git reset --hard origin/main       # Discard local changes

# Reset to previous state
git reflog              # Show all previous states
git reset --hard HEAD@{1}  # Reset to previous state

# Amend last commit
git commit --amend
git commit --amend --no-edit

# Fix previous commits
git rebase -i commit-hash^
# Mark commit with 'r' (reword) or 'e' (edit)
```

---

## 7. Remote Operations

### Remote Management

```bash
# View remotes
git remote
git remote -v          # Verbose (shows URLs)
git remote show origin # Show details

# Add remote
git remote add origin https://github.com/user/repo.git
git remote add upstream https://github.com/original/repo.git

# Remove remote
git remote remove origin

# Rename remote
git remote rename origin upstream

# Change remote URL
git remote set-url origin https://github.com/new/repo.git
git remote set-url --push origin git@github.com:user/repo.git

# View remote tracking
git branch -r
git branch -vv
```

### Fetch & Pull

```bash
# Fetch latest changes (no merge)
git fetch              # Fetch from all remotes
git fetch origin       # Fetch from specific remote
git fetch origin main  # Fetch specific branch

# Pull (fetch + merge)
git pull               # Pull from default remote
git pull origin main   # Pull from specific remote/branch
git pull --rebase      # Fetch + rebase instead of merge

# Update all branches
git fetch --all
git branch -u origin/main  # Update tracking
```

### Push Operations

```bash
# Push commits
git push               # Push to default remote
git push origin main   # Push specific branch
git push origin feature/login --set-upstream  # Create remote branch

# Push all branches
git push origin --all

# Push with tags
git push origin main --tags
git push origin v1.0.0          # Push specific tag
git push origin 'refs/tags/*'   # Push all tags

# Force push (use with care!)
git push --force               # Force overwrite remote
git push --force-with-lease    # Safer force push

# Delete remote branch
git push origin --delete feature/login
git push origin :feature/login  # Alternative syntax

# Push to multiple remotes
git push origin main
git push upstream main

# Dry-run push
git push --dry-run
git push -n              # Short form
```

---

## 8. History & Inspection

### View Commit History

```bash
# Show commits
git log                       # Full log
git log --oneline            # Compact view
git log --graph --oneline --all  # Visual graph
git log -n 10                # Last 10 commits
git log -p                   # Show changes
git log -p -n 3              # Last 3 commits with changes

# Filter log
git log --author="Name"      # By author
git log --since="2 weeks ago"  # Time range
git log --until="yesterday"
git log --grep="keyword"     # By commit message
git log -S"string"           # By code change

# Show specific commits
git log main..feature/login  # Commits in feature not in main
git log --reverse            # Oldest first
git log --decorate           # Show branches/tags

# Show commit statistics
git log --stat               # Changed files
git log --shortstat
git log --pretty=format:"%h - %an, %ar : %s"
```

### Inspect Commits

```bash
# Show specific commit
git show commit-hash         # Full details
git show commit-hash:file.txt  # File at commit
git show commit-hash^:file.txt # File in parent

# Show changes in commit
git diff commit-hash^..commit-hash
git diff commit-hash~1 commit-hash

# Who changed what
git blame file.txt           # Line-by-line authors
git blame -L 10,20 file.txt  # Specific lines
git blame -e file.txt        # Show email addresses

# Show author statistics
git shortlog -s -n          # Commits per author
git shortlog -sn            # Sorted
```

### Search History

```bash
# Find commits
git log -S "search string"   # Find code changes
git log --grep="message"     # Search messages
git log --author="name"      # By author
git log --since="date"       # Date range
git log -- file.txt          # Affecting file

# Find lost commits
git reflog                   # Reference logs
git reflog show main         # Specific branch
git fsck --lost-found        # Find dangling objects

# Bisect to find breaking commit
git bisect start
git bisect bad HEAD
git bisect good v1.0
git bisect run test-script.sh
git bisect reset
```

---

## 9. Advanced Workflows

### Feature Branch Workflow

```bash
# Create feature branch
git checkout -b feature/user-auth

# Make changes
git add .
git commit -m "feat(auth): add login form"

# Keep branch updated
git fetch origin
git rebase origin/main

# Push to remote
git push -u origin feature/user-auth

# Create pull request on GitHub/GitLab/Bitbucket

# After approval, merge and cleanup
git checkout main
git pull origin main
git merge feature/user-auth
git push origin main
git branch -d feature/user-auth
git push origin --delete feature/user-auth
```

### Forking Workflow

```bash
# Fork on GitHub/GitLab

# Clone your fork
git clone https://github.com/your-username/repository.git
cd repository

# Add upstream
git remote add upstream https://github.com/original-owner/repository.git

# Create feature branch
git checkout -b feature/new-feature

# Make changes and push to your fork
git push origin feature/new-feature

# Create pull request to upstream

# Keep fork updated
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

### Gitflow Workflow

```bash
# Initialize gitflow
git flow init

# Feature work
git flow feature start user-auth
# Make changes
git flow feature publish user-auth
git flow feature finish user-auth

# Release preparation
git flow release start 1.0.0
# Version bumps
git flow release publish 1.0.0
git flow release finish 1.0.0

# Hotfix for production
git flow hotfix start critical-bug
# Fix bug
git flow hotfix finish critical-bug

# Branches created:
# main       - production ready
# develop    - integration
# feature/*  - feature development
# release/*  - release candidates
# hotfix/*   - production patches
```

### Cherry-Pick Workflow

```bash
# Copy commits from another branch
git cherry-pick commit-hash

# Cherry-pick range
git cherry-pick commit1^..commit3

# Cherry-pick with edit
git cherry-pick -e commit-hash
git cherry-pick --edit commit-hash

# Abort cherry-pick
git cherry-pick --abort
```

---

## 10. Git Hooks

### Pre-commit Hook

```bash
# Create pre-commit hook
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/bash

# Run linting
npm run lint
if [ $? -ne 0 ]; then
  echo "Linting failed"
  exit 1
fi

# Run tests
npm test
if [ $? -ne 0 ]; then
  echo "Tests failed"
  exit 1
fi

exit 0
EOF

# Make executable
chmod +x .git/hooks/pre-commit
```

### Commit Message Hook

```bash
# Create commit-msg hook
cat > .git/hooks/commit-msg << 'EOF'
#!/bin/bash

# Enforce commit message format
if ! grep -qE "^(feat|fix|docs|style|refactor|perf|test|chore)(\(.+\))?:" "$1"; then
  echo "Commit message must follow: type(scope): subject"
  exit 1
fi

exit 0
EOF

chmod +x .git/hooks/commit-msg
```

### Using Git Hooks Framework

```bash
# Install husky (Node.js projects)
npm install husky --save-dev
npx husky install

# Add pre-commit hook
npx husky add .husky/pre-commit "npm run lint"
npx husky add .husky/pre-commit "npm test"

# Add commit-msg hook
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
```

---

## 11. Best Practices

### Commit Message Format

```
<type>(<scope>): <subject>

<body>

<footer>

Types:
- feat:     New feature
- fix:      Bug fix
- docs:     Documentation
- style:    Code style (formatting, semicolons, etc)
- refactor: Code refactoring
- perf:     Performance improvement
- test:     Tests
- chore:    Dependencies, build process, etc
- ci:       CI/CD configuration
- revert:   Revert previous commit

Examples:
feat(auth): add two-factor authentication
fix(login): resolve session timeout issue
docs: update API documentation
refactor(database): optimize query performance
perf(cache): implement Redis caching
test(auth): add unit tests for login
```

### Branching Strategy

```
Recommended Branch Structure:
├── main (production-ready)
├── develop (integration)
├── feature/* (new features)
├── bugfix/* (bug fixes)
├── release/* (release candidates)
└── hotfix/* (production patches)

Naming Convention:
- feature/user-authentication
- bugfix/login-redirect
- hotfix/critical-security-patch
- release/1.0.0
```

### Code Review Guidelines

```markdown
## Review Checklist

### Code Quality
- [ ] Follows project style guide
- [ ] No unnecessary complexity
- [ ] DRY principle applied
- [ ] Proper error handling

### Testing
- [ ] Unit tests added
- [ ] Tests are passing
- [ ] Edge cases covered
- [ ] No test flakiness

### Performance
- [ ] No performance regressions
- [ ] Efficient algorithms
- [ ] Database queries optimized
- [ ] Memory leaks avoided

### Security
- [ ] No security vulnerabilities
- [ ] Input validation present
- [ ] Secrets not committed
- [ ] Dependencies secure

### Documentation
- [ ] Code is clear and commented
- [ ] README updated
- [ ] API docs current
- [ ] Changelog updated
```

### Commit Best Practices

```bash
# Make logical, focused commits
git add src/features/login.js
git commit -m "feat(auth): add login validation"

# Avoid large commits
# Wrong: Too many unrelated changes
git add .
git commit -m "Update everything"

# Right: Focused changes
git add src/auth/
git commit -m "feat(auth): add password validation"

# Use interactive staging
git add -p
# Select specific hunks

# Review before committing
git diff --staged
git status
```

---

## 12. Troubleshooting

### Common Issues

```bash
# Issue: Wrong branch commit
# Solution:
git log main..feature/wrong      # See commits
git cherry-pick commit-hash      # Copy to correct branch
git reset --hard origin/wrong    # Reset wrong branch

# Issue: Cannot push (rejected)
# Solution:
git pull origin main --rebase    # Update and rebase
git push origin main

# Issue: Accidentally committed secrets
# Solution:
git filter-branch --tree-filter 'rm -f secret.txt' -- --all
git push origin --force-with-lease --all

# Issue: Merge conflicts
# Solution:
git status                       # See conflicted files
# Edit files to resolve
git add resolved-files
git commit -m "Resolve conflicts"

# Issue: Lost commits
# Solution:
git reflog                       # Show all states
git reset --hard HEAD@{1}        # Restore previous state

# Issue: Detached HEAD
# Solution:
git branch recover-branch        # Create branch from current state
git checkout main                # Return to main
```

### Performance Issues

```bash
# Remove large files from history
git filter-branch --index-filter 'git rm --cached --ignore-unmatch large-file.zip' -- --all
git push origin main --force-with-lease

# Garbage collection
git gc                           # Optimize repository
git gc --aggressive

# Large repository handling
git clone --depth 1 repo.git     # Shallow clone
git fetch --depth=1 origin main  # Shallow fetch

# List large objects
git rev-list --all --objects | sort -k2 | tail -20
```

### Debug Modes

```bash
# Enable verbose output
GIT_TRACE=1 git clone repo.git
GIT_TRACE_PERFORMANCE=1 git push

# SSH debugging
ssh -vvv git@github.com

# Test SSH connection
ssh -T git@github.com

# Show configuration
git config --list --show-origin

# Verbose commands
git push -v
git pull -v
git fetch -v
```

---

## Quick Reference

```bash
# Setup
git init                                  # Initialize repo
git config user.name "Name"               # Configure user
git clone https://github.com/user/repo.git

# Status & Diff
git status                                # Check status
git diff                                  # Show changes
git log                                   # Show history

# Staging & Commit
git add file.txt                          # Stage file
git add .                                 # Stage all
git commit -m "Message"                   # Commit

# Branching
git branch                                # List branches
git checkout -b feature/new               # Create & switch
git switch feature/new                    # Switch (newer)

# Merging
git merge feature/new                     # Merge branch
git rebase main                           # Rebase onto main
git merge --abort                         # Cancel merge

# Remote
git remote -v                             # Show remotes
git fetch origin                          # Fetch changes
git pull origin main                      # Pull & merge
git push origin main                      # Push commits

# Undo
git restore file.txt                      # Discard changes
git reset --soft HEAD~1                   # Undo commit
git revert commit-hash                    # Create undo commit

# Stash
git stash                                 # Save work temporarily
git stash pop                             # Restore work

# History
git log --oneline                         # Compact history
git log --graph --all                     # Visual graph
git show commit-hash                      # Show commit
git blame file.txt                        # Line-by-line authors
```

---

## Resources

| Resource | URL |
|----------|-----|
| **Official Documentation** | https://git-scm.com/doc |
| **Git Book** | https://git-scm.com/book/en/v2 |
| **Interactive Tutorial** | https://learngitbranching.js.org |
| **Git Workflows** | https://www.atlassian.com/git/tutorials |
| **GitHub Guides** | https://guides.github.com |
| **Commit Convention** | https://www.conventionalcommits.org |

---

**Last Updated:** 2026
**Git Version:** 2.40+
**Universal across all platforms**

For latest information, visit [Git Official Documentation](https://git-scm.com/doc)