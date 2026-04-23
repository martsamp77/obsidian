# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Vault Is

This Obsidian vault is Marty Sampson's personal operating system and second brain. It is the **single system** for knowledge capture, strategic planning, project tracking, task management, and reference. There is no external task manager — everything lives here.

Claude Code is the LLM that maintains and evolves this vault. Obsidian is the IDE. The vault is the codebase. You write and maintain the wiki; Marty curates sources and directs the work.

## The LLM Wiki Model

This vault follows the **LLM Wiki pattern**: instead of re-deriving answers from raw documents on every query, Claude incrementally builds and maintains a persistent wiki — structured, interlinked markdown files that sit between Marty and his raw sources. The wiki compounds over time. Cross-references are already there. Contradictions have already been flagged. Every new source and every good answer makes the wiki richer.

**Three layers:**
- **Raw sources** (`60-Knowledge/Clips/` and `60-Knowledge/Sources/`) — immutable captured documents. Never modify these.
- **The wiki** — the rest of the vault. Claude owns the maintenance of this layer.
- **This schema** (CLAUDE.md) — conventions, workflows, and protocols that make Claude a disciplined wiki maintainer, not a generic chatbot.

## The Three-Brand Model

```
37Metrics, LLC (legal holding entity)
├── Dimension42 (d/b/a 37Metrics) — tech brand, AI products, software, YouTube content
├── Marty Sampson — personal brand, consulting, speaking, zero-human narrative
└── [Forge42] — internal infrastructure layer (AI stack, machines, FastMail identity)
```

- **37Metrics** appears on invoices, contracts, legal pages, tax filings. Not a content brand.
- **Dimension42** is the customer-facing tech and product brand. Revenue lives here.
- **Marty Sampson** is the personal brand and storyteller. The "zero human company" narrative lives here.
- **Forge42** is internal identity — FastMail, infrastructure dashboard, not customer-facing.

## Folder Structure

```
00-Inbox/       Quick capture, unsorted — process weekly
01-Daily/       Daily notes (YYYY-MM-DD.md)
02-Templates/   Note templates (Templater syntax)
05-Projects/    Software project pipeline — Ideas → Active → Archive
10-Dimension42/ D42 brand: products, content, YouTube, website
20-MartySampson/Personal brand: consulting, speaking, website, content
30-37Metrics/   Holding company: finance, legal, admin, strategy
40-Forge42/     Infrastructure: AI stack, machines, runbooks, architecture
50-Personal/    Health (IronMarty), family, interests, vehicles, worldview
60-Knowledge/   Processed knowledge + raw sources (see below)
  ├── AI/       Synthesized AI knowledge pages
  ├── Business/ Business and strategy knowledge
  ├── Tech/     Technology reference pages
  ├── Physics/  Physics topics
  ├── Clips/    Raw web clips (Obsidian Web Clipper) — DO NOT MODIFY
  └── Sources/  Raw documents (PDFs, articles, papers) — DO NOT MODIFY
70-Archive/     Completed projects and retired notes
90-Meta/        Vault conventions, guides, changelogs, index, log
```

## Where Does This Go?

| What you're creating | Where it goes |
|---|---|
| New software product idea | `05-Projects/Ideas/[name].md` |
| Active software project | `05-Projects/Active/[name]/index.md` |
| Raw web clip (just captured) | `60-Knowledge/Clips/` — leave as-is |
| Raw source document | `60-Knowledge/Sources/` — leave as-is |
| Synthesized knowledge page (processed from source) | `60-Knowledge/[AI\|Business\|Tech\|Physics]/` |
| D42 brand, product, content, YouTube | `10-Dimension42/` |
| Personal brand, consulting, speaking | `20-MartySampson/` |
| Legal, finance, contracts, admin | `30-37Metrics/` |
| Infrastructure, AI stack, runbooks | `40-Forge42/` |
| Health, family, personal life | `50-Personal/` |
| Quick capture, unsorted thoughts | `00-Inbox/` |

## LLM Wiki Operations

### Ingest Protocol

When Marty drops a new source and asks to process it:

1. **Read** the source document fully
2. **Discuss** key takeaways with Marty if in conversation — let him redirect emphasis
3. **Summarize** — write a summary page in the appropriate `60-Knowledge/` subfolder (or relevant domain folder if it's D42/health/project-specific content)
4. **Update** existing wiki pages that the new information touches — entity pages, strategy docs, project notes. A single source may touch 5–15 pages.
5. **Update** `90-Meta/index.md` — add new page entries with one-line summaries
6. **Log** — append an entry to `90-Meta/log.md` with date, source title, and list of pages touched

Do not batch-ingest unless asked. Default is one source at a time with Marty's involvement.

### Query Protocol

When answering a research or synthesis question:

1. **Check** `90-Meta/index.md` first to find relevant pages
2. **Read** the relevant pages
3. **Synthesize** an answer
4. **File the answer** if it is non-obvious and would be valuable to find again — create a new page in the appropriate folder, add it to the index
5. **Log** the query if it produced a filed page

Good answers become permanent wiki pages. They don't disappear into chat history.

### Lint Protocol

When asked to health-check the wiki (or proactively when appropriate):

- Flag **contradictions** between pages — note both locations
- Identify **orphan pages** — wiki pages with no inbound links from other pages
- Find **missing pages** — concepts or entities mentioned frequently but lacking their own page
- Note **stale content** — claims in older pages that newer sources have superseded
- Suggest **new sources** to investigate based on gaps in the knowledge
- Suggest **new questions** Marty should explore given the current state of the wiki

### Index and Log Maintenance

**`90-Meta/index.md`** — update on every ingest and every time a significant new page is created. Format:

```
| [[page-name\|Page Title]] | Category | One-line summary | YYYY-MM-DD |
```

Categories: `Projects`, `Brand`, `Infrastructure`, `Health`, `Knowledge`, `Strategy`, `Reference`

**`90-Meta/log.md`** — append-only. Never edit previous entries. Format:

```
## [YYYY-MM-DD] action-type | Description

- What was done
- Pages created or updated
```

Action types: `ingest`, `query`, `lint`, `setup`, `update`

## Software Project Pipeline

All software projects move through three stages. The master view is `05-Projects/_pipeline.md`.

**Stage 1 — Idea** (`05-Projects/Ideas/[name].md`)
Lightweight capture of the concept, brand, and rough priority. Use template `02-Templates/project-idea.md`. No tasks, no commitments.

**Stage 2 — Active** (`05-Projects/Active/[name]/`)
When committed to building, promote the idea to a project folder with four files:
- `index.md` — brief, goals, status, phase (use `02-Templates/project-index.md`)
- `tasks.md` — current checklists (use `02-Templates/project-tasks.md`)
- `decisions.md` — key decisions log
- `notes.md` — running notes and research links

**Stage 3 — Archive** (`70-Archive/YYYY-Q[N]/[name]/`)
Move the completed or abandoned project folder here.

## Task Management

All tasks are Markdown checklists (`- [ ]` / `- [x]`). No external task manager.

- **Day-level tasks** → `01-Daily/YYYY-MM-DD.md`
- **Project tasks** → `05-Projects/Active/[name]/tasks.md`
- **Embedded tasks** → inline in any relevant note
- The **Tasks** community plugin queries tasks across all notes by tag, file, or path

## File Conventions

### Frontmatter

```yaml
---
date: YYYY-MM-DD
status: idea | active | paused | complete | abandoned
brand: d42 | marty | 37m | forge42 | personal
type: software | content | infrastructure | business | reference | runbook | source | summary
priority: critical | high | medium | low
phase: discovery | design | build | launch | live   # projects only
tags: [brand/d42, status/active, type/software]
---
```

### Tag System

- **Status:** `#status/idea` `#status/active` `#status/paused` `#status/complete` `#status/abandoned`
- **Brand:** `#brand/d42` `#brand/marty` `#brand/37m` `#brand/forge42` `#brand/personal`
- **Type:** `#type/software` `#type/content` `#type/runbook` `#type/reference` `#type/decision` `#type/meeting` `#type/draft` `#type/source` `#type/summary`
- **Priority:** `#priority/critical` `#priority/high` `#priority/medium` `#priority/low`

Obsidian Bases uses frontmatter properties to generate filtered pipeline views.

### Wikilinks

Internal links use `[[Note Name]]` or `[[Note Name|Display Text]]`. Always link to related pages when they exist — cross-references are the value of the wiki.

## Development Suite Context

Claude Code is the primary Development Suite operator for all Dimension42 software products. When working on a software project, follow this protocol:

1. **Check the handoff package first.** Before writing any code, read `05-Projects/Active/[name]/handoff/_index.md`. The handoff package is the primary specification — it defines what to build, not just how.
2. **Consult ADRs before architectural decisions.** ADRs live in `handoff/adr/`. Accepted ADRs are binding constraints. Do not re-litigate settled decisions. Create a new ADR if a genuinely new decision arises during implementation, and flag it for Director review.
3. **Follow the model routing policy.** The handoff package includes a model routing policy specifying which tasks should route to cheaper or local model tiers. Default: Claude Code handles everything; route down only for boilerplate and high-volume mechanical work.
4. **Surface questions, don't guess.** If a decision isn't covered by the handoff package, surface it to Marty rather than resolving it by assumption. The handoff package should be complete — gaps mean something needs to be answered at the business level first.
5. **Checkpoint awareness.** Know which checkpoint the project is at. Between Checkpoints 4 and 5, the dev suite runs autonomously. Don't wait for Marty on decisions that are covered by the handoff — do wait (and ask) on decisions that aren't.
6. **Commit discipline.** Every commit should reference its backlog item. Every PR should link to the relevant spec section in the handoff package.

See [[operating-model]] for the full two-suite architecture and [[handoff-package-spec]] for what's in a handoff package.

## IronMarty Health System

Marty's health tracking system lives in `50-Personal/Health/`:

| File | Purpose |
|---|---|
| `marty-health-context.md` | **Source of truth** — conditions, meds, supplements, labs, diet, exercise |
| `dr-paul-persona.md` | Dr. Paul AI persona definition and check-in protocol |
| `dr-paul-current-health-plan.md` | Living health plan, updated weekly |
| `dr-paul-check-in-history.md` | Running SOAP-format check-in log |
| `dr-paul-weekly-plan.md` | Printable weekly plan |

Weekly check-ins happen on Sundays. Always read `marty-health-context.md` before any health-related work.

## Git Notes

The `.obsidian/workspace-*.json` files change on every vault open — avoid committing them. Commit content files and configuration, not UI state.

## Key Reference Files

| File | Purpose |
|---|---|
| `30-37Metrics/Admin/37metrics-empire.md` | Master brand architecture and empire strategy |
| `05-Projects/_pipeline.md` | All software ideas and active projects |
| `50-Personal/Health/marty-health-context.md` | Health context source of truth |
| `90-Meta/index.md` | Wiki index — catalog of all significant pages |
| `90-Meta/log.md` | Chronological log of all LLM wiki operations |
| `90-Meta/Obsidian Master Guide 2026.md` | Vault setup, sync strategy, plugin guide |
