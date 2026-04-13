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
6. **Scope should reference the folder or feature**: `(vault)`, `(templates)`, `(daily)`, `(projects)`, `(knowledge)`, `(resources)`, `(meta)`, `(obsidian)`, `(inbox)`

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
│   ├── Articles/            # Articles, essays, YouTube videos, podcasts, research papers
│   ├── Books/               # Book notes and summaries
│   ├── Courses/             # Course notes and tutorials
│   ├── People/              # Person profiles and thought leaders
│   ├── Tools/               # Software and tool documentation
│   └── Web/                 # Webpages, bookmarks, social media
├── 05 - Knowledge/          # Permanent notes (Zettelkasten)
│   ├── Fleeting Notes/      # Quick, unrefined ideas
│   ├── Permanent Notes/     # Atomic, refined insights
│   └── MOCs/                # Maps of Content (topic indexes)
├── 06 - Archive/            # Completed/inactive items
├── 90 - Templates/          # Note templates
├── 91 - Attachments/        # Images, PDFs, media
├── 99 - Meta/               # System documentation
└── .obsidian/               # Obsidian settings (git-tracked, minus UI state)
```

## Tag Conventions

- `#type/daily`, `#type/fleeting`, `#type/literature`, `#type/permanent`, `#type/book`, `#type/project`, `#type/moc`, `#type/review`, `#type/resource`, `#type/index`, `#type/area`
- `#type/index` — reserved for structural PARA hub/index files (Home.md, Inbox.md, Daily.md, Projects.md, Areas.md, Resources.md, Knowledge.md, Archive.md, Meta.md). These are navigation hubs, not topic-based MOCs.
- `#status/active`, `#status/on-hold`, `#status/completed`, `#status/archived`, `#status/reading` (for book/course progress), `#status/unread` (for inbox notes awaiting processing), `#status/pending-extraction` (for source notes in Resources that yielded zero permanent ideas and await re-triage)
- Topic tags: lowercase, hyphenated (e.g., `#productivity`, `#machine-learning`)
- `#source/*` tags: `book`, `article`, `youtube`, `podcast`, `social-media`, `person`, `tool`, `research`, `conversation`, `web`, `newsletter`

## Ingestion Workflow (Claude-Driven)

This vault is primarily maintained by Claude Code. The user pastes raw content into the chat, and Claude processes it into the vault.

### Ingestion Pipeline

When the user provides content, follow this pipeline:

**Step 0: Inbox Triage (before ingestion)** — Scan `00 - Inbox/` for notes tagged `#status/unread`, and scan `04 - Resources/` for notes tagged `#status/pending-extraction`. If any exist, process them through Steps 1-6 of this pipeline, then delete the `#status/unread` or `#status/pending-extraction` tag from the triaged notes. This ensures no content sits in the inbox or pending queue indefinitely. **Triage attempt tracking**: Each time a source note is re-triaged, increment a `triage_attempts` frontmatter field (initialize to 0 on first triage). If `triage_attempts` reaches 2 and the note still yields zero permanent ideas, accept it as a standalone source note with no permanent ideas and leave it as-is. If the user explicitly provides new content to ingest, triage still happens at the start of the session (before new content processing) — do not skip inbox triage.

**Pre-Ingestion: Archive (conditional)** — If the new content makes an existing note obsolete (e.g., a project is completed, a tool is deprecated), move it to `06 - Archive/` before creating new notes. Commit the archive move separately with `archive: move completed project X` before any ingestion commits.

