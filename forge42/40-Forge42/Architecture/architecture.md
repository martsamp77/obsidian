---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-19
---

# Forge42 System Architecture

Overall architecture of the Forge42 infrastructure layer — the foundation running the Development Suite (Claude Code) and future Business Planning Suite.

## Design Principles

1. **Docker-native, Compose-first.** Every service runs in a container managed by Docker Compose. The entire stack can be reproduced from `git clone` + `docker compose up`.
2. **Tailscale-first access.** All services are accessed through the Tailscale VPN. Nothing is exposed to the public internet.
3. **Single-node for now.** Primary server is LUNA (Minisforum UM790 Pro). No clustering, no Kubernetes, no virtualization overhead.
4. **Credentials in Infisical.** No `.env` files with real values committed to Git. All secrets managed by the self-hosted Infisical instance.

## Architecture Diagram

```
┌────────────────────────────────────────────────────────────────┐
│  External Access (Tailscale VPN)                               │
│  Nova (MacBook) · Aurora (storage) · other devices             │
└─────────────────────────────┬──────────────────────────────────┘
                              │ Tailscale encrypted tunnel
┌─────────────────────────────▼──────────────────────────────────┐
│  LUNA — Primary Server (192.168.0.11)                          │
│  Minisforum UM790 Pro · Ubuntu Server 24.04 · Docker Engine    │
│                                                                │
│  ┌────────────────┐  ┌─────────────────────────────────────┐   │
│  │ Traefik        │  │ Authentik SSO                       │   │
│  │ Reverse proxy  │  │ ForwardAuth middleware for all svcs  │   │
│  └───────┬────────┘  └─────────────────────────────────────┘   │
│          │                                                     │
│  ┌───────▼────────────────────────────────────────────────┐    │
│  │  AI Stack                                              │    │
│  │  LiteLLM:4000 → OpenRouter → cloud models             │    │
│  │  Open WebUI:3000  SearXNG:8080  AnythingLLM:3001      │    │
│  │  mem0 MCP:8765    Dify (agent builder)                │    │
│  └────────────────────────────────────────────────────────┘    │
│                                                                │
│  ┌────────────────────────────────────────────────────────┐    │
│  │  Core Data Layer                                       │    │
│  │  PostgreSQL:5432  Redis:6379  Qdrant:6333              │    │
│  └────────────────────────────────────────────────────────┘    │
│                                                                │
│  ┌────────────────────────────────────────────────────────┐    │
│  │  Automation & Productivity                             │    │
│  │  n8n:5678  Plane  Infisical  Coolify:8000             │    │
│  └────────────────────────────────────────────────────────┘    │
│                                                                │
│  ┌────────────────────────────────────────────────────────┐    │
│  │  Monitoring                                            │    │
│  │  Uptime Kuma  Beszel  ntfy                            │    │
│  └────────────────────────────────────────────────────────┘    │
└────────────────────────────────────────────────────────────────┘
         │
         │  NFS backup — PLACEHOLDER (pending NAS purchase)
         ▼
┌──────────────────────┐
│  NAS (future)        │
│  Synology DS923+     │
│  ↓ Cloud Sync        │
│  Backblaze B2        │
└──────────────────────┘
```

## Layer Breakdown

### Networking Layer

| Component | Role | Notes |
|---|---|---|
| Tailscale | Zero-trust VPN, primary access method | Installed before any service |
| Traefik | Reverse proxy, SSL termination, service routing | Managed by Coolify |
| Authentik | SSO / ForwardAuth for all services | MFA, OAuth2, SAML |
| UFW firewall | Host-level: SSH + Tailscale only | Nothing else exposed |

### Core Data Layer

| Component | Role | Shared by |
|---|---|---|
| PostgreSQL | Primary relational database | n8n, Plane, Authentik, AnythingLLM, mem0, Infisical |
| Redis | Cache, session store, job queues | Authentik, n8n, LiteLLM |
| Qdrant | Vector database for embeddings | mem0, AnythingLLM |

### AI Stack

See [[ai-orchestration]] for the full AI layer architecture and model routing.

### Infrastructure as Code

All Docker Compose files, Traefik configs, and Authentik blueprints live in a private GitHub repo. Nothing committed except `.env.example` templates — actual values live in Infisical.

**Full recovery procedure:**
1. Install Ubuntu Server 24.04 on new hardware
2. Install Docker Engine + Docker Compose v2 plugin
3. Install Tailscale
4. `git clone` infrastructure repo
5. `docker compose up` per stack
6. Restore data from NAS backup
7. Back online in under 2 hours

## Key Architecture Decisions

| Decision | Choice | Reason |
|---|---|---|
| OS | Ubuntu Server 24.04 bare metal | Single-purpose Docker server; Proxmox adds overhead with no benefit |
| Container management | Docker Compose + Coolify | Portable, reproducible, every service ships as Docker image |
| LLM inference | Cloud via LiteLLM + OpenRouter | No discrete GPU in UM790 Pro; cloud is better quality anyway |
| Source control | GitHub (cloud) | Avoids 8GB GitLab self-hosted RAM overhead |
| File sync | Synology Drive (planned NAS) | Replaces standalone NextCloud on server |
| Auth | Authentik ForwardAuth | Single login protects everything via Traefik middleware |
| Secrets | Infisical self-hosted | Single source of truth; credentials never in flat files on disk |

## Related

- [[ai-orchestration]] — AI stack layer in detail
- [[docker-services]] — complete service inventory with ports
- [[luna-plan]] — LUNA hardware and implementation roadmap
- [[machine-inventory]] — all machines in the network
- [[model-routing-policy]] — which task types go to which model tier
