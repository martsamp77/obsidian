# Self-Hosted AI Development Stack
## Architecture, Decisions, and Best Practices

---

## Decision Summary

This document captures all hardware, software, and architectural decisions made during planning. Every choice below was deliberate and reflects specific trade-offs discussed.

### Hardware Decisions

**Chose Minisforum UM790 Pro over Beelink SER5 MAX because:**
- The Ryzen 9 7940HS (Zen 4, 4nm) is a full chip generation ahead of the Ryzen 7 5800H (Zen 3, 7nm), with ~14% better single-core performance and significantly better power efficiency for 24/7 server use.
- Minisforum uses a liquid metal cooling system (Cold Wave 2.0) with active cooling for both RAM and SSD — meaningfully better thermal headroom than Beelink's conventional heat pipes.
- Minisforum consistently ranks higher than Beelink for firmware stability, BIOS maturity, and community support — critical for a machine running production workloads continuously.
- The UM790 Pro has been on the market longer, meaning BIOS updates have resolved edge cases and the community has documented solutions for common issues.
- Both use SODIMM slots (user-upgradeable RAM), but the UM790 Pro supports DDR5 up to 64GB vs DDR4 on older Beelink models.

**Chose 32GB now with a planned upgrade to 64GB because:**
- The SER9 MAX with 64GB pre-installed was $1,179 — far over budget.
- The UM790 Pro 32GB + a 2x32GB DDR5-5600 SODIMM kit (~$65) reaches 64GB for ~$465–515 total.
- The RAM upgrade takes approximately 10 minutes — four screws, swap sticks, done.

**Chose Ubuntu Server (bare metal) over Proxmox because:**
- The entire stack is Docker-native. Running Proxmox adds a virtualization layer that consumes RAM and CPU without providing any benefit for a single-purpose Docker server.
- Every VM in Proxmox has overhead just to exist, eating into available RAM before services even start.
- Ubuntu Server bare metal with Docker Compose is simpler, faster, and easier to recover from failure.
- Proxmox is the right tool for multi-VM homelabs with hard isolation requirements — not for this use case.

**Did not choose GitLab (self-hosted) because:**
- Already using GitHub and happy with it. GitLab is the single most resource-hungry service that would have been on this stack — it wants 8GB RAM by itself.
- Removing GitLab significantly reduced hardware requirements and allowed a simpler, cheaper machine choice.

**Did not choose Ollama (local LLM inference) because:**
- Running meaningful models (anything above 13B parameters) requires dedicated GPU VRAM that no current mini PC provides.
- Removing Ollama from the stack eliminates the GPU requirement entirely and makes the stack practical on the UM790 Pro.
- Cloud LLM routing via OpenRouter + LiteLLM provides better model quality and flexibility without local hardware constraints.
- This decision can be revisited if a machine with a discrete GPU (e.g., a used workstation with an RTX 3090) is added to the setup later.

**Chose not to use Proxmox virtualization because:**
- Single-purpose Docker server. Proxmox virtualization overhead wastes RAM on a machine already running a dense Docker stack.
- Ubuntu bare metal + Docker Compose covers all requirements cleanly.

**NAS Decision — Pending:**
- A Synology NAS is the planned choice for bulk storage. This replaces the need for standalone NextCloud on the server.
- Synology Drive handles file sync natively, and the NAS will serve as the primary backup target for Docker volumes.
- This is a future purchase. All NAS-dependent functionality below is noted as placeholder until purchased.

---

## Hardware

### Primary Server — Purchased

| Component | Spec |
|---|---|
| Model | Minisforum UM790 Pro |
| CPU | AMD Ryzen 9 7940HS (Zen 4, 4nm, 8C/16T, up to 5.2GHz) |
| RAM (current) | 32GB DDR5-5600 SODIMM (2x16GB) |
| RAM (target) | 64GB DDR5-5600 SODIMM (2x32GB) |
| Storage | 1TB M.2 PCIe 4.0 NVMe SSD |
| GPU | AMD Radeon 780M (integrated, RDNA 3, 12-core) |
| Network | 2.5GbE LAN, Wi-Fi 6E |
| USB | 2x USB4 (40Gbps), 4x USB 3.2, 2x USB 2.0 |
| Display | 2x HDMI 2.1, 2x USB4 (4 monitors total) |
| Cooling | Cold Wave 2.0 — liquid metal + active RAM/SSD cooler |
| OS | Ubuntu Server 24.04 LTS |

