# Bandit Learning: SetUID and Privilege Boundaries

## Concept Overview

Bandit introduced privilege boundaries through executable permissions, SetUID behavior, effective user IDs, and restricted command execution. The important concept is understanding why some programs run with elevated privileges and why they must be reviewed carefully.

## Commands Used

```bash
find / -perm -4000 -type f 2>/dev/null
ls -l /path/to/binary
file /path/to/binary
strings /path/to/binary
id
whoami
./helper-command id
```

## What I Learned

- SetUID allows an executable to run with the file owner's privileges.
- In `ls -l`, `s` in the user execute position indicates SetUID.
- `id` shows real and effective identity information that can explain what privileges a process has.
- Root-owned SetUID binaries are sensitive and should be limited.
- Unexpected SetUID files can create serious security risk.
- Permissions and ownership must be reviewed together.
- Helper binaries should be inspected as privileged execution paths, not treated as normal scripts.
- Restricted shells and privileged helper programs are both examples of controlled privilege boundaries.

## Operational Notes

### Identifying SetUID Files

```bash
find / -perm -4000 -type f 2>/dev/null
```

Review ownership, path, package source, and expected purpose before assuming a SetUID file is safe or unsafe.

### Checking Effective Identity

```bash
id
whoami
./helper-command id
```

If a helper command reports a different effective user, it may be intentionally crossing a privilege boundary.

### Reviewing a Privileged Binary

```bash
ls -l ./helper-command
file ./helper-command
strings ./helper-command
```

This does not replace source review, but it can identify whether the file is an executable, script, or unexpected binary.

## Real-world Infrastructure Relevance

Infrastructure operators should understand SetUID because it affects host security, vulnerability assessment, and incident investigation on Linux systems. Unexpected privileged executables are important findings during hardening reviews and compromise investigations.

## Interview Questions

- What does the SetUID bit do?
- How do you find SetUID binaries on a Linux system?
- Why are root-owned SetUID files sensitive?
- What is the difference between authentication and authorization?
- How would you investigate an unexpected privileged binary?
- What is an effective user ID?
- Why should privileged helper programs have narrow behavior?
- How can file ownership change the meaning of execute permission?
