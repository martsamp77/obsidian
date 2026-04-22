# Monday.com and Obsidian Organization Guide

## Monday.com

### Account URL

**Use: `forge42.monday.com`**

Here's why. The Monday account URL is the top-level identity for your entire operational command center. It should not be brand-specific because you're managing multiple brands from one account. The options were:

- `37metrics.monday.com` — Technically correct (holding company), but 37Metrics is a legal entity that shows up on invoices, not an operational identity. It feels like filing cabinets, not a command center.
- `dimension42ai.monday.com` — Too narrow. D42 is one brand. You're also managing Marty Sampson content, personal health, infrastructure, and the holding company from this same account.
- `martysampson.monday.com` — Too personal. If you ever bring on a contractor or virtual assistant, "martysampson" feels like they're logging into your diary.
- **`forge42.monday.com`** — This is your operational identity. It's the forge where everything is built. It matches your FastMail infrastructure (forge42 prefix). It's brand-neutral but distinctly yours. It signals "command center" not "one specific brand."

If `forge42` is taken, fall back to `37metrics`.

### Workspace Structure

Monday.com hierarchy: **Account → Workspaces → Folders → Boards → Groups → Items → Sub-items**

Workspaces are the highest level of organization. You need five.

```
forge42.monday.com
├── Workspace: Dimension42
├── Workspace: Marty Sampson
├── Workspace: 37Metrics
├── Workspace: Forge42 (Infrastructure)
└── Workspace: Personal
```

---

### Workspace 1: Dimension42

Everything related to the D42 tech brand, products, content, and BAAS platform.

**Folder: Products**

|Board|Purpose|Key Columns|
|---|---|---|
|Product Roadmap|High-level feature planning across all D42 products|Product, Feature, Priority, Status, Quarter, Owner (agent name)|
|Active Development|Current sprint/build items|Task, Product, Status, Assigned To, Due Date, Effort (S/M/L), Blocked By|
|Bug Tracker|Reported issues across all products|Bug Title, Product, Severity, Status, Reported By, Reproduction Steps|
|Release Log|Track what shipped and when|Version, Product, Release Date, Changelog, Deployed To|

**Folder: Content**

|Board|Purpose|Key Columns|
|---|---|---|
|YouTube Pipeline (D42)|All D42 faceless channel content|Video Title, Status (Idea → Script → Record → Edit → Upload → Published), Topic Pillar, Script Link, Thumbnail, Publish Date, Views, Revenue|
|Blog Pipeline|dimension42.ai blog posts|Title, Status, Author (you or AI), Topic, Draft Link, Publish Date, Platform (D42 blog, Dev.to, Medium, Hashnode)|
|Social Content Calendar|Scheduled posts across X, LinkedIn, Reddit|Platform, Content, Scheduled Date, Status, Link, Engagement|

**Folder: BAAS Platform**

|Board|Purpose|Key Columns|
|---|---|---|
|Agent Development|Track each AI agent being built|Agent Name, Role, Status, Model Assignment, SOUL.md Link, Last Updated|
|BAAS Roadmap|Track A and Track B phases from your blueprint|Phase, Track (A/B), Status, Dependencies, Target Date, Blockers|
|Org Chart Tracker|Map the D42 agent org structure|Agent Name, Division, Reports To, Layer, Status (Active/Planned/Stub)|

**Folder: D42 Website**

|Board|Purpose|Key Columns|
|---|---|---|
|Website Tasks|dimension42.ai build and maintenance|Task, Page/Section, Status, Priority, Due Date|

---

### Workspace 2: Marty Sampson

Everything related to the personal brand, content, consulting, and speaking.

**Folder: Content**

|Board|Purpose|Key Columns|
|---|---|---|
|YouTube Pipeline (Marty)|Hybrid channel content|Video Title, Status (Idea → Script → Record → Edit → Upload → Published), Type (Face/AI/Hybrid), Topic Pillar, Script Link, Thumbnail, Publish Date|
|Substack Pipeline|Newsletter planning and tracking|Title, Status (Idea → Draft → Edit → Publish), Topic, Publish Date, Subscriber Count at Publish|
|Social Content Calendar|X, LinkedIn, Instagram, Reddit|Platform, Content, Scheduled Date, Status, Link|
|Content Ideas Backlog|Dumping ground for future content ideas|Idea, Source (conversation, shower thought, news), Brand (Marty/D42/Either), Priority, Notes|

