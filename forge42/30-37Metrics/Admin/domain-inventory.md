---
tags: [brand/37m, type/reference, status/active]
updated: 2026-04-19
---

# Domain Inventory

All domains owned and managed by 37Metrics LLC.

## Active Domains

| Domain | Registrar | Expiry | DNS Host | Purpose | Email Config | Status |
|---|---|---|---|---|---|---|
| 37metrics.com | *[fill in]* | *[fill in]* | *[fill in]* | LLC corporate site | Office 365 | Active |
| dimension42.ai | *[fill in]* | *[fill in]* | *[fill in]* | D42 tech brand — primary product/content domain | FastMail catch-all | Active |
| martysampson.com | *[fill in]* | *[fill in]* | *[fill in]* | Marty personal brand site | *[TBD — route through Gmail or FastMail]* | Active |

## Email Architecture

### dimension42.ai
- **Provider:** FastMail custom domain
- **Configuration:** Catch-all wildcard `*@dimension42.ai` routes to Forge42 FastMail identity
- **Sending identities:**
  - `hello@dimension42.ai` — general inquiries
  - `support@dimension42.ai` — product support
  - `eddie@dimension42.ai` — AI agent communications
  - `no-reply@dimension42.ai` — transactional email
  - `[agentname]@dimension42.ai` — per-agent communications as needed

See [[fastmail]] for the full email architecture and DNS setup.

### 37metrics.com
- **Provider:** Office 365 (existing, stays as-is)
- **Purpose:** Legal entity email; contracts, invoices, tax filings
- **Not a content email** — 37Metrics is invisible to the public

### martysampson.com
- **Current SSO identity:** `martysampson@gmail.com` — keep as the public/social login
- **Custom domain email:** TBD — can route contact forms through FastMail or Gmail

## Domain Renewal Notes

- Set auto-renew on all domains — losing dimension42.ai would be catastrophic
- Registrar login credentials stored in Infisical
- Note expiry dates in this table when confirmed

## DNS Records (dimension42.ai)

FastMail requires these MX/DKIM records. See [[fastmail]] for the complete DNS configuration.

Key records:
- MX records pointing to FastMail servers
- SPF record for FastMail
- DKIM records for email authentication
- DMARC record for spoofing protection

## Potential Future Domains

| Domain | Purpose | Priority |
|---|---|---|
| dimension42.io | Defensive registration | Low |
| forge42.com | If Forge42 becomes public | Low |

## Related

- [[fastmail]] — email architecture and DNS setup
- [[37metrics-empire]] — full brand architecture
- [[handle-registry]] — social media handles (separate from domains)
