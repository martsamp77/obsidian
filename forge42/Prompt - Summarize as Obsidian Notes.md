# Claude Conversation Summary — Obsidian Prompt

This is your reusable prompt snippet. Paste it as the **last message** in any Claude conversation you want summarized, or as a **standalone message** in a new conversation where you paste the prior transcript.

---

## How to Use

- **Option A (End of conversation):** Paste the prompt below as your final message. Claude will summarize everything above it.
- **Option B (New conversation):** Paste the prior conversation transcript first, then paste this prompt.
- **Option C (System prompt):** Save this as a Project instruction in Claude Enterprise so it's always available.

---

## The Prompt

Copy everything inside the fenced block below and save it wherever you store snippets.

```
Summarize this conversation as an Obsidian note. Follow these instructions exactly and in order.

---

STEP 1 — DETECT AND STATE CONTEXT

Before generating the note, output a short detection statement (2–4 sentences) that states:
- Whether this is Work (Molecular Designs / Streamline Scientific), Personal (Marty's personal life), or D42 (Dimension42 / 37Metrics / SheetSense / DeckEngine / OpenClaw)
- Which specific project or topic it belongs to
- Your reasoning for that classification

Then output the suggested full file path on its own line using this format:
Suggested path: [vault]\[subfolder]\YYYY-MM-DD-project-slug.md

Vault routing rules:
- Work → C:\Workspace\obsidian-work\vault-work\
- Personal → C:\Workspace\obsidian-marty\vault-marty\
- D42 / dev / startup → D:\00_Obsidian\forge42\

Subfolder guidance (suggest the most logical one):
- vault-work subfolders: NetSuite, BigCommerce, ActiveDirectory, Microsoft365, SOC2, HIPAA, LIMS, Infrastructure, Security, VoIP, Vendors, IT-General
- vault-marty subfolders: Health, Fitness, Longevity, Music, Reading, Finance, Career, Family, Personal
- forge42 subfolders: OpenClaw, SheetSense, DeckEngine, AIAgents, Infrastructure, AWS, DotUp, ProductIdeas, D42-General

Known project taxonomy for classification and tagging:
- Work: NetSuite, SuiteScript, SuiteQL, BigCommerce, Stencil, Active Directory, Hyper-V, VMware, Veeam, Orchard LIMS, Mirth Connect, HL7, Microsoft 365, Datto RMM, Sophos, BigLeaf, Broadvoice, Yealink, BlackPoint, Nessus, Bitwarden, KnowBe4, EasyDMARC, Traceable, Claude Enterprise, SOC2, HIPAA, CLIA, ISO 13485, FDA, Greenlight Guru, Molecular Designs, Streamline Scientific, Excellere Partners
- D42: Dimension42, OpenClaw, LiteLLM, Ollama, Qdrant, Redis, Postgres, Luna, Aurora, AWS, Terraform, SheetSense, Tauri, Rust, DeckEngine, React, Vite, Tailwind, DotUp, AI Diary, Adaptive Flashcards, Podcast Aggregator, Token Monitor, Desktop Console, CarPlay AI, Cash Flow, 37Metrics, Eddie, Blaine (agent)
- Personal: health, TRT, Retatrutide, GLP-1, longevity, carnivore, Peloton, lifting, fitness, guitar, SRV Stratocaster, reading, books, family, Mark Sampson, Debra Sampson, Blaine Sampson, finance, career, Birmingham, Hoover

If the conversation spans multiple contexts, classify by dominant context and note secondary context in tags.
If the context is genuinely ambiguous, say so and ask for confirmation before generating the note.

---

STEP 2 — GENERATE THE NOTE

Output the entire Obsidian note in a single fenced markdown code block. Nothing else outside the code block except the detection statement and suggested path from Step 1.

File naming format: YYYY-MM-DD-project-slug.md
- Use today's date
- Slug: 3–5 words, kebab-case, specific to the core topic
- Examples: 2025-03-15-netsuite-restlet-oauth-debug.md | 2025-06-01-openclaw-litellm-routing.md | 2025-09-10-retatrutide-protocol-notes.md

---

NOTE STRUCTURE (use this exact order):

--- FRONTMATTER ---

---
title: [Descriptive Title in Title Case]
date: YYYY-MM-DD
context: work | personal | d42
project: [primary project name, e.g. NetSuite, OpenClaw, SheetSense, Health]
topic: [one-line description of what this conversation was about]
tags: [tag1, tag2, tag3]
status: review
summary_type: conversation-summary
---

Tags should be lowercase and drawn from the known project taxonomy above. Include 3–6 relevant tags.

--- SECTIONS ---

## Summary
Full narrative recap of the conversation. Write 3–8 paragraphs as if briefing someone who needs complete context to pick up where this left off. Cover: what the problem or goal was, what was explored, what was tried, what worked or didn't, and what the final state is. Do not use bullet points in this section. Write in flowing prose.

## Key Decisions
Bullet list of concrete decisions made or conclusions reached during the conversation. Only include this section if real decisions were made. If not, omit entirely.

## Action Items
Checkbox list of next steps, open tasks, or follow-ups identified during the conversation. Format each item as: - [ ] Task description. Only include this section if action items exist. If not, omit entirely.

## Code / Technical Artifacts
Include only if the conversation produced code, queries, scripts, configs, schemas, API specs, or technical specifications of any kind. Use fenced code blocks with the correct language identifier (sql, javascript, rust, powershell, python, bash, typescript, json, yaml, etc.). If multiple artifacts exist, label each with a brief heading. If nothing technical was produced, omit this section entirely.

## Context for Next Conversation
This is the most important section. Write a dense, structured block of context — 150 to 300 words — that can be pasted verbatim at the start of a new Claude conversation to fully restore context without re-reading the original thread. Address Claude directly in second person. Include: what was being worked on and why, what was decided, what the current state of the work is, what comes next, and any relevant technical or personal background Claude would need to be immediately useful. Write this as a mission brief — tight, specific, no fluff.
```

---

## Notes

- The **Context for Next Conversation** section is what makes this workflow powerful over time. Treat it as the thing you paste, not the whole note.
- If a conversation had no decisions or action items, those sections are omitted automatically — the note won't have empty scaffolding.
- The detection statement appears _before_ the code block so you can scan it quickly without opening the block.
- You can add a `related: [[other-note]]` field to frontmatter manually after pasting if you want to wire it into your graph.