**RAM Upgrade Path:**
- Purchase: Crucial 2x32GB DDR5-5600 SODIMM kit (~$65)
- Installation: Remove 4 bottom screws, swap SODIMM sticks, reassemble (~10 minutes)
- Result: 64GB DDR5-5600 dual-channel

### NAS — PLACEHOLDER (Future Purchase)

A Synology NAS is the planned storage solution. Model TBD based on budget at time of purchase. Recommended options:
- **DS923+** — 4-bay, supports 10GbE add-in card, Ryzen R1600 CPU, expandable to 32GB RAM
- **DS1522+** — 5-bay, stronger CPU, better for heavier Docker workloads on the NAS itself

**When purchased, the NAS will provide:**
- Primary storage for file sync (replacing standalone NextCloud)
- Nightly backup target for Docker volumes from the server
- Media storage
- Synology Drive for cross-device file sync (replaces Dropbox/OneDrive/Box)
- Cloud Sync to Backblaze B2 for encrypted offsite backup

**Until NAS is purchased:**
- Docker volume data lives on the server's 1TB NVMe
- Backups are not yet configured (highest priority item after NAS acquisition)
- No file sync service is running

---

## Operating System and Container Runtime

### Ubuntu Server 24.04 LTS

Installed bare metal on the UM790 Pro. No virtualization layer.

**Initial setup checklist:**
- Disable automatic unattended upgrades for production services (schedule manually)
- Enable UFW firewall, allow only SSH + Tailscale
- Configure SSH key-only authentication, disable password auth
- Set static local IP via router DHCP reservation
- Install Docker Engine (not Docker Desktop) from Docker's official apt repo
- Install Docker Compose v2 plugin
- Add user to docker group
- Enable Wake-on-LAN in BIOS

### Docker

All services run in Docker containers managed via Docker Compose. This is the correct choice for this stack because:
- Every service below ships as a Docker image — this is the primary distribution format.
- Complete isolation between services with independent dependency management.
- Portability: the entire stack lives in compose files in a Git repo. Hardware failure recovery = `git clone` + `docker compose up`.
- Easy updates: `docker pull` + container restart.
- Easy rollback: pin to a previous image tag in the compose file.

**Docker management tools:**
- **Coolify** — primary deployment and management UI. Handles deploys, environment variables, reverse proxy config, and SSL for services deployed through it.
- **Portainer** (optional) — additional container management UI for manually-managed stacks not in Coolify.

**Architecture pattern:**
- Separate Docker Compose file per application stack
- All stacks connected to a shared external Docker network
- Coolify manages the Traefik reverse proxy for all exposed services
- Each service that does not need public exposure is internal-only

---

## Networking and Access

### Tailscale — Install First

Tailscale is the first thing installed before any other service. It creates a private encrypted network between the server and all working machines (home computers, work computers, laptop).

**Why Tailscale first:**
- All services are accessible via Tailscale IP without exposing anything to the public internet.
- Eliminates the need to open firewall ports for most services.
- Works across networks without any router configuration.
- Free tier covers unlimited personal use.

**Setup:**
- Install on the server: `curl -fsSL https://tailscale.com/install.sh | sh`
- Install on every machine used for development
- Set server as a Tailscale subnet router to expose the entire home network when needed
- Access all services via `server-tailscale-ip:port` or subdomain once Traefik is configured

### Traefik — Reverse Proxy

Traefik routes all incoming traffic to the correct service by domain name. Instead of accessing services by port (`server:3001`, `server:8080`), every service gets a proper subdomain:
- `n8n.yourdomain.com`
- `anythingllm.yourdomain.com`
- `plane.yourdomain.com`
- etc.

Traefik is Docker-aware: add labels to a container and Traefik automatically picks it up with no manual config file editing. Coolify manages Traefik configuration for services deployed through it.

