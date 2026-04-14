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
│   └── Reviews/             # Monthly/periodic review notes
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

**Step 0: Inbox Triage (before ingestion)** — Scan `00 - Inbox/` for notes tagged `#status/unread`, and scan `04 - Resources/` for notes tagged `#status/pending-extraction`. Process pending-extraction notes first, then unread inbox notes. For each note found, process it through Steps 1-6 of this pipeline. **Tag removal after triage**: After Steps 1-6 complete, handle status tags as follows:
  - For `#status/unread` notes: if the note was successfully classified and processed (placed in a final destination), remove the tag. If the note remained unclassified and was placed back in `00 - Inbox/`, **keep the `#status/unread` tag** so it will be retried next session.
  - For `#status/pending-extraction` notes: check `triage_attempts` **after** processing. If `triage_attempts` is 2, remove the tag (accept as standalone). If `triage_attempts` is less than 2 and the note still yielded zero permanent ideas (Step 2 re-added `#status/pending-extraction`), **do NOT remove the tag** — leave it so Step 0 will pick it up again next session. If the note yielded permanent ideas during re-triage, remove the tag (it succeeded).
This ensures no content sits in the inbox or pending queue indefinitely. **Triage attempt tracking**: Each source note gets a `triage_attempts` frontmatter field, initialized to `0` on first creation. Each time Step 0 re-triages a `#status/pending-extraction` note, increment `triage_attempts` by 1 **before** re-processing. If `triage_attempts` reaches 2 and the note still yields zero permanent ideas after re-processing, accept it as a standalone source note with no permanent ideas: remove the `#status/pending-extraction` tag (do not re-add it in Step 2) and leave it as-is. If the user explicitly provides new content to ingest, triage still happens at the start of the session (before new content processing) — do not skip inbox triage.

**Pre-Ingestion: Archive (conditional)** — If the new content makes an existing note obsolete (e.g., a project is completed, a tool is deprecated), move it to `06 - Archive/` before creating new notes. If the archive move fails (file not found, wrong path):
  - **Abort** if the contradictory content would cause a direct conflict (e.g., creating a note with the same title, or the new content explicitly references the old note as replaced).
  - **Proceed with ingestion** but flag the archive failure to the user if the archive move was for a different, non-conflicting note.
Commit the archive move separately with `archive: move completed project X` before any ingestion commits.

