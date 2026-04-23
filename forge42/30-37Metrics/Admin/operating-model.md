---
tags: [brand/37m, type/reference, status/active]
updated: 2026-04-22
---

# Operating Model: Two Suites, One Director

How 37Metrics builds and ships Dimension42 products. The operational implementation of the [[mission]].

## The Two-Suite Architecture

The Zero Human Company model runs on two deliberately separated tool suites, connected by a structured handoff package.

### Business Planning Suite (37Metrics layer)

**Purpose:** Everything from market research to product requirements to high-level architectural planning. Produces the documentation that defines what to build.

**Orchestration:** Paperclip Hermes routes work between specialist agents and maintains shared context across sessions.

**Current state:** Target model — being implemented as business operations mature. When operational, this suite runs before every product build cycle.

**Output:** The handoff package — a version-locked set of documents that specifies everything the dev suite needs to build without further input from the business side.

### Development Suite (Forge42 / Dimension42 layer)

**Purpose:** Building and shipping the products defined by the handoff package.

**Primary operator:** Claude Code

**Current state:** Operational now. Claude Code ingests the handoff package (or project spec) and executes against it.

**Output:** Running, tested, deployed software that ships under the Dimension42 brand.

### Why They're Separated

1. **Tooling fit.** Tools optimized for product thinking and business analysis are different from tools optimized for writing and shipping code.
2. **Constraint management.** Claude Code operates within specific Anthropic-defined constraints. Business planning happens outside those constraints so the coding agent stays well within them.
3. **Discipline.** The handoff forces the business side to finish its thinking before the dev side starts spending tokens on implementation.

---

## The Handoff Package

The handoff package is the single most important artifact in this model. It is:
- **Complete** — the dev suite should not need to ask the business suite for clarification mid-build
- **Version-controlled** — every change logged with an explicit diff
- **Binding** — dev suite executes against it; changes must route back through the orchestrator

When the director signs off on the handoff package (Checkpoint 4), control transfers to the development suite. Implementation begins.

See [[handoff-package-spec]] for the full contents and the spec for each document.

---

## Marty's Role: The Director

Marty Sampson is the one human in the system. Director, not executor. His time is the scarcest resource in the operation.

### The 7 Standard Checkpoints

| # | Checkpoint | Trigger |
|---|---|---|
| 1 | Approve Vision Brief + Business Brief | Before product definition begins |
| 2 | Approve PRD | Before design and architecture begin |
| 3 | Approve System Architecture Brief + ADRs | Before implementation begins |
| 4 | Approve full handoff package | Before dev suite starts work |
| 5 | Evaluate each shippable increment | Before each release is cut |
| 6 | Approve security/legal documents | Before publication |
| 7 | Review post-mortems + quarterly retrospectives | After incidents, every quarter |

**Between checkpoints:** Everything runs autonomously. Agents surface questions through the orchestrator rather than guessing. If a decision isn't covered by the handoff package, it routes to Marty — it does not get resolved by guesswork.

**Hard lines — always human:**
- Legal document review and approval (DPA, ToS, Privacy Policy)
- Security document sign-off (threat model, access control)
- Post-mortem narrative and analysis

---

## Company Roles in the Model

| Company | Role |
|---|---|
| **37Metrics** | Business Planning Suite context: strategy, market analysis, product definition, contracts |
| **Dimension42** | Products ship here — the Development Suite's deliverable |
| **Forge42** | Infrastructure running the Dev Suite: Claude Code, Ollama (local models via LUNA), OpenCode |
| **Marty Sampson** | The director: vision seeds, checkpoint approvals, increment evaluations |

---

## The Full Business Cycle

1. **Director** seeds a Vision Brief
2. **Business Suite** produces: Market Brief → Business Brief → PRD → System Architecture → Component Specs → ADRs → API Spec → Data Model → NFRs → Test Strategy
3. **Director** reviews and approves at Checkpoints 1–3
4. **Orchestrator** (Paperclip Hermes) assembles the full handoff package, version-locks it
5. **Director** signs off on the handoff package (Checkpoint 4)
6. **Dev Suite** (Claude Code) ingests the package and begins implementation
7. **Sub-agents or parallel terminals** pick up component specs and backlog items
8. **Director** evaluates each shippable increment (Checkpoint 5) — approves release or returns items
9. **Post-launch:** technical writer updates docs, compliance agent maintains evidence, orchestration agent synthesizes retrospective
10. **Director** reviews retrospective (Checkpoint 7) — cycle repeats with next feature

---

## Related

- [[agent-org-chart]] — full agent workforce in both suites
- [[handoff-package-spec]] — what a complete handoff package contains
- [[model-routing-policy]] — which model handles which task type
- [[37metrics-empire]] — brand architecture
- [[mission]] — why this model exists
- [[zero-human-narrative]] — the human story behind the model
- Source: [[60-Knowledge/Sources/agentic-saas-model|Agentic SaaS Model (source)]]