**Key features:**
- Automatic Let's Encrypt SSL certificates for all services
- Docker label-based service discovery
- Middleware support for Authentik forward auth (protects services with SSO)

### Authentik — Single Sign-On

Authentik is a self-hosted identity provider (IdP) that adds centralized authentication to every service. One login protects everything.

**Why Authentik:**
- Single point of access control — add or remove access for any service in one place.
- ForwardAuth middleware integrates with Traefik — unauthenticated requests redirect to the Authentik login page.
- Configuration stored as YAML blueprints in Git (infrastructure as code).
- Supports MFA, SSO, OAuth2, SAML.

All services that don't have built-in auth (or whose built-in auth is weak) sit behind Authentik ForwardAuth via Traefik.

---

## Secrets Management

### Infisical — Self-Hosted

All API keys, database passwords, and credentials are stored in Infisical — never in `.env` files on disk or committed to Git.

**Why Infisical:**
- Single source of truth for all secrets across every Docker Compose stack.
- Injects secrets at container runtime via environment variables — keys never touch disk.
- If the server is compromised, credentials are not exposed in flat files.
- Self-hosted, free, runs as a Docker container.

**Setup pattern:**
- Deploy Infisical via Docker Compose
- Add all secrets (API keys for OpenRouter, Anthropic, GitHub, etc.)
- Docker Compose stacks reference Infisical secrets instead of hardcoded values

---

## Core Data Layer

These services are shared infrastructure that multiple other services depend on. They run in a dedicated `core` Docker Compose stack that is always up.

### PostgreSQL

Primary relational database for all services that need structured storage. Running a single shared Postgres instance (rather than one per service) saves RAM and simplifies backups.

**Services using this Postgres instance:**
- n8n (workflow state, execution history)
- Plane (project data)
- Authentik (user data, sessions)
- AnythingLLM (metadata)
- mem0 (memory storage)
- Infisical (secrets storage)
- Any future services

Each service gets its own database within the shared Postgres instance for isolation.

### Redis

Shared cache and session store. Used by multiple services for job queues, session management, and caching.

**Services using Redis:**
- Authentik (session management)
- n8n (job queues)
- LiteLLM (response caching)
- Any future services requiring caching

### Qdrant

Purpose-built vector database for semantic search and RAG (Retrieval-Augmented Generation). Faster and more capable than pgvector (Postgres vector extension) for high-dimensional embeddings.

**Used by:**
- mem0 MCP server (agent memory embeddings)
- AnythingLLM (document embeddings for knowledge base workspaces)
- Any future RAG pipelines

---

## AI Stack

### LiteLLM — LLM Gateway

LiteLLM is the internal API gateway that sits between all AI-consuming services and the actual LLM providers. Every service (Open WebUI, n8n, AnythingLLM, Dify, Claude Code, Cursor) points to LiteLLM instead of calling providers directly.

**Why LiteLLM:**
- Single API key management point — rotate a key in LiteLLM, not in 6 different services.
- Model routing: different tasks can be routed to different models automatically.
- Response caching via Redis — identical requests return cached responses, saving API costs.
- Usage tracking and cost monitoring across all services.
- OpenAI-compatible API — any service that speaks to OpenAI can point to LiteLLM instead.

**Routing strategy:**
- Fast/cheap model (GPT-4o Mini or Claude Haiku) → code completion, quick questions, classification tasks
- Strong model (Claude Sonnet or GPT-4o) → architecture decisions, complex generation, reasoning
- Specialized coding model (DeepSeek Coder via OpenRouter) → pure code generation tasks
- LiteLLM selects the right model based on route configuration

### OpenRouter — Cloud LLM Routing

OpenRouter provides access to models from multiple providers (Anthropic, OpenAI, Google, Meta, Mistral, DeepSeek, etc.) through a single API endpoint. LiteLLM uses OpenRouter as a backend for cloud model access.

**Why OpenRouter:**
- Access to every major cloud model through one account and one API key in LiteLLM.
- Intelligent fallback — if one provider is down, OpenRouter routes to an equivalent model.
- Cost comparison across providers for the same model.
- Particularly useful for OpenClaw/open source model variants not available through direct APIs.

