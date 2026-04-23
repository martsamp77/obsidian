---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-19
---

# Home Network

## IP Assignments

| Device | Hostname | IP | Type |
|---|---|---|---|
| Gateway / Router | — | 192.168.0.1 | Static |
| Pi-Hole | pi-hole | 192.168.0.2 | DHCP reservation |
| Aurora | aurora | 192.168.0.10 | DHCP reservation |
| LUNA | luna | 192.168.0.11 | DHCP reservation |
| Nova (MacBook) | nova | 192.168.0.12 | DHCP reservation |
| NAS (future) | nas | TBD | DHCP reservation |

All LAN IPs are set via DHCP reservation on the router so they're stable without needing static IP configuration on each machine.

## RustDesk Remote Access

| Machine | RustDesk ID | Notes |
|---|---|---|
| Aurora | 468 516 030 | Windows |
| LUNA | 470 061 280 | Windows 11 host |

**Password format:** `*<FirstLove><CO>*`

## Machine Roles

| Device | Role |
|---|---|
| Gateway | Internet routing, DHCP, firewall |
| Pi-Hole | DNS resolution + ad blocking for all LAN devices |
| Aurora | Storage workstation — large disk array; media, big files |
| LUNA | Primary Forge42 server — all Docker services, AI stack |
| Nova | Development machine — primary coding and management |

## DNS

Pi-Hole at `192.168.0.2` is the DNS server for the home network. All LAN devices should point to `192.168.0.2` for DNS to get ad blocking. Pi-Hole forwards to upstream DNS (e.g., 1.1.1.1) for resolution.

## LUNA Service Ports (LAN Access)

Access these from any device on `192.168.0.0/24` or via Tailscale:

| Service | Port | Notes |
|---|---|---|
| SSH | 22 | Key-only auth |
| LiteLLM | 4000 | LLM gateway — API keys, localhost-preferred |
| Open WebUI | 3000 | AI chat interface |
| Qdrant | 6333 | Vector DB |
| AnythingLLM | 3001 | Knowledge base |
| n8n | 5678 | Workflow automation |
| Coolify | 8000 | Container management |
| Portainer | 9443 | Container management (HTTPS) |
| PostgreSQL | 5432 | Internal only by default |
| Redis | 6379 | Internal only |

**Security note:** LiteLLM routes to cloud APIs using your API keys. Only expose port 4000 to devices that need direct API access. PostgreSQL and Redis should stay internal unless a specific machine (Aurora/Nova) needs direct DB access.

## Related

- [[machine-inventory]] — hardware specs for each machine
- [[tailscale-map]] — Tailscale VPN for remote access
- [[aurora]] — Aurora disk inventory
- [[luna-plan]] — LUNA build and upgrade roadmap
