# Execution

Load the approved plan, review it critically, then execute tasks in batches with checkpoints for review.

## Step 1: Load and review

1. Read the plan file
2. Look it over critically — any questions or concerns?
3. If something's off: raise it with the user before starting
4. If it looks good: create a task checklist (TaskCreate) and get going

## Step 2: Execute a batch

Work through 3 tasks at a time (default batch size).

For each task:
1. Mark as in_progress
2. Follow each step exactly — the plan has bite-sized steps for a reason
3. Run verifications as specified
4. Mark as completed

## Step 3: Report

After each batch:
- Show what was implemented
- Show verification output
- Say: "Ready for feedback."

## Step 4: Continue

Based on user feedback:
- Changes requested → apply them, re-run verifications
- Approved → execute next batch of 3
- Different batch size → use their preference going forward
- Repeat until done

## Step 5: Finish up

After all tasks are done and verified:
- Read `${CLAUDE_SKILL_DIR}/references/finishing.md` and follow it

## When to stop and ask

Stop executing immediately when:
- You hit a blocker (missing dependency, test fails, instruction unclear)
- The plan has gaps that prevent starting
- You don't understand an instruction
- Verification fails repeatedly

Ask for clarification rather than guessing. Don't force through blockers.

## Keep in mind
- Review the plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Between batches: just report and wait
- Never start implementation on main/master without explicit user consent
