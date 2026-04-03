# God's Plan

[![Version](https://img.shields.io/badge/version-1.1.0-blue.svg)](https://github.com/MarioNitzke/claude-gods-plan)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Multi-model ultra-planning plugin for Claude Code. Orchestrates Claude, Gemini, and Codex through an 8-phase pipeline to create thoroughly vetted implementation plans.

![Planning Workflow](docs/workflow.png)

## Why?

Single-model planning misses things. A plan that looks good to one AI will have blind spots another would catch. God's Plan fixes this by:

- **Multi-angle internal review** — 3-8 specialized subagents check the plan from different perspectives before any external model sees it
- **External review** — Gemini and Codex independently review the plan, bringing different strengths
- **Fact-checked suggestions** — Claude verifies every external claim against the actual codebase before accepting it
- **Full audit trail** — you see what was suggested, what was accepted, what was rejected, and why

## Requirements

| Dependency | Purpose | Required? |
|---|---|---|
| [Claude Code](https://claude.com/claude-code) | Plugin host | Required |
| [Serena plugin](https://github.com/oraios/serena) | Semantic code analysis (Phase 1, 6) | Required |
| [Gemini plugin](https://github.com/MarioNitzke/gemini-plugin-cc) | External review (Phase 5) | Strongly recommended |
| [Codex plugin](https://github.com/openai/codex-plugin-cc) | External review (Phase 5) | Strongly recommended |

Serena is required for deep semantic code analysis. Gemini and Codex are strongly recommended — without them Phase 5 (external review) is skipped, but the plan still goes through internal replan validation.

## Installation

### From GitHub
```bash
git clone https://github.com/MarioNitzke/claude-gods-plan.git
claude --plugin-dir ./claude-gods-plan
```

### From marketplace (when available)
```bash
/plugin install gods-plan
```

## Usage

```
/gods-plan Add a notification system for booking confirmations
```

Claude runs through all 8 phases automatically. You interact during Phase 0 (brainstorming), Phase 2 (confession), and Phase 7 (presentation). Everything else is autonomous.

## Pipeline

```
Phase 0  BRAINSTORMING     Explore intent, propose 2-3 approaches, get design approval
Phase 1  ANALYSIS          Deep codebase analysis via Serena semantic tools (3 parallel subagents)
Phase 2  CONFESSION        Systematic requirements gathering (minimum 8 questions)
Phase 3  DRAFT PLAN        Write bite-sized implementation plan (internal, user does not see)
Phase 4  REPLAN            Parallel subagent review from 3-8 angles
Phase 5  FRIENDS           Send plan to Gemini + Codex for independent external review
Phase 6  VERIFICATION      Fact-check external claims against actual codebase via Serena
Phase 7  PRESENTATION      Present final plan with full audit trail
```

## Bundled skills

Each sub-skill can also be invoked independently:

| Skill | Phase | Purpose |
|---|---|---|
| `gods-plan:gods-plan` | — | Main orchestrator (8 phases) |
| `gods-plan:brainstorming` | 0 | Explore intent, propose approaches |
| `gods-plan:writing-plans` | 3 | Write bite-sized implementation plans |
| `gods-plan:replan` | 4 | Multi-angle parallel plan review |
| `gods-plan:executing-plans` | post | Execute approved plan with batch checkpoints |
| `gods-plan:finishing-branch` | post | Merge, PR, keep, or discard after implementation |

## Privacy considerations

Phase 5 sends your plan and project context to external models (Gemini, Codex) for review. If you work with sensitive code, consider what information you include in `{PROJECT_CONTEXT}`. The plugin does not send raw source code — only the plan text and project conventions summary.

If Gemini and Codex plugins are not installed, Phase 5 is skipped entirely and no data leaves Claude.

## Contributing

Issues and PRs welcome at [github.com/MarioNitzke/claude-gods-plan](https://github.com/MarioNitzke/claude-gods-plan).

## License

MIT
