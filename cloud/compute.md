# Cloud Compute

## Purpose

Compute instances provide virtual servers for running operating systems and workloads.

## Key Tasks

- Select an image and shape.
- Attach SSH public keys during provisioning.
- Assign a public IP only when needed.
- Apply least-required ingress rules.
- Monitor CPU, memory, disk, and service health.
- Reboot or stop instances safely.

## Linux Validation Commands

```bash
hostnamectl
uptime
uname -a
df -h
free -h
systemctl --failed
```

## Infrastructure Relevance

Compute operations map closely to data center server work: provisioning, access validation, patching, resource checks, service recovery, and documentation.
