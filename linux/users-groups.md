# Linux Users and Groups

## Purpose

Linux users and groups control local identity, access boundaries, service accounts, and administrative privileges.

## Common Commands

```bash
id
whoami
getent passwd
getent group
sudo adduser infraadmin
sudo usermod -aG sudo infraadmin
sudo groupadd deploy
sudo usermod -aG deploy infraadmin
sudo passwd -l serviceuser
```

## Important Files

| File | Purpose |
|---|---|
| `/etc/passwd` | Local user records |
| `/etc/shadow` | Password hashes and password aging data |
| `/etc/group` | Local group records |
| `/etc/sudoers` | Sudo policy |
| `/etc/sudoers.d/` | Drop-in sudo policy files |

## Service Accounts

Service accounts should have only the access needed to run the service.

Example:

```bash
sudo useradd --system --home /var/lib/app --shell /usr/sbin/nologin appsvc
sudo chown -R appsvc:appsvc /var/lib/app
```

## Sudo Management

Check sudo access:

```bash
sudo -l
```

Edit sudo policy safely:

```bash
sudo visudo
```

Use `/etc/sudoers.d/` for focused policy files instead of editing large shared configuration blocks.

## Troubleshooting

- Confirm identity with `id username`.
- Confirm group membership after login; users may need a new session after group changes.
- Check file ownership and parent directory permissions.
- Review `/var/log/auth.log` for authentication and sudo events on Ubuntu.

## Infrastructure Relevance

User and group control is central to least privilege, break-glass access, service isolation, operational accountability, and secure SSH administration.
