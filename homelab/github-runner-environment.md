# Homelab GitHub Runner Environment

## Purpose

The GitHub Actions self-hosted runner is used to practice automation operations on a Linux server.

## Design Notes

- Runner is installed under a dedicated account where practical.
- Runner labels identify the environment, such as `linux`, `ubuntu`, and `homelab`.
- The service is managed by `systemd`.
- Workflow access is limited to trusted repositories.

## Operations

```bash
systemctl status actions.runner.* --no-pager
journalctl -u 'actions.runner.*' -n 100 --no-pager
df -h
curl -I https://github.com
```

## Maintenance

- Keep the host patched.
- Review runner logs after workflow failures.
- Remove stale work directories and Docker artifacts when needed.
- Remove the runner from GitHub before deleting the host.

## Infrastructure Relevance

This environment connects Linux services, GitHub automation, network egress, credentials, host dependencies, and operational troubleshooting.
