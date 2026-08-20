---
name: auditing-skill-triggers
description: Use when Arvin says "audit my skills," "check skill triggers," "run the skills library audit," or asks whether the Skills in this vault are still working correctly. Runs sync-shared-skills.sh for drift, then walks each skill's description through a should-trigger/should-not-trigger/ambiguous eval and logs results with a date, so trigger reliability gets re-verified over time instead of checked once and forgotten.
---

# Auditing Skill Triggers

## Purpose
Closes a specific gap: Skills get written once, and nothing re-checks later whether their trigger descriptions still work as more Skills get added — a phrase that used to be unambiguous can start colliding once a fifth or sixth skill enters the picture. This Skill is the mechanism that keeps checking.

## Process

### 1. Drift check (cross-cutting skills only)
Run `../sync-shared-skills.sh` from this vault's parent (`Brain/`). Report any drift; don't silently fix it — same read-first behavior as the script itself.

### 2. Trigger eval, per skill in this vault's `.claude/skills/`
For each skill, read its `description` (and `when_to_use` if present). Write or reuse 3-5 test phrases covering:
- **Should-trigger** — a natural phrase Arvin would actually say for this skill's job
- **Should-NOT-trigger** — a phrase for a *different* skill's job; confirm it doesn't falsely match this one
- **Ambiguous** — a phrase that's genuinely underspecified; note whether this skill (or a sibling's description) resolves it, or whether it should just prompt Arvin to clarify

Reason through each against the description the way Claude actually decides whether to invoke a Skill — this is a judgment-based review, not a scripted test. For a higher-confidence pass on a *new* skill's initial tuning, use Anthropic's `skill-creator` plugin instead (measured train/test trigger-rate loop); use this lighter check for periodic re-checks across the whole library.

### 3. Coexistence check
Re-read every *other* skill's description in this vault side by side. Flag any pair whose trigger phrases could plausibly both fire on the same input — this is how the `weekly-review-processor`/`gmail-auto-label` "inbox" collision was caught (2026-08-16), fixed by editing the description, not the logic, per this vault's own Skills design principle.

### 4. Log results
Append to `.claude/skills/_trigger-log.md` in this vault: one row per skill, dated, pass/fail per test category, one-line notes on anything fixed or still open. Keep prior entries — this is a history, not a status snapshot to overwrite.

## Output
A short summary: what got tested, what passed, what got fixed, what's still open. Match the level of detail in `weekly-review-processor`'s changelog output — not a wall of text.

## When to run this
- After adding or editing any Skill in this vault
- Periodically as the Skill count grows (recall degrades past ~10-15 simultaneously-loaded skills — this is the check that catches that early)
- When Arvin asks directly
