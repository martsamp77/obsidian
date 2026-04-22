# Secure Browsing and Dev Environment Setup

## Overview

This document covers the full setup for isolated secure browsing via Windows Sandbox and a Docker-based development environment running inside WSL2. Everything is built on top of a VeraCrypt-encrypted volume where sensitive data lives at rest.

**The three layers of this setup:**

- VeraCrypt — encryption at rest
- Windows Sandbox — isolated, ephemeral browsing environment
- Docker in WSL2 + Portainer — persistent development environment

---

## Windows Features to Enable

Open **Turn Windows Features On/Off** (search it in Start) and enable the following:

|Feature|Action|Why|
|---|---|---|
|Windows Sandbox|Enable|The core isolated browser environment|
|Hyper-V|Enable (full parent + all children)|Sandbox runs on top of Hyper-V|
|Windows Hypervisor Platform|Enable|Allows WSL2 and Sandbox to coexist cleanly|
|Virtual Machine Platform|Already enabled — leave it|Required by WSL2|
|Windows Subsystem for Linux|Leave it alone|You're on the Store version — this checkbox controls the old inbox version and will conflict if enabled|

After enabling these three features, **reboot before proceeding**.

### WSL Note

If WSL appears unchecked but is working, you installed the modern Store version via `wsl --install`. That checkbox controls the legacy inbox version. Leave it unchecked. Verify your install is healthy by running:

```powershell
wsl --version
```

If it returns a version number cleanly, you're good.

---

## Windows Sandbox Setup

### Basic Launch

After the reboot, search for **Windows Sandbox** in Start. It will open a clean, disposable Windows session. Everything inside is wiped when you close it.

### Configuration File (.wsb)

To automatically map your VeraCrypt drive into the Sandbox so Chrome Portable is available, create a `.wsb` config file. Open Notepad and save the following as `sandbox-browser.wsb` somewhere convenient on your host:

```xml
<Configuration>
  <MappedFolders>
    <MappedFolder>
      <HostFolder>E:\PortableBrowser</HostFolder>
      <SandboxFolder>C:\Users\WDAGUtilityAccount\Desktop\Browser</SandboxFolder>
      <ReadOnly>true</ReadOnly>
    </MappedFolder>
  </MappedFolders>
  <AudioInput>Disable</AudioInput>
  <VideoInput>Disable</VideoInput>
  <MemoryInMB>4096</MemoryInMB>
</Configuration>
```

**Adjust the `HostFolder` path** to wherever your Chrome Portable folder lives on the VeraCrypt volume. The `ReadOnly` flag prevents anything inside the sandbox from writing back to your encrypted drive.

Double-click the `.wsb` file to launch Sandbox with the mapped folder. Chrome Portable will appear on the sandbox desktop.

### Audio and Video

Audio passthrough is enabled by default in Windows Sandbox — YouTube, Rumble, and other streaming sites work without any extra configuration. If you used the config file above, remove the `<AudioInput>` line or set it to `Enable` if you want microphone access as well.

### VPN Inside Sandbox

Because Sandbox wipes state on every close, you cannot install a persistent VPN client inside it. Your best options:

**Option A — Use the VPN on the host before launching Sandbox.** Connect NordVPN or PIA on Windows first. All Sandbox traffic routes through the host network, which is already tunneled. Simple but the host OS is part of the path.

**Option B — Run a portable VPN binary inside Sandbox each session.** Both NordVPN and PIA have Windows installers, but they require full installation. This is not practical for ephemeral sessions without a startup script.

**Option C — Run a VirtualBox VM instead of Sandbox for VPN-isolated browsing.** If a fully isolated VPN tunnel is a hard requirement, the VM approach is the better tool. See the VirtualBox section below.

---

## Docker in WSL2

### Why This Approach

Rather than installing Docker Desktop (heavy, background services, licensing), you run Docker Engine natively inside your existing WSL2 distro. This is lighter, gives you full Linux-native Docker behavior, and integrates naturally with your Cursor dev environment.

### Install Docker Engine in WSL2

Open your WSL2 terminal and run:

```bash
# Update package list
sudo apt update

# Install Docker and Compose
sudo apt install -y docker.io docker-compose-v2

# Add your user to the docker group so you don't need sudo
sudo usermod -aG docker $USER

# Log out of WSL and back in, then verify
docker run hello-world
```

### Start Docker Automatically

WSL2 does not run systemd by default on all configurations. If Docker doesn't start on WSL launch, add this to your `.bashrc` or `.zshrc`:

```bash
# Start Docker daemon if not running
if [ $(service docker status | grep -c "not running") -gt 0 ]; then
  sudo service docker start
fi
```

Or enable systemd in WSL2 (Windows 11 22H2+) by adding this to `/etc/wsl.conf` inside your distro:

```ini
[boot]
systemd=true
```

Then restart WSL with `wsl --shutdown` from PowerShell and relaunch.

### Install Portainer (Web GUI)

Portainer gives you a full browser-based dashboard to manage containers, volumes, networks, and logs — no Docker Desktop needed.

```bash
# Create a persistent volume for Portainer data
docker volume create portainer_data

# Run Portainer
docker run -d \
  -p 9000:9000 \
  --name portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce
```

Then open your Windows Chrome and go to:

```
http://localhost:9000
```

Set up your admin account on first launch. From there you can manage everything visually.

### Common Dev Containers

Here are quick Compose snippets for common dev services. Save as `docker-compose.yml` and run `docker compose up -d`.

**PostgreSQL:**

```yaml
services:
  postgres:
    image: postgres:16
    restart: always
    environment:
      POSTGRES_USER: dev
      POSTGRES_PASSWORD: devpassword
      POSTGRES_DB: devdb
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

**Redis:**

```yaml
services:
  redis:
    image: redis:7
    restart: always
    ports:
      - "6379:6379"
```

**Qdrant (Vector DB):**

```yaml
services:
  qdrant:
    image: qdrant/qdrant
    restart: always
    ports:
      - "6333:6333"
    volumes:
      - qdrant_data:/qdrant/storage

volumes:
  qdrant_data:
```

**Nginx (Web Server):**

```yaml
services:
  nginx:
    image: nginx:latest
    restart: always
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html:ro
```

---

## VirtualBox VM with VPN (Optional — Stronger Isolation)

If you need a persistent browsing environment with a dedicated VPN tunnel isolated from the host OS, use VirtualBox instead of Sandbox.

### Setup

- Install VirtualBox from virtualbox.org
- Create a Linux VM (Linux Mint or Fedora recommended)
- Set the network adapter to **Bridged Adapter** — gives the VM its own IP, host has no visibility into traffic
- Store the VM disk file on your VeraCrypt volume for encryption at rest
- Take a clean snapshot before first use so you can revert anytime

### Install PIA or NordVPN Inside the VM

**PIA (recommended — more mature Linux client):**

```bash
# Download from privateinternetaccess.com/pages/download
chmod +x pia-linux-installer.run
sudo ./pia-linux-installer.run
```

Enable the kill switch in PIA settings so traffic stops if the VPN drops rather than leaking your real IP.

**NordVPN:**

```bash
sh <(curl -sSf https://downloads.nordcdn.com/apps/linux/install.sh)
sudo usermod -aG nordvpn $USER
nordvpn login
nordvpn connect
nordvpn set killswitch on
```

### VPN Kill Switch

Always enable this. If the VPN connection drops mid-session, all traffic from the VM halts rather than falling back to your unencrypted real IP.

- PIA: Settings → Connection → VPN Kill Switch → Enable
- NordVPN: `nordvpn set killswitch on`

---

## Full Stack Summary

|Layer|Tool|Purpose|
|---|---|---|
|Encryption at rest|VeraCrypt|Encrypted volume for browser, VM disks, sensitive data|
|Isolated browsing|Windows Sandbox (.wsb config)|Ephemeral, wipes on close, hypervisor-isolated|
|Dev environment|Docker Engine in WSL2|Databases, web servers, containers|
|Container GUI|Portainer (localhost:9000)|Visual management without Docker Desktop|
|Stronger browsing isolation + VPN|VirtualBox VM (Bridged) + PIA/NordVPN|When you need persistent state and dedicated VPN tunnel|

---

## Quick Reference Checklist

- [ ] Enable Windows Sandbox, Hyper-V, Windows Hypervisor Platform in Windows Features
- [ ] Reboot
- [ ] Create `sandbox-browser.wsb` config file pointing to VeraCrypt Chrome Portable folder
- [ ] Test Sandbox launch and audio/video playback
- [ ] Open WSL2 terminal and install Docker Engine
- [ ] Add user to docker group, verify with `docker run hello-world`
- [ ] Enable systemd in WSL2 or add Docker autostart to shell profile
- [ ] Deploy Portainer container, confirm access at localhost:9000
- [ ] (Optional) Set up VirtualBox VM with Bridged adapter and PIA/NordVPN for persistent isolated browsing