# Homelab Docker Environment

## Purpose

Docker is used in the homelab to practice container lifecycle management, service exposure, logs, and persistent volumes.

## Setup

```bash
sudo apt update
sudo apt install -y docker.io docker-compose-plugin
sudo systemctl enable --now docker
sudo usermod -aG docker infraadmin
```

Verify after a new login session:

```bash
docker version
docker compose version
docker run --rm hello-world
```

## Directory Layout

```text
/srv/docker/
├── project-a/
│   └── compose.yml
└── project-b/
    └── compose.yml
```

## Operations

```bash
docker compose up -d
docker compose ps
docker compose logs -f
docker compose down
docker system df
```

## Infrastructure Relevance

The Docker environment provides repeatable practice for service deployment, host resource monitoring, port exposure, log review, and cleanup.
