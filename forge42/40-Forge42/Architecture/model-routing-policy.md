---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-22
---

# Model Routing Policy

Default policy for routing tasks to the right model tier in the Development Suite. Customize per-project in `handoff/10-model-routing-policy.md`.

The goal: maximize output quality where judgment matters, minimize cost everywhere else.

---

## Default Routing Table

| Task Type | Model Tier | Notes |
|---|---|---|
| Vision, strategy, architectural decisions | Frontier (Claude Sonnet/Opus, GPT-4o) | High judgment required — wrong calls are expensive |
| Code generation, implementation | Claude Code | Primary dev suite operator |
| Test writing from acceptance criteria | Claude Code | Needs PRD context, judgment on coverage |
| Complex debugging, architectural refactors | Claude Code | High-judgment dev work |
| PR review, security review | Claude Code | Requires reasoning across context |
| Boilerplate generation | OpenCode + OpenRouter (cheaper models) | Low judgment, high volume |
| Routine refactors (rename, restructure) | OpenCode + OpenRouter | Mechanical work |
| Documentation extraction from code | OpenCode + OpenRouter | Pattern matching, not reasoning |
| Release notes from commit log | OpenCode + OpenRouter | Template-based synthesis |
| High-volume batch tasks | Local models via Ollama (Forge42/LUNA) | Cost-critical, latency-sensitive |
| Latency-sensitive inline tasks | Local models via Ollama | Response time matters more than quality |
| Offline work | Local models via Ollama | No API dependency |

---

## Routing Principles

1. **Default to Claude Code.** When in doubt, use Claude Code. Route down only when task complexity clearly doesn't require it.
2. **Never route security decisions down.** Threat modeling, access control decisions, and cryptographic choices always route to frontier models or Claude Code — never to cheaper or local models.
3. **Never route legal/compliance work down.** Same reason: wrong output has real consequences.
4. **Batch similar low-judgment work.** When you have 10 boilerplate files to generate, route all 10 to the cheaper tier together rather than doing them one at a time in Claude Code.
5. **Local models for speed, not quality.** Ollama models on LUNA are fast and free but less capable. Use them for tasks where "good enough quickly" beats "excellent slowly."

---

## Per-Project Override

Each project's handoff package includes its own `10-model-routing-policy.md` that overrides these defaults based on:
- Project cost budget
- Specific quality requirements for each component
- Whether the project has a frontend (design token implementation may warrant Claude Code over cheaper models)
- Enterprise/compliance requirements (may restrict routing to frontier models only)

---

## Forge42 Local Tier

Local models are available via Ollama running on LUNA (Forge42 infrastructure). Access through Eddie's stack or directly via the Ollama API endpoint.

Current models: see `40-Forge42/Stack/model-catalog.md` (if available) or query `ollama list` on LUNA.

---

## Related

- [[handoff-package-spec]] — how this policy fits into the handoff package
- [[agent-org-chart]] — where each model tier sits in the dev suite
- [[operating-model]] — the two-suite architecture this policy supports
