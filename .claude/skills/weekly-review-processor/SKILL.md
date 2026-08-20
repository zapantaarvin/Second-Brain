---
name: weekly-review-processor
description: Use when Arvin says "do the weekly review," "process my vault inbox / 00_Inbox," or asks for a vault review, in either Second-Brain or Second-Brain-Personal. This is about the Obsidian vault's inbox, not Gmail — see gmail-auto-label for that. Walks the vault's own 4-step review process (CLAUDE.md) end-to-end and reports what changed.
---

# Weekly Review Processor

Runs the review process this vault's own `CLAUDE.md` already defines: empty the inbox, update project status, scan daily notes for durable insights, update MOCs. This Skill exists so that process is one trigger phrase, not a re-explained checklist every time.

## Step 1 — Empty `00_Inbox`
For each note in `00_Inbox`, this is a **two-stage decision** since the reorg to category-first folders (2026-08-19): first which category (per root `CLAUDE.md`'s category map), then which PARA bucket within it (`10_Projects`/`20_Areas`/`30_Resources`).
- Propose the full path: category + PARA bucket, or a new note of its own, or delete as dead.
- State the reasoning in one line per note — don't just silently file it. If the category itself is unclear, say so explicitly rather than guessing.
- **Deleting or moving notes is in this vault's "do not touch without asking" list** (see `CLAUDE.md`) — propose the action and get a yes before executing it, don't just do it. Batch the proposals into one list rather than asking note-by-note if there are several.

## Step 2 — Update project/area status
- Go through each category's `10_Projects/` (and relevant `20_Areas/`) notes.
- Flag anything marked "unconfirmed," unresolved, or clearly stale (compare dates in the note against today).
- Ask Arvin directly for the current state rather than guessing or leaving it stale — this is exactly the pattern that already worked for Zapee/Kanban Business Hub status updates.

## Step 3 — Scan daily notes for durable insights
- Look at `50_Daily/` entries since the last review (vault-wide, not per category).
- Anything that's actually a durable fact/decision (not just a log line) is a candidate to get promoted into the relevant category's `10_Projects`/`20_Areas`/`30_Resources` note — same two-stage decision as Step 1 — same pattern as how portfolio insights or Zapee/Kanban findings got written up permanently rather than left buried in a dated log.
- Leave genuinely time-bound log entries (what happened that day) where they are. Note: some `50_Daily/` entries (e.g. `Stock portfolio task.md`, `Email Auto Label.md`) are deliberately dated task-logs that stay in `50_Daily/` permanently, not just until reviewed — don't promote/move those out.

## Step 4 — Update MOCs
- Check `90_MOCs/Home MOC.md` (and any topic MOCs) link to everything new/moved from steps 1–3.
- Remove links to anything archived or deleted.

## Output
End with a short changelog, not a wall of text: how many inbox notes processed (and where each went), how many statuses updated, how many insights promoted, whether MOCs changed. Match the level of detail used when Zapee/Kanban Business Hub statuses were last updated — factual, dated, sourced ("per Arvin, [date]").

## Scope note
This Skill only touches the vault it's run in — a Second-Brain session reviews Second-Brain, a Second-Brain-Personal session reviews Second-Brain-Personal. Never pull one vault's inbox/notes into the other's review, even to cross-reference — that's exactly the domain boundary the two-repo split exists to enforce.
