# Bandit Learning: Git History Investigation

## Concept Overview

Bandit included tasks that required inspecting Git repositories, commit history, branches, tags, and file changes. The key lesson is that important operational information can exist outside the current working tree.

## Commands Used

```bash
git status
git log --oneline --decorate --all
git show COMMIT
git branch -a
git tag
git diff
git grep pattern
```

## What I Learned

- Git history may contain information removed from the latest version.
- Branches and tags can point to different states of a repository.
- `git show` and `git diff` are useful for targeted inspection.
- Repository investigation requires checking more than the current directory listing.

## Real-world Infrastructure Relevance

Git investigation helps operators review configuration changes, deployment scripts, infrastructure-as-code updates, and accidental secret exposure in commit history.

## Interview Questions

- How do you inspect recent commits?
- How can you search all tracked files for a pattern?
- Why can deleted secrets still be a risk in Git history?
- What is the difference between a branch and a tag?
- How would you compare two commits?
