# OCI Ubuntu Server Setup

This document describes a repeatable setup process for an Ubuntu Linux server running in Oracle Cloud Infrastructure. The goal is to create a stable homelab host for Linux administration, Docker practice, SSH operations, and GitHub Actions self-hosted runner experiments.

## Environment Overview

Typical environment:

- Cloud provider: Oracle Cloud Infrastructure
- Operating system: Ubuntu Server
- Access method: SSH with key-based authentication
- Primary use cases:
  - Linux administration practice
  - Docker and Docker Compose workloads
  - GitHub Actions self-hosted runner
  - Network and service troubleshooting

## Provisioning Checklist

Before creating the instance:

- Select an appropriate region and availability domain.
- Choose an Ubuntu image.
- Select a compute shape suitable for lab workloads.
- Create or select a virtual cloud network.
- Confirm subnet routing to an internet gateway if public SSH access is required.
- Add an SSH public key during instance creation.
- Record the assigned public and private IP addresses.

## Initial SSH Access

Connect using the default Ubuntu cloud user:

```bash
ssh -i ~/.ssh/oci_admin_key ubuntu@203.0.113.10
```

If the connection fails, check:

- Public IP address
- OCI security list or network security group ingress rule for TCP 22
- Local private key path and permissions
- Instance state
- Route table and internet gateway configuration

Recommended local key permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/oci_admin_key
```

## System Update

After first login:

```bash
sudo apt update
sudo apt upgrade -y
sudo reboot
```

Reconnect after reboot and confirm:

```bash
uptime
uname -a
lsb_release -a
```

## Basic Host Identity

Set a clear hostname:

```bash
sudo hostnamectl set-hostname oci-lab-01
```

Confirm:

```bash
hostnamectl
```

A descriptive hostname helps distinguish the server in logs, SSH prompts, monitoring output, and GitHub runner names.

## Administrative User Setup

Create a named administrative user:

```bash
sudo adduser infraadmin
sudo usermod -aG sudo infraadmin
```

Create the SSH directory:

```bash
sudo mkdir -p /home/infraadmin/.ssh
sudo cp /home/ubuntu/.ssh/authorized_keys /home/infraadmin/.ssh/authorized_keys
sudo chown -R infraadmin:infraadmin /home/infraadmin/.ssh
sudo chmod 700 /home/infraadmin/.ssh
sudo chmod 600 /home/infraadmin/.ssh/authorized_keys
```

Test login in a second terminal before changing SSH restrictions:

```bash
ssh -i ~/.ssh/oci_admin_key infraadmin@203.0.113.10
```

## SSH Hardening

Edit the SSH daemon configuration:

```bash
sudo nano /etc/ssh/sshd_config
```

Common settings:

```sshconfig
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

Validate before reloading:

```bash
sudo sshd -t
sudo systemctl reload ssh
```

Keep the existing SSH session open until a new session succeeds.

## Firewall and Cloud Network Rules

OCI network controls and host firewall controls are separate. A connection must be allowed by both layers when both are in use.

Common OCI ingress rules:

| Purpose | Protocol | Port | Source |
|---|---|---:|---|
| SSH administration | TCP | 22 | Trusted public IP range |
| HTTP test service | TCP | 80 | As needed |
| HTTPS test service | TCP | 443 | As needed |

Check host listening ports:

```bash
sudo ss -tulpn
```

If using UFW:

```bash
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status verbose
```

## Install Baseline Tools

```bash
sudo apt install -y \
  curl \
  wget \
  git \
  vim \
  tmux \
  htop \
  net-tools \
  dnsutils \
  unzip \
  ca-certificates
```

Operational value:

- `tmux` keeps sessions alive during long tasks.
- `htop` helps with process visibility.
- `dnsutils` provides `dig` and DNS troubleshooting tools.
- `curl` and `wget` support connectivity checks and downloads.

## Docker Preparation

Install Docker using the official repository or the distribution package depending on the lab goal. For basic practice:

```bash
sudo apt install -y docker.io docker-compose-plugin
sudo systemctl enable --now docker
sudo usermod -aG docker infraadmin
```

Log out and back in, then verify:

```bash
docker version
docker compose version
docker run --rm hello-world
```

Security note: users in the `docker` group can control containers with elevated impact on the host. Add only trusted accounts.

## Directory Layout

Create operational directories:

```bash
sudo mkdir -p /srv/docker
sudo mkdir -p /srv/github-runner
sudo chown -R infraadmin:infraadmin /srv/docker /srv/github-runner
```

Suggested use:

```text
/srv/docker           Docker Compose projects
/srv/github-runner    Runner installation or runner-related notes
/var/log              System and service logs
/etc                  System configuration
```

## Service and Log Checks

Check system services:

```bash
systemctl --failed
systemctl status ssh --no-pager
systemctl status docker --no-pager
```

Review recent logs:

```bash
journalctl -p warning --since "24 hours ago"
journalctl -u ssh --since "1 hour ago"
journalctl -u docker --since "1 hour ago"
```

Check resource usage:

```bash
df -h
free -h
uptime
top
```

## Backup and Recovery Notes

Operational habits:

- Keep SSH private keys protected and backed up securely.
- Document the instance shape, image, region, and network configuration.
- Avoid storing important data only on an ephemeral lab instance.
- Snapshot or back up important volumes before risky changes.
- Keep a recovery path through the cloud console.

## Validation Checklist

The server is ready for homelab use when:

- SSH key-based login works for the administrative user.
- System packages are updated.
- Hostname is set.
- Password SSH login is disabled after successful key testing.
- Required network ports are documented.
- Baseline tools are installed.
- Docker runs a test container if Docker is part of the lab.
- Failed services and critical logs have been reviewed.

## Infrastructure Relevance

This setup process reflects common data center and cloud operations tasks: provision a host, secure access, validate networking, patch the OS, manage users, install required services, inspect logs, and document the operational state clearly enough for another engineer to understand.
