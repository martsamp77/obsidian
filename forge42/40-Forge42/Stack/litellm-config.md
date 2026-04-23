---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-19
---

# LiteLLM Configuration

LiteLLM is the central LLM gateway. Every AI-consuming service (Open WebUI, n8n, AnythingLLM, Dify, Claude Code via MCP, Cursor) points to LiteLLM instead of calling cloud providers directly.

## Why LiteLLM

- **Single key management point.** Rotate an API key in LiteLLM — not in 6 different services.
- **Model routing.** Route tasks to the right model tier automatically by endpoint or prefix.
- **Response caching.** Redis caches identical requests — saves API costs on repeated queries.
- **Usage tracking.** Per-model and per-service cost monitoring in one place.
- **OpenAI-compatible API.** Any service that speaks OpenAI can point to LiteLLM at `http://luna:4000`.

## Service Endpoint

```
Base URL: http://192.168.0.11:4000   (LAN)
Base URL: http://<luna-ts-ip>:4000   (Tailscale)
```

Once Traefik is configured: `https://litellm.forge42.internal`

## Routing Strategy

LiteLLM routes requests to the appropriate model based on the endpoint called or explicit `model` parameter in the request.

| Routing Name | Model | Provider | Use Case | Cost Tier |
|---|---|---|---|---|
| `fast` | GPT-4o Mini | OpenAI via OpenRouter | Quick questions, classification, code completion | Cheap |
| `fast-claude` | Claude Haiku | Anthropic via OpenRouter | Fast tasks with better quality than GPT-4o Mini | Cheap |
| `strong` | Claude Sonnet | Anthropic direct | Architecture decisions, complex generation, reasoning | Mid |
| `strong-gpt` | GPT-4o | OpenAI via OpenRouter | Strong reasoning, fallback from Claude | Mid |
| `code` | DeepSeek Coder | OpenRouter | Pure code generation, bulk refactoring | Cheap |

For the project-level routing policy template, see [[model-routing-policy]].

## Redis Caching

LiteLLM uses Redis for response caching. Identical requests return cached responses without calling the cloud API.

- Reduces cost on repeated queries (e.g., classification tasks with fixed prompts)
- Redis runs at `redis:6379` on the shared Docker network

## OpenRouter Backend

LiteLLM connects to OpenRouter as the primary backend for cloud model access. OpenRouter provides:
- Access to all major providers (Anthropic, OpenAI, Google, Meta, Mistral, DeepSeek) through one API key
- Automatic fallback: if one provider is down, routes to an equivalent model
- Cost comparison across providers

**OpenRouter API key stored in:** Infisical

## Per-Service API Keys

LiteLLM can enforce per-service API keys (virtual keys). Each consuming service gets its own key, allowing granular usage tracking:

| Service | LiteLLM Virtual Key | Notes |
|---|---|---|
| Open WebUI | `sk-webui-...` | Chat interface |
| n8n | `sk-n8n-...` | Workflow automation |
| AnythingLLM | `sk-atllm-...` | Knowledge base |
| Dify | `sk-dify-...` | Agent builder |
| Claude Code | `sk-cc-...` | Dev suite (if routing through LiteLLM) |

## litellm_config.yaml Structure

```yaml
model_list:
  - model_name: fast
    litellm_params:
      model: openai/gpt-4o-mini
      api_base: https://openrouter.ai/api/v1
      api_key: os.environ/OPENROUTER_API_KEY

  - model_name: strong
    litellm_params:
      model: anthropic/claude-sonnet-4-6
      api_key: os.environ/ANTHROPIC_API_KEY

  - model_name: code
    litellm_params:
      model: openrouter/deepseek/deepseek-coder
      api_base: https://openrouter.ai/api/v1
      api_key: os.environ/OPENROUTER_API_KEY

general_settings:
  cache: True
  cache_params:
    type: redis
    host: redis
    port: 6379
```

*Actual values in Infisical — never in the repo.*

## Monitoring

LiteLLM exposes a dashboard at `http://luna:4000/ui` (when enabled) showing:
- Total spend per model and per virtual key
- Request counts and latency
- Cache hit rates

## Related

- [[model-catalog]] — all available models and their properties
- [[model-routing-policy]] — per-project task routing policy
- [[ai-orchestration]] — how LiteLLM fits in the AI stack
- [[docker-services]] — service ports and network
