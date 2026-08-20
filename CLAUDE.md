# CLAUDE.md

## What this is

Arvin's personal second brain: an Obsidian vault (human-readable knowledge store) paired with Claude Code as the reasoning/execution layer. The full architecture rationale — RAG pipeline design, agent patterns, memory system, sourced references — lives in [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md). This file is the lean, per-session brief; read the architecture doc only when you need the "why" behind a decision below. For how the system's own maintenance mechanisms (reviews, skill audits, sync, memory) actually run and how to verify they're working, see [`docs/MAINTENANCE.md`](docs/MAINTENANCE.md). For current build status, a full skills inventory, and an evidence-based audit of known flaws, see [`docs/STATUS.md`](docs/STATUS.md) — updated periodically, not every session.

**Golden rule:** default to the simplest thing that works — a single note, a single LLM call, a deterministic workflow. Only reach for retrieval pipelines, agent loops, or multi-agent orchestration once a simpler approach has demonstrably failed. Complexity is earned, not assumed.

**Current phase: Phase 2 — Obsidian MCP wiring done.** No RAG pipeline or agent loop yet. Do not build those until this file says otherwise.

**Category-first reorg (2026-08-19).** Flat PARA restructured into `General`/`School`/`Projects`/`Business` categories with PARA nested inside each — see "Folder map" below.

**This repo is public** (made public by Arvin, 2026-08-16). It holds Business/Resources content only. Personal and Personal Finance content lives in the separate **private** [`Second-Brain-Personal`](https://github.com/zapantaarvin/Second-Brain-Personal) repo — see "Domain boundaries" below before adding anything that touches Arvin's personal life or finances.

---

## Domain boundaries — read before adding personal/sensitive content

This repo went public on 2026-08-16. Before that, personal content (a dating playbook, a real budget dashboard, a stock portfolio) had already been added under the assumption of privacy. Excluding it via `.gitignore` alone was tried first and rejected as insufficient — see [`docs/isolation-architecture/00-data-isolation.md`](docs/isolation-architecture/00-data-isolation.md)'s core principle: **isolation must be enforced at the storage layer, not by prompting/diligence.** A `.gitignore` line depends on someone remembering it exists every time; a separate private repo doesn't.

**The actual rule:**
- Personal life, personal finances (income, portfolio, budget) → `Second-Brain-Personal` (private). Never write this content into a file inside this repo, even temporarily.
- Business ventures, coursework, general knowledge/reference → this repo (public). This includes things Arvin might not want randomly public-searchable (e.g. business SOPs) but that don't rise to the personal/financial sensitivity level — his call, already made when he chose to keep this repo public.
- If genuinely unsure which side something belongs on, ask rather than guess — see `docs/isolation-architecture/00-data-isolation.md`'s isolation-patterns table for the reasoning, and `docs/isolation-architecture/01`–`04` for the fuller planned architecture (tool layer, orchestration, marketing pipeline, cross-domain reporting — none of this is built yet, it's a plan).

---

## Tech stack (current)

- **Obsidian** — the vault itself, plain Markdown files, local-first. Installed via Homebrew cask.
- **Claude Code** — reasoning and file operations, reading this file each session. Has native filesystem access to this folder regardless of MCP.
- **Obsidian Local REST API plugin** (`coddingtonbear/obsidian-local-rest-api`, v5.1.0+) — runs an HTTPS REST + MCP server on `127.0.0.1:27124` whenever Obsidian is open. Config/API key lives in `.obsidian/plugins/obsidian-local-rest-api/data.json` (gitignored, local-only, never commit).
- **`.mcp.json`** (gitignored, local-only) — registers the `obsidian` MCP server with Claude Code for this project. **Obsidian must be running** for the MCP tools to work; if they're unavailable, check Obsidian is open before assuming something's broken.

## Tech stack (planned, not yet built — see build order)

- Chroma (local vector store) + BM25 for hybrid retrieval
- Cross-encoder reranker
- Mem0 or Letta, only if CLAUDE.md + auto-memory prove insufficient

Do not install or wire up anything in the "planned" list without asking first — each is a real dependency/infra decision, not a file edit.

---

## Folder map

Category-first as of 2026-08-19 — category folders are the new top level, PARA nested inside each. `00_Inbox/`, `50_Daily/`, `90_MOCs/` stay vault-wide (not per-category — see "Category map" below for why).

```
00_Inbox/      unsorted daily capture — no filtering at capture time, no category decision here either
General/       {CLAUDE.md, 10_Projects, 20_Areas, 30_Resources, 40_Archive}
School/        {CLAUDE.md, 10_Projects, 20_Areas, 30_Resources, 40_Archive}
Projects/      {CLAUDE.md, 10_Projects, 20_Areas, 30_Resources, 40_Archive}
Business/      {CLAUDE.md, 10_Projects, 20_Areas, 30_Resources, 40_Archive}
50_Daily/      daily notes; Templates/ holds the daily note template
90_MOCs/       Maps of Content — hub notes linking related notes by topic
docs/          ARCHITECTURE.md (RAG/agent design) + isolation-architecture/ (domain-boundary plan, 5 docs)
```

