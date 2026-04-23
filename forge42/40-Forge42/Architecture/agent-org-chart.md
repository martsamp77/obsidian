---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-22
---

# Agent Org Chart

The full agent workforce across both suites. These are **workflow roles, not named personas** — any capable AI fills the role as directed. Roles are defined by what they produce and what constraints they operate under.

---

## Business Planning Suite

Orchestrated by **Paperclip Hermes** (external tool). Runs on ChatGPT-backed agents with Claude available for refinement passes. Status: **target model** (being implemented).

| Role | Produces | Inputs | Status |
|---|---|---|---|
| **Strategist** | Vision Brief | Founder intent, market context | ⏳ target |
| **Market Researcher** | Market Brief, persona cards, JTBD statements | Web research, competitor analysis | ⏳ target |
| **Business Analyst** | Business Brief, SRS, functional specs | Vision Brief, Market Brief | ⏳ target |
| **Product Manager** | PRD, backlog, information architecture | Business Brief, Market Brief | ⏳ target |
| **Brand Designer** | Design tokens, component specs, visual language | Brand guidelines, product purpose | ⏳ target |
| **Graphic Designer** | Assets, thumbnails, brand visuals, iconography | Design tokens, brand guidelines | ⏳ target |
| **Head of Software** | System Architecture Brief, ADRs, API spec, data model, NFRs, component specs, test strategy | PRD, SRS, platform constraints | ⏳ target |
| **Technical Writer** | Help docs, admin guide, API reference, release notes, in-product help | Shipped behavior, PRD, API spec | ⏳ target |
| **Compliance Agent** | Security policies, DPA, SOC 2 evidence, SIG/CAIQ responses | Architecture, regulatory requirements | ⏳ target |
| **Security Agent** | Threat model, access control policy, vulnerability assessment | System architecture, data model | ⏳ target |
| **Orchestration Agent** | Routes work, maintains shared context, assembles handoff package | All suite outputs | Paperclip Hermes |

### Checkpoint Approver
All Business Suite outputs at defined checkpoints → **Marty Sampson (Director)**

---

## Development Suite

Primary operator is **Claude Code**. Parallel work runs via sub-agents or multiple terminals. Cost and latency drive model selection below Claude Code. Status: **operational now**.

| Tier | Model | Use Cases | Status |
|---|---|---|---|
| **Primary** | Claude Code | All code generation, implementation, test writing, architecture execution | ✓ operational |
| **Parallel** | Claude Code sub-agents / multiple terminals | Independent component specs executed simultaneously | ✓ operational |
| **Cost-sensitive** | OpenCode + OpenRouter models | Boilerplate, routine refactors, documentation extraction, high-volume simple tasks | ⏳ target |
| **Local / latency** | Ollama models (Forge42 / LUNA) | Latency-sensitive tasks, cost-critical batch work, offline operation | ✓ available via Eddie's stack |

### Dev Suite Inputs
The dev suite consumes:
- **Handoff package** (primary specification — lives in `05-Projects/Active/[name]/handoff/`)
- **CLAUDE.md** (operating context for this vault)
- **ADRs** (constraint library — consult before making architectural decisions)
- **Model routing policy** (what to route to cheaper/local tiers)

### Dev Suite Outputs
- Running, tested, deployed software
- Commits referencing backlog items
- PRs linking back to spec sections
- Evaluation reports at each shippable increment
- New ADRs for implementation-phase decisions

### Escalation Path
When the dev suite hits a decision not covered by the handoff package → route to orchestrator → Marty reviews if not auto-resolvable.

---

## Human Director

**Marty Sampson** — the one human in the system.

| Responsibility | When |
|---|---|
| Seed Vision Brief | Start of each product cycle |
| Approve at 7 checkpoints | See [[operating-model]] for full checkpoint schedule |
| Evaluate shippable increments | Before each release |
| Review legal/security docs | Before publication (hard line — always human) |
| Conduct post-mortem narrative | After each incident |
| Review quarterly retrospectives | Every quarter |

---

## What Is NOT In This Model

- **Eddie** — OpenClaw personal orchestration agent on LUNA. Separate system, personal use only. His Ollama stack is available as the local model tier of the dev suite, but Eddie as an agent/persona is completely separate.
- Named AI personas for business planning roles — these are workflow roles filled by any capable model.

---

## Related

- [[operating-model]] — the two-suite architecture and full business cycle
- [[handoff-package-spec]] — what the Business Suite produces for the Dev Suite
- [[model-routing-policy]] — model selection policy for the Dev Suite