### Open WebUI — Primary AI Chat Interface

Open WebUI is the main day-to-day interface for interacting with AI models. It connects to LiteLLM for model access.

**Features used:**
- Multi-model conversations
- Document upload and in-context chat
- Workspace/conversation organization
- Web search integration (via SearXNG)
- Agent pipelines

### SearXNG — Self-Hosted Web Search

SearXNG is a self-hosted meta-search engine that aggregates results from multiple search providers without tracking. Used by Open WebUI and n8n to give AI agents access to real-time web search.

**Why self-hosted search:**
- Privacy — search queries don't go to Google/Bing/etc. directly.
- No rate limiting compared to free API tiers.
- Aggregates results from multiple sources simultaneously.

### Dify — AI Application Builder

Dify is a drag-and-drop platform for building and deploying AI-powered features inside products. It is separate from AnythingLLM — it is for building things, not for personal knowledge management.

**Use cases:**
- Building AI-powered features for products being developed
- Creating agent workflows with visual tools rather than code
- Deploying AI chat widgets that embed in web apps
- Multi-step agentic pipelines with API deployment

Connects to LiteLLM for all model access.

---

## Memory and Knowledge Systems

### mem0 — Persistent Memory Across All AI Tools

mem0 is a self-hosted MCP (Model Context Protocol) server that gives persistent, semantic memory to every AI tool used: Claude Code, Cursor, VS Code, Open WebUI.

**The problem it solves:**
Without mem0, every conversation with every AI tool starts from zero. Debugging insight discovered in Cursor is not available in Claude Code the next day. Architectural decisions made two weeks ago are forgotten. mem0 fixes this.

**How it works:**
- When working in Claude Code, Cursor, or any MCP-compatible tool, the AI stores important information (debugging solutions, architectural decisions, project context, personal preferences) as memories.
- Memories are stored as vector embeddings in Qdrant.
- On the next session in any connected tool, relevant memories are automatically retrieved and injected into context.
- Semantic search — "flaky test debugging" retrieves a memory about "state leakage between test groups" even though the wording is different.

**Technical setup:**
- Uses the `mem0-mcp-selfhosted` server (github.com/elvismdev/mem0-mcp-selfhosted)
- Connects to existing Qdrant instance on `localhost:6333`
- Uses LiteLLM for embedding model (or a dedicated local embedding model)
- Optional: Neo4j for knowledge graph layer (entities and relationships between memories)

**Integration per tool:**
- **Claude Code:** `claude mcp add --scope user --transport stdio mem0 -- uvx --from git+https://github.com/elvismdev/mem0-mcp-selfhosted.git mem0-mcp-selfhosted` (global, works across all projects)
- **Cursor:** Point MCP config to the same server via HTTP/SSE transport
- **VS Code:** MCP extension pointing to server endpoint
- **ChatGPT / Grok:** Browser extension or manual context injection (limited integration)

**Scope:**
- Personal development knowledge (debugging solutions, code patterns, architectural decisions)
- Project context (what each product does, tech stack, decisions made)
- Personal life management information
- Preferences and working style

### AnythingLLM — Centralized Knowledge Base

AnythingLLM is the primary knowledge base — a RAG system where documents are embedded into vector storage and become queryable via natural language. Every piece of important company information, technical documentation, research, and process documentation lives here.

**Why AnythingLLM over a DIY pipeline:**
- Replaces the fragile DIY approach (Outline + n8n ingestion + custom Qdrant pipeline) with a single purpose-built tool.
- Built-in document chunking, embedding, and vector storage — no custom pipeline to maintain.
- Workspace-based organization — one workspace per company, per project, per topic.
- Built-in authentication and multi-user support.
- Citation support — answers reference specific passages in source documents.
- REST API at `http://localhost:3001/api` — n8n can programmatically add documents.
- Supports Qdrant as vector backend (uses existing Qdrant instance).
- MIT licensed, completely free.

