---
name: obsidian-vault
description: >
  Interact with an Obsidian vault via the `obsidian` CLI — search, read, create,
  move, tag, link, and manage notes, properties, tasks, templates, backlinks,
  and vault structure. Use this skill whenever you want to read or modify notes
  in an Obsidian vault, search the vault for text or tags, create new notes,
  append/prepend to notes, manage properties and tags, list or toggle tasks,
  check backlinks or unresolved links, list folders/files, manage plugins or
  themes, query vault statistics, or execute any Obsidian CLI command. Also use
  when the user asks "what notes do I have about X", "search my notes", "create
  a note", "open daily note", "what tags am I using", "show my tasks", "find
  orphaned notes", or references any Obsidian CLI command.
---

# Obsidian Vault Skill

Interact with the Obsidian vault using the `obsidian` CLI. The CLI communicates
with a running Obsidian instance — Obsidian must be open for these commands to
work.

## Quick Reference

```bash
obsidian <command> [options] vault=<vault-name>
```

- `file=<name>` resolves by wikilink name (fuzzy match)
- `path=<exact/path>` is an exact file path
- Quote values with spaces: `name="My Note"`
- Use `\n` for newlines in content values
- Most commands default to the active file when file/path is omitted

## Command Reference

### Search & Discovery

```bash
# Full-text search (returns file names)
obsidian search query="search term"
obsidian search query="term" path="folder/" limit=10

# Search with matching line context
obsidian search:context query="term"

# List all tags
obsidian tags
obsidian tags counts              # include counts
obsidian tags sort=count          # sort by usage
obsidian tags format=json         # structured output

# Get details on a specific tag
obsidian tag name="#tagname"
obsidian tag name="#tagname" verbose  # include file list

# Find orphaned notes (no incoming links)
obsidian orphans
obsidian orphans total
obsidian orphans all              # include non-markdown

# Find dead-end notes (no outgoing links)
obsidian deadends
obsidian deadends total

# Find unresolved/broken links
obsidian unresolved
obsidian unresolved counts
obsidian unresolved verbose       # include source files
```

### Read Notes

```bash
# Read a note
obsidian read file="Note Name"
obsidian read path="folder/note.md"

# Get file info
obsidian file file="Note Name"

# Show headings/outline
obsidian outline file="Note Name"
obsidian outline file="Note Name" format=json

# Word count
obsidian wordcount file="Note Name"
obsidian wordcount file="Note Name" words

# Read properties
obsidian properties file="Note Name"
obsidian property:read file="Note Name" name="tags"

# Read daily note
obsidian daily:read
obsidian daily:path               # get path to today's note
```

### Create & Modify Notes

```bash
# Create a new note
obsidian create name="Note Name" content="Initial content"
obsidian create path="folder/note.md" content="..."
obsidian create name="Note Name" template="Template Name"
obsidian create name="Note" overwrite   # overwrite if exists
obsidian create name="Note" open        # open after creating
obsidian create name="Note" newtab      # open in new tab

# Append content
obsidian append file="Note" content="New section"
obsidian append file="Note" content="inline" inline   # no newline

# Prepend content
obsidian prepend file="Note" content="Header"
obsidian prepend file="Note" content="inline" inline

# Append to daily note
obsidian daily:append content="Meeting notes"
obsidian daily:prepend content="Morning plan"

# Move or rename
obsidian move file="Old" to="new-folder/New"
obsidian rename file="Old Name" name="New Name"

# Delete
obsidian delete file="Note"
obsidian delete file="Note" permanent   # skip trash
```

### Links & Connections

```bash
# Backlinks (notes linking TO this one)
obsidian backlinks file="Note Name"
obsidian backlinks file="Note" counts
obsidian backlinks file="Note" format=json

# Outgoing links (links FROM this note)
obsidian links file="Note Name"
obsidian links file="Note" total

# Aliases
obsidian aliases file="Note Name"
obsidian aliases file="Note" verbose
```

### Tasks

