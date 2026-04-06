# Review Prompt Template

This template is used in Phase 5 (Friends Review). Fill in the variables before sending to each external model.

---

Here's an implementation plan I'm working on. I'd like your honest take on it.

## Project context
{PROJECT_CONTEXT}

## The plan
{PLAN_CONTENT}

## What I'd especially like your opinion on

{TOPIC}

Take your time with this. Read the whole plan first so you have the full picture, then focus on the topic above.

What I'm looking for:
- **Your honest reaction** — does this approach make sense to you?
- **What would you do differently?** — not just what's "wrong", but how you'd approach it. Show code if relevant.
- **What's missing?** — edge cases, failure modes, things I didn't think about.
- **What's unnecessary?** — over-engineering, complexity that doesn't earn its keep.
- **Your single most important recommendation** — if I could change one thing, what should it be?

Be specific — reference exact files, patterns, code. Vague feedback doesn't help.

For each finding, include:
- **Area:** (architecture / data / testing / security / performance / UX / other)
- **What:** your observation or suggestion
- **Why:** convince me
- **Severity:** CRITICAL / IMPORTANT / MINOR
