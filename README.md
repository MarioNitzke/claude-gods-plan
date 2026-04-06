![God's Plan](docs/banner.png)

# God's Plan

[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](https://github.com/MarioNitzke/claude-gods-plan)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Multi-model planning plugin for Claude Code. One command kicks off an 8-phase pipeline that brainstorms, analyzes, questions, drafts, reviews internally, sends to external models for feedback, verifies their suggestions, and presents a final plan with a full audit trail.

## Why?

One model planning alone misses things. God's Plan fixes that by getting multiple perspectives before any code is written:

- **Internal review** — 3-8 specialized subagents check the plan from different angles
- **External review** — Codex and Gemini each get targeted questions about the parts that matter most
- **Fact-checked** — every external suggestion is verified against the actual codebase before it's added
- **Full audit trail** — you see what was suggested, accepted, rejected, and why

## Requirements

| Dependency | Purpose | Required? |
|---|---|---|
| [Claude Code](https://claude.com/claude-code) | Plugin host | Required |
| [Codex plugin](https://github.com/openai/codex-plugin-cc) | External review (Phase 5) | Required |
| [Gemini plugin](https://github.com/MarioNitzke/gemini-plugin-cc) | External review (Phase 5) | Recommended |
| [Serena plugin](https://github.com/oraios/serena) | Semantic code analysis (Phase 1, 6) | Recommended |

**Works great with just Claude + Codex.** Add Gemini for broader coverage — it's especially good at frontend and UX feedback. Without it, all 6 external reviews go to Codex.

I personally use Serena for deeper code analysis (symbol resolution, reference mapping, semantic understanding). Without it, the plugin falls back to Glob/Grep — still works, just less depth. Set it up if you want the full experience, or skip it if you prefer to keep things simple.

## Installation

### From marketplace
```bash
claude plugin marketplace add MarioNitzke/claude-gods-plan
claude plugin install gods-plan@MarioNitzke-gods-plan
```

### Install Codex (required)
```bash
claude plugin marketplace add openai/codex-plugin-cc
claude plugin install codex@openai-codex
```

### Optional: Gemini
```bash
claude plugin marketplace add MarioNitzke/gemini-plugin-cc
claude plugin install gemini@MarioNitzke-gemini
```

### Optional: Serena
Follow install instructions at [github.com/oraios/serena](https://github.com/oraios/serena)

### From source
```bash
git clone https://github.com/MarioNitzke/claude-gods-plan.git
claude --plugin-dir ./claude-gods-plan
```

## Usage

Best used in plan mode:

```
/gods-plan Add a notification system for booking confirmations
```

You interact during three phases — brainstorming (Phase 0), confession (Phase 2), and presentation (Phase 7). Everything else runs autonomously.

### Example: what happens when you run it

```
Phase 0  You describe the idea, Claude proposes 2-3 approaches, you pick one
Phase 1  Claude analyzes the codebase (3 parallel subagents)
Phase 2  Claude asks you 8+ specific questions about requirements
Phase 3  Draft plan is written internally
Phase 4  3-8 internal reviewers check it from different angles
Phase 5  3 Codex + 3 Gemini agents each review a specific aspect
Phase 6  Claude verifies every external suggestion against the code
Phase 7  You see the final plan with full audit trail → approve or adjust
```

After approval, God's Plan executes the plan in batches of 3 tasks, checking in after each batch.

## Pipeline

```
Phase 0  BRAINSTORMING     Explore intent, propose approaches, get direction
Phase 1  ANALYSIS          Deep codebase analysis (Serena or Glob/Grep)
Phase 2  CONFESSION        Requirements gathering (minimum 8 questions)
Phase 3  DRAFT             Write bite-sized implementation plan
Phase 4  REPLAN            Internal review from 3-8 angles
Phase 5  FRIENDS           3x Codex + 3x Gemini with targeted questions
Phase 6  VERIFICATION      Fact-check external claims against code
Phase 7  PRESENTATION      Final plan with audit trail → approval
```

## How Phase 5 works

Claude picks 6 topics from the plan that most deserve outside eyes. Topics are split by model strengths:

- **Codex** gets backend-focused topics: architecture, data, security, error handling
- **Gemini** gets frontend-focused topics: UI/UX, user flows, components, accessibility

Each model gets the full plan for context, plus a specific question: *"Here's the plan. I especially want your take on how I handle the retry logic — how would you approach it? What doesn't sit right?"*

All 6 run in parallel. If Gemini isn't installed, all topics go to Codex.

## Privacy

Phase 5 sends the plan and a project context summary to external models. No raw source code is sent — only the plan text and project conventions. If Codex/Gemini plugins aren't installed, no data leaves Claude.

## Contributing

Issues and PRs welcome at [github.com/MarioNitzke/claude-gods-plan](https://github.com/MarioNitzke/claude-gods-plan).

## License

MIT
