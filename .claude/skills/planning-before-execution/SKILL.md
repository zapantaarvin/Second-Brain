---
name: planning-before-execution
description: Use whenever Arvin asks for a project or multi-step task — not a single small edit, a direct question, or a quick fix he's already fully specified. Always plan first (checklist, not prose), get explicit approval before executing, and when later asked to execute, work the checklist in order with visible progress on each item.
---

# Planning Before Execution

## Purpose
Arvin's standing rule (2026-08-20): any real project or task gets planned and checklisted before anything gets built — never jumped into directly. Core/cross-cutting because it governs *how* work gets approached in general, not one task's specific procedure.

## When this applies
Multi-step work — building something, restructuring something, anything with several distinct actions or genuine scope to figure out. **Does not apply** to a single small edit, answering a direct question, a quick fix already fully specified, or continuing work already mid-execution under an approved plan.

## Process

### 1. Plan first
Call `EnterPlanMode` before editing anything or taking any non-read-only action. Research/explore as needed under the harness's own plan-mode workflow.

### 2. Checklist, not prose
The plan itself must be an explicit checklist — `- [ ]` items, one per concrete action — not a narrative description of the approach. This vault's own `docs/EXECUTION-PLAN.md` is the reference format: tiered by who can act on each item, one line per item, dated completion notes added once done, not deleted.

### 3. Validate before execution
Call `ExitPlanMode` and get an explicit yes. Don't treat silence, a related-but-different reply, or an old approval from earlier in a long conversation as still valid — confirm again if real time or context has passed since the plan was approved.

### 4. Execute with visible progress
When asked to execute, work the checklist in order. Mark each item done as it completes and say what actually happened — a command's real output, a file that changed, a link to what got built — not just "done." Update the checklist as you go rather than batching everything into a single end-of-run summary; the checklist itself is the progress tracker, the same way `EXECUTION-PLAN.md` gets checked off item by item.

## Calibration
The 2026-08-19 category-first reorg and the skills-roadmap execution pass (2026-08-20) both already followed this shape successfully before it was written down as a rule — use those as the reference for what "right" looks like, not a fresh interpretation each time.
