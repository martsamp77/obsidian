---
tags: [brand/37m, type/reference, status/active]
updated: 2026-04-19
---

# 37Metrics Empire: Brand Architecture

## Corporate Hierarchy

```
37Metrics, LLC (Holding Company / Legal Entity)
├── 37metrics.com — LLC operations, contracts, invoicing, DBA registrations
├── Office 365 email — legal entity email, stays as-is
│
├── DBA: Dimension42 (d/b/a 37Metrics, LLC)
│   ├── dimension42.ai — Public tech brand, AI content, software products
│   ├── FastMail catch-all: *@dimension42.ai
│   ├── YouTube: AI/tech faceless channel (@dimension42ai)
│   ├── Software products sold under D42 brand
│   └── BAAS platform (in development)
│
└── Personal Brand: Marty Sampson
    ├── martysampson.com — Personal site, gated content, portfolio
    ├── YouTube: Hybrid channel (@martysampson)
    ├── Substack, social media
    └── "Zero Human" narrative lives here
```

**Legal logic:** 37Metrics is the legal shield. Dimension42 operates as a DBA — no separate LLC needed until D42 warrants investment or a sale. Marty Sampson is the personal brand that tells the story across all channels.

**When 37Metrics shows up publicly:** Invoices, contracts, legal notices, terms of service ("Dimension42 is a product of 37Metrics, LLC"), tax filings. It does not need social media or content — it shows up in footers and legal pages, not headlines.

## Naming Quick Reference

| Thing | Name | Why |
|---|---|---|
| Holding company (LLC) | 37Metrics | Existing legal entity |
| Tech/product brand | Dimension42 | Physics reference, marketable, owns .ai domain |
| Personal brand | Marty Sampson | Your name, your story |
| Orchestration platform | OpenClaw | Community-facing open source name |
| Primary AI agent | Eddie | Iron Maiden reference, distinct personality |
| Infrastructure dashboard | Forge42 | Tied to forge42 FastMail identity |
| Health project | IronMarty | Personal; "iron" fits health/fitness context |
| Internal email identity | forge42 | Established in FastMail architecture |
| Machine naming | Functional (hub, gpu-1, dev) | No personality lock-in |
| D42 social handle | @dimension42ai | Unique, consistent across all platforms |
| Personal social handle | @martysampson | Your name |

## Content Platform Map

| Platform | Marty Sampson | Dimension42 | 37Metrics |
|---|---|---|---|
| YouTube | Hybrid channel (face + AI) | Faceless tech channel | None |
| Substack | Personal essays, zero-human journey | Technical newsletter (optional) | None |
| X | Personal updates, hot takes | Product announcements, tech threads | Defensive handle only |
| Reddit | Engage in AI/tech subreddits as yourself | Post tutorials as D42 | None |
| Instagram | Behind-the-scenes, personal | Infographics, visual tutorials | None |
| LinkedIn | Professional profile, consulting leads | Company page | Company page |
| TikTok | Repurposed Shorts | Repurposed Shorts | None |
| Bluesky | Mirror X content | Mirror X content | None |

## Risks and Downsides

**Dimension42 name:**
- Level 42 is a real band (different industry/era) — no trademark conflict for software. Consult attorney if building music-adjacent products.
- Dimension 20 (YouTube, tabletop RPG) — using "@dimension42ai" cleanly avoids confusion.
- "dimension42 ai" is the primary search keyword to target — not just "dimension42."

**Two YouTube channels:**
- Slower growth on each vs. focusing on one. Mitigation: build Marty Sampson channel first; launch D42 only after a content pipeline can sustain 2+ videos/week without your direct time.
- YouTube may connect the two channels. Not a problem — the connection is legitimate under the LLC.

**Zero Human narrative:**
- Product bugs may invite blame on "no support team." Keep the narrative on Marty's brand; D42 products just need to work.
- Only works if you're building. Ship first, talk second.

**Handle squatting:**
- YouTube may reclaim unused handles. Post at least one piece of content on each platform within 30 days of claiming.

## Action Checklist

### Week 1: Lock Down Handles
- [ ] Register @dimension42ai on YouTube, X, Reddit, Instagram, TikTok, Bluesky, Threads
- [ ] Confirm @martysampson handles on YouTube and X
- [ ] Create github.com/dimension42ai organization
- [ ] Grab dimension42ai.substack.com and martysampson.substack.com

### Week 2: Domain Configuration
- [ ] Add dimension42.ai to FastMail as a custom domain with catch-all wildcard
- [ ] Set up hello@, support@, and eddie@ on dimension42.ai
- [ ] Create D42 folder in FastMail with routing rule for *@dimension42.ai
- [ ] Set up DNS MX records pointing to FastMail for dimension42.ai

### Week 3: Placeholder Pages
- [ ] 37metrics.com — Simple one-page site (logo, LLC info, links to D42 and Marty)
- [ ] dimension42.ai — Landing page (coming soon, email signup, links to YouTube)
- [ ] martysampson.com — Landing page (about, links to all channels, contact)

### Week 4: Content Pipeline
- [ ] Set up YouTube studio in extra bedroom
- [ ] Record first Marty Sampson video: "I'm building a company with zero employees"
- [ ] Produce first D42 faceless video: technical tutorial from the stack
- [ ] Start Substack with introductory post about the zero-human journey

## References

Detailed strategy lives where it belongs across the vault:

- **Mission & purpose:** [[mission]] — why 37Metrics exists and what it's building toward
- **Operating model:** [[operating-model]] — the Two-Suite Architecture: how 37Metrics builds and ships products
- **Brand & naming:** [[naming-strategy]] — Tier 1 (public) and Tier 2 (community) naming rules
- **Infrastructure naming:** [[naming-conventions]] — Tier 3 machine/service naming
- **Social handles:** [[handle-registry]] — All platform handles with status tracking
- **Email architecture:** [[fastmail]] — FastMail alias architecture + dimension42.ai setup
- **YouTube (D42):** [[10-Dimension42/Content/youtube-strategy|D42 YouTube Strategy]]
- **YouTube (Marty):** [[20-MartySampson/Content/youtube-strategy|Marty YouTube Strategy]]
- **Zero Human story:** [[zero-human-narrative]]
- **Revenue streams:** [[revenue-model]]
- **GitHub orgs:** [[github-org]]
- **Software projects:** [[_pipeline]]

---
*Living strategy document. Update as decisions are made and priorities shift.*
