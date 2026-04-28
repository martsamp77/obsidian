---
status: idea
brand: marty
type: software
priority: high
created: 2026-04-19
---

# martysampson.com Website

## What Is It

The personal brand hub for Marty Sampson. Serves as the canonical "home base" that ties together all content channels, consulting, speaking, and the zero-human narrative.

## Why It Matters

Anyone who discovers Marty through YouTube, LinkedIn, or word of mouth needs somewhere to land that tells the full story and drives them toward a CTA (newsletter, consulting inquiry, gated content). Build priority is high.

## Which Brand

- [x] Marty Sampson — personal brand site

## Rough Scope

Medium (weeks for MVP, ongoing) — launch basics fast, then build out the gated content system.

## Target Pages

- `/` — About, links to all channels, hero CTA
- `/about` — Full story, background, the zero-human journey
- `/links` — Linktree replacement: YouTube, Substack, X, LinkedIn, consulting
- `/blog` — Essays and long-form content
- `/private/[token]` — Gated content system (token-based access, no login required)
- `/consulting` — Services, pricing, contact

## Notes

- Gated content system: generate unique URL tokens that map to content sets. Revocable. No login required. Simple database + middleware.
- Email contact: can route through FastMail; `martysampson@gmail.com` stays as the public SSO identity — don't fight it
- See [[zero-human-narrative]] for the brand story that should drive the homepage
- See [[37metrics-empire]] for full domain strategy
