# Execution Plan — 2026-08-20

> Master checklist of everything outstanding across this project, compiled from `STATUS.md`'s audit and the roadmap discussion that followed it. Split by who can actually do each item — Claude executing a task Arvin hasn't asked for isn't progress. Updated in place as items complete; each checked item gets a one-line completion note, not deleted.

## Tier 1 — Claude executes now, no input needed

- [ ] Fold the confirmed roadmap decisions (build `finances-status-digest` first, extend `grilling` for School, add `design-token-guardian`, hold off on the Business decision-to-brief pipeline) into `AI Brain Skills Strategy.md`
- [ ] Build `finances-status-digest` (Second-Brain-Personal, nested under `Finances/.claude/skills/`) — pulls from `stock-portfolio`'s latest log + `Finance Dashboard.md`, produces a combined periodic status note
- [ ] Extend `grilling` with a Socratic-tutor / concept-mastery mode for School use, distinct from its existing plan-stress-test mode
- [ ] Build `design-token-guardian` (Second-Brain, nested under `Projects/.claude/skills/`) — checks UI code against the confirmed `frontend-design` tokens, flags drift
- [ ] Run `auditing-skill-triggers` against the 3 new/changed skills, log results
- [ ] Sync any cross-cutting changes via `sync-shared-skills.sh`
- [ ] Update `STATUS.md` to reflect what's now built

## Tier 2 — needs Arvin specifically, Claude cannot do these

- [ ] Open/reload `Second-Brain-Personal` in Obsidian once, so the Local REST API plugin generates its key — unblocks that vault's MCP wiring
- [ ] Run `/plugin install skill-creator@anthropic-agent-skills` in an interactive session
- [ ] Decide whether to push either repo to GitHub (currently local-only)
- [ ] Actually capture something real into `00_Inbox` — the one item on this whole list that isn't infrastructure

## Tier 3 — deliberately deferred, not part of this execution pass

- Business "decision-to-brief" pipeline (`grilling` → a `supplier-outreach-drafter` that doesn't exist yet) — both ventures are pre-revenue, revisit once there's an actual sourcing decision to run through it
- RAG/Phase 4-6 (retrieval, reranker, agentic escalation) — still blocked on Tier 2's content-capture item, not a tooling gap
- Scheduled automation for `weekly-review-processor`/`auditing-skill-triggers` — flagged in `MAINTENANCE.md`, deliberately not built (unattended edits are a bigger trust step than a scheduled report)
