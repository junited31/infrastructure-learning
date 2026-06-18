# Bandit Learning: Cron Jobs

## Concept Overview

Bandit demonstrated how scheduled jobs can create, modify, or expose files over time. The operational lesson is to inspect scheduled automation when system state changes unexpectedly.

## Commands Used

```bash
ls -la /etc/cron.d
cat /etc/crontab
crontab -l
grep CRON /var/log/syslog
```

## What I Learned

- Cron runs commands on a schedule and often under a specific user.
- System cron files may differ from user crontabs.
- Scheduled scripts can affect files even when no user is logged in.
- Logs and script paths are important for understanding cron behavior.

## Real-world Infrastructure Relevance

Cron jobs are used for backups, cleanup, report generation, certificate renewals, and maintenance tasks. Failed or misconfigured cron jobs can cause delayed operational problems.

## Interview Questions

- Where are system-wide cron jobs configured?
- How do you check a user's crontab?
- Why should cron scripts use absolute paths?
- How would you troubleshoot a scheduled job that did not run?
- What risks can come from writable cron scripts?
