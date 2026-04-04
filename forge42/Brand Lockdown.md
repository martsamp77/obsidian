# Brand Lockdown Master Checklist

## How to Use This Document

Work through each section in order. Mark items as you complete them. Some items depend on others (noted where applicable). Priority items are marked with [CRITICAL] and should be done first. Items marked [DEFENSIVE] are squat-to-hold, not active use.

---

## Section 1: Google Account Setup

Complete this section before anything else. The D42 Google account is the identity key for all D42 platform registrations.

- [ ] Create dimension42ai@gmail.com
- [ ] Set a strong unique password and store in Bitwarden
- [ ] Enable 2FA with hardware security key (YubiKey preferred)
- [ ] Generate and store backup/recovery codes in Bitwarden secure notes
- [ ] Set recovery email to forge42@fastmail.com (your primary, never shared)
- [ ] Set recovery phone to your mobile number
- [ ] Set profile display name to "Dimension42"
- [ ] Upload D42 logo/avatar as Google profile picture
- [ ] Set up Chrome Profile for D42 (separate from personal)
    - [ ] Sign into dimension42ai@gmail.com in the new Chrome profile
    - [ ] Set D42 logo as Chrome profile avatar
    - [ ] Install Bitwarden extension in D42 Chrome profile
    - [ ] Install uBlock Origin in D42 Chrome profile
    - [ ] Bookmark FastMail, GitHub, YouTube Studio, X in D42 profile
- [ ] Verify your personal Chrome profile is signed into martysampson@gmail.com
- [ ] Confirm you can switch profiles with two clicks (top-right avatar in Chrome)

---

## Section 2: Domain Verification and DNS

- [ ] Confirm ownership and renewal status of 37metrics.com
- [ ] Confirm ownership and renewal status of dimension42.ai
- [ ] Confirm ownership and renewal status of martysampson.com
- [ ] Enable auto-renew on all three domains
- [ ] Enable domain lock (registrar transfer protection) on all three domains
- [ ] Enable WHOIS privacy on all three domains
- [ ] Check domain expiration dates and calendar a reminder 90 days before each

### dimension42.ai DNS

- [ ] Add MX records pointing to FastMail (for email)
- [ ] Add SPF record for FastMail
- [ ] Add DKIM record for FastMail
- [ ] Add DMARC record
- [ ] Verify domain in FastMail (Settings → My Email Addresses)
- [ ] Enable catch-all wildcard (*@dimension42.ai) in FastMail
- [ ] Test receiving email at test@dimension42.ai

### martysampson.com DNS

- [ ] Decide if martysampson.com email will route through FastMail or stay Gmail
- [ ] If FastMail: Add MX, SPF, DKIM, DMARC records
- [ ] If FastMail: Verify domain in FastMail settings
- [ ] If FastMail: Create marty@martysampson.com sending identity
- [ ] If Gmail: Leave DNS as-is for email, just handle web hosting

### 37metrics.com DNS

- [ ] Confirm MX records point to Office 365 (leave as-is)
- [ ] Confirm SPF/DKIM/DMARC are configured for Office 365
- [ ] No changes needed, just verify everything is healthy

---

## Section 3: FastMail Configuration (D42 Integration)

Complete Section 2 DNS first.

### New Aliases and Sending Identities for dimension42.ai

- [ ] Create sending identity: hello@dimension42.ai
- [ ] Create sending identity: support@dimension42.ai
- [ ] Create sending identity: eddie@dimension42.ai
- [ ] Create sending identity: no-reply@dimension42.ai
- [ ] Create sending identity: marty@dimension42.ai
- [ ] Create sending identity: press@dimension42.ai
- [ ] Create sending identity: legal@dimension42.ai

### FastMail Folder and Rules

- [ ] Create "D42" folder in FastMail
- [ ] Create folder rule: Recipient address contains @dimension42.ai → Move to D42
- [ ] Test by sending email to randomtest@dimension42.ai from a personal account
- [ ] Confirm it arrives in the D42 folder

### Defensive Aliases for dimension42.ai

