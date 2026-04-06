# Phase 1: Analysis

Deep dive into the codebase to understand what you're working with. The user doesn't need to do anything here — this is all background research.

## How to run it

Launch 3 subagents in parallel (Agent tool, subagent_type: "Explore"):

**Agent 1 — Architecture overview:**
- Use `get_symbols_overview` on key files relevant to the task
- Understand classes, interfaces, handlers without reading full files
- Check CLAUDE.md (if it exists) for project conventions

**Agent 2 — Impact analysis:**
- Use `find_symbol` to locate handlers/components related to the task
- Use `find_referencing_symbols` to map what depends on what
- Figure out what would be affected by changes

**Agent 3 — Pattern discovery:**
- Use `search_for_pattern` to find existing patterns worth reusing
- Look for similar features already in the codebase

Each subagent returns a summary to keep the main context clean.

## About Serena

Serena is recommended for this phase — it gives you semantic understanding of the code (symbols, references, relationships) which is much richer than text search. But it's not required. If Serena isn't available, fall back to Glob/Grep/Read. The analysis won't be as deep, but it'll work.

## Result

Internal notes about: code state, architecture, dependencies, reusable patterns. These feed into Phase 3 (Draft). Don't present anything to the user.
