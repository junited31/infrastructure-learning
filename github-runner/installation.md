# GitHub Actions Self-Hosted Runner Installation

A GitHub Actions self-hosted runner allows workflow jobs to execute on a server that I operate. In an infrastructure learning environment, it is useful for practicing Linux service management, secure server access, automation reliability, and operational troubleshooting.

This document describes a typical Ubuntu-based installation workflow. Exact runner download links and registration tokens should be taken from the target GitHub repository or organization at installation time.

## Operational Goal

Install a self-hosted runner on an Ubuntu server so that GitHub Actions jobs can run on managed infrastructure while the runner process is controlled as a `systemd` service.

## Prerequisites

- Ubuntu server with SSH access
- A non-root administrative user with `sudo`
- Network egress to GitHub
- GitHub repository or organization administrator access
- Runner registration token from GitHub
- Basic packages installed:

```bash
sudo apt update
sudo apt install -y curl tar git
```

## Create a Dedicated Runner User

Running the service under a dedicated account reduces the blast radius of workflow execution.

```bash
sudo useradd --create-home --shell /bin/bash github-runner
sudo usermod -aG docker github-runner
```

The Docker group should only be added if workflows need Docker access. Membership in the Docker group is powerful because it can effectively allow root-level control of the host.

Switch to the runner user:

```bash
sudo su - github-runner
```

Create a runner directory:

```bash
mkdir -p ~/actions-runner
cd ~/actions-runner
```

## Download the Runner

GitHub provides the current download command in:

```text
Repository -> Settings -> Actions -> Runners -> New self-hosted runner
```

Example pattern:

```bash
curl -o actions-runner-linux-x64.tar.gz -L https://github.com/actions/runner/releases/download/vX.Y.Z/actions-runner-linux-x64-X.Y.Z.tar.gz
tar xzf ./actions-runner-linux-x64.tar.gz
```

After download, verify that the extracted directory contains runner scripts such as:

```text
config.sh
run.sh
svc.sh
```

## Configure the Runner

The GitHub UI provides a registration command with a temporary token.

Example pattern:

```bash
./config.sh --url https://github.com/OWNER/REPOSITORY --token REGISTRATION_TOKEN
```

Recommended labels:

```text
linux,ubuntu,self-hosted,homelab
```

For infrastructure practice, labels help route jobs to the intended environment and make runner purpose visible in GitHub.

During configuration, confirm:

- Runner name clearly identifies the host or environment
- Runner group is appropriate
- Labels are accurate
- Work directory is acceptable, commonly `_work`

Do not commit runner tokens or credentials to a repository.

## Install as a systemd Service

Exit the runner user session if needed, then install the service from the runner directory using `sudo`:

```bash
sudo ./svc.sh install github-runner
sudo ./svc.sh start
```

Check service status:

```bash
sudo ./svc.sh status
systemctl status actions.runner.* --no-pager
```

Enablement is normally handled by the service installation script, but it can be confirmed with:

```bash
systemctl is-enabled actions.runner.*
```

## Validate Runner Registration

In GitHub, check:

```text
Repository -> Settings -> Actions -> Runners
```

Expected status:

```text
Idle
```

If the runner is offline:

```bash
journalctl -u 'actions.runner.*' --since "30 minutes ago"
```

Also confirm outbound connectivity:

```bash
curl -I https://github.com
```

## Test with a Minimal Workflow

Example workflow:

```yaml
name: self-hosted-runner-check

on:
  workflow_dispatch:

jobs:
  check:
    runs-on: [self-hosted, linux]
    steps:
      - name: Print host info
        run: |
          hostname
          whoami
          uname -a
          df -h
```

This confirms that:

- GitHub can dispatch a job to the runner
- The runner user can execute shell commands
- The host has basic command availability
- Output is visible in workflow logs

## Security Considerations

- Use a dedicated runner user rather than a personal admin account.
- Avoid running untrusted workflows on a persistent self-hosted runner.
- Limit repository access to trusted maintainers.
- Keep the host patched.
- Review workflow permissions.
- Avoid storing long-lived secrets on the runner filesystem.
- Remove unused runners from GitHub and the host.

## Maintenance Commands

Check service:

```bash
systemctl status actions.runner.* --no-pager
```

View logs:

```bash
journalctl -u 'actions.runner.*' -n 100 --no-pager
```

Restart service:

```bash
sudo systemctl restart actions.runner.OWNER-REPOSITORY.RUNNER.service
```

Update system packages:

```bash
sudo apt update
sudo apt upgrade
```

Remove runner service:

```bash
sudo ./svc.sh stop
sudo ./svc.sh uninstall
```

Remove runner registration:

```bash
./config.sh remove --token REMOVE_TOKEN
```

## Troubleshooting Checklist

- Is the runner service active?
- Does the server have outbound access to GitHub?
- Is the runner visible in GitHub settings?
- Do workflow `runs-on` labels match the runner labels?
- Does the runner user have required permissions?
- Are job failures caused by host dependencies, secrets, or workflow syntax?
- Are disk space and memory sufficient?

## Real-World Infrastructure Relevance

Operating a self-hosted runner is practical infrastructure experience because it combines identity, service management, host security, network egress, package maintenance, logs, and incident response. It also teaches the operational difference between a managed SaaS runner and a persistent machine that must be patched, monitored, and controlled.
