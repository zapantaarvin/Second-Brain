---
type: resource
captured: 2026-08-16
moved_from: 00_Inbox
---

# The vault should be an "AI brain" — not just notes

Arvin's stated need (2026-08-16): when he gives a task, the system should already know how to think about it and execute — not require re-explaining context every time.

## Working notes
- `CLAUDE.md` already does part of this (standing context every session), but that's passive — it informs reasoning, it doesn't package a repeatable procedure.
- Claude Code has a first-class mechanism for exactly this: **Skills** (`.claude/skills/*.md`, invoked by name or auto-triggered by context) — package a procedure once, invoke it by name forever after, no re-explaining.
- ✅ **Done (2026-08-16):** proof-of-concept Skill built — `Second-Brain-Personal/.claude/skills/stock-portfolio/SKILL.md`. Original copy-paste prompt in `Stock portfolio task.md` replaced with a pointer to it.
- ✅ **Done (2026-08-16):** `Second-Brain-Personal/.claude/skills/gmail-auto-label/SKILL.md` built, reconstructed from one observed run of the `gmail-auto-label` scheduled task. Also relocated `Email Auto Label.md` from this (public) repo to `Second-Brain-Personal` — it's personal inbox content and had been misplaced before the domain-boundary rule existed.
- Arvin asked Perplexity to research a fuller skill taxonomy for this system (prompt drafted 2026-08-16) — results captured below.
- ✅ **Done (2026-08-16):** `weekly-review-processor` Skill built and duplicated into **both** repos' `.claude/skills/` — operationalizes the 4-step review process already defined in `CLAUDE.md`, respects the existing "don't bulk-move/delete without asking" rule by proposing changes rather than auto-executing them.
- ✅ **Done (2026-08-19):** Deep research on scaling a Skills library past a handful of skills — sourced live from Anthropic's current docs, not training-data guesses. Key findings: a real trigger-eval methodology exists (should-trigger/should-not/ambiguous, tested in isolation *and* against the active skill set); Anthropic ships a meta-skill (`skill-creator` plugin) with a measured train/test trigger-rate loop, install via `/plugin install skill-creator@anthropic-agent-skills` — **still pending, needs an interactive session to run the `/plugin` command, Claude can't install it headlessly**; recall degrades past ~10-15 simultaneously-loaded skills per vault, which is a real argument *for* keeping the two-repo split rather than consolidating. Full report was scratch-only and not persisted; see this file's own updates below for what got acted on.
- ✅ **Done (2026-08-19):** `auditing-skill-triggers` Skill built and duplicated into **both** repos' `.claude/skills/` (registered in `sync-shared-skills.sh`'s `SHARED_SKILLS`) — closes the "haven't verified any of the four yet" gap below by making trigger verification a repeatable, dated, logged process (`.claude/skills/_trigger-log.md` per vault) instead of a one-off check. First manual run already done as part of building it: found and fixed a real collision (`weekly-review-processor` vs `gmail-auto-label` both matching on bare "inbox" — description edited to disambiguate vault-inbox vs Gmail, in both repos); all 4 pre-existing skills pass should-trigger/should-not-trigger/ambiguous checks — see the trigger logs for detail.
- ✅ **Done (2026-08-16):** `casual-human-voice` Skill built and duplicated into **both** repos' `.claude/skills/` — not in Perplexity's original list (it's cross-cutting style guidance, not a task-execution skill), added at Arvin's request. Deliberately not domain-specific, so duplicating across repos doesn't violate the isolation principle in §4 below (that principle is about data leakage, not style preferences). First draft was generic; superseded same-day by a much more specific spec Arvin wrote himself (found in `Second-Brain`'s inbox, misfiled there — relocated to `Second-Brain-Personal/20_Areas/School/Casual Human Voice - source draft.md`) covering exact vocabulary to avoid, no em dashes, sentence-rhythm rules, and a UT Austin academic-integrity boundary. The Skill files in both vaults now use that richer version, generalized slightly beyond just school/email.

---