**Folder: Consulting**

|Board|Purpose|Key Columns|
|---|---|---|
|Consulting Pipeline|Track leads and active engagements|Client/Lead Name, Source (YouTube, LinkedIn, referral, cold), Status (Lead → Discovery → Proposal → Active → Complete), Engagement Type, Value, Start Date, End Date, Notes|
|Consulting Deliverables|Track deliverables for active clients|Client, Deliverable, Status, Due Date, Invoice Status|

**Folder: Speaking**

|Board|Purpose|Key Columns|
|---|---|---|
|Speaking Engagements|Track opportunities and booked events|Event Name, Organizer, Date, Location, Status (Inquiry → Negotiating → Booked → Prep → Delivered), Topic, Fee, Travel Required, Notes|
|Speaker Kit|Track your bio, headshots, sizzle reel versions|Asset, Current Version, Last Updated, File Link|

**Folder: Website**

|Board|Purpose|Key Columns|
|---|---|---|
|martysampson.com Tasks|Site build and maintenance|Task, Section, Status, Priority|
|Gated Content Tracker|Track private/gated URLs and who has access|Content Title, Token/URL, Shared With, Access Granted Date, Expires, Status|

---

### Workspace 3: 37Metrics

The holding company. Finance, legal, and admin.

**Folder: Finance**

|Board|Purpose|Key Columns|
|---|---|---|
|Revenue Tracker|All income streams|Source (D42 Product, YouTube D42, YouTube Marty, Consulting, Substack, Affiliate), Month, Amount, Status (Earned/Invoiced/Received), Notes|
|Expense Tracker|Business expenses|Vendor, Category (SaaS, hosting, API costs, hardware, marketing), Amount, Date, Recurring (Y/N), Notes|
|Subscription Manager|All active subscriptions|Service, Cost, Billing Cycle, Payment Method, Renewal Date, Cancel By Date, Category, Brand (D42/Marty/Shared)|
|Tax Prep|Quarterly and annual tax items|Item, Category, Quarter, Amount, Document Link, CPA Status|
|LLM Cost Tracker|API spend across providers|Provider, Model, Month, Tokens Used, Cost, Project/Brand|

**Folder: Legal**

|Board|Purpose|Key Columns|
|---|---|---|
|Entity Management|LLC status, DBAs, registrations|Entity/Filing, State, Status, Filed Date, Renewal Date, Document Link|
|Trademarks|Track filings and status|Mark, Class, Status (Search → File → Pending → Registered), Filing Date, Serial Number|
|Contracts|Active agreements|Party, Type (NDA, SOW, License), Status, Start Date, End Date, Document Link|

**Folder: Admin**

|Board|Purpose|Key Columns|
|---|---|---|
|Domain Manager|All owned domains|Domain, Registrar, Expiration Date, Auto-Renew (Y/N), DNS Provider, Purpose|
|Account Registry|Master list of all accounts from the lockdown checklist|Platform, Handle, Email Used, Brand (D42/Marty/37M), 2FA Enabled, Bitwarden Entry, Status|

---

### Workspace 4: Forge42 (Infrastructure)

The operational command center for all technical infrastructure.

**Folder: Servers and Services**

|Board|Purpose|Key Columns|
|---|---|---|
|Machine Inventory|All hardware in the fleet|Machine Name, Role, CPU, RAM, GPU, Storage, OS, Location, Status, IP/Tailscale Address, Last Inventoried|
|Docker Services|All running containers|Service Name, Machine, Port, Image, Status, Depends On, Notes|
|Service Health Log|Track outages and issues|Service, Date, Issue, Resolution, Downtime Duration|

**Folder: AI Stack**

|Board|Purpose|Key Columns|
|---|---|---|
|Model Catalog|All models available through LiteLLM|Logical Name, Backend, Context Window, Cost (in/out), Use Case, Status|
|Tool Evaluation|AI tools being tested or considered|Tool Name, Category, Status (Researching → Testing → Adopted → Rejected), Cost, Notes, Verdict|
|OpenClaw Development|Platform build tasks|Task, Phase, Status, Priority, Blocked By, Notes|

**Folder: Network and Security**

|Board|Purpose|Key Columns|
|---|---|---|
|Tailscale Inventory|Machines on the mesh|Machine, Tailscale IP, MagicDNS Name, Status, Last Seen|
|Security Checklist|Regular security review items|Item, Category, Frequency, Last Completed, Next Due, Status|

