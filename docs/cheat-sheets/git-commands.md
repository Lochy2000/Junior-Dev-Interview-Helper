# Git Commands Cheat Sheet

> Quick reference for essential Git commands

## 📋 Table of Contents

- [Setup & Configuration](#setup--configuration)
- [Creating Repositories](#creating-repositories)
- [Basic Snapshotting](#basic-snapshotting)
- [Branching & Merging](#branching--merging)
- [Remote Repositories](#remote-repositories)
- [Inspection & Comparison](#inspection--comparison)
- [Undoing Changes](#undoing-changes)
- [Stashing](#stashing)

---

## Setup & Configuration

```bash
# Set username
git config --global user.name "Your Name"

# Set email
git config --global user.email "your@email.com"

# Set default branch name
git config --global init.defaultBranch main

# View all settings
git config --list

# Get specific setting
git config user.name

# Edit config file
git config --global --edit
```

---

## Creating Repositories

```bash
# Initialize new repository
git init

# Clone existing repository
git clone <url>
git clone <url> <directory-name>

# Clone specific branch
git clone -b <branch-name> <url>
```

---

## Basic Snapshotting

```bash
# Check status
git status
git status -s  # Short format

# Add files to staging
git add <file>
git add .                    # All files
git add *.js                 # All JS files
git add -A                   # All changes
git add -p                   # Interactive staging

# Commit changes
git commit -m "Commit message"
git commit -am "Message"     # Add and commit (tracked files only)
git commit --amend           # Modify last commit
git commit --amend -m "New message"  # Change last commit message

# Remove files
git rm <file>                # Delete and stage removal
git rm --cached <file>       # Untrack but keep file

# Move/rename files
git mv <old-name> <new-name>

# View changes
git diff                     # Unstaged changes
git diff --staged            # Staged changes
git diff <branch1> <branch2> # Between branches
```

---

## Branching & Merging

```bash
# List branches
git branch                   # Local branches
git branch -a                # All branches
git branch -r                # Remote branches

# Create branch
git branch <branch-name>

# Switch branch
git checkout <branch-name>
git switch <branch-name>     # Newer syntax

# Create and switch
git checkout -b <branch-name>
git switch -c <branch-name>

# Rename branch
git branch -m <old-name> <new-name>
git branch -m <new-name>     # Rename current branch

# Delete branch
git branch -d <branch-name>  # Safe delete (merged only)
git branch -D <branch-name>  # Force delete

# Merge branch into current
git merge <branch-name>
git merge <branch-name> --no-ff  # Force merge commit

# Abort merge
git merge --abort

# Rebase
git rebase <branch-name>
git rebase --continue        # After resolving conflicts
git rebase --abort           # Cancel rebase
git rebase -i <commit>       # Interactive rebase
```

---

## Remote Repositories

```bash
# List remotes
git remote
git remote -v                # With URLs

# Add remote
git remote add <name> <url>
git remote add origin <url>

# Remove remote
git remote remove <name>

# Rename remote
git remote rename <old> <new>

# Change remote URL
git remote set-url origin <new-url>

# Fetch changes
git fetch                    # From all remotes
git fetch <remote>           # From specific remote
git fetch <remote> <branch>  # Specific branch

# Pull changes
git pull                     # Fetch and merge
git pull <remote> <branch>
git pull --rebase            # Rebase instead of merge

# Push changes
git push
git push <remote> <branch>
git push -u origin main      # Set upstream
git push --all               # All branches
git push --tags              # Push tags

# Push and set upstream
git push -u origin <branch>

# Delete remote branch
git push origin --delete <branch-name>
```

---

## Inspection & Comparison

```bash
# View commit history
git log
git log --oneline            # Compact view
git log --graph              # Visual graph
git log --all --decorate --oneline --graph  # Beautiful graph
git log -n 5                 # Last 5 commits
git log --since="2 weeks ago"
git log --author="Name"
git log --grep="keyword"     # Search commit messages
git log <file>               # Commits affecting file

# Show commit details
git show <commit-hash>
git show HEAD                # Last commit
git show HEAD~1              # Second to last

# View file at specific commit
git show <commit>:<file>

# Blame (who changed what)
git blame <file>

# View changes
git diff
git diff <commit1> <commit2>
git diff <branch1>..<branch2>

# List files
git ls-files                 # Tracked files
```

---

## Undoing Changes

```bash
# Discard changes in working directory
git restore <file>
git checkout -- <file>       # Old syntax

# Unstage file
git restore --staged <file>
git reset HEAD <file>        # Old syntax

# Discard all local changes
git restore .
git checkout .               # Old syntax

# Undo commits (keep changes)
git reset --soft HEAD~1      # Undo last commit
git reset --soft HEAD~3      # Undo last 3 commits

# Undo commits (discard changes)
git reset --hard HEAD~1      # ⚠️ Dangerous!
git reset --hard <commit>

# Undo commits (create new commit)
git revert <commit>          # Safest option
git revert HEAD              # Undo last commit

# Amend last commit
git commit --amend
git commit --amend --no-edit # Keep same message

# Reset to remote state
git reset --hard origin/main

# Clean untracked files
git clean -n                 # Dry run (preview)
git clean -f                 # Remove untracked files
git clean -fd                # Remove files and directories
```

---

## Stashing

```bash
# Stash changes
git stash
git stash save "Message"
git stash -u                 # Include untracked files

# List stashes
git stash list

# Apply stash
git stash apply              # Apply latest stash
git stash apply stash@{2}    # Apply specific stash

# Apply and remove stash
git stash pop
git stash pop stash@{2}

# View stash contents
git stash show
git stash show -p            # Show diff

# Drop stash
git stash drop stash@{2}

# Clear all stashes
git stash clear
```

---

## Tags

```bash
# List tags
git tag

# Create tag
git tag <tag-name>
git tag -a v1.0 -m "Version 1.0"  # Annotated tag

# Tag specific commit
git tag <tag-name> <commit>

# Delete tag
git tag -d <tag-name>

# Push tags
git push origin <tag-name>
git push --tags              # All tags

# Delete remote tag
git push origin --delete <tag-name>

# Checkout tag
git checkout <tag-name>
```

---

## Useful Aliases

Add to `~/.gitconfig`:

```bash
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    cm = commit -m
    ca = commit -am
    unstage = restore --staged
    last = log -1 HEAD
    lg = log --oneline --graph --all --decorate
    undo = reset --soft HEAD~1
```

Usage:
```bash
git st              # Instead of git status
git co main         # Instead of git checkout main
git lg              # Beautiful log graph
```

---

## Common Workflows

### Feature Branch Workflow

```bash
# Update main
git checkout main
git pull origin main

# Create feature branch
git checkout -b feature/new-feature

# Make changes and commit
git add .
git commit -m "Add new feature"

# Push feature branch
git push -u origin feature/new-feature

# After PR approval, update main
git checkout main
git pull origin main

# Delete feature branch
git branch -d feature/new-feature
git push origin --delete feature/new-feature
```

### Fix Merge Conflicts

```bash
# Start merge
git merge feature-branch
# CONFLICT!

# View conflicted files
git status

# Edit files to resolve conflicts
# Remove conflict markers: <<<<<<<, =======, >>>>>>>

# Mark as resolved
git add <resolved-files>

# Complete merge
git commit
```

### Update Fork

```bash
# Add upstream remote (once)
git remote add upstream <original-repo-url>

# Fetch upstream changes
git fetch upstream

# Merge into your main
git checkout main
git merge upstream/main

# Push to your fork
git push origin main
```

---

## Tips & Best Practices

```bash
# Create .gitignore
echo "node_modules/" >> .gitignore
echo ".env" >> .gitignore
git add .gitignore
git commit -m "Add gitignore"

# View what will be pushed
git diff origin/main..HEAD

# Dry run (see what would happen)
git merge --no-commit --no-ff <branch>  # Preview merge
git clean -n                            # Preview clean

# Save credentials (cache for 1 hour)
git config --global credential.helper cache
git config --global credential.helper 'cache --timeout=3600'
```

---

**Keywords**: Git commands, Git cheat sheet, Git reference, version control commands, Git workflow, Git branching, Git merge, Git rebase, Git stash
