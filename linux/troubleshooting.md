# Linux Troubleshooting

## Purpose

This page provides a practical first-response checklist for Linux server issues.

## Initial Checks

```bash
hostnamectl
uptime
date
who
df -h
free -h
systemctl --failed
journalctl -p warning --since "1 hour ago"
```

## Service Issue Workflow

```bash
systemctl status service-name --no-pager
journalctl -u service-name --since "30 minutes ago"
ss -tulpn
ps -eo user,pid,pcpu,pmem,cmd --sort=-pcpu | head
```

## Disk Issue Workflow

```bash
df -h
df -i
sudo du -xhd1 / | sort -h
sudo du -xhd1 /var | sort -h
```

## Permission Issue Workflow

```bash
namei -l /path/to/file
ls -l /path/to/file
id serviceuser
journalctl -u service-name --since "30 minutes ago"
```

## Network Issue Workflow

```bash
ip addr
ip route
ss -tulpn
ping -c 4 8.8.8.8
dig example.com
curl -v http://127.0.0.1:8080
```

## Infrastructure Relevance

Good troubleshooting is structured: define the symptom, check recent changes, isolate layers, read logs, test one assumption at a time, and document the fix.
