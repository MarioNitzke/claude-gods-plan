---
name: gods-plan
description: "8-phase ultra-planning with Claude, Codex, Gemini and Serena. Brainstorm, analyze, confess, draft, replan, friend-review, verify, present."
allowed-tools:
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
  - Skill(codex:rescue)
  - Skill(gemini:rescue)
  - WebSearch
  - WebFetch
effort: max
---

# God's Plan

"I'm using God's Plan for: $ARGUMENTS"

8 phases (0-7). Works alongside plan mode — fills the plan file incrementally. Use ultrathink reasoning throughout.

**First:** Read `${CLAUDE_SKILL_DIR}/references/ground-rules.md` — these rules aren't optional.

## Mode detection

If `$ARGUMENTS` points to an existing approved plan file → skip to execution (read `${CLAUDE_SKILL_DIR}/references/execution.md`).
Otherwise → start from Phase 0.

## The pipeline

Each phase has a reference file with detailed instructions. Read it when you reach that phase.

| Phase | What happens | Reference | User involved? |
|-------|-------------|-----------|----------------|
| 0 | Brainstorming — explore intent, propose approaches, get design approval | `phase-0-brainstorming.md` | Yes |
| 1 | Analysis — deep codebase dive via Serena or Glob/Grep | `phase-1-analysis.md` | No |
| 2 | Confession — systematic requirements gathering (min 8 questions) | `phase-2-confession.md` | Yes |
| 3 | Draft — write bite-sized implementation plan (internal) | `phase-3-draft.md` | No |
| 4 | Replan — parallel subagent review from 3-8 angles | `phase-4-replan.md` | No |
| 5 | Friends — 3x Codex + 3x Gemini review with targeted questions | `phase-5-friends.md` | No |
| 6 | Verification — fact-check external claims against actual code | `phase-6-verification.md` | No |
| 7 | Presentation — show final plan with full audit trail, get approval | `phase-7-presentation.md` | Yes |

After approval → `execution.md` → `finishing.md`

## How to read references

Each phase: `Read ${CLAUDE_SKILL_DIR}/references/<filename>` and follow the instructions there.

## Dependencies

- **Codex plugin** — required (Phase 5 external review)
- **Gemini plugin** — recommended. If missing, all 6 external reviews go to Codex.
- **Serena plugin** — recommended. If missing, fall back to Glob/Grep/Read for code analysis.

## External review (Phase 5)

Uses `Skill("codex:rescue")` and `Skill("gemini:rescue")` to send the plan for review. See `phase-5-friends.md` for the full process — 6 targeted questions, split by model strengths.

Review prompt template: `${CLAUDE_SKILL_DIR}/references/review-prompt-template.md`
