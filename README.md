# Git Tutorial - Complete Guide 🚀

> A comprehensive guide covering essential Git commands, workflows, and best practices.

## 📚 Table of Contents
- [Initial Setup](#initial-setup)
- [Basic Commands](#basic-commands)
- [Commit History](#commit-history)
- [Undoing Changes](#undoing-changes)
- [Recovery After Hard Reset](#recovery-after-hard-reset)
- [Handling Conflicts](#handling-conflicts)
- [Branch Management](#branch-management)
- [Advanced Workflows](#advanced-workflows)

---

## Initial Setup

Configure Git with your identity:

```bash
git config --global user.name "Sameer Kushwaha"
git config --global user.email "sameerkushwaha2003@gmail.com"
git config --global color.ui auto
```

---

## Basic Commands

### Staging and Committing

```bash
git add .                        # Stage all changes
git add <file>                   # Stage specific file
git commit -m "[message]"        # Commit with message
git status                       # Check working directory status
```

---

## Commit History

### Viewing Logs

```bash
git log                          # Full commit history
git log --oneline                # Clean & short (most used 🔥)
git log --oneline --graph --all  # With branch & merge graph (interview favorite)
git log -5                       # View last 5 commits
```

---

## Undoing Changes

### Reset Types

**Soft Reset**: Undo commit but keep changes staged
```bash
git reset --soft HEAD~1          # Undo last commit, keep changes staged
```

**Hard Reset**: Undo commit and discard changes
```bash
git reset --hard HEAD~1          # Undo last commit and discard changes
git reset --hard [commit_hash]   # Reset to specific commit
```

> ⚠️ **Warning**: Hard reset permanently deletes changes!

---

## Recovery After Hard Reset

If you accidentally did a hard reset, you can recover using `git reflog`:

### Example Scenario

```bash
# View commit history
git log --oneline
# Output:
# 6fd8337 little modification
# 0a89a0b Git command commit
# e5db5f1 commit for 3 commands only

# View reflog to find lost commits
git reflog
# Output:
# HEAD@{0} 6fd8337 commit: little modification   ← current
# HEAD@{1} 0a89a0b commit: Git command commit
# HEAD@{2} e5db5f1 reset: moving to HEAD~1       ← ⚠️ hard reset happened here
# HEAD@{3} 54e92e5 commit: commit for 3 commands only once again
# HEAD@{4} e5db5f1 commit (initial)

# Recover the lost commit
git reset --hard 54e92e5

# Verify recovery
git log --oneline
# Output:
# 54e92e5 (HEAD -> main) commit for 3 commands only once again
# e5db5f1 commit for 3 commands only
```

---

## Handling Conflicts

### Scenario: Friend Pushed Changes While You Were Working

**Error**: `! [rejected] main -> main (fetch first)`

### 🚫 What NOT to Do (Dangerous)

```bash
git push --force  # ❌ This can delete your friend's work!
```

> Only use `--force` if you 100% know what you're doing.

### ✅ What TO Do (Safe)

#### **Case 1: Changes Added to Staging Area (Not Committed)**

```bash
git stash                        # Stash your changes temporarily
git pull origin <branch>         # Pull latest changes from remote
git stash pop                    # Apply stashed changes back
git add .
git commit -m "your message"
git push origin <branch>
```

#### **Case 2: Changes Already Committed**

```bash
git reset --soft HEAD~1          # Undo commit but keep changes staged
git stash                        # Stash your changes
git pull origin <branch>         # Pull latest changes
git stash pop                    # Apply stashed changes
git add .
git commit -m "your message"
git push origin <branch>
```

#### **Case 3: Changes Not Added to Staging Area**

```bash
git stash                        # Stash your changes
git pull origin <branch>         # Pull latest changes
git stash pop                    # Apply stashed changes
```

---

## Branch Management

### Viewing Branches

```bash
git branch                       # Show local branches only
git branch -r                    # Show remote branches only
git branch -a                    # Show all branches (local + remote)
```

### Renaming Branches

#### Local Renaming

```bash
git checkout <old_branch_name>   # Switch to branch
git branch -m <new_branch_name>  # Rename branch
git branch                       # Verify rename
```

#### Remote Renaming

```bash
# Push new branch name
git push origin -u <new_branch_name>

# Delete old branch from remote
git push origin --delete <old_branch_name>

# Refresh remote refs
git fetch --prune

# Verify
git branch -r
```

### Deleting Branches

```bash
git branch -d <branch_name>      # Delete (warns if not merged)
git branch -D <branch_name>      # Force delete
```

### Deleting Default Branch from Remote

> ⚠️ **Note**: GitHub does NOT allow deleting the default branch directly.

**Steps**:
1. Go to GitHub repo settings
2. Change the default branch to another branch
3. Then delete the old default branch:
```bash
git push origin --delete main
```

---

## Advanced Workflows

### Creating a Fresh Branch (No History)

```bash
# Navigate to your repo
cd your-repo

# Create orphan branch (no history)
git checkout --orphan new-branch-name

# Remove all existing files from index
git rm -rf .

# Add your new file
echo "hello" > yourfile.txt
git add yourfile.txt
git commit -m "Initial commit in empty branch"
git push origin new-branch-name
```

### Stash Commands

```bash
git stash                        # Stash current changes
git stash list                   # List all stashes
git stash pop                    # Apply and remove latest stash
git stash apply                  # Apply latest stash (keep in list)
git stash drop                   # Remove latest stash
git stash clear                  # Remove all stashes
```

### Remote Operations

```bash
git remote -v                    # View remote URLs
git remote add origin <url>      # Add remote repository
git push origin <branch>         # Push to remote branch
git pull origin <branch>         # Pull from remote branch
git fetch --prune                # Remove deleted remote branches
```

---

## Best Practices 🌟

1. **Commit Often**: Make small, logical commits
2. **Write Clear Messages**: Use descriptive commit messages
3. **Pull Before Push**: Always pull latest changes before pushing
4. **Use Branches**: Create feature branches for new work
5. **Never Force Push**: Unless you're absolutely sure
6. **Review Before Commit**: Use `git status` and `git diff`
7. **Use .gitignore**: Exclude unnecessary files

---

## Common Git Workflows

### Feature Branch Workflow

```bash
# Create and switch to feature branch
git checkout -b feature/new-feature

# Make changes and commit
git add .
git commit -m "Add new feature"

# Push to remote
git push origin feature/new-feature

# Merge to main (after review)
git checkout main
git merge feature/new-feature
git push origin main
```

### Fixing Merge Conflicts

```bash
# Pull latest changes
git pull origin main

# If conflicts occur, Git will mark them in files
# Edit files to resolve conflicts
# Look for markers: <<<<<<<, =======, >>>>>>>

# After resolving
git add .
git commit -m "Resolve merge conflicts"
git push origin main
```

---

## Useful Git Aliases

Add these to your `.gitconfig` for shortcuts:

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.lg 'log --oneline --graph --all'
```

---

## Interview Favorite Commands 🔥

```bash
git log --oneline --graph --all  # Visual branch history
git reflog                       # Recovery tool
git stash                        # Temporary storage
git cherry-pick <commit>         # Apply specific commit
git rebase -i HEAD~3             # Interactive rebase
git bisect                       # Binary search for bugs
```

---

## Resources

- [Git Official Documentation](https://git-scm.com/doc)
- [GitHub Guides](https://guides.github.com/)
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials)

---

**Happy Coding! 🚀**
