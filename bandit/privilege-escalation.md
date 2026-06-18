# Bandit Learning: SetUID and Privilege Boundaries

## Concept Overview

Bandit introduced privilege boundaries through executable permissions and SetUID behavior. The important concept is understanding why some programs run with elevated privileges and why they must be reviewed carefully.

## Commands Used

```bash
find / -perm -4000 -type f 2>/dev/null
ls -l /path/to/binary
file /path/to/binary
strings /path/to/binary
```

## What I Learned

- SetUID allows an executable to run with the file owner's privileges.
- Root-owned SetUID binaries are sensitive and should be limited.
- Unexpected SetUID files can create serious security risk.
- Permissions and ownership must be reviewed together.

## Real-world Infrastructure Relevance

Infrastructure operators should understand SetUID because it affects host security, vulnerability assessment, and incident investigation on Linux systems.

## Interview Questions

- What does the SetUID bit do?
- How do you find SetUID binaries on a Linux system?
- Why are root-owned SetUID files sensitive?
- What is the difference between authentication and authorization?
- How would you investigate an unexpected privileged binary?
