---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-19
---

# Infrastructure Naming Conventions

Tier 3 naming: machines, services, and containers. Functional over personality.

## Machines

**Use functional role names, not personality names.** No LUNA, AURORA, Nova — those names lock documentation to specific hardware and create confusion when hardware changes. Reference machines by role, not name.

| Role | Naming Pattern | Example |
|---|---|---|
| Primary orchestration server | `hub` or `core` | `hub` |
| GPU compute nodes | `gpu-[N]` | `gpu-1`, `gpu-2` |
| Development machine | `dev` | `dev` |
| NAS / storage | `nas-[N]` | `nas-1` |

**If you want some personality without lock-in:** Use short physics particle names as a themed set — Muon, Gluon, Lepton, Quark, Boson. Short, distinctive, SSH-friendly. But the rule stands: **documentation always references role, never machine name.** Write "the orchestration server" not "LUNA."

## Docker Services and Containers

Name by function. Already doing this correctly — keep it.

| Service | Naming |
|---|---|
| LLM proxy | `litellm` |
| Local inference | `ollama` |
| Cache | `redis` |
| Database | `postgres` |
| Orchestration | `openclaw` |

No branding on infrastructure containers. They're plumbing.

## Tailscale / Network

- Machines registered in Tailscale should use the functional role name
- Avoid hostnames that reference physical location (e.g., `office-pc`) — locations change
- Format: `[role]-[optional-number]` (e.g., `hub`, `gpu-1`, `dev`)

## Related

- [[naming-strategy]] — Tier 1 (public) and Tier 2 (community) naming
- [[architecture]] — overall system architecture
- [[tailscale-map]] — current network topology
