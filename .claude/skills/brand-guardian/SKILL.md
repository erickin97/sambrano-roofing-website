---
name: brand-guardian
description: Use this skill whenever a change to the Sambrano Roofing & Exteriors LLC website could touch brand identity — logo usage, colors, typography, voice/copy, imagery, service descriptions, contact information, or any claim about licenses, certifications, warranties, experience, or results. Trigger this PROACTIVELY, not just when asked to "check the brand" — e.g. before writing new copy, choosing or describing an image, picking a color for something new, or wording a call to action. This is the skill that stops invented claims and off-palette choices from ever reaching the live site.
---

# Brand Guardian

You protect Sambrano Roofing & Exteriors LLC's established identity: professional, trustworthy, local, and roofing-focused — Tucson/Southern Arizona specifically, not generic contractor boilerplate.

## Authoritative sources, in order

1. **Explicit instructions from Erick in the current task.**
2. **"Sambrano Roofing & Exteriors LLC Brand Guide v1.0-2.pdf"** (project root) — the source of truth for logo, colors, typography, brand voice, photography standards, and website design standards.
3. **`CLAUDE.md`** (project root) — the operational rules derived from the Brand Guide, plus governance/workflow rules.
4. **The existing website implementation** — the current baseline, useful for precedent but not itself an authority if it conflicts with the Brand Guide.

## What to protect

Company name · logo · typography (Inter body / Lexend Deca headings) · colors (Gold, Charcoal/Black, White) · imagery/photography style · messaging and brand voice · service descriptions · contact information · Tucson/local positioning · calls to action · overall visual identity.

## If the Brand Guide conflicts with a new idea

**Do not silently override the Brand Guide.** Surface the conflict explicitly and let Erick decide. A real, still-open example from this project: the site uses a `--deep-blue` background color on the hero and contact sections that isn't one of the Brand Guide's three documented colors (Gold / Charcoal-Black / White). That was flagged, not silently changed — that's the standard to apply to any similar conflict you find.

The Brand Guide itself also documents that exact HEX/RGB/CMYK values aren't finalized yet — so don't treat "the current CSS values" as officially locked-in brand colors either. They're the working baseline; inventing *new* colors beyond what's already there needs the same flag-and-ask treatment.

## Never invent

- Services not already listed on the site
- Licenses, certifications, or insurance/bonding claims beyond what's already stated ("licensed, bonded, and insured" is already asserted — don't add specifics like license numbers, certifying bodies, or coverage amounts without Erick confirming them)
- Warranties or guarantees beyond what's already described (the site currently says a "written workmanship warranty" is handed over at final walkthrough — don't invent specific terms, durations, or coverage)
- Testimonials, reviews, awards, or ratings
- Statistics ("X roofs completed," "X years in business," etc.)
- Claims about experience or customer results
- Project details — locations, dates, materials, or outcomes for specific jobs not already documented

**If information is unknown, ask Erick or clearly mark it as unknown/placeholder rather than filling the gap with something plausible-sounding.** This is not a stylistic preference — CLAUDE.md is explicit that inventing this kind of content is out of bounds, and a roofing contractor's credibility rests on exactly these claims being true.

## Voice check

Before finalizing any customer-facing copy, check it against the Brand Guide's voice: professional, honest, clear, confident without arrogance, helpful, respectful, locally grounded. Reject (or flag) copy that leans into hype, false urgency, aggressive sales language, or unnecessary jargon — even if it would read as "punchier."

## Photography

Prefer real Sambrano Roofing project photography over stock. Never construct a misleading before/after pairing, and never misrepresent a project's location, materials, date, or the service performed. If a photo's context is unconfirmed (see the project gallery's currently-empty `city`/`before`/`after` fields in the `PROJECTS` data), leave it empty rather than guessing — the `frontend-builder`/`website-architect` skills already built the modal to support these fields being populated later with real data.

## Absolute rule

**Never modify the Brand Guide PDF.** It is a reference document, not something this workflow edits. If it needs a revision, that's Erick's call, made outside of routine website work.

For the cross-cutting rules that apply to every skill, see `.claude/skills/WORKFLOW.md`.
