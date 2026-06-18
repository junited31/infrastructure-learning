# Docker Compose

## Purpose

Docker Compose defines multi-container environments in a YAML file. It is useful for repeatable lab services and small infrastructure experiments.

## Common Commands

```bash
docker compose up -d
docker compose ps
docker compose logs
docker compose logs -f service-name
docker compose restart service-name
docker compose down
docker compose pull
```

## Example

```yaml
services:
  web:
    image: nginx:stable
    ports:
      - "8080:80"
    restart: unless-stopped
```

## Operational Notes

- Keep compose files in a documented directory such as `/srv/docker/project-name`.
- Use named volumes for persistent data.
- Review logs after startup.
- Avoid exposing ports that are not required.
- Document environment variables and secrets handling.

## Infrastructure Relevance

Compose supports repeatable deployment practice and helps connect container behavior to Linux networking, storage, and service monitoring.