These prevent others from impersonating roles at your domain. Since you have catch-all enabled, these all work automatically. Just document them as "reserved."

- [ ] Document reserved: admin@dimension42.ai
- [ ] Document reserved: info@dimension42.ai
- [ ] Document reserved: contact@dimension42.ai
- [ ] Document reserved: billing@dimension42.ai
- [ ] Document reserved: sales@dimension42.ai
- [ ] Document reserved: security@dimension42.ai
- [ ] Document reserved: abuse@dimension42.ai
- [ ] Document reserved: postmaster@dimension42.ai
- [ ] Document reserved: webmaster@dimension42.ai
- [ ] Document reserved: ceo@dimension42.ai
- [ ] Document reserved: team@dimension42.ai
- [ ] Document reserved: newsletter@dimension42.ai
- [ ] Document reserved: api@dimension42.ai
- [ ] Document reserved: dev@dimension42.ai
- [ ] Document reserved: noreply@dimension42.ai (alternate spelling)

---

## Section 4: YouTube

### Dimension42 Channel

- [ ] [CRITICAL] Sign into YouTube with dimension42ai@gmail.com
- [ ] Create a new YouTube channel
- [ ] Set channel name to "Dimension42"
- [ ] Claim custom handle: @dimension42ai
- [ ] If @dimension42ai is taken, try: @dimension42hq, @d42ai
- [ ] Upload profile picture (D42 logo)
- [ ] Upload channel banner (placeholder is fine)
- [ ] Write channel description (1-2 paragraphs about AI/tech content)
- [ ] Add links: dimension42.ai, GitHub, X
- [ ] Set channel keywords: AI, artificial intelligence, self-hosting, AI agents, automation
- [ ] Set default upload visibility to Private (so drafts don't go live accidentally)
- [ ] Enable 2FA on the Google account if not already done

### Marty Sampson Channel

- [ ] Confirm you have access to your existing Marty Sampson YouTube
- [ ] Confirm the Google account that owns it (martysampson@gmail.com)
- [ ] Check your current handle, attempt to claim @martysampson if not already done
- [ ] If @martysampson is taken, try: @martylsampson, @martysampsonai, @martysampson77
- [ ] Update channel description to reflect the zero-human company narrative
- [ ] Add links: martysampson.com, dimension42.ai, GitHub, X
- [ ] Upload current profile picture and banner

---

## Section 5: X (Twitter)

### Dimension42 Account

- [ ] [CRITICAL] Go to x.com and create new account
- [ ] Sign up with dimension42ai@gmail.com (or x@dimension42.ai for FastMail tracking)
- [ ] Claim handle: @dimension42ai
- [ ] If taken, try: @dimension42hq, @d42ai, @dim42ai
- [ ] Set display name to "Dimension42"
- [ ] Upload profile picture (D42 logo)
- [ ] Upload header image (placeholder)
- [ ] Write bio: something about AI infrastructure, zero-human software, tech content
- [ ] Add website link: dimension42.ai
- [ ] Set location: Birmingham, AL (or leave blank for brand accounts)
- [ ] Enable 2FA
- [ ] Store credentials in Bitwarden
- [ ] Post one placeholder tweet so the account looks alive
- [ ] Follow 10-20 relevant AI/tech accounts so the feed populates

### Marty Sampson Account

- [ ] Confirm you have access to your existing X account
- [ ] Confirm handle (@martysampson or whatever you have)
- [ ] Update bio to reflect zero-human company builder narrative
- [ ] Add website link: martysampson.com
- [ ] Add profile picture (your face)
- [ ] Enable 2FA if not already done

### 37Metrics Account [DEFENSIVE]

- [ ] Create account with your Office 365 37Metrics email
- [ ] Claim handle: @37metrics
- [ ] If taken, try: @37metricsllc, @37metricshq
- [ ] Set display name to "37Metrics"
- [ ] Write minimal bio: "Software holding company. @dimension42ai"
- [ ] Post one placeholder tweet
- [ ] Lock it and move on

---

## Section 6: GitHub

### Dimension42 Organization

- [ ] [CRITICAL] Sign into GitHub as martsamp77
- [ ] Create new organization: dimension42ai
- [ ] If taken, try: dimension42-ai, dimension42hq
- [ ] Set organization display name to "Dimension42"
- [ ] Upload D42 logo as org avatar
- [ ] Write org description
- [ ] Add website: dimension42.ai
- [ ] Set billing email to dimension42ai@gmail.com
- [ ] Add your personal account (martsamp77) as owner
- [ ] Create initial repos:
    - [ ] dimension42ai/openclaw (or transfer from current location)
    - [ ] dimension42ai/d42-website (private)
    - [ ] dimension42ai/forge42-dashboard (private)
    - [ ] dimension42ai/eddie-soul (public, Eddie's SOUL.md and configs)

### 37Metrics Organization (already exists)

- [ ] Confirm github.com/37metrics is accessible
- [ ] Confirm your account has owner permissions
- [ ] Review repos and clean up anything stale
- [ ] Verify org avatar and description are current

### Personal Account

- [ ] Confirm github.com/martsamp77 is accessible
- [ ] Update bio to mention D42 and zero-human narrative
- [ ] Add website: martysampson.com
- [ ] Pin relevant repos to profile

---

## Section 7: Reddit

### Dimension42 Account

- [ ] Create Reddit account: u/dimension42ai
- [ ] If taken, try: u/dimension42_ai, u/d42ai
- [ ] Use dimension42ai@gmail.com for registration
- [ ] Set display name to "Dimension42"
- [ ] Upload D42 avatar
- [ ] Join relevant subreddits: r/selfhosted, r/artificial, r/LocalLLaMA, r/homelab, r/SideProject, r/AItools, r/MachineLearning
- [ ] Store credentials in Bitwarden
- [ ] Do not post immediately, just subscribe and lurk to build history

### Marty Sampson Account

- [ ] Check if you have an existing Reddit account
- [ ] If not, create: u/martysampson or u/martsamp77
- [ ] Use martysampson@gmail.com for registration
- [ ] Update profile with personal brand info

### Subreddit Squat [DEFENSIVE]

- [ ] Create r/dimension42 (if available) and set to restricted
- [ ] Create r/dimension42ai (if available) and set to restricted
- [ ] Create r/37metrics (if available) and set to restricted
- [ ] You can build these into communities later, just hold the names

---

## Section 8: Instagram

### Dimension42 Account

- [ ] Create Instagram account: @dimension42ai
- [ ] Use dimension42ai@gmail.com for registration
- [ ] Set display name to "Dimension42"
- [ ] Upload D42 logo as profile picture
- [ ] Write bio about AI/tech content
- [ ] Add link: dimension42.ai
- [ ] Enable 2FA
- [ ] Store credentials in Bitwarden
- [ ] Post one placeholder image (logo, coming soon graphic)

### Marty Sampson Account

- [ ] Check if @martysampson is available or if you already have it
- [ ] If creating new: use martysampson@gmail.com
- [ ] Upload your face as profile picture
- [ ] Write bio about zero-human company builder
- [ ] Add link: martysampson.com

---

## Section 9: TikTok

### Dimension42 Account

- [ ] Create TikTok account: @dimension42ai
- [ ] Use dimension42ai@gmail.com for registration
- [ ] Set display name to "Dimension42"
- [ ] Upload D42 logo
- [ ] Write bio
- [ ] Add link: dimension42.ai
- [ ] Store credentials in Bitwarden

### Marty Sampson Account

- [ ] Create or claim: @martysampson
- [ ] Use martysampson@gmail.com
- [ ] Upload face photo
- [ ] Write bio
- [ ] Add link: martysampson.com

---

## Section 10: LinkedIn

### Dimension42 Company Page

- [ ] Create LinkedIn Company Page: "Dimension42"
- [ ] Set URL slug: linkedin.com/company/dimension42ai
- [ ] If taken, try: dimension42-ai, dimension42hq
- [ ] Upload D42 logo
- [ ] Write company description
- [ ] Set website: dimension42.ai
- [ ] Set industry: Software Development or AI
- [ ] Set company size: 1 (you)
- [ ] Set headquarters: Birmingham, AL

### 37Metrics Company Page [DEFENSIVE]

- [ ] Create LinkedIn Company Page: "37Metrics"
- [ ] Set URL slug: linkedin.com/company/37metrics
- [ ] Upload logo or placeholder
- [ ] Minimal description: "Software holding company"
- [ ] Set website: 37metrics.com

### Personal Profile

- [ ] Update your LinkedIn headline to reference AI/zero-human narrative
- [ ] Add Dimension42 as current position (Founder)
- [ ] Add 37Metrics as current position (Owner)
- [ ] Update About section with the builder narrative
- [ ] Add all three websites to contact info
- [ ] Add martysampson.com/consulting link

---

## Section 11: Bluesky

### Dimension42 Account

- [ ] Create account at bsky.app
- [ ] Claim handle: @dimension42ai.bsky.social
- [ ] Then set custom domain handle: @dimension42.ai (Bluesky supports this via DNS TXT record)
- [ ] Add DNS TXT record: _atproto.dimension42.ai → did=did:plc:[your DID]
- [ ] Verify the custom handle works
- [ ] Upload D42 logo
- [ ] Write bio
- [ ] Store credentials in Bitwarden

### Marty Sampson Account

- [ ] Create account at bsky.app
- [ ] Claim handle: @martysampson.bsky.social
- [ ] Optionally set custom handle: @martysampson.com (same DNS TXT method)
- [ ] Upload face photo
- [ ] Write bio

---

## Section 12: Threads (Meta)

Threads accounts are tied to Instagram accounts.

- [ ] [CRITICAL] Create D42 Instagram account first (Section 8)
    
- [ ] Sign into Threads with D42 Instagram account
    
- [ ] Confirm handle matches: @dimension42ai
    
- [ ] Upload D42 logo
    
- [ ] Post one placeholder
    
- [ ] Sign into Threads with Marty Sampson Instagram account
    
- [ ] Confirm handle matches
    
- [ ] Post one placeholder
    

---

## Section 13: Facebook

### Dimension42 Page

- [ ] Create Facebook Page: "Dimension42"
- [ ] Claim vanity URL: facebook.com/dimension42ai
- [ ] Upload D42 logo as profile picture
- [ ] Upload cover photo (placeholder)
- [ ] Write About section
- [ ] Add website: dimension42.ai
- [ ] Set category: Software Company or Technology Company

### Marty Sampson Page (Optional)

- [ ] Decide if you want a separate creator/public figure page
- [ ] If yes: Create page, claim facebook.com/martysampsonofficial or similar
- [ ] If no: Use your personal Facebook profile for personal brand, add links to bio

### 37Metrics Page [DEFENSIVE]

- [ ] Create Facebook Page: "37Metrics"
- [ ] Claim vanity URL: facebook.com/37metrics
- [ ] Minimal info, just hold the name

---

## Section 14: Substack

### Dimension42 Newsletter

- [ ] Create Substack: dimension42ai.substack.com
- [ ] If taken, try: dimension42.substack.com, d42ai.substack.com
- [ ] Use dimension42ai@gmail.com for registration
- [ ] Set publication name to "Dimension42"
- [ ] Upload D42 logo
- [ ] Write publication description
- [ ] Set up custom domain later if desired: blog.dimension42.ai
- [ ] Store credentials in Bitwarden

### Marty Sampson Newsletter

- [ ] Create Substack: martysampson.substack.com
- [ ] Use martysampson@gmail.com for registration
- [ ] Set publication name to "Marty Sampson" (or a creative name)
- [ ] Upload face photo
- [ ] Write publication description about the zero-human journey

---

## Section 15: Podcast Platforms (Future, but Squat Now)

If you ever launch a podcast (highly likely given YouTube content), grab these names.

### Apple Podcasts

- [ ] Cannot pre-register. Requires an RSS feed. Skip for now, revisit when you have audio content.

### Spotify for Podcasters

- [ ] Create account at podcasters.spotify.com with dimension42ai@gmail.com
- [ ] Claim podcast name: "Dimension42"
- [ ] You don't need episodes to register. Just hold the name.
- [ ] Store credentials in Bitwarden

### Pocket Casts / Overcast / Castbox

- [ ] These pull from RSS. No pre-registration needed.

---

## Section 16: Developer and Tech Platforms

### npm (Node Package Manager)

- [ ] Create npm account: dimension42ai
- [ ] Use dimension42ai@gmail.com
- [ ] Create npm organization: @dimension42ai
- [ ] Store credentials in Bitwarden
- [ ] This matters if you publish any open-source JavaScript/TypeScript packages

### PyPI (Python Package Index)

- [ ] Create PyPI account: dimension42ai
- [ ] Use dimension42ai@gmail.com
- [ ] Enable 2FA
- [ ] Store credentials in Bitwarden
- [ ] This matters if you publish any Python packages

### Docker Hub

- [ ] Create Docker Hub account: dimension42ai
- [ ] Use dimension42ai@gmail.com
- [ ] Create organization: dimension42ai
- [ ] Enable 2FA
- [ ] Store credentials in Bitwarden
- [ ] This matters for publishing container images

### Cloudflare

- [ ] Create Cloudflare account with dimension42ai@gmail.com (or use existing if you have one)
- [ ] Add dimension42.ai to Cloudflare (for DNS management, CDN, DDoS protection)
- [ ] Add martysampson.com to Cloudflare
- [ ] Consider adding 37metrics.com to Cloudflare
- [ ] Enable Cloudflare Pages for static sites if desired

### Vercel

- [ ] Create Vercel account: link to dimension42ai GitHub org
- [ ] Use dimension42ai@gmail.com
- [ ] This is useful for deploying dimension42.ai and martysampson.com

### Netlify [DEFENSIVE]

- [ ] Create Netlify account with dimension42ai@gmail.com
- [ ] Hold the account even if you use Vercel. Free tier, no cost.

### HuggingFace

- [ ] Create HuggingFace account: dimension42ai
- [ ] Use dimension42ai@gmail.com
- [ ] This matters if you publish AI models or datasets
- [ ] Store credentials in Bitwarden

### Product Hunt

- [ ] Create Product Hunt account as Marty Sampson (personal)
- [ ] Create a Product Hunt "maker" profile
- [ ] You'll launch D42 products here eventually

### Hacker News (Y Combinator)

- [ ] Create account: dimension42ai (or similar)
- [ ] Use dimension42ai@gmail.com
- [ ] HN accounts are valuable for launching products and getting visibility
- [ ] Post sparingly and authentically. HN detects and punishes self-promotion.

### Dev.to

- [ ] Create account: @dimension42ai
- [ ] Use dimension42ai@gmail.com
- [ ] Cross-post technical blog content here for SEO

### Hashnode

- [ ] Create account: @dimension42ai
- [ ] Use dimension42ai@gmail.com
- [ ] Alternative tech blogging platform, free, good SEO
- [ ] Can map to blog.dimension42.ai as custom domain

### Medium

- [ ] Create account: @dimension42ai
- [ ] Use dimension42ai@gmail.com
- [ ] Good for reach but Medium owns the relationship with readers
- [ ] Use primarily for cross-posting, not as primary blog

---

## Section 17: Communication and Community Platforms

### Discord

- [ ] Create Discord account: dimension42ai
- [ ] Use dimension42ai@gmail.com
- [ ] Create Discord server: "Dimension42"
- [ ] Set up basic channels: #general, #announcements, #ai-tools, #self-hosting, #off-topic
- [ ] Set server invite link to something clean: discord.gg/dimension42 (if available)
- [ ] Don't promote yet. Just have it ready.
- [ ] Store credentials in Bitwarden

### Slack [DEFENSIVE]

- [ ] Create Slack workspace: dimension42ai.slack.com
- [ ] Use dimension42ai@gmail.com
- [ ] You may never use this, but holding the workspace name is free

### Telegram [DEFENSIVE]

- [ ] Consider creating a Telegram channel: @dimension42ai
- [ ] Useful for announcements, not required immediately

### Mastodon [DEFENSIVE]

- [ ] Create account on a tech-focused instance (e.g., mastodon.social or hachyderm.io)
- [ ] Claim: @dimension42ai@mastodon.social (or similar)
- [ ] Low priority but free to hold

---

## Section 18: Business and Financial Platforms

### Stripe

- [ ] Create Stripe account for 37Metrics, LLC
- [ ] Use your 37Metrics Office 365 email
- [ ] This is the payment processor for all software product revenue
- [ ] Connect bank account
- [ ] Verify business identity (EIN, LLC docs)
- [ ] Store credentials in Bitwarden

### PayPal Business [DEFENSIVE]

- [ ] Create PayPal Business account for 37Metrics, LLC
- [ ] Some customers will want PayPal. Have it ready.

### Google AdSense

- [ ] Create AdSense account with dimension42ai@gmail.com
    
- [ ] This is for D42 YouTube monetization
    
- [ ] Will need to verify with 37Metrics LLC info (business name, address, tax ID)
    
- [ ] You cannot monetize YouTube without AdSense
    
- [ ] Verify your Marty Sampson YouTube AdSense is set up under martysampson@gmail.com
    
- [ ] Both channels need separate or linked AdSense. Google allows one AdSense per person. You'll link both YouTube channels to the same AdSense account but track revenue separately in YouTube Studio.
    

### Google Analytics

- [ ] Set up GA4 property for dimension42.ai (under dimension42ai@gmail.com)
- [ ] Set up GA4 property for martysampson.com (under dimension42ai@gmail.com or personal)
- [ ] Set up GA4 property for 37metrics.com (optional, low priority)

### Google Search Console

- [ ] Add and verify dimension42.ai
- [ ] Add and verify martysampson.com
- [ ] Add and verify 37metrics.com
- [ ] Submit sitemaps for each once sites are live

---

## Section 19: Email Marketing Platforms

You'll need to send newsletters and product announcements. Pick one.

### ConvertKit (now Kit) or Buttondown or Loops

- [ ] Create account with dimension42ai@gmail.com
- [ ] Set up for dimension42.ai domain
- [ ] Configure SPF/DKIM for sending domain
- [ ] This can be deferred until you have an email list to send to
- [ ] Store credentials in Bitwarden

### Substack handles this for newsletters, but for product email (onboarding, receipts, announcements) you'll need a transactional email service:

### Resend or Postmark or AWS SES

- [ ] Create account for transactional email from D42 products
- [ ] Configure for dimension42.ai domain
- [ ] This can be deferred until you have a product shipping email

---

## Section 20: Legal and IP

### DBA Registration

- [ ] File DBA for "Dimension42" under 37Metrics, LLC in Alabama
- [ ] Check Alabama Secretary of State for name conflicts
- [ ] Keep filed documents in a safe/Bitwarden secure note

### Trademark Search

- [ ] Search USPTO TESS database for "Dimension42" in software/tech classes
- [ ] Search USPTO for "Dimension 42" (with space)
- [ ] Search USPTO for "OpenClaw"
- [ ] Search USPTO for "Forge42"
- [ ] Search USPTO for "Eddie" in AI/software context (probably too generic to trademark)
- [ ] If no conflicts found, consider filing Intent-to-Use trademark application for "Dimension42" in Class 9 (software) and Class 42 (SaaS) when revenue justifies the cost (~$250-350 per class)

### Business Licenses

- [ ] Confirm 37Metrics LLC is in good standing with Alabama Secretary of State
- [ ] Confirm annual report is filed and current
- [ ] Check if Birmingham/Hoover requires a local business license for operating from home

---

## Section 21: Additional Platforms to Squat [DEFENSIVE]

These are lower priority but free to grab. Do them in a batch one evening.

### Gravatar

- [ ] Create Gravatar for dimension42ai@gmail.com (used across WordPress, GitHub comments, etc.)
- [ ] Upload D42 logo

### Keybase [DEFENSIVE]

- [ ] Create Keybase account: dimension42ai
- [ ] Verify GitHub, X, and domain
- [ ] Good for proving identity across platforms

### Linktree [DEFENSIVE]

- [ ] Create Linktree: linktr.ee/dimension42ai
- [ ] Create Linktree: linktr.ee/martysampson
- [ ] Even if you build your own link page on martysampson.com, hold these

### Gumroad [DEFENSIVE]

- [ ] Create account: dimension42ai.gumroad.com
- [ ] Alternative product sales platform. Hold the name.

### Patreon [DEFENSIVE]

- [ ] Create account: patreon.com/dimension42ai
- [ ] Create account: patreon.com/martysampson
- [ ] Hold these even if you use Substack for paid content

### Ko-fi [DEFENSIVE]

- [ ] Create account: ko-fi.com/dimension42ai
- [ ] Create account: ko-fi.com/martysampson
- [ ] Tip jar / support platform. Hold the names.

### Buy Me a Coffee [DEFENSIVE]

- [ ] Create account: buymeacoffee.com/dimension42ai
- [ ] Create account: buymeacoffee.com/martysampson

### Calendly or Cal.com

- [ ] Create account for consulting bookings
- [ ] Use martysampson@gmail.com or marty@dimension42.ai
- [ ] Set URL: cal.com/martysampson or calendly.com/martysampson
- [ ] This is for the martysampson.com/consulting booking flow

### Notion [DEFENSIVE]

- [ ] Create workspace under dimension42ai@gmail.com
- [ ] Hold the workspace name

### Figma [DEFENSIVE]

- [ ] Create account: dimension42ai
- [ ] For future design work on products

### Canva

- [ ] Create account under dimension42ai@gmail.com
- [ ] Set up brand kit with D42 colors, fonts, logos
- [ ] Useful for thumbnails, social graphics, presentations

### Loom [DEFENSIVE]

- [ ] Create account: dimension42ai
- [ ] For product demos, async video communication

---

## Section 22: Domain Squatting (Additional Domains to Consider)

These are optional but worth checking availability and price:

- [ ] dimension42.com — Check availability. If reasonable price, buy it and redirect to dimension42.ai
- [ ] dimension42.io — Check availability. Popular for tech companies.
- [ ] dimension42.dev — Check availability. Google-run TLD for developers.
- [ ] dimension42.tech — Check availability.
- [ ] d42.ai — Check availability. Short alias domain.
- [ ] d42.io — Check availability.
- [ ] forge42.com — Check availability. Matches your email identity.
- [ ] forge42.dev — Check availability.
- [ ] ironmarty.com — Check availability. For the health project if it ever goes public.
- [ ] openclaw.dev — Check availability. For the open source project.
- [ ] openclaw.ai — Check availability.
- [ ] openclaw.io — Check availability.
- [ ] zerohuman.com — Check availability. Could be a powerful brand domain for the narrative.
- [ ] zerohuman.ai — Check availability.
- [ ] zerohumans.com — Check availability.
- [ ] 0human.com — Check availability.

Do NOT buy all of these. Check prices, pick the high-value ones (dimension42.com redirect, and one zerohuman variant for the narrative brand), and skip the rest unless they're under $15/year.

---

## Section 23: Content Platform Accounts

### Anchor / Spotify for Podcasters

- [ ] Already in Section 15, confirm completed

### YouTube Music (auto-created with YouTube channel)

- [ ] No action needed, comes with the channel

### Vimeo [DEFENSIVE]

- [ ] Create account: vimeo.com/dimension42ai
- [ ] Backup video hosting platform. Hold the name.

### Rumble [DEFENSIVE]

- [ ] Create account: rumble.com/dimension42ai
- [ ] Alternative video platform. Growing. Hold the name.

### Odyssey/LBRY [DEFENSIVE]

- [ ] Create account: @dimension42ai
- [ ] Decentralized video platform. Low priority but free.

### Twitch [DEFENSIVE]

- [ ] Create account: twitch.tv/dimension42ai
- [ ] Even if you never stream, hold the name
- [ ] Live coding sessions are a possibility later

---

## Section 24: AI Platform Accounts (Under D42 Identity)

These are platforms where you might publish, build, or showcase AI work.

### OpenAI

- [ ] You likely have this under martysampson@gmail.com. Keep it there. Personal tool.
- [ ] If you ever need a separate D42 API account with its own billing: create with dimension42ai@gmail.com

### Anthropic

- [ ] Same as OpenAI. Personal tool stays personal.
- [ ] Separate D42 API account if needed for product billing

### Replicate

- [ ] Create account: dimension42ai
- [ ] For hosting/running ML models via API

### Weights & Biases

- [ ] Create account: dimension42ai
- [ ] For ML experiment tracking if relevant

### Kaggle [DEFENSIVE]

- [ ] Create account: dimension42ai
- [ ] For datasets and ML competitions visibility

---

## Section 25: Security and Credential Management

### Bitwarden Organization

- [ ] Review all newly created accounts are stored in Bitwarden
- [ ] Create a Bitwarden folder: "D42 Brand Accounts"
- [ ] Create a Bitwarden folder: "Marty Personal Accounts"
- [ ] Create a Bitwarden folder: "37Metrics Business"
- [ ] Every account should have:
    - [ ] Unique, strong password (generated by Bitwarden)
    - [ ] 2FA enabled where supported
    - [ ] Recovery codes stored in the Bitwarden notes field
    - [ ] The email used for registration noted

### HaveIBeenPwned

- [ ] Register dimension42ai@gmail.com for breach notifications
- [ ] Confirm martysampson@gmail.com is already registered
- [ ] Register key FastMail aliases (forge42@fastmail.com at minimum)

---

## Section 26: Physical Setup

### YouTube Studio (Extra Bedroom)

- [ ] Designate the room
- [ ] Get basic lighting (one key light minimum, ring light or softbox)
- [ ] Get a decent microphone (Blue Yeti, Rode PodMic, or Audio-Technica AT2020)
- [ ] Get a webcam or use phone as camera (iPhone works great with Continuity Camera)
- [ ] Set up a clean background (bookshelf, monitor with code, or plain wall)
- [ ] Test audio levels and lighting before recording first video
- [ ] Get a simple teleprompter app for scripted segments

---

## Section 27: Post-Lockdown Verification

After completing all sections, run through this verification pass:

- [ ] Google "dimension42ai" and confirm your profiles dominate the results
- [ ] Google "dimension42 ai" and check results
- [ ] Google "Dimension42" and see what competes (Level 42, Dimension 20, etc.)
- [ ] Google "Marty Sampson" and see what competes (note: there is a SpongeBob character voice actor named Marty Sampson, and Marty Sampson paintings memes)
- [ ] Search each platform for your handle and confirm you own it
- [ ] Test login to every new account from Bitwarden
- [ ] Confirm 2FA works on all critical accounts (Google, GitHub, Stripe, YouTube)
- [ ] Send a test email to every new D42 email alias and confirm delivery
- [ ] Verify all DNS changes have propagated (use dig or whatsmydns.net)

---

## Quick Stats

|Category|Dimension42 Accounts|Marty Sampson Accounts|37Metrics Accounts|Total|
|---|---|---|---|---|
|Google|1|1 (existing)|0|2|
|Video (YouTube, TikTok, etc.)|5|3|0|8|
|Social (X, Instagram, etc.)|8|6|2|16|
|Developer (GitHub, npm, etc.)|8|1 (existing)|1 (existing)|10|
|Business (Stripe, AdSense, etc.)|2|1|2|5|
|Content (Substack, Medium, etc.)|5|3|0|8|
|Defensive squats|~15|~5|~3|~23|
|**Estimated total**||||**~72**|

---

_Document created: April 4, 2026_ _Review and update after completing each section._