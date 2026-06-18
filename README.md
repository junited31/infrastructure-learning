# Infrastructure Learning Portfolio

This repository documents my practical learning in Linux administration, cloud infrastructure, networking, containers, and operations troubleshooting. It is built for infrastructure, cloud operations, and data center operations roles, with a focus on the skills used to operate reliable systems rather than software application development.

## About Me

I am an Applied Physics graduate preparing for infrastructure and data center operations roles, including the AWS Data Center Operations Trainee program. My background combines analytical problem solving with hands-on system administration practice across Ubuntu Linux, Oracle Cloud Infrastructure, Docker, SSH, and self-hosted automation runners.

My current focus is building operational confidence in Linux servers, cloud compute instances, network troubleshooting, identity and access control, and repeatable infrastructure documentation.

## Certifications

- Oracle Cloud Infrastructure 2025 Certified Foundations Associate
- MySQL Implementation Certified Associate
- SQLD
- ADsP

## Technical Skills

### Linux Administration

- Ubuntu server installation and post-install configuration
- SSH-based remote administration
- User, group, and permission management
- Filesystem navigation and inspection
- Process monitoring and service checks
- `systemd` service management
- Cron job review and basic scheduling
- Shell-based troubleshooting with standard Linux utilities

### Networking Fundamentals

- TCP/IP concepts, ports, sockets, and common service behavior
- DNS lookup and basic name resolution troubleshooting
- SSH authentication and secure remote access
- TLS certificate inspection and encrypted connection testing
- Port scanning concepts for service discovery and validation
- Connectivity checks using tools such as `ping`, `ss`, `nc`, `curl`, and `dig`

### Cloud and Infrastructure

- Oracle Cloud Infrastructure fundamentals
- Compute instance provisioning and access
- Virtual cloud network concepts
- Security list and network security group awareness
- Block storage and boot volume basics
- IAM concepts including users, groups, policies, and least privilege

### Containers and Automation

- Docker image and container lifecycle basics
- Docker Compose for multi-service environments
- Container log inspection and restart behavior
- GitHub Actions self-hosted runner installation
- Runner service management with `systemd`
- Basic CI runner troubleshooting and operational checks

## Learning Areas

This repository is organized around the core areas I am strengthening for infrastructure operations work:

- Linux administration
- Networking fundamentals
- Cloud infrastructure operations
- Docker and containerized services
- GitHub Actions self-hosted runner operations
- Security-minded command-line practice through OverTheWire Bandit
- Homelab documentation and repeatable setup notes

## Review Guide

For a short reviewer-oriented path through the strongest documents, see [docs/review-path.md](docs/review-path.md).

## Repository Structure

```text
infrastructure-learning/
├── README.md
├── docs/
│   └── review-path.md
├── linux/
│   ├── permissions.md
│   ├── filesystem.md
│   ├── users-groups.md
│   ├── processes.md
│   ├── systemd.md
│   ├── cron.md
│   └── troubleshooting.md
├── networking/
│   ├── tcp-ip.md
│   ├── dns.md
│   ├── ssh.md
│   ├── tls.md
│   ├── port-scanning.md
│   └── troubleshooting.md
├── cloud/
│   ├── oci-fundamentals.md
│   ├── compute.md
│   ├── networking.md
│   ├── storage.md
│   └── iam.md
├── docker/
│   ├── docker-basics.md
│   ├── docker-compose.md
│   └── troubleshooting.md
├── github-runner/
│   ├── installation.md
│   ├── service-management.md
│   └── troubleshooting.md
├── bandit/
│   ├── linux-permissions.md
│   ├── ssh-authentication.md
│   ├── networking.md
│   ├── ssl-tls.md
│   ├── privilege-escalation.md
│   ├── cron-jobs.md
│   └── git-investigation.md
└── homelab/
    ├── architecture.md
    ├── oci-server-setup.md
    ├── user-management.md
    ├── docker-environment.md
    └── github-runner-environment.md
```

## Homelab Experience

My homelab work is based on running and administering an Ubuntu Linux server in Oracle Cloud Infrastructure. The environment is used to practice:

- Secure SSH access with key-based authentication
- Initial server hardening and package updates
- Linux user and group administration
- Docker and Docker Compose service deployment
- GitHub Actions self-hosted runner installation
- Service management with `systemd`
- Log review and troubleshooting
- Network access validation through cloud firewall rules and host-level checks

The goal is to treat the homelab as an operations practice environment: document changes, verify access paths, understand failure modes, and keep procedures repeatable.

## Current Learning Goals

- Strengthen Linux troubleshooting workflows for processes, disks, services, logs, and permissions
- Practice network debugging from both host and cloud firewall perspectives
- Improve understanding of cloud IAM and least privilege design
- Build confidence with incident-style checks: what changed, what failed, what logs confirm it, and how to recover
- Continue documenting homelab operations in a clear, recruiter-readable format

## Notes on Bandit Documentation

The Bandit section is organized by infrastructure concepts, not by challenge level. It does not include passwords, flags, or direct challenge solutions. The focus is on transferable skills such as permissions analysis, SSH authentication, network service inspection, TLS validation, cron review, SetUID behavior, and Git history investigation.

Bandit practice strengthened my command-line troubleshooting habits in these areas:

- Handling hidden files, filenames with spaces, and filenames that begin with option-like characters
- Searching files by metadata such as size, owner, group, readability, and executable status
- Filtering command output with `grep`, `sort`, `uniq`, `strings`, `base64`, `tr`, and archive tools
- Connecting to SSH services with non-standard ports, private keys, and non-interactive commands
- Testing local TCP services with `nc`, `ncat`, `telnet`, and small authorized port scans
- Inspecting TLS-wrapped services with `openssl s_client` and `ncat --ssl`
- Understanding SetUID behavior, effective users, and restricted shell behavior
- Reviewing cron jobs, scheduled scripts, output redirection, and run-as users
- Investigating Git history, branches, tags, ignored files, and SSH-based Git remotes
