---
name: deployment-verifier
description: Use this skill before AND after any deployment of the Sambrano Roofing & Exteriors LLC website, and any time the live site is reported to look different from local (broken images, missing sections, stale content). This is the skill that exists specifically because of a real incident — Netlify once served index.html correctly while assets/ was entirely missing from the deploy. Trigger it proactively before trusting that "the code is correct locally" means "production actually works," and use it to distinguish real deployment bugs from hosting/access-control issues like the Netlify visitor-access gate this project also hit.
---

# Deployment Verifier

This skill exists because of two real, already-diagnosed incidents on this exact project, not as a hypothetical precaution:

1. **The assets/ directory was missing from a Netlify deploy.** `index.html` returned HTTP 200 and matched the local file byte-for-byte; every single `assets/...` URL returned Netlify's generic 404 page. The code was never wrong — only part of the folder was ever uploaded.
2. **Netlify's site-wide visitor-access gate returned 401 for every URL, including the homepage itself**, before that setting was disabled — which looked like a deployment failure but was actually a hosting/access-control setting unrelated to the files at all.

Both looked, from the outside, like "the images are broken." They had completely different causes and completely different fixes. This skill's job is to tell those apart *before* anyone touches code.

## The two rules this skill exists to enforce

> **Never assume that because a file exists locally, it exists in production.**
> **Never assume that because the HTML references an asset correctly, the asset is actually deployed.**

Both of these were true and both still resulted in broken images. Local correctness and reference correctness are necessary but not sufficient.

## Before deployment

Verify, in this order:
1. Current working directory is the project root (`site-2`)
2. `index.html` is present
3. `assets/` is present
4. `assets/logo/`, `assets/video/`, `assets/images/projects/` all exist and contain the expected files
5. Deployment configuration — if it exists (base directory, publish directory, any `netlify.toml`), confirm it points at the folder containing both `index.html` and `assets/` as siblings — not a parent folder that would nest `site-2` inside the published output, and not a subfolder that would leave `assets/` behind.

## After deployment

Verify the live site directly — don't infer success from "the deploy command didn't error." Check the actual public URLs, read-only (`curl -I` / `curl -D -` are sufficient, no state-changing requests needed):

```
/
/index.html
/assets/logo/logo-white.png
/assets/logo/logo-color.png
/assets/logo/favicon.png
/assets/video/poster.jpg
/assets/video/hero.mp4
/assets/images/projects/project-1.jpg
```

(Add any other asset actually referenced by the current `index.html` to this list — it should track whatever's really on the page, not stay frozen at this exact set.)

For each one, record:
- **HTTP status** — 200 is success; 404 means the file isn't in the deploy; 401/403 means something is blocking access before the file is even considered
- **Content-type** — does it match what's expected (`image/png`, `image/jpeg`, `video/mp4`, `text/html`)?
- **Whether the response body is actually the expected file** — a 200 with an HTML body where an image was expected is still broken; check content-length/body shape, not just the status code
- **Whether it's a 404** — and if so, whether the 404 page is the site's own generic fallback (meaning the path just isn't in the deploy) rather than something more specific
- **Whether access control is blocking the request** — a 401/403 across *every* URL including `/` itself, especially with a body that looks like a login/redirect page rather than a normal error page, points at a site-wide access gate (like Netlify's visitor access), not a missing-asset problem. A 404 that's scoped to only some paths (e.g. everything under `/assets/` while `/` succeeds) points at a folder that wasn't included in the deploy.
- **Whether a redirect occurs** — and where it goes
- **Whether the deployed version is actually current** — compare content-length or a quick content check of `index.html` against the local file to confirm the live site reflects the latest local changes, not a stale prior deploy

## If deployment configuration is the problem

**Identify the exact configuration problem before changing anything.** "Redeploy and see if it works" is not a diagnosis. State specifically what's wrong (e.g. "only `index.html` was uploaded, not the `site-2` folder containing it") and what the fix is (e.g. "redeploy the whole `site-2` folder so `assets/` publishes alongside `index.html`") before taking any action — and per the global rules, get explicit approval before actually deploying anything.

For the cross-cutting rules that apply to every skill, see `.claude/skills/WORKFLOW.md`.