**Workspace structure:**
- One workspace per company being built
- One workspace for personal life management
- One workspace for technical stack documentation
- One workspace for research and notes
- Workspaces are completely isolated from each other

**Document ingestion:**
- Manual upload via UI (PDFs, Word docs, Markdown, text files)
- n8n automation for automatic ingestion from watched folders
- GitHub repo ingestion for codebase documentation
- API-based ingestion from other tools

**Relationship to mem0:**
- mem0 handles *agent memory* — episodic, conversational, automatically captured during work sessions
- AnythingLLM handles *knowledge* — intentionally curated documents, references, and information
- They are complementary, not redundant

---

## Automation

### n8n — Workflow Automation

n8n is the automation backbone. It connects all services and automates repetitive tasks without writing custom code.

**Planned workflows:**
- Document ingestion: watch a folder or GitHub repo → chunk and ingest into AnythingLLM workspace
- Notification routing: aggregate alerts from Uptime Kuma, backups, and services → send to ntfy
- AI pipeline automation: trigger AI tasks on schedules or events
- Cross-service integration: when something happens in Plane, update AnythingLLM
- Web research: trigger SearXNG searches, process results with AI via LiteLLM, store in knowledge base

Connects to: Postgres, Redis, LiteLLM, AnythingLLM API, Plane API, GitHub API, SearXNG, ntfy.

---

## Project Management

### Plane — Self-Hosted Monday.com Alternative

Plane is an open-source project management tool that covers the same use case as Monday.com: projects, issues, boards, cycles (sprints), modules, and Gantt views.

**Why Plane over other options:**
- Closest 1:1 match to Monday.com's mental model — views include board, list, spreadsheet, and Gantt.
- Actively maintained, modern UI.
- Completely free and open source (AGPL v3).
- Docker Compose deployment.

**Use cases:**
- Tracking tasks across all companies being built
- Sprint planning for product development
- Personal task management

### AppFlowy — Self-Hosted Notion Alternative (Optional)

AppFlowy is an open-source Notion alternative for notes, wikis, and flexible document organization. Listed as optional — AnythingLLM may cover enough of this use case.

---

## Monitoring and Observability

### Uptime Kuma — Service Status Monitoring

Uptime Kuma monitors every running service and sends alerts when something goes down. Lightweight, single container, clean UI.

**Monitors:**
- All Docker services (HTTP endpoint checks)
- Postgres (TCP check)
- Redis (TCP check)
- Qdrant (HTTP check)
- Server SSH availability

**Notifications:** Connected to ntfy for phone push notifications.

### Beszel — Server Metrics

Beszel provides lightweight CPU, RAM, disk, and network monitoring for the server. More appropriate than the full Grafana + Prometheus stack for a single server — less overhead and easier to maintain.

### ntfy — Push Notifications

Self-hosted push notification service. All other monitoring and automation tools (Uptime Kuma, n8n, backup scripts) send alerts to a single ntfy instance, which delivers them to the phone via the ntfy app.

**Notification sources:**
- Uptime Kuma (service down/up alerts)
- Backup completion/failure
- n8n workflow failures
- Docker container crashes (via Monocker or Watchtower)

---

## Backup Strategy

Backups follow the 3-2-1 rule: three copies, on two different media, with one offsite.

| Copy | Location | Method | Status |
|---|---|---|---|
| Copy 1 | Server NVMe (live data) | Docker volumes | Active |
| Copy 2 | NAS (local backup) | docker-volume-backup via NFS | **PLACEHOLDER — pending NAS purchase** |
| Copy 3 | Backblaze B2 (offsite) | Synology Cloud Sync | **PLACEHOLDER — pending NAS purchase** |

### Docker Volume Backup

`offen/docker-volume-backup` runs nightly at 3:00 AM and backs up all Docker volumes to the NAS NFS share. It gracefully stops containers before backup to prevent partial writes, then restarts them.

**Until NAS is available:** No automated backup is in place. This is the highest priority item after the NAS is purchased.

### PostgreSQL Logical Backups

In addition to volume backups, `pg_dump` runs nightly for each Postgres database. Volume backups can be corrupted; logical dumps are always restorable regardless of storage state.

