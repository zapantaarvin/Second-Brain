---
name: frontend-design
description: Create distinctive, production-grade frontend interfaces with high design quality, defaulting to a warm-neutral, confident, editorial aesthetic (warm beige/cream ground, near-black text, geometric sans, restrained radius, solid color-blocked accents). Use this skill when the user asks to build web components, pages, artifacts, dashboards, or applications (websites, landing pages, dashboards, React components, HTML/CSS layouts, or styling/beautifying any web UI). Generates creative, polished code that avoids generic AI aesthetics.
license: Complete terms in LICENSE.txt
---

> Adapted 2026-08-20 by Arvin from `davila7/claude-code-templates`'s `creative-design/frontend-design` skill (Apache 2.0, upstream unmodified copy of `LICENSE.txt` kept alongside this file). **Revised 2026-08-20 via `reflecting-on-skills`** after a real A/B test (see `REFLECTIONS.md`): the original soft/pastel/cute default was built, shown against Financial Compass's Bank Statement Analyzer, and explicitly rejected ("I hate the design"). Arvin then supplied a concrete reference (`david-hckh.com`'s actual computed CSS) and confirmed the rebuilt version ("i like this better"). This file now encodes that confirmed direction, not a guess.

This skill guides creation of distinctive, production-grade frontend interfaces that avoid generic "AI slop" aesthetics. Implement real working code with exceptional attention to aesthetic detail — confident, warm, and precise rather than soft or decorative.

The user provides frontend requirements: a component, page, application, or interface to build. They may include context about the purpose, audience, or technical constraints.

## Design Thinking

Before coding, understand the context and commit to the default aesthetic lane below, deviating only when the user's request clearly calls for something else:
- **Default direction**: Warm-neutral and confident — a cream/beige ground, near-black text, generous but *not* airy spacing, small-to-moderate radii (not blobby), solid color-blocked accents rather than pastel washes. The interface should feel considered and premium, like an editorial/portfolio site, not soft or twee.
- **Purpose**: What problem does this interface solve? Who uses it — is this a personal tool, a small business page, or something public-facing?
- **Tone**: Confident, precise, warm-but-not-cute. Tight type (line-height ~1.1 for titles, ~1.3 for body), a touch of letter-spacing (~0.02em), bold large headlines. Friendly through warmth of palette, not through roundness or whimsy.
- **Constraints**: Technical requirements (framework, performance, accessibility). Prefer React/Next.js + Tailwind conventions when unspecified.
- **Differentiation**: What makes this UNFORGETTABLE while staying warm and precise? One confident signature move — a bold oversized headline, a strong accent color used sparingly, a well-orchestrated load sequence — rather than decorative flourish.

**CRITICAL**: Choose a clear conceptual direction and execute it with precision. Warmth here comes from color and confident type, not from softness, roundness, or decoration.

Then implement working code (HTML/CSS/JS, React, Vue, etc.) that is:
- Production-grade and functional
- Visually confident, warm, and precise
- Cohesive with a clear editorial point-of-view
- Meticulously refined in spacing, type scale, and restraint

## Frontend Aesthetics Guidelines

Focus on:
- **Typography**: Avoid generic fonts (Arial, Roboto, Inter, Space Grotesk, system-ui) *and* avoid rounded/playful faces (Quicksand, Nunito, Fredoka, Comfortaa) — the confirmed direction is a confident geometric sans like **Urbanist** (or similar: Sora, General Sans). Tight line-height (1.1 titles / 1.3 body), slight letter-spacing (~0.02em), large confident headline scale (don't undersize titles).
- **Color & Theme**: Warm beige/cream ground (`#f5efe6`-family), near-black warm text (`#2d2a24`-family), muted warm-brown secondary text — **not** pastel tints. One or two solid, saturated accent colors used deliberately (e.g. a vivid orange + a cyan/blue), applied as solid color-blocking (fills, badges, bars) rather than soft tinted backgrounds. Use CSS variables for consistency.
- **Dark mode — commit to light, don't auto-invert.** Arvin's confirmed preference is the light warm-neutral ground; a previous draft's improvised `prefers-color-scheme: dark` variant (guessed muted-brown-dark) was explicitly disliked before he'd even reacted to the actual design, purely because it auto-triggered on his system's dark mode. **Default to a single committed light theme — no `@media (prefers-color-scheme: dark)` block, no `[data-theme="dark"]` block** — unless a specific request explicitly asks for dark mode support. This overrides the general artifact-design guidance to always build both themes; here, explicit confirmed preference wins.
- **Motion**: CSS-only for HTML, Motion library for React when available. Smooth, engineered easing — `cubic-bezier(.25,.46,.45,.94)` ("power2-out") or similar — **not** bouncy/spring/wiggle. A staggered fade-and-rise on load (elements arriving in sequence, ~400-600ms, modest delay offsets) reads as considered; avoid playful overshoot or rotation on hover, prefer subtle translate/opacity shifts.
- **Spatial Composition**: Restrained radius scale (roughly 6/12/16/24px — small to moderate, never pill-everything or blob-everything). Grid-based, confident layouts; generous but purposeful whitespace, not maximal airiness. Thin 1px borders/dividers over heavy shadows.
- **Backgrounds & Visual Details**: Minimal decoration — solid color blocks, thin rule lines, tabular numerals for data — over blobs, grain, gradient washes, or illustrated doodles. If a background accent is used at all, keep it geometric and restrained, not organic/blobby.

NEVER use generic AI-generated aesthetics like overused font families (Inter, Roboto, Arial, system fonts), cliched color schemes (particularly purple gradients on white backgrounds), predictable layouts and component patterns, and cookie-cutter design that lacks context-specific character. Also avoid the soft/pastel/cute/blobby register this skill used to default to — it was built and explicitly rejected (see `REFLECTIONS.md`) — unless the user's request specifically calls for that register.

Interpret creatively within the warm-neutral/confident/editorial register described above. Vary specific accent-color pairings, exact type choices, and layout rhythm across generations so outputs don't feel templated, but keep the warm-ground, confident-type, restrained-decoration foundation consistent.

**IMPORTANT**: Match implementation complexity to the aesthetic vision. This register needs precision in type scale, spacing rhythm, and restraint — the impact comes from confident execution of a few well-chosen elements (one bold headline, one accent color used well, one orchestrated load sequence), not from adding more elements.

Remember: the goal is an interface that feels considered, warm, and premium — like a well-crafted portfolio or editorial site, not a soft toy and not a corporate dashboard.