---

### Workspace 5: Personal

Non-business, non-brand personal life management.

**Folder: Health (IronMarty)**

|Board|Purpose|Key Columns|
|---|---|---|
|Health Dashboard|Weekly stats and progress|Week, Weight, Gym Sessions, Cardio Minutes, Diet Compliance, Sleep Score (Oura), Notes|
|Supplements and Meds|Current protocol tracking|Item, Dose, Timing, Purpose, Started Date, Status (Active/Paused/Stopped), Notes|
|Lab Results|Track bloodwork and tests|Test, Date, Key Values, Doctor Notes, Follow-up Needed|
|Gym Log|Workout tracking|Date, Type (Strength/Cardio/Peloton), Duration, Notes|

**Folder: Life Admin**

|Board|Purpose|Key Columns|
|---|---|---|
|Vehicle Maintenance|Lexus RX500h service tracking|Service Type, Date, Mileage, Cost, Shop, Notes|
|Home Projects|House maintenance and improvements|Project, Status, Priority, Budget, Notes|
|Reading List|Books to read and books finished|Title, Author, Status (Queue → Reading → Finished → Abandoned), Rating, Format (Kindle/Audible/Physical), Notes|
|Guitar Practice|Track practice and learning|Date, Duration, What I Worked On, Notes|
|Wish List|Things you want to buy|Item, Category, Priority, Estimated Cost, Link, Purchased (Y/N)|

**Folder: Family**

|Board|Purpose|Key Columns|
|---|---|---|
|Family Contacts|Key info for family members|Name, Relationship, Birthday, Phone, Email, Address, Notes|
|Dad (BigDog)|Track health concerns, visits, tech help needed|Item, Category, Date, Status, Notes|

---

### Monday.com Dashboard Setup

Create one master dashboard per workspace that pulls from all boards in that workspace. Then create one "Command Center" dashboard at the top level that shows:

- Overdue items across all workspaces
- This week's deadlines across all workspaces
- Revenue this month (from Finance tracker)
- Content published this week (from both YouTube pipelines)
- Active consulting engagements
- Service health (from Forge42)

---

## Obsidian

### One Vault, Not Multiple

Use a single Obsidian vault called **`forge42-vault`** (or just **`vault`**). Here's why:

- One vault means cross-linking works everywhere. A note in your D42 product docs can link to an infrastructure note, which can link to a personal health note. This is how your brain actually works. Multiple vaults break the graph.
- One vault means one search. You don't have to remember which vault something is in.
- One vault means one sync config, one plugin set, one theme.
- Privacy is handled by folders, not by vaults. You can .gitignore sensitive folders if you ever sync to GitHub.

### Vault Folder Structure

