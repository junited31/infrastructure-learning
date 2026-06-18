# Bandit Learning: Networking Tools and Port Scanning

## Concept Overview

Bandit introduced practical use of networking tools to connect to local and remote services, inspect ports, understand simple request-response protocols, and test whether a service expects plaintext or encrypted input.

The important infrastructure habit is to test one layer at a time: local listener, network reachability, protocol expectations, and service response.

## Commands Used

```bash
nc host port
nc -z localhost 31000-32000
nc -l -p 9000
telnet host port
ncat --ssl host port
nmap localhost
ss -tulpn
curl
printf 'health-check\n' | ncat localhost 30002
```

## What I Learned

- Network services listen on specific TCP or UDP ports.
- `nc` can test raw TCP connectivity.
- `nc -z` can check whether a TCP port is open without sending application data.
- `nc -l -p PORT` can open a simple listener for local testing.
- `telnet` can still be useful for simple plaintext TCP protocol checks.
- `ncat --ssl` is useful when a service requires TLS but still behaves like a simple line-based protocol.
- Port scanning identifies which services are reachable.
- Localhost services may be available only from the same machine.
- Connection refused and timeout indicate different failure modes.
- Some services require a precise input format, so controlled test input is useful when validating service behavior.

## Operational Notes

### Checking a Listening Port

On the server:

```bash
ss -tulpn
```

From the client:

```bash
nc -vz example.com 443
```

If the server is listening locally but remote access fails, check host firewall rules, cloud security rules, bind address, and routing.

### Scanning a Small Authorized Range

```bash
nc -z localhost 31000-32000
```

Only scan systems and ranges where testing is authorized. In operations, this technique is useful for validating expected service exposure and detecting accidental open ports.

### Testing a Local Listener

One terminal can listen:

```bash
nc -l -p 9000
```

Another terminal can connect:

```bash
nc localhost 9000
```

This is useful for understanding basic TCP behavior and checking whether local processes can communicate.

## Real-world Infrastructure Relevance

These skills support firewall validation, service checks, load balancer troubleshooting, and incident response when a service is expected to be reachable but is not. They also help distinguish:

- Service not running
- Service listening only on localhost
- Firewall blocked
- Wrong port
- Wrong protocol
- TLS expected but plaintext sent

## Interview Questions

- What is the difference between timeout and connection refused?
- How can you check whether a service is listening locally?
- Why should port scanning only be done with authorization?
- What does `localhost` mean?
- How does `nc` help troubleshoot a service?
- What is the difference between testing a port and validating an application protocol?
- Why might a service work locally but fail from another host?
- When would you use `ss` instead of scanning from the outside?
