# Phase 4 — Marketing Content Pipeline

## Goal
Build an AI-assisted content pipeline (ideation → draft → review → publish → feedback) for your e-commerce business, isolated from Personal and Finance domains.

## 1. Domain Placement
This pipeline lives entirely inside the **Business** domain — it reads only from `Business/` vault notes (product info, past campaign performance, brand voice notes) and never pulls from Finance or Personal.

## 2. Four-Stage Lifecycle
1. **Ideation** — a Claude Code skill scans `Business/Resources` and recent performance notes to propose content topics/angles.
2. **Drafting** — AI writer prompts (via Claude Code skill, optionally backed by tools like Jasper/Copy.ai for extra channels) produce first drafts in your brand voice; drafts save into `Business/10_Projects/marketing/drafts`.
3. **Review (maker-checker)** — you approve/edit drafts before anything is scheduled; nothing auto-publishes without this gate.
4. **Distribution** — n8n schedules/publishes approved content across channels via their respective APIs.
5. **Feedback loop** — performance data (engagement, sales lift) flows back into `Business/30_Resources/marketing-performance` notes so the next ideation round is informed by what worked.

## 3. Isolation Notes
- The marketing MCP/automation tools only need access to your content/social APIs and the Business vault — no need for finance or personal data access, so don't grant it.
- If you ever want cross-domain insight (e.g., "does a marketing push affect stock analysis time available"), that's a deliberate Shared-domain question you ask manually, not an automatic data merge.

## 4. Build Order
1. Pilot with one content pillar and one channel — don't build all channels at once.
2. Wire ideation skill to read only Business vault notes.
3. Add drafting skill; store outputs in the drafts folder, not auto-published.
4. Add the approval gate (manual review before scheduling).
5. Connect n8n for distribution once approval is granted.
6. Add the feedback-loop automation last, once you have real performance data to close the loop.

## 5. Verification Checklist
- Confirm the marketing pipeline cannot read Finance or Personal vault folders.
- Confirm no content publishes without a logged human approval step.
- Confirm performance data writes back only into Business vault, not Shared, unless you deliberately promote a general lesson there.
