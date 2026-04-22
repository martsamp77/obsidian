---
status: idea
brand: forge42
type: software
priority: low
created: 2026-04-19
---

# Apple TV Corporate Dashboard

## What Is It

A native Apple TV application that displays key corporate data — project pipeline status, revenue metrics, active tasks, infrastructure health — on the living room TV as an ambient information display.

## Why It Matters

Real-time visibility into business metrics without being at a desk. The TV becomes a passive "mission control" dashboard. As the AI agent ecosystem grows, having a glanceable view of what's running, what's blocked, and what's shipping is valuable.

## Which Brand

- [x] Forge42 — internal infrastructure tool, not customer-facing (initially)

## Rough Scope

Medium (weeks for MVP, months for polish) — tvOS development requires Xcode and Swift/SwiftUI knowledge.

## Notes

- Data sources: Obsidian vault properties (via API or file read), revenue tracking, project status from `05-Projects/`
- Could pull from a lightweight local API served by the Forge42 infrastructure stack
- Apple TV app could eventually become a D42 product for other solo operators or small teams
- Dependency: the CRM/metrics data needs to exist in a queryable form first
- Related: [[crm]] idea could feed data into this dashboard
