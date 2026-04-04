# Docker Operations and Portainer Guide for LUNA

This document covers managing Docker inside WSL2 on LUNA, setting up Portainer for visual container management, and exposing containerized services to both the Windows host and the local network.

**Environment:** LUNA — Windows 11 host, WSL2 Ubuntu 24.04, Docker Engine (not Docker Desktop), all AI services on the `luna-net` bridge network.

## Understanding the Network Layers

Before diving into setup, it helps to understand how traffic flows through the stack. There are three boundaries a request might need to cross:

**Docker container → WSL2 host:** Containers on `luna-net` talk to each other by container name. To reach a container from inside WSL2 (but outside Docker), the container must publish a port (the `-p` or `ports:` directive). Once published, the service is available at `localhost:<port>` inside WSL2.

**WSL2 → Windows host:** By default, Windows automatically forwards ports that WSL2 binds on `localhost`. If you expose Postgres on port 5432 inside WSL2, you can reach it from Windows at `localhost:5432` without any extra configuration. This works out of the box on modern WSL2 builds.

**Windows host → LAN (other machines):** This is where it gets more involved. WSL2's automatic port forwarding only listens on `localhost`, which means other computers on your network cannot reach those services. To expose a service to AURORA, the Mac, or anything else on your LAN, you need to either configure Windows port forwarding or switch WSL2 to mirrored networking mode.

```
┌───────────────────────────────────────────────────────────────────┐
│  LAN (AURORA, Mac, other devices)                                 │
│       │                                                           │
│       ▼  Requires: firewall rule + port proxy OR mirrored mode    │
│  ┌────────────────────────────────────────────────────────┐       │
│  │  Windows 11 Host (LUNA)                                │       │
│  │       │                                                │       │
│  │       ▼  Automatic localhost forwarding from WSL2      │       │
│  │  ┌─────────────────────────────────────────────┐       │       │
│  │  │  WSL2 Ubuntu 24.04                          │       │       │
│  │  │       │                                     │       │       │
│  │  │       ▼  Published ports (-p flag)          │       │       │
│  │  │  ┌──────────────────────────────────┐       │       │       │
│  │  │  │  Docker Engine (luna-net bridge)  │       │       │       │
│  │  │  │  ┌──────────┐ ┌──────────┐       │       │       │       │
│  │  │  │  │ Postgres │ │ LiteLLM  │ ...   │       │       │       │
│  │  │  │  └──────────┘ └──────────┘       │       │       │       │
│  │  │  └──────────────────────────────────┘       │       │       │
│  │  └─────────────────────────────────────────────┘       │       │
│  └────────────────────────────────────────────────────────┘       │
└───────────────────────────────────────────────────────────────────┘
```

## Setting Up Portainer

Portainer Community Edition gives you a web UI for managing containers, images, volumes, networks, and logs. It runs as a container itself and connects to the Docker socket.

### Create a Persistent Volume

```bash
# Portainer stores its config and user database in this volume
docker volume create portainer_data
```

### Run Portainer

```bash
docker run -d \
  --name portainer \
  --restart=always \
  -p 9443:9443 \
  -p 9000:9000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
```

Port 9443 is the HTTPS UI and port 9000 is HTTP. Mounting `/var/run/docker.sock` gives Portainer full control over Docker on this host.

### Access the UI

From inside WSL2: `https://localhost:9443` From Windows: `https://localhost:9443` (WSL2 auto-forwards this)

On first load, Portainer asks you to create an admin account. Set a strong password — this UI has full Docker control.

### Adding Portainer to Your Compose Stack

If you prefer to manage Portainer alongside your other services in Docker Compose, add it to your `docker-compose.yml`:

```yaml
# docker-compose.yml — add this service block
services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    restart: always
    ports:
      - "9443:9443"
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data

volumes:
  portainer_data:
```

Note: Portainer does not need to be on `luna-net` unless you want it to communicate with your AI containers by name. For pure management purposes, socket access is sufficient.

### What You Can Do in Portainer

Once Portainer is running, the web UI gives you quick access to everything you would otherwise do on the command line:

- **Containers:** Start, stop, restart, remove, view logs, open a console (exec into a running container), inspect environment variables and mounts
- **Images:** Pull new images, remove unused images, see image layers and tags
- **Volumes:** Create, inspect, and remove volumes; see which containers use them
- **Networks:** View bridge networks (you will see `luna-net` here), inspect connected containers, create new networks
- **Stacks:** Deploy and manage Docker Compose stacks directly from the UI by pasting in your YAML
- **Events:** Real-time stream of Docker events (container starts, stops, pulls, errors)
- **Resource usage:** CPU, memory, and network stats per container

