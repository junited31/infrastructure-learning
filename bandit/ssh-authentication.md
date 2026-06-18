# Bandit Learning: SSH Authentication

## Concept Overview

Bandit provided practice with SSH login, private keys, remote users, non-standard ports, and strict key permissions. No passwords or challenge solutions are documented here.

## Commands Used

```bash
ssh user@host
ssh -p 2220 user@host
ssh -i ./private_key user@host
chmod 600 private_key
ssh -vvv user@host
```

## What I Learned

- SSH requires the correct username, host, port, and authentication method.
- Private keys must be protected with restrictive permissions.
- Verbose SSH output helps isolate authentication failures.
- Non-standard ports must be specified explicitly.
- Remote administration depends on both network reachability and valid identity.

## Real-world Infrastructure Relevance

SSH is a core operations skill for Linux servers, cloud instances, and emergency troubleshooting. Understanding SSH failures helps separate identity problems from firewall or service availability problems.

## Interview Questions

- How do you connect to SSH on a non-standard port?
- Why does SSH reject private keys with open permissions?
- What does `Permission denied (publickey)` usually mean?
- How would you debug an SSH connection failure?
- What is the difference between a private key and a public key?
