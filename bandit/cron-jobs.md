# Bandit Learning: Cron Jobs

## Concept Overview

Bandit demonstrated how scheduled jobs can create, modify, execute, or delete files over time. The operational lesson is to inspect scheduled automation when system state changes unexpectedly.

Cron is not just a scheduling tool. It is also a source of hidden system behavior because jobs may run as a different user, discard output, modify temporary files, or execute scripts from watched directories.

## Commands Used

```bash
ls -la /etc/cron.d
cat /etc/crontab
crontab -l
grep CRON /var/log/syslog
cat /usr/bin/example-cron-script.sh
stat ./script.sh
chmod 700 ./script.sh
mktemp -d
```

## What I Learned

- Cron runs commands on a schedule and often under a specific user.
- System cron files may differ from user crontabs.
- Scheduled scripts can affect files even when no user is logged in.
- Logs and script paths are important for understanding cron behavior.
- `@reboot` jobs run when the cron daemon starts.
- `* * * * *` means a job runs every minute.
- `&> /dev/null` discards both standard output and standard error.
- Some scheduled scripts process files from spool directories and remove them after execution.
- File ownership and execute permissions matter when a scheduled job decides whether to run a script.
- Temporary directories should be created with safe permissions when used for operational testing.

## Operational Notes

### Inspecting System Cron

```bash
ls -la /etc/cron.d
cat /etc/crontab
sudo systemctl status cron
```

System cron files often include the user that the command runs as, unlike normal user crontabs.

### Reading a Scheduled Script

```bash
cat /usr/bin/example-cron-script.sh
```

When reviewing a cron job, identify:

- Schedule
- Run-as user
- Script path
- Input directory
- Output destination
- Error handling
- File ownership checks

### Understanding Output Redirection

```bash
command &> /dev/null
```

This hides both successful output and errors. In production, important scheduled jobs should log enough information for troubleshooting.

## Real-world Infrastructure Relevance

Cron jobs are used for backups, cleanup, report generation, certificate renewals, monitoring scripts, and maintenance tasks. Failed or misconfigured cron jobs can cause delayed operational problems, especially when output is discarded or scripts silently delete temporary files.

## Interview Questions

- Where are system-wide cron jobs configured?
- How do you check a user's crontab?
- Why should cron scripts use absolute paths?
- How would you troubleshoot a scheduled job that did not run?
- What risks can come from writable cron scripts?
- What does `@reboot` mean in cron?
- What does `&> /dev/null` do?
- Why does the run-as user matter for a cron job?
- What should a production cron job log?
