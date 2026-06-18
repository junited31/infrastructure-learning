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

## Troubleshooting Checklist

- Does the instance have the expected private and public IP?
- Does the subnet route to an internet gateway when public access is required?
- Does the security rule allow the correct protocol, port, and source?
- Is the host firewall also allowing the traffic?
- Is the service listening on the expected address and port?

## Commands Used

```bash
ip addr
ip route
sudo ss -tulpn
curl -v http://127.0.0.1
nc -vz PUBLIC_IP 22
```

## Infrastructure Relevance

Cloud networking practice supports firewall validation, SSH access, service exposure, and troubleshooting between cloud control-plane settings and guest operating system state.