Script runs via cron or n8n, outputs to a dedicated backup directory, then gets picked up by the volume backup process.

### Backblaze B2 — Offsite Cold Storage

Backblaze B2 is the offsite disaster recovery destination. At $6/TB/month it is the cheapest reliable offsite storage available. The Synology NAS handles the sync via Hyper Backup, pushing encrypted snapshots automatically.

**This is configured on the NAS when purchased — not on the server directly.**

### Restore Testing

The most overlooked step in any backup strategy. Once per month, actually restore something from backup to verify it works. Untested backups are not real backups.

---

## Personal Productivity

### Hoarder (or Linkwarden) — Bookmark Manager with AI

Self-hosted bookmark manager that auto-tags and summarizes web pages on save using AI. Every research link, tool discovery, and reference gets automatically organized and searchable.

**Integration:** n8n can push saved bookmarks into AnythingLLM for inclusion in the knowledge base.

### PostHog — Self-Hosted Product Analytics

PostHog provides real user behavior analytics for products being built. Self-hosted, free, and privacy-preserving — no data goes to Google Analytics.

Used for: tracking usage of products being built, understanding user behavior, funnel analysis.

### Glance — Homelab Dashboard

Single-page dashboard showing all running services with live status, quick links, and system stats. Replaces a browser full of bookmarked `server:port` URLs with a clean unified home page.

---

## Development Workflow

### AI Coding Tools

| Tool | Usage |
|---|---|
| Claude Code | Primary agentic coding — terminal-based, all projects |
| Cursor | Primary IDE with AI, used extensively for vibe coding |
| VS Code | Secondary editor, available with MCP via extension |
| ChatGPT | Occasional use |
| Grok | Occasional use |

All tools point to the mem0 MCP server for persistent memory. Claude Code and Cursor are the primary tools; all others are supplementary.

### GitHub

All project code lives on GitHub. Decision was made to stay with GitHub rather than self-hosting GitLab, avoiding ~8GB RAM overhead from GitLab's full stack.

GitHub Actions can be supplemented with a self-hosted runner connected to the server for private pipeline work.

### Infrastructure as Code

All Docker Compose files, Traefik configurations, Authentik blueprints, and environment templates live in a private GitHub repository. No service configuration exists only on the server.

**Recovery procedure:**
1. Install Ubuntu Server 24.04 on new hardware
2. Install Docker, Tailscale
3. `git clone` infrastructure repo
4. `docker compose up` per stack
5. Restore data from Synology NAS backup
6. Back online in under 2 hours

**What goes in Git:**
- All `docker-compose.yml` files
- All Traefik config files
- Authentik blueprints
- `.env.example` files (templates without values — actual values in Infisical)
- n8n workflow exports
- Backup scripts and cron configurations

**What never goes in Git:**
- Actual `.env` files with credentials
- API keys of any kind
- Database dumps
- Private keys or certificates

---

## Complete Service Inventory

### Always-On Core Services

| Service | Purpose | Port | Notes |
|---|---|---|---|
| Traefik | Reverse proxy, SSL | 80, 443 | Managed by Coolify |
| Authentik | SSO / identity provider | 9000, 9443 | Protects all services |
| PostgreSQL | Primary database | 5432 | Shared by all services |
| Redis | Cache / session store | 6379 | Shared by all services |
| Qdrant | Vector database | 6333, 6334 | Shared by mem0 + AnythingLLM |
| Tailscale | VPN / secure access | — | System service, not Docker |

### AI Stack Services

| Service | Purpose | Port | Notes |
|---|---|---|---|
| LiteLLM | LLM gateway / router | 4000 | All AI tools point here |
| Open WebUI | AI chat interface | 3000 | Connects to LiteLLM |
| SearXNG | Self-hosted web search | 8080 | Used by Open WebUI + n8n |
| AnythingLLM | RAG knowledge base | 3001 | Workspace per project/company |
| Dify | AI app/agent builder | 80 | For building AI features in products |
| mem0 MCP server | Persistent AI memory | 8765 | MCP via stdio or HTTP |

### Automation and Productivity Services

