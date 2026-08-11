---
name: visual-qa
description: Use this skill BEFORE declaring any visual or layout change to the Sambrano Roofing & Exteriors LLC website complete — after implementing, before reporting "done" to Erick. Trigger for responsive/mobile work, new sections, CSS changes, or anything visual, even if not explicitly asked to "test" it. This skill is what actually looks at the rendered page instead of assuming that code which reads correctly renders correctly.
---

# Visual QA

Writing CSS that looks correct on the page and confirming the page actually looks correct are two different steps. This skill is the second one, and it doesn't get skipped just because the first one went smoothly.

## Why this is its own skill and not just a step in frontend-builder

On this project, a real mobile bug (the nav bar's CTA button overlapping the wrapped logo text at narrow widths) was only caught by actually rendering the page at 375px and looking — it wasn't visible from reading the CSS. Static reasoning about flexbox and media queries is a reasonable first pass, but it has already been proven insufficient on its own for this exact site.

## What to inspect

- Desktop layout
- Tablet-width layout
- Mobile layout — and specifically narrow phones (~375px), not just "mobile-ish"
- Navigation (including the hamburger menu open/close state on mobile)
- Buttons and CTAs (including hover and keyboard focus states)
- Forms (field layout, labels, focus states)
- Images (do they actually load and display, or show as broken?)
- Video (does it play, does the poster/fallback work if it doesn't?)
- Spacing and alignment
- Typography (correct font, size, weight at each breakpoint)
- Contrast (can the text actually be read against its background?)
- Accessibility (focus indicators visible, alt text present, heading order sane)
- Console errors, when the browser tooling is available
- Obvious layout regressions elsewhere on the page — a fix in one section can shift something unrelated

## For every change

Compare the result against what was actually intended — not against "does it look plausible." If the task was to fix a specific bug, confirm that specific bug is gone, at the specific breakpoint(s) it occurred at, without assuming a fix at one width holds at another.

**Do not introduce unrelated visual changes while checking.** QA is where you notice things — it is not license to fix them on the spot without telling Erick first, per `safe-editor`'s rules.

## Use the actual browser tooling

If a screenshot or live browser preview is available, use it — don't rely on reading CSS alone. That said, be aware of a real quirk observed on this project: this environment's preview pane can occasionally return a stale or blank screenshot for local `file://` pages (particularly right after a `navigate` or JS execution). When a screenshot looks wrong in a way that doesn't match what the DOM should show, cross-check with `get_page_text`, `read_page` (accessibility tree), or a direct `getComputedStyle`/`matches()` check via the JS execution tool before concluding the page itself is broken — confirm it's a real rendering problem, not a stale capture, before reporting it as one.

## When something looks wrong, identify which kind of problem it is

- **Implementation problem** — the code doesn't do what was intended
- **Asset problem** — hand off to `media-debugger`
- **Responsive problem** — works at one breakpoint, breaks at another
- **Browser problem** — a quirk specific to one browser/engine, not a defect in the code
- **Deployment problem** — looks fine locally but wrong live; hand off to `deployment-verifier`

**Do not guess.** If it's unclear which category a problem falls into, say so and investigate further rather than picking the most convenient explanation.

For the cross-cutting rules that apply to every skill, see `.claude/skills/WORKFLOW.md`.
