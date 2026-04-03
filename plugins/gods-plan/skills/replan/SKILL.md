---
name: replan
description: "Validate an implementation plan by launching parallel review subagents from multiple angles — codebase, best practices, feasibility, and fresh perspectives."
effort: high
---

# Plan Review (/replan)

Review and validate an existing plan before execution by dispatching specialized review agents in parallel.

## Step 1: Understand the Plan

Read the plan carefully and identify:
- **Plan type:** implementation, research, design, migration, refactoring, infrastructure
- **Key domains:** which parts of the codebase it touches
- **Technologies:** specific frameworks, libraries, APIs mentioned
- **Assumptions:** what the plan takes for granted

## Step 2: Design Review Agents

Select review perspectives based on the ACTUAL plan scope (not a fixed set):

| Perspective | When to include | What it checks |
|---|---|---|
| Codebase alignment | Always for code-touching plans | Do referenced files/functions exist? Do patterns match? |
| Best practices & architecture | Always for code-touching plans | SOLID, DRY, YAGNI, error handling, security (OWASP) |
| Project standards | When CLAUDE.md/conventions exist | Naming, file structure, tests, commits, tech stack compliance |
| Feasibility & risks | Always | Missing steps, dependency ordering, complexity, blockers |
| Fresh perspective | **Always** (wildcard) | Blind spots, edge cases, simpler alternatives, UX gaps |
| Research & fact-checking | APIs, libs, protocols mentioned | API existence, version validation, assumption verification |
| Design & UX | UI/visual components | Consistency, accessibility, responsiveness, states |
| Data & schema | Database/data model changes | Migration safety, backwards compatibility, indexing |
| Security | Auth, user input, APIs | Auth flows, input validation, secrets, attack surface |
| Operations | Infrastructure, deployment | Rollback strategy, monitoring, config management |

**Agent count rules:**
- Minimum 3 agents (even for simple plans)
- **Always include fresh perspective** — it consistently catches what others miss
- Scale: roughly 1 agent per 3-4 tasks in the plan
- 3-task plan → 3-4 agents, 15-task plan → 6-8 agents
- Maximum 8 agents — beyond this, diminishing returns and coordination overhead

## Step 3: Dispatch All Agents in Parallel

Each agent receives:
1. Complete plan text (full copy)
2. Relevant codebase context (prefer Serena semantic tools over Glob/Grep when available)
3. Specific review mandate
4. Structured output format

**Fresh perspective agent framing:**
> You are reviewing this plan with completely fresh eyes. Read it cold and ask:
> - What would go wrong that nobody anticipated?
> - What's missing that seems obvious once you think about it?
> - Are there simpler alternatives?
> - What will the executor wish they'd known upfront?
> - Edge cases, error states, user scenarios ignored?

## Step 4: Synthesize and Update the Plan

1. Collect all findings from agents
2. Categorize by severity:
   - **Critical:** Must fix before implementation (wrong assumptions, security holes, missing steps, non-existent references)
   - **Important:** Should fix (better approaches, missing error handling, edge cases)
   - **Minor:** Nice to have (style suggestions, optional improvements)
3. Present summary to user
4. Update plan with Critical and Important fixes
5. Note what changed and why

**When called from God's Plan (Phase 4):** skip user presentation in step 3. The user sees the final plan only in Phase 7 (Presentation). Incorporate findings and continue.

**Output format for findings:**

| # | Area | Finding | Severity | Suggestion |
|---|------|---------|----------|------------|
| 1 | ... | ... | Critical/Important/Minor | ... |

The synthesized summary adds an "Incorporated?" column.

## Adapting to Plan Type

- **Implementation plan?** → Codebase alignment + best practices + feasibility + fresh perspective
- **Research plan?** → Fact-checking + feasibility + fresh perspective
- **Design plan?** → Design & UX + best practices + fresh perspective
- **Migration plan?** → Data & schema + operations + feasibility + fresh perspective
- **Refactoring plan?** → Codebase alignment + best practices + project standards + fresh perspective
- **Infrastructure plan?** → Operations + security + feasibility + fresh perspective
