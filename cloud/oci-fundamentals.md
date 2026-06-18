# OCI Fundamentals

## Purpose

Oracle Cloud Infrastructure provides compute, networking, storage, identity, and managed services. This note focuses on the infrastructure concepts used in a small Linux homelab.

## Core Concepts

- Region: geographic area containing OCI resources.
- Availability domain: isolated facility within a region.
- Compartment: logical container for organizing and controlling access to resources.
- VCN: virtual cloud network.
- Compute instance: virtual machine running an operating system.
- Security list or network security group: cloud firewall control.
- IAM policy: statement granting access to resources.

## Homelab Resource Model

For a small infrastructure lab, the resource model should stay simple and easy to audit:

| Resource | Purpose | Operational Note |
|---|---|---|
| Compartment | Group lab resources | Keep compute, VCN, and volumes in a known boundary |
| VCN | Network container | Document CIDR range and attached gateways |
| Public subnet | SSH-accessible subnet | Use only when public administration is required |
| Internet gateway | Public egress and ingress path | Required for direct public SSH access |
| Compute instance | Ubuntu operations host | Track image, shape, public IP, and boot volume |
| Security rule | Ingress and egress control | Limit SSH source range where possible |
| Boot volume | Operating system disk | Monitor capacity and snapshot before risky changes |

## Operational Checks

- Confirm instance state and public IP.
- Confirm subnet, route table, and internet gateway.
- Confirm ingress rules for required ports.
- Confirm boot volume and attached block volumes.
- Confirm IAM policy scope.

## Provisioning Review Checklist

Before treating an instance as ready for use:

1. Confirm the instance is in the expected compartment.
2. Confirm the image and shape match the lab purpose.
3. Confirm the public IP is intentional.
4. Confirm SSH ingress is limited to the expected source range.
5. Confirm route table rules support the intended access path.
6. Confirm the SSH key was added during provisioning.
7. Confirm the first login works and the operating system is patched.

## Change Documentation

For cloud operations practice, each meaningful change should be documented with:

- Date
- Resource changed
- Reason for change
- Expected behavior
- Validation command or console check
- Rollback note

Example:

```text
Change: Add TCP 443 ingress for test HTTPS service
Reason: Validate TLS endpoint from external client
Validation: nc -vz <public-ip> 443 and curl -Iv https://<host>
Rollback: Remove TCP 443 ingress rule after test
```

## Infrastructure Relevance

OCI practice builds transferable cloud operations skills: provisioning hosts, securing access, validating network rules, managing identities, and documenting resource ownership. The most important habit is to connect cloud control-plane settings with what the Linux guest actually shows through commands and logs.
