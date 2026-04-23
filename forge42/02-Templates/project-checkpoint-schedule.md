---
project: [Project Name]
tags: [type/reference]
---

# [Project Name] — Checkpoint Schedule

Marty's 7 review points for this project. Fill in target dates when the project enters active status.

| # | Checkpoint | Trigger | Target Date | Actual Date | Notes |
|---|---|---|---|---|---|
| 1 | Vision Brief + Business Brief approved | Before product definition begins | | | |
| 2 | PRD approved | Before design and architecture begin | | | |
| 3 | System Architecture Brief + ADRs approved | Before implementation begins | | | |
| 4 | Full handoff package approved — **Dev Suite starts** | Before Claude Code begins work | | | |
| 5 | Shippable increment evaluated | Before each release | | | |
| 6 | Security/legal docs approved | Before publication | | | |
| 7 | Post-mortem + retrospective reviewed | After incidents; quarterly | | | |

---

## Checkpoint 4 Sign-off Checklist

Before the dev suite starts, confirm:

- [ ] All core handoff documents are complete (see `handoff/_index.md`)
- [ ] ADRs cover all major architectural decisions
- [ ] Model routing policy is set and cost budget is approved
- [ ] Backlog items are scoped with acceptance criteria
- [ ] NFRs are expressed as measurable metrics
- [ ] Test strategy specifies pass criteria
- [ ] No open questions that should be answered before implementation starts

**Director signature:** _______________  
**Date:** _______________  
**Package version locked at:** _______________

---

## Between-Checkpoint Rules

- Dev Suite runs autonomously between approved checkpoints
- Questions not answered by the handoff package → route to orchestrator → Marty if not auto-resolvable
- Do NOT make assumptions about unresolved business decisions — surface them
- New implementation-phase ADRs → create in `handoff/adr/`, flag for Director review at next checkpoint

*See [[operating-model]] for the full checkpoint rationale.*
