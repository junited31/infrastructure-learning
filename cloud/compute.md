# Cloud Compute

## Purpose

Compute instances provide virtual servers for running operating systems and workloads.

## Key Tasks

- Select an image and shape.
- Attach SSH public keys during provisioning.
- Assign a public IP only when needed.
- Apply least-required ingress rules.
- Monitor CPU, memory, disk, and service health.
- Reboot or stop instances safely.

## Instance Information to Record

| Field | Why It Matters |
|---|---|
| Region and availability domain | Helps identify physical or logical placement |
| Image name and version | Explains OS defaults and package behavior |
| Shape | Defines CPU and memory limits |
| Boot volume size | Affects disk pressure and patch capacity |
| Public and private IPs | Required for access and troubleshooting |
| Subnet and VCN | Connects compute behavior to network policy |
| SSH username and key name | Supports access recovery and documentation |

## Linux Validation Commands

```bash
hostnamectl
uptime
uname -a
df -h
free -h
systemctl --failed
```

## Post-Provisioning Validation

After first SSH access, validate the server from the guest OS:

```bash
hostnamectl
ip addr
ip route
df -h
free -h
systemctl --failed
journalctl -p warning --since "1 hour ago"
```

Expected outcome:

- Hostname and OS version are known.
- Network interface has the expected private address.
- Default route exists.
- Disk and memory are not under immediate pressure.
- No unexpected failed services are present.
- Recent warnings are understood or documented.

## Operational Actions

### Reboot Check

```bash
sudo reboot
```

After reconnecting:

```bash
uptime
systemctl --failed
journalctl -b -p warning
```

This confirms that the system returns to a healthy state after a normal reboot.

### Resource Pressure Check

```bash
uptime
free -h
df -h
ps -eo user,pid,pcpu,pmem,cmd --sort=-pcpu | head
```

These commands help identify CPU load, memory pressure, disk usage, and high-impact processes.

### Safe Stop or Termination Preparation

Before stopping or rebuilding an instance:

- Confirm whether the public IP will change.
- Confirm important data is not only on the instance filesystem.
- Confirm boot volume or block volume backup needs.
- Remove or disable external dependencies such as self-hosted runners.
- Document the rollback or rebuild path.

## Infrastructure Relevance

Compute operations map closely to data center server work: provisioning, access validation, patching, resource checks, service recovery, reboot validation, and documentation. The cloud console shows the resource state, but the Linux host must still be inspected directly.
