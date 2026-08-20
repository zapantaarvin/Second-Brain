# Data Isolation Architecture — Personal vs Business vs Training

## 0. The Core Problem
You are running multiple domains through one AI system: personal life (health, family, home purchase), business (e-commerce financials, supplier contacts, margins), and finance (portfolio, trades, income). These must NOT freely mix in the same context window, memory store, or training pipeline. The system needs explicit, enforced boundaries — not just "the agent will be careful."

**Key principle:** isolation must be enforced at the storage and authorization layer, not just by prompting. A well-behaved prompt is not a security boundary — a namespace filter that the query physically cannot bypass is.

## 1. Isolation Patterns (industry standard, 2026)

| Pattern | Description | When to use |
|---|---|---|
| Fully isolated (siloed) | Separate vault, separate vector DB, separate MCP server per domain | Highest sensitivity (e.g. financial/business data tied to your job's clearance considerations) |
| Logical isolation (shared infra, filtered) | One vector DB, every record tagged with a `domain_id`, every query forced to filter on it | Good middle ground for personal use across domains you trust yourself with |
| Namespace isolation (hybrid) | Shared infrastructure, hard namespace boundaries (e.g. Chroma collections per domain) | **Recommended for Arvin** — best balance of simplicity and safety |

Namespace isolation is the standard hybrid architecture most modern agent platforms converge on: shared compute, but data physically separated by named boundary so a query in one namespace mathematically cannot return another's records.

## 2. Recommended Structure for Your System

### 2.1 Vault-Level Separation
```
Second-Brain/
├── Personal/         # health, family, travel, home purchase
├── Business/         # e-commerce ops, suppliers, marketing
├── Finance/          # portfolio, trades, income, taxes
└── Shared/           # cross-domain notes you explicitly promote here
```
Each top-level folder gets its own PARA substructure. Nothing crosses into `Shared/` automatically — you (or an explicit agent action you approve) must move it there.

> **Update, 2026-08-19 (Claude):** what actually got built diverges from this sketch in one way and converges in another. Diverges: Personal/Finance ended up in the separate **private `Second-Brain-Personal` repo**, not folders inside one vault — a stronger form of isolation than this section originally called for, decided 2026-08-16 (see this file's own §0 core principle: storage-layer isolation, not folders + prompting). Converges: on 2026-08-19, both repos were independently restructured to category-first folders with nested PARA — `Second-Brain` got `General/School/Projects/Business`, `Second-Brain-Personal` got `Personal/Finances` — which is structurally the same idea as this section's per-domain top-level folders, just split across two repos instead of one, and without a `Shared/` folder (no cross-domain reporting layer exists yet — still §5 below, not built).

### 2.2 Vector Store Namespace Separation
- Use **separate collections/namespaces** per domain in Chroma (or separate indexes in Qdrant): `personal_vault`, `business_vault`, `finance_vault`.
- Every embedded chunk is stamped with `domain_id` metadata at ingestion time.
- Every retrieval call requires a `domain_id` filter — this is enforced in code, not left to the agent's judgment.
- Cross-domain queries are only possible through an explicit "bridge" query that the agent must request and you approve, not a default behavior.

### 2.3 MCP Server Separation
- Run **separate MCP server instances** per domain (`obsidian-personal-mcp`, `obsidian-business-mcp`, `finance-mcp`), each pointed only at its own vault folder / data source.
- Each MCP server issues its own scoped, short-lived tokens — never one master token with access to everything.
- Tool-level scoping: a finance MCP tool that reads portfolio data should not also carry file-delete or business-data permissions. Scope at the tool level, not the server level.

### 2.4 Claude Code Session Separation
- Use separate CLAUDE.md files per domain folder (path-scoped), so a session opened in `Business/` never auto-loads `Finance/` or `Personal/` instructions or memory.
- Keep a top-level CLAUDE.md that only knows domain names exist and where they live — not their contents.

## 3. Handling "Training Purposes" and Cross-Domain Learning

You mentioned wanting some data to be usable "for training purposes or other things" without leaking sensitive specifics. The safe pattern is **de-identify before promoting**:

1. Domain-specific raw data stays siloed (as above) and is never used directly for cross-domain learning.
2. If you want the agent to learn *patterns* (e.g., "what marketing angles work" or "what trading setups I favor") without exposing sensitive specifics (exact revenue, account balances, supplier pricing), create a **redaction/summarization step**: an agent task that extracts the abstracted lesson (e.g., "carousel posts outperform single-image posts by 2x") and writes only that lesson into `Shared/`.
3. Treat this redaction step as a maker-checker gate — you review the abstracted note before it moves to `Shared/`, so nothing sensitive slips through in a summary.
4. Never pipe raw financial or business records into a general-purpose fine-tuning or embedding process that other tools/domains draw from. If you eventually fine-tune a personal model, do it on the redacted `Shared/` corpus only, not raw domain vaults.

## 4. Guardrails Checklist

- Every memory read/write is namespaced by domain — no exceptions, enforced in code.
- Every MCP tool is scoped to one domain's data only; no MCP server has blanket access across all three.
- Cross-domain movement requires an explicit "promote to Shared" action, ideally with your approval (maker-checker).
- Audit log every cross-namespace access attempt — if the agent ever tries to pull Finance data into a Business session, that's flagged and logged.
- Regularly test isolation with "canary" data: put a fake sensitive note in Finance, then ask the agent a Business question and confirm it never surfaces.
- No long-lived, all-access tokens. Scoped, short-lived, per-domain credentials only.

## 5. References
- Fastio, *Multi-Tenant AI Agent Architecture: Design Guide (2026)* — namespace isolation as the standard hybrid pattern
- AWS Well-Architected, *Implement memory isolation and integrity* — hierarchical namespace schema for agent memory
- Cloud Security Alliance, *Agentic MCP Security Best Practices Guide* — tool-level scoping, canary testing for cross-tenant leakage
- JumpCloud, *What Is Namespace-Isolated Memory Partitioning?* — retrieval scoping mechanics
- WorkOS, *The security risks specific to MCP servers* — session-scoped auth, no long-lived tokens
- dev.to (Jack M), *AI Agent Tenant Isolation* — context isolation design rules
