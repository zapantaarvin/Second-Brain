# Maintenance — How This System Stays Healthy

> Covers both `Second-Brain` and `Second-Brain-Personal`. Lives in the public repo (like `ARCHITECTURE.md`) since it's system-level, not repo-specific — `Second-Brain-Personal/CLAUDE.md` points back here.

## Purpose

Every mechanism below already exists. None of them run on a schedule, and the logic for "when does what run" was scattered across `CLAUDE.md`, the Skills strategy doc, and individual skill files. This is the one place that states the actual rhythm, and how to check each piece is really doing its job — not just that the file exists.

## The mechanisms

| Mechanism | What it does | Run it by saying | Cadence | Where it logs |
|---|---|---|---|---|
| `weekly-review-processor` | Empties `00_Inbox`, updates project/area status, promotes durable daily-note insights, updates `Home MOC.md` | "do the weekly review" | Weekly | Prints a changelog each run — no persistent history file (see gap below) |
| `auditing-skill-triggers` | Drift check + should/should-not/ambiguous trigger eval + coexistence check across every skill in the vault | "audit my skills" | After adding/editing any skill; monthly as the library grows | `.claude/skills/_trigger-log.md` (per repo) |
| `reflecting-on-skills` | Proposes a confidence-leveled skill update from a real, judged session outcome | "reflect on [skill]" or a decisive verdict ("I hate this," "this is right") | After any skill produces output you've actually judged | `<skill-folder>/REFLECTIONS.md` |
| `sync-shared-skills.sh` | Keeps the 6 cross-cutting skills byte-identical across both repos | `./sync-shared-skills.sh` from `Brain/` | After editing any shared skill | Terminal output only — run with no args any time to check drift |
| Auto-memory | Cross-session continuity — project state, confirmed preferences, working style | Automatic — Claude writes it opportunistically | End of any session with a real decision | `~/.claude/projects/-Users-arvinzapanta-Documents-Brain/memory/` |
| Archive, don't delete | Finished projects/areas/skills move to `40_Archive` or get moved out of `.claude/skills/`, never deleted | Part of weekly review / skill deprecation | As needed | Git history + `40_Archive/` folders |

## Recommended cadence

- **Weekly** — `weekly-review-processor`, one vault at a time.
- **Immediately after any skill add or edit** — run `sync-shared-skills.sh` if it's a shared skill; spot-check the trigger with `auditing-skill-triggers` if the description changed.
- **Immediately after any skill's output gets a real, decisive reaction** — `reflecting-on-skills`, while the context is fresh (this is how `frontend-design` got fixed same-day instead of drifting for weeks).
- **Monthly, or whenever the skill count in a vault climbs noticeably** — a full `auditing-skill-triggers` pass across every skill, not just the newest ones.
- **Ongoing** — memory gets backfilled at the end of any session with a durable decision. If a session ends and nothing got written, that's a miss, not a non-event.

## Known gaps (not fixed yet, flagged on purpose)

- `weekly-review-processor` has no history file — there's no way to see *when* the last review happened without checking git log for its commits. A `50_Daily/`-style dated log entry per run would close this the same way `_trigger-log.md` closed it for trigger audits.
- Nothing is actually scheduled. Every mechanism above is manual-trigger only. `schedule`/cron automation exists (already used for the `stock-portfolio` routines) and could run `weekly-review-processor` or `auditing-skill-triggers` on a timer — deliberately not set up yet, since a scheduled process editing/archiving notes unattended is a bigger trust step than a scheduled read-only report. Revisit if manual triggering keeps not happening.

---

## How to verify each one is actually working

Don't take "the skill exists" as evidence it's running. Check the actual log/output:

**`weekly-review-processor`** — no dedicated log yet (see gap above). Proxy check: `git log --oneline -10` in each repo and look for a "docs: weekly review" style commit within the last week. If the most recent one is old, it's overdue.

**`auditing-skill-triggers`** — read the log directly:
```
cat "Second-Brain/.claude/skills/_trigger-log.md"
cat "Second-Brain-Personal/.claude/skills/_trigger-log.md"
```
Check the most recent date per skill. Any skill added/edited more recently than its last log entry hasn't been re-audited.

**`reflecting-on-skills`** — check for `REFLECTIONS.md` inside a skill's folder:
```
find /Users/arvinzapanta/Documents/Brain -name "REFLECTIONS.md"
```
A skill that's been corrected or praised repeatedly but has no `REFLECTIONS.md` is a sign the loop isn't actually being closed, just talked about.

**`sync-shared-skills.sh`** — the most direct one, just run it:
```
/Users/arvinzapanta/Documents/Brain/sync-shared-skills.sh
```
Every shared skill should print `IN SYNC`. Anything else means an edit landed in one repo and never got propagated.

**Auto-memory** — check what's actually there and how stale it is:
```
cat "/Users/arvinzapanta/.claude/projects/-Users-arvinzapanta-Documents-Brain/memory/MEMORY.md"
```
Open the linked files and check the `modified` date in each one's frontmatter. If a memory describes something that's since changed (a build phase, a confirmed preference that got revised), it needs updating, not just existing.

**Archive convention** — spot check: any note in an active `10_Projects`/`20_Areas` folder that's actually finished/dead should have moved to `40_Archive` by the next weekly review. If `40_Archive` folders haven't grown at all across several reviews, either nothing's finishing (unlikely) or the step's being skipped.
