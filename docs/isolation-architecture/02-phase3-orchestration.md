# Phase 3 — Orchestration Layer (Automation + Agent Coordination)

## Goal
Connect your isolated tools through a deterministic automation backbone (n8n), while keeping Claude Code as the judgment layer for anything ambiguous — without collapsing domain boundaries.

## 1. n8n as the Automation Spine
- Self-host n8n; it becomes the "glue" that triggers mechanical actions (new order → Kanban card, price alert → note in Finance vault).
- Each n8n workflow should touch only one domain's credentials/webhooks at a time. Avoid building a single workflow that pulls Business and Finance data into the same automation unless you've explicitly decided that's a "Shared" workflow.
- Use n8n's AI Agent node only for judgment calls within a single domain (e.g., "categorize this incoming order"), not cross-domain reasoning.

## 2. Claude Code as Planner/Executor for Non-Routine Work
- Ambiguous tasks (drafting content, evaluating a stock thesis, deciding on a reorder) route through Claude Code + domain-scoped subagents.
- Each subagent only has MCP access to its own domain's tools — a "finance-analysis" subagent should never be handed business MCP credentials.
- Results get written back into that domain's Obsidian folder; nothing writes cross-domain without an explicit "promote to Shared" step.

## 3. Maker-Checker Gates
- Any automated action touching money, publishing, or inventory requires human approval before execution — insert an n8n "wait for approval" node or a Claude Code confirmation step.
- Log every gated decision (what was proposed, what was approved/rejected) for audit purposes.

## 4. Build Order
1. Stand up n8n; connect it to Kanban MCP/webhooks only, test one simple automation (new card → notification).
2. Add e-commerce webhook triggers (new order → Kanban card) — single domain only.
3. Add finance price-alert automation (price threshold → note in Finance vault) — single domain only.
4. Add approval gates on any workflow that would write, spend, or publish.
5. Only after single-domain automations are stable, evaluate whether any specific cross-domain workflow is worth building deliberately (with explicit promotion-to-Shared logic), rather than defaulting to shared access.

## 5. Verification Checklist
- No n8n workflow holds credentials for more than one domain unless it's a deliberately-designed "Shared" workflow you've reviewed.
- Every gated action has a visible approval step in the audit log.
- Test failure mode: disconnect one domain's MCP server and confirm other domains' automations keep working unaffected.
