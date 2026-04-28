# Quark Migration Guide

Migration plan for moving the IronForge platform from LUNA (Windows 11 / WSL2) to Quark (native Ubuntu 24.04).

## Hardware Comparison

|Spec|LUNA|Quark|
|---|---|---|
|CPU|AMD Ryzen 7 7840HS (8C/16T)|AMD Ryzen 9 7940HS (8C/16T)|
|RAM|32 GB DDR5-5600|~29 GB DDR5|
|Storage|1 TB NVMe|~1.8 TB NVMe|
|GPU|AMD Radeon 780M (integrated)|AMD Radeon 780M (integrated)|
|OS|Windows 11 → WSL2 → Ubuntu 24.04|Ubuntu 24.04.4 LTS (native)|
|Tailscale IP|(existing)|100.94.21.96|

The WSL2 layer is eliminated entirely. Architecture simplifies from `Windows 11 → WSL2 → Docker Engine` to `Ubuntu 24.04 → Docker Engine`.

## Phase 1: System Prep

Update the base system and install prerequisites.

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl git tmux htop jq unzip wget ca-certificates gnupg lsb-release zsh
```

Set timezone to Central:

```bash
sudo timedatectl set-timezone America/Chicago
```

Confirm hostname:

```bash
hostnamectl hostname quark
```

## Phase 2: Create the Corey User (Claude Code Sysadmin)

Corey is a dedicated service account for Claude Code to manage infrastructure. This user needs full sudo access, Docker group membership, and ownership of the stack and data directories.

### Create the User

```bash
# Create corey with a home directory and bash shell
sudo useradd -m -s /bin/bash -c "Claude Code Sysadmin" corey
```

### Set a Password

```bash
sudo passwd corey
```

### Grant Sudo Access

```bash
# Add corey to the sudo group
sudo usermod -aG sudo corey

# Grant passwordless sudo for non-interactive Claude Code sessions
echo 'corey ALL=(ALL) NOPASSWD:ALL' | sudo tee /etc/sudoers.d/corey
sudo chmod 0440 /etc/sudoers.d/corey
```

### Group Memberships

Corey needs membership in groups that allow system administration and Docker management.

```bash
sudo usermod -aG docker corey      # Docker socket access
sudo usermod -aG adm corey         # Read system logs (/var/log)
sudo usermod -aG systemd-journal corey  # journalctl access
```

### SSH Key for Corey

Generate a key pair that Claude Code will use to authenticate as corey.

```bash
sudo -u corey mkdir -p /home/corey/.ssh
sudo -u corey ssh-keygen -t ed25519 -f /home/corey/.ssh/id_ed25519 -C "corey@quark" -N ""
sudo chmod 700 /home/corey/.ssh
sudo chmod 600 /home/corey/.ssh/id_ed25519
```

If Claude Code connects remotely (from Nova or another machine), add its public key to corey's authorized_keys:

```bash
# Paste the Claude Code public key into this file
sudo -u corey tee /home/corey/.ssh/authorized_keys <<'EOF'
<paste-public-key-here>
EOF
sudo chmod 600 /home/corey/.ssh/authorized_keys
```

### Git Config for Corey

```bash
sudo -u corey git config --global user.name "Corey (Claude Code)"
sudo -u corey git config --global user.email "corey@quark.local"
```

### tmux Config for Corey

```bash
sudo -u corey tee /home/corey/.tmux.conf > /dev/null <<'EOF'
set -g mouse on
set -g history-limit 50000
set -g default-terminal "screen-256color"
EOF
```

## Phase 3: Directory Structure

On a Linux server, Docker stacks belong in `/opt` (third-party/self-contained application bundles). Persistent data that needs to survive stack rebuilds and live outside Docker volumes belongs in `/srv` (site-specific data served by the system).

### Stack Directory

`/opt/stacks/` is the parent directory for all Docker Compose projects. Each stack gets its own subdirectory. This is where the git repos, compose files, configs, and environment files live.

```bash
sudo mkdir -p /opt/stacks
sudo chown root:docker /opt/stacks
sudo chmod 2775 /opt/stacks
```

The setgid bit (`2`) ensures new files and directories inherit the `docker` group, so both Marty and Corey (both in the `docker` group) can manage any stack.

### Persistent Data Directory

`/srv/data/` is for files that Docker containers produce or consume but that must persist independently of any stack's lifecycle. Think: generated documents, exports, reports, shared media, backups. Each stack or logical grouping gets a subdirectory.

```bash
sudo mkdir -p /srv/data
sudo chown root:docker /srv/data
sudo chmod 2775 /srv/data
```

### Create the IronForge Layout

```bash
# Stack directory (git repo + compose + config)
mkdir -p /opt/stacks/ironclaw

