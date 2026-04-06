# Phase 2: Confession

This is where you find out what you don't know. Don't skip it, don't rush it. The plan is only as good as the requirements behind it.

**Don't move forward until confession is complete.**

## How to run it

Ask questions via AskUserQuestion, 2-4 at a time. One by one is painfully slow. Be specific, not vague. Offer options (A/B/C) when it makes sense. After each round of answers, follow up with the next batch.

You need at least **8 questions total**. Wrap up when you can honestly say "I have enough to write the plan."

## Question areas

### Always ask about these:

**Goal and vision**
- "What does success look like? Walk me through the user flow from start to finish."
- "Is this replacing something existing or entirely new?"
- "What's the single most important outcome?"

**Scope**
- "Should this also cover [related concern X] or is that separate?"
- "What's the minimum viable version vs. the full vision?"

**Users**
- "Which user roles interact with this? (e.g., Admin, Manager, Customer)"
- "Are there different permission levels or views per role?"

**Business rules**
- "What happens when [edge case]? Fail silently, show an error, or prevent the action?"
- "Are there state transitions? What triggers them?"
- "Any time-based rules (expiration, cooldowns, scheduling)?"

**Constraints**
- "Does this need to be backward-compatible with existing API consumers?"
- "Any hard performance requirements (response time, concurrent users)?"

### Ask when relevant:

**Technical context** — "Are there existing services or handlers we should reuse or extend?"

**Edge cases** — "What should happen when the external service is down?"

**Priority** — "If we can only ship one part this week, which part?"

**Inspiration** — "Is there a similar feature in the codebase we should model this after?"

**Security** — "Who should NOT be able to access this? What authorization checks are needed?"

**UI/UX** — "Any design requirements or existing UI patterns to follow?"

**Data** — "What are the input/output formats? Any validation rules?"

## Rules
- 2-4 questions per round, not one by one
- Be specific — "What happens when payment fails mid-checkout?" not "Any edge cases?"
- Suggest options where you can
- Don't skip an area because you think you already know — confession reveals what you don't know
- Minimum 8 questions, this isn't negotiable
