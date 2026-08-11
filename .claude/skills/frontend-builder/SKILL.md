---
name: frontend-builder
description: Use this skill for hands-on HTML/CSS/JS implementation work on the Sambrano Roofing & Exteriors LLC website (site-2) — building or modifying markup, styles, responsive layouts, forms, buttons, animations, or accessibility markup inside index.html. Trigger for any "add/change/fix this on the page" request, especially anything touching mobile vs. desktop layout, a new section, or a component's styling. Always follow up with the visual-qa skill after implementing — writing correct-looking code is not the same as confirming it renders correctly.
---

# Frontend Builder

You implement changes to `index.html` — the single file holding all HTML, CSS, and JS for this site — while keeping the established visual identity intact.

## Why "preserve, don't rebuild" matters here

Every visual decision already on this site (the dark charcoal/gold palette, Inter + Lexend Deca typography, the card/button system) traces back to the Brand Guide and reflects real work already reviewed and approved. A frontend change that quietly drifts from that — a slightly different shade, a new button style, an extra font weight — isn't a neutral implementation detail, it's a brand inconsistency. Reusing what's already there isn't just less work, it's the thing that keeps the site looking like one coherent product instead of a patchwork of one-off decisions.

## Rules

1. **Preserve the existing design unless explicitly instructed otherwise.** If a task doesn't ask for a visual change, don't make one as a side effect.
2. **Do not arbitrarily change colors.** The palette lives in `:root` custom properties (`--accent-gold`, `--charcoal`, `--white`, etc.) — reuse them. Introducing a new color value is a brand-guardian-level decision, not a routine implementation choice.
3. **Do not arbitrarily change typography.** `--font-sans` (Inter) and `--font-heading` (Lexend Deca) are the only two fonts on the site. Don't add a third or change weights/sizes outside what's needed for the specific task.
4. **Do not arbitrarily change spacing.** The section rhythm (`8rem` desktop / `5rem` mobile padding) and component spacing patterns are consistent on purpose — match them rather than inventing new values.
5. **Do not remove functionality without permission.** If implementing a request would mean deleting or disabling something that currently works, flag that explicitly before doing it.
6. **Do not replace working components simply because another implementation is preferred.** A component that works and matches the design system doesn't need to be rebuilt to use a "better" pattern.
7. **Reuse existing components and styles whenever possible.** Before writing new CSS, check whether `.btn`, `.card`-style patterns (`.metric-card`, `.service-card`, `.project-card`), or an existing class already does what's needed.
8. **Make the smallest change that solves the requested problem.** This is the same discipline the `safe-editor` skill enforces at the file level — apply it at the code level too: a targeted CSS rule addition beats a broad rewrite of a working block.

## What this covers

HTML structure, CSS (all inline in the `<style>` block), vanilla JS (all inline in the `<script>` block), responsive layout across breakpoints, navigation, forms, buttons, animations/transitions, section markup, accessibility attributes (alt text, ARIA roles, focus states, semantic headings), and keeping implementations performance-conscious (e.g., preferring `transform`/`opacity` for animation, `loading="lazy"` for below-the-fold images — both patterns already used on this site).

## Responsive work is not optional

For any layout-affecting change, structurally account for:
- Desktop (roughly ≥900px, the point where nav switches from full links to a hamburger)
- Tablet-ish widths between the two breakpoints
- Mobile (≤900px, and specifically check narrow phones around 375px — this site has had a real, confirmed mobile nav-overlap bug before, so narrow widths are not a theoretical edge case here)

**Do not declare responsive work complete without actually inspecting the affected layout** — mentally tracing the CSS is a starting point, not a substitute for looking at the rendered result. Hand off to the `visual-qa` skill (or use the browser preview tools directly) before calling implementation done.

## Before you start

Read the section of `index.html` you're about to touch — CSS rules for a component are often not adjacent to its HTML, so search for the class name across the whole file first. Check `website-architect`'s notes on what depends on what (nav anchors ↔ section ids, `data-project` ↔ `PROJECTS` keys) so a "small" change doesn't quietly break something else.

For the cross-cutting rules that apply to every skill, see `.claude/skills/WORKFLOW.md`.
