# Skill Trigger Log

Append-only history of trigger-reliability checks. See the `auditing-skill-triggers` Skill for the process. Don't overwrite prior rows — this is a history, not a snapshot.

| Date | Skill | Should-trigger | Should-not-trigger | Ambiguous | Notes |
|---|---|---|---|---|---|
| 2026-08-19 | weekly-review-processor | Pass | Pass | Pass (with fix) | Found collision with `gmail-auto-label` over bare "inbox" wording; description edited in both repos to specify vault-inbox vs Gmail, re-tested, passes. |
| 2026-08-19 | casual-human-voice | Pass | Pass | Pass | "help me with [assignment]" correctly triggers; the skill's own academic-integrity boundary already handles the graded-work edge case. |
