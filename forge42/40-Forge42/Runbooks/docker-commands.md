---
tags: [brand/forge42, type/runbook, status/active]
updated: 2026-04-19
---

# Docker Commands Reference

Practical Docker command reference for day-to-day operations on LUNA. All commands run inside WSL2 Ubuntu 24.04 (or natively on Ubuntu Server when migrated).

See [[docker-portainer-guide]] for the full networking guide (WSL2 → Windows host → LAN exposure, Portainer setup).

## Container Lifecycle

```bash
# List running containers
docker ps

# List all containers including stopped
docker ps -a

# Start a stopped container
docker start <container_name>

# Stop a running container (graceful)
docker stop <container_name>

# Restart a container
docker restart <container_name>

# Remove a stopped container
docker rm <container_name>

# Force remove a running container
docker rm -f <container_name>
```

## Logs and Debugging

```bash
# View logs (last 100 lines, follow new output)
docker logs --tail 100 -f <container_name>

# Open an interactive shell inside a running container
docker exec -it <container_name> /bin/bash

# Alpine-based images (no bash)
docker exec -it <container_name> /bin/sh

# Run a one-off command inside a container
docker exec <container_name> <command>

# PostgreSQL: open psql in the postgres container
docker exec -it postgres psql -U postgres

# Connect to a specific database
docker exec -it postgres psql -U postgres -d <database>
```

## Images

```bash
# Pull or update an image
docker pull <image>:<tag>

# List local images
docker images

# Remove an image
docker rmi <image>:<tag>

# Remove all unused images (dangling and unreferenced)
docker image prune -a
```

## Volumes

```bash
# List all volumes
docker volume ls

# Inspect a volume (shows mount point)
docker volume inspect <volume_name>

# Remove a specific volume (container must be stopped first)
docker volume rm <volume_name>

# Remove all unused volumes — use with caution
docker volume prune
```

## Networks

```bash
# List all networks
docker network ls

# Inspect a network (shows connected containers)
docker network inspect luna-net

# Create a bridge network
docker network create luna-net

# Connect a running container to a network
docker network connect luna-net <container_name>
```

## Docker Compose Operations

All compose commands run from the directory containing `docker-compose.yml`.

```bash
# Start all services (detached)
docker compose up -d

# Start a specific service
docker compose up -d <service_name>

# Stop all services (keeps volumes)
docker compose down

# Stop and DESTROY all volumes — data loss
docker compose down -v

# Restart a single service without touching others
docker compose restart <service_name>

# Rebuild images and restart
docker compose up -d --build

# View status of all services
docker compose ps

# Follow logs for all services
docker compose logs -f

# Follow logs for a specific service
docker compose logs -f <service_name>

# Pull latest images for all services
docker compose pull
```

## System Cleanup

```bash
# Remove stopped containers, unused networks, dangling images, build cache
docker system prune

# Same but also removes unused images (not just dangling)
docker system prune -a

# See how much disk space Docker is using
docker system df
```

## Service Health Checks

Quick checks for each core service:

```bash
# LiteLLM health
curl http://localhost:4000/health

# Qdrant health
curl http://localhost:6333/readyz

# AnythingLLM health
curl http://localhost:3001/api/ping

# PostgreSQL (check it's accepting connections)
docker exec postgres pg_isready -U postgres

# Redis ping
docker exec redis redis-cli ping
```

## Portainer Quick Reference

Portainer UI at `https://localhost:9443`. Equivalent to CLI operations:

| CLI | Portainer |
|---|---|
| `docker logs` | Container → Logs tab |
| `docker exec -it ... bash` | Container → Console tab |
| `docker compose ps` | Stacks → Stack name |
| `docker restart` | Container → Restart button |
| `docker system df` | Home → Resource usage |

See [[docker-portainer-guide]] for full Portainer setup instructions.

## WSL2 Networking Notes

Services published by Docker in WSL2 are available at `localhost:<port>` from Windows automatically. For LAN access (reaching LUNA from Aurora or Nova), use WSL2 mirrored networking mode:

```ini
# C:\Users\Marty\.wslconfig
[wsl2]
memory=20GB
networkingMode=mirrored
```

After editing, restart WSL2: `wsl --shutdown` from PowerShell.

## Related

- [[docker-portainer-guide]] — full network exposure guide and Portainer setup
- [[docker-services]] — complete service inventory with ports
- [[backup-recovery]] — volume backup and restore procedures
- [[incident-response]] — what to check when a service goes down
