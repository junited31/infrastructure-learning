# Bandit Learning: Git History Investigation

## Concept Overview

Bandit included tasks that required inspecting Git repositories, commit history, branches, tags, ignored files, and file changes. The key lesson is that important operational information can exist outside the current working tree.

This maps well to infrastructure work because deployment files, automation scripts, and configuration repositories often require history review during incident investigation.

## Commands Used

```bash
git status
git log --oneline --decorate --all
git show COMMIT
git branch -a
git tag
git show-ref
git diff
git grep pattern
git add -f file.txt
git commit -m "message"
git push
git clone ssh://user@host:port/path/to/repo
```

## What I Learned

- Git history may contain information removed from the latest version.
- Branches and tags can point to different states of a repository.
- `git show` and `git diff` are useful for targeted inspection.
- Repository investigation requires checking more than the current directory listing.
- A file can be ignored by `.gitignore` and still be intentionally added with `git add -f`.
- `git show-ref` exposes lower-level references, including tags and remote references.
- Remote repositories over SSH may require an explicit port.
- Commit messages can provide useful context, but the actual diff must be inspected.
- Deleted sensitive data can remain in history unless it is properly rotated and removed from all reachable history.

## Operational Notes

### Cloning Over SSH with an Explicit Port

```bash
git clone ssh://git-user@example.com:2220/path/to/repo
```

If the port is omitted, SSH uses port 22 by default. This is a common source of connection failures in lab and controlled environments.

### Reviewing History

```bash
git log --oneline --decorate --all
git show COMMIT
git branch -a
git tag
git show-ref
```

Check branches, tags, and full history when a value is not present in the current file but appears to have existed before.

### Handling Ignored Files

```bash
git status
git add -f key.txt
git commit -m "add required key file"
```

Ignored files are normally excluded from commits. `git add -f` should be used deliberately and only after confirming the file does not contain secrets.

### Shell Differences When Creating Files

Different shells handle quotes differently in `echo`. When exact file content matters, verify it:

```bash
printf '%s\n' 'expected content' > key.txt
cat -A key.txt
```

`printf` is often more predictable than `echo` for exact scripted output.

## Real-world Infrastructure Relevance

Git investigation helps operators review configuration changes, deployment scripts, infrastructure-as-code updates, and accidental secret exposure in commit history. It also supports incident timelines by showing when a configuration changed and who changed it.

## Interview Questions

- How do you inspect recent commits?
- How can you search all tracked files for a pattern?
- Why can deleted secrets still be a risk in Git history?
- What is the difference between a branch and a tag?
- How would you compare two commits?
- How do you list remote branches?
- When would you use `git show-ref`?
- Why should ignored files be reviewed carefully before force-adding them?
- Why might an SSH Git clone fail when the port is omitted?