# AI Brain Skills Taxonomy & Build Plan
> Perplexity research, captured 2026-08-16.

## 1. Taxonomy of Skills for a Personal AI Brain

Not every recurring task deserves a Skill. Rule of thumb: **if you're re-explaining the same context more than twice a week, it's a Skill candidate.**

| Category | What it covers | Worth a Skill when... |
|---|---|---|
| **Capture/Triage** | Inbox processing, categorization, routing to project/area/resource | Sorting logic is stable and repeated on a schedule (weekly review, Gmail labeling) |
| **Research/Analysis** | Multi-step investigation, data pulls, synthesis | The same analytical steps recur (stock reads, competitor scans, formulation checks) |
| **Drafting/Content** | Generating first drafts in a consistent voice/format | You have a defined output format and brand voice worth encoding once |
| **Monitoring/Reporting** | Scheduled or triggered status digests | Report structure is fixed and recurring (weekly digest, portfolio snapshot) |
| **Decision-Support** | Structured recommendation with explicit criteria (position sizing, reorder decisions) | Judgment needed but the *framework* for judgment is stable and reusable |
| **Compliance/Checklist** | Regulatory or procedural steps that must not be skipped | Missing a step has real cost (DTI/FDA labeling, food safety) — checklists reduce error, not creativity |

**Ad hoc, no Skill needed:** one-off decisions, exploratory brainstorming, anything where the procedure changes every time. Packaging those wastes context budget for no reliability gain.

***

## 2. Highest-Value Skills for My Context, Prioritized

Ranked by recurrence × re-explaining cost:

| Rank | Skill | Category | Status | Why it's high-value now |
|---|---|---|---|---|
| 1 | **Stock portfolio co-pilot** | Decision-support | ✅ Built | Already run informally; screenshot-parsing + position-sizing logic is exactly what a Skill should encode |
| 2 | **Gmail auto-labeling** | Capture/triage | ✅ Built | Already scheduled and repetitive — near-zero judgment variance, ideal first Skill + hook combo |
| 3 | **Weekly review processor** | Capture/triage | ✅ Built | Existing inbox→PARA habit; turns "sort my inbox" into a one-line trigger |
| 4 | **Kanban triage/status digest** | Monitoring/reporting | Not built | Recurs across all three businesses; one parameterized Skill avoids re-explaining structure each time |
| 5 (later) | Community marketing content drafter | Drafting/content | Hold | High value but needs a stable brand-voice reference file first — build after enough real drafts exist to distill a voice guide |
| 6 (later) | Finance-coaching-app content/logic skill | Research/analysis + drafting | Hold | Premature — product methodology still being defined; would need rewriting as it evolves |
| — (added, not ranked) | **Casual human voice** | Cross-cutting (style) | ✅ Built | Not a task-execution skill, so it doesn't fit the recurrence-ranking above — applies to all written output in both vaults regardless of domain |
| — (added, not ranked) | **Auditing skill triggers** | Cross-cutting (meta — audits the Skills system itself) | ✅ Built | Not a task-execution skill either — periodically re-verifies the other skills' trigger reliability as the library grows, per the "recall degrades past ~10-15 skills" research finding above |

***

## 3. What Good Skill Design Looks Like

- **Narrow scope, one job.** Split `dti-fda-labeling-check` from `shopee-listing-optimizer` rather than merging into one "e-commerce stuff" Skill.
- **The `description` field is everything.** It's what Claude matches against a request to decide whether to load the Skill — state both *what* it does and *when* to use it, with concrete trigger phrases.
  - Good: "Use when reviewing a new SKU or ingredient list for DTI/FDA compliance before listing or packaging."
  - Bad: "Helps with compliance."
