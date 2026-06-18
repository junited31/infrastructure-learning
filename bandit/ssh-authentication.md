# Bandit Learning: SSH Authentication

## Concept Overview

Bandit provided practice with SSH login, private keys, remote users, non-standard ports, non-interactive command execution, and strict key permissions. No passwords, private keys, or challenge solutions are documented here.

The important operational skill is being able to separate SSH failures into identity, key permission, shell behavior, network path, port, and server-side configuration problems.

## Commands Used

```bash
ssh user@host
ssh -p 2220 user@host
ssh -i ./private_key user@host
chmod 600 private_key
ssh -vvv user@host
ssh user@host "command"
ssh -T user@host
cat /etc/passwd
echo "$0"
```

## What I Learned

- SSH requires the correct username, host, port, and authentication method.
- Private keys must be protected with restrictive permissions.
- Verbose SSH output helps isolate authentication failures.
- Non-standard ports must be specified explicitly.
- Remote administration depends on both network reachability and valid identity.
- SSH can run a remote command without opening a normal interactive shell.
- `-T` disables pseudo-terminal allocation, which can be useful when testing non-interactive behavior.
- Login shells can be restricted or replaced by wrapper programs.
- `/etc/passwd` shows a user's configured shell, which helps explain unusual login behavior.
- `$0` can show the current shell name and help identify whether the session is running `sh`, `bash`, or a restricted wrapper.

## Operational Notes

### Connecting to a Non-Standard Port

```bash
ssh -p 2220 user@example.com
```

Many lab and production systems expose SSH on a non-default port. The port must be specified by the client or stored in `~/.ssh/config`.

### Using a Private Key

```bash
chmod 600 ./private_key
ssh -i ./private_key user@example.com
```

SSH rejects private keys that are readable by other users because the private key is an identity credential.

### Running a Remote Command

```bash
ssh user@example.com "hostname && uptime"
```

This is useful for operational checks, automation, and recovery when an interactive shell has problems.

### Restricted Shell Behavior

Some accounts are configured to run a specific command or wrapper instead of a normal shell. When a login immediately exits or opens a pager/editor, useful checks include:

```bash
cat /etc/passwd | grep username
echo "$0"
```

This helps identify whether the issue is SSH authentication or post-authentication shell behavior.

## Real-world Infrastructure Relevance

SSH is a core operations skill for Linux servers, cloud instances, and emergency troubleshooting. Understanding SSH failures helps separate identity problems from firewall, daemon, account, key permission, and shell configuration problems.

## Interview Questions

- How do you connect to SSH on a non-standard port?
- Why does SSH reject private keys with open permissions?
- What does `Permission denied (publickey)` usually mean?
- How would you debug an SSH connection failure?
- What is the difference between a private key and a public key?
- What is the difference between interactive and non-interactive SSH?
- How can a user's login shell affect SSH behavior?
- What does `ssh -T` do?
