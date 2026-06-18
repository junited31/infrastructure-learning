# Bandit Learning: SSL and TLS

## Concept Overview

Bandit included encrypted service connections that required understanding TLS-enabled clients and certificate-aware communication.

## Commands Used

```bash
openssl s_client -connect host:port
openssl s_client -connect host:port -servername host
ncat --ssl host port
```

## What I Learned

- TLS wraps application traffic in encryption.
- Some services require TLS before sending or receiving meaningful data.
- SNI can be required when multiple certificates are served from one endpoint.
- Certificate output includes issuer, subject, validity, and chain details.

## Real-world Infrastructure Relevance

TLS knowledge is important for HTTPS services, internal APIs, monitoring endpoints, certificate renewal, and diagnosing certificate-related outages.

## Interview Questions

- What does TLS provide for a network connection?
- How can you inspect a server certificate from the command line?
- Why is certificate expiration operationally important?
- What is SNI?
- What symptoms might appear when a certificate chain is broken?
