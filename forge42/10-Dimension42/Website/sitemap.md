---
tags: [brand/d42, type/reference, status/active]
updated: 2026-04-19
---

# dimension42.ai — Sitemap

Planned page structure for the Dimension42 website. Priority order: ship the hero landing page fast, then iterate.

## Page Structure

```
dimension42.ai/
├── /                    Home — hero, CTA, email signup
├── /blog                Technical blog and tutorials
├── /products            Product listing (expand as products ship)
│   └── /products/[slug] Individual product pages
├── /docs                Documentation (per-product, expands over time)
├── /about               Company story, D42/37Metrics relationship
└── /support             Support contact form
```

## Page Details

### `/` — Home

**Purpose:** Primary landing page for all traffic (YouTube viewers, search, direct)
**Core elements:**
- Hero: "AI-native software for builders" (or equivalent — refine based on current products)
- Email signup CTA — build the list before products launch
- Links to YouTube channel
- Product cards (as they launch)
- Footer: "Dimension42 is a product of 37Metrics, LLC"

**SEO target:** "dimension42 ai" (primary search keyword)

### `/blog`

**Purpose:** Technical content hub — tutorials, AI tool coverage, product deep dives
**Note:** D42 blog is separate from Marty's personal essays (which live on Substack / martysampson.com)
**Content cadence:** Driven by YouTube content pipeline — video content → blog adaptation

### `/products`

**Purpose:** Product directory; starts empty, grows as D42 ships
**Initial products to list:** TBD (see [[_pipeline]] for ideas in progress)

### `/docs`

**Purpose:** Product documentation
**Format:** Per-product documentation; consider Docusaurus or similar static docs generator

### `/about`

**Purpose:** Brand story and legitimacy
**Content:**
- What Dimension42 builds (AI-native software)
- The technology/physics branding story
- "Dimension42 is a DBA of 37Metrics, LLC" (no more, no less)
- Links to YouTube and products

**What NOT to include:** Zero-human narrative, Marty's personal story. Those live on martysampson.com.

### `/support`

**Purpose:** Contact for product support
**Route:** Form → `support@dimension42.ai` (FastMail catch-all)

## Technical Considerations

- **Email:** All forms route to `*@dimension42.ai` on FastMail; see [[fastmail]]
- **SEO:** "dimension42 ai" is the primary search keyword to target
- **Stack TBD:** Next.js, Astro, or a static site generator — decide when project becomes Active
- **Launch strategy:** Ship a single landing page first. `/blog`, `/products`, `/docs` can be stubs.

## Launch Milestone

MVP = Home page live with email capture. Everything else is iteration.

## Related

- [[d42-website]] — project idea file with scope and notes
- [[fastmail]] — email architecture for dimension42.ai
- [[naming-strategy]] — brand naming context
- [[handle-registry]] — @dimension42ai social handles
