# Bandit Learning: SSL and TLS

## Concept Overview

Bandit included encrypted service connections that required understanding TLS-enabled clients, certificate-aware communication, and the difference between plaintext TCP and TLS-wrapped TCP.

## Commands Used

```bash
openssl s_client -connect host:port
openssl s_client -connect host:port -servername host
openssl s_client -connect host:port -quiet
ncat --ssl host port
curl -Iv https://example.com
```

## What I Learned

- TLS wraps application traffic in encryption.
- Some services require TLS before sending or receiving meaningful data.
- SNI can be required when multiple certificates are served from one endpoint.
- Certificate output includes issuer, subject, validity, and chain details.
- `openssl s_client` is useful for debugging but may print TLS protocol details that are not part of application output.
- `-quiet` can reduce protocol noise when the goal is to inspect the service response.
- `ncat --ssl` can be simpler for interactive testing of TLS-wrapped line-based services.
- A successful TCP connection does not guarantee the correct application protocol was used.

## Operational Notes

### Inspecting a TLS Endpoint

```bash
openssl s_client -connect example.com:443 -servername example.com
```

Review:

- Certificate subject and SANs
- Issuer
- Validity dates
- Verification result
- Whether the chain is complete

### Testing Application Output Over TLS

```bash
ncat --ssl example.com 443
openssl s_client -connect example.com:443 -servername example.com -quiet
```

Use the simpler tool when the task is interaction. Use `openssl s_client` when the task is certificate and handshake inspection.

## Real-world Infrastructure Relevance

TLS knowledge is important for HTTPS services, internal APIs, monitoring endpoints, certificate renewal, and diagnosing certificate-related outages. An operator should be able to determine whether the problem is port reachability, TLS negotiation, certificate validity, hostname mismatch, or application response.

## Interview Questions

- What does TLS provide for a network connection?
- How can you inspect a server certificate from the command line?
- Why is certificate expiration operationally important?
- What is SNI?
- What symptoms might appear when a certificate chain is broken?
- What is the difference between `nc` and `ncat --ssl`?
- Why can `openssl s_client` show output that is not application data?
- Why does hostname validation matter for TLS?
