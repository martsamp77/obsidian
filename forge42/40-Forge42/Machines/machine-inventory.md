---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-19
---

# Machine Inventory

All machines in the Forge42 network.

## Machine Table

| Hostname | Role | OS | Local IP | RustDesk ID | Status |
|---|---|---|---|---|---|
| LUNA | Primary server (Docker host, AI stack) | Ubuntu 24.04 (WSL2) / Windows 11 | 192.168.0.11 | 470 061 280 | Active |
| Aurora | Storage / desktop workstation | Windows | 192.168.0.10 | 468 516 030 | Active |
| Nova | Development machine (MacBook) | macOS | 192.168.0.12 | — | Active |
| Pi-Hole | DNS / ad blocker | Raspberry Pi OS | 192.168.0.2 | — | Active |
| Router/Gateway | Home network gateway | — | 192.168.0.1 | — | Active |
| NAS | Bulk storage + backup target | Synology DSM | TBD | — | **Planned** |

## Machine Details

### LUNA — Primary Server

The Forge42 compute and AI infrastructure node. All Docker services run here.

- **Hardware:** Minisforum UM790 Pro
- **CPU:** AMD Ryzen 9 7940HS (Zen 4, 8C/16T, up to 5.2GHz)
- **RAM:** 32GB DDR5-5600 (target: 64GB)
- **Storage:** 1TB NVMe PCIe 4.0
- **GPU:** AMD Radeon 780M (integrated, not used for AI inference)
- **Network:** 2.5GbE LAN, Wi-Fi 6E
- **OS:** Windows 11 host with WSL2 Ubuntu 24.04 (Docker runs in WSL2)

See [[luna-plan]] for the full build and upgrade roadmap.

### Aurora — Storage / Workstation

Large-capacity workstation with a multi-drive storage array. Likely used for media storage, large file work, and as an interim backup target until the NAS arrives.

- **Local IP:** 192.168.0.10
- **RustDesk:** 468 516 030
- **OS:** Windows
- **Storage:** See [[aurora]] for full disk inventory

### Nova — MacBook

Primary development machine for coding and management work.

- **Local IP:** 192.168.0.12
- **OS:** macOS

### Pi-Hole — DNS / Ad Blocker

Network-level DNS filtering for the entire home network.

- **Local IP:** 192.168.0.2
- **Purpose:** DNS resolution + ad blocking for all devices on 192.168.0.0/24

### NAS — Planned

A Synology NAS is the highest-priority pending hardware purchase. When acquired, it will serve as:
- Primary Docker volume backup target (via NFS share)
- File sync server (Synology Drive — replaces cloud sync services)
- Cloud Sync → Backblaze B2 for offsite backup

**Recommended models:** DS923+ (4-bay) or DS1522+ (5-bay, stronger CPU)

## Tailscale Network

All machines (LUNA, Nova, and any other enrolled devices) are connected via Tailscale for encrypted remote access. Tailscale IPs are separate from the LAN IPs listed above.

See [[tailscale-map]] for the Tailscale network map.

## Access Methods

| Method | Use Case |
|---|---|
| LAN IP | Local network access (same building) |
| Tailscale IP | Remote access from any enrolled device |
| RustDesk | Remote desktop / GUI access to Windows machines |
| SSH | Terminal access to LUNA/Nova |

## Related

- [[luna-plan]] — LUNA hardware specs and upgrade roadmap
- [[aurora]] — Aurora disk inventory
- [[home-network]] — full home network topology
- [[tailscale-map]] — Tailscale VPN network
