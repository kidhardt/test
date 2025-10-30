# How to Revert to Earlier Versions in GitHub

Git automatically tracks all changes as versions, so you can always revert to any earlier version of your files.

## View Commit History

### On GitHub
1. Go to https://github.com/kidhardt/test/commits/main
2. Click on any commit to see what changed
3. Click "Browse files" to see the repository at that point in time
4. Or go to a specific file and click "History" to see all versions

### Command Line
```bash
# View commit history
git log --oneline

# View detailed history with file changes
git log --stat

# View changes in a specific commit
git show <commit-hash>
```

## Revert Methods

### Method 1: Restore a Specific File from a Previous Commit

This method replaces the current file with a version from a previous commit:

```bash
# Find the commit hash you want to restore from
git log --oneline

# Restore the file from that commit
git checkout <commit-hash> index.html

# Commit the restored version
git commit -m "Revert index.html to previous version"

# Push to GitHub
git push
```

### Method 2: Revert a Specific Commit

This method creates a new commit that undoes the changes from a specific commit:

```bash
# Find the commit you want to undo
git log --oneline

# Revert that commit (creates a new commit that undoes it)
git revert <commit-hash>

# Push to GitHub
git push
```

### Method 3: View and Download from GitHub Web Interface

1. Go to your file on GitHub: https://github.com/kidhardt/test/blob/main/index.html
2. Click "History" button
3. Browse through the versions
4. Click on a specific commit to see that version
5. Click "View file" or download the raw content
6. Copy the content if you want to restore it

### Method 4: Reset to a Previous Commit (Use with Caution)

This method moves your branch back to a previous commit. **Warning:** This rewrites history!

```bash
# View history
git log --oneline

# Reset to a specific commit (keeps changes in working directory)
git reset --soft <commit-hash>

# Or reset and discard all changes since that commit
git reset --hard <commit-hash>

# Force push to GitHub (only if you're sure!)
git push --force
```

## Best Practices

1. **Always commit frequently** - More commits = more restore points
2. **Write clear commit messages** - Makes it easier to find the version you want
3. **Use branches for experiments** - Keep main branch stable
4. **Create backups before major changes** - Like the index-bu.html file we created
5. **Never force push to shared branches** - Unless you're absolutely sure

## Important Notes

- Git keeps ALL commit history forever (unless you explicitly remove it)
- Changes made via GitHub web editor, VS Code, or any other tool all create commits
- You can revert to any point in the history at any time
- The commit history is your primary version control system
