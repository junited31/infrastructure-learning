# Network Troubleshooting

## Purpose

Network troubleshooting separates failures across local host configuration, routing, DNS, firewalls, listening services, and remote dependencies.

## Layered Workflow

```bash
ip addr
ip route
ping -c 4 8.8.8.8
dig example.com
nc -vz example.com 443
curl -v https://example.com
ss -tulpn
```

## Common Symptoms

| Symptom | Likely Area |
|---|---|
| Timeout | Firewall, route, remote service down |
| Connection refused | Host reachable but service not listening |
| DNS failure | Resolver, record, or domain issue |
| TLS warning | Certificate, hostname, chain, or expiry |
| Works locally, fails remotely | Bind address or firewall rule |

## Infrastructure Relevance

Data center and cloud operations often require quick isolation of whether a problem is on the host, network path, firewall, DNS, or application service.
