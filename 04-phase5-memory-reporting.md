# Phase 5 — Cross-Tool Memory & Reporting (Shared Domain, Carefully)

## Goal
Give yourself a single command-center view across Personal, Business, and Finance — without merging the underlying sensitive data. This is the one phase that deliberately touches multiple domains, so it needs the most explicit isolation discipline.

## 1. The "Shared" Domain Pattern
- Each domain (Personal, Business, Finance) runs a scheduled summarization task that distills its own week into an **abstracted, non-sensitive digest** (e.g., "Kanban: 12 tasks closed, 3 overdue"; "Finance: portfolio up 2.1% this week"; "Marketing: 2 posts published, engagement +15%").
- Only these digests — not raw records — get written into `Shared/50_Daily/weekly-digest`.
- A separate, low-privilege "digest agent" is the only thing with read access to all three domains' digest outputs; it never has access to raw domain vaults.

## 2. Weekly Digest Automation
- n8n (or a Claude Code skill run on a schedule) pulls each domain's digest note and compiles one combined weekly report.
- This report is for your eyes — a dashboard/summary — not a new store of raw sensitive data. Keep it high-level by design.

## 3. Cross-Domain Questions (Manual, Not Automatic)
- If you want to ask something like "did the marketing push affect my portfolio review time," that's a manual, explicit query you run yourself, pulling from whichever domains you choose to open in that session — never a standing automated merge.

## 4. Build Order
1. Build the per-domain digest generator first (Personal, Business, Finance each produce their own abstracted summary).
2. Verify each digest genuinely excludes sensitive specifics — spot-check for account numbers, exact figures, supplier names, etc.
3. Build the Shared weekly compiler that only reads digests, never raw vaults.
4. Add a review step where you personally confirm no sensitive detail leaked into a digest before it's saved to Shared.
5. Set the automation schedule (e.g., every Sunday night) once you trust the digest quality.

## 5. Verification Checklist
- Digest notes contain no account numbers, exact dollar figures tied to identifiable accounts, supplier names, or personal health/family details — only trends and abstracted metrics.
- The Shared digest compiler has zero MCP access to raw domain vaults — only to the digest notes.
- Periodically run a canary test: insert a fake sensitive detail into a domain, generate the digest, and confirm it does not appear in Shared.
