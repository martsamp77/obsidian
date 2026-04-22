---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-19
---

# GitHub Organization Strategy

Three GitHub identities, each with a distinct purpose.

## martsamp77 (personal, existing)

- Personal repos, experiments, IronMarty health project, dotfiles
- Mix of public and private
- Anything not belonging to D42 or 37Metrics

## 37metrics (org, existing)

- Private repos for LLC operations, contract templates, internal tools
- Mostly private — legal entity materials
- What lives here: billing templates, internal ops scripts, anything belonging to the legal entity

## dimension42ai (org, create: github.com/dimension42ai)

- Public and private repos for all D42 projects
- What lives here:
  - OpenClaw (if/when open-sourced)
  - Eddie's SOUL.md and agent configs (public — community facing)
  - Software products (private)
  - Tutorial code from YouTube videos (public)
  - dimension42.ai website (private)
  - BAAS platform (private)

## Repo Naming Convention

- Lowercase kebab-case: `openclaw`, `forge42-dashboard`, `d42-website`
- Prefix `d42-` for Dimension42 repos that aren't standalone products
- Standalone products: clean name only (`openclaw`, `[productname]`)
- Tutorial repos from YouTube: `d42-tutorial-[topic]`

## Related

- [[naming-strategy]] — broader naming rules (Tier 1, 2, 3)
- [[37metrics-empire]] — full brand architecture
