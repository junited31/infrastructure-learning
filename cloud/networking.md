# Cloud Networking

## Purpose

Cloud networking defines how instances communicate with users, other services, and the internet.

## Concepts

- VCN or VPC
- Subnet
- Route table
- Internet gateway
- NAT gateway
- Security list
- Network security group
- Public and private IP address

## Access Path Model

For SSH into a public lab instance, the access path is:

```text
Workstation
    |
    | TCP 22
    v
Public IP
    |
    v
OCI security list or network security group
    |
    v
Subnet route table and internet gateway
    |
    v
Instance network interface
    |
    v
Host firewall and sshd
```

Every layer must allow the connection. A single misconfiguration can make the service unreachable even if the instance is running.

## Troubleshooting Checklist

- Does the instance have the expected private and public IP?
- Does the subnet route to an internet gateway when public access is required?
- Does the security rule allow the correct protocol, port, and source?
- Is the host firewall also allowing the traffic?
- Is the service listening on the expected address and port?
- Is the client testing from an allowed source IP?
- Is the service bound to `0.0.0.0`, the private IP, or only `127.0.0.1`?
- Does the failure look like timeout, refused connection, or authentication failure?

## Commands Used

```bash
ip addr
ip route
sudo ss -tulpn
curl -v http://127.0.0.1
nc -vz PUBLIC_IP 22
```

## Failure Pattern Guide

| Symptom | Likely Layer | First Checks |
|---|---|---|
| Timeout | Cloud firewall, route table, host firewall, wrong source IP | OCI ingress rule, route table, UFW status |
| Connection refused | Host reachable but service not listening | `sudo ss -tulpn`, `systemctl status service` |
| Works on localhost only | Service bind address | `ss -tulpn`, service config |
| SSH prompts but login fails | Authentication or account issue | SSH key, username, `/var/log/auth.log` |
| Works from one network but not another | Source restriction | Security rule source CIDR, client public IP |

## Validation Workflow

1. Confirm the instance IPs.

   ```bash
   ip addr
   ```

2. Confirm the host route.

   ```bash
   ip route
   ```

3. Confirm the service is listening.

   ```bash
   sudo ss -tulpn
   ```

4. Test locally on the instance.

   ```bash
   curl -v http://127.0.0.1:8080
   ```

5. Test remotely from the client.

   ```bash
   nc -vz <public-ip> 8080
   ```

6. Compare the result with OCI security rules and host firewall state.

## Security Notes

- Do not expose management ports to `0.0.0.0/0` unless there is a temporary and documented reason.
- Prefer source-restricted SSH rules for administration.
- Remove test service ingress rules after validation.
- Document every public port with owner, purpose, and rollback.

## Infrastructure Relevance

Cloud networking practice supports firewall validation, SSH access, service exposure, and troubleshooting between cloud control-plane settings and guest operating system state. This is important for infrastructure roles because many incidents require proving whether a failure is caused by routing, firewall policy, listener state, DNS, TLS, or authentication.
