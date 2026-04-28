---
tags: [type/reference, status/active]
updated: 2026-04-19
---

# Folder Conventions

The vault uses a **two-digit prefix** folder system. Every folder has a defined purpose tier. This makes the vault predictable: you always know which area a note belongs in based on its topic.

## Prefix Map

| Range | Tier | Contents |
|---|---|---|
| `00-09` | Transient / operational | Inbox, daily notes, templates — temporary or frequently updated |
| `10-29` | Brands | Customer-facing brand areas: Dimension42, Marty Sampson |
| `30-49` | Operations | Company operations: 37Metrics admin/finance, Forge42 infrastructure |
| `50` | Personal | Private life: health, family, interests, worldview |
| `60` | Knowledge | Processed knowledge and raw source documents |
| `70` | Archive | Completed or retired projects and notes |
| `90` | Meta | Vault conventions, index, log, guides |

## Folder Directory

### `00-Inbox/`
Quick capture — dump anything here and process weekly. Notes here are not organized.

### `01-Daily/`
Daily notes in `YYYY-MM-DD.md` format. Use the `02-Templates/daily-note.md` template.

### `02-Templates/`
Note templates using Templater syntax (`{{date:...}}`). Install the **Templater** community plugin for automatic expansion.

### `05-Projects/`
Software project pipeline. Three stages:
- `Ideas/` — lightweight concept captures
- `Active/[name]/` — active projects with `index.md`, `tasks.md`, `decisions.md`, `notes.md`, and `handoff/` subfolder
- `Archive/` → moved to `70-Archive/YYYY-Q[N]/[name]/` when done

See the [[_pipeline]] master view for current status of all projects.

### `10-Dimension42/`
Everything for the Dimension42 tech brand:
- `Brand/` — brand guidelines, naming, handles, graphics
- `Content/` — YouTube strategy, content ideas, scripts
- `Products/` — product-level documentation
- `Website/` — site planning, sitemap, copy drafts

### `20-MartySampson/`
Personal brand operations:
- `Brand/` — personal brand identity
- `Content/` — YouTube strategy, content ideas
- `Website/` — site planning, sitemap, about page draft

### `30-37Metrics/`
Holding company operations:
- `Admin/` — empire architecture, mission, operating model, domain inventory, account registry
- `Finance/` — revenue model, financial tracking
- `Legal/` — contracts, DPA, privacy policy (empty until needed)

### `40-Forge42/`
Infrastructure layer:
- `Architecture/` — system architecture, AI stack, agent org chart, naming conventions
- `Machines/` — machine inventory, home network, individual machine pages
- `Runbooks/` — operational procedures (backup, incident response, Docker commands)
- `Stack/` — service configurations (LiteLLM, docker-services, model-catalog, FastMail)

### `50-Personal/`
Private content — health, family, personal interests, worldview. The [[IronMarty]] health system lives here.

### `60-Knowledge/`
Two types of content live here:
- `AI/`, `Business/`, `Tech/`, `Physics/` — synthesized knowledge pages (LLM-maintained wiki)
- `Clips/` — raw web clips (Obsidian Web Clipper) — **never modify**
- `Sources/` — raw source documents — **never modify**

### `70-Archive/`
Completed or abandoned projects and retired notes. Organized by `YYYY-Q[N]/` quarter.

### `90-Meta/`
Vault operations:
- `index.md` — catalog of all significant pages (read first on any research query)
- `log.md` — append-only chronological log of all LLM wiki operations
- `folder-conventions.md` — this file
- `tag-taxonomy.md` — full tag schema and Obsidian Bases query patterns
- `vault-readme.md` — vault orientation guide

## File Placement Routing

| What you're creating | Where it goes |
|---|---|
| Software product idea | `05-Projects/Ideas/[name].md` |
| Active software project | `05-Projects/Active/[name]/index.md` |
| Raw web clip (just captured) | `60-Knowledge/Clips/` — leave as-is |
| Raw source document | `60-Knowledge/Sources/` — leave as-is |
| Synthesized knowledge (processed from source) | `60-Knowledge/[AI\|Business\|Tech\|Physics]/` |
| D42 brand, product, content | `10-Dimension42/` |
| Personal brand, consulting, speaking | `20-MartySampson/` |
| Legal, finance, contracts, admin | `30-37Metrics/` |
| Infrastructure, AI stack, runbooks | `40-Forge42/` |
| Health, family, personal life | `50-Personal/` |
| Quick capture, unsorted | `00-Inbox/` |

## Related

- [[tag-taxonomy]] — tag schema and query patterns
- [[vault-readme]] — vault orientation guide
- `CLAUDE.md` — full LLM wiki protocols and file conventions
