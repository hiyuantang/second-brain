# Second Brain

A personal knowledge base built with Obsidian, synced via GitHub, and **maintained by Claude Code**. You feed it raw content; Claude processes, organizes, and commits structured knowledge.

## How It Works

```
You → Paste raw content into Claude chat
Claude → Classifies, processes, links, and commits
Vault → Grows as a connected knowledge graph
```

You don't need to worry about folders, templates, or linking. Just give Claude content and it figures out where things go.

### What You Can Feed Claude

| Content Type | Example | What Claude Does |
|---|---|---|
| **Article URL or text** | "Save this article about habit formation: https://..." | Creates a Literature Note + extracts Permanent Notes |
| **Book notes/highlights** | "Here are my highlights from Atomic Habits..." | Creates a Book Note + key idea notes |
| **Quick idea or thought** | "I just realized that compound effects apply to learning too" | Creates a Fleeting Note, later promotes to Permanent |
| **Meeting or conversation notes** | "Met with Alex about the new project plan..." | Creates/updates a Project Note |
| **Transcripts** | Podcast, video, or conversation transcripts | Extracts key ideas into structured notes |
| **Daily log** | "Today I worked on X, learned Y, met Z" | Appends to today's Daily Note |
| **Bulk content** | "Here are 5 articles I want to save" | Processes each with individual commits |

## Vault Structure

```
Second Brain/
├── Home.md                  # Vault entry point
├── 00 - Inbox/              # Quick capture, pending triage
├── 01 - Daily/              # Daily notes (YYYY-MM-DD.md)
├── 02 - Projects/           # Active projects with deadlines
├── 03 - Areas/              # Ongoing responsibilities
├── 04 - Resources/          # External reference material (articles, books, tools)
├── 05 - Knowledge/          # Permanent notes — your synthesized ideas
├── 06 - Archive/            # Completed and inactive items
├── 90 - Templates/          # Note templates
├── 91 - Attachments/        # Images, PDFs, media
├── 99 - Meta/               # System documentation
└── .obsidian/               # Obsidian configuration
```

The vault combines **PARA** (actionability-based organization) with **Zettelkasten** (atomic, linked permanent notes). Claude handles the categorization.

## How to Use

### Opening the Vault

1. Clone this repository: `git clone <your-repo-url> second-brain`
2. Open Obsidian → "Open folder as vault" → select the `Second Brain/` folder
3. Open `Home.md` as your starting point

### Ingesting Content (Primary Workflow)

Just paste content into the Claude Code chat. Examples:

```
"Save this article: https://example.com/productivity-system"

"Here are my notes from the book Deep Work by Cal Newport..."

"Idea: the best way to learn something is to teach it. Write that down."

"Process this transcript of the Huberman Lab episode on sleep..."

"Today I had a meeting with Sarah about the website redesign..."
```

Claude will:
1. Classify the content and choose the right template
2. Create the note(s) with proper frontmatter and tags
3. Search for and link to related existing notes
4. Commit to git with a descriptive message
5. Summarize what was added

### Viewing Your Knowledge

Open the vault in Obsidian to browse, search, and visualize:

| Method | How | Best For |
|--------|-----|----------|
| **Quick Switcher** | `Cmd/Ctrl+O` | Finding a note by title |
| **Global Search** | `Cmd/Ctrl+Shift+F` | Full-text search across all notes |
| **Tag Search** | `tag:#topic` in search | Finding all notes on a topic |
| **Graph View** | Visual node map | Discovering clusters of related ideas |
| **Backlinks** | Bottom of each note | Discovering what references this note |
| **MOCs** | Maps of Content in `05 - Knowledge/MOCs/` | Browsing a topic systematically |

### Asking Claude to Retrieve

You can also ask Claude to find things for you:

```
"What do I have on the topic of habit formation?"
"Show me all notes related to machine learning"
"What books have I saved?"
"Find connections between my notes on sleep and productivity"
```

## How to Expand

### Adding New Resource Types

Tell Claude: "I want to save podcast episodes as a new type. Create a template and folder for it."

Claude will:
1. Create the subfolder (e.g., `04 - Resources/Podcasts/`)
2. Create a Podcast Note Template in `90 - Templates/`
3. Update this README

### Adding New Knowledge Domains

Tell Claude: "I'm starting to learn about cryptography. Set up a knowledge area for it."

Claude will:
1. Create relevant subfolder structure if needed
2. Create a `MOC - Cryptography.md` to serve as the topic hub
3. Tag and file new notes appropriately

### Creating MOCs (Maps of Content)

When a topic accumulates 5+ notes, Claude should proactively suggest creating an MOC. You can also request one:

```
"I have a lot of notes about productivity. Create an MOC for it."
```

### Adding Custom Templates

Tell Claude: "Create a new template for lecture notes."

Claude will create it in `90 - Templates/` using the established frontmatter and tag conventions.

## Syncing with GitHub

This vault uses git for version control. Claude commits after each ingestion session.

```bash
# Pull latest changes (e.g., on another device)
git pull origin main

# Push if you made manual changes in Obsidian
git add .
git commit -m "feat(knowledge): add notes on cryptography"
git push origin main
```

Or use the **Obsidian Git** community plugin for automatic sync.

## Maintenance

### Claude's Responsibilities
- Classify and ingest all incoming content
- Link new notes to existing ones
- Create commits with conventional commit messages
- Suggest MOCs when topics grow large
- Periodically flag orphaned or unlinked notes

### Your Responsibilities
- Review and triage flagged items when Claude asks
- Complete monthly/annual reviews (use templates in `90 - Templates/`)
- Archive completed projects (or ask Claude to do it)
- Provide feedback if categorization isn't working

### Review Cadence

| Frequency | Action |
|-----------|--------|
| **Daily** | Feed Claude content as it comes up |
| **Weekly** | Review the week's ingestions, ask Claude for a summary |
| **Monthly** | Complete a Monthly Review (ask Claude or use the template) |
| **Yearly** | Annual reflection, prune stale areas, set new priorities |

## Tag Conventions

- **Type tags**: `#type/daily`, `#type/fleeting`, `#type/literature`, `#type/permanent`, `#type/book`, `#type/project`, `#type/moc`, `#type/review`
- **Status tags**: `#status/active`, `#status/on-hold`, `#status/completed`, `#status/archived`
- **Topic tags**: lowercase, hyphenated (e.g., `#productivity`, `#machine-learning`)
- **Source tags**: `#source/article`, `#source/book`, `#source/podcast`, `#source/video`, `#source/conversation`
