# Status & Roadmap — 2026-08-20

> Covers both `Second-Brain` and `Second-Brain-Personal`. Lives here (public repo) for the same reason as `ARCHITECTURE.md` and `MAINTENANCE.md` — system-level, not repo-specific. A full audit (not just a recall of what's been built) was run against both repos same day; findings below are evidence-based — see each section for how it was checked.

---

## 1. Build order status (`CLAUDE.md` §Build order, `ARCHITECTURE.md` §8)

| Phase | Status | Note |
|---|---|---|
| 1. Vault scaffold | ✅ Done | Restructured category-first 2026-08-19 (see §2) |
| 2. CLAUDE.md v1 | ✅ Done | Root + 6 category files, all under line-count guidance |
| 3. Obsidian MCP wiring | 🟡 Half done | `Second-Brain` works. `Second-Brain-Personal` has been stuck for multiple days — plugin staged, port set, but never opened in Obsidian to generate its key. **The single most-repeated unfinished action in this whole project.** |
| 4. Static hybrid RAG | ⬜ Not started | Deliberately deprioritized — see §5 |
| 5. Reranker | ⬜ Not started | Depends on 4 |
| 6. Agentic RAG escalation | ⬜ Not started | Depends on 4/5 |
| 7. Subagents/Skills | 🟢 Active | 10 skills built, see §3 |
| 8. Memory layer | 🟡 Partial | Auto-memory active (4 memories), no Mem0/Letta evaluation, not needed yet |
| 9. Governance | 🟡 Partial | `auditing-skill-triggers` + `reflecting-on-skills` cover skill-layer governance. No tracing, no golden eval set, no verifier subagent for high-stakes output |

---

## 2. Vault structure

Category-first since 2026-08-19: `Second-Brain` → General/School/Projects/Business, `Second-Brain-Personal` → Personal/Finances, PARA nested inside each, `00_Inbox`/`50_Daily`/`90_MOCs` vault-wide.

**Content inventory (checked 2026-08-20, not estimated):**
- `Second-Brain`: 13 real notes — 2 Business ventures, 2 Projects (apps), 8 School (UT Austin courses + the voice-spec source draft), 1 General (the Skills strategy doc).
- `Second-Brain-Personal`: 5 real notes — Finance Dashboard, Dating Playbook, 2 daily-task logs, README.
- **This count has not changed since the reorg.** Every commit since 2026-08-19 has been structure/tooling, not new knowledge. See §6, flaw #1.

---

## 3. Skills (10 total)

| Skill | Scope | Job | Trigger-tested | Has REFLECTIONS.md |
|---|---|---|---|---|
| `casual-human-voice` | Cross-cutting | Writing style + condensed visual-identity reference | ✅ 2026-08-19 | — |
| `weekly-review-processor` | Cross-cutting | 4-step vault review (inbox/status/insights/MOC) | ✅ 2026-08-19 | — |
| `auditing-skill-triggers` | Cross-cutting | Re-verifies trigger reliability across the whole library | ✅ 2026-08-19 (self-test) | — |
| `grill-me` | Cross-cutting | User-only entry point into `grilling` | ✅ 2026-08-20 | — |
| `grilling` | Cross-cutting | Design-Tree Mode (stress-test a plan) + Study Mode (concept mastery for School), added 2026-08-20 | ✅ 2026-08-20 (both modes) | — |
| `reflecting-on-skills` | Cross-cutting | Proposes confidence-leveled skill updates from real usage | ✅ 2026-08-20 (description narrowed after audit found over-firing risk) | — |
| `stock-portfolio` | `Second-Brain-Personal` only | Portfolio co-pilot, 2 scheduled cloud routines | ✅ 2026-08-19 | — |
| `gmail-auto-label` | `Second-Brain-Personal` only | Gmail triage | ✅ 2026-08-19 | — |
| `frontend-design` | `Second-Brain/Projects/` only (nested) | UI code generation, warm-neutral confirmed direction | ✅ 2026-08-20 | ✅ Yes — logged the A/B test that confirmed the direction |
| `design-token-guardian` | `Second-Brain/Projects/` only (nested) | Checks UI work against `frontend-design`'s confirmed tokens, flags drift | ✅ 2026-08-20 | — |
| `finances-status-digest` | `Second-Brain-Personal/Finances/` only (nested) | Combines portfolio + budget into one dated status note — proof case for the general digest pattern | ✅ 2026-08-20 | — |

All 6 shared skills confirmed `IN SYNC` via `sync-shared-skills.sh` as of this audit. Full trigger-eval history in each repo's `.claude/skills/_trigger-log.md`.

---

## 4. Still outstanding (needs something from Arvin, not just from Claude)

1. **`Second-Brain-Personal` MCP wiring** — open/reload that vault in Obsidian once. Still the single most-repeated unfinished item in this project.
2. **`skill-creator` plugin** — needs `/plugin install skill-creator@anthropic-agent-skills` run in an interactive session; can't be done headlessly.
3. **GitHub push** — every commit in both repos is local-only. Not a bug (never asked for), but a real backup gap worth a decision.
4. **Actual content capture** — the zero-growth problem in §2. `finances-status-digest` exists now and would produce a real dated note every time it runs, but like `weekly-review-processor` it's built and never actually executed yet. Building a skill isn't the same as using it.

**Resolved 2026-08-20**: the Perplexity roadmap loop (asked for 3 times, never pasted in full) — Arvin delegated roadmap planning to Claude instead of continuing to chase it. See `AI Brain Skills Strategy.md`'s "Roadmap decisions, 2026-08-20" section and `docs/EXECUTION-PLAN.md`.

---

## 5. Deliberately not done yet (and why)

- **RAG/retrieval (Phase 4-6)**: explicitly deprioritized 2026-08-16 in favor of closing feedback loops first (verify Skills work, get memory writing) — see [[prioritize-closing-loops-before-infra]] in Claude's memory. Still the right call: there still isn't enough content (§2) to make retrieval meaningfully useful yet.
- **Scheduled automation** for `weekly-review-processor`/`auditing-skill-triggers`: flagged in `MAINTENANCE.md` as a real option, deliberately not built — a scheduled process editing/archiving notes unattended is a bigger trust step than a scheduled read-only report.
- **`.claude/rules/*.md` path-scoped rules**: considered during the reorg, not needed — category `CLAUDE.md` files cover the same need with less machinery.

---

## 6. Findings from this session's full audit (2026-08-20)

Verified, not assumed — see the conversation for the exact commands run.

- ✅ Both repos clean, no uncommitted drift.
- ✅ All 6 shared skills byte-identical across repos.
- ✅ Every `CLAUDE.md` well under the ~150-200 line guidance.
- ✅ Every wikilink in both vaults resolves to a real note (two false-positive matches were prose examples, not broken links).
- ✅ No personal/financial content leaked into the public repo.
- ✅ Both live app repos (Compass, kanban-business-hub) haven't pushed since their vault notes were written — no drift between the vault's claims and reality.
- ❌ **Flaw #1**: zero new content notes since the 2026-08-19 reorg (§2).
- ❌ **Flaw #2**: `weekly-review-processor` has never actually executed — confirmed via git log and both `00_Inbox` folders being permanently empty.
- ❌ **Flaw #3**: `Second-Brain-Personal` MCP wiring stuck for multiple sessions (§4.1).
- ❌ **Flaw #4**: the confirmed `frontend-design` visual identity wasn't linked back into `Financial Compass App.md` — **fixed same day**, see that note's new "Visual identity" section.
- ⚠️ **Flaw #5**: auto-memory has no git history/backup — real risk assessed as low (most substantive decisions duplicate into vault `CLAUDE.md` files) but worth knowing.

---

## 7. What "next" actually means, ranked by leverage

1. **Run something for real.** Both `weekly-review-processor` and `finances-status-digest` exist and have never been executed. Every other item on this list is Claude building or organizing; this is the one where the built tooling actually gets used.
2. Finish the MCP wiring (§4.1) — two minutes, unblocks nothing critical but has been open too long.
3. Decide on GitHub push (§4.3) — a real decision, not a default.
4. Longer-term, once content volume actually justifies it: revisit Phase 4 (RAG) — not before.
