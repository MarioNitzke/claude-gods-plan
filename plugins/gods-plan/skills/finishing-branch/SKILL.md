---
name: finishing-branch
description: "Complete development work by verifying tests, presenting 4 integration options (merge, PR, keep, discard), and cleaning up."
---

# Finishing a Development Branch

## Overview

Complete development work by verifying tests pass, then presenting structured options for integration.

**Announce at start:** "I'm using the finishing-branch skill to complete this work."

## The Process

### Step 1: Verify Tests

Run the test suite BEFORE offering any options. If tests fail, STOP and fix them first.

### Step 2: Determine Base Branch

Identify which branch the feature branch split from (typically `main`).

### Step 3: Present Options

Present exactly 4 choices:

1. **Merge** back to base branch locally
2. **Push and create a Pull Request**
3. **Keep** the branch as-is (I'll handle it later)
4. **Discard** this work

### Step 4: Execute Choice

**Option 1 — Merge:**
- Checkout base branch, pull latest
- Merge feature branch
- Run tests on merged result
- Delete feature branch

**Option 2 — Pull Request:**
- Push branch to origin
- Create PR with structured body:
  ```
  ## Summary
  - [What was implemented]
  - [Key architectural decisions]

  ## Test plan
  - [ ] [Verification steps]

  ## God's Plan audit trail
  - Replan findings: X critical, Y important, Z minor
  - External review: Gemini (N accepted/M rejected), Codex (N accepted/M rejected)
  ```
- Keep branch alive

**Option 3 — Keep:**
- Report keeping the branch
- Do not cleanup worktree

**Option 4 — Discard:**
- Warn: "This will delete all work on this branch (commits and worktree)"
- Require user to type "discard" as confirmation
- Delete branch and worktree

### Step 5: Cleanup Worktree

Before cleanup: check if currently in a worktree. If not (e.g., working on main), skip worktree cleanup.

- Options 1, 4: Remove worktree
- Option 2: Keep worktree (branch still active)
- Option 3: Keep worktree

## Rules
- Never proceed with failing tests
- Always verify tests before offering options
- Present exactly 4 options — no open-ended questions
- Require typed "discard" confirmation for Option 4
- Always run tests on merged result before deleting feature branch
- If user wants to cancel mid-process, stop immediately and confirm