1. **Classify** — Determine the note type based on content. Apply rules using this priority hierarchy: first check if the content matches a **specific** rule below; if multiple rules match, prefer the most specific one (e.g., "Social media post / thread" beats "Webpage / bookmark" beats "Article/essay"). The rules are listed in approximate order, but specificity always wins over generality:
   - **Article/essay** → Literature Note → `04 - Resources/Articles/`
   - **Link (URL only)** → Inspect the URL domain to determine what it points to, then apply the matching specific rule below (YouTube → YouTube rule, Twitter/X → social media rule, GitHub → code snippet rule, etc.). Known blog/newsletter platforms: Medium, Substack, Dev.to, Hashnode → treat as Article/essay or Newsletter based on content. Do not default to Articles/ for generic links. **If the URL domain does not match any specific rule, classify it as "Webpage / bookmark"** (Resource Note → `04 - Resources/Web/`).
   - **Book/book summary/highlights** → Book Note → `04 - Resources/Books/`
   - **User-original content (own analysis, framework, decision, insight)** — content the user authored themselves, not derived from any external source → If it's a complete, self-contained concept with clear implications, examples, or reasoning → Permanent Note → `05 - Knowledge/Permanent Notes/`. If it's a raw thought, observation, or half-formed idea → Fleeting Note → `05 - Knowledge/Fleeting Notes/`. **Disambiguation**: Same mechanism/framework/actionable-takeaway test as derived content. Tag with `#type/permanent` or `#type/fleeting` as appropriate. No `#source/*` tag — provenance is "original." Capture author in frontmatter as `author: "self"`.
   - **Quick idea or thought** — a single insight, observation, or half-formed idea that lacks a complete argument or actionable framework — → Fleeting Note → `05 - Knowledge/Fleeting Notes/`
   - **Refined, atomic insight** — a complete, self-contained concept with clear implications, examples, or reasoning — → Permanent Note → `05 - Knowledge/Permanent Notes/`
   - **Note**: The "Quick idea" and "Refined, atomic insight" rules above apply to content with an external source. For user-original content, use the user-original content rule (line 144) which has its own disambiguation.
   - **Disambiguation (for derived content from external sources)**: If the content includes a mechanism, framework, or actionable takeaway with reasoning, classify as Permanent. If it's a raw thought, observation, or question without developed reasoning, classify as Fleeting. When in doubt, start with Fleeting — it can be promoted to Permanent later. **For user-original content, use the same mechanism/framework/actionable-takeaway test** (the inline disambiguation on the user-original content line above).
   - **Project update or plan** → Project Note → `02 - Projects/<Name>/`
   - **Ongoing responsibility / area of focus** — health, finance, team management, etc. (no fixed deadline) → Area Note → `03 - Areas/<Name>/`
   - **Daily log** → Daily Note → `01 - Daily/YYYY-MM-DD.md`
   - **Meeting notes** → Create a standalone note in `02 - Projects/<Name>/` if the meeting is project-specific, or `03 - Areas/<Name>/` if area-specific. Use the meeting date in the filename (e.g., `2026-04-13 - Project Kickoff.md`). Tag with `#type/project` or `#type/area` (matching the folder's type). If the meeting is not tied to a specific project or area (e.g., a networking lunch, a casual conversation), place it in `00 - Inbox/` with a note about what it might be. Do not append to a dated daily note unless the meeting notes are truly part of a daily log.
   - **Course notes / tutorials** → Literature Note → `04 - Resources/Courses/`
   - **Person profile / thought leader** → Resource Note → `04 - Resources/People/`
   - **Software / tool documentation** → Resource Note → `04 - Resources/Tools/`
   - **Webpage / bookmark** → Resource Note → `04 - Resources/Web/`
   - **YouTube video / video essay** → Literature Note → `04 - Resources/Articles/` (use Literature Note template with `type: "video"` and `#source/youtube` tag)
   - **Podcast episode / audio** → Literature Note → `04 - Resources/Articles/` (use Literature Note template with `type: "podcast"` and `#source/podcast` tag)
   - **Social media post / thread** → Resource Note → `04 - Resources/Web/` (use Resource Note template with `#source/social-media` tag)
   - **Research paper** → Literature Note → `04 - Resources/Articles/` (use Literature Note template with `type: "paper"` and `#source/research` tag). **Classify by content, not format**: a research paper is defined by its characteristics. If it has at least three of the following — hypothesis, methodology, empirical results, literature review, peer review — it is a research paper. A financial report, slide deck, or legal contract is not a research paper — classify it as a Resource Note in the appropriate folder instead.
   - **Newsletter** → Literature Note → `04 - Resources/Articles/` (use Literature Note template with `type: "newsletter"` and `#source/newsletter` tag). Newsletters often contain multiple distinct articles — treat each distinct article/section as a separate source if they cover unrelated topics.
   - **Code snippet / GitHub repo** → Resource Note → `04 - Resources/Tools/` (use Resource Note template with `#source/tool` tag; capture repo URL, key features, and why it's useful). **Exception**: If the code snippet contains a reusable pattern, algorithm, or architectural insight that stands on its own as knowledge, first create a Permanent Note for the underlying concept, then create the Resource Note as the source.
   - **Infographic / chart / diagram** → Resource Note → `04 - Resources/Web/` (save image to `91 - Attachments/`, use Resource Note template with description and key takeaways). Apply the `#source/*` tag based on actual provenance (e.g., `#source/research` if from a paper, `#source/social-media` if from Twitter), not the folder it's stored in.
   - **Conversation transcript** → Save the transcript as a Resource Note in `04 - Resources/Web/` with `#source/conversation`, then extract key ideas as Fleeting or Permanent Notes (two-part ingestion — see Rule 9)
   - **Hybrid content (e.g., book review + personal reflection)** → Classify by external source first (Book Note, Literature Note, Resource Note). Extract any personal reactions, opinions, or reflections as separate Fleeting Notes — unless the reaction is a complete, self-contained analysis with clear implications, in which case extract it as a Permanent Note. The source-driven note captures the external content; the extracted note captures the user's analysis.
   - **Monthly/periodic review** → Review Note → `99 - Meta/Reviews/YYYY-MM.md` with `#type/review` tag (use Monthly Review Template). Create the `Reviews/` subfolder if it does not exist.
   - **Unclassified / ambiguous** → If none of the above categories clearly fit, place it in `00 - Inbox/` with a note about what it might be, and flag it for the user to triage

2. **Process** — Apply the appropriate template from `90 - Templates/`
   - **Execution model**: Steps 1-6 are executed **per source**. Within a single source ingestion, notes are created and processed **sequentially per note** — not in batch. The order is: create source note with tags → for each derived note, create it with tags → for each derived note, run Step 3 linking (3d can see all pre-existing notes AND previously-created notes in the same batch, since they are already on disk) → after all notes are linked, run Step 5 (MOCs) → run Step 6 (commit). **Step 4 (Tag) is a specification reference only** — all tag application happens during note creation. When the pipeline references "Step 4" or "Step 4 tags," it means "apply the tag rules defined in Step 4" — which was already done during creation. Do not re-apply tags in Step 4.
   - **Re-triage handling** (Step 0): When Step 0 re-processes an existing source note (inbox triage or pending-extraction re-triage), the source note already exists on disk. **Do NOT recreate it or re-write its frontmatter.** Only update `triage_attempts` (Step 0 handles this before re-processing). Re-run the extraction logic (Step 2 "Extract key ideas") to derive new permanent/fleeting notes from the existing source note. Link and tag any newly derived notes per Steps 3 and the tag specification in Step 4. Preserve existing `## Connections` and `## Derived Permanent Notes` sections in the source note — only append new links.
   - **Create the source note FIRST** (literature/book/resource/fleeting note) before any derived permanent notes. This ensures the source file exists for linking.
   - Fill in frontmatter (created date, tags, source)
   - Write content in the user's voice — summarize, don't copy-paste verbatim
   - Extract key ideas as separate bullet points or sections
   - **Tag derivation happens here**: Before proceeding to Step 3, derive and apply all topic tags to the newly created note(s). Use the Rule 5 algorithm (exact-match vocabulary lookup, deduplication). This ensures tags are on disk and consistent before any linking step runs.
   - **Short-circuit check**: If a source note (literature/book/resource) yields zero extractable permanent ideas, skip the Link step (3b–3g), the Orphan check (4a), and MOC step (5). **Still apply tag logic** — add `#type/*`, topic tags, and `#source/*` using the same Rule 5 algorithm as Step 2's tag derivation, ensuring tag consistency across all paths. Also tag it with `#status/pending-extraction` to flag for later processing, then proceed to Commit.
     - **triage_attempts handling** (read in this order):
       1. **IF re-processing an existing note** (Step 0 re-triage): do NOT modify `triage_attempts` — Step 0 already incremented it before re-processing.
       2. **IF first creation** (new source note): set `triage_attempts: 0` in frontmatter.
       3. **IF `triage_attempts` is already 2** (terminating re-triage): do NOT re-add `#status/pending-extraction` — accept the note as standalone and leave it as-is.
     Step 0 will increment the counter on each re-triage. If `triage_attempts` reaches 2 and the note still yields zero permanent ideas, Step 0 removes the tag and the note is accepted as standalone.

3. **Link** — Connect to existing notes (execute in this order):
   a. The new note file has already been created in Step 2
   b. **Link to source (if applicable)**: If this note was derived from a source literature/book/resource note, link the new note back to it via `[[wiki-link]]`. If the note was created directly (e.g., a standalone permanent or fleeting idea with no source), skip this step.
   c. Edit the source note to add a forward link to the new note under the `## Connections` section. If that section does not exist in the source note, create it at the end of the note body (after all content). **If this note has no source** (user-original content, standalone permanent/fleeting idea), skip this step.
   d. **Bidirectional topical linking**: Use the topic tags that were already derived and applied to this note in Step 2 (per Rule 5). Use Grep to search for **exact topic tag matches** (e.g., `#productivity`) in existing notes' frontmatter. **Only use tag-based matching, not keyword-in-content matching** — keyword matching produces false positives (e.g., "compound" in finance vs. chemistry). Since notes are created sequentially, this grep will also find notes created earlier in the same session (same-batch and cross-source). **If no existing notes share any of the note's topic tags**, link to `05 - Knowledge/Knowledge.md` as a fallback. For each related note found via tag matching, add **bidirectional** links: add links to the existing notes *in the new note's* `## Connections` section, AND edit each existing note's `## Connections` section to add a link back to the new note. **When linking to `Knowledge.md` as a fallback, add the link bidirectionally**: add `[[Knowledge]]` to the new note's `## Connections` section, AND add `[[New Note Title]]` to `Knowledge.md` under its MOCs/index section (or create an "## Unindexed Notes" section if none exists — this section holds fallback links for notes that have no topical match and are not yet covered by an MOC).
   e. **Sibling linking**: When multiple permanent notes are derived from the same source note, link them to each other in their `## Connections` sections (they are topically related by construction). **Also link fleeting notes to permanent notes from the same source**: for each fleeting note created in the same batch, add a link to it in each same-source permanent note's `## Connections` section, and add a link back from the fleeting note to each same-source permanent note. **Also link fleeting notes to each other** if multiple are derived from the same source. **Note**: Since notes are created sequentially, 3d can find same-batch siblings that were created earlier in the sequence. 3e provides a semantic guarantee that all same-source notes link to each other regardless of whether 3d's tag matching found them.
   f. **Link placement**: Always use the `## Connections` section for new wiki-links between content notes. For permanent notes derived from a source note, also add them under `## Derived Permanent Notes` in the source note (create this section if absent) — this is in addition to `## Connections`, not a replacement. Never duplicate an existing link — check with Grep for `\[\[Note Title\]\]|\[\[Note Title\|[^]]*\]\]` (matches `[[Note Title]]` or `[[Note Title|alias]]` with proper closing brackets, but NOT `[[Note Title Something]]` or malformed lines without `]]`) in the target file before adding.
   g. **Fleeting note linking**: Fleeting notes created via the pipeline must be linked to at least one topically related existing note. Use the fleeting note's own topic tags (already applied during Step 2 creation — either inherited from the source note or derived from content per Rule 10). Use Grep to search for **exact topic tag matches** in existing notes' frontmatter. If no topically related note exists, link to `05 - Knowledge/Knowledge.md` as a fallback (bidirectionally, per 3d). Place the link in the fleeting note's `## Connections` section.

4. **Tag** — *This step was already completed during Step 2 note creation.* The rules below define the tag specification and are referenced by Step 2 and the short-circuit logic.
   - Always include the `#type/*` tag matching the note type
   - Add topic tags based on content (lowercase, hyphenated). **Deduplicate**: each tag appears at most once in the tags list.
   - **Source tags**: Add `#source/*` tags (e.g., `#source/youtube`, `#source/book`, `#source/article`) **only to source notes** (literature, book, resource notes). **Do NOT add `#source/*` tags to derived permanent notes or derived fleeting notes** — their provenance is captured via frontmatter and backlinks. User-original content (classified as permanent or fleeting with no external source) gets no `#source/*` tag; set `author: "self"` in frontmatter instead.

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

4a. **Orphan check** (runs after tagging so topic tags are available): After creating a permanent note, verify it has at least one incoming wiki-link from another note. Use Grep to search for `\[\[Note Title\]\]|\[\[Note Title\|[^]]*\]\]` (alias-aware: matches `[[Note Title]]` and `[[Note Title|alias]]` with proper closing brackets) across all .md files. **Note**: In the normal pipeline, Step 3c (source → permanent link) and Step 3d (bidirectional topical link) each guarantee at least one incoming link, so this check should not trigger. It exists as a safety net for edge cases (e.g., a standalone permanent note with no source and no topical matches that somehow missed the Knowledge.md fallback in 3d). If it has no incoming links, find the most topically related existing note (use Grep to find notes sharing the permanent note's **exact topic tags** in frontmatter) and place a link to the orphaned permanent note in **that topically related note's** `## Connections` section. **If no topically related note exists** (this is the first note on a new topic), link to `05 - Knowledge/Knowledge.md` as a fallback (bidirectionally, using the same insertion strategy as Step 3d).

5. **MOC (create or update)** — After tagging, check if MOC action is needed:
   - **Definition of "topic"**: For MOC purposes, a "topic" is any individual topic tag (e.g., `#productivity`). When a permanent note has multiple topic tags, each tag is a candidate topic for its own MOC. Evaluate each topic tag independently.
   - **Note counting**: Count only files tagged `#type/permanent`. **Exclude files tagged `#type/moc` and `#type/fleeting`** — MOCs are index files, fleeting notes are unrefined ideas, neither counts as a permanent note for MOC threshold purposes. **Also exclude files in `06 - Archive/`** — archived notes should not count toward MOC thresholds. **Count across all `.md` files on disk, including uncommitted changes** — use Grep across the filesystem, not `git grep` (which only searches tracked files).
   - **Create**: After adding a permanent note, count all permanent notes sharing each topic tag. If the count for any topic is >= 5 and no MOC exists for that topic in `05 - Knowledge/MOCs/`, create one:
     - Name it `MOC - Topic.md` where "Topic" is the capitalized topic tag (e.g., `MOC - Productivity.md`)
     - Use the MOC Template from `90 - Templates/`
     - Populate frontmatter with `created` date and `#type/moc` tag
     - Add links to all related permanent notes under the "Permanent Notes" section (flat list, no sub-theme grouping required)
     - Add a `## Related MOCs` section if other MOCs share overlapping topics (notes that appear in multiple MOCs indicate overlapping topics). When creating a new MOC, also update existing MOCs that share topics to reference the new MOC in their `## Related MOCs` section.
     - Add a link to the new MOC in `05 - Knowledge/Knowledge.md` under the MOCs section. **If the placeholder HTML comment `<!-- MOC links will be added here -->` exists, replace it with the first MOC link.** Otherwise, append as `- [[MOC - Topic]]` at the end of the MOCs section. **Prune stale links**: When creating a new MOC, remove individual `[[Note Title]]` links from `Knowledge.md`'s MOCs section or the "## Unindexed Notes" section (sections that exist solely as link indexes). **Do NOT prune links from semantically organized sections** (e.g., "Core Mental Models," "Key Concepts") — those links represent intentional thematic organization and should coexist with the MOC. **Do not prune links added in the current session** — only prune links from prior sessions to avoid removing fallback links that were just added for the same note.
   - **Update**: If an MOC already exists for the topic, add the new permanent note's link to the MOC's "Permanent Notes" section or the appropriate `### Sub-theme:` subsection. **Before adding, grep the MOC file for `\[\[Note Title\]\]|\[\[Note Title\|[^]]*\]\]` to catch both `[[Note Title]]` and `[[Note Title|alias]]` forms with proper closing brackets. Do not add a duplicate link.**
   - **Below-threshold signal**: If more than 3 permanent notes share a topic tag but have no MOC, **and fewer than half of those notes have bidirectional links to other permanent notes on the same topic**, create the MOC anyway. This catches the bootstrap phase where the first few permanent notes on a topic are weakly connected, preventing `Knowledge.md` from becoming a dumping ground. **Before creating the MOC, run the MOC similarity check** (previous bullet) — if an existing MOC has >= 50% overlap, add as sub-theme instead of creating a new MOC.
   - **MOC similarity check** (runs before creating a new MOC, including for the below-threshold signal path): Before creating `MOC - Topic.md`, check for existing MOCs that may overlap. Use this deterministic algorithm:
     1. For each existing MOC file in `05 - Knowledge/MOCs/`, **extract its note set** by grepping all `- [[...]]` bullet-point wiki-links in the file body (excluding YAML frontmatter, `## Related MOCs`, and `## Sub-topics` sections). This captures links under `## Permanent Notes`, `### Sub-theme:`, or any other section the MOC uses for listing notes.
     2. Build the candidate topic's note set: all permanent notes sharing the candidate topic tag. **Guard**: if the candidate set is empty, skip the similarity check and proceed with normal creation logic.
     3. For each existing MOC, count how many permanent notes appear in **both** the existing MOC's note set and the candidate topic's note set.
     4. Compute the overlap ratio as `overlap / len(candidate_notes)`. Collect all existing MOCs where this ratio is **50% or more**.
     5. If no existing MOC reaches 50% overlap, proceed with creating the new MOC.
     6. If one or more existing MOCs reach 50% overlap, **select the one with the highest overlap ratio**. If two or more MOCs tie, select the one with the **most total notes** in its note set. If still tied, select the alphabetically first MOC name. Add the candidate notes to that MOC's **nested sub-theme section**: create a `### Sub-theme: Topic` heading under the `## Permanent Notes` section and list the candidate notes there as indented list items (e.g., `  - [[Note Title]]`). Also add a `## Sub-topics` section at the end of the MOC if not present: `- `Topic` — covered as sub-theme`.
   - **MOC lifecycle**: If a permanent note's topic tags change after MOC creation, remove its link from the old MOC and add it to the new MOC (if the new topic warrants one). **Before removing, grep the old MOC to confirm the link exists.** If all permanent notes in a MOC are archived or deleted, archive the MOC itself with a separate `archive: move empty MOC - Topic` commit — **commit this BEFORE the ingestion commit that triggered the lifecycle event**. If multiple MOCs become empty from the same event, batch them into a single commit. Keep the subject line under 72 characters: use `archive: move N empty MOCs` as the subject, and list the MOC names in the commit body. If the archive commit fails, abort the ingestion commit. **Sub-theme promotion**: If a `### Sub-theme:` section accumulates >= 5 notes, promote it to its own standalone MOC: create a new MOC file, move the sub-theme notes to it, remove the `### Sub-theme:` section from the parent MOC, and add a link to the new MOC in the parent MOC's `## Related MOCs` section.

6. **Commit** — Create a small, focused commit
   - One ingestion = one commit, **unless the pipeline explicitly requires multiple commits** (e.g., conversation transcripts require 2 commits per Rule 9, archive moves require a separate commit per the Pre-Ingestion Archive step, empty MOC lifecycle moves require a separate `archive:` commit). In multi-commit cases, make each commit as soon as its work is complete rather than batching.
   - Use `feat(resources):` for articles/books/courses/people/tools/web/newsletters, `feat(knowledge):` for fleeting/permanent notes, `feat(daily):` for daily notes, `feat(projects):` for project notes and updates, `feat(inbox):` for items placed in the inbox for triage, or `feat(areas):` for ongoing-responsibility area notes
   - Include what was ingested and where it was placed. Reference the **source** (article title, book name, etc.), not individual notes. "Item" in commit rules means one source and all notes derived from it.
   - If multiple sources are ingested in one session, make separate commits per source. Each commit message must be unique and under 72 characters — vary the description by referencing the specific source title, author, or a distinguishing topic. For similar titles, include a distinguishing detail (author, key topic, or source domain) to ensure uniqueness.
   - If a commit fails mid-batch, **attempt `git add` + `git commit` again once**. If it fails a second time, **stash the uncommitted files including untracked ones** (`git stash push -u -- <file paths>`) and continue with the remaining items. Note the failure and stashed files in the session summary so they can be recovered.

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
   - If the source has no topic tags, derive 1-3 topic tags from the content. Derive them from the most prominent nouns, concepts, or frameworks in the content. **Use the existing tag vocabulary with exact matching**: search the vault for `#tag-name` patterns in frontmatter across all `.md` files using Grep. Match the derived concept to an existing tag only if the tag name is an **exact string match** for the concept (e.g., derived concept "productivity" matches existing `#productivity` exactly). Do not use fuzzy or semantic matching. If no exact match exists, create a new tag using the derived concept name (lowercase, hyphenated). Prefer established tags over novel ones to maintain a controlled vocabulary.
   - **Deduplicate tags**: Before writing frontmatter, remove any duplicate tags. A tag should appear at most once in the tags list.
6. **Bootstrap exception** — If the vault has fewer than 3 permanent notes **before this session began** (count all existing `.md` files tagged `#type/permanent` on disk, including uncommitted, but **exclude the note(s) being created in the current session**), the new permanent note must be linked to `05 - Knowledge/Knowledge.md` as the parent index (bidirectionally, per Step 3d). **From the 3rd permanent note onward** (i.e., when the vault already has 3+ permanent notes on disk before the current session), link to at least one topically relevant existing permanent note via Step 3d. If no topically relevant permanent note exists, link to `05 - Knowledge/Knowledge.md` as a fallback instead of forcing an unrelated link. **Rule 6 sets a minimum guarantee, not a ceiling**: during bootstrap, the new note is linked to `Knowledge.md` *and* to any topically relevant notes found by Step 3d. Both links coexist. **Note**: Rule 6 uses a pre-session count (excluding current notes) to determine bootstrap behavior. Step 5 (MOC) uses a post-creation count (including uncommitted) to determine MOC thresholds. These are different by design — do not apply one counting methodology to the other step.
7. **Handle ambiguity** — If unsure where something belongs, place it in `00 - Inbox/` with a note about what it might be, and flag it for the user to triage (this fallback is also integrated as the last option in Classify)
8. **Bulk ingestion** — For multiple sources (e.g., a list of articles), process them sequentially with individual commits per source. "Item" in this context means one source and all notes derived from it. Provide the batch summary as a message to the user at the end of the session, listing each source and its destination.
9. **Conversation transcript processing** — This is a two-part ingestion with two commits:
   - Commit 1: `feat(resources): save conversation transcript with [person/topic]` — Save the transcript as a Resource Note in `04 - Resources/Web/` with `#source/conversation`.
   - Commit 2: `feat(knowledge): extract N fleeting/permanent notes from [person] conversation` — Create the derived fleeting/permanent notes, link them back to the transcript, and edit the transcript note to add links to all derived notes under its `## Derived Permanent Notes` section.
10. **Fleeting note tag inheritance** — Fleeting notes extracted from a source note inherit the source's topic tags (same rule as permanent notes), plus `#type/fleeting`. This ensures fleeting and permanent notes about the same idea share consistent topic tags. **If the source has no topic tags, derive 1-3 topic tags from the fleeting note's content using the Rule 5 algorithm.** **Do NOT inherit `#source/*` tags on fleeting notes** — source provenance for fleeting notes is captured via backlinks to the source note and frontmatter, the same as for permanent notes.

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
