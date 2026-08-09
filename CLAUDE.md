# CLAUDE.md — SERA Operating Manual

> SERA = your Second Brain. Claude reads this file at the start of every session.
> This file tells Claude how your brain is organized and how to maintain it.
> Last updated: [YOUR DATE]

---

## What SERA Is

This vault is your external brain. It holds your notes, ideas, project context, the people
you work with, and a continuously-updated picture of where AI is heading. The deal is simple,
and it follows Andrej Karpathy's LLM Wiki principle:

> **You decide what to read and what questions to ask. Claude does the bookkeeping —
> filing, linking, summarizing, and keeping everything current.**

You capture raw material. Claude organizes it. Neither of us lets the vault rot.

---

## The Three Layers (Karpathy's model)

| Layer | Folders | Who owns it | Rule |
|---|---|---|---|
| **Raw / immutable** | `00-Inbox`, `04-Resources` | You capture, Claude processes | Sources of truth. Don't rewrite the original capture; compile *from* it. |
| **Compiled / maintained** | `02-Notes`, `MOCs`, `AI-Trends` | Claude writes & maintains | Evergreen notes. Keep current, cross-linked, deduplicated. |
| **Schema / rules** | This file (`CLAUDE.md`) | We co-evolve it | When we find a better workflow, update this file. |

---

## Folder Map

| Folder | What goes here |
|---|---|
| `00-Inbox/` | Everything you capture, unprocessed. Pasted articles, half-thoughts, links. **Process these first every session.** |
| `01-Daily/YYYY/` | Daily notes (`YYYY-MM-DD.md`). Session logs, what you learned, what you're thinking about. |
| `02-Notes/Concepts/` | Evergreen notes — one idea per note. The core of your knowledge. |
| `02-Notes/People/` | One note per person you work with or follow. |
| `02-Notes/Places/` | Locations, companies, institutions. |
| `02-Notes/Ideas/` | Raw ideas worth developing. |
| `03-Projects/` | Active projects — plans, tasks, deadlines, current state. |
| `04-Resources/` | Reference material — articles, links, tools, papers (the raw sources). |
| `04-Resources/Papers/` | **arXiv paper notes** — one per paper. Subfiles: `_Papers-MOC`, `_Reading-Queue`, `_Author-Watch`, `_Topics`, `_Weekly-Digest`. |
| `05-Archive/` | Completed or stale items. Nothing is deleted; it's archived. |
| `MOCs/` | Maps of Content — hub notes that connect big ideas. See `MOCs/Home.md`. |
| `AI-Trends/` | Your living picture of AI developments. The thing that keeps you current. |
| `Templates/` | Note templates. Use these so every note has consistent frontmatter. |

---

## Session Start Protocol

At the beginning of every conversation, do this before anything else:

1. Read this file (`CLAUDE.md`).
2. Read `MOCs/Home.md` — the dashboard and entry point.
3. Check `00-Inbox/` — if there are unprocessed items, report how many and offer to process them.
4. Read today's daily note in `01-Daily/YYYY/` (create it if it doesn't exist).
5. Glance at the most recent daily note to recall where we left off.
6. **Give a 2-3 sentence orientation**: what was last worked on, what's in the inbox,
   anything that needs attention. Then ask what to do.

---

## The Daily Flow (Capture → Process → Organize → Connect → Review)

This is the engine. You handle Capture; Claude handles the middle; Review is done together.

**1. Capture (you)** — Dump anything into `00-Inbox/` with zero friction. No organizing.

**2. Process (Claude)** — For each inbox item, decide what it is:
   - A reference/article → file in `04-Resources/`, then compile its key ideas into `02-Notes/`
   - A fact or concept → create/update an evergreen note in `02-Notes/Concepts/`
   - A person → `02-Notes/People/`
   - A task or project thing → `03-Projects/`
   - A raw idea → `02-Notes/Ideas/`
   After processing, the inbox item is moved or deleted (its compiled knowledge now lives in Notes).

**3. Organize (Claude)** — Put it in the right folder. Add intentional tags (tags that help
   *find*, not just collect). Add complete frontmatter from the template.

**4. Connect (Claude)** — This is the most important step. Link the new note to related notes
   with `[[wikilinks]]`. Update the relevant MOC. A note with no links is a dead note.

**5. Review (together)** — Daily: update today's note. Weekly: run a review
   (see Review Protocol). Monthly: lint the whole vault.

