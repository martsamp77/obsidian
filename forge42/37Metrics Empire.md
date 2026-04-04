# 37Metrics Empire: Naming, Branding, and Organizational Strategy

## The Corporate Hierarchy

```
37Metrics, LLC (Holding Company / Legal Entity)
├── 37metrics.com — LLC operations, contracts, invoicing, DBA registrations
├── Office 365 email stays as-is
│
├── DBA: Dimension42 (d/b/a 37Metrics, LLC)
│   ├── dimension42.ai — Public tech brand, AI content, software products
│   ├── YouTube: AI/tech faceless channel
│   ├── Software products sold under D42 brand
│   └── BAAS platform (when ready)
│
└── Personal Brand: Marty Sampson
    ├── martysampson.com — Personal site, gated content, portfolio
    ├── YouTube: Hybrid channel (face + AI produced)
    ├── Substack, social media presence
    └── "Zero Human" narrative lives here
```

**Why this structure works:** 37Metrics is the legal shield. You file a DBA for "Dimension42" under 37Metrics so D42 can operate publicly without needing its own LLC. If D42 ever gets big enough to warrant its own entity (investment, liability separation, or sale), you spin it off then. Marty Sampson is the personal brand that ties everything together and tells the story. All three brands are complementary but serve different audiences.

**When 37Metrics shows up publicly:** Invoices, contracts, legal notices, terms of service on software products ("Dimension42 is a product of 37Metrics, LLC"), tax filings. It does not need its own YouTube channel, social media presence, or content strategy. It's the holding company. It shows up in footers and legal pages, not in headlines.

## The Naming Convention

### The System

Your naming convention has three tiers that map to visibility:

**Tier 1: Public / Customer-Facing** — Conventional, marketable names. These are software products, services, and content channels that need to appeal to people who don't care about your infrastructure. They should be easy to say, easy to spell, easy to Google. The metal/physics theme can inform the aesthetic and personality but shouldn't make the name impenetrable.

**Tier 2: Community / Builder-Facing** — Metal and physics themed names. These are the projects, tools, and repos that your technical audience (YouTube viewers, GitHub followers, fellow builders) will see and interact with. The naming convention should be consistent, recognizable, and signal that it's part of the D42 ecosystem.

**Tier 3: Internal / Infrastructure** — Functional names. Machines, services, containers, internal tools. No branding needed. Named for what they do or where they are, not for personality.

### Tier 1: Public Product Names

When you build a SaaS product or a public-facing tool, give it a clean name that works in a sentence: "I use [Product] to manage my AI agents." Don't force the metal/physics theme here. However, the product can live under the Dimension42 umbrella, which gives it the sci-fi/physics flavor without being obscure.

Examples of the naming style that works:

- dimension42.ai/[productname]
- A name that's 1 to 2 syllables, easy to spell
- Can reference physics concepts loosely (Flux, Pulse, Signal, Lattice) but should prioritize clarity

Before naming any product, check availability across GitHub, npm, YouTube, X, and domain registration. If you can't get the name clean on at least GitHub and X, pick a different name.

### Tier 2: Community Projects (The Forge Namespace)

Here's the key insight: you already have "forge" embedded in your infrastructure. Your FastMail identity is forge42. Your dashboard is IronForge. The forge metaphor (a place where things are built, shaped, hardened) is more ownable and more specific than "Iron" as a generic prefix.

**Recommendation: Adopt "Forge" as your internal project namespace instead of "Iron."**

The "Iron" prefix has two problems. First, "IronForge" is heavily associated with World of Warcraft (the dwarven capital city) and there are existing companies using that name. Second, "Iron" as a prefix is generic and widely used (IronMQ, IronWorker, IronPython, IronOCR). You'll constantly collide with existing projects.

