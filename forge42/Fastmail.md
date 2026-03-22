
# Fastmail Alias Architecture

## Overview

Primary account username is `forge42@fastmail.com` — this address is never shared or given out under any circumstances. All inbound mail flows through purpose-built aliases and their subdomains into dedicated folders. Subdomain addressing is disabled on Vault and Personal for compatibility and security reasons respectively. All other active aliases use subdomain addressing as the primary intake mechanism.

**Three-way financial split:** Vault is small and sacred — banks, credit cards, taxes, government only. Finance handles investment accounts and brokerage activity. Bills handles recurring utility and subscription payments. Do not mix these categories.

---

## Active Aliases

These are aliases you will actively use and route mail through.

| Prefix           | Email Address                 | Subdomain Addressing | Unlocks                         | Folder   | Purpose                                                                                                                  |
| ---------------- | ----------------------------- | -------------------- | ------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------ |
| `forge42`        | `forge42@fastmail.com`        | No                   | —                               | —        | Primary username. Never share or give out.                                                                               |
| `forge42ai`      | `forge42ai@fastmail.com`      | Yes                  | `*@forge42ai.fastmail.com`      | AI       | AI tool accounts, model update alerts, and AI service signups.                                                           |
| `forge42ainews`  | `forge42ainews@fastmail.com`  | Yes                  | `*@forge42ainews.fastmail.com`  | AI       | AI-specific newsletters and model update digests.                                                                        |
| `forge42alerts`  | `forge42alerts@fastmail.com`  | Yes                  | `*@forge42alerts.fastmail.com`  | Alerts   | Infra monitoring, uptime alerts, D42 agent notifications, Datto, BlackPoint.                                             |
| `forge42bills`   | `forge42bills@fastmail.com`   | Yes                  | `*@forge42bills.fastmail.com`   | Bills    | Utilities, recurring subscriptions, and service billing accounts.                                                        |
| `forge42dev`     | `forge42dev@fastmail.com`     | Yes                  | `*@forge42dev.fastmail.com`     | Dev      | API keys, GitHub alerts, developer tools, and service notifications.                                                     |
| `forge42finance` | `forge42finance@fastmail.com` | Yes                  | `*@forge42finance.fastmail.com` | Finance  | Brokerage accounts, investing apps, crypto exchanges, financial news. Distinct from Vault.                               |
| `forge42health`  | `forge42health@fastmail.com`  | Yes                  | `*@forge42health.fastmail.com`  | Health   | Doctors, pharmacy, insurance, health apps, and labs.                                                                     |
| `forge42media`   | `forge42media@fastmail.com`   | Yes                  | `*@forge42media.fastmail.com`   | Media    | Netflix, Spotify, HBO Max, and all streaming service accounts.                                                           |
| `forge42news`    | `forge42news@fastmail.com`    | Yes                  | `*@forge42news.fastmail.com`    | News     | General tech newsletters and news subscriptions.                                                                         |
| `forge42read`    | `forge42read@fastmail.com`    | Yes                  | `*@forge42read.fastmail.com`    | Read     | Audible, Kindle, Substack, and book-related alerts.                                                                      |
| `forge42sec`     | `forge42sec@fastmail.com`     | Yes                  | `*@forge42sec.fastmail.com`     | Security | HaveIBeenPwned, breach notifications, security advisory lists.                                                           |
| `forge42shop`    | `forge42shop@fastmail.com`    | Yes                  | `*@forge42shop.fastmail.com`    | Shop     | Amazon, eBay, and all retail and e-commerce accounts.                                                                    |
| `forge42social`  | `forge42social@fastmail.com`  | Yes                  | `*@forge42social.fastmail.com`  | Social   | Twitter/X, Reddit, LinkedIn, Facebook, and all social platform accounts.                                                 |
| `forge42test`    | `forge42test@fastmail.com`    | Yes                  | `*@forge42test.fastmail.com`    | Dev      | Dummy recipient for code and script testing. Routes to Dev folder.                                                       |
| `forge42travel`  | `forge42travel@fastmail.com`  | Yes                  | `*@forge42travel.fastmail.com`  | Travel   | Airlines, hotels, Airbnb, rental cars, TSA Precheck.                                                                     |
| `forge42vault`   | `forge42vault@fastmail.com`   | **No**               | —                               | Vault    | Banks, credit cards, taxes, IRS, government, legal. Direct alias only — no subdomain due to legacy system compatibility. |
| `forge42work`    | `forge42work@fastmail.com`    | Yes                  | `*@forge42work.fastmail.com`    | Work     | Recruiters, LinkedIn outreach, and anything work-adjacent outside your work email.                                       |
| `martylee`       | `martylee@fastmail.com`       | **No**               | —                               | Personal | Family, friends, and real humans. Subdomain disabled — no need for intake variations on a personal address.              |

---

## Defensive Aliases

These are grabbed to lock the `forge42` prefix on Fastmail's shared namespace. You may never actively route mail through them, but holding them prevents anyone else from registering them. No folder rules needed unless you begin using one.

