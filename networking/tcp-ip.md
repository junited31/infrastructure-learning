# TCP/IP Fundamentals

## Purpose

TCP/IP knowledge helps operators understand how systems communicate, how services listen on ports, and where to inspect failures.

## Core Concepts

- IP addresses identify hosts or interfaces.
- TCP provides reliable connection-oriented communication.
- UDP provides connectionless communication.
- Ports identify application services on a host.
- Routes determine the next hop for traffic.

## Commands Used

```bash
ip addr
ip route
ss -tulpn
ping -c 4 8.8.8.8
traceroute example.com
curl -v http://example.com
nc -vz example.com 443
```

## Common Ports

| Port | Protocol | Service |
|---:|---|---|
| 22 | TCP | SSH |
| 53 | TCP/UDP | DNS |
| 80 | TCP | HTTP |
| 123 | UDP | NTP |
| 443 | TCP | HTTPS |
| 3306 | TCP | MySQL |

## Infrastructure Relevance

TCP/IP fundamentals support cloud firewall configuration, service checks, load balancer troubleshooting, SSH access, DNS validation, and incident response.