## Common Docker Operations

This section is a reference for the most frequent tasks you will run from the WSL2 terminal.

### Container Lifecycle

```bash
# List running containers
docker ps

# List all containers including stopped ones
docker ps -a

# Start a stopped container
docker start <container_name>

# Stop a running container (graceful shutdown)
docker stop <container_name>

# Restart a container
docker restart <container_name>

# Remove a stopped container
docker rm <container_name>

# Force remove a running container
docker rm -f <container_name>
```

### Logs and Debugging

```bash
# View logs for a container (last 100 lines, follow new output)
docker logs --tail 100 -f <container_name>

# Open an interactive shell inside a running container
docker exec -it <container_name> /bin/bash

# If bash isn't available (Alpine-based images), use sh
docker exec -it <container_name> /bin/sh

# Run a one-off command inside a container
docker exec <container_name> <command>

# Example: run a psql query against the Postgres container
docker exec -it postgres psql -U openclaw -d openclaw_db
```

### Images

```bash
# Pull an image (or update to latest)
docker pull <image>:<tag>

# List local images
docker images

# Remove an image
docker rmi <image>:<tag>

# Remove all unused images (dangling and unreferenced)
docker image prune -a
```

### Volumes

```bash
# List all volumes
docker volume ls

# Inspect a volume (shows mount point on host)
docker volume inspect <volume_name>

# Remove a specific volume (container must be stopped/removed first)
docker volume rm <volume_name>

# Remove all unused volumes — be careful with this
docker volume prune
```

### Networks

```bash
# List all networks
docker network ls

# Inspect a network (shows connected containers and their IPs)
docker network inspect luna-net

# Create a bridge network
docker network create luna-net

# Connect a running container to a network
docker network connect luna-net <container_name>

# Disconnect a container from a network
docker network disconnect luna-net <container_name>
```

### Docker Compose Operations

All of these run from the directory containing your `docker-compose.yml`.

```bash
# Start all services (detached)
docker compose up -d

# Start a specific service
docker compose up -d postgres

# Stop all services
docker compose down

# Stop and remove volumes too — THIS DESTROYS DATA
docker compose down -v

# Rebuild images and restart
docker compose up -d --build

# View status of all services in the stack
docker compose ps

# View logs for all services (follow mode)
docker compose logs -f

# View logs for a specific service
docker compose logs -f litellm

# Pull latest images for all services
docker compose pull

# Restart a single service without touching others
docker compose restart ollama
```

### Cleanup

```bash
# Nuclear option: remove all stopped containers, unused networks,
# dangling images, and build cache
docker system prune

# Same as above but also removes unused images (not just dangling)
docker system prune -a

# See how much disk space Docker is using
docker system df
```

## Exposing Services to the Windows Host

This is the simplest case and mostly works automatically.

### How It Works

When a Docker container publishes a port (via `-p` or `ports:` in Compose), that port becomes available at `localhost` inside WSL2. Modern WSL2 builds (Windows 11 22H2+) automatically mirror these ports to `localhost` on the Windows side.

So if your Compose file has:

```yaml
services:
  litellm:
    ports:
      - "4000:4000"
```

Then from Windows, `http://localhost:4000` reaches LiteLLM. No extra config needed.

### Exposing Postgres to Windows

Your current setup keeps Postgres internal (no published ports). To make it reachable from Windows tools like pgAdmin, DBeaver, or DataGrip, add a port mapping:

```yaml
services:
  postgres:
    image: postgres:16
    container_name: postgres
    restart: unless-stopped
    environment:
      POSTGRES_USER: openclaw
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: openclaw_db
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - luna-net
    ports:
      - "5432:5432"    # <-- add this line
```

After adding the port mapping:

```bash
docker compose up -d postgres
```

From Windows, connect your database tool to `localhost:5432` with the credentials defined in your environment.

### If Localhost Forwarding Is Not Working

On older WSL2 builds or if something is misconfigured, localhost forwarding might not work. Check these things:

```bash
# Inside WSL2, verify the service is listening
ss -tlnp | grep 5432

# From Windows PowerShell, test the connection
Test-NetConnection -ComputerName localhost -Port 5432
```