---

## Note Creation Rules

- **Always** use the matching template in `Templates/` — every note gets full frontmatter.
- **One idea per note** (atomic). If a note covers three things, split it into three.
- **Name clearly** — descriptive, consistent titles. `attention-mechanism.md`, not `notes2.md`.
- **Link everything** — every new note links to at least 2 related notes, and gets added to a MOC.
- **Tags are for finding** — use a small, consistent tag set. Don't invent a new tag per note.

---

## Memory Rules (how Claude remembers you across sessions)

- When you express a **preference** ("I like concise answers", "I always use Python") →
  add it to `MOCs/Home.md` under "About Me".
- When you mention a **new project** → create a note in `03-Projects/`.
- When you have a **good idea** during a conversation → capture it in `02-Notes/Ideas/`.
- When we reach a **good answer** to a real question → file it as an evergreen note in
  `02-Notes/Concepts/` so the next session benefits (Karpathy's "queries compound" principle).
- At the **end of a working session**, update today's daily note with what was done,
  what you learned, and any open loops.

---

## arXiv Research Paper Protocol

There is a dedicated paper research system in `04-Resources/Papers/`.

**No external tools or installations required.** Two built-in tools are always available:
- `web_search("site:arxiv.org [query]")` — finds arXiv papers via web search
- `web_fetch("https://arxiv.org/abs/[id]")` — fetches full paper details (title, authors, abstract, date, categories)

This is safer and simpler than any MCP server — nothing touches the filesystem except the vault.

### The Five Paper Commands

**"search arxiv for [topic]"**
1. Run `web_search("site:arxiv.org [topic] [YEAR]")` — add category hints like `cs.AI` or `cs.LG` for precision
2. From search results, identify 5–8 candidate arXiv URLs
3. `web_fetch` each abstract page to get structured metadata (title, authors, date, arxiv_id, abstract)
4. Present results: title, authors, year, one-line summary
5. Ask which to ingest — never auto-ingest
6. For confirmed papers: create note in `04-Resources/Papers/` using `paper-template.md`
7. Add to `_Reading-Queue.md` at priority specified (default: medium)
8. Link paper note to relevant `AI-Trends/` or `02-Notes/Concepts/` note

**"run weekly paper digest"**
1. Read `04-Resources/Papers/_Topics.md` for the full topic list
2. For each topic: `web_search("site:arxiv.org [topic query] [YEAR]")`, filter results to last 7 days by checking submission dates via `web_fetch`
3. Skip papers whose `arxiv_id` already exists anywhere in vault (search vault first)
4. Write a new dated section in `04-Resources/Papers/_Weekly-Digest.md`
5. Add standout papers (≥2 topics affected, highly relevant) to `_Reading-Queue.md`
6. Update "Last Digest" dates in `_Topics.md`

**"check author updates"**
1. Read `04-Resources/Papers/_Author-Watch.md`
2. For each author: `web_search("site:arxiv.org [Author Name] [YEAR]")` + check dates
3. `web_fetch` abstract pages for candidates to confirm authorship and get metadata
4. Surface papers from the last 30 days not already in vault
5. Ask which to add to queue; update "Last checked" dates

**"I finished [paper]"**
1. Update paper note `status`: `reading` → `read`, set `read_date`
2. Extract key claims → compile as a dated entry into the relevant `AI-Trends/` note
3. If foundational: create/update a `02-Notes/Concepts/` note
4. Check if this paper resolves any open gap in `AI-Trends/` → update if yes
5. Log in today's daily note

**"fill gap: [description]"**
1. Read the "What I Should Learn Next" section of the relevant `AI-Trends/` note
2. `web_search("site:arxiv.org [gap keywords]")` targeted at filling that exact gap
3. Surface 3–5 candidates with summaries; ask which to queue

### Paper Ingestion Rules
- Always store `arxiv_id` in frontmatter — it is the dedup key (e.g., `2605.26252`)
- Before ingesting: run `sera:search_notes(arxiv_id)` to check for duplicates
- Paper notes in `04-Resources/Papers/` are raw sources — never edited after creation
- Compiled knowledge goes in `02-Notes/Concepts/` and `AI-Trends/` (the wiki layer)
- One paper can and should update multiple concept/trend notes

---

## AI-Trends Protocol (staying current)

A mix of: deep technical (papers, architectures), applied (tools, products),
and broad awareness (releases, big shifts). The `AI-Trends/` folder is how Claude keeps you current.

When you ask to "check AI trends" or "what's new", or when you paste something AI-related:

1. **Search the web** for recent developments.
2. **Compare against what's already in `AI-Trends/`** — only surface what's genuinely new.
3. **Update the relevant trend note**, don't create duplicates. Each major area gets one
   living note (e.g. `AI-Trends/agent-memory.md`, `AI-Trends/model-releases.md`).
4. **Record the date** of each update inside the note so you can see how a topic evolved.
5. **Link new trend notes** to existing `02-Notes/Concepts/` so trends connect to what you know.
6. **Flag what to learn next** based on gaps between trends and existing notes.

See `AI-Trends/_index.md` for the structure.

---

## Review Protocol

**Weekly review** (when you say "weekly review"):
- Summarize what was learned and worked on this week (from daily notes).
- List any inbox items still unprocessed.
- Surface notes that have no links (orphans) and suggest connections.
- Tell what's changed in AI this week that's relevant to your notes.
- Suggest 1-2 things worth focusing on next week.

**Monthly lint** (when you say "lint the vault"):
- Find orphan notes (no inbound links) and suggest where to link them.
- Find stale notes (especially in `AI-Trends/` — AI moves fast).
- Find contradictions between notes.
- Find concepts mentioned in `[[links]]` that don't have a note yet.
- Suggest what to archive to `05-Archive/`.
- Output a short lint report; don't make destructive changes without asking.

---

## Claude Conversation Memory (Long-Term Memory via SERA)

Every meaningful Claude conversation is migrated as a structured note into `05-Archive/History/`. These notes are Claude's long-term memory across sessions — searchable reference, not live graph nodes.

### Graph Topology (important — do not break this)

```
MOCs/Home  ←——  MOCs/claude-memory-index  ←——  05-Archive/History/*.md
```

- `MOCs/Home` links **to** `MOCs/claude-memory-index` (one edge, one direction)
- `MOCs/claude-memory-index` links **to** every history note
- History notes link **only back** to `MOCs/claude-memory-index` — never into active notes, never into `02-Notes/`, `03-Projects/`, `AI-Trends/`, or `MOCs/Home`
- This keeps the history subgraph contained; it never clutters the live knowledge graph

### Template
All migrations use `Templates/claude-conversation-template.md`.
Filing path: `05-Archive/History/YYYY-MM-DD-{slug}.md`

### What Gets Migrated (signal, not noise)
Migrate a conversation when it produces:
- A reusable pattern, snippet, or workflow
- A key architectural or design decision
- A lesson from something that failed
- Interview prep or career strategy that may recur
- A project summary worth referencing later

**Do NOT migrate:** one-off trivia, simple factual Q&A, single debugging fixes with no broader lesson.

### Update Gate — when to append to an existing history note
When a future conversation revisits a past topic, update the relevant note **only if** the new information is:
1. **New and non-obvious** — not already implied by what's there
2. **Durable** — a pattern or lesson that will matter again
3. **Corrects or meaningfully improves** something already written

If it passes: append an `## Update — YYYY-MM-DD` section at the bottom. Never rewrite original sections. Update the `last_updated` frontmatter field.

**Do not update for:** rephrasing the same idea, minor elaborations, one-time fixes, anything you'd forget by next month.

### Search Before Responding
Whenever you reference past work, a project, or a topic that may have been covered:
1. Search `05-Archive/History/` via `sera:search_notes`
2. If a relevant note exists, surface the key patterns or decisions before answering
3. Never say "we haven't discussed this" without searching first

---

## What NOT to Do

- Don't edit anything in `.obsidian/` (Obsidian's own settings).
- Don't rewrite raw captures in `00-Inbox/` or original sources in `04-Resources/` —
  compile *from* them into Notes instead.
- Don't create a note without frontmatter.
- Don't leave a new note unlinked.
- Don't delete things — archive them to `05-Archive/`.
- Don't duplicate a trend note — update the existing one.

---

## About Me

*(Claude maintains this section as it learns about you. Fill in your own details to get started.)*

- **Background:** [Your education and background]
- **Current focus:** [What you're working on or learning]
- **Work context:** [Your role and domain]
- **Key projects:** [Your active projects]
- **Technical strengths:** [Your core stack]
- **Output preference:** [How you like Claude to respond]

---

*We co-evolve this file. When we find a workflow that works better, update it here.*