```bash
# List tasks
obsidian tasks
obsidian tasks todo              # incomplete only
obsidian tasks done              # completed only
obsidian tasks verbose           # grouped by file with line numbers
obsidian tasks format=json       # structured output
obsidian tasks daily             # from daily note only
obsidian tasks status="-"        # filter by status char

# Toggle or update a single task
obsidian task ref="path/file.md:12" toggle
obsidian task ref="path/file.md:12" done
obsidian task ref="path/file.md:12" todo
obsidian task ref="path/file.md:12" status="/"
```

### Templates

```bash
# List available templates
obsidian templates
obsidian templates total

# Read a template
obsidian template:read name="Template Name"
obsidian template:read name="Template Name" resolve
obsidian template:read name="Template Name" title="Note Title"

# Insert template into active file
obsidian template:insert name="Template Name"
```

### Vault Structure

```bash
# Vault info
obsidian vault
obsidian vault info=name
obsidian vault info=files
obsidian vault info=folders
obsidian vault info=size

# List known vaults
obsidian vaults
obsidian vaults verbose          # include paths

# List files
obsidian files
obsidian files folder="path/"
obsidian files ext=md
obsidian files total

# List folders
obsidian folders
obsidian folders folder="path/"  # filter by parent
obsidian folder path="folder/"   # details on one folder
```

### Properties

```bash
# List all properties in vault
obsidian properties
obsidian properties counts       # include occurrence counts
obsidian properties sort=count   # sort by frequency
obsidian properties format=json

# Set a property on a file
obsidian property:set file="Note" name="status" value="done" type=text
obsidian property:set file="Note" name="tags" value="a, b" type=list
obsidian property:set file="Note" name="rating" value="5" type=number
obsidian property:set file="Note" name="done" value="2026-04-13" type=date

# Remove a property
obsidian property:remove file="Note" name="status"
```

### Plugins & Themes

```bash
# List plugins
obsidian plugins
obsidian plugins.enabled
obsidian plugins filter=community
obsidian plugins versions
obsidian plugins format=json

# Manage plugins
obsidian plugin:install id=dataview enable
obsidian plugin:enable id=dataview
obsidian plugin:disable id=dataview
obsidian plugin:uninstall id=dataview

# Themes
obsidian themes
obsidian theme:set name="Minimal"
obsidian theme:install name="AnuPpuccin" enable
obsidian theme:uninstall name="Old Theme"
```

### Daily Notes

```bash
obsidian daily                    # open today's note
obsidian daily:read               # read contents
obsidian daily:append content="..."
obsidian daily:prepend content="..."
obsidian daily:path               # get file path
```

### Sync & History

```bash
# Sync
obsidian sync:status
obsidian sync on                  # resume
obsidian sync off                 # pause

# File history
obsidian history file="Note"
obsidian history:read file="Note" version=1
obsidian history:restore file="Note" version=1

# Sync versions
obsidian sync:history file="Note"
obsidian sync:read file="Note" version=1
obsidian sync:restore file="Note" version=1
```

### Commands & Hotkeys

```bash
# List Obsidian commands
obsidian commands
obsidian commands filter="toggle"

# List hotkeys
obsidian hotkeys
obsidian hotkeys verbose
obsidian hotkeys format=json
obsidian hotkey id="app:reload"

# Execute a command
obsidian command id="app:reload"
```

### Tabs & Workspace

```bash
# Open files
obsidian open file="Note Name"
obsidian open file="Note" newtab

# List open tabs
obsidian tabs
obsidian tabs ids

# Workspace tree
obsidian workspace
```

## Error Handling

- **Obsidian not running**: Commands fail with connection refused. Tell the user
  to open Obsidian first.
- **Vault not found**: Run `obsidian vaults` to list available vaults. Use
  `vault=<name>` to target a specific one.
- **File not found**: Use `obsidian files` to see what exists. The `file=`
  parameter resolves like wikilinks — if ambiguous, use `path=` for exact paths.
- **Template not found**: Use `obsidian templates` to list available ones.

## Tips

- Prefer `format=json` on list commands (tags, bookmarks, hotkeys, plugins,
  files, folders) for structured output.
- Use `search:context` instead of plain `search` when you need to see matching
  lines, not just file names.
- Use `obsidian unresolved` to find broken wiki-links that need fixing.
- Use `obsidian random:read folder="path/"` to surface a random note for review.
- Use `obsidian recents` to see recently opened files.
- Use `obsidian reload` if the vault state seems stale.
