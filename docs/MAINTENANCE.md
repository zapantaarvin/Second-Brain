# Maintenance — How This System Stays Healthy

> Covers both `Second-Brain` and `Second-Brain-Personal`. Lives in the public repo (like `ARCHITECTURE.md`) since it's system-level, not repo-specific — `Second-Brain-Personal/CLAUDE.md` points back here.

## Purpose

Every mechanism below already exists. Most are manual-trigger only, and the logic for "when does what run" was scattered across `CLAUDE.md`, the Skills strategy doc, and individual skill files. This is the one place that states the actual rhythm, and how to check each piece is really doing its job — not just that the file exists.

## The mechanisms

| Mechanism | What it does | Run it by saying | Cadence | Where it logs |
|---|---|---|---|---|
| `weekly-review-processor` | Empties `00_Inbox`, updates project/area status, promotes durable daily-note insights, updates `Home MOC.md` | "do the weekly review" | `Second-Brain`: automated (see below). `Second-Brain-Personal`: manual, weekly — kept local on purpose (private repo, not given to the cloud routine) | Prints a changelog each run — no persistent history file (see gap below) |
| `auditing-vault-freshness` | Read-only sweep of every category's PARA folders (plus loose `Brain/` root files) for staleness/misplacement/duplication/domain-boundary signals; outputs a candidate table, never moves/archives/deletes itself | "clean up the vault" / "find stale notes" / "what can I archive" | As needed, or monthly as the vault grows | Terminal output only — no persistent log yet (same gap as below) |
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

## Automated: `Second-Brain` weekly review (added 2026-08-22)

A cloud routine (`schedule`/`RemoteTrigger`) runs every Monday 8am America/New_York (`0 12 * * 1` UTC) against `Second-Brain` only — **not** `Second-Brain-Personal`, kept local per Arvin's explicit call (the cloud service would need GitHub App access granted to that private repo, and he chose not to). Routine: https://claude.ai/code/routines/trig_01NsQHtc5SRktw7CzxN76fur

This closes the "nothing is scheduled" gap for one vault only, and does it in **report-only** mode, not by letting the unattended run actually move/delete/edit notes — `CLAUDE.md`'s "do not touch without asking" rule can't be satisfied by a cloud agent with no one present to say yes. Each run:
- Walks the same 4 steps as `weekly-review-processor` plus the `auditing-vault-freshness` checks, propose-only
- Writes exactly one new file, `50_Daily/Weekly Review Report - <date>.md`, commits it, pushes to `origin main`
- Touches no other file

**This also means `Second-Brain` now needs to stay pushed** — the cloud routine clones from GitHub, not your local disk, so local commits that never get pushed are invisible to it. Push after real work, not just when convenient.

**Verify it's running**: `git log --oneline -5` on `Second-Brain` should show a `docs: automated weekly review report <date>` commit each Monday; `ls 50_Daily/ | grep "Weekly Review Report"` should show one growing list. If a Monday is missing, check the routine directly (`get`/`list_runs` via `RemoteTrigger`) before assuming it silently failed.

## Known gaps (not fixed yet, flagged on purpose)

- `weekly-review-processor` (the interactive, executing version) still has no history file of its own — the automated report above is a parallel mechanism, not the same thing; a manual "do the weekly review" run still only prints a changelog, nothing persisted.
- `Second-Brain-Personal` review stays fully manual — deliberate, not an oversight (see above). Revisit only if Arvin decides to grant the private repo to the cloud service later.
- `auditing-skill-triggers` and `reflecting-on-skills` are still manual-trigger only in both vaults.

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
