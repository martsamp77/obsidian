---
tags: [brand/marty, type/reference, status/active]
updated: 2026-04-19
---

# martysampson.com — Sitemap

Planned page structure for the Marty Sampson personal brand site. This is the canonical "home base" that ties together all channels, consulting, and the zero-human narrative.

## Page Structure

```
martysampson.com/
├── /                        Home — about, links, hero CTA
├── /about                   Full story, background, zero-human journey
├── /links                   Linktree replacement
├── /blog                    Essays and long-form content
├── /private/[token]         Gated content (token-based, no login)
└── /consulting              Services, pricing, contact
```

## Page Details

### `/` — Home

**Purpose:** The first impression for anyone who finds Marty online
**Core elements:**
- Brief headline + bio: who Marty is, what he's building
- Hero CTA: newsletter signup or "follow the journey"
- Links to YouTube, Substack, X, LinkedIn
- Quick links to consulting and gated content
- Featured content (latest video, essay, or milestone)

### `/about`

**Purpose:** Full brand story — the zero-human journey as a narrative
**Content:**
- "I'm building a software company, a media company, and a consulting practice with zero employees."
- Personal background (Marty fills in specifics)
- What Dimension42 is and how it fits
- What Marty believes about AI + work + the future
- Call to follow the journey (YouTube, Substack)

See [[about-page-draft]] for the draft copy.

### `/links`

**Purpose:** Linktree replacement — one URL for bio links on social platforms
**Content:**
- YouTube: @martysampson
- YouTube D42: @dimension42ai
- Substack: martysampson.substack.com
- X: @martysampson
- LinkedIn
- Consulting inquiry

### `/blog`

**Purpose:** Long-form essays about building, AI, decisions, failures, insights
**Note:** This overlaps with Substack. Strategy: publish essays on Substack first (drives subscribers), then syndicate to the site after 7 days.

### `/private/[token]`

**Purpose:** Gated content access for subscribers or consulting clients
**Technical approach:** Generate unique URL tokens mapped to content sets. No login required — the token IS the access credential. Tokens are revocable.

**Use cases:**
- Bonus content for paid Substack subscribers
- Resource packs for consulting clients
- Early access to products

See [[marty-website]] project idea for technical scope notes.

### `/consulting`

**Purpose:** Convert LinkedIn/YouTube visitors into consulting inquiries
**Content:**
- What Marty offers (strategy, advisory, fractional AI leadership)
- Who it's for
- How to engage
- Contact form (routes to Marty's email or calendar link)
- **Pricing section:** Marty fills in

## Technical Considerations

- **Email:** Route through FastMail; `martysampson@gmail.com` stays as public SSO identity
- **Gated content system:** Token-based URL access — simple database + middleware (no auth library needed)
- **Stack TBD:** Next.js or Astro — decide when project becomes Active

## Launch Milestone

MVP = Home + About pages live, links page working, consulting inquiry form functional. Blog and gated content can come later.

## Related

- [[marty-website]] — project idea file
- [[zero-human-narrative]] — the brand story that drives the About page
- [[about-page-draft]] — draft copy for the About page
- [[handle-registry]] — @martysampson social handles
