---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-19
---

# Model Catalog

All LLM models available through the LiteLLM gateway. These are the models in use or planned for the Forge42 AI stack.

## Active Models

| Routing Alias | Model | Provider | Access Via | Cost Tier | Primary Use Case |
|---|---|---|---|---|---|
| `fast` | GPT-4o Mini | OpenAI | OpenRouter | $ (cheap) | Quick questions, classification, completion, summarization |
| `fast-claude` | Claude Haiku 4.5 | Anthropic | OpenRouter or direct | $ (cheap) | Fast tasks, short analysis, drafts |
| `strong` | Claude Sonnet 4.6 | Anthropic | Direct API | $$ (mid) | Architecture, complex reasoning, long generation |
| `strong-gpt` | GPT-4o | OpenAI | OpenRouter | $$ (mid) | Strong reasoning, fallback from Claude |
| `code` | DeepSeek Coder | DeepSeek | OpenRouter | $ (cheap) | Bulk code generation, refactoring, boilerplate |

## Model Tier Guidelines

### Cheap Tier ($) — Fast models
Use for: classification, tagging, summarization, short completions, code completion, questions with known answers, repetitive pipeline steps.

**Why cheap first:** The strong models cost 5–20x more per token. For tasks that don't require deep reasoning, the cheap tier is indistinguishable in quality.

### Mid Tier ($$) — Strong models
Use for: architectural decisions, complex code generation, multi-step reasoning, long-form generation, nuanced judgment calls, security and legal analysis.

**Rule:** Default to `strong` for the Development Suite primary tasks (Claude Code already routes here). Explicit routing to `fast` should be a conscious cost/quality tradeoff.

### Coding Specialist
Use for: pure code generation tasks where the prompt is well-specified and the output is mostly code, not reasoning. DeepSeek Coder has strong code generation at cheap-tier cost.

## Claude Code Default

Claude Code (the primary Development Suite operator) uses Claude Sonnet 4.6 by default. This does not route through LiteLLM — it calls the Anthropic API directly. LiteLLM is used for the AI stack services (Open WebUI, n8n, AnythingLLM, etc.).

When a project's [[model-routing-policy]] specifies routing certain task types to cheaper models, those tasks are handed off to separate API calls routed through LiteLLM.

## Future Models (Planned)

| Model | Provider | Notes |
|---|---|---|
| Local embedding model | Ollama (future GPU) | For mem0 + AnythingLLM embeddings without cloud cost |
| Local fast model | Ollama (future GPU) | Latency-sensitive tasks when GPU added to stack |

**Ollama is currently ruled out** — the Minisforum UM790 Pro has no discrete GPU and integrated graphics (Radeon 780M) cannot run useful model sizes. Revisit if a GPU workstation is added. See [[architecture]] decision log.

## OpenRouter Access

Models marked "via OpenRouter" are accessed through the OpenRouter aggregator. OpenRouter provides:
- Fallback routing between providers
- Access to model variants not available through direct provider APIs
- Cost comparison dashboard

## Related

- [[litellm-config]] — how models are configured in LiteLLM
- [[model-routing-policy]] — per-project policy for which tasks go where
- [[ai-orchestration]] — the full AI stack architecture
