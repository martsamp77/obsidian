# Obsidian Mastery Guide 2026

## Your Complete Playbook for Building an AI-Powered Second Brain

---

## Why Obsidian Changes Everything

Obsidian is a local-first, markdown-based knowledge management tool that stores your notes as plain text files in a folder on your computer. This is not Notion. This is not Evernote. Your data is yours, stored as `.md` files you can open in any text editor, forever. If Obsidian disappears tomorrow, your notes survive.

For someone already deep in the AI infrastructure space running Ollama, PostgreSQL, Redis, and building persistent memory systems, Obsidian is the missing human-facing layer. It is the interface where your thinking meets your AI agents. Your markdown files become the oxygen that LLMs breathe.

As of February 2026, Obsidian has crossed 1.5 million users with 22% year-over-year growth. The community has produced over 2,700 plugins. The Mobile 2.0 update brings native widgets, Siri/voice integration, and a redesigned interface. The new Bases feature adds Notion-style database views. And the official Obsidian Skills repo (13.9k+ GitHub stars) created by CEO Steph Ango (kepano) now teaches AI agents how to natively work inside your vault.

---

## Your Multi-Vault Architecture

You're running three distinct contexts: personal/business (37Metrics, Molecular Designs), and work (with an IT team). Each needs a different sync strategy. Here's the complete architecture.

### The Three-Vault Layout

| Vault              | Location                                           | Sync Method               | Git Backup            | Mobile Access           |
| ------------------ | -------------------------------------------------- | ------------------------- | --------------------- | ----------------------- |
| Personal/37Metrics | `C:\Workspace\obsidian`                            | Obsidian Sync             | GitHub private repo   | Yes (phone)             |
| Molecular Designs  | `C:\Workspace\obsidian-molecular`                  | Obsidian Sync             | GitHub private repo   | Yes (phone)             |
| Work/IT Team       | Network file share `\\server\shared\obsidian-team` | Network share IS the sync | Git repo on the share | No (work machines only) |

### Why NOT OneDrive for Vault Storage

Obsidian explicitly recommends against using Obsidian Sync alongside cloud storage services like OneDrive, Dropbox, or iCloud. Both services try to sync the same files simultaneously and step on each other, creating duplicate files, corrupted configs, and mangled `.obsidian` settings folders. OneDrive is especially aggressive with files inside Desktop and Documents folders.

The solution: move your vaults OUT of OneDrive to regular local folders and let each sync layer handle its own job.

### The Three Sync Layers

**Layer 1: Local vault files.** Your working copy lives in `C:\Workspace\obsidian` (and similar paths for other vaults). This is outside OneDrive's sync folders. This is what Obsidian opens and edits every day.

**Layer 2: Obsidian Sync.** Handles real-time sync to your phone and other personal machines. End-to-end encrypted, built specifically for Obsidian's file structure, handles the `.obsidian` config folder properly. One Obsidian account, all personal/business vaults under it.

**Layer 3: Git (GitHub repos).** Your version history and backup layer. Not for live sync, but for rolling back any file to any point in time, seeing diffs of what changed, and having an offsite backup independent of Obsidian's servers. Install the `obsidian-git` plugin and set it to auto-commit every 10-30 minutes.

### Obsidian Sync Plans

**Standard ($4/mo annual):** 1 vault, 1 GB storage, 5 MB max file size, 1 month version history.

**Plus ($8/mo annual):** 10 vaults, 10 GB storage (upgradeable to 100 GB for $16/mo), 200 MB max file size, 12 months version history.

For multiple personal/business vaults with phone access, Plus is the way to go. The 10 vault capacity covers personal, 37Metrics, Molecular Designs, and leaves room for future vaults. The 200 MB file size limit means you can sync PDFs, images, and audio. The 12-month version history is a meaningful safety net beyond what Git provides.

Start with Standard if you only need one vault synced to mobile. Upgrade to Plus when you need more. Pricing is prorated, so there's no penalty for starting low.

### Setting Up Obsidian Sync (Step by Step)

