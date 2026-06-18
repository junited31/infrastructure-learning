# Cron Jobs

## Purpose

Cron schedules recurring commands. It is commonly used for maintenance, backups, cleanup tasks, and periodic scripts.

## Common Locations

| Location | Purpose |
|---|---|
| `crontab -e` | Current user's crontab |
| `/etc/crontab` | System-wide cron file |
| `/etc/cron.d/` | Package or admin cron definitions |
| `/etc/cron.daily/` | Daily scripts |
| `/etc/cron.hourly/` | Hourly scripts |

## Commands Used

```bash
crontab -l
sudo crontab -l
sudo ls -la /etc/cron.d
sudo systemctl status cron
grep CRON /var/log/syslog
```

## Cron Format

```text
* * * * * command
| | | | |
| | | | day of week
| | | month
| | day of month
| hour
minute
```

Example:

```cron
0 2 * * * /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1
```

## Troubleshooting

- Use absolute paths in scripts.
- Redirect stdout and stderr to a log file.
- Confirm the script has execute permission.
- Confirm the cron user has required file access.
- Check `/var/log/syslog` on Ubuntu for cron execution records.

## Infrastructure Relevance

Cron jobs can be responsible for backups, cleanup, certificate renewals, and scheduled maintenance. When they fail silently, infrastructure reliability can degrade over time.
