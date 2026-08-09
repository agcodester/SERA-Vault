# SERA — AI-Powered Second Brain

> *"I decide what to read and what questions to ask. Claude does the bookkeeping — filing, linking, summarizing, and keeping everything current."*

SERA is a self-maintaining personal knowledge management system built on [Andrej Karpathy's LLM Wiki principle](https://gist.github.com/karpathy/e1e7b9d7e3e7e7e7e7e7e7e7e7e7e7e7) — the idea that an LLM should not just answer questions but actively maintain a structured, compounding knowledge base on your behalf. You capture. Claude organizes, links, and keeps everything current.

This repo is a **template** — fork it, drop in your own vault, and have a fully operational AI second brain in under 30 minutes.

---

## The Core Idea

Most people use AI as a search engine: ask a question, get an answer, forget it. SERA flips this. Every question you ask, every paper you read, every insight you have gets **compiled into a structured, cross-linked knowledge graph** that Claude can read and build on in every future session.

The key insight from Karpathy: **maintenance cost is near zero when the LLM does the bookkeeping.** You only need to capture. Claude handles everything else.

```
You capture raw material  →  Claude compiles, links, maintains
                          →  Knowledge compounds across sessions
                          →  Future you benefits from past you's work
```

---

## Architecture

### The Three-Layer Model

SERA implements Karpathy's three-layer knowledge architecture:

```
┌─────────────────────────────────────────────────────┐
│  Layer 3 — Schema / Rules                           │
│  CLAUDE.md  (the operating manual)                  │
│  Co-evolved by you and Claude                       │
├─────────────────────────────────────────────────────┤
│  Layer 2 — Compiled / Maintained                    │
│  02-Notes/  MOCs/  AI-Trends/                       │
│  Evergreen notes, written and maintained by Claude  │
├─────────────────────────────────────────────────────┤
│  Layer 1 — Raw / Immutable                          │
│  00-Inbox/  04-Resources/                           │
│  Sources of truth — you capture, Claude reads       │
└─────────────────────────────────────────────────────┘
```

Raw sources are never rewritten. Claude compiles knowledge *from* them into the wiki layer above. The schema layer tells Claude exactly how to behave in every session.

### The Three Memory Types

SERA implements three distinct memory layers, mapping cognitive science categories to vault folders:

| Memory Type | What it stores | Vault location |
|---|---|---|
| **Episodic** | What happened, when, in which session | `01-Daily/`, `05-Archive/History/` |
| **Semantic** | Facts, concepts, what you know | `02-Notes/Concepts/`, `AI-Trends/` |
| **Procedural** | How to do things, workflows, SOPs | `02-Notes/Concepts/`, `CLAUDE.md` |

---

## Vault Structure

```
SERA-vault/
│
├── CLAUDE.md                    ← The operating manual. Claude reads this first, every session.
│
├── 00-Inbox/                    ← Zero-friction capture. Dump everything here.
│
├── 01-Daily/
│   └── YYYY/
│       └── YYYY-MM-DD.md        ← Session logs, daily notes, open loops
│
├── 02-Notes/
│   ├── Concepts/                ← Evergreen atomic notes (one idea per note)
│   ├── People/                  ← One note per person you follow or work with
│   ├── Places/                  ← Companies, institutions, locations
│   └── Ideas/                   ← Raw ideas worth developing
│
├── 03-Projects/                 ← Active projects — plans, tasks, current state
│
├── 04-Resources/
│   ├── (articles, links, tools) ← Raw reference material, never rewritten
│   └── Papers/
│       ├── _Papers-MOC.md       ← Index of all ingested papers
│       ├── _Reading-Queue.md    ← Prioritized reading list
│       ├── _Author-Watch.md     ← Authors to track on arXiv
│       ├── _Topics.md           ← Research topics for weekly digest
│       └── _Weekly-Digest.md    ← Auto-generated weekly paper summaries
│
├── 05-Archive/
│   └── History/                 ← Migrated Claude conversations (long-term memory)
│       └── YYYY-MM-DD-slug.md
│
├── MOCs/                        ← Maps of Content — hub notes connecting big ideas
│   ├── Home.md                  ← Dashboard. Claude reads this every session.
│   ├── claude-memory-index.md   ← Index linking all history notes
│   ├── AI-Trends-MOC.md
│   ├── Concepts-MOC.md
│   ├── Projects-MOC.md
│   └── People-MOC.md
│
├── AI-Trends/                   ← Living notes on where AI is heading
│   ├── _index.md
│   ├── agent-memory.md
│   ├── agent-frameworks.md
│   └── ...
│
└── Templates/                   ← Frontmatter templates for every note type
    ├── concept-template.md
    ├── paper-template.md
    ├── project-template.md
    ├── daily-template.md
    └── claude-conversation-template.md
```

---

## Knowledge Graph Topology

SERA maintains a deliberate graph topology. Links are intentional, not accidental.

```
                    ┌─────────────┐
                    │  MOCs/Home  │  ← Dashboard, entry point
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          ▼                ▼                ▼
   ┌─────────────┐  ┌───────────┐  ┌──────────────┐
   │ AI-Trends/  │  │ 02-Notes/ │  │ 03-Projects/ │
   │  (trends)   │  │ (concepts)│  │  (active)    │
   └──────┬──────┘  └─────┬─────┘  └──────────────┘
          │               │
          └───────┬────────┘
                  ▼
          ┌──────────────┐
          │  04-Resources │  ← Raw sources (read-only, compiled from)
          └──────────────┘

  Memory subgraph (isolated — never links into active graph):

  MOCs/Home → MOCs/claude-memory-index → 05-Archive/History/*.md
                                                    ↑
                                         (links back only to index)
```

**Key rule:** The `05-Archive/History/` subgraph is topologically isolated. History notes never link into active notes — this keeps the live knowledge graph clean while preserving full session history.

---

## The Daily Flow

```
Capture  →  Process  →  Organize  →  Connect  →  Review
  (you)      (Claude)    (Claude)     (Claude)    (together)
```

1. **Capture** — Drop anything into `00-Inbox/` with zero friction. No formatting, no organizing.
2. **Process** — Claude reads each inbox item and decides: resource, concept, person, project, or idea.
3. **Organize** — Claude files it in the right folder with complete frontmatter from the template.
4. **Connect** — Claude links the new note to at least 2 related notes and updates the relevant MOC. A note with no links is a dead note.
5. **Review** — Daily note updated. Weekly review surfaces orphans and trends. Monthly lint checks health.

---

## MCP Integration

SERA connects Claude to your Obsidian vault via an MCP (Model Context Protocol) server, giving Claude direct read/write access to vault notes across sessions.

### Recommended MCP bridges

| Bridge | Best for | Notes |
|---|---|---|
| `obsidian-mcp` (npx) | Simplest setup, no Obsidian open required | Recommended for most users |
| `mcp-obsidian` (uvx) | Python-based agents | Most GitHub stars |
| `MCPVault` | Token efficiency, BM25 search | Good for large vaults |
| Claudian / Claudesidian | Full agent mode inside Obsidian sidebar | Most integrated |

### Setup (obsidian-mcp)

```bash
# 1. Install the MCP bridge
npx obsidian-mcp /path/to/your/vault

# 2. Add to your Claude MCP config
# (claude_desktop_config.json or equivalent)
{
  "mcpServers": {
    "sera": {
      "command": "npx",
      "args": ["obsidian-mcp", "/path/to/your/vault"]
    }
  }
}

# 3. Drop CLAUDE.md into your vault root
# 4. Start a Claude session — it reads CLAUDE.md automatically
```

### No MCP? No problem.

The arXiv research system and AI-Trends protocols require **no MCP installation** — they use Claude's built-in `web_search` and `web_fetch` tools. MCP is only needed for Claude to read/write vault notes directly.

---

## Built-in Protocols

### arXiv Research Pipeline
Five commands that turn Claude into a research assistant:
- `search arxiv for [topic]` — finds, fetches, and ingests papers into the vault
- `run weekly paper digest` — auto-scans your topic list for the last 7 days of papers
- `check author updates` — surfaces new work from authors on your watch list
- `I finished [paper]` — compiles key claims into trend and concept notes
- `fill gap: [description]` — targeted search to fill a specific knowledge gap

### AI-Trends Tracking
Claude maintains living notes in `AI-Trends/` — one note per major area (agent memory, model releases, frameworks, etc.) — updated with dated entries every time something new is ingested. Ask "what's new in AI?" and Claude cross-references its web search against what's already in the vault, surfacing only what's genuinely new to you.

### Session Memory
Every meaningful conversation can be migrated to `05-Archive/History/` as a structured note — giving Claude searchable memory of every architectural decision, lesson learned, and workflow developed across all past sessions. Claude searches this archive before responding to any question about past work.

### Review Protocols
- **Weekly review** — learning summary, orphan detection, trend surfacing, focus suggestions
- **Monthly lint** — orphan notes, stale claims, missing concept notes, archival candidates

---

## Getting Started

```
1. Fork this repo
2. Copy the vault structure into your Obsidian vault
3. Edit CLAUDE.md → fill in the "About Me" section with your details
4. Set up your MCP bridge (or skip it and use web_search mode)
5. Start a Claude session with: "Read CLAUDE.md and give me an orientation"
6. Drop something into 00-Inbox/ and ask Claude to process it
```

The system is immediately useful from session one. It compounds in value over time as the knowledge graph grows.

---

## Design Principles

**Capture-first.** Zero friction on input. The system only works if adding to it costs nothing.

**Synthesis over retrieval.** Claude doesn't just find notes — it compiles, links, and maintains them. The bottleneck in personal knowledge management isn't retrieval, it's bookkeeping. LLMs solve the bookkeeping problem.

**Queries compound.** Every good answer becomes a new note. Future sessions benefit from past sessions. Unlike RAG (which re-derives from scratch every query), SERA accumulates.

**Human-readable substrate.** Plain Markdown files. No proprietary database, no lock-in. The vault is yours — readable, editable, and portable whether or not Claude is involved.

**Topological discipline.** Links are intentional. The memory subgraph is isolated from the live knowledge graph. Structure prevents the vault from becoming a pile of disconnected notes.

---

## Inspiration

- [Andrej Karpathy's LLM OS / LLM Wiki concept](https://x.com/karpathy) — the foundational mental model
- [Tiago Forte's PARA method](https://fortelabs.com/blog/para/) — Projects, Areas, Resources, Archive
- [Andy Matuschak's evergreen notes](https://notes.andymatuschak.org/) — atomic, linked, concept-first notes
- [Zettelkasten](https://zettelkasten.de/) — the original linked knowledge system

---

## License

MIT — fork freely, adapt to your own workflow, and build your own second brain.
