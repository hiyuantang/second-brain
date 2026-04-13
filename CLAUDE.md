# CLAUDE.md — Second Brain Repository Conventions

Rules for Claude when working in this repository.

## Git Commit Conventions

### Commit Message Format

Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

### Types

| Type       | When to use                                    | Example                                  |
|------------|------------------------------------------------|------------------------------------------|
| `feat`     | New note, template, or structural addition     | `feat(vault): add book note template`    |
| `docs`     | README, CLAUDE.md, or documentation changes    | `docs: update vault structure guide`     |
| `refactor` | Reorganizing notes/folders without new content | `refactor: flatten resource subfolders`  |
| `fix`      | Correcting broken links, typos, formatting     | `fix: correct broken wiki-link in Home`  |
| `chore`    | Config changes, Obsidian settings, gitignore   | `chore: update Obsidian plugin config`   |
| `archive`  | Moving content to the archive folder           | `archive: move completed project X`      |

### Title Rules

1. **Maximum 72 characters** for the subject line
2. **Use imperative mood**: "add" not "added" or "adds"
3. **No period** at the end of the subject line
4. **Lowercase type**, lowercase scope: `feat(vault):` not `Feat(Vault):`
5. **Be specific**: `feat: add daily note template` not `feat: update stuff`
6. **Scope should reference the folder or feature**: `(vault)`, `(templates)`, `(daily)`, `(projects)`, `(knowledge)`, `(resources)`, `(meta)`, `(obsidian)`

### Body Rules (optional)

- Separate from subject with a blank line
- Explain **what** and **why**, not **how** (the diff shows how)
- Wrap at 72 characters
- Use bullet points for multiple items

### Commit Rules

- **Never add `Co-Authored-By: Claude`** or any co-author footer. The user finds this strange. Do not append it to commit messages.

### Example Commits

```
feat(vault): add PARA+Zettelkasten folder structure

Create numbered folder hierarchy combining PARA method
with Zettelkasten note types for scalable knowledge management.

feat(templates): add 8 note templates

- Daily note, fleeting note, literature note
- Permanent note, book note, project note
- MOC template, monthly review template

fix(knowledge): repair orphaned links in permanent notes

chore(obsidian): configure daily notes and templates plugin

docs: rewrite README with usage and retrieval guide
```

## Branch Naming

- `main` — the current state of the vault
- For feature work (if using branches): `feat/<description>`, `fix/<description>`, `refactor/<description>`
- Keep branches short-lived; merge back to main quickly

## File Naming

- **Daily notes**: `YYYY-MM-DD.md` (e.g., `2026-04-13.md`)
- **Project folders**: `Project Name/` (Title Case with spaces)
- **Permanent notes**: Descriptive title in Title Case.md (e.g., `Compound Effect.md`)
- **MOCs**: `MOC - Topic.md` (e.g., `MOC - Productivity.md`)
- **Templates**: `<Type> Note Template.md`
- **Resources**: Source title or descriptive name.md

## Folder Structure

```
Second Brain/
├── Home.md                  # Vault entry point
├── 00 - Inbox/              # Quick capture, pending triage
├── 01 - Daily/              # Daily notes (YYYY-MM-DD.md)
├── 02 - Projects/           # Active projects (with deadlines)
├── 03 - Areas/              # Ongoing responsibilities
├── 04 - Resources/          # Reference material from outside
├── 05 - Knowledge/          # Permanent notes (Zettelkasten)
├── 06 - Archive/            # Completed/inactive items
├── 90 - Templates/          # Note templates
├── 91 - Attachments/        # Images, PDFs, media
├── 99 - Meta/               # System documentation
└── .obsidian/               # Obsidian settings (git-tracked, minus UI state)
```

## Tag Conventions

- `#type/daily`, `#type/fleeting`, `#type/literature`, `#type/permanent`, `#type/book`, `#type/project`, `#type/moc`, `#type/review`
- `#status/active`, `#status/on-hold`, `#status/completed`, `#status/archived`
- Topic tags: lowercase, hyphenated (e.g., `#productivity`, `#machine-learning`)

## Ingestion Workflow (Claude-Driven)

This vault is primarily maintained by Claude Code. The user pastes raw content into the chat, and Claude processes it into the vault.

### Ingestion Pipeline

When the user provides content, follow this pipeline:

1. **Classify** — Determine the note type based on content:
   - **Article/essay/link** → Literature Note → `04 - Resources/Articles/`
   - **Book/book summary/highlights** → Book Note → `04 - Resources/Books/`
   - **Quick idea or thought** → Fleeting Note → `05 - Knowledge/Fleeting Notes/`
   - **Refined, atomic insight** → Permanent Note → `05 - Knowledge/Permanent Notes/`
   - **Project update or plan** → Project Note → `02 - Projects/<Name>/`
   - **Meeting notes / daily log** → Daily Note → `01 - Daily/YYYY-MM-DD.md`
   - **Conversation transcript** → Extract key ideas → Fleeting or Permanent Notes

2. **Process** — Apply the appropriate template from `90 - Templates/`
   - Fill in frontmatter (created date, tags, source)
   - Write content in the user's voice — summarize, don't copy-paste verbatim
   - Extract key ideas as separate bullet points or sections

3. **Link** — Connect to existing notes
   - Search the vault for related notes using Grep/Glob
   - Add `[[wiki-links]]` to relevant existing notes
   - If a topic has 5+ notes now (or after this addition), check if an MOC exists; if not, flag it in the commit message

4. **Tag** — Apply relevant tags
   - Always include the `#type/*` tag matching the note type
   - Add topic tags based on content (lowercase, hyphenated)
   - Add `#source/*` tags if the content has a clear source (e.g., `#source/youtube`, `#source/book`, `#source/article`)

5. **Commit** — Create a small, focused commit
   - One ingestion = one commit
   - Use `feat(resources):`, `feat(knowledge):`, or `feat(daily):` type
   - Include what was ingested and where it was placed
   - If multiple items are ingested in one session, make separate commits per item

### Content Processing Rules

1. **Write in the user's voice** — Summarize and synthesize, don't just copy-paste. The note should read like the user wrote it.
2. **One idea per permanent note** — If the source covers multiple ideas, create multiple permanent notes and link them.
3. **Always capture the source** — URL, book title, conversation partner, etc. in frontmatter.
4. **Flag connections** — If the new content relates to existing notes, explicitly mention the connection in a "Connections" section.
5. **Handle ambiguity** — If unsure where something belongs, place it in `00 - Inbox/` with a note about what it might be, and flag it for the user to triage.
6. **Bulk ingestion** — For multiple items (e.g., a list of articles), process them sequentially with individual commits. Summarize the batch at the end.

### Example Ingestion

User pastes an article about productivity. Claude:
1. Creates a Literature Note in `04 - Resources/Articles/` using the Literature Note Template
2. Extracts 2-3 key ideas as Permanent Notes in `05 - Knowledge/Permanent Notes/`
3. Links the permanent notes back to the literature note
4. Commits with: `feat(knowledge): ingest article on time blocking + extract 2 permanent notes`

## Rules

1. Never delete content — move to `06 - Archive/` instead
2. Always use `[[wiki-links]]` for internal connections
3. All notes must have YAML frontmatter with at least `created` date and `tags`
4. Keep Obsidian config files (`.obsidian/`) tracked in git for reproducibility, except volatile UI state files (workspace.json, graph.json)
5. Do not commit `91 - Attachments/` binary files if they are large — use `.gitignore` for files over 1MB
6. After each ingestion session, provide a summary to the user of what was added and where
