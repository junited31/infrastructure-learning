# Bandit Learning: Linux Permissions and Hidden Files

## Concept Overview

Bandit reinforced how Linux permissions, ownership, hidden files, unusual filenames, and directory traversal affect access to data. The focus was not memorizing challenge answers, but learning how to inspect files safely and understand why access is allowed or denied.

## Commands Used

```bash
ls -la
file ./filename
cat ./filename
find . -type f
find . -readable
find . -size 1033c
stat ./filename
chmod
```

## What I Learned

- Hidden files are normal filesystem entries with names beginning with `.`.
- Filenames can contain spaces, dashes, and special characters, so quoting and `./` prefixes matter.
- `find` is useful when file names are unknown but metadata is known.
- File type should be confirmed before assuming content format.
- Permission errors should be inspected through ownership, mode bits, and parent directories.

## Real-world Infrastructure Relevance

These skills apply to configuration discovery, log inspection, permission troubleshooting, backup review, and safe handling of files with unusual names on production systems.

## Interview Questions

- How do you list hidden files in a directory?
- Why might `cat -file` behave differently from `cat ./-file`?
- How would you find readable files owned by a specific user?
- What command shows detailed file metadata?
- How can parent directory permissions block access to a readable file?