| Service | Purpose | Port | Notes |
|---|---|---|---|
| n8n | Workflow automation | 5678 | Connects everything |
| Coolify | Docker management / deploys | 8000 | Manages other containers |
| Plane | Project management | 3000 | Monday.com alternative |
| AppFlowy | Notes / wiki | 8080 | Notion alternative (optional) |
| Infisical | Secrets management | 8080 | All API keys stored here |
| Hoarder / Linkwarden | Bookmarks with AI | 3000 | Research and reference links |
| PostHog | Product analytics | 8000 | For products being built |

### Monitoring Services

| Service | Purpose | Port | Notes |
|---|---|---|---|
| Uptime Kuma | Service uptime monitoring | 3001 | Alerts to ntfy |
| Beszel | Server metrics | 3000 | CPU, RAM, disk, network |
| ntfy | Push notifications | 80 | Aggregates all alerts |

### Backup Services

| Service | Purpose | Notes |
|---|---|---|
| docker-volume-backup | Nightly Docker volume backups | Target: NAS NFS share (PLACEHOLDER) |
| pg_dump cron | Nightly Postgres logical backups | Runs per-database |
| Backblaze B2 sync | Offsite cold storage | Via Synology Hyper Backup (PLACEHOLDER) |

### Glance Dashboard

| Service | Purpose | Port |
|---|---|---|
| Glance | Unified homelab dashboard | 8080 |

---

## Implementation Order

Do not try to deploy everything at once. Follow this order — each phase builds on the previous one.

### Phase 1 — Foundation (Week 1)

1. Install Ubuntu Server 24.04 LTS bare metal on UM790 Pro
2. Install Docker Engine + Docker Compose v2
3. Install Tailscale — do this before anything else is exposed
4. Configure UFW firewall (allow SSH + Tailscale only)
5. Configure SSH key-only auth, disable password login
6. Deploy Traefik via Coolify
7. Deploy Authentik
8. Verify all traffic routes through Tailscale + Authentik before continuing

### Phase 2 — Backup and Secrets (Week 1–2)

1. Deploy Infisical, add all API keys and credentials
2. Update all `.env` files to pull from Infisical
3. Purchase NAS (highest priority infrastructure item)
4. Configure NFS share on NAS
5. Deploy docker-volume-backup pointed at NAS
6. Set up pg_dump cron
7. Configure Synology Cloud Sync to Backblaze B2
8. **Test a restore before moving on**

### Phase 3 — Core Data Layer (Week 2)

1. Deploy shared PostgreSQL with per-service databases
2. Deploy shared Redis
3. Deploy Qdrant
4. Verify all three are accessible internally and behind Tailscale

### Phase 4 — AI Stack (Week 2–3)

1. Deploy LiteLLM, configure OpenRouter as backend
2. Set up routing rules (cheap model / strong model / coding model)
3. Deploy Open WebUI, point to LiteLLM
4. Deploy SearXNG, connect to Open WebUI
5. Deploy mem0 MCP server, connect to Qdrant
6. Wire mem0 into Claude Code (`claude mcp add --scope user`)
7. Wire mem0 into Cursor (MCP config)
8. Validate memory is persisting across sessions

### Phase 5 — Knowledge Base (Week 3)

1. Deploy AnythingLLM
2. Configure Qdrant as vector backend
3. Create workspaces (one per company, personal, dev stack, research)
4. Begin populating with existing documents and notes
5. Set up n8n ingestion workflow for automatic document addition

### Phase 6 — Automation and Productivity (Week 3–4)

1. Deploy n8n
2. Deploy Plane
3. Deploy Infisical (if not done in Phase 2)
4. Deploy Dify
5. Deploy Hoarder/Linkwarden
6. Deploy PostHog
7. Deploy Glance dashboard

### Phase 7 — Monitoring (Week 4)

1. Deploy Uptime Kuma, add all service checks
2. Deploy Beszel
3. Deploy ntfy
4. Connect all alert sources to ntfy
5. Verify phone receives alerts

### Phase 8 — GitOps (Week 4)

