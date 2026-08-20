---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea (design-tree mode), or quiz him relentlessly on a concept until he can actually explain/derive/apply it (study mode, for School coursework). Use for either when the user wants to stress-test their thinking or understanding, or uses any 'grill'/'quiz me' trigger phrases.
---

> **2026-08-20, Arvin's extension**: added the Study Mode section below on top of the imported skill (`mattpocock/skills`, unmodified original is the Design-Tree Mode section that follows). Same underlying mechanic (rounds, a frontier, don't move on until settled) applied to a different subject: a concept's own dependency structure instead of a plan's decision structure. Kept as one skill rather than a second bespoke Socratic-questioning skill per domain, per the 2026-08-20 roadmap decision in `AI Brain Skills Strategy.md`.

## Study Mode — for concept mastery (School coursework)

Use when Arvin wants to actually understand a concept, not just get it explained once — "quiz me on X," "make sure I get this before the exam," "help me really understand Y."

The tree here is the concept's own **prerequisite structure**: what sub-concepts does understanding X actually depend on? The frontier is whatever sub-concept can be tested *now* without assuming mastery of something not yet checked.

Each round, ask him to explain, derive, or apply a frontier concept in his own words — don't just ask if he "gets it." Unlike Design-Tree Mode, **you already know the correct answer** — the goal isn't gathering his decision, it's checking his understanding against it:
- If his answer is right (even if imperfectly worded), confirm *why* it's right and move the frontier forward.
- If it's wrong or shaky, don't just supply the correct answer — ask a smaller, more targeted question that isolates exactly which part is missing, and let him try again before explaining it yourself.
- Only after he's genuinely attempted a concept does a full explanation from you belong in the conversation.

Respect the academic-integrity boundary already defined in `casual-human-voice`: this mode is for building his own understanding, never for producing graded-assignment answers on his behalf.

Session is done when every prerequisite sub-concept in the tree has actually been demonstrated, not just asked about once.

## Design-Tree Mode — for plans, decisions, and ideas

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Each question should be formatted like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
