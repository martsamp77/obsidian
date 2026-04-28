---
tags: [brand/forge42, type/reference, status/active]
updated: 2026-04-19
---

# AI Orchestration

How the AI stack layers work together on Forge42, and how each layer connects to the two-suite operating model.

## Stack Overview

```
┌─────────────────────────────────────────────────────────────────┐
│  AI Tool Clients                                                │
│  Claude Code · Cursor · Open WebUI · n8n · AnythingLLM         │
└──────────────────────────────┬──────────────────────────────────┘
                               │ OpenAI-compatible API
┌──────────────────────────────▼──────────────────────────────────┐
│  LiteLLM Gateway (port 4000)                                    │
│  Route by task type → correct model tier                        │
│  Redis caching · usage tracking · API key management           │
└────────────┬─────────────────────────────────────┬─────────────┘
             │                                     │
             ▼ Cloud (primary)                     ▼ Local (future)
┌────────────────────────────┐     ┌───────────────────────────────┐
│  OpenRouter                │     │  Ollama (planned, GPU needed)  │
│  · Anthropic (Claude)      │     │  For local latency-sensitive   │
│  · OpenAI (GPT)            │     │  tasks when GPU is available   │
│  · DeepSeek                │     └───────────────────────────────┘
│  · Fallback routing        │
└────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  Memory & Knowledge Layer                                       │
│                                                                 │
│  mem0 MCP server (port 8765)                                    │
│  ├── Qdrant:6333 — vector storage for memory embeddings         │
│  └── Persistent across all tools: Claude Code, Cursor, WebUI   │
│                                                                 │
│  AnythingLLM (port 3001)                                        │
│  ├── Qdrant:6333 — document embeddings for knowledge base       │
│  └── Workspace-per-company RAG system                          │
└─────────────────────────────────────────────────────────────────┘
```

## Components

### LiteLLM — The Gateway

Every AI-consuming service points to LiteLLM instead of calling providers directly. One API key rotation in LiteLLM propagates everywhere.

**Why this matters:** 6 services × 3 providers = 18 credential management points without LiteLLM. With LiteLLM: 1 point.

**Routing strategy:**

| Task Type | Model Tier | Model |
|---|---|---|
| Architecture decisions, complex reasoning | Frontier | Claude Sonnet or GPT-4o |
| Code generation, implementation | Primary | Claude Code (via Claude Sonnet) |
| Quick questions, classification, completion | Fast/cheap | GPT-4o Mini or Claude Haiku |
| Pure code generation (bulk) | Specialized | DeepSeek Coder via OpenRouter |

See [[model-catalog]] for the full model list and [[model-routing-policy]] for the per-project routing policy template.

### OpenRouter — Model Aggregator

OpenRouter provides access to all major cloud models (Anthropic, OpenAI, Google, Meta, Mistral, DeepSeek) through one API endpoint. LiteLLM uses OpenRouter as its backend for cloud access.

**Key benefit:** Automatic fallback — if Anthropic is down, OpenRouter routes to an equivalent Claude-class model without any config change.

### Open WebUI — Daily Chat Interface

Day-to-day AI interaction. Connects to LiteLLM for model access, SearXNG for web search, and uses the mem0-connected context for persistent memory.

Access at `http://luna-tailscale-ip:3000` or via subdomain once Traefik is configured.

### SearXNG — Self-Hosted Web Search

Aggregates results from multiple search providers. Used by Open WebUI and n8n for real-time web access without leaking queries to Google/Bing. No rate limiting.

### mem0 — Persistent Agent Memory

The memory layer that fixes the "every session starts from zero" problem.

- **What it stores:** Debugging solutions, architectural decisions, project context, personal preferences
- **How it stores:** Vector embeddings in Qdrant; retrieved by semantic similarity, not keyword match
- **What tools use it:** Claude Code, Cursor, VS Code (via MCP), Open WebUI

**Claude Code setup:**
```bash
claude mcp add --scope user --transport stdio mem0 -- \
  uvx --from git+https://github.com/elvismdev/mem0-mcp-selfhosted.git mem0-mcp-selfhosted
```

**mem0 vs AnythingLLM distinction:**
- mem0 = episodic, automatically captured during work sessions
- AnythingLLM = curated documents, intentionally added reference material

### AnythingLLM — Curated Knowledge Base

RAG system for structured company knowledge. Every document worth keeping (PDFs, tech specs, process docs, meeting notes) goes into a workspace here.

**Workspace structure:**
- `dimension42` — D42 products and tech strategy
- `37metrics-ops` — LLC operations, contracts, reference
- `personal` — personal life management content
- `dev-stack` — infrastructure documentation
- `research` — research and notes

### Dify — AI Application Builder

Visual platform for building AI-powered features inside products. Drag-and-drop agent pipelines, API deployment, embedded chat widgets. Separate from AnythingLLM — this is for building product features, not managing personal knowledge.

## Connection to the Operating Model

This AI stack is the infrastructure layer for both suites defined in [[operating-model]]:

| Suite | Primary Tools | This Stack's Role |
|---|---|---|
| Development Suite | Claude Code (primary), OpenCode (cost-sensitive) | LiteLLM routes model calls; mem0 provides memory; AnythingLLM holds project docs |
| Business Planning Suite (future) | ChatGPT + Paperclip Hermes | Same LiteLLM gateway; n8n can automate Business Suite workflows |

## Related

- [[architecture]] — overall system architecture
- [[docker-services]] — all services with ports
- [[model-catalog]] — available models and routing aliases
- [[model-routing-policy]] — per-project routing policy template
- [[operating-model]] — two-suite architecture this stack supports
