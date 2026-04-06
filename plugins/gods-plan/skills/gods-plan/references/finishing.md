# Finishing Up

Development is done. Now verify everything works and give the user clean options for what to do next.

## Step 1: Verify tests

Run the test suite before offering any options. If tests fail, stop and fix them first. Don't move forward with broken tests.

## Step 2: Figure out the base branch

Identify which branch the feature branch split from (usually `main`).

## Step 3: Present 4 options

Give the user exactly these choices:

1. **Merge** — merge back to base branch locally
2. **Pull Request** — push and create a PR
3. **Keep** — leave the branch as-is ("I'll handle it later")
4. **Discard** — delete this work

## Step 4: Execute their choice

**Merge:**
- Checkout base branch, pull latest
- Merge the feature branch
- Run tests on the merged result
- Delete the feature branch

**Pull Request:**
- Push the branch to origin
- Create PR with structured body:
  ```
  ## Summary
  - [What was implemented]
  - [Key architectural decisions]

  ## Test plan
  - [ ] [Verification steps]

  ## God's Plan audit trail
  - Replan findings: X critical, Y important, Z minor
  - External review: Codex (N accepted/M rejected), Gemini (N accepted/M rejected)
  ```
- Keep the branch alive

**Keep:**
- Just confirm and leave it alone
- Don't clean up the worktree

**Discard:**
- Warn: "This will delete all work on this branch"
- Require the user to type "discard" as confirmation
- Delete branch and worktree

## Step 5: Worktree cleanup

If you're in a worktree:
- Merge or Discard → remove the worktree
- PR or Keep → leave the worktree

If you're not in a worktree (working on main), skip this step.

## Rules
- Never proceed with failing tests
- Always verify before offering options
- Exactly 4 options — no open-ended questions
- Require typed "discard" for the delete option
- Run tests on merged result before deleting the feature branch
