# Bandit Learning: Networking Tools and Port Scanning

## Concept Overview

Bandit introduced practical use of networking tools to connect to local and remote services, inspect ports, and understand how simple protocols exchange data.

## Commands Used

```bash
nc host port
telnet host port
nmap localhost
ss -tulpn
curl
```

## What I Learned

- Network services listen on specific TCP or UDP ports.
- `nc` can test raw TCP connectivity.
- Port scanning identifies which services are reachable.
- Localhost services may be available only from the same machine.
- Connection refused and timeout indicate different failure modes.

## Real-world Infrastructure Relevance

These skills support firewall validation, service checks, load balancer troubleshooting, and incident response when a service is expected to be reachable but is not.

## Interview Questions

- What is the difference between timeout and connection refused?
- How can you check whether a service is listening locally?
- Why should port scanning only be done with authorization?
- What does `localhost` mean?
- How does `nc` help troubleshoot a service?
