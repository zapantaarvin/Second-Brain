---
name: reflecting-on-skills
description: Use when Arvin explicitly says "reflect on [skill]," "update this skill based on how that went," or gives a clear, decisive verdict on a skill's output ("I hate this," "this is exactly right," a firm keep/scrap call) — not on routine mid-session corrections like "make it shorter" or "try again." Analyzes the session for corrections/approvals/patterns and proposes a skill update with a confidence level — never edits the skill without explicit confirmation.
---

# Reflecting on Skills

## Purpose
Skills don't improve on their own. Adapted from the "reflect" pattern described in Developers Digest's "Self-Improving Skills: Claude Code That Learns From Every Session" (2026-08-19 research) — but built to match this vault's own "propose, don't auto-execute" convention rather than the article's optional auto-hook variant. This Skill is how a Skill actually gets sharper from real use, deliberately, with a human confirming each change.

## Process
1. Identify which Skill just ran (or ask if ambiguous) and review the session for:
   - **Corrections** Arvin made to the output
   - **Approvals** he gave without correction (confirms current behavior already works)
   - **Patterns** that succeeded or failed, not yet captured in the skill's instructions
2. Draft a proposed change to the Skill's `SKILL.md` — a specific edit, not vague guidance — with a confidence level:
   - **High** — a clear, repeated correction or an explicit instruction Arvin gave
   - **Medium** — inferred from one session, plausible but unconfirmed
   - **Low** — a hunch worth logging, not worth acting on yet
3. Show the proposed diff and confidence level. Get an explicit yes before editing the actual `SKILL.md` — never auto-apply, regardless of confidence level.
4. Whether applied or not, append an entry to `<skill-folder>/REFLECTIONS.md` (create it if it doesn't exist yet): date, what was observed, confidence, and whether it was applied. Keep prior entries — this is a history, not a snapshot to overwrite.

## Scope
Works on any Skill in either vault, repo-root or nested. If the target Skill is one of the shared/synced ones (`casual-human-voice`, `weekly-review-processor`, `auditing-skill-triggers`, `grill-me`, `grilling`), remind Arvin to run `sync-shared-skills.sh` after applying a change — `REFLECTIONS.md` itself is per-repo history, not synced (same reasoning as `_trigger-log.md`: the skill definition is shared, the usage history is per-vault).

## What this is not
Not the fully automatic version some write-ups describe — a stop-hook silently rewriting skills after every session, with no review. That contradicts this vault's own do-not-touch conventions ("don't touch without asking," "propose changes, get a yes"). Every applied change here is explicitly confirmed by Arvin first, same as `weekly-review-processor`'s inbox proposals.
