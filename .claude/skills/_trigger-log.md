# Skill Trigger Log

Append-only history of trigger-reliability checks. See the `auditing-skill-triggers` Skill for the process. Don't overwrite prior rows — this is a history, not a snapshot.

| Date | Skill | Should-trigger | Should-not-trigger | Ambiguous | Notes |
|---|---|---|---|---|---|
| 2026-08-19 | weekly-review-processor | Pass | Pass | Pass (with fix) | Found collision with `gmail-auto-label` over bare "inbox" wording; description edited in both repos to specify vault-inbox vs Gmail, re-tested, passes. |
| 2026-08-19 | casual-human-voice | Pass | Pass | Pass | "help me with [assignment]" correctly triggers; the skill's own academic-integrity boundary already handles the graded-work edge case. |
| 2026-08-19 | weekly-review-processor | Pass | n/a | n/a | Re-check after the category-first reorg: description unchanged (still passes prior tests above), only internal Step 1/3 logic changed to a two-stage category+PARA decision. No new trigger risk introduced. |
| 2026-08-19 | auditing-skill-triggers | Pass | Pass | Pass | First test of its own trigger. "audit my skills" / "check skill triggers" correctly fire; doesn't collide with any other skill's phrasing (none of the other 4 skills use "audit" or "trigger" language). |
| 2026-08-20 | grill-me | Pass | Pass | Pass | User-invoked only (`disable-model-invocation: true`) — correctly unreachable by phrase alone, by design; must be explicitly named. No collision (only skill using "grill" naming for explicit invocation). |
| 2026-08-20 | grilling | Pass | Pass | Pass | "grill me on this plan" / "stress-test my thinking" correctly trigger; "review this plan" (no stress-test framing) correctly does not. Soft ambiguity noted, not fixed: "review this business plan" sits near `weekly-review-processor`'s territory in surface wording, but that skill's scope (vault inbox review) is specific enough that no real collision risk exists. |
| 2026-08-20 | reflecting-on-skills | Fail → Fixed | Pass | Pass (after fix) | Found real risk: original description's "after any session where a Skill's output was corrected..." clause was broad enough to over-fire on routine mid-session corrections (e.g. "make it shorter"), not just decisive verdicts. Narrowed to require an explicit "reflect on X" ask or a clear decisive verdict, excluding routine corrections. Re-tested, passes. |
| 2026-08-20 | frontend-design | Pass | Pass | Pass | "build me a landing page" / "style this dashboard" correctly trigger; "write me an email" correctly does not. Overlaps intentionally (not a bug) with `casual-human-voice`'s Design System section when both are loaded inside `Projects/` — `casual-human-voice`'s own text already defers to `frontend-design` for actual build tasks, confirmed still correct. |
