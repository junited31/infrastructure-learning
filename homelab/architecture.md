# Homelab Architecture

## Purpose

The homelab is a small cloud-hosted infrastructure environment used to practice Linux administration, cloud networking, Docker operations, and automation runner management.

## Components

```text
Administrator Workstation
        |
        | SSH
        v
OCI Ubuntu Server
        |
        | systemd
        v
Docker Services and GitHub Runner
```

## Operational Boundaries

- SSH access is limited to key-based authentication.
- Cloud firewall rules define allowed inbound traffic.
- Docker workloads are isolated by project directory.
- GitHub runner runs as a dedicated service account where practical.
- Logs are reviewed through `journalctl`, Docker logs, and workflow logs.

## Infrastructure Relevance

This architecture creates a controlled environment for practicing real operational workflows: provisioning, access control, service management, network checks, logs, and recovery.
