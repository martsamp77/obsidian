---
tags: [brand/d42, type/reference, status/active]
updated: 2026-04-19
---

# Naming Strategy

Three tiers map to visibility: public products, community projects, internal infrastructure.

## Tier 1: Public / Customer-Facing

Conventional, marketable names. These are software products, services, and content channels that need to appeal to people who don't care about infrastructure. Easy to say, easy to spell, easy to Google.

**Rules:**
- 1–2 syllables preferred
- Don't force the metal/physics theme — prioritize clarity
- Physics concepts work loosely: Flux, Pulse, Signal, Lattice — but clarity over cleverness
- Products can live under the Dimension42 umbrella without having an obscure name

**Before naming any public product:** Check availability across GitHub, npm, YouTube, X, and domain registration. If you can't lock the name on at least GitHub and X, pick a different name.

## Tier 2: Community / Builder-Facing (The Forge Namespace)

Metal and physics-themed names for projects, tools, and repos that the technical audience (YouTube viewers, GitHub followers, builders) will see and interact with.

**The Forge namespace** is the established community identity:
- `forge42` is the FastMail identity
- "Forge" (a place where things are built, shaped, hardened) is more defensible than "Iron" (too generic: IronMQ, IronWorker, IronPython, etc.)

**Current Tier 2 name decisions:**

| Old / Draft Name | Final Name | What It Is |
|---|---|---|
| IronForge | Forge42 Dashboard | Infrastructure dashboard on the home server |
| IronClaw | Eddie (keep as-is) | The OpenClaw orchestration agent persona |
| IronMarty | IronMarty (keep) | Personal health tracking — "iron" fits fitness context |
| OpenClaw | OpenClaw (keep) | The orchestration platform (community/open source) |
| Eddie/Ed | Eddie (keep) | AI agent persona — no prefix needed |

**Why IronMarty stays:** It's personal, not a D42 product. "Iron" fits the health/fitness context literally (pumping iron, iron will). It's a standalone brand within the Marty Sampson personal ecosystem.

## GitHub Repo Naming

- Lowercase kebab-case: `openclaw`, `forge42-dashboard`, `d42-website`
- Prefix with `d42-` for Dimension42 repos that aren't standalone products
- Standalone products: clean name only (`openclaw`, `[productname]`)
- Tutorial repos: `d42-tutorial-[topic]`

## Related

- [[naming-conventions]] — Tier 3 infrastructure naming (machines, services, containers)
- [[handle-registry]] — Social platform handles and status
- [[37metrics-empire]] — Full naming quick reference
