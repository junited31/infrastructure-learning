# Homelab Architecture

## Purpose

The homelab is a small cloud-hosted infrastructure environment used to practice Linux administration, cloud networking, Docker operations, and automation runner management.

## High-Level Architecture

```text
Administrator Workstation
        |
        | SSH key authentication
        v
OCI Virtual Cloud Network
        |
        | Public subnet + ingress rule for TCP 22
        v
Ubuntu Compute Instance
        |
        | systemd services
        v
Docker Engine + GitHub Actions Runner
```

## Components

| Component | Role | Operational Focus |
|---|---|---|
| Administrator workstation | SSH client and operations terminal | Key management, remote access, file transfer |
| OCI compartment | Logical resource boundary | Resource organization and IAM scope |
| Virtual cloud network | Cloud network boundary | Subnet, route table, internet gateway, ingress rules |
| Ubuntu compute instance | Primary Linux server | Patching, users, services, logs, Docker, runner |
| Docker Engine | Container runtime | Service lifecycle, logs, ports, volumes |
| GitHub Actions runner | Automation worker | `systemd` service, labels, job execution, troubleshooting |

## Network Flow

```text
Workstation
    |
    | TCP 22
    v
OCI security rule
    |
    v
Ubuntu sshd
    |
    | Local administration
    v
systemd, Docker, runner services
```

Inbound access is intentionally narrow. SSH is the primary administrative entry point. Any additional service exposure should be documented with the reason, protocol, port, source range, and rollback plan.

## Operational Boundaries

- SSH access is limited to key-based authentication.
- Cloud firewall rules define allowed inbound traffic.
- Host-level firewall rules are checked separately from OCI network rules.
- Administrative work is performed through a named sudo-capable user rather than direct root login.
- Docker workloads are isolated by project directory.
- GitHub runner runs as a dedicated service account where practical.
- Logs are reviewed through `journalctl`, Docker logs, and workflow logs.
- Runner workloads are treated as trusted only when the source repository and workflow are trusted.

## Directory Layout

```text
/srv/docker/            Docker Compose projects
/srv/github-runner/     Runner installation and runner-related notes
/etc/ssh/               SSH daemon configuration
/var/log/               System and service logs
/home/infraadmin/       Administrative user home
```

The goal is to make service ownership clear. Operational files should live in predictable locations instead of being scattered across temporary directories or personal shell history.

## Operating Workflows

### Provision and Baseline

1. Create the OCI compute instance with an Ubuntu image.
2. Attach an SSH public key during provisioning.
3. Confirm VCN, subnet, route table, and ingress rules.
4. Update packages and reboot.
5. Create the administrative user and test SSH access.
6. Disable password SSH login after key login is confirmed.

### Service Operations

```bash
systemctl --failed
systemctl status ssh --no-pager
systemctl status docker --no-pager
journalctl -p warning --since "24 hours ago"
docker ps
docker compose ps
```

These checks show whether the host is healthy, whether core services are running, and whether recent warnings require investigation.

### Network Validation

```bash
ip addr
ip route
sudo ss -tulpn
nc -vz <public-ip> 22
```

The same issue may exist in different layers: OCI security rule, subnet routing, host firewall, daemon state, or bind address. The workflow is designed to isolate the layer before changing configuration.

## Failure Scenarios Practiced

| Scenario | First Checks | Recovery Direction |
|---|---|---|
| SSH unavailable | OCI ingress rule, instance state, `sshd` status | Use console access or previous session, restore SSH config |
| Docker service stopped | `systemctl status docker`, `journalctl -u docker` | Restart service, inspect logs, check disk space |
| Runner offline | Runner service status, GitHub runner page, outbound connectivity | Restart runner, confirm labels and GitHub access |
| Disk pressure | `df -h`, `docker system df`, log size | Clean unused images, rotate logs, expand volume if needed |
| Port unreachable | `ss -tulpn`, cloud rule, host firewall | Fix listener, ingress source, route, or local firewall |

## Evidence to Capture

For portfolio and operations review, useful evidence includes:

- Sanitized architecture diagram
- Instance shape and operating system version
- SSH hardening settings without keys or secrets
- Service status screenshots or command outputs with sensitive data removed
- Runner labels and successful workflow run summary
- Troubleshooting notes that explain symptom, checks, cause, and fix

## Infrastructure Relevance

This architecture creates a controlled environment for practicing real operational workflows: provisioning, access control, service management, network checks, logs, recovery, and documentation. It also mirrors the mindset required in data center operations: verify the physical or virtual host, confirm network access, inspect service state, review logs, and document the current operating condition.
