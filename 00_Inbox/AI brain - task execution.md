---
type: inbox
captured: 2026-08-16
---

# The vault should be an "AI brain" — not just notes

Arvin's stated need (2026-08-16): when he gives a task, the system should already know how to think about it and execute — not require re-explaining context every time.

## Working notes
- `CLAUDE.md` already does part of this (standing context every session), but that's passive — it informs reasoning, it doesn't package a repeatable procedure.
- `Second-Brain-Personal/50_Daily/Stock portfolio task.md` already has a real example of the actual mechanism that closes this gap: a documented trigger phrase ("execute the stock portfolio task") + a full role/procedure written out, meant to be pasted at the start of a session. That's a skill in everything but name.
- Claude Code has a first-class mechanism for exactly this: **Skills** (`.claude/skills/*.md`, invoked by name or auto-triggered by context) — package a procedure once, invoke it by name forever after, no re-explaining.
- Next concrete step: convert the stock portfolio prompt into a real Skill as a proof of concept, then identify which other recurring tasks (Gmail auto-labeling already exists as a scheduled task per `Email Auto Label.md`; business task types for Zapee/Protein Bar) are worth the same treatment.

## To process during next weekly review
- Decide: is one Skill worth building now as a proof of concept, or wait until more recurring task types are identified?
