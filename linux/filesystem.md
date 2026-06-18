# Linux Filesystem

## Purpose

This note summarizes the Linux filesystem layout and the commands used to inspect disks, paths, mounts, and storage usage during server operations.

## Key Directories

| Path | Purpose |
|---|---|
| `/` | Root of the filesystem tree |
| `/etc` | System and service configuration |
| `/home` | User home directories |
| `/var` | Logs, caches, spools, and variable application data |
| `/var/log` | System and service logs |
| `/opt` | Optional third-party software |
| `/srv` | Service data managed by the host |
| `/tmp` | Temporary files |
| `/usr` | Installed userland programs and libraries |
| `/proc` | Runtime kernel and process information |

## Commands Used

```bash
pwd
ls -la
tree -L 2 /etc
df -h
du -sh /var/log/*
mount
findmnt
lsblk
stat /etc/hosts
namei -l /srv/app/config.yml
```

## Operational Checks

- Use `df -h` to confirm filesystem capacity.
- Use `du -sh` to locate large directories.
- Use `lsblk` to inspect block devices and attached volumes.
- Use `findmnt` to verify mount points.
- Use `namei -l` to troubleshoot permissions across every parent directory in a path.

## Troubleshooting Examples

Disk full:

```bash
df -h
sudo du -xhd1 /var | sort -h
```

Unexpected mount state:

```bash
findmnt
cat /etc/fstab
```

Path access issue:

```bash
namei -l /srv/app/data/file.txt
```

## Infrastructure Relevance

Filesystem knowledge is important for log growth, volume expansion, service configuration, backup planning, and incident response. Many service failures begin with simple causes such as full disks, missing paths, incorrect mounts, or permissions on parent directories.
