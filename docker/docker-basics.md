# Docker Basics

## Purpose

Docker runs applications in containers. For infrastructure practice, Docker is useful for learning service isolation, logs, port mapping, volumes, and restart behavior.

## Commands Used

```bash
docker version
docker ps
docker ps -a
docker images
docker pull nginx
docker run --rm hello-world
docker run -d --name web -p 8080:80 nginx
docker logs web
docker stop web
docker rm web
```

## Operational Concepts

- Image: packaged filesystem and metadata.
- Container: running instance of an image.
- Volume: persistent data outside the container lifecycle.
- Network: container connectivity boundary.
- Port mapping: exposes container ports through the host.

## Infrastructure Relevance

Docker practice builds familiarity with service lifecycle, log inspection, network exposure, and host resource use.