1. Create private GitHub repo for infrastructure
2. Commit all Docker Compose files
3. Commit all config files, Authentik blueprints
4. Verify full stack can be reproduced from Git + NAS backup
5. Set up GitHub Actions self-hosted runner for CI/CD pipeline

---

## Services Explicitly Ruled Out

These were considered and rejected during planning.

| Service | Reason Rejected |
|---|---|
| GitLab (self-hosted) | 8GB RAM overhead, already using GitHub and happy with it |
| Ollama (local LLM) | Requires dedicated GPU VRAM; no practical mini PC has sufficient VRAM for useful models |
| Proxmox | Single-purpose Docker server; virtualization overhead with no benefit |
| NextCloud (standalone) | Replaced by Synology Drive on the NAS |
| Box | Enterprise compliance tool; no value for solo development setup |
| Dropbox / OneDrive | Cloud vendor lock-in; Synology Drive serves same purpose self-hosted |
| Kubernetes / K3s | Overkill for single-node setup; Docker Compose covers all requirements |
| Supermemory.ai / memoryplugin.com | Browser-only; not persistent across all development tools |
| DIY RAG pipeline (Outline + n8n + custom Qdrant) | Fragile multi-tool pipeline replaced by AnythingLLM |
| Supabase (as primary DB) | Heavy 8-10 container stack; existing Postgres + pgvector covers the use case without the overhead |

---

## RAM Budget Reference

This is an approximate RAM allocation guide for planning purposes. Actual usage will vary.

| Category | Services | Approx. RAM |
|---|---|---|
| Core infrastructure | Postgres, Redis, Qdrant | ~2–3GB |
| Networking / Auth | Traefik, Tailscale, Authentik | ~1–2GB |
| AI stack | LiteLLM, Open WebUI, SearXNG, AnythingLLM, Dify | ~4–6GB |
| Memory | mem0 MCP server | ~500MB |
| Automation | n8n | ~1GB |
| Productivity | Plane, AppFlowy, Infisical | ~2–3GB |
| Monitoring | Uptime Kuma, Beszel, ntfy | ~500MB |
| Backup | docker-volume-backup | ~200MB |
| Productivity extras | Hoarder, PostHog, Glance | ~1–2GB |
| OS + Docker overhead | Ubuntu, Docker daemon | ~2GB |
| **Total estimated** | | **~15–20GB active** |
| **With 64GB RAM** | | **~44GB headroom** |

This confirms 64GB RAM is the right target. With 32GB during the initial period before the RAM upgrade, the stack will run but with tighter headroom. Deploy Phase 1–3 first, then upgrade RAM before deploying the full AI stack.

---

## Quick Reference — Key Ports

Once Traefik and subdomains are configured, port access is replaced by domain names. This table is for initial setup and debugging.

| Service | Default Port |
|---|---|
| Traefik dashboard | 8080 |
| Authentik | 9000 |
| PostgreSQL | 5432 |
| Redis | 6379 |
| Qdrant | 6333 |
| LiteLLM | 4000 |
| Open WebUI | 3000 |
| SearXNG | 8080 |
| AnythingLLM | 3001 |
| n8n | 5678 |
| Coolify | 8000 |
| Plane | varies |
| Infisical | 8080 |
| Uptime Kuma | 3001 |
| Beszel | 3000 |
| ntfy | 80 |
| Glance | 8080 |
| PostHog | 8000 |

---

## Pending Items and Open Questions

| Item | Status | Notes |
|---|---|---|
| NAS purchase | Pending | Highest priority hardware item; unlocks backup strategy |
| RAM upgrade to 64GB | Pending | Buy 2x32GB DDR5-5600 SODIMM (~$65); 10-minute install |
| Domain name for internal services | Decision needed | Required for Traefik subdomain routing and SSL certs |
| Backblaze B2 account | Pending | Set up when NAS arrives |
| GitHub Actions self-hosted runner | Phase 8 | Set up after core stack is stable |
| NAS model selection | Pending | DS923+ or DS1522+ recommended |

---

*Document last updated: April 2026*
*Stack status: Planning complete, hardware purchased (UM790 Pro), pending initial Ubuntu setup*