# Second Brain

A personal knowledge base built with Obsidian, synced via GitHub, and **maintained by Claude Code**. You feed it raw content; Claude processes, organizes, and commits structured knowledge.

## Prerequisites

- **[Obsidian](https://obsidian.md/)** — The local-first markdown app that renders your vault
- **[Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview)** — The AI agent that classifies, writes, links, and commits notes on your behalf
- **Git** — For version control and sync

That's it. Claude Code handles all commits, linking, and organization automatically — you don't need to run git commands, configure Obsidian plugins, or manage sync manually.

## How It Works

```
You → Paste raw content into Claude Code chat
Claude → Classifies, processes, links, and commits
Vault → Grows as a connected knowledge graph
```

You don't need to worry about folders, templates, or linking. Just give Claude content and it figures out where things go.

## Quick Start

### 1. Clone this repository

```bash
git clone https://github.com/hiyuantang/second-brain.git
```

**Do not fork** — just clone. If you want a private copy, create a new private repo and push here instead of to origin.

### 2. Open in Obsidian

Open Obsidian → "Open folder as vault" → select the `Second Brain/` folder.

### 3. Start feeding it content

Paste anything into the Claude Code chat:

```
"Save this article about habit formation: https://..."
"Here are my highlights from Atomic Habits..."
"Idea: compound effects apply to learning too"
"Today I had a meeting with Sarah about the project..."
```

Claude will classify, write notes, link them together, and commit for you.

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

## What You Can Feed Claude

| Content | Example | What Claude Does |
|---------|---------|-----------------|
| Article URL or text | "Save this: https://..." | Creates a Literature Note + extracts Permanent Notes |
| Book highlights | "Highlights from Deep Work..." | Creates a Book Note + key idea notes |
| Quick idea | "Idea: the best way to learn is to teach it" | Creates a Fleeting Note, promotes to Permanent later |
| Meeting notes | "Met with Alex about the new project..." | Creates/updates a Project Note |
| Daily log | "Today I worked on X, learned Y..." | Appends to today's Daily Note |
| Bulk content | "Here are 5 articles I want to save" | Processes each with individual commits |

## Retrieving Info

Open the vault in Obsidian and use:

| Method | Shortcut | Best For |
|--------|----------|----------|
| Quick Switcher | `Cmd/Ctrl+O` | Finding a note by title |
| Global Search | `Cmd/Ctrl+Shift+F` | Full-text search |
| Tag Search | `tag:#topic` | Finding notes on a topic |
| Graph View | Visual node map | Discovering idea clusters |
| Backlinks | Bottom of each note | Seeing what references this note |
| MOCs | Maps of Content in `05 - Knowledge/MOCs/` | Browsing a topic systematically |

Or ask Claude directly:

```
"What do I have on habit formation?"
"Show me all notes related to machine learning"
"What books have I saved?"
```

## How to Expand

### Adding New Resource Types

Tell Claude: "I want to save podcast episodes as a new type. Create a template and folder for it."

Claude will create the folder, template, and update the config.

### Adding New Knowledge Domains

Tell Claude: "I'm starting to learn about cryptography. Set up a knowledge area for it."

Claude will create a `MOC - Cryptography.md` to serve as the topic hub and tag new notes appropriately.

### Creating MOCs (Maps of Content)

When a topic accumulates 5+ notes, Claude proactively suggests creating an MOC. You can also request one:

```
"I have a lot of notes about productivity. Create an MOC for it."
```

## Tag Conventions

- **Type tags**: `#type/daily`, `#type/fleeting`, `#type/literature`, `#type/permanent`, `#type/book`, `#type/project`, `#type/moc`, `#type/review`
- **Status tags**: `#status/active`, `#status/on-hold`, `#status/completed`, `#status/archived`
- **Topic tags**: lowercase, hyphenated (e.g., `#productivity`, `#machine-learning`)
- **Source tags**: `#source/article`, `#source/book`, `#source/podcast`, `#source/video`, `#source/conversation`

## Maintenance

### Claude's Responsibilities

- Classify and ingest all incoming content
- Link new notes to existing ones
- Create commits with conventional commit messages
- Suggest MOCs when topics grow large
- Periodically flag orphaned or unlinked notes

### Your Responsibilities

- Review and triage flagged items when Claude asks
- Complete monthly/annual reviews (or ask Claude to use the templates)
- Archive completed projects (or ask Claude to do it)
- Provide feedback if categorization isn't working

### Review Cadence

| Frequency | Action |
|-----------|--------|
| **Daily** | Feed Claude content as it comes up |
| **Weekly** | Review the week's ingestions, ask Claude for a summary |
| **Monthly** | Complete a Monthly Review (ask Claude or use the template) |
| **Yearly** | Annual reflection, prune stale areas, set new priorities |
