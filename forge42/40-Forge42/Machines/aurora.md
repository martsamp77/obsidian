---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-19
---

# Aurora

Desktop workstation with a large multi-drive storage array. Current role: personal workstation and interim storage. Future role: may serve as supplementary backup target until NAS arrives.

## Identity

| Attribute | Value |
|---|---|
| Hostname | aurora |
| Local IP | 192.168.0.10 |
| RustDesk ID | 468 516 030 |
| OS | Windows |

## Disk Inventory

| Drive | Capacity | Type | Read | Write | Notes |
|---|---|---|---|---|---|
| C: | 2TB | NVMe | 3,182 MB/s | 2,302 MB/s | Primary OS + apps |
| D: | 512GB | NVMe | 2,671 MB/s | 1,819 MB/s | Secondary fast storage |
| E: | 4TB | SSD | 514 MB/s | 440 MB/s | Large files, media |
| F: | 4TB | HDD | 107 MB/s | 75 MB/s | Archival storage |
| G: | 16TB | SSD | 276 MB/s | 251 MB/s | Mass storage |

**Total capacity:** ~26.5TB across 5 drives

## Role in the Network

Aurora is a high-capacity workstation primarily used for large file work and media. The 16TB G: drive makes it a viable interim backup target for LUNA Docker volumes before the Synology NAS is purchased.

**Interim backup path (until NAS):**
- Docker volumes can be backed up to Aurora via SMB share or rsync over the LAN
- This is manual/ad-hoc — the proper backup solution is the NAS + docker-volume-backup automation

See [[backup-recovery]] for the full backup strategy.

## Remote Access

Access Aurora remotely via:
1. **RustDesk** (ID: 468 516 030) — remote desktop GUI
2. **Tailscale** → LAN IP `192.168.0.10` — once enrolled in Tailscale

## Related

- [[machine-inventory]] — all machines in the network
- [[home-network]] — home network topology
- [[backup-recovery]] — backup strategy
