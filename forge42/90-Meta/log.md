# Wiki Log

Append-only chronological record of LLM wiki operations. Never edit previous entries.

**Entry format:** `## [YYYY-MM-DD] action-type | Description`
**Action types:** `ingest` | `query` | `lint` | `setup` | `update`

**Quick grep for last 5 entries:**
```
grep "^## \[" 90-Meta/log.md | tail -5
```

---

## [2026-04-22] ingest | Agentic SaaS Model — Zero Human Company operating methodology

Source: "The Agentic SaaS Model: The Zero Human Company Approach to Product Development"

Pages created:
- `60-Knowledge/Sources/agentic-saas-model.md` — raw source (immutable)
- `30-37Metrics/Admin/operating-model.md` — two-suite architecture adapted to 37Metrics/D42 context
- `40-Forge42/Architecture/agent-org-chart.md` — full agent workforce (Business Suite roles + Dev Suite tiers)
- `40-Forge42/Architecture/handoff-package-spec.md` — complete handoff package spec with artifact ownership
- `40-Forge42/Architecture/model-routing-policy.md` — default model routing policy for the dev suite
- `02-Templates/project-handoff.md` — handoff package `_index.md` template
- `02-Templates/project-checkpoint-schedule.md` — 7-checkpoint Director schedule template

Pages updated:
- `CLAUDE.md` — added Development Suite Context section (handoff package protocol, ADR discipline, checkpoint awareness)
- `20-MartySampson/zero-human-narrative.md` — added The Director Role section with 7 checkpoints
- `30-37Metrics/Admin/mission.md` — added operating model reference
- `30-37Metrics/Admin/37metrics-empire.md` — added operating model wikilink
- `90-Meta/index.md` — added Operating Model table + new entries

Key decisions:
- Agent roles are workflow roles, not named personas
- Eddie/OpenClaw is separate from this model (personal use only)
- Paperclip Hermes is an existing external tool (Business Suite orchestrator)
- Business Planning Suite is target state; Dev Suite (Claude Code) is operational now

---

## [2026-04-22] update | Created 37Metrics mission document

- Created `30-37Metrics/Admin/mission.md` — formal mission and purpose statement for 37Metrics LLC
- Added wikilink from `37metrics-empire.md` References section
- Added to `90-Meta/index.md`

---

## [2026-04-19] update | Vault audit — filled 30 empty/incomplete files from existing vault knowledge

Full audit of ~159 vault files found ~57 empty stubs and ~53 partial files. Filled all derivable pages using `my-stack.md` (primary infrastructure source), `brand-graphics.md`, `CLAUDE.md`, and existing strategy/brand documents as sources.

Pages created (from empty stubs):

Infrastructure:
- `40-Forge42/Architecture/architecture.md` — Forge42 system architecture with diagram and decision log
- `40-Forge42/Architecture/ai-orchestration.md` — AI stack layers: LiteLLM, OpenRouter, mem0, AnythingLLM
- `40-Forge42/Architecture/luna-plan.md` — LUNA hardware specs + 8-phase implementation checklist
- `40-Forge42/Architecture/ai-org-structure.md` — redirect to agent-org-chart.md (redundant page)
- `40-Forge42/Machines/machine-inventory.md` — all machines with IPs and RustDesk IDs
- `40-Forge42/Machines/tailscale-map.md` — Tailscale VPN network and subnet router setup
- `40-Forge42/Stack/docker-services.md` — complete service inventory organized by tier
- `40-Forge42/Stack/litellm-config.md` — LiteLLM gateway config and routing strategy
- `40-Forge42/Stack/model-catalog.md` — all LLM models available through LiteLLM
- `40-Forge42/Runbooks/backup-recovery.md` — 3-2-1 backup strategy and restore procedures
- `40-Forge42/Runbooks/docker-commands.md` — Docker CLI and Compose reference
- `40-Forge42/Runbooks/incident-response.md` — service outage runbook with severity levels

Pages updated (from partial content):
- `40-Forge42/Machines/home-network.md` — expanded with machine roles, service ports, DNS notes
- `40-Forge42/Machines/aurora.md` — expanded with disk inventory table and role description

Templates filled (from empty stubs):
- `02-Templates/meeting-note.md` — agenda, notes, decisions, action items
- `02-Templates/content-idea.md` — brand, platform, format, hook, status
- `02-Templates/consulting-engagement.md` — 37Metrics LLC invoicing context, session log
- `02-Templates/tool-evaluation.md` — pros/cons scorecard with Forge42 stack fit section
- `02-Templates/weekly-review.md` — per-brand sections + health + next week priorities
- `02-Templates/project-brief.md` — pre-handoff one-pager tied to handoff package workflow

Brand & Content:
- `10-Dimension42/Brand/brand-guidelines.md` — D42 color palette, voice, logo concepts
- `10-Dimension42/Website/sitemap.md` — dimension42.ai page structure
- `10-Dimension42/Content/youtube-ideas.md` — 30+ video ideas across 5 D42 content pillars
- `20-MartySampson/Website/sitemap.md` — martysampson.com page structure
- `20-MartySampson/Content/youtube-ideas.md` — 25+ video ideas across 4 Marty content pillars
- `20-MartySampson/Website/about-page-draft.md` — draft copy for the About page

Admin:
- `30-37Metrics/Admin/domain-inventory.md` — all domains with email config
- `30-37Metrics/Admin/account-registry.md` — platform account registry with status

Meta:
- `90-Meta/folder-conventions.md` — two-digit prefix system and placement routing guide
- `90-Meta/tag-taxonomy.md` — full frontmatter schema and Obsidian Bases query examples
- `90-Meta/vault-readme.md` — vault orientation guide

Pages updated:
- `90-Meta/index.md` — added 20 new entries across Infrastructure, Admin, Content, Brand, Vault Meta tables

Files NOT filled (require Marty's personal data):
- `50-Personal/` health files — medical history, conditions, meds, labs
- Marty personal profile specifics (career history, education)
- Consulting pricing
- Speaker bio
- Personal finances

---

## [2026-04-19] setup | LLM Wiki integration and vault reorganization

- Integrated LLM Wiki pattern into vault (CLAUDE.md updated with ingest/query/lint protocols)
- Created `90-Meta/index.md` and `90-Meta/log.md`
- Created `05-Projects/` folder with full pipeline system (Ideas, Active, Archive)
- Created 6 seed idea files: CRM, Apple TV app, PowerPoint killer, D42 website, Marty website, 37M website
- Created 3 project templates: `project-idea.md`, `project-index.md`, `project-tasks.md`
- Transformed `30-37Metrics/Admin/37metrics-empire.md` from 382-line monolith to ~130-line strategic index
- Distributed empire.md content to dedicated home files:
  - `10-Dimension42/Brand/naming-strategy.md` — Tier 1 & 2 naming conventions
  - `10-Dimension42/Brand/handle-registry.md` — social handle tables with status tracking
  - `20-MartySampson/zero-human-narrative.md` — zero human brand story
  - `30-37Metrics/Finance/revenue-model.md` — revenue stream map
  - `10-Dimension42/Content/youtube-strategy.md` — D42 YouTube strategy
  - `20-MartySampson/Content/youtube-strategy.md` — Marty YouTube strategy
  - `40-Forge42/Architecture/github-org.md` — GitHub organization strategy
  - `40-Forge42/Architecture/naming-conventions.md` — Tier 3 infrastructure naming
  - `40-Forge42/Stack/fastmail.md` — expanded with dimension42.ai setup
- Removed all Monday.com references (vault is now the single system)
