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

## Operational Checks

- Confirm instance state and public IP.
- Confirm subnet, route table, and internet gateway.
- Confirm ingress rules for required ports.
- Confirm boot volume and attached block volumes.
- Confirm IAM policy scope.

## Infrastructure Relevance

OCI practice builds transferable cloud operations skills: provisioning hosts, securing access, validating network rules, managing identities, and documenting resource ownership.
