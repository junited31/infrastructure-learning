# TLS

## Purpose

TLS protects network traffic by encrypting communication and validating server identity through certificates.

## Concepts

- Certificate: proves the identity of a server name.
- Private key: secret key used by the server.
- Certificate chain: links the server certificate to a trusted root.
- Expiration: certificates are only valid for a defined time.
- SNI: allows a server to present the correct certificate for a requested hostname.

## Commands Used

```bash
openssl s_client -connect example.com:443 -servername example.com
curl -Iv https://example.com
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | openssl x509 -noout -dates -subject -issuer
```

## Troubleshooting

- Confirm the certificate common name or SAN matches the hostname.
- Check expiration dates.
- Confirm the full certificate chain is served.
- Use SNI with `openssl s_client` when testing virtual hosts.
- Check whether firewall or load balancer changes affected port 443.

## Infrastructure Relevance

TLS is important for secure administration portals, APIs, package repositories, and customer-facing services. Certificate expiration or chain issues can create outages even when the service process is healthy.
