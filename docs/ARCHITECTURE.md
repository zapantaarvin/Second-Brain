# Arvin's Second Brain + Agentic System — Master Architecture Spec
> **Reference document, not the standing brief.** Per its own §5.1 guidance ("keep CLAUDE.md under ~150–200 lines"), this full spec now lives here as background reading, while the actual per-session brief is the root [`CLAUDE.md`](../CLAUDE.md). Update this file when the long-term architecture changes; update the root CLAUDE.md when day-to-day conventions or current build status changes.
>
> Defines philosophy, architecture, tech stack, folder structure, and build order — based on 2026 best practices from Anthropic, LangChain, and the PKM/agentic-RAG community. Every claim below is sourced.

## 0. Purpose & Operating Principle

This system is a **personal second brain + agentic execution layer**. Obsidian is the human-readable knowledge store; Claude Code is the reasoning/execution engine; a memory + retrieval layer connects the two so that every future query benefits from everything captured before.

**Golden rule (Anthropic):** default to the simplest structure that works — a single LLM call, then a deterministic workflow, and only escalate to a full autonomous agent loop when the task is genuinely ambiguous and worth the added cost/latency. Complexity (memory, planning, multi-agent, reflection loops) is earned, not assumed.

---

## 1. System Layers (Top-Level Mental Model)

| Layer | Role | Primary Tech |
|---|---|---|
| Capture | Fast, frictionless input from any device | Obsidian Inbox + mobile app |
| Knowledge Store | Durable, linked, human-readable notes | Obsidian vault, Markdown, PARA + Zettelkasten |
| Retrieval | Turn notes into machine-queryable knowledge | Hybrid RAG: BM25 + dense embeddings + reranker |
| Memory | Persist facts/preferences across sessions | CLAUDE.md + auto-memory + Mem0/Letta |
| Reasoning/Orchestration | Plan, decide, delegate | Claude Code + subagents, ReAct loop |
| Tool/Action Layer | Execute real work (files, APIs, trading data, e-commerce) | MCP servers |
| Governance/Observability | Trace, verify, cap runaway loops | Tracing, stop conditions, human-in-loop gates |

---

## 2. Layer 1 — Obsidian as the Second Brain

### 2.1 Method: CODE + PARA (Tiago Forte)
- **Capture** everything into a single `00_Inbox` folder/note — no filtering at capture time.
- **Organize** using PARA — sort by *actionability*, not topic: Projects, Areas, Resources, Archive.
- **Distill** — compress each note to its essence so future-you (or the agent) grasps it in seconds.
- **Express** — notes should feed outputs: trade theses, e-commerce SOPs, travel plans, reports.

### 2.2 Recommended Vault Structure
```
Second-Brain/
├── 00_Inbox/            # unsorted daily capture
├── 10_Projects/         # e-commerce launch, home purchase, trip planning
├── 20_Areas/            # investing, career, health, business ops
├── 30_Resources/        # semiconductor research, market notes, SOPs
├── 40_Archive/
├── 50_Daily/            # daily notes (journal + market log)
├── 90_MOCs/             # Maps of Content — topic hubs
└── CLAUDE.md            # standing instructions for Claude Code (see §5)
```

> **Update, 2026-08-19 (Claude):** this diagram is the original flat-PARA sketch and is now historical — the vault was restructured to category-first (`General`/`School`/`Projects`/`Business`, each nesting its own PARA + `CLAUDE.md`) per Arvin's request. `00_Inbox`/`50_Daily`/`90_MOCs` stayed vault-wide as sketched here; `10_Projects`/`20_Areas`/`30_Resources`/`40_Archive` moved inside each category folder instead. See the root `CLAUDE.md`'s category map for the current structure — not rewriting this section since it's the original recommendation, not obsolete reasoning.

### 2.3 Core Plugins
Daily Notes, Templater, Dataview (query notes like a database), Calendar, Smart Connections (chat with your notes), Obsidian Git/Sync for backup.

### 2.4 Linking Discipline
Link concepts with `[[double brackets]]` only when a genuine relationship exists; use the backlinks panel to surface emergent connections; build MOCs as topic hubs. Do a weekly review: empty inbox, update project status, scan daily notes for durable insights, update MOCs.

