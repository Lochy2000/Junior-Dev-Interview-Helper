# Git & GitHub - Version Control Guide

> Master Git fundamentals, branching, collaboration, and common workflows

## 📋 Table of Contents

- [What is Git?](#what-is-git)
- [Basic Commands](#basic-commands)
- [Branching & Merging](#branching--merging)
- [Remote Repositories](#remote-repositories)
- [Common Workflows](#common-workflows)
- [Undoing Changes](#undoing-changes)
- [Best Practices](#best-practices)
- [Interview Questions](#interview-questions)

---

## What is Git?

**Git** is a distributed version control system that tracks changes in your code. **GitHub** is a platform for hosting Git repositories online.

### Why Use Git?

✅ Track all changes to your code
✅ Collaborate with teams
✅ Revert to previous versions
✅ Create experimental branches
✅ Review code before merging

---

## Basic Commands

### Setup

```bash
# Configure user info
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# Check configuration
git config --list
```

### Starting a Repository

```bash
# Initialize new repo
git init

# Clone existing repo
git clone https://github.com/user/repo.git

# Check status
git status
```

### Basic Workflow

```bash
# 1. Check what changed
git status

# 2. Add files to staging
git add file.txt           # Specific file
git add .                  # All files
git add *.js              # All JS files

# 3. Commit changes
git commit -m "Add new feature"

# 4. Push to remote
git push origin main
```

### Viewing History

```bash
# See commit history
git log
git log --oneline          # Compact view
git log --graph            # Visual branch graph

# See changes
git diff                   # Unstaged changes
git diff --staged          # Staged changes
git diff main..feature     # Between branches
```

---

## Branching & Merging

### Why Use Branches?

Branches let you work on features independently without affecting main code.

### Branch Commands

```bash
# List branches
git branch                 # Local branches
git branch -a              # All branches (including remote)

# Create new branch
git branch feature-name

# Switch to branch
git checkout feature-name
# or (newer syntax)
git switch feature-name

# Create and switch in one command
git checkout -b feature-name
# or
git switch -c feature-name

# Delete branch
git branch -d feature-name # Safe delete (merged only)
git branch -D feature-name # Force delete
```

### Merging

```bash
# Merge feature into current branch
git checkout main
git merge feature-name

# If merge conflicts occur:
# 1. Open conflicted files
# 2. Resolve conflicts (choose which code to keep)
# 3. Remove conflict markers (<<<<, ====, >>>>)
# 4. Add resolved files
git add resolved-file.txt
# 5. Complete merge
git commit
```

### Merge Conflict Example

```
<<<<<<< HEAD
const greeting = "Hello World";
=======
const greeting = "Hi there";
>>>>>>> feature-branch
```

**Resolution**: Choose one or combine:
```javascript
const greeting = "Hello World";
```

---

## Remote Repositories

### Working with Remotes

```bash
# View remotes
git remote -v

# Add remote
git remote add origin https://github.com/user/repo.git

# Remove remote
git remote remove origin

# Change remote URL
git remote set-url origin https://new-url.git
```

### Push & Pull

```bash
# Push to remote
git push origin main
git push origin feature-branch

# Set upstream (first push)
git push -u origin main

# After upstream set
git push

# Pull latest changes
git pull origin main
# or
git pull  # From tracked branch

# Fetch without merging
git fetch origin
```

---

## Common Workflows

### Feature Branch Workflow

```bash
# 1. Start from updated main
git checkout main
git pull origin main

# 2. Create feature branch
git checkout -b feature/user-auth

# 3. Make changes and commit
git add .
git commit -m "Add user authentication"

# 4. Push feature branch
git push -u origin feature/user-auth

# 5. Create Pull Request on GitHub

# 6. After PR approved, merge via GitHub

# 7. Update local main
git checkout main
git pull origin main

# 8. Delete feature branch
git branch -d feature/user-auth
git push origin --delete feature/user-auth
```

### Gitflow Workflow

```
main          - Production-ready code
develop       - Integration branch
feature/*     - New features
hotfix/*      - Urgent production fixes
release/*     - Release preparation
```

### Fork & Pull Request

```bash
# 1. Fork repo on GitHub

# 2. Clone your fork
git clone https://github.com/YOUR-USERNAME/repo.git

# 3. Add upstream remote
git remote add upstream https://github.com/ORIGINAL-OWNER/repo.git

# 4. Create branch
git checkout -b fix-typo

# 5. Make changes and push
git add .
git commit -m "Fix typo in README"
git push origin fix-typo

# 6. Create Pull Request on GitHub

# 7. Keep fork updated
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

## Undoing Changes

### Before Commit

```bash
# Discard changes in working directory
git restore file.txt
# or (old syntax)
git checkout -- file.txt

# Unstage file (keep changes)
git restore --staged file.txt
# or
git reset HEAD file.txt

# Discard all local changes
git restore .
```

### After Commit

```bash
# Undo last commit, keep changes
git reset --soft HEAD~1

# Undo last commit, discard changes
git reset --hard HEAD~1

# Undo specific commit (creates new commit)
git revert <commit-hash>
```

### Amend Last Commit

```bash
# Change commit message
git commit --amend -m "New message"

# Add forgotten files to last commit
git add forgotten-file.txt
git commit --amend --no-edit
```

---

## Best Practices

### Commit Messages

```bash
# ✅ Good commit messages
git commit -m "Add user login functionality"
git commit -m "Fix navbar responsive layout"
git commit -m "Update dependencies to latest versions"

# ❌ Bad commit messages
git commit -m "fix"
git commit -m "changes"
git commit -m "asdfgh"
```

**Format**:
```
Type: Short description (50 chars or less)

Detailed explanation if needed (72 chars per line)

- Bullet points if helpful
- Explain why, not what (code shows what)
```

**Types**: feat, fix, docs, style, refactor, test, chore

### .gitignore

```bash
# Create .gitignore file
touch .gitignore

# Example .gitignore
node_modules/
.env
*.log
dist/
.DS_Store
```

### Branch Naming

```bash
# ✅ Good branch names
feature/user-authentication
bugfix/login-error
hotfix/security-patch
docs/update-readme

# ❌ Bad branch names
test
new-stuff
branch1
```

---

## Interview Questions

### 1. What is Git?
**Answer**: Distributed version control system that tracks code changes, enables collaboration, and maintains project history.

### 2. Git vs GitHub?
**Answer**:
- **Git**: Version control software (local)
- **GitHub**: Cloud platform for hosting Git repos (remote)

### 3. What is a commit?
**Answer**: Snapshot of your code at a specific point. Contains changes, author, timestamp, and message.

### 4. What is a branch?
**Answer**: Independent line of development. Allows working on features without affecting main code. Main branch typically called `main` or `master`.

### 5. What is merging?
**Answer**: Combining changes from different branches. Git automatically merges when possible, otherwise creates merge conflicts to resolve manually.

### 6. What is a merge conflict?
**Answer**: Occurs when Git can't automatically merge changes (same lines modified differently). Must be resolved manually.

### 7. git pull vs git fetch?
**Answer**:
- `git fetch`: Downloads changes but doesn't merge
- `git pull`: Downloads and merges changes (`fetch + merge`)

### 8. What is a Pull Request?
**Answer**: Request to merge your branch into another. Allows code review, discussion, and approval before merging.

### 9. What is .gitignore?
**Answer**: File listing patterns for Git to ignore (e.g., node_modules, .env). Prevents tracking sensitive or generated files.

### 10. How do you undo a commit?
**Answer**:
- `git reset --soft HEAD~1`: Undo commit, keep changes staged
- `git reset --hard HEAD~1`: Undo commit, discard changes
- `git revert <hash>`: Create new commit that undoes changes (safe for shared code)

### 11. Explain git rebase
**Answer**: Moves or combines commits to new base. Creates cleaner history than merge but rewrites history (avoid on shared branches).

### 12. What is HEAD in Git?
**Answer**: Pointer to current branch/commit. Usually points to latest commit on current branch.

---

## Useful Aliases

```bash
# Add to ~/.gitconfig or use git config

git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.lg "log --oneline --graph --all"
```

---

## Resources

- [Git Documentation](https://git-scm.com/doc)
- [GitHub Docs](https://docs.github.com/)
- [Learn Git Branching](https://learngitbranching.js.org/) - Interactive tutorial
- [Oh My Git!](https://ohmygit.org/) - Game to learn Git

---

**Keywords**: Git tutorial, GitHub, version control, Git commands, Git branching, merge conflicts, Pull Request, Git workflow, Git interview questions, Git best practices
