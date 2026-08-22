---
name: auditing-vault-freshness
description: Use when Arvin says "clean up the vault," "find stale notes," "what can I archive," "scan for outdated files," or asks what in the vault no longer belongs. Read-only: sweeps every category's 10_Projects/20_Areas/30_Resources for staleness, misplacement, duplication, and domain-boundary signals, plus loose files sitting outside any vault at the Brain/ root. Produces a reviewable candidate list only — never moves, archives, or deletes a file itself. Distinct from weekly-review-processor (that one runs the recurring 00_Inbox/status/MOC ritual); this one sweeps the whole vault for things that have gone stale since they were filed.
---

# Auditing Vault Freshness

## Purpose
Notes get filed once and then nothing re-checks whether they're still earning their spot. This Skill is a read-only sweep that surfaces candidates for archiving/deletion/re-filing — Arvin reviews and decides, this never acts on its own. That split isn't a style choice: this vault's `CLAUDE.md` already lists "deleting or bulk-moving notes across folders" under "do not touch without asking," so a propose-only skill is the only shape this can correctly take.

## Step 0 — Loose files outside any vault (only when run from `Brain/` root)
List anything sitting directly in `Brain/` that isn't inside `Second-Brain/`, `Second-Brain-Personal/`, or a dotfile/config dir. For each, name it and suggest a destination: which vault + category (per that vault's root `CLAUDE.md` category map) it should be captured into, or `00_Inbox` if the category isn't obvious. Don't move anything — just report.

## Step 1 — Enumerate scan targets
Auto-detect every category folder in the current vault (any top-level dir containing its own `CLAUDE.md` plus `10_Projects/20_Areas/30_Resources/40_Archive`) — don't hardcode category names, they differ between `Second-Brain` (General/School/Projects/Business) and `Second-Brain-Personal` (Personal/Finances).

## Step 2 — Flag candidates, with evidence per note
For each note in `10_Projects`/`20_Areas`/`30_Resources` (skip `40_Archive` itself), check for concrete evidence, not guesses — cite what you actually found, e.g. a real `mtime`/frontmatter date or an exact quoted phrase, never an inferred one:

- **Explicit completion/dead language** — content says "done," "shipped," "cancelled," "shut down," "no longer doing this," etc., but the note is still outside `40_Archive`.
- **Stale by date** — frontmatter date or file mtime (`ls -la` / `stat`) is old relative to today, for a note framed as currently active.
- **Misplaced** — note's actual topic doesn't match its category per that vault's category map (e.g. a business note under `School/`). In `Second-Brain` specifically, also check for personal/financial content that violates the public/private domain boundary — flag this as high-priority regardless of any other signal.
- **Duplicate/superseded** — another note covers the same topic more recently or more completely.
- **Empty or stub** — zero-byte, template-only, or placeholder content that was never filled in.
- **Orphaned + stale** — no `[[wikilink]]` references from any other note or from `90_MOCs/Home MOC.md` (grep for the note's title, or use the Obsidian MCP backlinks tool if Obsidian is running), combined with an old mtime. Mark this signal **low confidence** on its own — an orphan alone isn't proof of deadness, pair it with another signal before flagging.

Also scan aged `00_Inbox` items, but only as a narrower check than `weekly-review-processor` does: flag items that look abandoned (old, no longer relevant) rather than just unsorted — unsorted-but-current inbox items are that other Skill's job, not this one's.

## Output
A single reviewable table, not prose: `Path | Category/PARA location | Signal(s) found | Evidence (exact quote/date) | Suggested action | Confidence`. Suggested action is always one of: archive to `40_Archive`, re-file to [category], delete-candidate, or merge with [other note] — never phrased as already done. Group high-confidence (2+ signals, or the domain-boundary flag) above low-confidence (single orphan/staleness signal) so Arvin can triage the obvious ones first.

## Scope note
Same domain rule as `weekly-review-processor`: a `Second-Brain` session scans `Second-Brain`, a `Second-Brain-Personal` session scans `Second-Brain-Personal`. Never pull one vault's notes into the other's output, even to cross-reference. Step 0 (loose root files) is the one exception, since those files aren't inside either vault's boundary yet.

## After this runs
Nothing gets archived/deleted/moved automatically. Once Arvin confirms specific rows, execute those moves individually (or hand them to the next `weekly-review-processor` pass) — don't batch-execute the whole table on an implied yes.