|Prefix|Email Address|Reason to Hold|
|---|---|---|
|`forge42admin`|`forge42admin@fastmail.com`|Admin-flavored alias useful for self-hosted tools and dashboards.|
|`forge42d42`|`forge42d42@fastmail.com`|Locks the D42 project identity on Fastmail's namespace.|
|`forge42hello`|`forge42hello@fastmail.com`|Contact/intro alias people might guess or try.|
|`forge42me`|`forge42me@fastmail.com`|Natural "personal" variation — lock it even though martylee is the active personal alias.|
|`forge42ops`|`forge42ops@fastmail.com`|Ops-flavored alias for D42 infra as it grows.|
|`forge42reply`|`forge42reply@fastmail.com`|Useful as a reply-to on outbound mail where you don't want to expose a category alias.|
|`forgefortytwo`|`forgefortytwo@fastmail.com`|Spelled-out variation of the prefix — cheap to hold, annoying to lose.|

---

## How Subdomain Addressing Works

For any alias with subdomain addressing enabled, you can invent a unique address on the fly for any service — no pre-creation required. The format is:

```
servicename@alias.fastmail.com
```

**Example:** Alabama Power gets `alapower@forge42bills.fastmail.com`. Fastmail strips the subdomain, delivers to `forge42bills@fastmail.com`, and your folder rule moves it to Bills. If Alabama Power ever leaks your address or starts sending spam, you know exactly who leaked it and can disable that specific subdomain without touching anything else.

**Folder auto-filing:** If the subdomain portion matches a folder name, Fastmail will auto-file the message without needing a rule. Creating matching subfolders amplifies this — for example a `Bills/AlabamaP` subfolder would auto-receive `alabamap@forge42bills.fastmail.com` without any rule needed.

---

## Folders to Create

Create these folders in Fastmail before setting up rules. Defensive aliases do not require folders.

|Folder|Fed By|
|---|---|
|AI|`forge42ai@fastmail.com`, `forge42ainews@fastmail.com`|
|Alerts|`forge42alerts@fastmail.com`|
|Bills|`forge42bills@fastmail.com`|
|Dev|`forge42dev@fastmail.com`, `forge42test@fastmail.com`|
|Finance|`forge42finance@fastmail.com`|
|Health|`forge42health@fastmail.com`|
|Media|`forge42media@fastmail.com`|
|News|`forge42news@fastmail.com`|
|Personal|`martylee@fastmail.com`|
|Read|`forge42read@fastmail.com`|
|Security|`forge42sec@fastmail.com`|
|Shop|`forge42shop@fastmail.com`|
|Social|`forge42social@fastmail.com`|
|Travel|`forge42travel@fastmail.com`|
|Vault|`forge42vault@fastmail.com`|
|Work|`forge42work@fastmail.com`|

---

## Folder Rules

Set one rule per alias in Settings → Rules. For subdomain aliases, use "contains" so the rule catches any subdomain variation, not just the base alias address.

|Rule Condition|Action|
|---|---|
|Recipient address contains `@forge42ai.fastmail.com`|Move to **AI**|
|Recipient address contains `@forge42ainews.fastmail.com`|Move to **AI**|
|Recipient address contains `@forge42alerts.fastmail.com`|Move to **Alerts**|
|Recipient address contains `@forge42bills.fastmail.com`|Move to **Bills**|
|Recipient address contains `@forge42dev.fastmail.com`|Move to **Dev**|
|Recipient address contains `@forge42finance.fastmail.com`|Move to **Finance**|
|Recipient address contains `@forge42health.fastmail.com`|Move to **Health**|
|Recipient address contains `@forge42media.fastmail.com`|Move to **Media**|
|Recipient address contains `@forge42news.fastmail.com`|Move to **News**|
|Recipient address contains `@forge42read.fastmail.com`|Move to **Read**|
|Recipient address contains `@forge42sec.fastmail.com`|Move to **Security**|
|Recipient address contains `@forge42shop.fastmail.com`|Move to **Shop**|
|Recipient address contains `@forge42social.fastmail.com`|Move to **Social**|
|Recipient address contains `@forge42test.fastmail.com`|Move to **Dev**|
|Recipient address contains `@forge42travel.fastmail.com`|Move to **Travel**|
|Recipient address is `forge42vault@fastmail.com`|Move to **Vault**|
|Recipient address contains `@forge42work.fastmail.com`|Move to **Work**|
|Recipient address is `martylee@fastmail.com`|Move to **Personal**|

---

## Vault Usage Guidelines

Because subdomain addressing is disabled for Vault, each high-security institution gets the same `forge42vault@fastmail.com` address. To maintain traceability, record which institution is registered with this address in the Bitwarden notes field for each login. If a bank or government site rejects even this address, fall back to a dedicated Masked Email address generated from within Fastmail — this gives you a unique throwaway that still routes to the Vault folder via a rule targeting the masked address.

**Boundary reminder:**

- Vault — bank accounts, credit cards, taxes, government, anything that can drain money or affect your identity
- Finance — Robinhood, Fidelity, crypto exchanges, investment accounts, financial news
- Bills — utility payments, SaaS subscriptions, recurring service charges

---

## API and Claude Access

Fastmail's JMAP API is the integration layer for Claude and D42 agents.

- Generate a dedicated API token at **Settings → Privacy & Security → Manage API tokens**
- JMAP session endpoint: `https://api.fastmail.com/jmap/session`
- Authenticate with: `Authorization: Bearer {token}`
- Scope access by folder when building MCP connectors — expose only the folder relevant to a given agent or Claude integration, never the full account

Generate one token per integration. Never reuse tokens across agents or tools.

---

## Custom Domain Strategy

For owned domains (e.g., `dimension42.ai`), set up a catch-all wildcard (`*@yourdomain.com`) in **Settings → My Email Addresses**. This allows any address at the domain to be invented on the fly without pre-creation — ideal for D42 agent identities, project personas, and developer tooling.