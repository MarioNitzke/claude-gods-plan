# Phase 6: Verification

Codex and Gemini gave you feedback. Now check whether it's actually true before adding anything to the plan. They don't have access to your codebase — you do.

## Step 1: Sort the claims

Go through each piece of feedback and categorize it:

- **Factual / checkable** — "file X doesn't exist", "pattern Y is used elsewhere", "this constraint applies" → send to a verification agent
- **Opinion / suggestion** — "you should use caching", "consider a different architecture", "this could be simpler" → mark as controversial for the user to decide in Phase 7

## Step 2: Check the facts

Group claims by source (Codex, Gemini). Spawn 1 verification agent per 2-3 claims (Agent tool, subagent_type: "Explore"):

- "Codex says file X doesn't exist" → `find_file` + `find_symbol` to check
- "Gemini suggests using pattern Y" → `find_symbol` + `find_referencing_symbols` — does Y exist? Is it compatible?
- "Codex flags missing edge case Z" → `get_symbols_overview` + `search_for_pattern` — can Z actually happen?

If Serena is available, use it. If not, fall back to Glob/Grep/Read.

Each agent reports whether the claim is **VERIFIED** or **UNVERIFIED**.

## Step 3: Decide what to do with each claim

- **Verified + worth adding** — add it to the plan, tag with "[Codex]" or "[Gemini]"
- **Verified but not worth it** — true, but irrelevant or over-engineered. Skip it with a note.
- **Unverified** — couldn't confirm it. Discard with an explanation.
- **Controversial** — valid but debatable. Flag for the user in Phase 7.
