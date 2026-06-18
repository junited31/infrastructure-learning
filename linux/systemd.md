# systemd Service Management

## Purpose

`systemd` manages services, dependencies, logs, timers, and boot behavior on many Linux distributions including Ubuntu.

## Common Commands

```bash
systemctl status ssh
sudo systemctl start docker
sudo systemctl stop docker
sudo systemctl restart docker
sudo systemctl reload ssh
sudo systemctl enable docker
sudo systemctl disable docker
systemctl is-active docker
systemctl is-enabled docker
systemctl --failed
journalctl -u docker --since "1 hour ago"
```

## Unit File Locations

| Path | Purpose |
|---|---|
| `/lib/systemd/system/` | Package-provided unit files |
| `/etc/systemd/system/` | Local unit files and overrides |
| `/etc/systemd/system/*.d/` | Drop-in overrides |

## Safe Change Workflow

```bash
sudo systemctl edit service-name
sudo systemctl daemon-reload
sudo systemctl restart service-name
systemctl status service-name --no-pager
journalctl -u service-name -n 50 --no-pager
```

## Troubleshooting

- Use `systemctl --failed` to find failed services.
- Use `journalctl -xeu service-name` for detailed failure context.
- Confirm environment variables and working directory in the unit file.
- Check whether the service user can access required files.

## Infrastructure Relevance

Data center and cloud operations require reliable service control. Understanding `systemd` helps with boot issues, service restarts, log review, dependency failures, and runner or container host management.
