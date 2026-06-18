# Linux Permissions

Linux permissions define who can read, modify, or execute files and directories. For infrastructure operations, permissions are a daily concern because incorrect ownership or access modes can break services, expose secrets, or prevent automation from running.

## Core Concepts

Every Linux file has:

- An owner user
- An owner group
- Permission bits for user, group, and others
- Optional special permission bits such as SetUID, SetGID, and sticky bit

Permission groups are commonly displayed by `ls -l`:

```text
-rw-r----- 1 appuser appgroup 2048 Jun 18 10:00 config.yml
```

This means:

- `-` indicates a regular file
- `rw-` allows the owner to read and write
- `r--` allows the group to read
- `---` gives others no access
- `appuser` is the owning user
- `appgroup` is the owning group

## File Permission Bits

| Permission | Symbol | Numeric Value | File Meaning |
|---|---:|---:|---|
| Read | `r` | 4 | View file contents |
| Write | `w` | 2 | Modify file contents |
| Execute | `x` | 1 | Run file as a program or script |

Common file modes:

| Mode | Meaning | Common Use |
|---:|---|---|
| `600` | Owner read/write only | Private keys, sensitive config |
| `644` | Owner write, everyone read | Public config, static files |
| `640` | Owner write, group read | Service config shared with admin group |
| `755` | Owner write, everyone execute | Scripts or binaries |
| `700` | Owner full access only | Private directories or scripts |

## Directory Permission Bits

Directory permissions behave differently from file permissions:

- `r` allows listing names in the directory
- `w` allows creating, deleting, or renaming entries
- `x` allows entering the directory and accessing known file paths

For directories, execute permission is required to traverse the path.

Example:

```bash
chmod 750 /opt/app
chown appuser:appgroup /opt/app
```

This allows:

- `appuser` full access
- members of `appgroup` to read and enter
- all other users no access

## Ownership Commands

Inspect ownership:

```bash
ls -l /etc/ssh/sshd_config
stat /etc/ssh/sshd_config
```

Change owner:

```bash
sudo chown appuser /opt/app/config.yml
```

Change owner and group:

```bash
sudo chown appuser:appgroup /opt/app/config.yml
```

Change ownership recursively:

```bash
sudo chown -R appuser:appgroup /opt/app
```

Recursive ownership changes should be used carefully. On production systems, confirm the target path before running recursive commands.

## Permission Commands

Set numeric mode:

```bash
chmod 640 /opt/app/config.yml
```

Add execute permission for the owner:

```bash
chmod u+x script.sh
```

Remove all access for others:

```bash
chmod o-rwx secret.txt
```

Set directory permissions recursively:

```bash
find /opt/app -type d -exec chmod 750 {} \;
find /opt/app -type f -exec chmod 640 {} \;
```

This is safer than applying one mode recursively to both files and directories.

## Special Permission Bits

### SetUID

SetUID causes an executable to run with the permissions of the file owner.

Example display:

```text
-rwsr-xr-x 1 root root 68208 Jan 01 12:00 /usr/bin/passwd
```

Infrastructure relevance:

- Legitimate SetUID binaries support privileged operations for normal users.
- Unexpected SetUID files may indicate privilege escalation risk.

Find SetUID files:

```bash
find / -perm -4000 -type f 2>/dev/null
```

### SetGID

SetGID on a directory causes new files to inherit the directory group.

```bash
sudo chmod g+s /srv/shared
```

This is useful for shared operational directories where multiple administrators or service accounts need group-based access.

### Sticky Bit

The sticky bit allows users to create files in a shared directory but prevents them from deleting files owned by other users.

Example:

```text
drwxrwxrwt 10 root root 4096 Jun 18 10:00 /tmp
```

## SSH Permission Requirements

SSH is strict about private key permissions.

Common expected modes:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/authorized_keys
```

If permissions are too open, SSH may reject the key for safety.

## Troubleshooting Workflow

When a service cannot read a file or execute a script:

1. Identify the service user.

   ```bash
   ps -eo user,pid,cmd | grep service-name
   systemctl cat service-name
   ```

2. Check the file and parent directory permissions.

   ```bash
   namei -l /opt/app/config.yml
   ls -l /opt/app/config.yml
   ```

3. Verify group membership.

   ```bash
   id appuser
   getent group appgroup
   ```

4. Check logs for permission errors.

   ```bash
   journalctl -u service-name --since "1 hour ago"
   ```

5. Apply the minimum required permission change.

   ```bash
   sudo chown appuser:appgroup /opt/app/config.yml
   sudo chmod 640 /opt/app/config.yml
   ```

## Operational Examples

### Secure a Private Key

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
```

Reason: private keys should only be readable by the account that uses them.

### Allow a Service Group to Read Configuration

```bash
sudo chown root:appgroup /etc/app/app.conf
sudo chmod 640 /etc/app/app.conf
```

Reason: root controls the file, while the application group can read it.

### Prepare a Shared Deployment Directory

```bash
sudo groupadd deploy
sudo mkdir -p /srv/deploy
sudo chown root:deploy /srv/deploy
sudo chmod 2775 /srv/deploy
```

Reason: SetGID keeps new files associated with the `deploy` group.

## Real-World Infrastructure Relevance

Strong permission management supports:

- Secure handling of SSH keys and service credentials
- Controlled access for operators and service accounts
- Reliable service startup after deployments
- Faster root cause analysis for `Permission denied` errors
- Reduced risk from overly broad access such as `777`

## Interview Questions

- What is the difference between file execute permission and directory execute permission?
- Why should private SSH keys usually be set to `600`?
- How would you troubleshoot a service that cannot read its configuration file?
- What is the risk of using `chmod -R 777`?
- What does the SetUID bit do, and why should it be monitored?
