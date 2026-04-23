---
tags: [type/reference, status/active]
updated: 2026-04-19
---

# Vault README

Orientation guide for this Obsidian vault.

## What This Vault Is

This is Marty Sampson's **personal operating system and second brain** — the single system for knowledge capture, strategic planning, project tracking, task management, and reference. There is no external task manager. Everything lives here.

The vault covers three domains:
1. **Business operations** — 37Metrics LLC, Dimension42, Marty Sampson brand
2. **Infrastructure** — Forge42 AI stack, machines, runbooks
3. **Personal** — health (IronMarty), personal interests

## Who Maintains It

**Marty Sampson** — curates sources, directs strategy, fills in personal data, approves major structural changes.

**Claude Code** — maintains the wiki layer. Writes and updates reference pages, processes ingested sources, fills in derivable content, keeps the index and log current.

## How to Navigate

**Start here:** `90-Meta/index.md` — catalog of all significant wiki pages by category.

**Find operational context:** `CLAUDE.md` — full protocols for the LLM wiki pattern (Ingest, Query, Lint operations).

**Find what's being built:** `05-Projects/_pipeline.md` — master view of all software project ideas and active projects.

**Find the brand strategy:** `30-37Metrics/Admin/37metrics-empire.md` — corporate hierarchy and brand architecture index.

## The LLM Wiki Pattern

Instead of re-deriving answers from raw documents on every query, Claude incrementally builds and maintains a persistent wiki — structured, interlinked markdown files. The wiki compounds over time.

**Three layers:**
- **Raw sources** (`60-Knowledge/Clips/`, `60-Knowledge/Sources/`) — immutable captured documents. Never modify.
- **The wiki** — all other vault content. Claude maintains this layer.
- **This schema** (`CLAUDE.md`) — conventions and protocols.

**Three operations:**
- **Ingest** — process a new source document into wiki pages
- **Query** — answer a research question, file the answer as a new page
- **Lint** — health-check the wiki for contradictions, orphans, stale content

## Folder Layout

```
00-Inbox/       Quick capture, unsorted
01-Daily/       Daily notes (YYYY-MM-DD.md)
02-Templates/   Note templates (Templater syntax)
05-Projects/    Software project pipeline
10-Dimension42/ Tech brand: products, content, YouTube, website
20-MartySampson/Personal brand: consulting, speaking, content, website
30-37Metrics/   Holding company: admin, finance, legal, strategy
40-Forge42/     Infrastructure: AI stack, machines, runbooks
50-Personal/    Health, family, personal interests
60-Knowledge/   Wiki pages + raw sources
70-Archive/     Completed and retired notes
90-Meta/        Vault conventions, index, log (you are here)
```

See [[folder-conventions]] for the full placement guide.

## Tag Schema

All notes use structured frontmatter (status, brand, type, priority, phase) enabling filtered Obsidian Bases views and Tasks plugin queries.

See [[tag-taxonomy]] for the full schema and query examples.

## Git Notes

The vault is in a Git repository. `.obsidian/workspace-*.json` files (UI state) change on every open — avoid committing them. Commit content files and configuration, not UI state.

## Related

- [[folder-conventions]] — detailed folder placement guide
- [[tag-taxonomy]] — tag schema reference
- `90-Meta/index.md` — wiki page catalog
- `90-Meta/log.md` — operation history
- `CLAUDE.md` — full protocols and conventions
