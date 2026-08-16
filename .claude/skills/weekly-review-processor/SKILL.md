---
name: weekly-review-processor
description: Use when Arvin says "do the weekly review," "process my inbox," or asks for a vault review, in either Second-Brain or Second-Brain-Personal. Walks the vault's own 4-step review process (CLAUDE.md) end-to-end and reports what changed.
---

# Weekly Review Processor

Runs the review process this vault's own `CLAUDE.md` already defines: empty the inbox, update project status, scan daily notes for durable insights, update MOCs. This Skill exists so that process is one trigger phrase, not a re-explained checklist every time.

## Step 1 — Empty `00_Inbox`
For each note in `00_Inbox`:
- Propose where it goes: merge into an existing `10_Projects`/`20_Areas`/`30_Resources` note, becomes a new note of its own, or gets deleted as dead.
- State the reasoning in one line per note — don't just silently file it.
- **Deleting or moving notes is in this vault's "do not touch without asking" list** (see `CLAUDE.md`) — propose the action and get a yes before executing it, don't just do it. Batch the proposals into one list rather than asking note-by-note if there are several.

## Step 2 — Update project/area status
- Go through `10_Projects/` (and relevant `20_Areas/`) notes.
- Flag anything marked "unconfirmed," unresolved, or clearly stale (compare dates in the note against today).
- Ask Arvin directly for the current state rather than guessing or leaving it stale — this is exactly the pattern that already worked for Zapee/Kanban Business Hub status updates.

## Step 3 — Scan daily notes for durable insights
- Look at `50_Daily/` entries since the last review.
- Anything that's actually a durable fact/decision (not just a log line) is a candidate to get promoted into the relevant `10_Projects`/`20_Areas`/`30_Resources` note, same as how portfolio insights or Zapee/Kanban findings got written up permanently rather than left buried in a dated log.
- Leave genuinely time-bound log entries (what happened that day) where they are.

## Step 4 — Update MOCs
- Check `90_MOCs/Home MOC.md` (and any topic MOCs) link to everything new/moved from steps 1–3.
- Remove links to anything archived or deleted.

## Output
End with a short changelog, not a wall of text: how many inbox notes processed (and where each went), how many statuses updated, how many insights promoted, whether MOCs changed. Match the level of detail used when Zapee/Kanban Business Hub statuses were last updated — factual, dated, sourced ("per Arvin, [date]").

## Scope note
This Skill only touches the vault it's run in — a Second-Brain session reviews Second-Brain, a Second-Brain-Personal session reviews Second-Brain-Personal. Never pull one vault's inbox/notes into the other's review, even to cross-reference — that's exactly the domain boundary the two-repo split exists to enforce.
