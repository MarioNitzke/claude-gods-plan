# Phase 7: Presentation

This is the first time the user sees the result. Make it count.

## What to show

1. **The final plan** — with all revisions from Phases 4-6 applied. Update the plan file.

2. **Pipeline Report** (at the top of the plan):
   - Replan findings: X critical, Y important, Z minor
   - Codex suggestions: N total (accepted: X, rejected: Y)
   - Gemini suggestions: N total (accepted: X, rejected: Y)

3. **Replan summary** — what the internal review found and changed

4. **External review summary:**
   - What Codex and Gemini suggested
   - What you verified and added (and why)
   - What you rejected (and why)
   - Controversial suggestions → ask the user via AskUserQuestion

5. **Ask for approval** — the user decides whether to proceed

## After approval

The plan is approved. Move to execution:
- Read `${CLAUDE_SKILL_DIR}/references/execution.md` and follow it

## Output format

The plan should follow this structure:

~~~markdown
# {Feature} Implementation Plan (God's Plan)

> **For Claude:** Run /gods-plan on this plan file to execute it task-by-task.

**Goal:** {one sentence}
**Architecture:** {2-3 sentences}
**Tech Stack:** {key technologies/libraries}

**Pipeline Report:**
- Replan findings: {critical} critical, {important} important, {minor} minor
- Codex suggestions: {count} (accepted: {n}, rejected: {m})
- Gemini suggestions: {count} (accepted: {n}, rejected: {m})

## Replan Summary
| Finding | Severity | Incorporated? |
|---------|----------|--------------|

## External Review Summary
| Suggestion | Source | Verified? | Accepted? | Reason |
|------------|--------|-----------|-----------|--------|

## Task 1: ...
- [ ] Step 1: ...
~~~
