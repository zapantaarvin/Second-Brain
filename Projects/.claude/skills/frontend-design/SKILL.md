---
name: frontend-design
description: Create distinctive, production-grade frontend interfaces with high design quality, defaulting to a soft, minimalist, and approachable aesthetic. Use this skill when the user asks to build web components, pages, artifacts, dashboards, or applications (websites, landing pages, dashboards, React components, HTML/CSS layouts, or styling/beautifying any web UI). Generates creative, polished code that avoids generic AI aesthetics while staying warm, gentle, and inviting rather than cold or corporate.
license: Complete terms in LICENSE.txt
---

> Adapted 2026-08-20 by Arvin from `davila7/claude-code-templates`'s `creative-design/frontend-design` skill (Apache 2.0, upstream unmodified copy of `LICENSE.txt` kept alongside this file). Original let the aesthetic swing between many "BOLD" extremes (brutalist, maximalist, luxury, playful, retro, etc.) chosen per-project; this version pins the default to soft/minimalist/pastel, deviating only when a request clearly calls for something else. Installed scoped to `Projects/` only (nested skill — loads when Claude works inside this category, not repo-wide) since the aesthetic fit for Arvin's taste isn't confirmed yet; promote to a cross-cutting skill if it proves right.

This skill guides creation of distinctive, production-grade frontend interfaces that avoid generic "AI slop" aesthetics. Implement real working code with exceptional attention to aesthetic detail — soft, uncluttered, and quietly charming rather than loud or maximalist.

The user provides frontend requirements: a component, page, application, or interface to build. They may include context about the purpose, audience, or technical constraints.

## Design Thinking

Before coding, understand the context and commit to a default aesthetic lane, deviating only when the user's request clearly calls for something else:
- **Default direction**: Minimalist and soft — generous whitespace, gentle curves, low-contrast layering, and a sense of calm. The interface should feel welcoming and unintimidating, like it's designed to put someone at ease, not impress them with density or power.
- **Purpose**: What problem does this interface solve? Who uses it — is this a personal tool, a small business page, or something public-facing?
- **Tone**: Soft, cute, and accepting. Rounded shapes over sharp edges. Friendly micro-copy over terse labels. Approachable imperfection (hand-drawn accents, blobby shapes, gentle wobble in illustrations) over sterile precision. Nothing should feel cold, clinical, or corporate.
- **Constraints**: Technical requirements (framework, performance, accessibility). Prefer React/Next.js + Tailwind conventions when unspecified.
- **Differentiation**: What makes this UNFORGETTABLE while staying soft? One charming signature detail — a custom cursor, a playful hover wiggle, a hand-lettered accent font, a soft illustrated mascot or motif — rather than visual noise.

**CRITICAL**: Choose a clear conceptual direction and execute it with precision. Minimalist and soft does not mean plain — restraint should feel intentional and cared-for, with small delightful details doing the work instead of scale or intensity.

Then implement working code (HTML/CSS/JS, React, Vue, etc.) that is:
- Production-grade and functional
- Visually gentle, warm, and memorable
- Cohesive with a clear soft, minimalist point-of-view
- Meticulously refined in spacing, softness of shapes, and small charming touches

## Frontend Aesthetics Guidelines

Focus on:
- **Typography**: Avoid generic fonts (Arial, Roboto, system-ui) and overused "safe" choices (Inter, Space Grotesk). Prefer rounded, friendly sans-serifs (e.g., Quicksand, Nunito, Fredoka, Comfortaa) or a soft handwritten/script accent font for headers, paired with a clean, highly legible rounded body font. Favor slightly larger line-height and letter-spacing for an airy, gentle feel.
- **Color & Theme**: Default to light backgrounds in warm off-whites or the palest pastels (blush, sage, butter yellow, powder blue, lavender) with one or two soft accent colors — never harsh saturation. Avoid cold grays, neon accents, and purple gradients on white. Use CSS variables for consistency. Dark themes should be avoided unless explicitly requested; if used, keep them soft (muted charcoal, not pure black) with pastel accents.
- **Motion**: CSS-only for HTML, Motion library for React when available. Favor gentle, bouncy easing (soft spring curves, not linear or sharp) on hover states and page load — a soft fade-and-rise stagger on load, a little wiggle or bounce on buttons, a gentle scale on hover. Motion should feel like a warm nudge, never jarring or fast.
- **Spatial Composition**: Generous whitespace, centered and breathable layouts, soft asymmetry (gentle offsets, not aggressive diagonal grid-breaking). Rounded containers, pill-shaped buttons, soft card shapes with large border-radius. Avoid dense grids or sharp-edged panels.
- **Backgrounds & Visual Details**: Create atmosphere through softness — blurred pastel blobs, subtle grain, soft drop shadows (diffuse, colored, not harsh black), gentle gradient washes, rounded organic shapes, small illustrated doodles or icons. Avoid geometric patterns, sharp borders, or anything that reads as "techy" or "corporate."

NEVER use generic AI-generated aesthetics like overused font families (Inter, Roboto, Arial, system fonts), cliched color schemes (particularly purple gradients on white backgrounds), predictable layouts and component patterns, and cookie-cutter design that lacks context-specific character. Also avoid defaulting to dark, technical, brutalist, or corporate registers — those work against the soft, accepting feel this skill targets unless the user explicitly asks for a different context.

Interpret creatively within the soft/minimalist/cute register described above. Vary specific pastel palettes, accent fonts, and signature charming details across generations so outputs don't feel templated, but keep the gentle, uncluttered, welcoming foundation consistent.

**IMPORTANT**: Match implementation complexity to the aesthetic vision. Soft minimalism needs restraint and precision in spacing and easing curves — the charm comes from a handful of well-placed, well-animated details, not from adding more elements. Resist the urge to over-decorate; one well-executed soft blob background and one bouncy button interaction beats five competing effects.

Remember: the goal is an interface that feels like a warm hug — calm, uncluttered, a little playful, and genuinely welcoming to anyone who lands on it. Don't hold back on charm and polish, but let softness and restraint carry the design rather than intensity.
