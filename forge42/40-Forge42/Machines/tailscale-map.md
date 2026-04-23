---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-19
---

# Tailscale Network Map

Tailscale creates a private, encrypted WireGuard-based mesh network (a "tailnet") across all enrolled devices. This is the primary access method for all Forge42 services — nothing is exposed to the public internet.

## Why Tailscale First

Tailscale is installed before any Docker service. The architecture principle: all service access goes through Tailscale, not through the public internet. UFW on LUNA allows only SSH and Tailscale — all other ports are blocked.

## Enrolled Devices

| Device | Role | LAN IP | Tailscale IP | Notes |
|---|---|---|---|---|
| LUNA | Primary server | 192.168.0.11 | PLACEHOLDER | Subnet router (exposes home LAN) |
| Nova | MacBook (dev machine) | 192.168.0.12 | PLACEHOLDER | Primary work device |
| Aurora | Workstation / storage | 192.168.0.10 | PLACEHOLDER | |
| [Phone] | Mobile access | — | PLACEHOLDER | iOS/Android Tailscale app |

*Tailscale IPs are assigned by Tailscale in the `100.x.x.x` range. Update this table after checking `tailscale status` on LUNA.*

## Subnet Router

LUNA is configured as a Tailscale **subnet router** for the `192.168.0.0/24` home network. This means any enrolled Tailscale device can reach all home network devices (including Pi-Hole at `192.168.0.2`) via their LAN IPs, even when remote.

**Setup on LUNA:**
```bash
# Advertise the home subnet
sudo tailscale up --advertise-routes=192.168.0.0/24

# Approve in the Tailscale admin console: Machines → LUNA → Edit route settings
```

## Service Access via Tailscale

All Docker services on LUNA are accessible at `<LUNA-tailscale-ip>:<port>` from any enrolled device. Example:

| Service | Access URL |
|---|---|
| LiteLLM | `http://<luna-ts-ip>:4000` |
| Open WebUI | `http://<luna-ts-ip>:3000` |
| AnythingLLM | `http://<luna-ts-ip>:3001` |
| Qdrant | `http://<luna-ts-ip>:6333` |
| n8n | `http://<luna-ts-ip>:5678` |
| Coolify | `http://<luna-ts-ip>:8000` |

Once Traefik + a custom domain are configured, port access is replaced by subdomains (e.g., `https://litellm.forge42.internal`).

## DNS via Tailscale

Pi-Hole at `192.168.0.2` can be set as the DNS server for the Tailscale network via the Tailscale admin console (DNS settings). This gives all Tailscale devices ad blocking + local DNS resolution automatically.

## MagicDNS

Tailscale's MagicDNS feature assigns `<machine-name>.tail<id>.ts.net` hostnames to all enrolled devices. Enable in the Tailscale admin console for hostname-based access instead of IP addresses.

## Useful Commands

```bash
# Check Tailscale status on LUNA
tailscale status

# Get LUNA's Tailscale IP
tailscale ip -4

# See all connected devices
tailscale status --peers

# Check current advertised routes
tailscale status | grep -A5 "Routes"
```

## Related

- [[machine-inventory]] — all machines and LAN IPs
- [[home-network]] — home network topology
- [[architecture]] — overall system architecture
