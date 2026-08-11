---
name: media-debugger
description: Use this skill IMMEDIATELY whenever an image, video, icon, favicon, or poster on the Sambrano Roofing & Exteriors LLC website appears broken, missing, or won't load/play — whether reported on a local preview or on the live Netlify deployment. Trigger this BEFORE writing any code fix, even if the cause seems obvious. This skill enforces diagnose-before-modify for every media problem, including the specific "index.html deployed but assets/ was left out" failure mode this exact project has hit before.
---

# Media Debugger

When media is broken, the instinct is to jump straight to editing a path in `index.html`. Resist that. On this project, the actual root cause has already turned out to be something code changes couldn't have fixed — a Netlify deployment that published `index.html` but not the `assets/` directory. Editing `index.html` in that situation would have been a wasted, risky change to a file that was never the problem. Diagnosis first isn't caution for its own sake here — it's the difference between a real fix and a plausible-looking wrong one.

## The known project baseline (check against this first)

`assets/` contains exactly:
- `logo/logo-white.png`, `logo/logo-color.png`, `logo/favicon.png`
- `video/hero.mp4`, `video/hero.webm`, `video/poster.jpg`
- `images/projects/project-1.jpg` through `project-6.jpg`

Every reference to these in `index.html` has already been verified correct (relative paths, exact case, no leading slash, no absolute/local paths) as of the last full audit. **If a media problem shows up, that audit is the baseline to compare against — a new mismatch would be a real, notable finding, not just "probably a typo somewhere."**

## Full inspection checklist — work through this before touching code

1. The HTML reference (`src`, `href`, `poster` attributes)
2. CSS references (e.g. `content: url(...)`, `background-image`)
3. JavaScript references (e.g. the `PROJECTS` data object's image paths)
4. The actual files on disk in `assets/`
5. Exact filenames, character by character
6. Case sensitivity — macOS is case-insensitive by default; Netlify's servers are not. A mismatch that works locally can break only after deploy.
7. File extensions (`.jpg` vs `.jpeg`, `.png` vs `.PNG`, etc.)
8. Relative paths (should be plain `assets/...`, no leading `/` needed for this deployment, no `../`)
9. Absolute/local paths (`/Users/...`, `file://`, `C:\...`) — these are always wrong once deployed
10. Directory structure — does the folder nesting in `assets/` actually match what the reference expects?
11. File size — is the local file plausible (not 0 bytes, not truncated)?
12. File integrity when it can be checked (does the file actually open/render locally?)
13. Deployment structure — is `assets/` actually present in the published site, not just locally? (This is the failure mode that has already happened once here — see `deployment-verifier`.)
14. HTTP response behavior when the live site can be inspected — status code, content-type, and whether the body is actually the expected file or a fallback/error page.

## Classify the cause before fixing anything

For every broken-media report, determine which of these it actually is:
- Code (wrong path/reference in HTML, CSS, or JS)
- Filename mismatch
- Case mismatch
- Path structure problem
- Missing asset (never existed, or was deleted)
- Corrupted asset (exists but unreadable/truncated)
- Deployment problem (exists locally, wasn't published)
- Hosting configuration (e.g. wrong publish/base directory)
- Browser behavior (a specific browser/OS quirk, not a real defect)
- Caching (stale CDN or browser cache serving an old response)
- MIME type (server serving the file with a content-type the browser won't render as expected)
- Unsupported format (e.g. a codec the browser can't decode)

Only once you know which of these it is should you decide what to change — and what to change may be a Netlify setting, not a line of code.

## Hard rules

- **Never "fix" a media problem by swapping in a random or placeholder image.** If the right asset is genuinely missing, say so and ask — don't paper over it.
- **Never create a replacement asset unless explicitly instructed.** This includes generating a new image/icon/video as a "fix."

## Video-specific checklist

For a video that won't play, verify all of:
- Source path (`<source src="...">`) resolves to an existing file
- File actually exists and isn't corrupted
- MIME/type attribute matches the actual file format
- Format/codec is supported by the browser being tested
- `poster` attribute path is correct (fallback image)
- Autoplay requirements are met — most browsers require `muted` for autoplay to be allowed at all
- `playsinline` is present (needed for autoplay on mobile Safari)
- Browser-specific autoplay restrictions (some browsers block autoplay under certain conditions regardless of `muted`/`playsinline`)
- Actual network response for the video file (status code, whether bytes are actually returned)
- Deployment availability — same caution as images: local existence doesn't prove production existence

For the cross-cutting rules that apply to every skill, see `.claude/skills/WORKFLOW.md`.
