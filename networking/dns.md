# DNS

## Purpose

DNS translates names into records such as IP addresses. Infrastructure operators use DNS checks to determine whether failures are caused by name resolution, routing, service availability, or application behavior.

## Common Record Types

| Type | Purpose |
|---|---|
| `A` | IPv4 address |
| `AAAA` | IPv6 address |
| `CNAME` | Alias to another DNS name |
| `MX` | Mail exchange |
| `TXT` | Text records, often verification or policy |
| `NS` | Authoritative name servers |

## Commands Used

```bash
dig example.com
dig A example.com
dig +short example.com
dig NS example.com
nslookup example.com
resolvectl status
cat /etc/resolv.conf
```

## Troubleshooting

- Compare results from different resolvers.
- Check TTL values when expecting a recent change.
- Confirm whether the issue is DNS resolution or service connectivity.
- Use `curl -v` after DNS resolves to test the application port.

## Infrastructure Relevance

DNS issues can look like application outages. A clear DNS workflow helps separate name resolution problems from network, firewall, TLS, or service failures.
