# GitHub Runner Service Management

## Purpose

Once installed, a self-hosted runner should be treated like any other Linux service: monitored, restarted safely, patched, and removed cleanly when retired.

## Commands Used

```bash
systemctl status actions.runner.* --no-pager
journalctl -u 'actions.runner.*' -n 100 --no-pager
sudo systemctl restart actions.runner.OWNER-REPOSITORY.RUNNER.service
sudo ./svc.sh status
sudo ./svc.sh stop
sudo ./svc.sh start
```

## Operational Checks

- Runner is visible and idle in GitHub.
- `systemd` service is active.
- Runner labels match workflow `runs-on` labels.
- Host has enough disk space.
- Required tools such as Docker or Git are installed.
- Logs do not show repeated registration or connectivity errors.

## Infrastructure Relevance

Runner service management demonstrates Linux service operations, automation reliability, log review, and host maintenance responsibilities.
