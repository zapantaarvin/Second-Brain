---
type: project
repo: https://github.com/zapantaarvin/kanban-business-hub
status: active
imported: 2026-08-16
---

# Kanban Business Hub

A Next.js + TypeScript + Tailwind kanban app for managing multiple businesses — separate boards per business, team access per business, task tracking with attachments/links/comments. Built per its own in-repo `CLAUDE.md` spec (Next.js, Supabase for auth/DB/realtime, Vercel deployment).

**Confirmed (per Arvin, 2026-08-16): this is the main repo** — the same effort as the `Kanban.md Reusable Prompt` project in `~/Documents/Business-Partnership/Kanban/`, and Arvin's actual organizational/planning/tracking tool for all business ideas and tasks going forward, not just the ones currently seeded.

**Operating model implication:** this app is where task-by-task execution lives for Arvin's businesses. This vault (Second-Brain) holds research, knowledge, and reference material; Kanban Business Hub holds live task tracking. Don't try to rebuild task-tracking structure inside the vault — link out to the relevant business/board here instead.

Confirmed tracking [[Zapee]] task-by-task — the app now displays "Zapee" (per Arvin, 2026-08-16), matching the team's rename. The seed file (`tuggo_ecommerce_seed.sql`) still has the old name in its filename, but that's just a historical artifact now.

## Businesses currently seeded in the app (per `database/*_seed.sql`, checked 2026-08-16)
- **Zapee** (renamed from "Tug-go Ecommerce" — confirmed live in the app, 2026-08-16) — see [[Zapee]]
- **"Pilipinas Fuel"** (placeholder name) → this is actually **[[Protein Bar Business (FFN)|the protein bar business]]**, per Arvin — real brand not finalized yet

## Repo structure (as of import)
`app/`, `components/`, `database/`, `lib/`, `types/`, plus its own `CLAUDE.md` and `AGENTS.md`.

## Status
**Current build phase: Done** (per Arvin, 2026-08-16) — no longer just "somewhat operational," the phase itself is complete. Actively tracking at least two real businesses ([[Zapee]], protein bar). Which phase exactly (per the repo's own `CLAUDE.md` roadmap) isn't specified here — worth noting the phase number/name next time this is checked, in case there's a Phase N+1 still ahead.

## Links
- Repo: https://github.com/zapantaarvin/kanban-business-hub (private)