---

## 3. Layer 2 — Retrieval: Turning the Vault into Agentic RAG

### 3.1 Why Agentic RAG (not static RAG)
Static RAG = one retrieval, then generate. **Agentic RAG** treats retrieval as a tool the model can call multiple times, deciding whether to retrieve, rewriting the query, judging results, and retrying before answering. Use it when: queries are multi-hop, ambiguous, or faithfulness matters more than latency (e.g., financial theses, investment decisions). **Start static, earn the loop** — ship classic hybrid-search RAG first, add agentic escalation only where the static baseline demonstrably fails.

### 3.2 Six-Layer RAG Pipeline (2026 default)
1. **Ingestion** — parse Markdown, chunk (512–1024 tokens, 50–100 overlap), semantic re-chunk on section boundaries, dedupe.
2. **Indexing** — hybrid: BM25 (lexical) + dense embeddings; add a graph layer for cross-note relationships.
3. **Query processing** — classify intent, rewrite ambiguous queries, decompose multi-hop questions into sub-queries.
4. **Retrieval** — hybrid search with reciprocal rank fusion across BM25 + dense.
5. **Reranking** — cross-encoder reranker over an over-fetched candidate set (50 in, 5–8 out).
6. **Generation + evaluation** — LLM answers grounded on context; a judge scores faithfulness/groundedness; loop back to retrieval if flagged.

### 3.3 Agentic Loop Guardrails (critical — prevents runaway cost)
- Hard max-hop budget: 3–5 hops, enforced in code, not just prompt instruction.
- Falling confidence threshold per hop — each retry must clear a lower bar to stop, forcing convergence.
- Forced answer on final hop, with explicit acknowledgment of gaps.
- Latency SLA with streaming fallback (~8s).
- Degrade to plain RAG if planner errors — never fail to a blank page.
- Prompt-injection defense on retrieved chunks: sandwich/strip untrusted text, never let it override the system prompt.

### 3.4 Vector Store Choice
| Option | Best for | Notes |
|---|---|---|
| **Chroma** | Local prototyping, personal vault size (<1–2M chunks) | Zero-config, in-process, default for LangChain/LlamaIndex |
| **Qdrant** | If you scale to a large corpus / need rich metadata filters | Best filtered-query performance, self-hostable in Docker |
| **pgvector** | If you already run Postgres for other business data (e-commerce) | No new service, fine under ~5M vectors |

**Recommendation for Arvin:** start with **Chroma**, embedded locally alongside the Obsidian vault; migrate to Qdrant only if the corpus or query load grows materially.

---

## 4. Layer 3 — Agent Architecture (Loop Engineering)

### 4.1 Core Definition
An agent is "an LLM in a `while` loop with tools." **Loop engineering** is the discipline of designing that loop deliberately: what tools it can call, when it stops, what stays in context, how it verifies work, and the guardrails around it — rather than hand-crafting each prompt.

### 4.2 The Four Pillars (Brain, Planning, Memory, Tools)
- **Brain** — the LLM itself (reasoning core).
- **Planning** — decomposes a goal into ordered steps; ReAct (Thought→Action→Observation→repeat) is the dominant pattern in 2026.
- **Memory** — short-term (context window) + long-term (persisted across sessions).
- **Tools** — the agent-computer interface; documented and tested like an API.

### 4.3 Proven Architecture Patterns — pick per task, not one-size-fits-all
| Pattern | When to use |
|---|---|
| Single-agent ReAct | Simple tool-use, one loop |
| Plan-and-Execute (planner + cheaper executor) | Long-horizon, decomposable tasks |
| Hierarchical supervisor/orchestrator-worker | Multiple specialists, e.g. research + writing + trading-analysis subagents |
| Maker-checker / Evaluator-Optimizer | High-stakes outputs (investment memos, financial models) |
| Reflexion (self-critique loop) | Quality-sensitive iterative work |
| Task Graph + Policy Loop | Parallel sub-tasks with monitoring |

