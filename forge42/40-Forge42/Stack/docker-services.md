---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-19
---

# Docker Services

Complete inventory of all Docker services running on LUNA. All services run in containers managed by Docker Compose.

## Core Infrastructure — Always On

These run in a dedicated `core` stack. All other services depend on them.

| Service | Purpose | Port | Auth | Status |
|---|---|---|---|---|
| Traefik | Reverse proxy, SSL termination | 80, 443 | — | Managed by Coolify |
| Authentik | SSO / identity provider (ForwardAuth) | 9000, 9443 | Admin login | Protects all services |
| PostgreSQL | Primary relational database (shared) | 5432 | DB credentials | Internal only by default |
| Redis | Cache + session store (shared) | 6379 | — | Internal only |
| Qdrant | Vector database | 6333, 6334 | — | Shared by mem0 + AnythingLLM |
| Infisical | Secrets management | 8080 | Admin login | All API keys stored here |

## AI Stack

| Service | Purpose | Port | Auth | Notes |
|---|---|---|---|---|
| LiteLLM | LLM gateway / router | 4000 | API key | All AI tools point here |
| Open WebUI | AI chat interface | 3000 | Authentik / built-in | Primary day-to-day AI UI |
| SearXNG | Self-hosted web search | 8080 | — | Used by Open WebUI + n8n |
| AnythingLLM | RAG knowledge base | 3001 | Built-in | Workspace per company |
| Dify | AI app / agent builder | 80 | Built-in | For building AI product features |
| mem0 MCP server | Persistent AI memory | 8765 | — | MCP via stdio or HTTP |

## Automation & Productivity

| Service | Purpose | Port | Auth | Notes |
|---|---|---|---|---|
| n8n | Workflow automation | 5678 | Built-in | Connects all services |
| Coolify | Docker deployment / management UI | 8000 | Admin login | Manages Traefik + other containers |
| Plane | Project management (Monday.com alt) | varies | Built-in | Issues, boards, sprints |
| AppFlowy | Notes / wiki (Notion alt, optional) | 8080 | Built-in | May be superseded by AnythingLLM |
| Hoarder / Linkwarden | Bookmarks with AI tagging | 3000 | Built-in | Research and reference links |
| PostHog | Product analytics | 8000 | Admin login | For D42 products being built |

## Monitoring

| Service | Purpose | Port | Auth | Notes |
|---|---|---|---|---|
| Uptime Kuma | Service uptime monitoring | 3001 | Built-in | Alerts to ntfy |
| Beszel | Server metrics (CPU, RAM, disk, net) | 3000 | Built-in | Lightweight, single-server |
| ntfy | Push notifications | 80 | — | Aggregates alerts from all services |
| Glance | Homelab dashboard | 8080 | — | Single-page overview of all services |

## Backup

| Service | Purpose | Notes |
|---|---|---|
| docker-volume-backup (offen) | Nightly Docker volume backups | Target: NAS NFS share — PLACEHOLDER until NAS purchased |
| pg_dump cron | Nightly PostgreSQL logical backups | Runs per-database; output to backup directory |
| Backblaze B2 sync | Offsite cold storage | Via Synology Hyper Backup — PLACEHOLDER until NAS purchased |

## Port Quick Reference

| Port | Service |
|---|---|
| 80, 443 | Traefik |
| 3000 | Open WebUI |
| 3001 | AnythingLLM (also Uptime Kuma — check for conflict) |
| 4000 | LiteLLM |
| 5432 | PostgreSQL |
| 5678 | n8n |
| 6333 | Qdrant |
| 6379 | Redis |
| 8000 | Coolify |
| 8080 | SearXNG / Traefik dashboard |
| 8765 | mem0 MCP server |
| 9000, 9443 | Authentik |

## Docker Network

All services run on a shared external Docker network for inter-container communication by name (e.g., `postgres`, `litellm`, `qdrant`). Separate Compose files per stack; all stacks reference the same external network.

## Services Explicitly Not Running

| Service | Reason |
|---|---|
| GitLab (self-hosted) | 8GB RAM overhead; already using GitHub |
| Ollama (local LLM) | No discrete GPU; cloud models better quality |
| Proxmox | Single-purpose Docker server; virtualization overhead adds nothing |
| NextCloud (standalone) | Replaced by Synology Drive on planned NAS |
| Supabase | 8–10 container overhead; existing Postgres covers the use case |
| DIY RAG pipeline | Fragile; replaced by AnythingLLM |

## Related

- [[architecture]] — overall system architecture
- [[docker-commands]] — common Docker operations and compose commands
- [[litellm-config]] — LiteLLM gateway configuration
- [[backup-recovery]] — backup services detail
