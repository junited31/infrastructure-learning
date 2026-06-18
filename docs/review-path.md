# Portfolio Review Path

This guide highlights the strongest evidence in the repository for infrastructure, cloud operations, and data center operations review.

## Recommended Review Path

1. [OCI Ubuntu Server Setup](../homelab/oci-server-setup.md)

   Shows end-to-end server provisioning, SSH hardening, baseline tooling, Docker preparation, log checks, backup notes, and validation criteria.

2. [Homelab Architecture](../homelab/architecture.md)

   Shows the cloud-hosted lab layout, OCI network path, operational boundaries, service locations, failure scenarios, and evidence to capture.

3. [SSH Operations](../networking/ssh.md)

   Shows key-based access, client configuration, port forwarding, server-side checks, common failure patterns, and troubleshooting workflow.

4. [Linux Permissions](../linux/permissions.md)

   Shows ownership, permission modes, special bits, SSH key permissions, service access troubleshooting, and operational examples.

5. [GitHub Runner Installation](../github-runner/installation.md)

   Shows self-hosted runner setup, dedicated user handling, `systemd` service management, workflow validation, and security considerations.

6. [Bandit Learning Notes](../bandit/linux-permissions.md)

   Shows command-line troubleshooting practice organized by infrastructure concepts instead of challenge answers.

## Portfolio Evidence

This repository is intended to show practical infrastructure readiness through documentation, not application code. The main evidence areas are:

- A repeatable Ubuntu server setup process on OCI
- Secure remote access practices with SSH keys and service validation
- Linux troubleshooting workflows for permissions, processes, disks, services, and logs
- Network troubleshooting across host checks and cloud firewall assumptions
- Container and GitHub runner operations on a managed Linux host
- Clear notes from command-line security practice without exposing challenge secrets

## What This Repository Is Not

- Not a resume repository
- Not a certification file archive
- Not a LeetCode or algorithm repository
- Not an AI or LLM application portfolio

The focus is infrastructure operations: Linux administration, cloud server setup, networking fundamentals, Docker operations, GitHub runner management, and troubleshooting practice.