```
forge42-vault/
│
├── 00-Inbox/                          ← Quick capture, unsorted notes
│   └── (dump anything here, sort later)
│
├── 01-Daily/                          ← Daily notes (YYYY-MM-DD.md)
│   ├── 2026-04-04.md
│   ├── 2026-04-05.md
│   └── ...
│
├── 02-Templates/                      ← Note templates
│   ├── daily-note.md
│   ├── meeting-note.md
│   ├── project-brief.md
│   ├── content-idea.md
│   ├── tool-evaluation.md
│   ├── consulting-engagement.md
│   └── weekly-review.md
│
├── 10-Dimension42/                    ← D42 brand, products, BAAS
│   ├── Products/
│   │   ├── [product-name]/
│   │   │   ├── PRD.md                 (product requirements)
│   │   │   ├── architecture.md
│   │   │   ├── decisions.md           (decision log)
│   │   │   └── notes/
│   │   └── product-ideas.md
│   ├── BAAS/
│   │   ├── baas-blueprint.md          (your existing doc)
│   │   ├── agent-roster.md
│   │   ├── org-structure.md
│   │   └── phases/
│   │       ├── track-a.md
│   │       └── track-b.md
│   ├── Content/
│   │   ├── youtube-ideas.md
│   │   ├── blog-drafts/
│   │   └── scripts/
│   ├── Brand/
│   │   ├── naming-strategy.md         (the doc we built)
│   │   ├── brand-guidelines.md
│   │   ├── handle-registry.md
│   │   └── lockdown-checklist.md      (the checklist we built)
│   └── Website/
│       ├── sitemap.md
│       └── copy-drafts/
│
├── 20-MartySampson/                   ← Personal brand
│   ├── Content/
│   │   ├── youtube-ideas.md
│   │   ├── substack-drafts/
│   │   ├── scripts/
│   │   └── social-posts/
│   ├── Consulting/
│   │   ├── services-offered.md
│   │   ├── pricing.md
│   │   ├── clients/
│   │   │   └── [client-name].md
│   │   └── proposals/
│   ├── Speaking/
│   │   ├── speaker-bio.md
│   │   ├── talk-topics.md
│   │   └── events/
│   ├── Website/
│   │   ├── sitemap.md
│   │   ├── about-page-draft.md
│   │   └── gated-content-plan.md
│   └── zero-human-narrative.md        ← The story, talking points, milestones
│
├── 30-37Metrics/                      ← Holding company
│   ├── Finance/
│   │   ├── budget-2026.md
│   │   ├── revenue-model.md
│   │   └── tax-notes/
│   ├── Legal/
│   │   ├── llc-info.md
│   │   ├── dba-filings.md
│   │   ├── trademark-research.md
│   │   └── contracts/
│   └── Admin/
│       ├── domain-inventory.md
│       ├── subscription-audit.md
│       └── account-registry.md
│
├── 40-Forge42/                        ← Infrastructure and technical
│   ├── Architecture/
│   │   ├── architecture.md            (your existing doc)
│   │   ├── ai-orchestration.md        (your existing doc)
│   │   ├── ai-org-structure.md        (your existing doc)
│   │   └── luna-plan.md               (your existing doc)
│   ├── Eddie/
│   │   ├── eddie-soul.md              (SOUL.md)
│   │   └── eddie-notes.md
│   ├── OpenClaw/
│   │   ├── openclaw-docs.md
│   │   └── development-log.md
│   ├── Stack/
│   │   ├── my-stack.md                (your existing doc)
│   │   ├── docker-services.md
│   │   ├── litellm-config.md
│   │   ├── model-catalog.md
│   │   └── fastmail.md                (your existing doc)
│   ├── Machines/
│   │   ├── machine-inventory.md
│   │   └── tailscale-map.md
│   ├── Tools/
│   │   ├── tool-evaluations/
│   │   │   ├── [tool-name].md
│   │   │   └── ...
│   │   └── tool-comparison-matrix.md
│   └── Runbooks/
│       ├── docker-commands.md
│       ├── backup-recovery.md
│       └── incident-response.md
│
├── 50-Personal/                       ← Non-business personal
│   ├── Health/
│   │   ├── ironmarty-overview.md
│   │   ├── current-protocol.md
│   │   ├── supplements.md
│   │   ├── lab-results/
│   │   │   ├── 2026-01-bloodwork.md
│   │   │   └── ...
│   │   └── weekly-checkins/
│   ├── Family/
│   │   ├── dad-bigdog.md
│   │   ├── mom-debbie.md
│   │   └── uncle-blaine.md
│   ├── Interests/
│   │   ├── guitar/
│   │   │   ├── practice-log.md
│   │   │   └── gear.md
│   │   ├── music/
│   │   │   └── favorites.md
│   │   ├── books/
│   │   │   ├── reading-list.md
│   │   │   └── book-notes/
│   │   ├── comedy/
│   │   │   └── comedians.md
│   │   └── motorcycles/
│   │       └── history.md
│   ├── Vehicles/
│   │   └── lexus-rx500h.md
│   ├── Career/
│   │   ├── resume.md
│   │   ├── career-history.md
│   │   └── marty-profile.md           (your existing doc)
│   └── Worldview/
│       ├── pirate-founder.md
│       ├── values.md
│       └── conspiracy-fun.md
│
├── 60-Knowledge/                      ← Reference material, research
│   ├── AI/
│   │   ├── model-comparison-notes.md
│   │   ├── prompt-engineering.md
│   │   ├── agent-frameworks.md
│   │   └── research-papers/
│   ├── Business/
│   │   ├── pricing-strategies.md
│   │   ├── saas-metrics.md
│   │   └── youtube-monetization.md
│   ├── Tech/
│   │   ├── docker-notes.md
│   │   ├── terraform-notes.md
│   │   ├── tailscale-notes.md
│   │   └── self-hosting-notes.md
│   ├── Physics/
│   │   └── (whatever you want here)
│   └── Clips/
│       └── (web clips, quotes, saved articles)
│
├── 70-Archive/                        ← Completed projects, old notes
│   ├── 2026-Q1/
│   └── ...
│
└── 90-Meta/                           ← Vault management
    ├── vault-readme.md
    ├── tag-taxonomy.md
    ├── folder-conventions.md
    └── plugin-config-notes.md
```

