# Phase 5: Friends Review (3x Codex + 3x Gemini)

Time to get outside perspectives. Claude picks 6 topics from the plan that most deserve a fresh pair of eyes, then asks Codex and Gemini for their honest take.

## How it works

**Step 1: Pick 6 topics**

Go through the plan and identify the 6 things that matter most for external review — architectural decisions, risky parts, trade-offs, areas where you're not 100% sure. Each topic should be distinct, no overlap.

**Step 2: Assign topics to models by their strengths**

| Codex (3 topics) | Gemini (3 topics) |
|---|---|
| Backend logic, architecture | Frontend, UI/UX, components |
| Data, migrations, schema | User flows, states, UX |
| Error handling, security, APIs | Visual consistency, accessibility |

If the plan has no frontend, Gemini gets other topics — simplification, testing strategy, alternative approaches. Same the other way around if there's no backend. The goal is 6 different angles, maximum coverage.

**Step 3: Build the prompts**

Read `${CLAUDE_SKILL_DIR}/references/review-prompt-template.md` for the template. Each agent gets:
- The full plan and project context (they need the big picture)
- One specific topic to focus on

The tone is conversational: "Here's what I'm building. I especially want your take on X. How would you approach it? What doesn't sit right?"

**Step 4: Launch all 6 in parallel**

```
Skill("codex:rescue", args: "<prompt with topic 1>")
Skill("codex:rescue", args: "<prompt with topic 2>")
Skill("codex:rescue", args: "<prompt with topic 3>")
Skill("gemini:rescue", args: "<prompt with topic 4>")
Skill("gemini:rescue", args: "<prompt with topic 5>")
Skill("gemini:rescue", args: "<prompt with topic 6>")
```

## When Gemini isn't available

All 6 topics go to Codex. Adjust the topic selection — without Gemini, give Codex a broader range including the frontend/UX topics it would normally skip.

## When something fails

- One agent fails → continue without it, note it in the Pipeline Report
- Multiple agents fail → warn the user, continue with what you got
- All 6 fail (unlikely) → skip to Phase 7, note that external review wasn't available

## What comes next

Don't add anything from the responses yet. That's Phase 6's job — verify first, then incorporate.
