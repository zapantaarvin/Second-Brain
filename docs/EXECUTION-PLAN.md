# Execution Plan — 2026-08-20

> Master checklist of everything outstanding across this project, compiled from `STATUS.md`'s audit and the roadmap discussion that followed it. Split by who can actually do each item — Claude executing a task Arvin hasn't asked for isn't progress. Updated in place as items complete; each checked item gets a one-line completion note, not deleted.

## Tier 1 — Claude executes now, no input needed

- [x] Fold the confirmed roadmap decisions into `AI Brain Skills Strategy.md` — done 2026-08-20
- [x] Build `finances-status-digest` (Second-Brain-Personal, nested under `Finances/.claude/skills/`) — done 2026-08-20, pulls from `stock-portfolio`'s latest log + `Finance Dashboard.md`
- [x] Extend `grilling` with Study Mode (concept-mastery) for School, alongside its existing Design-Tree Mode — done 2026-08-20, synced to both repos
- [x] Build `design-token-guardian` (Second-Brain, nested under `Projects/.claude/skills/`) — done 2026-08-20, reads `frontend-design`'s tokens at check-time rather than hardcoding them
- [x] Run `auditing-skill-triggers` against the 3 new/changed skills — done 2026-08-20, all pass, two soft ambiguities logged not fixed (low risk)
- [x] Sync cross-cutting changes via `sync-shared-skills.sh` — confirmed `IN SYNC` for all 8 shared skills (grown from 6 during this pass)
- [x] Update `STATUS.md` to reflect what's now built — done 2026-08-20 (see below)
- [x] Build `planning-before-execution` core skill (plan mode + checklist + explicit approval before executing) — done 2026-08-20
- [x] Build `giving-actionable-instructions` core skill (Tier-2-style items always get concrete steps) — done 2026-08-20
- [x] Merge Arvin's own Voice/Formatting/Context Handling rewrite into `casual-human-voice`, keeping Identity + Design System untouched — done 2026-08-20

## Tier 2 — needs Arvin specifically, Claude cannot do these

- [ ] **`Second-Brain-Personal` MCP wiring — config written, connection not actually working.** Reopened 2026-08-20: the plugin generated a key and `.mcp.json` was written correctly (gitignored, confirmed), but the live MCP connection doesn't work — reported by Arvin, not yet diagnosed (Obsidian running at the time? port 27125 actually listening? something else). Deprioritized per Arvin — revisit later, don't treat the file existing as proof it works.
- [ ] **Run `/plugin install skill-creator@anthropic-agent-skills`.** Steps: (1) open an interactive Claude Code session (terminal, not this one), (2) type that exact command at the prompt, (3) confirm any install prompt it shows.
- [ ] **Decide whether to push either repo to GitHub.** Not an instruction, a decision — tell me yes/no per repo and I'll handle the actual `git push`.
- [ ] **Capture something real into `00_Inbox`.** Steps: (1) open Obsidian on whichever vault fits (public `Second-Brain` for business/school/general, private `Second-Brain-Personal` for personal/finance), (2) create a new note anywhere inside that vault's `00_Inbox/` folder, (3) write whatever's on your mind, no formatting or categorizing needed at capture time, (4) next time you ask for a weekly review, I'll help sort it from there.

## Tier 3 — deliberately deferred, not part of this execution pass

- Business "decision-to-brief" pipeline (`grilling` → a `supplier-outreach-drafter` that doesn't exist yet) — both ventures are pre-revenue, revisit once there's an actual sourcing decision to run through it
- RAG/Phase 4-6 (retrieval, reranker, agentic escalation) — still blocked on Tier 2's content-capture item, not a tooling gap
- Scheduled automation for `weekly-review-processor`/`auditing-skill-triggers` — flagged in `MAINTENANCE.md`, deliberately not built (unattended edits are a bigger trust step than a scheduled report)