1. **Log in**: Settings (gear icon) → Account → Log in with your Obsidian account
2. **Enable the Sync core plugin**: Settings → Core plugins → find **Sync** → toggle ON. This is the step most people miss. Until you flip this toggle, the Sync section doesn't appear in your sidebar at all.
3. **Open Sync settings**: Back in the Settings sidebar, click the new **Sync** entry, then click the gear/cog icon
4. **Create the remote vault**: Click **"Choose"** → **"Create new remote vault"** → name it (e.g., `37Metrics`) → set an encryption password
5. **CRITICAL: Store the encryption password in a password manager immediately.** Obsidian cannot recover it. If you lose it, you must delete the remote vault and re-upload everything, losing all version history.
6. **Wait for initial sync**: Watch the sync status icon in the bottom status bar. First sync can take a while depending on vault size.
7. **Connect other devices**: On your phone or second computer, open Obsidian → log into same account → enable Sync core plugin → Sync settings → Choose → select your remote vault → enter encryption password → choose to merge with any existing local content or start fresh

### Work Vault: Network File Share for Team Collaboration

The work vault uses a completely different approach. Instead of Obsidian Sync (which requires accounts and doesn't support multi-user editing), the vault lives on a network file share that you and your IT staff all access.

**How it works:** Everyone points Obsidian at the same shared folder (e.g., `\\server\shared\obsidian-team` or a mapped drive like `Z:\obsidian`). Obsidian is just reading a folder of files, so if multiple people can access the folder, they can all open it as a vault. No Obsidian accounts needed for the team.

**Recommended folder structure for a shared work vault:**

```
work-vault/
├── _shared/          # Team documentation, runbooks, SOPs
├── _templates/
├── _attachments/
├── noam/             # Your notes, daily logs, scratch
├── mike/             # IT team member's space
├── sarah/            # Another team member
└── projects/         # Shared project notes
```

Everyone can read everything, but each person primarily writes in their own space. The `_shared` and `projects` folders are communal. This natural partitioning makes simultaneous edit conflicts extremely rare.

**Simultaneous editing caveat:** Obsidian does not support real-time collaboration like Google Docs. If two people edit the exact same note at the exact same moment, the last save wins. With a small IT team writing mostly in their own folders, this is a non-issue in practice. Just communicate naturally about who's updating shared docs.

**Shared config note:** The `.obsidian` config folder stores plugin settings, workspace layouts, and hotkey configs. Everyone sharing the vault shares the same config. If you install a plugin, it shows up for everyone. For a small IT team this is usually a feature (everyone gets the same setup), but be aware of it.

**Git on the network share:** Initialize the work vault as a Git repo too. One person (you) manages the repo. This gives version history on all shared documentation. If someone accidentally deletes a runbook or overwrites a procedure, you just roll it back. The `obsidian-git` plugin can auto-commit, or you can do manual commits when meaningful changes happen.

### The Account Situation: Why This Architecture Works

Obsidian only supports one logged-in account at a time per app installation. You can't be logged into a personal account and a work account simultaneously. This architecture avoids the problem entirely:

- Your personal/business vaults sync via Obsidian Sync under your one personal account
- Your work vault syncs via the network share with zero Obsidian account involvement
- Your phone stays logged into your personal account, giving you mobile access to personal and business vaults
- Work data stays on work machines, which is what most companies prefer for data governance

### Git Setup for Your Vaults

For each personal/business vault, initialize a Git repo and push to GitHub:

```bash
cd C:\Workspace\obsidian
git init
git remote add origin https://github.com/youruser/obsidian-vault.git
```

**Recommended `.gitignore`:**

```
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/plugins/recent-files-obsidian/data.json
.trash/
```

You want to commit most of `.obsidian/` (so your plugin configs are versioned) but exclude workspace state files that change constantly and create noisy commits.

Install the `obsidian-git` community plugin for auto-commits every 10-30 minutes. This means your vault is continuously backed up without you thinking about it.

### Migration Steps: OneDrive to New Architecture

1. Copy your vault from the OneDrive `00_Obsidian` folder to `C:\Workspace\obsidian`
2. Open Obsidian → "Open folder as vault" → point to `C:\Workspace\obsidian`
3. Verify everything looks correct (notes, plugins, settings all intact)
4. Set up Obsidian Sync (enable core plugin, create remote vault, set encryption password)
5. Wait for initial sync to complete
6. Initialize Git in the vault folder, create `.gitignore`, push to GitHub
7. Install the `obsidian-git` community plugin for auto-commits
8. On your phone, install Obsidian → log in → enable Sync → connect to remote vault
9. Remove or archive the old vault from OneDrive (keep as static backup if you want, just don't open it in Obsidian anymore)
10. Repeat for any additional personal/business vaults

### Notion Integration for Mobile

Your plan to use Notion for phone-based push is practical. Obsidian Mobile 2.0 has improved significantly with native widgets and Siri shortcuts, but Notion still has better quick-capture on mobile. Options for bridging:

- Use Obsidian for all deep thinking and writing, push select items to Notion for mobile reference
- Use the Obsidian Web Clipper for browser-to-vault capture
- Consider the Drafts app as a quick-capture intermediary that can append to Obsidian files
- For task push notifications specifically, apps like TaskForge can read your Obsidian vault and send mobile reminders

---

## Essential Settings to Configure First

These settings prevent headaches down the road. Open Settings immediately after creating your vault.

### Files and Links

- **Automatically update internal links**: TURN ON. Without this, renaming a note breaks every link pointing to it. This is the single most important setting in Obsidian.
- **Default location for new attachments**: Create a folder called `_attachments` or `files` and point all attachments there. This prevents images and PDFs from cluttering your sidebar.
- **Use Wikilinks**: Leave ON. Double-bracket `[[linking]]` is faster and more readable than standard markdown links for internal notes.
- **New link format**: Shortest path when possible (default, keep it).

### Editor

- **Default editing mode**: Live Preview. This renders markdown formatting in real-time while still showing raw syntax when your cursor is on that line. Best of both worlds.
- **Fold heading / Fold indent**: TURN ON for both. Lets you collapse sections of long notes, essential for progressive disclosure.
- **Spell check**: ON, unless you prefer a plugin like Harper.
- **Auto pair brackets**: ON. Typing `[[` will auto-close with `]]`.
- **Smart lists**: ON. Hitting Enter after a bullet creates the next bullet automatically.

### Appearance

- **Theme**: The default theme is now excellent since the Minimal theme creator (kepano) became CEO. Community themes can break with updates. Start with default, customize with CSS snippets later.
- **Readable line length**: ON. Limits text width for readability on wide screens.
- **Show inline title**: ON.

### Core Plugins to Enable

- **Daily notes**: Your daily driver for capture
- **Templates** (or better: install the Templater community plugin)
- **Backlinks**: Shows what links TO the current note
- **Graph view**: Visual map of your knowledge connections
- **Unique note creator**: Creates timestamped notes for quick capture
- **Workspaces**: Save and switch between different layout configurations
- **Bases**: Obsidian's new database/table feature (like Notion databases but local)
- **Web viewer**: Browse the web inside Obsidian
- **Canvas**: Infinite whiteboard for visual thinking

---

## Folder Structure: Start Simple, Earn Complexity

The biggest mistake new users make is over-organizing with deep folder hierarchies. Ideas are messy and belong to multiple categories. Let structure emerge from use, not from planning.

### Recommended Starting Structure (ACE Framework by Nick Milo)

```
C:\Workspace\obsidian\
├── Atlas/          # Things to KNOW (people, concepts, evergreen notes, quotes)
│   ├── dots/       # Atomic concepts, things, statements, quotes
│   └── references/ # External entities (people, movies, books, places)
├── Calendar/       # Time-based notes (daily, weekly, monthly)
│   └── daily/
├── Efforts/        # Things to DO (projects, tasks, areas of effort)
│   ├── projects/
│   │   ├── active/
│   │   ├── simmering/
│   │   └── sleeping/
│   └── areas/
├── Plus/           # Quick capture inbox for new unprocessed notes
├── Templates/      # Note templates
├── _attachments/   # Images, PDFs, media files
└── _bases/         # Obsidian Bases database views
```

### Alternative: Kepano's Minimalist Approach (Obsidian CEO)

Steph Ango's personal system is radically simple:

- **Root of vault**: Your own notes, journal entries, evergreen ideas. Most notes live here, not in folders.
- **References folder**: Things outside your world (people, movies, books, podcasts)
- **Templates folder**: Note templates
- **Attachments folder**: Media files
- **Daily folder**: Daily notes
- **Clippings folder**: Web clips

His philosophy: avoid folders because most entries belong to more than one area of thought. The system is oriented toward speed and laziness. Organization comes from **properties** (frontmatter metadata) and **Bases views**, not folder hierarchies.

### What NOT to Do

- Do NOT import thousands of old notes from another app. Start fresh. Link your own thoughts.
- Do NOT create deeply nested folder trees. You will forget where things go.
- Do NOT chase plugins on day one. Focus on writing and linking first.
- Do NOT skip learning hotkeys. Speed is what makes Obsidian enjoyable.

---

## The Core Workflow: Linking as Thinking

The defining feature of Obsidian is bidirectional linking. This is what separates it from every other note app.

### How Links Work

- Type `[[` to create a link to another note
- The note doesn't need to exist yet. Obsidian creates placeholder links that become real notes when you click them.
- **Link the first mention** of anything: people, concepts, projects, books, quotes
- Even if you never flesh out that note, you're setting an intention that you might in the future

### Backlinks: The Hidden Power

When Note A links to Note B, Note B automatically shows that Note A references it. This creates a web of connections that mirrors how your brain works. You can trace how ideas emerged and the branching paths they created.

Open the backlinks pane (right sidebar) to see every note that mentions the current note.

### Maps of Content (MOCs)

Instead of tags or folders for organization, create hub notes that link to related notes by topic. For example, a note called `AI Infrastructure` might link to notes on Ollama, Qdrant, LiteLLM, Mem0, and IronForge. These MOCs become powerful navigation tools and connective hubs in your graph view.

### Evergreen Notes

Ideas that you want to reuse and reference repeatedly become evergreen notes. These are atomic, standalone concepts written in your own words. They compound in value over time as more notes link to them. Tag them with `#evergreen` for easy filtering via Bases.

---

## Essential Hotkeys

Learn these immediately. The faster you work, the more you'll use Obsidian.

| Action                          | Mac                     | Windows        |
| ------------------------------- | ----------------------- | -------------- |
| Create new note                 | `Cmd+N`                 | `Ctrl+N`       |
| Quick switcher (find/open note) | `Cmd+O`                 | `Ctrl+O`       |
| Command palette                 | `Cmd+P`                 | `Ctrl+P`       |
| Insert link                     | `[[`                    | `[[`           |
| Bold                            | `Cmd+B`                 | `Ctrl+B`       |
| Italic                          | `Cmd+I`                 | `Ctrl+I`       |
| Find in note                    | `Cmd+F`                 | `Ctrl+F`       |
| Navigate back/forward           | `Cmd+Opt+←/→`           | `Ctrl+Alt+←/→` |
| Open in new tab                 | `Cmd+Click`             | `Ctrl+Click`   |
| New tab                         | `Cmd+T`                 | `Ctrl+T`       |
| Close tab                       | `Cmd+W`                 | `Ctrl+W`       |
| Toggle reading/editing view     | Click icon in title bar | Same           |
| Move line up/down               | `Alt+↑/↓`               | `Alt+↑/↓`      |
| Indent/outdent                  | `Tab` / `Shift+Tab`     | Same           |
| Insert template                 | Set custom hotkey       | Same           |

---

## Markdown Quick Reference

```markdown
# Heading 1

## Heading 2

### Heading 3

**bold text**
_italic text_
~~strikethrough~~
==highlighted text==

- bullet list

1. numbered list

- [ ] checkbox
- [x] completed checkbox

> blockquote

[[internal link]]
[external link](https://url.com)
![[embedded note]]
![[image.png]]

`inline code`

--- (horizontal divider)
```

---

## Templates: Consistency Without Effort

Templates save massive time and ensure consistent structure. Create these in your `Templates` folder.

### Daily Note Template

```markdown
---
created: { { date } }
tags:
  - daily
---

# {{date:YYYY-MM-DD dddd}}

## Plan

- [ ]

## Notes

## Wins

## Log
```

### Meeting Template

```markdown
---
created: { { date } }
tags:
  - meeting
categories:
  - meetings
people:
topics:
location:
---

# {{title}}

## Attendees

## Agenda

## Notes

## Action Items

- [ ]
```

### Project Template

```markdown
---
created: { { date } }
tags:
  - project
status: active
rank: 5
area:
---

# {{title}}

## Objective

## Key Results

- [ ]

## Notes

## Resources
```

### Evergreen Note Template

```markdown
---
created: { { date } }
tags:
  - evergreen
---

# {{title}}

---

## Related
```

---

## Bases: Obsidian's Database Layer

Bases (introduced 2025, rapidly improving in 2026) bring Notion-style database views to Obsidian. They query your notes' properties (frontmatter metadata) and display them as sortable, filterable tables, card galleries, lists, or maps.

### How to Use Bases

1. Create a new Base (right-click in file explorer or Cmd+P → "Create new base")
2. Add filters based on properties: tags, folders, created date, custom fields
3. Add columns for any property: status, rank, people, dates
4. Create multiple views: table for data, cards for visual browsing, list for scanning
5. Sort and group by any property

### Practical Base Examples

- **All Projects**: Filter by tag `project`, group by `status` (active/simmering/sleeping), sort by `rank`
- **Meetings**: Filter by tag `meeting`, show `date`, `people`, `topics` columns
- **Reading List**: Filter by tag `book`, show `status`, `rating`, `author`
- **People**: Filter by tag `person`, show `organization`, `birthday`, `website`
- **Evergreen Ideas**: Filter by tag `evergreen`, sort by modified date

### Rating System (from Kepano)

Apply a 1-7 rating to anything: books, movies, ideas, experiences. 7 = absolutely life-changing, 1 = negatively life-changing. Create a Ratings base to see everything you've rated, sorted by impact.

---

## Workspaces: Context Switching on Demand

Workspaces save your entire layout (which notes are open, sidebar configuration, bases displayed) and let you switch between them instantly.

### Recommended Workspaces

- **Blank Slate**: Clean starting point, recent notes in sidebar
- **Producer Mode**: Active projects base in sidebar, efforts overview
- **Inner Guide**: Daily notes calendar, reflections, journal entries
- **Synthesizer**: Dynamic relationship views showing unrequited links and shared connections
- **Creative**: Evergreen ideas, quotes, concept notes for brainstorming

Set up hotkeys for workspace switching: `Cmd+Ctrl+Opt+5` for manage layouts.

---

## Recommended Plugins (Start with These)

### Tier 1: Install Immediately

| Plugin                 | Purpose                                                                                |
| ---------------------- | -------------------------------------------------------------------------------------- |
| **Templater**          | Far more powerful than core Templates. Supports dynamic dates, prompts, and scripting  |
| **Calendar**           | Visual calendar for daily notes. Click any date to create/open that day's note         |
| **Periodic Notes**     | Extends daily notes to weekly, monthly, quarterly, yearly                              |
| **Notebook Navigator** | Replaces file explorer with a much better navigation pane with shortcuts, tags, search |
| **Obsidian Git**       | Version control and backup. Restore any file to any previous state                     |

### Tier 2: Add When Ready

| Plugin         | Purpose                                                                               |
| -------------- | ------------------------------------------------------------------------------------- |
| **Tasks**      | Query and manage tasks across your entire vault with due dates, priorities, recurring |
| **Dataview**   | SQL-like queries over your notes. Create dynamic dashboards                           |
| **QuickAdd**   | Macro system for rapid note creation with templates                                   |
| **Linter**     | Auto-format notes on save for consistency                                             |
| **Omnisearch** | Fuzzy search that's better than default                                               |

### Tier 3: Power User

| Plugin              | Purpose                                                        |
| ------------------- | -------------------------------------------------------------- |
| **Excalidraw**      | Embedded whiteboard/diagramming                                |
| **Kanban**          | Visual project boards                                          |
| **Advanced Canvas** | Extra canvas features (shapes, text alignment, search)         |
| **Style Settings**  | Deep UI customization                                          |
| **Fold Properties** | Collapse the properties section by default (keeps notes clean) |

### Plugins to AVOID at First

- Dataview dashboards (too complex before you have content)
- Advanced Tables (not needed with Bases now)
- Anything that dramatically changes the editing experience

---

## The Note-Taking Rhythm (from Kepano's System)

### Daily

- Create a new unique note for any thought, insight, or journal entry
- Link first mentions of people, places, concepts, quotes
- Add a descriptive title and use templates to auto-populate properties
- Don't worry about perfect organization. Capture first.

### Every Few Days

- Review recent daily notes
- Compile thoughts that are relevant into new standalone notes
- Identify patterns across your thinking

### Weekly

- Create a weekly note
- Write simple to-dos for the week (just markdown checklists)
- Process your daily notes: extract important ideas into their own notes, add links

### Monthly

- Review your idea compilations from the month
- Create a higher-level monthly reflection
- Notice what themes and directions are emerging

### Every Few Months

- Hit "Open random note" and traverse your vault
- Rediscover old ideas, find unexpected connections
- Add new links between things you didn't previously connect

### Yearly

- Review all monthly reviews
- Reflect on big questions about your direction, growth, and goals

---

## Claude Code + Obsidian: The AI-Native Knowledge System

This is where your IronForge infrastructure vision meets personal knowledge management. The integration of Claude Code with Obsidian is the breakthrough workflow of 2026.

### Why This Works

1. Obsidian notes are plain markdown files in a folder. Claude Code has instant access to everything.
2. Markdown is the native format LLMs prefer. Claude can create, edit, and structure `.md` files natively.
3. Obsidian's `[[wikilinks]]` are trivial for Claude Code to work with.
4. Your vault becomes persistent AI memory across sessions.

### Setup Steps

1. **Install Claude Code** (requires Claude Pro or higher)
2. **Clone the official Obsidian Skills repo** from `github.com/kepano/obsidian-skills`
3. **Add skills to your vault**: Copy the repo contents into a `.claude/` folder in your vault root
4. **Run `/init`**: This creates `CLAUDE.md` at the root of your vault, which becomes Claude's memory file loaded into every session
5. **Install the Terminal plugin** in Obsidian to run Claude Code from within the app
6. **Enable Obsidian CLI** in settings so Claude can programmatically interact with your vault

### The Five Official Obsidian Skills

| Skill                 | What It Does                                                                    |
| --------------------- | ------------------------------------------------------------------------------- |
| **obsidian-markdown** | Teaches Claude proper Obsidian syntax: wikilinks, callouts, frontmatter, embeds |
| **obsidian-bases**    | Creates and manages Bases databases with typed properties, filters, sorts       |
| **json-canvas**       | Creates visual canvas whiteboards with nodes, edges, groups                     |
| **obsidian-cli**      | Read notes, create/update files, search content, manage tasks via CLI           |
| **defuddle**          | Extract clean markdown from messy web pages for vault import                    |

### What You Can Do

- **Research agent**: Claude reads your vault, finds connections between notes you never noticed, surfaces patterns across domains
- **Context engineering**: Every note you write becomes context Claude can use. Your preferences, your project details, your thinking patterns are all available.
- **Automated note creation**: Claude can process meeting transcripts, web clips, PDFs into properly formatted Obsidian notes with frontmatter, links, and tags
- **Knowledge queries**: Ask Claude questions about your own notes across months or years of thinking
- **Slash commands**: Create custom commands like `/weekly-review` that summarizes your week from daily notes, or `/format-notes` that cleans up files
- **Persistent memory**: The `CLAUDE.md` file and your vault structure give Claude long-term memory that persists across sessions

### The Golden Rule

**Your vault should contain your authentic thinking. Claude reads it for context but should not pollute it with AI-generated content.** Keep Claude's outputs (plans, memory, generated research) in a separate folder like `_ai-output/`. Your personal notes, reflections, and ideas remain untouched and genuine.

### Integration with Your IronForge Stack

Your existing infrastructure maps perfectly:

- **Mem0 + Qdrant**: Your AI memory layer. Obsidian becomes the human-readable interface to that memory.
- **Claude Code**: Reads your vault, powers slash commands, creates structured notes
- **Ollama**: Local models can power privacy-first AI plugins like Smart Connections (RAG over your vault) or Local GPT
- **MCP Protocol**: Connect Claude Code to your vault, your APIs, your tools. The Claudesidian MCP plugin adds semantic search via Ollama embeddings.
- **n8n**: Automate flows between web clips, email, calendar events and your Obsidian vault

---

## The Knowledge Ingestion Process

This is the workflow that makes everything compound:

### Capture (Frictionless)

- Daily notes for stream of consciousness
- Web Clipper for articles and research
- Quick capture from mobile (Notion push or Drafts app)
- Voice memos via audio recorder core plugin

### Process (Weekly)

- Go through recent daily notes
- Extract standalone ideas into their own notes
- Add proper frontmatter properties and tags
- Create links to existing related notes
- Move reference items (people, books, tools) to the references folder

### Compile (Monthly)

- Create monthly reflection notes
- Identify recurring themes and patterns
- Update project notes and evergreen ideas
- Let Claude Code surface connections you missed

### Retrieve (Ongoing)

- Quick Switcher (`Cmd+O`) for known notes
- Bases views for filtered browsing
- Graph view for visual exploration
- Claude Code for semantic querying across your entire vault
- Random note for serendipitous rediscovery

---

## Advanced Tips and Tricks

### Embedding Notes Inside Notes

Add `!` before a link to embed the full content: `![[note name]]`. This lets you compose documents from smaller atomic notes.

### Linking to Specific Sections

Use `#` to link to a heading: `[[note name#Section Heading]]`. Use `^` to link to a specific block: `[[note name^block-id]]`.

### Folding for Progressive Disclosure

With fold heading and fold indent enabled, click the arrow next to any heading or list item to collapse its contents. The collapsed state persists when you close and reopen the file.

### Multiple Cursors

Hold `Alt/Option` and click to place multiple cursors. Edit several lines simultaneously.

### Swap Lines

Set hotkeys for "Swap line up" and "Swap line down" (`Alt+↑/↓`). Instantly reorder lists, outlines, and priorities.

### CSS Snippets

Add custom `.css` files to your vault's `.obsidian/snippets/` folder. Toggle them on/off in Appearance settings. Useful for tweaking fonts, colors, spacing without a full theme.

### Embeds from the Web

Paste YouTube embed codes or CodePen iframes directly into notes. They render in Reading View. Great for study notes alongside video content.

### Properties as Links

In frontmatter, use `[[wikilinks]]` inside property values. This makes properties navigable. For example, `people: [[John Smith]]` creates a clickable link in your Bases views.

---

## Security and Privacy Considerations

- Obsidian stores everything locally. No telemetry, no tracking. The team doesn't even know how many users they have.
- Obsidian Sync provides end-to-end encryption
- Multiple independent third-party security audits confirm highest security standards (downloadable at obsidian.md/security)
- Community plugins have full computer access. Only install widely-used, actively maintained plugins.
- The restricted mode toggle exists for a reason. Vet plugins before enabling them.
- Use Git for versioning and backup. Sync is NOT backup. OneDrive sync is NOT backup either.
- **Work vault data governance**: Keeping the work vault on a network file share (not Obsidian Sync) means work data never touches Obsidian's cloud servers or your personal devices. This is usually what companies prefer. Work stays on work machines.
- **Never run OneDrive sync and Obsidian Sync on the same vault folder.** They conflict and cause file corruption. Pick one sync method per vault.
- **Encryption passwords for Sync are unrecoverable.** Store them in a password manager the moment you create them. Losing the password means deleting the remote vault and re-uploading everything.

---

## Your First Week Action Plan

### Day 1: Foundation and Vault Migration

- [ ] Download Obsidian from obsidian.md
- [ ] Create your vault folder at `C:\Workspace\obsidian` (NOT in OneDrive)
- [ ] Copy any existing notes from OneDrive `00_Obsidian` to the new location
- [ ] Open Obsidian → "Open folder as vault" → point to `C:\Workspace\obsidian`
- [ ] Configure essential settings (auto-update links, attachment folder, live preview)
- [ ] Create your folder structure (ACE or Kepano-style)

### Day 2: Sync and Backup Setup

- [ ] Log into your Obsidian account (Settings → Account)
- [ ] Enable the Sync core plugin (Settings → Core plugins → Sync → ON)
- [ ] Create a remote vault in Sync settings (Choose → Create new remote vault)
- [ ] Set encryption password and SAVE IT IN A PASSWORD MANAGER
- [ ] Wait for initial sync to complete
- [ ] Initialize Git: `cd C:\Workspace\obsidian && git init`
- [ ] Create `.gitignore` (exclude workspace.json, workspace-mobile.json, .trash/)
- [ ] Push to a private GitHub repo
- [ ] Install the `obsidian-git` community plugin, set auto-commit to 15-30 minutes
- [ ] Install Obsidian on your phone, log in, enable Sync, connect to remote vault

### Day 3: Templates and Hotkeys

- [ ] Install Templater plugin
- [ ] Create daily note, meeting, project, and evergreen templates
- [ ] Learn the five essential hotkeys: Cmd+O, Cmd+N, Cmd+P, `[[`, Cmd+B
- [ ] Practice creating and linking notes
- [ ] Create your first daily note

### Day 4: Plugins and Workflow

- [ ] Install Calendar, Periodic Notes, Notebook Navigator
- [ ] Create your first Base (start with "All Notes" or "Projects")
- [ ] Start journaling in daily notes

### Day 5: Work Vault Setup

- [ ] Create a shared folder on the network file share for the work vault
- [ ] Set up the folder structure with personal namespaces (noam/, team members/)
- [ ] Open the network share folder as a vault in Obsidian on your work machine
- [ ] Initialize Git on the work vault for version history
- [ ] Have IT team members open the same folder as their vault
- [ ] Create shared templates for the team (meeting notes, runbooks, incident reports)

### Day 6: AI Integration

- [ ] Clone kepano/obsidian-skills into your personal vault's `.claude/` folder
- [ ] Install Terminal plugin
- [ ] Run Claude Code with `/init` in your vault
- [ ] Try asking Claude to summarize your notes or find connections

### Day 7: Rhythm

- [ ] Do your first weekly review
- [ ] Process daily notes from the week
- [ ] Create your first Maps of Content
- [ ] Explore the graph view
- [ ] Set up workspaces for different modes (producer, creative, review)
- [ ] Remove or archive the old OneDrive vault (keep as static backup, don't open in Obsidian)

---

## Resources

- **Obsidian Help**: help.obsidian.md
- **Obsidian Forum**: forum.obsidian.md
- **Obsidian Discord**: discord.gg/obsidianmd
- **Obsidian Skills (AI)**: github.com/kepano/obsidian-skills
- **Kepano's Blog**: stephango.com
- **Nick Milo (Linking Your Thinking)**: YouTube channel for ACE framework and Ideaverse
- **Obsidian CLI**: Built into Obsidian settings, enable for Claude Code integration
- **Claudesidian MCP Plugin**: Community plugin for semantic search via Ollama embeddings

---

_This guide synthesizes content from 12 video transcripts (Nick Milo, Kepano's vault walkthrough, advanced techniques, Obsidian Bases, settings guide, Claude Code integration, and more) plus current web research from Reddit, forums, and developer blogs as of March 2026. It includes a complete multi-vault sync architecture covering Obsidian Sync, Git backup, network file share team collaboration, and OneDrive migration. Tailored for a power user building AI infrastructure who values markdown files, local-first tools, and deep integration with Claude Code and LLM workflows._
