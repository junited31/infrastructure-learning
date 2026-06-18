# Cloud IAM

## Purpose

Identity and Access Management controls who can access cloud resources and what actions they can perform.

## Concepts

- User: human or service identity.
- Group: collection of users.
- Policy: permission statement.
- Compartment: resource boundary for organization and access.
- Least privilege: grant only required access.

## Operational Practices

- Separate admin and daily-use access where possible.
- Use groups rather than assigning permissions one user at a time.
- Scope policies to the smallest practical compartment.
- Review unused users and credentials.
- Protect API keys and console access.

## Infrastructure Relevance

IAM mistakes can expose infrastructure or block operations. Understanding least privilege and policy scope is important for cloud operations and security.
