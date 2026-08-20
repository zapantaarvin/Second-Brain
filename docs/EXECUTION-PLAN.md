# Execution Plan — 2026-08-20

> Master checklist of everything outstanding across this project, compiled from `STATUS.md`'s audit and the roadmap discussion that followed it. Split by who can actually do each item — Claude executing a task Arvin hasn't asked for isn't progress. Updated in place as items complete; each checked item gets a one-line completion note, not deleted.

## Tier 1 — Claude executes now, no input needed

- [x] Fold the confirmed roadmap decisions into `AI Brain Skills Strategy.md` — done 2026-08-20
- [x] Build `finances-status-digest` (Second-Brain-Personal, nested under `Finances/.claude/skills/`) — done 2026-08-20, pulls from `stock-portfolio`'s latest log + `Finance Dashboard.md`
- [x] Extend `grilling` with Study Mode (concept-mastery) for School, alongside its existing Design-Tree Mode — done 2026-08-20, synced to both repos
- [x] Build `design-token-guardian` (Second-Brain, nested under `Projects/.claude/skills/`) — done 2026-08-20, reads `frontend-design`'s tokens at check-time rather than hardcoding them
- [x] Run `auditing-skill-triggers` against the 3 new/changed skills — done 2026-08-20, all pass, two soft ambiguities logged not fixed (low risk)
- [x] Sync cross-cutting changes via `sync-shared-skills.sh` — confirmed `IN SYNC` for all 6 shared skills
- [x] Update `STATUS.md` to reflect what's now built — done 2026-08-20 (see below)

## Tier 2 — needs Arvin specifically, Claude cannot do these

- [ ] Open/reload `Second-Brain-Personal` in Obsidian once, so the Local REST API plugin generates its key — unblocks that vault's MCP wiring
- [ ] Run `/plugin install skill-creator@anthropic-agent-skills` in an interactive session
- [ ] Decide whether to push either repo to GitHub (currently local-only)
- [ ] Actually capture something real into `00_Inbox` — the one item on this whole list that isn't infrastructure

## Tier 3 — deliberately deferred, not part of this execution pass

- Business "decision-to-brief" pipeline (`grilling` → a `supplier-outreach-drafter` that doesn't exist yet) — both ventures are pre-revenue, revisit once there's an actual sourcing decision to run through it
- RAG/Phase 4-6 (retrieval, reranker, agentic escalation) — still blocked on Tier 2's content-capture item, not a tooling gap
- Scheduled automation for `weekly-review-processor`/`auditing-skill-triggers` — flagged in `MAINTENANCE.md`, deliberately not built (unattended edits are a bigger trust step than a scheduled report)