### Why This Numbering System

The two-digit prefix (00, 10, 20, etc.) forces the folders into a specific order in Obsidian's file explorer. Without them, folders sort alphabetically and "Archive" sits at the top while "Templates" is buried. The numbering creates a logical flow:

- **00-09:** Transient (inbox, daily notes, templates)
- **10-29:** Brands and businesses (D42, Marty, 37Metrics)
- **30-49:** Operations and infrastructure (37Metrics admin, Forge42 tech)
- **50-59:** Personal life
- **60-69:** Reference knowledge base
- **70-79:** Archive
- **90-99:** Meta/vault management

The gaps between numbers (10, 20, 30 instead of 1, 2, 3) leave room to insert new top-level sections later without renumbering everything.

### Obsidian Tags

Use tags to create cross-cutting views that transcend the folder structure. Standardize these from day one:

**Status tags:**

- `#status/active`
- `#status/paused`
- `#status/complete`
- `#status/abandoned`
- `#status/idea`

**Brand tags:**

- `#brand/d42`
- `#brand/marty`
- `#brand/37m`
- `#brand/forge42`
- `#brand/personal`

**Type tags:**

- `#type/decision`
- `#type/meeting`
- `#type/research`
- `#type/draft`
- `#type/reference`
- `#type/runbook`
- `#type/evaluation`

**Priority tags:**

- `#priority/critical`
- `#priority/high`
- `#priority/medium`
- `#priority/low`

This lets you do things like search for all `#brand/d42 #type/decision` to see every decision made about Dimension42, regardless of which folder the note lives in.

### Obsidian Plugins to Install

These are the essential plugins for this vault structure:

- **Templater** — For consistent note creation from templates
- **Calendar** — Visual calendar tied to daily notes
- **Dataview** — Query your notes like a database (critical for dashboards)
- **Tasks** — Track to-do items across all notes
- **Periodic Notes** — Daily, weekly, monthly note generation
- **Quick Add** — Fast note creation with templates
- **Tag Wrangler** — Rename and manage tags in bulk
- **Obsidian Git** — Auto-commit vault to a private GitHub repo for backup
- **Kanban** — Board view for content pipelines (optional, Monday handles this too)

### Daily Note Template

```markdown
---
date: {{date:YYYY-MM-DD}}
day: {{date:dddd}}
tags: #type/daily
---

# {{date:dddd, MMMM D, YYYY}}

## Focus Today
- [ ] 

## Notes


## Dimension42


## Marty Sampson


## Forge42


## Personal


## End of Day
### What got done:

### What's carrying over:

### Random thoughts:
```

### How Monday and Obsidian Work Together

They serve different purposes. Do not try to make them do the same thing.

**Monday.com is for:**

- Task tracking with status, dates, and assignments
- Pipeline management (content, consulting, development)
- Dashboards with real-time status
- Anything where you need to see "what's overdue" at a glance
- Recurring operational workflows
- Anything you'd want to view as a Kanban board or Gantt chart

**Obsidian is for:**

- Long-form thinking and writing
- Documentation and architecture decisions
- Research notes and knowledge capture
- Drafts (scripts, blog posts, proposals)
- Personal journaling and reflection
- Permanent reference material
- Anything you'd want to search full-text six months from now

**The handoff pattern:** An idea starts in Obsidian (Inbox or Daily Note). If it becomes actionable, it gets a Monday item. The Monday item links to the Obsidian note where the deep thinking lives. The Monday board tracks progress. Obsidian holds the substance.

Example: You think of a YouTube video idea in the shower. You open Obsidian on your phone and dump it in `00-Inbox`. On Sunday during your weekly review, you move it to `10-Dimension42/Content/youtube-ideas.md` and flesh it out. Then you create a Monday item in the D42 YouTube Pipeline board, set the status to "Idea," and paste the Obsidian note link in the item's notes column. When it moves to "Script," you write the script in Obsidian. When it moves to "Published," the Monday board tracks the metrics. Obsidian still has the script and the original idea for reference.