# Docker Troubleshooting

## Purpose

Docker troubleshooting focuses on container state, logs, networking, volumes, image versions, and host resources.

## Commands Used

```bash
docker ps -a
docker logs container-name
docker inspect container-name
docker stats
docker exec -it container-name sh
docker network ls
docker volume ls
docker compose logs
```

## Common Issues

| Symptom | Check |
|---|---|
| Container exits | `docker logs`, command, environment variables |
| Port unavailable | Host port already in use, compose port mapping |
| Data missing | Volume mount path or named volume |
| Cannot reach service | Bind address, container network, host firewall |
| Image pull fails | DNS, registry access, credentials |

## Infrastructure Relevance

Container issues often require checking both container configuration and host state. This is similar to troubleshooting services across multiple infrastructure layers.
