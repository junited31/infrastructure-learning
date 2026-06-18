# Cloud Storage

## Purpose

Cloud storage includes boot volumes, block volumes, object storage, and backups. For Linux operations, block storage and filesystem management are especially important.

## Concepts

- Boot volume: contains the operating system.
- Block volume: attachable disk-like storage for instances.
- Object storage: API-based storage for files and backups.
- Snapshot or backup: point-in-time recovery option.

## Linux Commands

```bash
lsblk
df -h
sudo fdisk -l
findmnt
sudo mount /dev/sdb1 /mnt/data
cat /etc/fstab
```

## Operational Notes

- Confirm device names before formatting or mounting.
- Back up important data before resizing or changing filesystems.
- Use `/etc/fstab` carefully; invalid entries can affect boot.
- Monitor disk capacity and inode usage.

## Infrastructure Relevance

Storage issues commonly cause outages through full disks, missing mounts, failed backups, or incorrect volume attachment.