- **Progressive disclosure.** Keep `SKILL.md`'s always-loaded body lean (well under 500 lines); push edge cases, templates, and reference material into companion files (`reference.md`, `examples.md`) loaded only when needed.
- **Explicit input/output contract.** State expected input (e.g., "a brokerage screenshot or ticker + position size question") and expected output (e.g., "trend, risk flags, suggested position size, confidence level").
- **Deterministic work goes in scripts, not prose.** Bundle a script for fixed computations (CSV parsing, labeling schema checks) rather than re-deriving logic from instructions every time.
- **Avoid overlap.** If two Skills could both plausibly trigger on the same phrase, merge or sharply differentiate their descriptions — ambiguous triggers cause silent misfires.
- **Test the trigger, not just the logic.** Say the natural phrase you'd use and confirm the right Skill loads; if not, fix the description, not the underlying logic.

***

## 4. Scoping Skills to Respect Personal / Finance / Business Isolation

- **Location-based isolation is the primary control.** Skills in a repo's `.claude/skills/` only load in sessions opened against that repo/vault. Keep Business Skills in the Business repo, Personal/Finance Skills in their own repo — never in the global `~/.claude/skills/`.
- **Scope tool permissions (`allowed-tools`) per domain.** A Kanban/e-commerce Skill should list only Business MCP tools; the stock co-pilot Skill should list only Finance MCP tools. Never grant broader tool access than the one job needs.
- **Use Subagents for rare, deliberate cross-domain reads.** A subagent's isolated context window means the main session doesn't inherit cross-domain data by default — only a distilled result returns.
- **Hooks/permissions enforce hard boundaries; Skills only carry contextual knowledge.** Absolute rules ("never let a Business skill open the Finance vault") belong in a permission/hook, not just a description — descriptions are guidance, not security.
- **Naming convention makes scope obvious at a glance:** prefix by domain — `business-dti-fda-check`, `finance-portfolio-review`, `personal-weekly-review`. Prevents accidental duplicate triggers across repos.

***

## 5. Sensible Build Order

**Build now (first set — high recurrence, stable logic, immediate ROI):**
1. ✅ Gmail auto-labeling — simplest, already scheduled, near-zero ambiguity; validates the workflow before investing more.
2. ✅ Weekly review/inbox processor — reinforces the existing PARA habit, immediately reduces manual triage time.
3. ✅ Stock portfolio co-pilot — highest re-explaining cost today; codify the screenshot-read → position-sizing logic already done manually.

**Build next (once the first set is stable and trusted):**
4. Kanban status digest, parameterized per business — wait until real usage reveals preferred digest format.

**Hold off — premature right now:**
- Community marketing content drafter — needs a distilled brand-voice reference file first.
- Finance-coaching-app-specific Skills — product logic isn't stable yet; revisit once the app's methodology settles.

**Guiding principle:** prove reliability on narrow, low-ambiguity tasks first, then extend into higher-judgment or less-stable domains.

***

## Candidates surfaced by 2026-08-19 research (not yet built, not yet ranked into §5)
- **Monthly budget review co-pilot** (`Second-Brain-Personal`) — same proven pattern as `stock-portfolio`, applied to the numeric structure already in `Finance Dashboard.md`. Arvin passed on building this now (2026-08-19); revisit later.
- **Zapee competitor price & reorder-decision check** (`Second-Brain`) — ties to Zapee's own blueprint's "weekly reorder rhythm." Build after Kanban status digest per the existing build order — more judgment variance than the narrow tasks proven so far.
- **UT Austin semester deadline capture** (`Second-Brain`) — no deadline tracking exists across the 6 course notes. Arvin passed on building this now (2026-08-19); lower urgency, per-semester not weekly.

## Next Actions
- [x] Move this note out of 00_Inbox into the appropriate Area/Project once reviewed
- [x] Create `.claude/skills/` folder in Finance/Personal repo (Second-Brain-Personal)
- [x] Draft SKILL.md for Gmail auto-labeling
- [x] Create `.claude/skills/` folder in Business repo (Second-Brain) — done, holds `casual-human-voice` and `weekly-review-processor`
- [x] Draft SKILL.md for weekly review processor
- [x] Draft SKILL.md for casual human voice (added mid-stream, not in original Perplexity plan)
- [ ] Test each built Skill's trigger actually fires on a natural phrase (per §3's own advice — haven't verified any of the four yet, only that the files exist)