### 4.4 Anthropic's Build Checklist (apply before adding any complexity)
1. Can this be a single LLM call + good prompt? If yes, stop there.
2. Can it be a deterministic workflow? If yes, use that — cheaper, faster, more debuggable.
3. Is the task genuinely ambiguous enough to require autonomy?
4. Is the task's value high enough to justify agent-level token cost?
5. Design for the cost of errors first: permissions, approval gates, sandboxing.

Build order: Environment → Tools → System prompt → Model loop → *then* memory/planning/reflection only if still needed.

### 4.5 Framework Choice
| Framework | Best for | 2026 status |
|---|---|---|
| **LangGraph** | Complex, stateful, long-horizon workflows; built-in checkpointing/time-travel debugging | Production leader, 36.8K★, 34.5M downloads/mo |
| **CrewAI** | Fast multi-agent prototyping, role-based crews | Largest community (44–46K★), broad MCP/A2A support |
| **OpenAI Agents SDK** | Minimal, fast-to-ship handoff chains | Simplest mental model, now supports 100+ models via LiteLLM |
| **Claude Agent SDK / Claude Code itself** | Coding + file-heavy autonomous work, "give the agent a computer" | Best for Arvin's actual use case — building via Claude Code natively |

**Recommendation for Arvin:** use **Claude Code + subagents + skills** as the primary orchestrator, and reach for **LangGraph** only if/when you build a standalone automated pipeline (e.g., a scheduled market-scanning agent) outside the Claude Code session.

---

## 5. Layer 4 — Memory System (Cross-Session Intelligence)

### 5.1 CLAUDE.md — The Standing Brief
- Lives at vault/project root; Claude Code reads it automatically every session.
- **Keep under ~150–200 lines.** Longer files dilute adherence and cost context budget every session.
- Structure: project/system overview → tech stack → architecture/folder map → conventions → "do not touch" list → pointers (`@imports`) to detailed docs.
- State the *why*, not the obvious (Claude can read the file tree itself).
- Use progressive disclosure: move verbose, one-off procedures into **Skills** invoked on demand, not into standing context.
- Never duplicate what a linter/rule engine already enforces deterministically.

### 5.2 Auto-Memory
Claude Code now writes its own memory notes from corrections/preferences into `MEMORY.md` (~200-line cap), loaded every session. Review and prune this periodically — it's organized by theme, not chronology.

### 5.3 Path-Scoped Rules
Put domain-specific instructions in `.claude/rules/*.md` with `paths:` frontmatter so, e.g., e-commerce rules load only when working in the e-commerce folder, trading/finance rules only when in the investing folder.

### 5.4 Long-Term Structured Memory (beyond CLAUDE.md)
- **Mem0** — lightweight, framework-agnostic memory layer; `add()`/`search()` API, bolt-on to any agent.
- **Letta (MemGPT)** — full agent runtime with tiered memory (core/archival), if you want the agent itself to self-edit its memory.

**Recommendation:** Start with CLAUDE.md + native auto-memory (zero extra infra). Add Mem0 only if you outgrow the file-based approach.

### 5.5 Context Engineering Discipline
Context ≠ memory: context is what the model sees *right now* (finite, per-session); memory is what persists across sessions. Practical rules:
- Audit context consumption with `/context` in Claude Code.
- Compact deliberately at milestones (`/compact keep the failing test output...`), not desperately at the context edge.
- Promote durable decisions to memory explicitly ("# remember: ...") rather than letting them die with the session.
- Use subagents for verbose exploration (log dives, broad searches) — only the conclusion returns to your main context.

---

## 6. Layer 5 — MCP: Connecting Claude Code to Your Tools

### 6.1 Recommended MCP Servers for Arvin's Stack
| MCP Server | Purpose |
|---|---|
| **Obsidian MCP server** | Direct read/write into the vault; local-first |
| **Desktop Commander** | Local file operations outside the vault |
| **GitHub MCP** | Manage your e-commerce/dashboard repos directly |
| **Filesystem / Fetch MCP** | General file + web fetch tool access |
| Custom finance MCP (build later) | Wrap your Robinhood/portfolio scripts as callable tools |