# Persistent data directories for IronForge services
mkdir -p /srv/data/ironclaw/{documents,exports,backups}
mkdir -p /srv/data/ironclaw/postgres
mkdir -p /srv/data/ironclaw/qdrant
mkdir -p /srv/data/ironclaw/ollama
mkdir -p /srv/data/ironclaw/redis
```

### Example Layout for Future Stacks

This structure scales to multiple stacks on the same machine.

```
/opt/stacks/
├── ironclaw/              # IronForge stack (git repo)
│   ├── docker-compose.yml
│   ├── config/
│   ├── scripts/
│   └── .env
├── monitoring/            # Future: Prometheus + Grafana stack
│   ├── docker-compose.yml
│   └── .env
└── n8n/                   # Future: n8n automation stack
    ├── docker-compose.yml
    └── .env

/srv/data/
├── ironclaw/
│   ├── documents/         # Agent-generated docs, reports
│   ├── exports/           # Data exports, CSVs, dumps
│   ├── backups/           # Scheduled backups
│   ├── postgres/          # Postgres data (bind mount)
│   ├── qdrant/            # Qdrant storage (bind mount)
│   ├── ollama/            # Ollama model cache (bind mount)
│   └── redis/             # Redis AOF/RDB persistence (bind mount)
├── monitoring/
│   ├── grafana/
│   └── prometheus/
└── shared/                # Cross-stack shared files
    └── uploads/
```

### Grant Marty Access

Make sure your primary user is also in the docker group and can manage stacks:

```bash
sudo usermod -aG docker $USER
# Log out and back in, or: newgrp docker
```

## Phase 4: Install Docker Engine

```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Verify:

```bash
docker --version
docker compose version
docker run --rm hello-world
```

## Phase 5: Configure Docker Daemon

```bash
sudo tee /etc/docker/daemon.json > /dev/null <<'EOF'
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  },
  "default-address-pools": [
    { "base": "172.20.0.0/16", "size": 24 }
  ]
}
EOF

sudo systemctl restart docker
sudo systemctl enable docker
```

## Phase 6: SSH and Git for the Ironclaw Repo

This sets up the deploy key under corey's account, since corey is the sysadmin that manages the stack.

```bash
# As corey
sudo -u corey ssh-keygen -t ed25519 -f /home/corey/.ssh/id_ironclaw -C "quark-ironclaw-deploy" -N ""

sudo -u corey tee -a /home/corey/.ssh/config > /dev/null <<'EOF'
Host github-ironclaw
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ironclaw
    IdentitiesOnly yes
EOF

sudo chmod 600 /home/corey/.ssh/config
```

Register the public key in GitHub:

1. Print the key: `sudo -u corey cat /home/corey/.ssh/id_ironclaw.pub`
2. Go to `https://github.com/37metrics/ironclaw/settings/keys`
3. Add the key, enable write access
4. Remove LUNA's old deploy key

Test and clone:

```bash
sudo -iu corey
ssh -T git@github-ironclaw
git clone git@github-ironclaw:<owner>/ironclaw.git /opt/stacks/ironclaw
exit
```

Also set up Marty's access to the same repo via a separate deploy key or personal SSH key, so both users can pull and push.

## Phase 7: Adapt the Symlink Layout

The LUNA layout used `/Luna` as a runtime root with symlinks back into the repo. On Quark the runtime root becomes `/Quark`.

```bash
sudo mkdir -p /Quark/config /Quark/logs /Quark/runtime
sudo chown -R corey:docker /Quark
sudo chmod 2775 /Quark
```

Create the symlinks:

```bash
ln -sf /opt/stacks/ironclaw/docker-compose.yml /Quark/docker-compose.yml
ln -sf /opt/stacks/ironclaw/config/litellm_config.yaml /Quark/config/litellm_config.yaml
```

The bootstrap script supports custom paths. Run it with the new locations:

```bash
cd /opt/stacks/ironclaw
./scripts/luna/02-bootstrap-luna-layout.sh --repo-root /opt/stacks/ironclaw --luna-root /Quark --force
```

## Phase 8: Environment File

```bash
cd /opt/stacks/ironclaw
cp .env.example .env
```

Pull values from LUNA if it's still running:

```bash
ssh luna cat ~/ironclaw/.env
```

Copy the values into Quark's `.env`. Create the runtime env copy:

```bash
cp .env running-config/env/.env.quark
ln -sf /opt/stacks/ironclaw/running-config/env/.env.quark /Quark/.env
```

## Phase 9: Update docker-compose.yml for Bind Mounts

Before starting the stack, update `docker-compose.yml` to use bind mounts under `/srv/data/ironclaw/` instead of (or in addition to) named Docker volumes. This gives you persistent data that's easy to back up and survives `docker compose down -v`.

Example volume mapping changes:

```yaml
services:
  postgres:
    volumes:
      - /srv/data/ironclaw/postgres:/var/lib/postgresql/data

  qdrant:
    volumes:
      - /srv/data/ironclaw/qdrant:/qdrant/storage

  ollama:
    volumes:
      - /srv/data/ironclaw/ollama:/root/.ollama

  redis:
    volumes:
      - /srv/data/ironclaw/redis:/data
```

For agent-generated documents that need to be accessible outside the stack:

```yaml
services:
  openclaw-gateway:
    volumes:
      - /srv/data/ironclaw/documents:/app/output
```

Set ownership so Docker containers can write:

```bash
sudo chown -R 999:999 /srv/data/ironclaw/postgres    # postgres UID
sudo chown -R 1000:1000 /srv/data/ironclaw/qdrant    # qdrant UID
sudo chown -R root:root /srv/data/ironclaw/ollama     # ollama runs as root
sudo chown -R 999:999 /srv/data/ironclaw/redis        # redis UID
```

Verify the correct UIDs by checking the container images after first pull if these defaults don't match.

## Phase 10: Data Migration from LUNA

### Postgres

On LUNA:

```bash
docker exec luna-postgres-1 pg_dumpall -U postgres > /tmp/pg_dump_all.sql
```

Copy to Quark:

```bash
scp luna:/tmp/pg_dump_all.sql /tmp/
```

### Qdrant

On LUNA, find the volume mount:

```bash
docker inspect luna-qdrant-1 | jq '.[0].Mounts[] | select(.Destination=="/qdrant/storage")'
```

Rsync to Quark:

```bash
rsync -avz luna:<volume_path>/ /srv/data/ironclaw/qdrant/
```

### Ollama Models

Don't migrate these. They'll be re-pulled fresh on Quark (faster than copying multi-GB blobs).

### Redis

If Redis has AOF/RDB persistence with meaningful state, copy the dump file:

```bash
# On LUNA
docker exec luna-redis-1 redis-cli BGSAVE
# Then rsync the dump.rdb from the volume
```

Otherwise, skip this. Redis state is typically ephemeral.

## Phase 11: Start the Stack

```bash
cd /opt/stacks/ironclaw
docker compose up -d
docker compose ps
```

## Phase 12: Restore Data and Pull Models

### Restore Postgres

```bash
# Wait for postgres to be healthy
docker compose exec postgres psql -U postgres < /tmp/pg_dump_all.sql
```

### Restore Qdrant

If you rsync'd the storage directory into `/srv/data/ironclaw/qdrant/` before starting the stack, Qdrant should pick it up automatically. If the stack was already running:

```bash
docker compose restart qdrant
```

### Pull Ollama Models

```bash
./scripts/pull-models.sh
```

This pulls `llama3.2:3b`, `mistral:7b-instruct-q4_K_M`, and `phi3:mini`.

## Phase 13: Health Check

```bash
./scripts/health.sh
```

Verify individual services:

```bash
# LiteLLM
curl -s http://localhost:4000/health | jq

# Ollama
curl -s http://localhost:11434/api/tags | jq

# Qdrant
curl -s http://localhost:6333/collections | jq

# Redis
docker compose exec redis redis-cli ping
```

## Phase 14: Tailscale and Network Updates

Quark already has Tailscale connected at `100.94.21.96`. Update all references across the network:

### Tailscale Admin Console

- Review ACLs for any rules referencing LUNA's IP or hostname
- Update DNS records if you had custom entries pointing to LUNA

