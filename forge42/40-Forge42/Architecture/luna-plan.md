---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-19
---

# LUNA Build and Upgrade Plan

LUNA is the primary Forge42 server. This document tracks current hardware specs, implementation progress, and the upgrade/migration roadmap.

## Hardware Specs

| Component | Spec | Status |
|---|---|---|
| Model | Minisforum UM790 Pro | Purchased |
| CPU | AMD Ryzen 9 7940HS (Zen 4, 4nm, 8C/16T, up to 5.2GHz) | Active |
| RAM (current) | 32GB DDR5-5600 SODIMM (2x16GB) | Active |
| RAM (target) | 64GB DDR5-5600 SODIMM (2x32GB) | **Pending upgrade** |
| Storage | 1TB M.2 PCIe 4.0 NVMe SSD | Active |
| GPU | AMD Radeon 780M (integrated, RDNA 3, 12-core) | Not used for AI inference |
| Network | 2.5GbE LAN, Wi-Fi 6E | Active |
| Cooling | Cold Wave 2.0 — liquid metal + active RAM/SSD cooler | Active |
| OS | Ubuntu Server 24.04 LTS (bare metal) | Active |
| IP | 192.168.0.11 (DHCP reservation) | Active |
| RustDesk | 470 061 280 | Active |

## RAM Upgrade

**Buy:** Crucial 2x32GB DDR5-5600 SODIMM kit (~$65)
**Install:** Remove 4 bottom screws, swap SODIMM sticks — ~10 minutes
**Result:** 64GB dual-channel, ~44GB headroom after full stack deployment

**When to upgrade:** Before deploying the full AI stack (Phase 4). At 32GB the stack runs but tightly — deploy Phases 1–3 first.

## Implementation Progress

8 deployment phases. Check off phases as completed.

### Phase 1 — Foundation
- [ ] Install Ubuntu Server 24.04 LTS bare metal
- [ ] Install Docker Engine + Docker Compose v2 plugin
- [ ] Install Tailscale (do this before anything else is exposed)
- [ ] Configure UFW firewall (SSH + Tailscale only)
- [ ] SSH key-only auth, disable password login
- [ ] Deploy Traefik via Coolify
- [ ] Deploy Authentik
- [ ] Verify all traffic routes through Tailscale + Authentik

### Phase 2 — Backup and Secrets
- [ ] Deploy Infisical, add all API keys
- [ ] Update all `.env` references to pull from Infisical
- [ ] Purchase NAS (highest priority hardware)
- [ ] Configure NFS share on NAS
- [ ] Deploy docker-volume-backup pointed at NAS NFS share
- [ ] Set up pg_dump cron
- [ ] Configure Synology Cloud Sync → Backblaze B2
- [ ] **Test a restore before moving on**

### Phase 3 — Core Data Layer
- [ ] Deploy PostgreSQL (shared, per-service databases)
- [ ] Deploy Redis
- [ ] Deploy Qdrant
- [ ] Verify all three accessible internally via Tailscale

### Phase 4 — AI Stack (upgrade RAM to 64GB first)
- [ ] Deploy LiteLLM, configure OpenRouter as backend
- [ ] Set up routing rules (fast/cheap, strong, coding)
- [ ] Deploy Open WebUI, point to LiteLLM
- [ ] Deploy SearXNG, connect to Open WebUI
- [ ] Deploy mem0 MCP server, connect to Qdrant
- [ ] Wire mem0 into Claude Code globally
- [ ] Wire mem0 into Cursor via MCP config
- [ ] Validate memory persists across sessions

### Phase 5 — Knowledge Base
- [ ] Deploy AnythingLLM
- [ ] Configure Qdrant as vector backend
- [ ] Create workspaces (dimension42, 37metrics-ops, personal, dev-stack, research)
- [ ] Begin populating with existing documents
- [ ] Set up n8n ingestion workflow

### Phase 6 — Automation and Productivity
- [ ] Deploy n8n
- [ ] Deploy Plane
- [ ] Deploy Dify
- [ ] Deploy Hoarder/Linkwarden
- [ ] Deploy PostHog
- [ ] Deploy Glance dashboard

### Phase 7 — Monitoring
- [ ] Deploy Uptime Kuma, add all service checks
- [ ] Deploy Beszel
- [ ] Deploy ntfy, connect all alert sources
- [ ] Verify phone receives alerts

### Phase 8 — GitOps
- [ ] Create private GitHub repo for infrastructure
- [ ] Commit all Docker Compose files and configs
- [ ] Verify full stack reproducible from Git + NAS backup
- [ ] Set up GitHub Actions self-hosted runner

## Pending Items

| Item | Priority | Notes |
|---|---|---|
| NAS purchase | Critical | Unlocks backup strategy; DS923+ or DS1522+ recommended |
| RAM upgrade to 64GB | High | ~$65 Crucial kit; install before Phase 4 AI stack |
| Domain for internal services | Decision needed | Required for Traefik subdomain routing + SSL certs |
| Backblaze B2 account | Pending | Set up when NAS arrives |
| GitHub Actions self-hosted runner | Phase 8 | After core stack is stable |

## Network Notes

LUNA is accessible on the LAN at `192.168.0.11`. All Docker services are accessible via Tailscale VPN from any enrolled device.

**Current networking mode:** LUNA is Windows 11 with WSL2 Ubuntu 24.04 (per docker-portainer-guide.md). For LAN service exposure, WSL2 mirrored networking mode is the recommended path — see [[docker-commands]] for setup.

## Related

- [[machine-inventory]] — all machines in the network
- [[docker-services]] — complete service inventory
- [[architecture]] — overall system architecture
- [[backup-recovery]] — backup strategy and restore procedures
