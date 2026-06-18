# Port Scanning

## Purpose

Port scanning is used to discover which services are reachable. In operations, it helps validate firewall rules, confirm expected listeners, and identify unexpected exposure.

## Commands Used

```bash
ss -tulpn
nc -vz 203.0.113.10 22
nmap -sV 203.0.113.10
nmap -p 22,80,443 203.0.113.10
```

## Responsible Use

Only scan systems you own or have permission to test. Port scanning can trigger security alerts and may violate policy when performed against unauthorized targets.

## Troubleshooting Workflow

1. Confirm the service is listening on the host.

   ```bash
   sudo ss -tulpn
   ```

2. Test from the client network.

   ```bash
   nc -vz 203.0.113.10 443
   ```

3. Compare results with cloud firewall and host firewall rules.

## Infrastructure Relevance

Port scanning helps operators validate the real exposed surface of a server and detect differences between intended and actual network access.
