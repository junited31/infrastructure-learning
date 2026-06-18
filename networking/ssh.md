# SSH Operations

Secure Shell, or SSH, is the primary remote administration protocol for Linux servers. In infrastructure operations, SSH is used to access cloud instances, inspect logs, manage services, transfer files, and recover systems when automation fails.

## Core Concepts

SSH provides encrypted remote access between a client and a server.

Common components:

- SSH client: the local command used to connect, usually `ssh`
- SSH server: the remote daemon, usually `sshd`
- User account: the Linux account being accessed
- Authentication method: password, public key, or certificate-based authentication
- Host key: the server identity key used to detect unexpected server changes

Typical connection format:

```bash
ssh ubuntu@203.0.113.10
```

## Key-Based Authentication

Key-based authentication uses a private key on the client and a matching public key on the server.

Generate an Ed25519 key:

```bash
ssh-keygen -t ed25519 -C "infrastructure-admin"
```

Expected files:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

The private key stays on the client. The public key is placed in the remote user's `~/.ssh/authorized_keys` file.

Recommended permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/authorized_keys
```

## Connecting with a Specific Key

```bash
ssh -i ~/.ssh/oci_admin_key ubuntu@203.0.113.10
```

Verbose mode is useful for troubleshooting:

```bash
ssh -vvv -i ~/.ssh/oci_admin_key ubuntu@203.0.113.10
```

Important output areas to review:

- Which identity files were offered
- Whether the server accepted the key
- Whether authentication failed before or after key exchange
- Whether the client refused to use a key because permissions were too open

## SSH Client Configuration

The SSH client config file can simplify repeated connections.

Example `~/.ssh/config` entry:

```sshconfig
Host oci-lab
    HostName 203.0.113.10
    User ubuntu
    IdentityFile ~/.ssh/oci_admin_key
    IdentitiesOnly yes
```

Connect using:

```bash
ssh oci-lab
```

Operational value:

- Reduces connection mistakes
- Documents the expected user and key
- Makes repeated administration faster

## Server-Side SSH Files

Common server-side paths:

```text
/etc/ssh/sshd_config
~/.ssh/authorized_keys
/var/log/auth.log
```

Check server status:

```bash
systemctl status ssh
```

Review logs on Ubuntu:

```bash
sudo journalctl -u ssh --since "1 hour ago"
sudo tail -n 100 /var/log/auth.log
```

Validate SSH daemon configuration before reload:

```bash
sudo sshd -t
```

Reload after a safe config change:

```bash
sudo systemctl reload ssh
```

## Common Hardening Settings

Typical server hardening options in `/etc/ssh/sshd_config`:

```sshconfig
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

Before disabling password login, confirm that key-based login works in a second active session. This avoids locking yourself out of the server.

## File Transfer with SSH

Copy a file to a server:

```bash
scp ./config.yml ubuntu@203.0.113.10:/tmp/config.yml
```

Copy a file from a server:

```bash
scp ubuntu@203.0.113.10:/var/log/syslog ./syslog.copy
```

Use `rsync` for repeatable transfers:

```bash
rsync -avz ./site/ ubuntu@203.0.113.10:/srv/site/
```

## Port Forwarding

Local port forwarding can expose a remote service only to the local workstation.

```bash
ssh -L 8080:127.0.0.1:80 ubuntu@203.0.113.10
```

Use case:

- Inspect a service listening only on the remote server's loopback interface
- Avoid opening a temporary public firewall rule

## Troubleshooting Workflow

### 1. Confirm Network Reachability

```bash
ping 203.0.113.10
nc -vz 203.0.113.10 22
```

If the port is unreachable, check:

- Cloud firewall or security list rules
- Host firewall rules
- Server public IP address
- Whether `sshd` is running

### 2. Run SSH Verbose Mode

```bash
ssh -vvv -i ~/.ssh/oci_admin_key ubuntu@203.0.113.10
```

Look for:

- `Permission denied (publickey)`
- `Offering public key`
- `Server accepts key`
- `Bad permissions`
- `Connection timed out`
- `Connection refused`

### 3. Check Local Key Permissions

```bash
ls -ld ~/.ssh
ls -l ~/.ssh/oci_admin_key
chmod 700 ~/.ssh
chmod 600 ~/.ssh/oci_admin_key
```

### 4. Check the Remote Account

From an existing session or console:

```bash
id ubuntu
ls -ld /home/ubuntu /home/ubuntu/.ssh
ls -l /home/ubuntu/.ssh/authorized_keys
```

### 5. Check Server Logs

```bash
sudo journalctl -u ssh --since "30 minutes ago"
sudo grep sshd /var/log/auth.log | tail -n 50
```

## Common Failure Patterns

| Symptom | Likely Area | Check |
|---|---|---|
| Connection timed out | Network path blocked | Cloud firewall, route table, public IP |
| Connection refused | SSH daemon not listening | `systemctl status ssh`, `ss -tlnp` |
| Permission denied | Authentication failed | Key, user, authorized keys |
| Host key warning | Server identity changed | Confirm rebuild or IP reuse before accepting |
| Bad permissions | Key or `.ssh` mode too open | `chmod 700 ~/.ssh`, `chmod 600 key` |

## Real-World Infrastructure Relevance

SSH troubleshooting is directly relevant to data center and cloud operations because server access problems can block incident response. A strong SSH workflow helps an operator determine whether the issue is identity, file permissions, daemon state, routing, firewall policy, or server availability.

## Interview Questions

- What happens during SSH key-based authentication?
- How would you troubleshoot `Permission denied (publickey)`?
- Why should `PasswordAuthentication` often be disabled on cloud servers?
- What is the purpose of the known hosts file?
- How can SSH port forwarding help during troubleshooting?
