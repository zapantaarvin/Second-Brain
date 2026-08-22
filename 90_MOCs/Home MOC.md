---
type: moc
---

# Home MOC

The top-level Map of Content — the front door to the vault. Link out to other MOCs as they're created (one per major Area or long-running Project).

Category-first as of 2026-08-19 — see `CLAUDE.md`'s category map for what belongs where. Personal/Finances content lives in the private `Second-Brain-Personal` repo, not this vault at all (see `docs/isolation-architecture/00-data-isolation.md`).

## General
- [[AI Brain Skills Strategy]] — Skills taxonomy, priority list, and build order; tracks which Skills exist and where

## School
- [[_index|UT-Austin Coursework]] — 7 courses, reference notes only (full files stay in the private UT-Austin repo)
- [[Casual Human Voice - source draft]] — original UT Austin voice spec the `casual-human-voice` Skill was built from (archived 2026-08-21 — superseded by the Skill itself, kept as historical source)

## Projects (app building)
- [[Kanban Business Hub]]
- [[Financial Compass App]]

## Business
- [[Zapee]] — import/e-commerce
- [[Protein Bar Business (FFN)]]

## Skills (`.claude/skills/`)
- `weekly-review-processor` — runs this vault's 4-step review (inbox → status → daily-note scan → MOC update); also in `Second-Brain-Personal`
- `casual-human-voice` — writing style for anything drafted in this vault; also in `Second-Brain-Personal`
- `auditing-skill-triggers` — periodically re-verifies Skill trigger reliability as the library grows; also in `Second-Brain-Personal`

## Architecture docs
- [[00-data-isolation]] — why Personal/Business/Finance are kept structurally separate
- [[01-phase2-tool-layer]], [[02-phase3-orchestration]], [[03-phase4-marketing-content]], [[04-phase5-memory-reporting]] — future build phases, not yet started

---
*A MOC is a hub note, not a folder — it exists to surface backlinks and give the agent (and you) a starting point for a topic. Create a new MOC when a topic accumulates ~5+ related notes.*