If the service is up in WSL2 but Windows cannot reach it, check your WSL version:

```powershell
# In PowerShell
wsl --version
```

If you are on WSL2 with a kernel version below 5.15, you may need to use the WSL2 VM's IP address instead of localhost:

```bash
# Inside WSL2, get the IP
hostname -I
# Returns something like 172.28.x.x
```

Then connect from Windows using that IP instead of localhost.

## Exposing Services to Your Local Network

This is where you make services reachable from AURORA, the Mac, or any other device on your LAN.

### Option 1: WSL2 Mirrored Networking Mode (Recommended)

Mirrored mode makes WSL2 share the Windows host's network interfaces directly. Ports published in WSL2 become available on LUNA's LAN IP, just as if they were running natively on Windows.

**Step 1:** Create or edit `%USERPROFILE%\.wslconfig` on the Windows side:

```ini
# C:\Users\Marty\.wslconfig
[wsl2]
memory=20GB
networkingMode=mirrored
```

The `memory=20GB` line preserves your existing memory cap.

**Step 2:** Restart WSL2 from PowerShell:

```powershell
wsl --shutdown
# Then reopen your WSL2 terminal
```

**Step 3:** Verify. Inside WSL2, your IP should now match LUNA's Windows IP:

```bash
hostname -I
# Should show your LAN IP (e.g., 192.168.x.x) instead of 172.28.x.x
```

**Step 4:** Ensure Windows Firewall allows inbound traffic on the ports you want to expose. From an elevated PowerShell:

```powershell
# Allow Postgres from LAN
New-NetFirewallRule -DisplayName "WSL2 Postgres" `
  -Direction Inbound -Protocol TCP -LocalPort 5432 -Action Allow

# Allow Portainer UI from LAN
New-NetFirewallRule -DisplayName "WSL2 Portainer" `
  -Direction Inbound -Protocol TCP -LocalPort 9443 -Action Allow

# Allow LiteLLM from LAN
New-NetFirewallRule -DisplayName "WSL2 LiteLLM" `
  -Direction Inbound -Protocol TCP -LocalPort 4000 -Action Allow

# Allow Qdrant from LAN
New-NetFirewallRule -DisplayName "WSL2 Qdrant" `
  -Direction Inbound -Protocol TCP -LocalPort 6333 -Action Allow

# Allow Ollama from LAN
New-NetFirewallRule -DisplayName "WSL2 Ollama" `
  -Direction Inbound -Protocol TCP -LocalPort 11434 -Action Allow
```

Now any machine on your LAN can reach these services at `<LUNA's LAN IP>:<port>`.

### Option 2: Windows Port Proxy (If You Cannot Use Mirrored Mode)

If mirrored networking causes issues or you need to stay on NAT mode, you can use `netsh` to forward specific ports from LUNA's LAN interface into WSL2.

**Step 1:** Get the WSL2 VM's internal IP:

```bash
# Inside WSL2
hostname -I
# Example output: 172.28.176.1
```

**Step 2:** From an elevated PowerShell, set up the port proxy:

```powershell
# Forward Postgres
netsh interface portproxy add v4tov4 `
  listenport=5432 listenaddress=0.0.0.0 `
  connectport=5432 connectaddress=172.28.176.1

# Forward Portainer
netsh interface portproxy add v4tov4 `
  listenport=9443 listenaddress=0.0.0.0 `
  connectport=9443 connectaddress=172.28.176.1

# Forward LiteLLM
netsh interface portproxy add v4tov4 `
  listenport=4000 listenaddress=0.0.0.0 `
  connectport=4000 connectaddress=172.28.176.1
```

**Step 3:** Add the same firewall rules as in Option 1.

**Step 4:** Verify the port proxies are active:

```powershell
netsh interface portproxy show all
```

**Important caveat:** WSL2's internal IP can change on reboot. If you go the port proxy route, you'll want a small script that refreshes the `connectaddress` on startup. Mirrored mode avoids this problem entirely.

### Removing Port Proxy Rules

```powershell
# Remove a specific rule
netsh interface portproxy delete v4tov4 `
  listenport=5432 listenaddress=0.0.0.0

