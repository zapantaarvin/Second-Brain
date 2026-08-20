# Second Brain

Arvin's personal second brain: an Obsidian vault (category-first structure, PARA nested inside each category) paired with Claude Code as the reasoning/execution layer, built incrementally per the phased plan below.

- **Standing brief:** [`CLAUDE.md`](./CLAUDE.md) — lean, session-loaded project brief. Claude Code reads this automatically every session.
- **Full architecture spec:** [`docs/ARCHITECTURE.md`](./docs/ARCHITECTURE.md) — the sourced rationale, RAG pipeline design, agent patterns, and 10-step build order. Read on demand, not loaded every session.

## Vault structure

Category folders are the top level as of 2026-08-19; each nests its own PARA (`10_Projects/20_Areas/30_Resources/40_Archive`) and `CLAUDE.md`. See root `CLAUDE.md`'s category map for what belongs where.

```
00_Inbox/      unsorted daily capture (vault-wide, not per-category)
General/       cross-cutting reference (e.g. the Skills system itself)
School/        UT Austin coursework
Projects/      apps Arvin is personally coding
Business/      venture strategy/ops (no dev artifact of Arvin's own)
50_Daily/      daily notes + templates (vault-wide)
90_MOCs/       Maps of Content (vault-wide)
docs/          architecture reference and future detailed docs
```

## Status

**Phase 1 (vault scaffold) — done.** Folder structure, seed notes, and a starter daily-note template are in place. No RAG pipeline, MCP servers, or agent loop yet — see `CLAUDE.md`'s build order for what's next.

## How to keep this updated

1. Edit notes, `CLAUDE.md`, or `docs/ARCHITECTURE.md` locally, in Obsidian, or via Claude Code itself.
2. Commit and push — this repo is the single source of truth.
3. Pull the latest version into any new machine/vault before starting a Claude Code session.