1. **Classify** — Determine the note type based on content:
   - **Article/essay/link** → Literature Note → `04 - Resources/Articles/`
   - **Book/book summary/highlights** → Book Note → `04 - Resources/Books/`
   - **Quick idea or thought** — a single insight, observation, or half-formed idea that lacks a complete argument or actionable framework — → Fleeting Note → `05 - Knowledge/Fleeting Notes/`
   - **Refined, atomic insight** — a complete, self-contained concept with clear implications, examples, or reasoning — → Permanent Note → `05 - Knowledge/Permanent Notes/`
   - **Disambiguation**: If the content includes a mechanism, framework, or actionable takeaway with reasoning, classify as Permanent. If it's a raw thought, observation, or question without developed reasoning, classify as Fleeting. When in doubt, start with Fleeting — it can be promoted to Permanent later.
   - **Project update or plan** → Project Note → `02 - Projects/<Name>/`
   - **Ongoing responsibility / area of focus** — health, finance, team management, etc. (no fixed deadline) → Area Note → `03 - Areas/<Name>/`
   - **Meeting notes / daily log** → Daily Note → `01 - Daily/YYYY-MM-DD.md`
   - **Course notes / tutorials** → Literature Note → `04 - Resources/Courses/`
   - **Person profile / thought leader** → Resource Note → `04 - Resources/People/`
   - **Software / tool documentation** → Resource Note → `04 - Resources/Tools/`
   - **Webpage / bookmark** → Resource Note → `04 - Resources/Web/`
   - **YouTube video / video essay** → Literature Note → `04 - Resources/Articles/` (use Literature Note template with `type: "video"` and `#source/youtube` tag)
   - **Podcast episode / audio** → Literature Note → `04 - Resources/Articles/` (use Literature Note template with `type: "podcast"` and `#source/podcast` tag)
   - **Social media post / thread** → Resource Note → `04 - Resources/Web/` (use Resource Note template with `#source/social-media` tag)
   - **Research paper / PDF** → Literature Note → `04 - Resources/Articles/` (use Literature Note template with `type: "paper"` and `#source/research` tag)
   - **Newsletter** → Literature Note → `04 - Resources/Articles/` (use Literature Note template with `type: "newsletter"` and `#source/newsletter` tag)
   - **Code snippet / GitHub repo / tool discovery** → Resource Note → `04 - Resources/Tools/` (use Resource Note template with `#source/tool` tag; capture repo URL, key features, and why it's useful)
   - **Infographic / chart / diagram** → Resource Note → `04 - Resources/Web/` (save image to `91 - Attachments/`, use Resource Note template with description and key takeaways)
   - **Conversation transcript** → Save the transcript as a Resource Note in `04 - Resources/Web/` with `#source/conversation`, then extract key ideas as Fleeting or Permanent Notes (two-part ingestion — see Rule 9)
   - **Hybrid content (e.g., book review + personal reflection)** → Classify by external source first (Book Note, Literature Note, Resource Note). Extract any personal reactions, opinions, or reflections as separate Fleeting Notes. The source-driven note captures the external content; the fleeting note captures the user's reaction.
   - **Monthly/periodic review** → Review Note → `99 - Meta/Reviews/YYYY-MM.md` with `#type/review` tag (use Monthly Review Template)
   - **Unclassified / ambiguous** → If none of the above categories clearly fit, place it in `00 - Inbox/` with a note about what it might be, and flag it for the user to triage

2. **Process** — Apply the appropriate template from `90 - Templates/`
   - **Create the source note FIRST** (literature/book/resource/fleeting note) before any derived permanent notes. This ensures the source file exists for linking.
   - Fill in frontmatter (created date, tags, source)
   - Write content in the user's voice — summarize, don't copy-paste verbatim
   - Extract key ideas as separate bullet points or sections
   - **Short-circuit check**: If a source note (literature/book/resource) yields zero extractable permanent ideas, skip the Link step (3b–3g), the Orphan check (4a), and MOC step (5). **Still apply Step 4 (Tag)** — add `#type/*`, topic tags, and `#source/*` as normal. Also tag it with `#status/pending-extraction` to flag for later processing, then proceed to Commit. Initialize `triage_attempts: 1` in frontmatter. Tagged notes will be reprocessed during the next session's Inbox Triage step (Step 0) — each re-triage increments the counter. If `triage_attempts` reaches 2 and the note still yields zero permanent ideas, accept it as a standalone source note with no permanent ideas and leave it as-is.

3. **Link** — Connect to existing notes (execute in this order):
   a. The new note file has already been created in Step 2
   b. **Link to source (if applicable)**: If this note was derived from a source literature/book/resource note, link the new note back to it via `[[wiki-link]]`. If the note was created directly (e.g., a standalone permanent or fleeting idea with no source), skip this step.
   c. Edit the source note to add a forward link to the new note under the `## Connections` section. If that section does not exist in the source note, create it at the end of the note body (after all content).
   d. **Bidirectional topical linking**: Determine the topical keywords/concepts from the note's content (these will become topic tags in Step 4). Use Grep to find existing notes sharing these topics (search for matching topic tags or keywords in their content). For each related note found, add **bidirectional** links: add links to the existing notes *in the new note's* `## Connections` section, AND edit each existing note's `## Connections` section to add a link back to the new note. **If no topically related notes exist**, link to `05 - Knowledge/Knowledge.md` as a fallback.
   e. **Sibling linking**: When multiple permanent notes are derived from the same source note, link them to each other in their `## Connections` sections (they are topically related by construction).
   f. **Link placement**: Always use the `## Connections` section for new wiki-links between content notes. For permanent notes derived from a source note, also add them under `## Derived Permanent Notes` in the source note (create this section if absent) — this is in addition to `## Connections`, not a replacement. Never duplicate an existing link — check before adding.
   g. **Fleeting note linking**: Fleeting notes created via the pipeline must be linked to at least one topically related existing note. Use Grep to find notes sharing a topic tag (determine intended topics from the note's content; tags will be applied in Step 4). If no topically related note exists, link to `05 - Knowledge/Knowledge.md` as a fallback. Place the link in the fleeting note's `## Connections` section.

4. **Tag** — Apply relevant tags
   - Always include the `#type/*` tag matching the note type
   - Add topic tags based on content (lowercase, hyphenated)
   - **Source tags**: Add `#source/*` tags (e.g., `#source/youtube`, `#source/book`, `#source/article`) **only to source notes** (literature, book, resource, fleeting notes with a clear origin). **Do NOT add `#source/*` tags to derived permanent notes** — their provenance is captured via frontmatter and backlinks.

   **Type-to-tag mapping**:
   | Note type          | `#type/*` tag       |
   |--------------------|---------------------|
   | Literature Note    | `#type/literature`  |
   | Book Note          | `#type/book`        |
   | Resource Note      | `#type/resource`    |
   | Fleeting Note      | `#type/fleeting`    |
   | Permanent Note     | `#type/permanent`   |
   | Project Note       | `#type/project`     |
   | Area Note          | `#type/area`        |
   | Daily Note         | `#type/daily`       |
   | MOC                | `#type/moc`         |
   | Review Note        | `#type/review`      |
   | Index/Hub          | `#type/index`       |

4a. **Orphan check** (runs after tagging so topic tags are available): After creating a permanent note, verify it has at least one incoming wiki-link from another note. Use Grep to search for `\[\[Note Title\]\]` across all .md files. If it has no incoming links, find the most topically related existing note (use Grep to find notes sharing the permanent note's topic tags) and place a link to the orphaned permanent note in **that topically related note's** `## Connections` section. **If no topically related note exists** (this is the first note on a new topic), link to `05 - Knowledge/Knowledge.md` as a fallback.

5. **MOC (create or update)** — After tagging, check if MOC action is needed:
   - **Definition of "topic"**: For MOC purposes, a "topic" is any individual topic tag (e.g., `#productivity`). When a permanent note has multiple topic tags, each tag is a candidate topic for its own MOC. Evaluate each topic tag independently.
   - **Note counting**: Count only files tagged `#type/permanent`. **Exclude files tagged `#type/moc` and `#type/fleeting`** — MOCs are index files, fleeting notes are unrefined ideas, neither counts as a permanent note for MOC threshold purposes.
   - **Create**: After adding a permanent note, count all permanent notes sharing each topic tag. If the count for any topic is >= 5 and no MOC exists for that topic in `05 - Knowledge/MOCs/`, create one:
     - Name it `MOC - Topic.md` where "Topic" is the capitalized topic tag (e.g., `MOC - Productivity.md`)
     - Use the MOC Template from `90 - Templates/`
     - Populate frontmatter with `created` date and `#type/moc` tag
     - Add links to all related permanent notes under the "Permanent Notes" section (flat list, no sub-theme grouping required)
     - Add a `## Related MOCs` section if other MOCs share overlapping topics (notes that appear in multiple MOCs indicate overlapping topics). When creating a new MOC, also update existing MOCs that share topics to reference the new MOC in their `## Related MOCs` section.
     - Add a link to the new MOC in `05 - Knowledge/Knowledge.md` under the MOCs section (append as `- [[MOC - Topic]]` after the first bullet in that section)
   - **Update**: If an MOC already exists for the topic, add the new permanent note's link to the MOC's "Permanent Notes" section. Do not duplicate existing links.
   - **Below-threshold signal**: If more than 3 permanent notes share a topic tag but have no MOC, **and fewer than half of those notes have bidirectional links to other permanent notes on the same topic**, create the MOC anyway. This catches the bootstrap phase where the first few permanent notes on a topic are weakly connected, preventing `Knowledge.md` from becoming a dumping ground.
   - **MOC lifecycle**: If a permanent note's topic tags change after MOC creation, remove its link from the old MOC and add it to the new MOC (if the new topic warrants one). If all permanent notes in a MOC are archived or deleted, archive the MOC itself. Before creating a new MOC, check for existing MOCs with similar topics (e.g., `#productivity` vs `#time-management`) to avoid duplicates — if a close match exists, use the existing MOC instead.

6. **Commit** — Create a small, focused commit
   - One ingestion = one commit, **unless the pipeline explicitly requires multiple commits** (e.g., conversation transcripts require 2 commits per Rule 9, archive moves require a separate commit per the Pre-Ingestion Archive step). In multi-commit cases, make each commit as soon as its work is complete rather than batching.
   - Use `feat(resources):` for articles/books/courses/people/tools/web/newsletters, `feat(knowledge):` for fleeting/permanent notes, `feat(daily):` for daily notes, `feat(projects):` for project notes and updates, `feat(inbox):` for items placed in the inbox for triage, or `feat(areas):` for ongoing-responsibility area notes
   - Include what was ingested and where it was placed
   - If multiple items are ingested in one session, make separate commits per item. Each commit message must be unique and under 72 characters — vary the description by referencing the specific source title, author, or a distinguishing topic. For similar titles, include a distinguishing detail (author, key topic, or source domain) to ensure uniqueness.
   - If a commit fails mid-batch, **attempt `git add` + `git commit` again once**. If it fails a second time, **stash the uncommitted files** (`git stash push` with specific file paths) and continue with the remaining items. Note the failure and stashed files in the session summary so they can be recovered.

### Content Processing Rules

1. **Write in the user's voice** — Summarize and synthesize, don't just copy-paste. The note should read like the user wrote it for their own reference:
   - Use clear, direct language. Prefer short sentences over long ones
   - Write in first-person analytical tone ("This suggests that...", "Key takeaway:...")
   - Blockquote direct quotes from sources with attribution
   - Compress aggressively: a 2000-word article should become 3-5 key bullets, not a paragraph-by-paragraph summary
   - Capture why the idea matters, not just what it says
2. **One idea per permanent note** — If the source covers multiple ideas, create multiple permanent notes. Each permanent note should be answerable by a single declarative title (e.g., "Compound Effect" not "Ideas from Book X"). If a bullet point introduces a new mechanism, framework, or actionable insight distinct from the preceding one, it warrants its own permanent note. Limit to 3-5 permanent notes per source to prevent over-extraction.
3. **Always capture the source** — URL, book title, conversation partner, etc. in frontmatter.
4. **Flag connections** — If the new content relates to existing notes, explicitly mention the connection in a `## Connections` section.
5. **Inherit tags during extraction** — When creating Permanent Notes from a Literature, Book, or Resource Note:
   - Inherit the source's topic tags (the `#topic-name` tags, not the `#source/*` tags — provenance is captured via frontmatter and backlinks, not source tags on permanent notes)
   - Add `#type/permanent` as the primary type tag
   - If the source has no topic tags, derive 1-3 topic tags from the content. Derive them from the most prominent nouns, concepts, or frameworks in the content. Use the existing tag vocabulary where possible (search the vault for similar tags before creating new ones). Prefer established tags over novel ones to maintain a controlled vocabulary.
6. **Bootstrap exception** — If the vault has fewer than 3 permanent notes, link the new permanent note to `05 - Knowledge/Knowledge.md` as the parent index. From the 3rd permanent note onward, link to at least one topically relevant existing permanent note. If no topically relevant permanent note exists, link to `05 - Knowledge/Knowledge.md` as a fallback instead of forcing an unrelated link.
7. **Handle ambiguity** — If unsure where something belongs, place it in `00 - Inbox/` with a note about what it might be, and flag it for the user to triage (this fallback is also integrated as the last option in Classify)
8. **Bulk ingestion** — For multiple items (e.g., a list of articles), process them sequentially with individual commits. Provide the batch summary as a message to the user at the end of the session, listing each item and its destination.
9. **Conversation transcript processing** — This is a two-part ingestion with two commits:
   - Commit 1: `feat(resources): save conversation transcript with [person/topic]` — Save the transcript as a Resource Note in `04 - Resources/Web/` with `#source/conversation`.
   - Commit 2: `feat(knowledge): extract N fleeting/permanent notes from [person] conversation` — Create the derived fleeting/permanent notes, link them back to the transcript, and edit the transcript note to add links to all derived notes under its `## Derived Permanent Notes` section.
10. **Fleeting note tag inheritance** — Fleeting notes extracted from a source note inherit the source's topic tags (same rule as permanent notes), plus `#type/fleeting`. This ensures fleeting and permanent notes about the same idea share consistent topic tags.

### Example Ingestion

User pastes an article about productivity. Claude:
1. Creates a Literature Note in `04 - Resources/Articles/` using the Literature Note Template
2. Extracts 2-3 key ideas as Permanent Notes in `05 - Knowledge/Permanent Notes/`
3. Links the permanent notes back to the literature note AND adds links from the literature note to the new permanent notes
4. Commits with: `feat(resources): ingest time blocking article + extract 2 permanent notes`

## Rules

1. Never delete content — move to `06 - Archive/` instead
2. Always use `[[wiki-links]]` for internal connections
3. All notes must have YAML frontmatter with at least `created` date and `tags`
4. Keep Obsidian config files (`.obsidian/`) tracked in git for reproducibility, except volatile UI state files (workspace.json, graph.json)
5. Do not commit `91 - Attachments/` binary files if they are large — use `.gitignore` for files over 1MB
6. After each ingestion session, provide a summary to the user of what was added and where
