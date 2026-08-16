# Second Brain

Arvin's personal second brain: an Obsidian vault (PARA structure) paired with Claude Code as the reasoning/execution layer, built incrementally per the phased plan below.

- **Standing brief:** [`CLAUDE.md`](./CLAUDE.md) — lean, session-loaded project brief. Claude Code reads this automatically every session.
- **Full architecture spec:** [`docs/ARCHITECTURE.md`](./docs/ARCHITECTURE.md) — the sourced rationale, RAG pipeline design, agent patterns, and 10-step build order. Read on demand, not loaded every session.

## Vault structure

```
00_Inbox/      unsorted daily capture
10_Projects/   things with a finish line
20_Areas/      ongoing responsibilities, no finish line
30_Resources/  reference material by topic
40_Archive/    finished/inactive, never deleted
50_Daily/      daily notes + templates
90_MOCs/       Maps of Content (topic hubs)
docs/          architecture reference and future detailed docs
```

## Status

**Phase 1 (vault scaffold) — done.** Folder structure, seed notes, and a starter daily-note template are in place. No RAG pipeline, MCP servers, or agent loop yet — see `CLAUDE.md`'s build order for what's next.

## How to keep this updated

1. Edit notes, `CLAUDE.md`, or `docs/ARCHITECTURE.md` locally, in Obsidian, or via Claude Code itself.
2. Commit and push — this repo is the single source of truth.
3. Pull the latest version into any new machine/vault before starting a Claude Code session.
