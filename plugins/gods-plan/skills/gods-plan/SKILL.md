---
name: gods-plan
description: "8-phase ultra-planning with Claude, Gemini, Codex and Serena. Brainstorm, analyze, confess, draft, replan, friend-review, verify, present."
allowed-tools:
  - Skill(*)
  - Read
  - Glob
  - Grep
  - Bash
  - AskUserQuestion
  - Agent
  - mcp__plugin_serena_serena__find_symbol
  - mcp__plugin_serena_serena__get_symbols_overview
  - mcp__plugin_serena_serena__find_referencing_symbols
  - mcp__plugin_serena_serena__search_for_pattern
  - mcp__plugin_serena_serena__find_file
  - mcp__plugin_serena_serena__list_dir
  - mcp__plugin_serena_serena__read_file
  - WebSearch
  - WebFetch
effort: max
---

# God's Plan

"I'm using God's Plan to create a multi-model ultra-plan for: $ARGUMENTS"

Phases are numbered 0 through 7 (8 total). God's Plan works WITH plan mode — the plan file is managed by plan mode, God's Plan fills it incrementally. Use ultrathink reasoning throughout for maximum depth.

## Iron Laws
- **No plan without confession** — minimum 8 questions. The plan must not exist before confession is complete.
- **No skipping external review** — always send to Gemini + Codex, even if the plan looks perfect.
- **Evidence before adding** — do not add a suggestion from Gemini/Codex without subagent verification.
- **User decides** — controversial suggestions always go to user via AskUserQuestion.

## Phase Restart
If a later phase reveals fundamental problems, restart the appropriate phase with user confirmation:
- Approach is wrong → restart Phase 0 (Brainstorming)
- Missing requirements → restart Phase 2 (Confession)
- Plan structure is wrong → restart Phase 3 (Draft)

## Red Flags
| Thought pattern | What to do instead |
|---|---|
| "This task is simple, no confession needed" | User invoked God's Plan — they want the full process. Do the confession. |
| "I already know what to do, skip questions" | Confession reveals what you don't know. Always ask. |
| "Gemini/Codex feedback isn't useful" | Present to user — they decide what's useful. |
| "Replan isn't needed, plan is good" | Replan is mandatory. It consistently finds improvements. |
| "I'll use Glob/Grep for analysis" | Use Serena semantic tools first — deeper analysis, fewer tokens. |

## Integration
- `Skill("gods-plan:brainstorming")` — Phase 0, always first
- `Skill("gods-plan:writing-plans")` — Phase 3 (does not present to user)
- `Skill("gods-plan:replan")` — Phase 4, external plan review
- `Skill("gemini:rescue")` — Phase 5, Gemini review
- `Skill("codex:rescue")` — Phase 5, Codex review
- `Skill("gods-plan:executing-plans")` — handoff after user approves plan in Phase 7

---

## Phase 0: BRAINSTORMING

Invoke `Skill("gods-plan:brainstorming")` first. This explores user intent, proposes 2-3 approaches, and gets design approval before any code analysis begins.

Once brainstorming is complete and user approved the design direction, continue to Phase 1.

---

## Phase 1: ANALYSIS (via Serena + subagents)

Launch subagents IN PARALLEL (Agent tool, subagent_type: "Explore") that use Serena semantic tools for token-efficient deep analysis:

**Agent 1 — Architecture overview:**
- `get_symbols_overview` on key files relevant to `$ARGUMENTS`
- Understand classes, interfaces, handlers without reading full files
- Read CLAUDE.md (if exists) for project conventions

**Agent 2 — Impact analysis:**
- `find_symbol` to locate specific handlers/components related to the task
- `find_referencing_symbols` to map what depends on what
- Identify what would be affected by changes

**Agent 3 — Pattern discovery:**
- `search_for_pattern` to find existing patterns that can be reused
- Identify similar features already implemented in the codebase

Each subagent returns a summary — main context stays clean. Fallback to Glob/Grep/Read only if Serena is unavailable.

Result: internal notes about code state, architecture, dependencies, and reusable patterns. User does nothing.

---

## Phase 2: CONFESSION

Do not proceed until confession is complete.

Read `${CLAUDE_SKILL_DIR}/references/confession-questions.md` and systematically ask via AskUserQuestion.

Rules:
- Ask **2-4 questions at a time** via AskUserQuestion — not one by one, that's too slow
- Ask SPECIFIC questions, not vague ones
- Suggest options (A/B/C) where appropriate
- After each batch of answers, follow up with the next batch
- **Minimum 8 questions total** (Iron Law)
- Confession ends when you say "I have enough information for the plan"

---

## Phase 3: DRAFT PLAN (internal — user does NOT see this)

