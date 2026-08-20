---
name: design-token-guardian
description: Use when Arvin asks to check a page/component/artifact against the design system, says "does this match our identity," or after any UI work in Projects/ to self-check before presenting it. Verifies colors, fonts, radius, and motion against frontend-design's confirmed tokens and flags drift.
---

# Design Token Guardian

## Purpose
`frontend-design`'s warm-neutral/confident direction was A/B tested and explicitly confirmed (2026-08-20, see its `REFLECTIONS.md`) — not a guess anymore. This skill exists so future UI work (mine or anything else that touches these apps) can't silently drift back toward a generic or previously-rejected look without anyone noticing.

## Source of truth
**Read `Projects/.claude/skills/frontend-design/SKILL.md` at check-time — don't hardcode the token values here.** If that file's tokens ever get revised (e.g. via `reflecting-on-skills`), this skill should automatically check against the current version, not a stale copy. Duplicating the values here would recreate exactly the kind of drift this skill is supposed to prevent.

## What to check
Given a piece of UI code, an artifact, or a live page:
- **Color**: background/text/accent hex values match the confirmed palette (warm beige/cream ground, near-black text, orange + cyan accents) — flag any hardcoded color outside it, especially pastel tints or purple gradients.
- **Typography**: font-family matches the confirmed direction (Urbanist or an explicitly-approved substitute) — flag generic fonts (Arial, Roboto, system-ui, Inter) and flag rounded/playful fonts (Quicksand, Nunito, Fredoka) specifically, since that's the exact register that was tested and rejected.
- **Radius**: border-radius values fall in the confirmed restrained range (roughly 6-24px) — flag pill-shaped buttons, blob shapes, or anything using `border-radius: 9999px`/`50%` outside of genuinely circular elements (avatars, icon badges).
- **Motion**: easing functions are smooth/engineered (power2-style cubic-beziers), not bouncy/spring/overshoot curves.
- **Dark mode**: flag any `@media (prefers-color-scheme: dark)` or `[data-theme="dark"]` block that wasn't explicitly requested — `frontend-design` now says commit to light-only by default, and an unrequested dark variant already caused one real false-negative reaction (see `frontend-design/REFLECTIONS.md`).

## Output
A short list: what matches, what doesn't, with the specific line/value and what it should be instead. If everything matches, say so briefly — don't pad a clean check with commentary.

## When it doesn't apply
Anything outside `Projects/` (this skill is nested, only loads there) or explicitly non-default work (a client with their own brand kit, an intentional one-off style). Not a universal linter — a check against *this specific confirmed identity*.