"Forge42" or just "Forge" as a namespace prefix is much more defensible because you already own the identity on FastMail, the number 42 makes it distinctive, and the metaphor fits perfectly (you're forging businesses, forging tools, forging agents).

**Proposed Tier 2 naming convention:**

|Current Name|Proposed Name|What It Is|
|---|---|---|
|IronForge|Forge42 Dashboard|The infrastructure dashboard on your home server|
|IronClaw|ForgeEddie or just Eddie|The OpenClaw orchestration agent|
|IronMarty|ForgeHealth or keep IronMarty|Personal health tracking project|
|OpenClaw|OpenClaw (keep as-is)|The orchestration platform (community/open source name)|
|Eddie/Ed|Eddie (keep as-is)|The AI agent persona|

Actually, there's a case for keeping "IronMarty" as-is since it's personal and not part of the D42 ecosystem. It's your health project. The "Iron" prefix works there because it's literally about iron (as in pumping iron, iron will, Iron Maiden). IronMarty is a standalone brand within the Marty Sampson personal ecosystem.

**Revised recommendation:**

- **Forge42** — The namespace for infrastructure tools, dashboards, and internal systems
- **IronMarty** — Stays. It's personal. It's on-brand for health/fitness. It's not customer-facing.
- **Eddie** — Stays. No prefix needed. He's a persona, not a product.
- **OpenClaw** — Stays. It's the open-source orchestration framework name.

### Tier 3: Machine and Infrastructure Names

You said you're moving away from machine-specific naming. Good. Here's the approach:

**Machines get functional names, not personality names.** Instead of LUNA, AURORA, Nova, just use names that describe the role:

- `hub` or `core` — Primary orchestration server (whatever replaces LUNA's role)
- `gpu-1`, `gpu-2` — GPU compute nodes
- `dev` — Development machine
- `nas-1` — Storage

Or, if you want to keep some personality without being locked to it, use a simple themed set that you rotate. Since you like physics, use particle names: Muon, Gluon, Lepton, Quark, Boson. Short, distinctive, easy to type in SSH configs. But the key rule is: no project documentation should ever reference a machine name as if it's permanent. Always reference by role. "The orchestration server" not "LUNA."

**For Docker services and containers:** Name by function. `litellm`, `ollama`, `redis`, `postgres`, `openclaw`. This is already what you're doing. Keep it.

## Domain Assignments

### 37metrics.com

- **Purpose:** Legal/corporate identity
- **Content:** Simple landing page with LLC info, links to D42 and Marty Sampson
- **Email:** Stays on Office 365 as-is. Do not move to FastMail.
- **Subpages:** /about, /legal, /privacy, /terms
- **Who sees it:** Clients, partners, accountants, lawyers
- **Build priority:** Low. A one-page site is fine for now.

### dimension42.ai

- **Purpose:** The tech brand. AI content hub. Software product home.
- **Content:** Product pages, blog (technical), documentation, pricing
- **Email:** Catch-all wildcard on FastMail (*@dimension42.ai). Agent personas, product support, and outbound marketing all send from this domain.
- **Subpages:** /blog, /products, /docs, /about, /support
- **Who sees it:** Developers, builders, YouTube audience, potential customers
- **Build priority:** High. This is where revenue lives.

### martysampson.com

- **Purpose:** Personal brand hub
- **Content:** About page, links to all channels, gated content sections, blog/essays
- **Email:** Can route through FastMail if you want, or keep martysampson@gmail.com as the public contact. Don't fight the Gmail SSO problem. Let it be.
- **Subpages:** /about, /links (Linktree replacement), /private (gated sections with custom URL tokens), /blog
- **Who sees it:** Everyone who discovers you through content, social media, or word of mouth
- **Build priority:** Medium. Get the basics up, then iterate on the gated content system.
- **Gated content system:** Build a simple token-based access system. Generate unique URLs like martysampson.com/private/[token]. Each token maps to a set of content the viewer is allowed to see. No login required, just a unique URL you share with specific people. Revocable. You can build this with a simple database lookup and middleware.

## FastMail Integration with Dimension42

Your FastMail architecture is already excellent. Here's how D42 fits in:

1. **Add dimension42.ai as a custom domain in FastMail** (Settings → My Email Addresses)
2. **Enable catch-all wildcard** (*@dimension42.ai) — this is already in your plan
3. **Create purpose-built sending identities:**
    - hello@dimension42.ai — General contact, about page
    - support@dimension42.ai — Product support
    - eddie@dimension42.ai — Eddie persona outbound
    - no-reply@dimension42.ai — Automated notifications
    - [agentname]@dimension42.ai — Any BAAS agent persona
4. **Folder routing:** Create a D42 folder in FastMail. Route *@dimension42.ai catch-all to it.
5. **Keep 37metrics.com on Office 365.** Don't touch it. It works, it gives you Office, and it's your legal entity email.

## Social Media Handle Strategy

### Handles to Lock Down Immediately

You need to grab these across all platforms before someone else does. Even if you won't use them all right away, squat on them.

**For Dimension42 brand:**

|Platform|Handle|Notes|
|---|---|---|
|YouTube|@dimension42ai|The ".ai" suffix avoids collision with Dimension 20 and Level 42|
|X (Twitter)|@dimension42ai|Consistent with YouTube|
|GitHub|dimension42ai|Under 37Metrics org, or standalone org|
|Reddit|u/dimension42ai|For posting in relevant subreddits|
|Substack|dimension42ai.substack.com|If you want a D42-specific newsletter|
|Instagram|@dimension42ai|For visual content, infographics|
|TikTok|@dimension42ai|Repurpose YouTube Shorts|
|Bluesky|@dimension42.ai|Custom domain handle (Bluesky supports this)|
|Threads|@dimension42ai|Meta's platform, grab it|
|LinkedIn|/company/dimension42ai|Company page|

**For Marty Sampson brand:**

|Platform|Handle|Notes|
|---|---|---|
|YouTube|@martysampson (check availability)|May need @martylsampson or @martysampson77|
|X (Twitter)|@martysampson (if you have it, great)|Lock it down|
|Substack|martysampson.substack.com|Personal newsletter|
|Reddit|u/martysampson (if available)|Or u/martsamp77 to match GitHub|
|Instagram|@martysampson|Personal brand visuals|
|LinkedIn|Your existing personal profile|Link to martysampson.com|

**For 37Metrics (minimal):**

|Platform|Handle|
|---|---|
|GitHub|37metrics (already have)|
|LinkedIn|/company/37metrics|
|X|@37metrics (grab defensively)|

### Handle Naming Rules

1. **Always use the same handle across every platform for a given brand.** dimension42ai everywhere. Not dimension42 on one and d42ai on another.
2. **If the exact handle is taken, append "ai" or "hq" consistently.** Don't mix suffixes.
3. **Register the handle even if you won't post for months.** Put up a profile picture and a one-line bio. That's enough to hold it.

## YouTube Strategy

### Channel 1: Marty Sampson (Hybrid)

- **Handle:** @martysampson (or closest available)
- **Content mix:** 70% AI-produced, 30% you on camera
- **Your face appears in:** Intros, commentary segments, opinion pieces, behind-the-scenes of building the zero-human empire, vlogs from the studio
- **AI-produced content:** Tutorials, explainers, news roundups, deep dives into AI tools
- **Core narrative:** "I'm one person building a software company with zero employees using AI. Here's how."
- **Why this works:** Your face establishes you as a real person with editorial authority. YouTube's 2026 algorithms specifically look for a "human creative fingerprint" on channels. Your face-on-camera segments protect the AI-produced content from being flagged as inauthentic. The hybrid approach also wins in sponsorship deals because advertisers see a human anchor.
- **Content pillars:** Zero-human company building, AI tool reviews, personal journey/progress updates, technical tutorials (hosting, stacks, agents)
- **Posting cadence:** 1 to 2 long-form per week, 3 to 5 Shorts per week (repurposed clips)

### Channel 2: Dimension42 (Faceless)

- **Handle:** @dimension42ai
- **Content mix:** 100% AI-produced (with your editorial direction)
- **No face. No personal brand.** This is a media property.
- **Content type:** Deep technical explainers, "How to build X with AI" tutorials, infrastructure walkthroughs, tool comparisons, the BAAS platform showcase
- **Voice:** Use a consistent AI voice. Pick one and make it the channel's identity. Clone your own voice if you want (YouTube allows this without disclosure requirements as of 2026).
- **Why separate:** Faceless channels scale differently. You can produce 3 to 5 videos per week without bottlenecking on your availability. The content is evergreen and search-driven. This channel is a content factory with a specific technical niche. It complements the Marty Sampson channel without competing with it.
- **Content pillars:** Self-hosting AI stacks, cheap model routing, building software with AI agents, zero-human infrastructure blueprints, tool and platform reviews
- **Posting cadence:** 2 to 3 long-form per week, daily Shorts

### Cross-promotion Strategy

- Marty Sampson channel references D42 naturally: "I built this using my Dimension42 platform..."
- D42 channel never references Marty by name. It's a standalone brand.
- Both channels link to dimension42.ai for products and resources
- Shorts from both channels get cross-posted to TikTok, Instagram Reels, and X

### YouTube Content Safety Guidelines for AI-Produced Videos

1. **Disclose AI-generated content** using YouTube's "altered content" setting during upload when the content could be mistaken for real events or real people
2. **Use a consistent AI voice** — this becomes your brand identity and differentiates you from mass-produced channels
3. **Never batch-upload** more than 2 to 3 videos per day. YouTube's 2026 algorithm flags channels that look like content factories
4. **Every video needs unique editorial direction** — original research, unique data, your own analysis. Don't just prompt-and-publish
5. **Write scripts with intention** — pacing, emphasis, rhythm. Treat AI voice like a voice actor you're directing
6. **Custom thumbnails** for every video. No templated thumbnails that look identical across videos

## The "Zero Human" Brand Narrative

This is your single most compelling differentiator and it should be central to the Marty Sampson personal brand. Here's how to deploy it:

**The story:** "I'm building a software company, a media company, and a consulting practice with zero employees. Every role is filled by an AI agent. I'm the only human. Here's what I'm learning."

**Where it lives:**

- martysampson.com — Featured prominently on the homepage
- Marty Sampson YouTube — This is the primary narrative arc of the channel
- Substack — Long-form essays about the journey, decisions, failures, insights
- X/Twitter — Short updates, milestones, behind-the-scenes screenshots
- LinkedIn — Professional credibility and consulting lead generation

**Where it does NOT live:**

- Dimension42 — D42 is a tech brand. It doesn't need to explain that it's run by one person. It just delivers value. The zero-human story is Marty's story, not D42's story.
- 37Metrics — Nobody cares about the holding company's staffing model.

**Content opportunities from this narrative:**

- "Month 1: I built an AI agent that writes my marketing copy. Here's what happened."
- "My AI CFO just flagged a budget overrun I missed."
- "Zero employees, $X revenue: What I've learned running a company with AI agents."
- "I gave my AI CEO a mandate. This is what it did."

This narrative feeds YouTube, Substack, X, LinkedIn, and potentially a book or paid course down the road. It's the gift that keeps giving because every milestone in your D42 build is a content piece for the Marty Sampson brand.

## GitHub Organization Strategy

### 37Metrics GitHub Org (already exists)

- **Purpose:** Private repos for LLC operations, contracts templates, internal tools
- **Visibility:** Mostly private repos
- **What lives here:** Anything that belongs to the legal entity

### Dimension42 GitHub Org (create: github.com/dimension42ai)

- **Purpose:** Public and private repos for all D42 projects
- **Visibility:** Mix of public (open source tools, tutorials) and private (products, BAAS)
- **What lives here:**
    - OpenClaw (if you open-source it)
    - Eddie's SOUL.md and agent configs (public, for community)
    - Software products (private)
    - Tutorial code from YouTube videos (public)
    - dimension42.ai website (private)
    - BAAS platform (private)

### martsamp77 (personal GitHub, already exists)

- **Purpose:** Personal repos, experiments, IronMarty
- **Visibility:** Mix
- **What lives here:** IronMarty health project, personal experiments, dotfiles, anything not D42 or 37Metrics

### Repo Naming Convention

- Use lowercase kebab-case: `openclaw`, `forge42-dashboard`, `d42-website`
- Prefix with `d42-` for Dimension42 repos that aren't standalone products
- Standalone products get their own clean name: `openclaw`, `[productname]`
- Tutorial repos from YouTube: `d42-tutorial-[topic]`

## Content Platform Map

|Platform|Marty Sampson|Dimension42|37Metrics|
|---|---|---|---|
|YouTube|Hybrid channel, personal brand|Faceless tech channel|None|
|Substack|Personal essays, journey updates|Technical newsletter (optional)|None|
|X|Personal updates, hot takes, engagement|Product announcements, tech threads|Defensive handle only|
|Reddit|Engage in AI/tech subreddits as yourself|Post tutorials, engage as D42|None|
|Instagram|Behind-the-scenes, studio, personal|Infographics, visual tutorials|None|
|LinkedIn|Professional profile, consulting leads|Company page|Company page|
|TikTok|Repurposed Shorts|Repurposed Shorts|None|
|Facebook|Personal, minimal effort|Page for D42 (optional)|None|
|Bluesky|Mirror X content|Mirror X content|None|

## Revenue Stream Mapping

|Revenue Stream|Brand|Domain|Priority|
|---|---|---|---|
|Software products (SaaS)|Dimension42|dimension42.ai|Primary|
|YouTube ad revenue (faceless)|Dimension42|@dimension42ai|High|
|YouTube ad revenue (personal)|Marty Sampson|@martysampson|Medium|
|Consulting / services|Marty Sampson + 37Metrics|martysampson.com, 37metrics.com|Medium|
|Substack subscriptions|Marty Sampson|Substack|Medium|
|Gated content / courses|Marty Sampson|martysampson.com/private|Future|
|BAAS platform (tenant fees)|Dimension42|dimension42.ai|Future|
|Affiliate / sponsorships|Both YouTube channels|Both|Passive|

## Immediate Action Items

### Week 1: Lock Down Handles

1. Register @dimension42ai on YouTube, X, Reddit, Instagram, TikTok, Bluesky, Threads
2. Confirm @martysampson handles on YouTube and X (you said you have these)
3. Create github.com/dimension42ai organization
4. Grab dimension42ai.substack.com and martysampson.substack.com

### Week 2: Domain Configuration

1. Add dimension42.ai to FastMail as a custom domain with catch-all wildcard
2. Set up hello@dimension42.ai, support@dimension42.ai, eddie@dimension42.ai
3. Create D42 folder in FastMail with routing rule for *@dimension42.ai
4. Set up basic DNS (MX records pointing to FastMail for dimension42.ai)

### Week 3: Placeholder Pages

1. 37metrics.com — Simple one-page site (logo, LLC info, links to D42 and Marty)
2. dimension42.ai — Landing page (coming soon, email signup, links to YouTube)
3. martysampson.com — Landing page (about, links to all channels, email contact)

### Week 4: Content Pipeline

1. Set up YouTube studio in extra bedroom
2. Record first Marty Sampson video: "I'm building a company with zero employees"
3. Produce first D42 faceless video: Technical tutorial on a topic from your stack
4. Start Substack with introductory post about the zero-human journey

## Naming Quick Reference Card

|Thing|Name|Why|
|---|---|---|
|Holding company (LLC)|37Metrics|Already exists, stays as-is|
|Tech/product brand|Dimension42|Owns domain, physics reference, marketable|
|Personal brand|Marty Sampson|Your name, your story|
|Orchestration platform|OpenClaw|Community-facing name, keep as-is|
|Primary AI agent|Eddie|Iron Maiden reference, has personality|
|Infrastructure dashboard|Forge42|Tied to forge42 email identity, "forge" metaphor|
|Health project|IronMarty|Personal, "iron" fits health/fitness context|
|Internal email identity|forge42|Already established in FastMail architecture|
|Machine names|Functional (hub, gpu-1, dev)|No personality, no lock-in|
|Public social handle|dimension42ai|Unique, available, consistent|
|Personal social handle|martysampson|Your name|

## Risks and Downsides

**Dimension42 name risks:**

- Level 42 is a real band (1980s jazz-funk) that also references Hitchhiker's Guide. Different industry, different era, no trademark conflict for software. But if you ever sell music-adjacent products, consult a trademark attorney.
- Dimension 20 is a popular YouTube channel (907K subs, tabletop RPG). Using "@dimension42ai" cleanly avoids confusion.
- The word "Dimension" is generic enough that you'll have SEO competition. Adding "42" and the .ai domain helps, but plan for "dimension42 ai" as your primary search keyword, not just "dimension42."

**Two YouTube channels risk:**

- Splitting your effort between two channels means slower growth on each compared to focusing on one. Mitigation: Build the Marty Sampson channel first (it's the story channel, it's easier to start, and your face gives it instant credibility). Launch D42 channel only after you have a content production pipeline that can sustain 2 or more videos per week without your direct involvement.
- YouTube may connect the two channels to the same person. This isn't a problem unless you're trying to hide the connection. Since D42 is a brand under your LLC, the connection is legitimate.

**"Zero Human" narrative risk:**

- If your software products have bugs, customers may blame the "zero human" model. Mitigation: The narrative is for the Marty Sampson brand and content. Products sold under D42 just need to work. You don't need to advertise that there's no support team on the product itself.
- The narrative only works if you're actually building things. If the content outpaces the execution, it becomes hollow. Ship first, talk second.

**Handle squatting risk:**

- Some of these handles may get reclaimed if you don't post within platform-specific timeframes. YouTube in particular may release unused handles. Mitigation: Post at least one piece of content on each platform within the first 30 days, even if it's just a placeholder.

---

_This is a living strategy document. Update it as decisions are made, handles are secured, and priorities shift._