# Remove all port proxy rules
netsh interface portproxy reset
```

### Removing Firewall Rules

```powershell
# By display name
Remove-NetFirewallRule -DisplayName "WSL2 Postgres"
```

## Security Considerations

Once you expose services to the LAN, keep these things in mind:

**Postgres** should never be exposed without a strong password. If you have not set one, update `POSTGRES_PASSWORD` in your `.env` file and restart the container. Consider restricting which IPs can connect by editing `pg_hba.conf` inside the container or mounting a custom one.

**Portainer** has full Docker control. If you expose port 9443 to the LAN, anyone on your network with the admin password can manage your entire container stack. Keep the password strong and consider only exposing it to Windows (localhost) rather than the full LAN.

**Ollama** has no built-in authentication. If exposed to the LAN, any machine can send inference requests. This is fine on a trusted home network but something to be aware of.

**LiteLLM** routes to cloud APIs using your API keys. Exposing port 4000 to the LAN means any device could burn through your OpenAI/Anthropic credits. Consider whether LAN exposure is actually needed or if localhost-only access is sufficient.

**General rule:** Only expose what you actually need. If AURORA needs to hit Ollama for inference offloading, expose 11434. If the Mac just needs to query Postgres, expose 5432. Don't open everything by default.

## Binding to Specific Interfaces

By default, `ports: "5432:5432"` in Compose binds to `0.0.0.0`, meaning all interfaces. You can restrict this:

```yaml
# Reachable from everywhere (Windows, LAN if mirrored/proxied)
ports:
  - "5432:5432"

# Reachable only from localhost (WSL2 internal + Windows via auto-forward)
ports:
  - "127.0.0.1:5432:5432"
```

Binding to `127.0.0.1` is a good default for services that only Windows needs to reach. Bind to `0.0.0.0` (or just the port pair without an IP) only for services that need to be reachable from other machines.

## Quick Reference: Port Map for the Full Stack

This table summarizes the current and recommended port exposure for your services:

|Service|Container Port|Current Exposure|Recommended for Windows|Recommended for LAN|
|---|---|---|---|---|
|Portainer|9443, 9000|New (not yet deployed)|`9443:9443`|Localhost only — full Docker control|
|LiteLLM|4000|Published|`4000:4000`|Localhost only — has your API keys|
|Ollama|11434|Published|`11434:11434`|Open if AURORA needs inference access|
|Qdrant|6333|Published|`6333:6333`|Localhost only unless other machines need vector search|
|Postgres|5432|Internal only|`5432:5432`|Open only if Mac/AURORA need direct DB access|
|Redis|6379|Internal only|Keep internal|Keep internal|
|OpenClaw|TBD|Not yet deployed|TBD|TBD|

## Verifying Connectivity

A quick checklist for testing after you make changes.

**From inside WSL2:**

```bash
# Check a service is listening
ss -tlnp | grep <port>

# Test connectivity
curl http://localhost:4000/health    # LiteLLM
curl http://localhost:11434/api/tags  # Ollama
curl http://localhost:6333/collections # Qdrant
```

**From Windows PowerShell:**

```powershell
# Test TCP connectivity
Test-NetConnection -ComputerName localhost -Port 5432

# Test HTTP
Invoke-RestMethod http://localhost:4000/health
```

**From another machine on the LAN:**

```bash
# Replace with LUNA's actual LAN IP
curl http://192.168.1.x:4000/health

# Or test with netcat
nc -zv 192.168.1.x 5432
```

If LAN tests fail but localhost works, the issue is almost always Windows Firewall. Double check that the inbound rules are active and that the port proxy (if using NAT mode) is pointing to the right WSL2 IP.

## Useful Portainer Workflows

Once Portainer is running, here are the most practical things to use it for in your day-to-day workflow:

**Tailing logs across multiple containers:** Instead of running `docker compose logs -f` in the terminal, open each container in Portainer and view logs in separate browser tabs. Easier to cross-reference when debugging agent interactions across OpenClaw, LiteLLM, and Ollama.

**Quick container restarts:** When testing config changes to `litellm_config.yaml`, click the restart button in Portainer instead of typing commands. Small convenience that adds up.

**Console access:** The "Console" button on any container opens an interactive shell in your browser. Handy for quick inspection without opening a new terminal and typing `docker exec`.

**Stack management:** Paste your `docker-compose.yml` into Portainer's Stacks feature and deploy from the UI. This also gives you a diff view when you update the YAML.

**Resource monitoring:** The container stats view shows real-time CPU and memory usage per container. Useful for spotting when Ollama is under heavy load or when a container is leaking memory.