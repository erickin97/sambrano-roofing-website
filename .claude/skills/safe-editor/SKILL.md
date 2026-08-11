---
name: safe-editor
description: This is the baseline editing discipline for EVERY change to the Sambrano Roofing & Exteriors LLC website's files — apply it on all edits, not just risky-looking ones. It enforces inspect-first, smallest-safe-change, and the absolute protections around CLAUDE.md, the Brand Guide PDF, and other protected assets. Treat this as the default operating mode whenever touching index.html, assets/, or any project file — it's what prevents unnecessary rewrites and destructive changes before they happen, not damage control after.
---

# Safe Editor

This is the discipline layer underneath every other skill in this system. `website-architect` decides *what* should change structurally, `frontend-builder` decides *how* to implement it, `brand-guardian` decides *whether* it's on-brand — this skill governs *how carefully* any of that actually gets written to disk.

## Core rule

**Inspect first. Plan second. Edit third. Verify fourth.**

In that order, every time. Skipping straight to editing because a fix "seems obvious" is exactly how a plausible-looking wrong fix gets made — this project has already had a real case of that risk (see `media-debugger`'s note on the assets/-missing-from-deploy incident: the obvious-looking fix would have been to edit `index.html`, which was never the problem).

## Before modifying a file

1. Read it — the actual current content, not a remembered or assumed version.
2. Understand its role in the site.
3. Identify the exact reason it needs modification — state the specific problem, not a general sense that "this could be improved."
4. Determine whether the requested result can be achieved *without* touching this file at all.
5. Make the smallest safe modification that achieves the stated goal.

**Never perform a wholesale rewrite when a targeted edit is sufficient.** If a change touches 3 lines, the diff should be 3 lines, not a regenerated section.

## Never, without explicit authorization from Erick for that specific change

- Modify `CLAUDE.md`
- Modify the Brand Guide PDF
- Modify or replace protected brand assets (the logo files, primarily)
- Delete any file

"The user asked me to fix X" does not itself authorize touching these — if fixing X seems to require it, stop and ask, explaining why.

## Before major changes, report

- **Files affected** — every file, not just the primary one
- **Reason** — what specific problem this solves
- **Expected result** — what should be true after the change that isn't true now
- **Potential risks** — what could break, what depends on the thing being changed (cross-reference `website-architect`'s notes on dependencies between nav/sections/data)

## After modification

- Inspect the actual diff — confirm it matches what was intended, nothing more
- Verify syntax (valid HTML/CSS/JS — an unclosed tag or mismatched brace in this single-file site can break far more than the section you touched)
- Verify the specific functionality the change targeted actually works (hand off to `visual-qa` for anything visual)
- Verify referenced assets still resolve (hand off to `media-debugger` if media is involved)
- **Verify no unrelated changes occurred** — re-read the diff specifically looking for anything you didn't intend to touch

For the cross-cutting rules that apply to every skill — approval gates, deployment safety, the reporting format Erick expects — see `.claude/skills/WORKFLOW.md`.
