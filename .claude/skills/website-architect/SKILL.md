---
name: website-architect
description: Use this skill whenever a change to the Sambrano Roofing & Exteriors LLC website (the site-2 project) touches how the page is structured or organized rather than just how one thing looks or reads — adding or reordering sections, changing navigation, reorganizing assets, or any request that could ripple across multiple parts of the page. Trigger this BEFORE any change that sounds bigger than a single line, even if the user didn't say "architecture" or "structure." Also consult it when a request seems to call for a bigger rebuild than necessary, so you can find the smallest structural change that actually solves it.
---

# Website Architect

You understand how this specific website is put together, and your job is to keep changes consistent with that structure instead of letting the codebase drift toward accidental complexity.

## Why this matters

This is a small, single-file static site. Its simplicity is a feature, not a gap — Erick's CLAUDE.md explicitly says not to introduce a framework, bundler, or dependency unnecessarily, and to preserve existing working functionality unless a change is explicitly requested. Every architectural decision here should ask "what's the smallest change that fits the existing structure?" before asking "what would be cleanest to build from scratch?"

## The current architecture (know this before touching anything)

- **One file, `index.html`**, containing everything: a `<style>` block with all CSS, the full page markup, and a `<script>` block with all JS. There is no build step, no framework, no separate CSS/JS files.
- **`assets/`** holds all media, organized by type:
  - `assets/logo/` — `logo-white.png` (primary, used on dark sections), `logo-color.png` (prepared for future light sections via the `.theme-light` class, not currently used anywhere), `favicon.png`
  - `assets/video/` — `hero.mp4`, `hero.webm`, `poster.jpg` (hero background video + fallback)
  - `assets/images/projects/` — `project-1.jpg` through `project-6.jpg`
- **Sections, in document order**: nav → hero (`#top`) → about (`#about`) → services (`#services`) → projects (`#projects`) → project gallery modal (hidden, JS-driven) → service areas (`#areas`) → process (`#process`) → contact (`#contact`) → footer.
- **Navigation is anchor-based.** `.nav-link` hrefs (`#about`, `#services`, etc.) must match section `id` attributes exactly. If you ever rename or remove a section id, the nav (and any other internal link to it, including footer links) breaks silently — no build step will catch this for you.
- **The project gallery is data-driven.** Each `.project-card` has a `data-project="N"` attribute; the JS `PROJECTS` object (keyed `'1'`–`'6'`) supplies the modal's category/title/description/images for that card. These two things — the `data-project` value and the `PROJECTS` key — must stay in sync.
- **Responsive breakpoints**: `900px` (desktop → mobile nav, grids collapse to 1 column) and `560px` (hero-specific tightening). A layout change that isn't checked at both is unfinished — see the `visual-qa` skill.
- **Reveal-on-scroll** (`.reveal` class + `IntersectionObserver`) and the hero fade-in are self-contained JS patterns that don't depend on section order, only on the elements existing in the DOM with the right class names.

## Responsibilities

- Understand how sections, navigation, and assets depend on each other before changing any one of them.
- Identify architectural risk *before* editing — e.g., "renaming this section id will break these 3 nav links and this footer link."
- Recommend the smallest safe structural change. If a request could be solved with a CSS/content tweak instead of moving markup around, prefer that.
- Prevent unnecessary rewrites — a working section should not be restructured just because a different structure would be marginally cleaner.
- Keep content, presentation (CSS), and behavior (JS) reasonably separated *within* the existing single-file convention — don't invent a build step to achieve this.

## Before major architectural changes

1. Inspect the existing implementation (read the relevant part of `index.html` — don't assume from memory).
2. Explain what will change, in plain terms.
3. Identify every file affected (in practice, this almost always means "which parts of `index.html`").
4. Identify potential regressions — what else references the thing you're about to change (nav links, footer links, JS selectors, the `PROJECTS` object, CSS class names shared across components).
5. Prefer incremental changes over wholesale restructuring.

## The hard rule

**Never redesign the site merely because a different architecture would be technically cleaner.** The existing site is the baseline. A change is justified by an actual, stated need — not by aesthetic preference for a different way of organizing the same functionality. If you think a structural change would genuinely help, say so and explain why, then wait for Erick to decide — per CLAUDE.md, he's the final decision-maker on major layout/navigation/architecture changes.

For the cross-cutting rules that apply to every skill (approval gates, reporting format, production safety), see `.claude/skills/WORKFLOW.md`.
