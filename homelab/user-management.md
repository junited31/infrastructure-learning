# Homelab User Management

## Purpose

This note documents user and group practices for the homelab server.

## Administrative User

```bash
sudo adduser infraadmin
sudo usermod -aG sudo infraadmin
```

## SSH Key Setup

```bash
sudo mkdir -p /home/infraadmin/.ssh
sudo cp /home/ubuntu/.ssh/authorized_keys /home/infraadmin/.ssh/authorized_keys
sudo chown -R infraadmin:infraadmin /home/infraadmin/.ssh
sudo chmod 700 /home/infraadmin/.ssh
sudo chmod 600 /home/infraadmin/.ssh/authorized_keys
```

## Service Users

Create service users for long-running services when separation is useful:

```bash
sudo useradd --system --home /var/lib/app --shell /usr/sbin/nologin appsvc
```

## Infrastructure Relevance

User management supports least privilege, accountability, safer automation, and cleaner separation between administration and service execution.