Every PARA subfolder has a `_readme.md` explaining its purpose — read it before adding structure it doesn't already have.

### Category map — route here before reading anything else
A category's own `CLAUDE.md` only loads once Claude actually reads/edits a file inside it (Claude Code's nested-memory behavior) — this map exists so a new note can be routed correctly *before* that happens.

| Category | What goes here |
|---|---|
| **General** | Cross-cutting reference that doesn't fit the other three — e.g. the Skills system itself. Used deliberately, not as a dumping ground. |
| **School** | UT Austin coursework — course notes, assignments, academic writing. |
| **Projects** | Apps Arvin is personally coding — has its own repo/tech stack/build-phase (Kanban Business Hub, Financial Compass App). See `Projects/CLAUDE.md` for the litmus test vs. Business. |
| **Business** | Venture strategy/ops/reference with no dev artifact of Arvin's own (Zapee, Protein Bar Business/FFN). |

If genuinely unsure which category, ask rather than guess — same principle as the public/private domain-boundary rule below.

---

## Conventions

- **PARA, not topic folders.** File by actionability (Project / Area / Resource / Archive), not by subject. A note about "investing" is an Area; "research the Q1 semiconductor thesis" inside it is a Project.
- **Capture first, organize later.** Anything new goes to `00_Inbox` unless its destination is obvious. Don't invent new top-level folders to avoid deciding where something goes.
- **Link deliberately.** Use `[[wikilinks]]` only where a real relationship exists — don't link for the sake of linking. Let the backlinks panel surface emergent structure.
- **Distill, don't dump.** When saving external material into `30_Resources`, compress it into your own words. Raw clippings defeat the point of a second brain.
- **MOCs are earned.** Create a new MOC in `90_MOCs` once a topic has ~5+ related notes, not before.
- **Weekly review is real work.** Empty the inbox, update project status, scan daily notes for durable insights, update MOCs. If Claude is asked to "do the weekly review," it means literally walking through these four steps.
- **Cross-cutting Skills ("core skills") are duplicated, not shared, across the two repos.** They live in both `Second-Brain/.claude/skills/` and `Second-Brain-Personal/.claude/skills/` as separate copies (they carry no domain data, so duplication doesn't violate the isolation principle — see `AI Brain Skills Strategy.md` §4). The current list is `sync-shared-skills.sh`'s `SHARED_SKILLS` array — read that file, don't trust a name list written here, since it's drifted out of date before. After editing any shared skill, run `../sync-shared-skills.sh` from `Brain/` to check for drift and optionally sync it — nothing does this automatically.
- **Category-specific conventions live in each category's own `CLAUDE.md`**, not here — e.g. `Business/CLAUDE.md` has the "vault ≠ task tracker" and business-name-churn rules, `Projects/CLAUDE.md` has the Business-vs-Projects litmus test, `School/CLAUDE.md` has the academic-integrity boundary pointer. This file only holds what applies vault-wide.

---

## Do not touch without asking

- Installing any package or dependency (Python, npm, etc.)
- Adding any further MCP server to `~/.claude/settings.json` or project `.mcp.json` (the Obsidian one is already wired — see below)
- Regenerating or rotating the Obsidian REST API key (Settings → Local REST API in the app) without updating `.mcp.json` to match
- Building or modifying any retrieval/RAG pipeline code
- Standing up a vector database or any new persistent service
- Deleting or bulk-moving notes across folders
- Editing `docs/ARCHITECTURE.md` in ways that change the recommended stack (fine to append notes; ask before changing a recommendation)
- Anything involving credentials, API keys, or OAuth (Gmail, Google Drive, finance APIs)
- Writing personal/financial content into this repo (see "Domain boundaries" above) — that goes in `Second-Brain-Personal` instead, no exceptions, don't ask "just this once"

## Prefer

- Small, reversible edits: one note, one folder, one template at a time
- Explaining *why* a note belongs where it's being filed, if it's not obvious
- Flagging when a request belongs in a later build phase instead of doing it now

---

## Build order (see `docs/ARCHITECTURE.md` §8 for full detail)

1. ✅ Vault scaffold — folder structure, seed notes, templates
2. ✅ CLAUDE.md v1 — this file
3. ✅ Obsidian MCP wiring — Local REST API plugin + `.mcp.json`, both gitignored (local secrets)
4. ⬜ Static hybrid RAG v1 — Chroma + BM25, no agent loop
5. ⬜ Reranker
6. ⬜ Agentic RAG escalation — only for query classes where static RAG fails
7. ⬜ Subagents/skills for recurring workflows
8. ⬜ Memory layer (Mem0/Letta) — only if needed. Auto-memory backfilled 2026-08-16 (Claude's own cross-session memory index, outside this repo) with current project status and standing preferences; Mem0/Letta still not evaluated, not needed yet.
9. ⬜ Governance: tracing, verifier skill, eval set

When a step here gets built, mark it ✅ and add one line on what changed. If Claude repeats the same mistake twice, that's a missing line in this file, not a reason to write a longer prompt.
