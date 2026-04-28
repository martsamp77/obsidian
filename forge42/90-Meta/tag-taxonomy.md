---
tags: [type/reference, status/active]
updated: 2026-04-19
---

# Tag Taxonomy

The vault uses a structured tag schema to enable filtered views in Obsidian Bases and the Tasks plugin. All frontmatter properties follow this schema.

## Frontmatter Schema

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

## Tag Groups

### Status Tags

| Tag | When to Use |
|---|---|
| `#status/idea` | Concept captured, not yet committed |
| `#status/active` | Actively being worked on |
| `#status/paused` | Deprioritized temporarily |
| `#status/complete` | Finished |
| `#status/abandoned` | Stopped and archived |

### Brand Tags

| Tag | Context |
|---|---|
| `#brand/d42` | Dimension42 — tech brand, products, YouTube |
| `#brand/marty` | Marty Sampson personal brand |
| `#brand/37m` | 37Metrics LLC — admin, legal, finance |
| `#brand/forge42` | Internal infrastructure (machines, stack, runbooks) |
| `#brand/personal` | Personal life (health, family, interests) |

### Type Tags

| Tag | When to Use |
|---|---|
| `#type/software` | Software projects and products |
| `#type/content` | YouTube, Substack, social media content |
| `#type/infrastructure` | Machines, Docker stack, networking |
| `#type/business` | Strategy, finance, legal, admin |
| `#type/reference` | Permanent reference pages (most wiki pages) |
| `#type/runbook` | Operational procedures |
| `#type/source` | Raw ingested documents — do not modify |
| `#type/summary` | Synthesized knowledge pages |
| `#type/decision` | ADRs and key decision logs |
| `#type/meeting` | Meeting notes |
| `#type/draft` | Draft content (emails, copy, scripts) |

### Priority Tags

| Tag | When to Use |
|---|---|
| `#priority/critical` | Blocking — must do now |
| `#priority/high` | Important — do this week |
| `#priority/medium` | Should do — do this month |
| `#priority/low` | Nice to have |

### Phase Tags (Projects Only)

| Tag | Phase |
|---|---|
| `#phase/discovery` | Exploring the problem and solution space |
| `#phase/design` | PRD, architecture, system design |
| `#phase/build` | Active development |
| `#phase/launch` | Launch prep, testing, release |
| `#phase/live` | Shipped and in production |

## Obsidian Bases Query Examples

These queries work in Obsidian Bases (or Dataview if you prefer).

### All active D42 software projects
```
WHERE brand = "d42" AND status = "active" AND type = "software"
```

### All ideas by brand
```
WHERE status = "idea"
GROUP BY brand
```

### All content in scripted or production stage
```
WHERE type = "content" AND status = "active"
```

### All high-priority items across all projects
```
WHERE priority = "critical" OR priority = "high"
SORT BY brand ASC
```

### All runbooks in Forge42
```
WHERE brand = "forge42" AND type = "runbook"
```

## Tasks Plugin Queries

The **Tasks** community plugin queries inline task checkboxes (`- [ ]`) across the vault.

```
# All incomplete tasks tagged with a project
not done
path includes 05-Projects/Active

# All tasks due today
not done
due today

# All tasks in health files
not done
path includes 50-Personal
```

## Related

- [[folder-conventions]] — where files go
- [[vault-readme]] — vault orientation guide
- `CLAUDE.md` — full file conventions schema