1. Invoke `Skill("gods-plan:writing-plans")` without announcing to user — the draft is internal. Only announce phases that involve user interaction (0, 2, 7).
2. Create comprehensive draft plan based on:
   - Code analysis (Phase 1)
   - User context (Phase 2)
   - CLAUDE.md rules and project conventions
3. Plan includes: tasks, bite-sized steps, exact file paths, code snippets, testing strategy
4. Use Edit tool to write draft to the active plan file. Plan mode manages the file path. `<topic>` = key design direction from Phase 0 (e.g., `notification-async-queue`, `auth-multi-provider`).
5. **Do not present to user. Continue to Phase 4.**

---

## Phase 4: REPLAN

1. Invoke `Skill("gods-plan:replan")` on the draft from Phase 3
2. Replan adaptively selects review perspectives based on plan type
3. Dispatches parallel subagents (3-8 based on complexity)
4. Result: findings categorized as Critical / Important / Minor
5. Incorporate Critical and Important findings into the draft
6. **Continue to Phase 5 — do not present to user.**

**If 2+ of 3+ agents fail:** warn user and present partial findings. Ask: (a) retry, (b) continue with partial replan, (c) abort.

---

## Phase 5: FRIENDS (Gemini + Codex review)

Read `${CLAUDE_SKILL_DIR}/references/review-plan-prompt.md`. Replace:
- `{PROJECT_CONTEXT}` — relevant CLAUDE.md content + tech stack from Phase 1
- `{PLAN_CONTENT}` — current draft plan after Phase 4

Send to BOTH models IN PARALLEL via their rescue skills:

```
Skill("gemini:rescue", args: "<full review prompt with plan>")
Skill("codex:rescue", args: "<full review prompt with plan>")
```

**If a rescue skill fails:** warn user and continue without that model. Both models failing is unlikely but not fatal — Phase 4 (Replan) already validated the plan.

**If BOTH rescue skills fail:** log the failure, inform the user, and proceed directly to Phase 7 (Presentation). Phase 6 (Verification) is skipped since there are no external claims to verify. Note in the Pipeline Report that external review was unavailable.

---

## Phase 6: VERIFICATION (via Serena + subagents)

Claude does not blindly accept feedback from friends. Verify claims before incorporating them.

**Step 1 — Categorize claims:**
- **(a) Factual/verifiable** (file exists, pattern is used, architecture constraint) → spawn verification agent
- **(b) Opinion/suggestion** (caching, refactoring, alternative approach) → mark as CONTROVERSIAL and present to user in Phase 7

**Step 2 — Verify factual claims:**
Group claims by source (Gemini, Codex). Spawn 1 verification agent per 2-3 claims (Agent tool, subagent_type: "Explore") using Serena:
- "Gemini claims file X doesn't exist" → `find_file` + `find_symbol` verification
- "Codex suggests using pattern Y" → `find_symbol` + `find_referencing_symbols` — does Y exist? is it compatible?
- "Gemini says edge case Z is missing" → `get_symbols_overview` + `search_for_pattern` — is Z actually possible?

Each agent reports VERIFIED or UNVERIFIED status per claim.

**Step 3 — Categorize after verification:**
- **VERIFIED + ADD** — claim confirmed and improves plan → add, mark "[Gemini]" / "[Codex]"
- **VERIFIED + SKIP** — claim is true but irrelevant / over-engineered
- **UNVERIFIED** — claim could not be verified → discard with reason
- **CONTROVERSIAL** — claim is valid but unclear if user wants it → note for presentation

---

## Phase 7: PRESENTATION

Only now does user see the result. Present:

1. **Final plan** (after all revisions) — update draft in the plan file
2. **Replan summary** — what changed after replan skill
3. **Friends summary:**
   - What Gemini/Codex suggested
   - What Claude verified and added (and why)
   - What Claude rejected (and why)
   - Controversial suggestions → AskUserQuestion
4. **Plan for approval** by user

After approval: handoff to `Skill("gods-plan:executing-plans")`.

---

## Output format

Plan in `.claude/plans/YYYY-MM-DD-gods-plan-<topic>.md`:

```markdown
# {Feature} Implementation Plan (God's Plan)

> **For Claude:** REQUIRED SUB-SKILL: Use gods-plan:executing-plans to implement this plan task-by-task.

**Goal:** {one sentence}
**Architecture:** {2-3 sentences}
**Tech Stack:** {key technologies/libraries}

**Pipeline Report:**
- Replan findings: {critical} critical, {important} important, {minor} minor
- Gemini suggestions: {count} (verified+accepted: {n}, rejected: {m})
- Codex suggestions: {count} (verified+accepted: {n}, rejected: {m})

## Replan Summary
| Finding | Severity | Incorporated? |
|---------|----------|--------------|

## External Review Summary
| Suggestion | Source | Verified? | Accepted? | Reason |
|------------|--------|-----------|-----------|--------|

## Task 1: ...
- [ ] Step 1: ...
```