### Update Other Machines

On Nova, AURORA, and lambda-dual, update any configs that reference `luna.tail892569.ts.net`:

- LiteLLM configs on lambda-dual (if callbacks point to LUNA)
- Any scripts that SSH into LUNA
- Bookmarks to the IronForge dashboard

The new base URL is `quark.tail892569.ts.net`.

### Update Caddy Config

Replace hostname references in the Caddy reverse proxy config:

```bash
# In your Caddyfile, replace luna.tail892569.ts.net with quark.tail892569.ts.net
```

### Update Repo References

These files contain hardcoded LUNA references and should be updated:

- `docs/architecture.md` — machine topology diagram, hostname references
- `README.md` — all LUNA references
- `CLAUDE.md` — working directory paths, hostname references
- `config/litellm_config.yaml` — if any callback URLs reference LUNA
- `openclaw/organization.md` — if infrastructure descriptions reference LUNA

## Phase 15: OpenClaw CLI Wrapper

```bash
sudo -u corey mkdir -p /home/corey/bin

sudo -u corey tee /home/corey/bin/openclaw > /dev/null <<'WRAPPER'
#!/usr/bin/env bash
docker exec -it $(docker ps -qf "name=openclaw-gateway") openclaw "$@"
WRAPPER

sudo chmod +x /home/corey/bin/openclaw
```

Add to corey's PATH:

```bash
echo 'export PATH="$HOME/bin:$PATH"' | sudo -u corey tee -a /home/corey/.bashrc > /dev/null
```

Do the same for Marty's user if needed:

```bash
mkdir -p ~/bin
cp /home/corey/bin/openclaw ~/bin/openclaw
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
```

## Phase 16: Auto-start via systemd

On LUNA the stack started when WSL2 booted. On native Ubuntu, use a systemd service.

```bash
sudo tee /etc/systemd/system/ironclaw.service > /dev/null <<'EOF'
[Unit]
Description=IronForge Docker Stack
After=docker.service
Requires=docker.service

[Service]
Type=oneshot
RemainAfterExit=yes
WorkingDirectory=/opt/stacks/ironclaw
ExecStart=/usr/bin/docker compose up -d
ExecStop=/usr/bin/docker compose down
User=corey
Group=docker

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable ironclaw.service
```

This runs the stack as corey on boot. To manage it manually:

```bash
sudo systemctl start ironclaw    # bring stack up
sudo systemctl stop ironclaw     # bring stack down
sudo systemctl status ironclaw   # check status
```

## Phase 17: Decommission LUNA

Once everything is verified on Quark:

1. Stop the Docker stack on LUNA: `docker compose down`
2. Remove LUNA's deploy key from GitHub
3. Take a final snapshot of LUNA's ironclaw state for the archive
4. Update Tailscale admin console (remove LUNA or update its role)
5. Update memory and documentation to reflect Quark as the primary orchestration hub

## What Gets Simpler on Quark

- No WSL2 memory cap (`.wslconfig` is gone)
- No Windows → WSL2 port forwarding
- Docker starts natively via systemd instead of the WSL2 init shim
- No `.wslconfig` or Windows-side Tailscale considerations
- Phases 1 and 2 from the original luna-commands.md (WSL2 + Ubuntu install) are irrelevant
- The architecture layer diagram simplifies from three layers to two: `Ubuntu 24.04 → Docker Engine`

## Quick Reference: Key Paths on Quark

|Path|Purpose|
|---|---|
|`/opt/stacks/ironclaw/`|IronForge git repo, compose file, configs|
|`/opt/stacks/`|Parent for all Docker Compose stacks|
|`/srv/data/ironclaw/`|Persistent data: Postgres, Qdrant, Ollama, Redis, documents|
|`/srv/data/`|Parent for all persistent stack data|
|`/Quark/`|Runtime symlink root (mirrors old `/Luna` pattern)|
|`/home/corey/`|Claude Code sysadmin home directory|
|`/home/corey/bin/openclaw`|OpenClaw CLI wrapper|

## Quick Reference: User and Group Summary

|User|Role|Groups|
|---|---|---|
|marty (or primary user)|Owner, operator|`sudo`, `docker`, `adm`|
|corey|Claude Code sysadmin|`sudo`, `docker`, `adm`, `systemd-journal`|