Setup pattern: install the server, point it at the vault path, register it in `~/.claude/settings.json` under `mcpServers`.

### 6.2 Keep the Tool Surface Tight
Every connected MCP server ships its schemas into context on every session — keep the per-project tool set minimal; prefer Anthropic's guidance of 3–5 always-loaded tools.

---

## 7. Layer 6 — Governance, Verification, Observability

- **Trace everything**: every retrieval, reflection, and tool call logged with timestamps, token counts, and decision rationale (OpenTelemetry-style).
- **Human-in-the-loop gates** on low-confidence or high-stakes outputs (e.g., before executing a trade thesis or a real purchase).
- **Maker-checker pattern**: a verifier subagent/skill checks the output before it's treated as done.
- **Reflection log**: after each significant task, write a short self-reflection back into memory so failures are not repeated.
- **Evaluation set**: build a small golden set of real past queries and score faithfulness/relevance whenever you change the pipeline.

---

## 8. Build Order for Claude Code (Sequenced Roadmap)

1. **Vault scaffold**: create the folder structure in §2.2, install core plugins, migrate any existing notes into `00_Inbox`.
2. **CLAUDE.md v1**: write the lean standing brief — under 150 lines.
3. **Obsidian MCP wiring**: connect Claude Code to the vault; verify read/write with a simple test note.
4. **Static hybrid RAG v1**: chunk + index the vault (Chroma + BM25), no agentic loop yet. Test on 10 real questions.
5. **Add reranker**: cross-encoder reranking on top of hybrid retrieval; re-test.
6. **Agentic escalation**: only for query classes where static RAG fails — add hop budget, confidence threshold, forced-answer fallback (§3.3).
7. **Subagents/skills**: split verbose recurring workflows into dedicated Claude Code skills invoked on demand.
8. **Memory layer**: enable auto-memory; add Mem0 only if cross-project persistence is needed.
9. **Governance**: add tracing/logging, a verifier skill, and a small golden eval set.
10. **Iterate**: revise CLAUDE.md whenever Claude repeats a mistake twice — that's a missing line, not a bigger prompt.

---

## 9. Key References

- Anthropic, *Building Effective AI Agents* — https://www.anthropic.com/engineering/building-effective-agents
- Anthropic, *Trustworthy agents in practice* — https://www.anthropic.com/research/trustworthy-agents
- Anthropic, *The new rules of context engineering for Claude* — https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models
- Claude Code Docs, *How Claude remembers your project* — https://code.claude.com/docs/en/memory
- IBM, *What Is Loop Engineering?* — https://www.ibm.com/think/topics/loop-engineering
- eesel.ai, *Loop engineering explained* — https://www.eesel.ai/blog/loop-engineering
- Future AGI, *Agentic RAG 2026: Patterns, Code, Observability* — https://futureagi.com/blog/agentic-rag-systems-2025/
- digitalapplied, *Agentic RAG Patterns 2026* — https://www.digitalapplied.com/blog/agentic-rag-patterns-multi-step-reasoning-guide
- iotdigitaltwinplm, *Agentic RAG Architecture* — https://iotdigitaltwinplm.com/agentic-rag-architecture-retrieval-agents-2026/
- Vector DB comparisons — https://jangwook.net/en/blog/en/vector-db-comparison-2026-qdrant-chroma-pgvector/, https://vucense.com/dev-corner/vector-databases-comparison-2026/
- LangGraph vs CrewAI vs OpenAI Agents SDK — https://particula.tech/blog/langgraph-vs-crewai-vs-openai-agents-sdk-2026, https://letsdatascience.com/blog/ai-agent-frameworks-compared
- Mem0 vs Letta — https://vectorize.io/articles/mem0-vs-letta
- Obsidian second-brain setup guides — https://ainotely.com/blog/obsidian-second-brain/, https://www.obsibrain.com/blog/obsidian-second-brain-template
- MCP servers for personal knowledge management — https://noverload.com/blog/best-mcp-servers-personal-knowledge-2026, https://mymcptools.com/blog/best-mcp-servers-for-knowledge-management
