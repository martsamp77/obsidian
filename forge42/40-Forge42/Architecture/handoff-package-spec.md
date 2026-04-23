---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-22
---

# Handoff Package Specification

The handoff package is the interface between the Business Planning Suite and the Development Suite. The Business Suite produces it; the Development Suite consumes it. It is version-controlled, complete, and binding.

Each active project stores its handoff package at:
```
05-Projects/Active/[project-name]/handoff/
```

Use the `[[project-handoff]]` template to create the `handoff/_index.md` landing page.

---

## Required Contents

### Build Brief
One-page summary: what is being built, for whom, and why. The entry point for any dev agent picking up the project cold.

**Produced by:** Head of Software  
**Consumed by:** All dev agents (first thing read on every session)

### PRD — Product Requirements Document
User stories (atomic, explicit), acceptance criteria (measurable), feature scope boundaries. Each story has an explicit boundary so sub-agents know exactly where their responsibility ends.

**Produced by:** Product Manager  
**Consumed by:** Claude Code, test-writing sub-agents, evaluation harness

### System Architecture Brief
Services, data stores, integrations, deployment topology, trust boundaries.

**Produced by:** Head of Software  
**Consumed by:** Claude Code (architecture phase), all dev agents

### Component Specs
One spec per service or module, sized so a single Claude Code sub-agent can own it end to end. The decomposition that makes parallel execution possible.

**Produced by:** Head of Software  
**Consumed by:** Claude Code sub-agents (one per component)

### API Specification (OpenAPI)
The contract between services. Produced during planning, not discovered during implementation. Drives server stub generation, client SDK generation, contract tests, and mock servers.

**Produced by:** Head of Software  
**Consumed by:** Dev Suite (generates stubs, SDKs, tests, mocks)

### Data Model
Entity-relationship diagrams and schema-as-code. Includes tenant isolation strategy for multi-tenant products.

**Produced by:** Head of Software  
**Consumed by:** Claude Code (database implementation), compliance agent

### Non-Functional Requirements (NFRs)
Performance, scalability, availability, security, maintainability, and observability targets — each as a measurable metric that the dev suite can verify automatically.

**Produced by:** Head of Software  
**Consumed by:** Claude Code (verification), test agents

### Test Strategy
Required testing levels (unit, integration, contract, E2E), tools, environments, pass criteria.

**Produced by:** Head of Software  
**Consumed by:** Test-writing sub-agents in Claude Code

### ADRs — Architecture Decision Records
Markdown files in `handoff/adr/`. Standard lifecycle: `proposed` → `accepted` → `superseded`.

- **Planning-phase ADRs** drafted by Head of Software
- **Implementation-phase ADRs** created by Claude Code when new decisions arise
- Dev agents consult all accepted ADRs before making architectural decisions
- New implementation ADRs require Director review before they move to `accepted`

**Produced by:** Head of Software + Claude Code (new decisions during implementation)  
**Consumed by:** All dev agents (constraint library — do not re-litigate accepted decisions)

### Model Routing Policy
Which task types go to which model tier. The cost and capability budget for the dev suite. See [[model-routing-policy]] for the default template.

**Produced by:** Head of Software, approved by Director  
**Consumed by:** Claude Code (routes appropriate tasks to cheaper/local models)

### Checkpoint Schedule
The 7 human review points with target dates and pre-conditions.

**Produced by:** Director (Marty)  
**Consumed by:** Orchestrator (triggers checkpoints), dev agents (know when to surface vs. proceed)

### Backlog
User stories and epics from the PRD in dev-ingestible format. Each item: estimated complexity, dependencies, acceptance criteria.

**Produced by:** Product Manager  
**Consumed by:** Claude Code (picks up items), orchestrator (tracks progress)

### Design Tokens and Component Specs
Color tokens, typography, spacing, component specs, iconography. Code-ready — dev suite implements directly from the spec without Figma dependency.

**Produced by:** Brand Designer + Graphic Designer  
**Consumed by:** Claude Code (frontend implementation)

### Cost Budget
Per-project and per-feature spending caps.

**Produced by:** Director + Head of Software  
**Consumed by:** Orchestrator (fires alerts when approaching cap)

---

## Folder Structure

```
handoff/
├── _index.md                 Landing page + contents status table (use project-handoff template)
├── checkpoint-schedule.md    7 checkpoints with dates and Director approval notes
├── 01-vision-brief.md
├── 02-market-brief.md
├── 03-business-brief.md
├── 04-prd.md
├── 05-system-architecture.md
├── 06-api-spec.md            (or link to repo /doc/api-spec.yaml)
├── 07-data-model.md
├── 08-nfr.md
├── 09-test-strategy.md
├── 10-model-routing-policy.md
├── 11-cost-budget.md
├── 12-backlog.md             (or link to project tasks.md)
├── 13-design-tokens.md       (optional — only if frontend)
└── adr/
    └── 0001-[decision].md    (ADRs as they are created)
```

Not every project needs every file. The `_index.md` tracks what's been created and what's pending.

---

## Version Control Rules

- Every change to the handoff package is logged in the project's `decisions.md` with a diff summary
- The package is version-locked when the Director signs off (Checkpoint 4)
- Post-lock changes require a new signed version (`v1.1`, `v2.0`) and an explicit diff section
- The dev suite executes against the locked version; changes do not take effect until the updated package is signed

---

## Related

- [[operating-model]] — why the handoff package exists and how it's used in the business cycle
- [[agent-org-chart]] — who produces and consumes each artifact
- [[model-routing-policy]] — the routing policy that lives inside each package
- [[project-handoff]] — template for the `handoff/_index.md` landing page
