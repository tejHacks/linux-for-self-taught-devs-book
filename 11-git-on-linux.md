# Chapter 11: Git on Linux

> _"Version control isn't just for teams—it's for anyone who wants to track their own progress."_

## Introduction

Git is the backbone of modern software development. Whether you're collaborating with a team or working solo, Git helps you:

- Track changes over time
- Revert to previous versions
- Create branches for experiments
- Merge changes cleanly

This chapter covers Git setup and workflow on Linux.

## Installing Git

### 11.1 Installation

```bash
# Ubuntu/Debian
sudo apt install git

# Fedora
sudo dnf install git

# Arch
sudo pacman -S git

# Verify
git --version
# Output: git version 2.43.0
```

### 11.2 Initial Configuration

```bash
# Set your identity (required before first commit)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default editor
git config --global core.editor nano

# Enable helpful colors
git config --global color.ui auto

# Set default branch name
git config --global init.defaultBranch main

# View all settings
git config --list
```

## Basic Concepts

### 11.3 Key Terms

| Term           | Definition                           |
| -------------- | ------------------------------------ |
| **Repository** | A project folder tracked by Git      |
| **Commit**     | A snapshot of changes                |
| **Branch**     | A parallel version of code           |
| **Merge**      | Combining branches                   |
| **Remote**     | A server-hosted version of your repo |
| **Clone**      | Copy a remote repo locally           |
| **Push**       | Upload commits to remote             |
| **Pull**       | Download commits from remote         |

### 11.4 The Three States

Files in Git can be in one of three states:

```
Working Directory → Staging Area → Git Directory
     (modified)    →   (staged)   →  (committed)
```

## Basic Workflow

### 11.5 Creating a Repository

```bash
# Create new directory
mkdir myproject
cd myproject

# Initialize repository
git init

# View hidden .git folder
ls -la

# Check status
git status
```

### 11.6 Staging and Committing

```bash
# Create a file
echo "# My Project" > README.md

# Check status
git status
# Output: Untracked files: README.md

# Stage file (add to tracking)
git add README.md

# Or stage all files
git add .

# Commit with message
git commit -m "Initial commit: Added README"

# View log
git log
```

### 11.7 Making Changes

```bash
# Edit file
echo "## Features" >> README.md

# Check status
git status
# Output: Modified files: README.md

# See changes
git diff

# Stage specific file
git add README.md

# Commit
git commit -m "Added features section"

# View history
git log --oneline
```

## Branching and Merging

### 11.8 Working with Branches

```bash
# List branches
git branch

# Create new branch
git branch feature-login

# Switch to branch
git checkout feature-login
# Or in modern Git:
git switch feature-login

# Create and switch in one command
git checkout -b feature-signup
# Or:
git switch -c feature-signup

# Make commits on branch
echo "Login page" > login.html
git add login.html
git commit -m "Added login page"

# Switch back to main
git checkout main
# Or:
git switch main
```

### 11.9 Merging Branches

```bash
# Ensure you're on main
git checkout main

# Merge branch
git merge feature-login

# If there are conflicts, resolve them manually
# Then:
git add .
git commit -m "Resolved merge conflicts"

# Delete branch (after merge)
git branch -d feature-login
```

### 11.10 Handling Merge Conflicts

```bash
# When merge conflicts occur, Git marks them
# In conflicted file:
<<<<<<< HEAD
Current main content
=======
Incoming branch content
>>>>>>> feature-branch

# Resolve manually, then:
git add resolved-file.txt
git commit -m "Resolved merge conflict"
```

## Remote Repositories

### 11.11 Connecting to GitHub/GitLab

```bash
# Clone existing repository
git clone https://github.com/username/repo.git

# Or with SSH
git clone git@github.com:username/repo.git

# Add remote (if not cloned)
git remote add origin https://github.com/username/repo.git

# View remotes
git remote -v
```

### 11.12 Push and Pull

```bash
# Push to remote
git push origin main

# Set upstream (first push only)
git push -u origin main

# Pull changes
git pull origin main

# Fetch without merging
git fetch origin
```

### 11.13 Working with Forks

```bash
# Add original repository as upstream
git remote add upstream https://github.com/original/repo.git

# Fetch from upstream
git fetch upstream

# View all branches
git branch -a

# Create branch from upstream
git checkout -b feature upstream/main
```

## Advanced Git

### 11.14 Stashing Changes

```bash
# Save uncommitted changes
git stash

# List stashes
git stash list

# Apply most recent stash
git stash pop

# Apply specific stash
git stash apply stash@{0}

# Drop stash
git stash drop stash@{0}
```

### 11.15 Rewriting History

```bash
# Amend last commit (change message or add files)
git commit --amend

# Interactive rebase (squash commits, reorder)
git rebase -i HEAD~3

# Reset to previous commit (careful!)
git reset --hard HEAD~1
```

### 11.16 Useful Aliases

```bash
# Add to ~/.gitconfig or use git config
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual 'log --graph --oneline --all'
```

## Git on Linux Specifics

### 11.17 Line Endings

```bash
# Configure line endings (important for cross-platform)
# For Linux (recommended):
git config --global core.autocrlf input

# For Windows:
git config --global core.autocrlf true

# For Mac:
git config --global core.autocrlf input
```

### 11.18 Ignoring Files

```bash
# Create .gitignore
cat > .gitignore << 'EOF'
# Dependencies
node_modules/
venv/
__pycache__/

# Build outputs
dist/
build/

# IDE
.vscode/
.idea/

# OS
.DS_Store
Thumbs.db

# Logs
*.log

# Environment
.env
EOF

# Stage and commit
git add .gitignore
git commit -m "Added .gitignore"
```

## Practice Exercises

```bash
# 1. Initialize a new repository
mkdir practice-git
cd practice-git
git init

# 2. Configure Git
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# 3. Create and commit files
echo "# My App" > README.md
git add README.md
git commit -m "Initial commit"

# 4. Create a branch, make changes
git checkout -b feature
echo "New feature" > feature.txt
git add .
git commit -m "Added feature"

# 5. Switch to main and merge
git checkout main
git merge feature

# 6. View history
git log --oneline --graph --all
```

## Summary

You've learned Git fundamentals:

- **Installation**: Package managers
- **Configuration**: User identity, editor
- **Basic workflow**: init, add, commit, status
- **Branching**: branch, checkout, merge
- **Remotes**: clone, push, pull
- **Advanced**: stash, rebase, aliases

Git is essential for modern development. Practice these commands until they become second nature.

In the next chapter, we'll explore Bash scripting—automating your workflow with scripts.

---

**Previous Chapter**: [Chapter 10: Permissions and Security](10-permissions-security.md)
**Next Chapter**: [Chapter 12: Bash Scripting](12-bash-scripting.md) →
