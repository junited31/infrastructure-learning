# Linux Processes

## Purpose

Process management helps operators understand what is running, how resources are being used, and why services may be slow or unavailable.

## Commands Used

```bash
ps aux
ps -eo pid,ppid,user,stat,pcpu,pmem,cmd
top
htop
pgrep -a nginx
pidof sshd
kill -TERM 1234
kill -KILL 1234
nice
renice
```

## Process States

| State | Meaning |
|---|---|
| `R` | Running or runnable |
| `S` | Sleeping |
| `D` | Uninterruptible sleep, often I/O wait |
| `Z` | Zombie process |
| `T` | Stopped or traced |

## Operational Workflow

1. Identify the process.

   ```bash
   ps -eo pid,user,pcpu,pmem,cmd --sort=-pcpu | head
   ```

2. Check service ownership.

   ```bash
   systemctl status service-name
   ```

3. Inspect logs.

   ```bash
   journalctl -u service-name --since "30 minutes ago"
   ```

4. Stop gracefully before forcing termination.

   ```bash
   sudo systemctl stop service-name
   sudo kill -TERM PID
   ```

## Infrastructure Relevance

Process inspection supports incident response for high CPU, memory pressure, hung services, failed deployments, and unexpected background jobs.
