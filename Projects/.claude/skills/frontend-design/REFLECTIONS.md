# Reflections — frontend-design

Append-only history of skill updates driven by real usage. See the `reflecting-on-skills` Skill for the process. Don't overwrite prior entries.

## 2026-08-20 — Default aesthetic direction reversed (soft/pastel → warm-neutral/confident)

**Observed**: Built a real test artifact (Financial Compass's Bank Statement Analyzer results screen, using representative data) with the skill's original soft/minimalist/pastel/bouncy default. Arvin's reaction: "i hate the design." No ambiguity, no partial credit.

Arvin then supplied a concrete reference: the actual computed CSS from `david-hckh.com` (colors, radius scale, spacing tokens, font name "Urbanist", GSAP-style easing curves, `Lenis` smooth-scroll). Rebuilt the same screen using those exact tokens — warm beige/cream ground, near-black text, Urbanist, small-radius scale, solid orange/cyan color-blocking, smooth (not bouncy) motion. Reaction: "i like this better."

One bug surfaced mid-test: the rebuild's improvised `prefers-color-scheme: dark` variant (guessed muted-brown-dark, since the reference site's dark-mode tokens weren't given) auto-triggered because Arvin's system is in dark mode, and he initially reacted to *that* ("i don't like the dark background, especially brown") before the actual light design had been seen. Fixed by dropping the dark-mode block entirely — matches the reference site's own behavior (stays light regardless of OS theme, likely a manual toggle only) and Arvin's confirmed preference.

**Confidence**: High — two real builds, one clear rejection, one clear confirmation, plus a specific named reference site with exact tokens (not inferred).

**Applied**: Yes. Full rewrite of `SKILL.md`'s Design Thinking and Frontend Aesthetics Guidelines sections to the warm-neutral/confident/editorial direction, replacing every reference to soft/pastel/cute/blobby/bouncy. Also added an explicit "commit to light, don't auto-invert to dark" rule, since the dark-mode bug is a real, repeatable failure mode worth guarding against directly, not just a one-off mistake.

**Downstream**: `casual-human-voice`'s Design System section (repo-root, cross-cutting) updated the same day to point to this confirmed direction instead of its prior neutral/info-dense placeholder guess.
