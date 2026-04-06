# Phase 4: Replan

Review and validate the draft plan by dispatching specialized subagents in parallel. This consistently finds issues — don't skip it.

## Step 1: Understand the plan

Read the plan carefully and identify:
- **Plan type:** implementation, research, design, migration, refactoring, infrastructure
- **Key domains:** which parts of the codebase it touches
- **Technologies:** specific frameworks, libraries, APIs
- **Assumptions:** what the plan takes for granted

## Step 2: Pick review perspectives

Select perspectives based on what the plan actually covers (not a fixed set):

| Perspective | When to include | What it checks |
|---|---|---|
| Codebase alignment | Always for code-touching plans | Do referenced files/functions exist? Do patterns match? |
| Best practices | Always for code-touching plans | SOLID, DRY, YAGNI, error handling, security |
| Project standards | When CLAUDE.md/conventions exist | Naming, file structure, tests, tech stack compliance |
| Feasibility & risks | Always | Missing steps, dependency ordering, blockers |
| Fresh perspective | **Always** | Blind spots, edge cases, simpler alternatives |
| Research | APIs, libs, protocols mentioned | API existence, version validation |
| Design & UX | UI/visual components | Consistency, accessibility, states |
| Data & schema | Database/data model changes | Migration safety, backwards compatibility |
| Security | Auth, user input, APIs | Auth flows, input validation, secrets |
| Operations | Infrastructure, deployment | Rollback, monitoring, config |

**How many agents:**
- Minimum 3, maximum 8
- Always include fresh perspective — it consistently catches what others miss
- Roughly 1 agent per 3-4 tasks in the plan
- 3-task plan → 3-4 agents, 15-task plan → 6-8 agents

## Step 3: Dispatch all agents in parallel

Each agent gets:
1. The complete plan text
2. Relevant codebase context (prefer Serena if available, otherwise Glob/Grep)
3. Their specific review mandate
4. Structured output format

**Fresh perspective agent gets this framing:**
> Read this plan with completely fresh eyes. What would go wrong that nobody anticipated? What's missing that seems obvious once you think about it? Are there simpler alternatives? What will the executor wish they'd known upfront?

## Step 4: Synthesize and update

1. Collect findings from all agents
2. Categorize:
   - **Critical** — must fix (wrong assumptions, security holes, missing steps, non-existent references)
   - **Important** — should fix (better approaches, missing error handling, edge cases)
   - **Minor** — nice to have (style, optional improvements)
3. Fold Critical and Important findings into the draft
4. Note what changed and why

**If 2+ agents fail:** warn user, present partial findings. Ask: retry, continue with partial results, or abort.

**Don't present to user — continue to Phase 5.**

## Findings format

| # | Area | Finding | Severity | Suggestion | Incorporated? |
|---|------|---------|----------|------------|---------------|
