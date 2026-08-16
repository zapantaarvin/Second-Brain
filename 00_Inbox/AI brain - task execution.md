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
- ✅ **Done (2026-08-16):** proof-of-concept Skill built — `Second-Brain-Personal/.claude/skills/stock-portfolio/SKILL.md`. Original copy-paste prompt in `Stock portfolio task.md` replaced with a pointer to it.
- Arvin also asked Perplexity to research a fuller skill taxonomy for this system (prompt drafted 2026-08-16) — pending his results.

## To process during next weekly review
- Once Perplexity's research comes back: pick the next 2-3 highest-value Skills to build (candidates already identified: Gmail auto-labeling — already exists as a scheduled task per `Email Auto Label.md`, just needs converting; business task types for Zapee/Protein Bar).
- Decide where each new Skill should live — domain matters: personal/finance skills go in `Second-Brain-Personal/.claude/skills/`, business skills in `Second-Brain/.claude/skills/`, matching the same isolation principle as the notes themselves.
