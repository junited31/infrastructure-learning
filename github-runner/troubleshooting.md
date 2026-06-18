# GitHub Runner Troubleshooting

## Purpose

Troubleshooting a self-hosted runner requires checking GitHub registration, service state, labels, host dependencies, network egress, and workflow permissions.

## Checklist

```bash
systemctl status actions.runner.* --no-pager
journalctl -u 'actions.runner.*' --since "1 hour ago"
df -h
free -h
curl -I https://github.com
git --version
docker version
```

## Common Issues

| Symptom | Likely Cause |
|---|---|
| Workflow waiting for runner | Label mismatch or runner offline |
| Runner offline | Service stopped or network issue |
| Job fails on command not found | Missing host dependency |
| Docker permission denied | Runner user not in Docker group or new session needed |
| Disk full | Workflow artifacts, caches, or Docker images |

## Infrastructure Relevance

Self-hosted runner issues are operational problems involving identity, services, dependencies, disk space, network connectivity, and logs.
