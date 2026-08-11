# Website Operations — Master Workflow

This is the shared operating system for the seven skills below. It's not a skill itself (nothing here triggers automatically) — it's the reference each skill points back to for the rules that apply no matter which one is active.

## The skills

| Skill | Handles |
|---|---|
| [`website-architect`](website-architect/SKILL.md) | Structure, sections, navigation, how things depend on each other |
| [`frontend-builder`](frontend-builder/SKILL.md) | Actual HTML/CSS/JS implementation |
| [`brand-guardian`](brand-guardian/SKILL.md) | Logo, colors, typography, voice, claims, photography — brand integrity |
| [`media-debugger`](media-debugger/SKILL.md) | Broken/missing images, video, icons — diagnosis before fixes |
| [`visual-qa`](visual-qa/SKILL.md) | Confirming a change actually renders correctly before calling it done |
| [`deployment-verifier`](deployment-verifier/SKILL.md) | Confirming production actually serves what local development has |
| [`safe-editor`](safe-editor/SKILL.md) | The baseline discipline underneath every edit: inspect → plan → edit → verify |

These usually chain together on a real task — e.g., a "the hero image looks wrong on mobile" report might touch `media-debugger` (is it actually a media problem?), `frontend-builder` (implement the fix), `visual-qa` (confirm it), and `safe-editor` (throughout).

## Workflow — feature/content requests

```
REQUEST
  ↓
INSPECT      (read the relevant code/assets — safe-editor, website-architect)
  ↓
PLAN         (smallest change that solves it — flag anything brand- or architecture-sensitive)
  ↓
BUILD        (frontend-builder implements)
  ↓
TEST         (does it function as intended?)
  ↓
VISUAL QA    (does it actually render correctly at every breakpoint? — visual-qa)
  ↓
DEPLOYMENT VERIFICATION   (only if this task includes deploying — deployment-verifier)
  ↓
REPORT       (see the reporting format below)
```

## Workflow — technical problems (something's broken)

```
REQUEST
  ↓
DIAGNOSE               (media-debugger / deployment-verifier — don't guess)
  ↓
IDENTIFY ROOT CAUSE     (state it specifically, not just "probably a path issue")
  ↓
PLAN FIX
  ↓
ASK APPROVAL IF PRODUCTION-IMPACTING
  ↓
IMPLEMENT
  ↓
VERIFY                  (re-check the exact thing that was broken, not just "looks fine now")
  ↓
REPORT
```

The diagnose-before-modify step is not a formality here — this project has already had two real incidents (assets/ missing from a Netlify deploy; a Netlify visitor-access gate returning 401 site-wide) where the surface symptom ("broken images") had a completely different, non-code root cause. Guessing and editing `index.html` would not have fixed either one.

## Global operating rules (apply across every skill)

**1. Preserve the baseline.** The current website is the baseline. Don't redesign it unless explicitly instructed.

**2. Don't guess.** If something can be verified, verify it. Don't invent explanations for technical failures.

**3. Diagnose before modifying** — especially for images, video, deployment, forms, routing, and responsive problems.

**4. Minimal changes.** Prefer the smallest change that solves the actual, stated problem.

**5. Protect business information.** Never invent or modify business information (services, credentials, claims, project details) without authorization.

**6. Production safety.** Never deploy a change simply because the code compiles or looks right locally — verify the actual public result.

**7. Deployment is different from local development.** A site working locally does not prove production works. Always verify production directly.

**8. Keep Erick in control.** Claude handles the technical work, but Erick remains the final decision-maker for: brand direction, marketing, offers, pricing, business claims, major design changes, major architecture changes, and any production-impacting decision.

**9. Explain important decisions clearly, using this structure when reporting:**

```
WHAT I FOUND
WHAT CAUSED IT
WHAT I RECOMMEND
WHAT WILL CHANGE
WHAT I NEED TO APPROVE
```

Don't overwhelm with technical detail beyond what's needed to make the decision.

**10. No unauthorized deployment.** Never publish production changes without explicit authorization to deploy, specifically, for that change.
