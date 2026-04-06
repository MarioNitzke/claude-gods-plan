# Phase 3: Draft Plan

Write the implementation plan. This is internal — the user doesn't see it yet. Don't announce it.

## What kind of plan

A plan that someone with zero codebase context could follow. Every step spells out: which files to touch, the actual code, how to test it, what to expect. Broken into small, manageable tasks.

Assume the person executing is a solid developer but new to the codebase and the problem domain. Assume they could use guidance on good test design.

Principles: DRY, YAGNI, TDD, commit often.

## How small is "bite-sized"?

Each step = one action, roughly 2-5 minutes:
- "Write the failing test" — a step
- "Run it and confirm it fails" — a step
- "Write the minimal code to make it pass" — a step
- "Run the tests and confirm they pass" — a step
- "Commit" — a step

## Plan header

Every plan starts with:

~~~markdown
# [Feature Name] Implementation Plan (God's Plan)

> **For Claude:** Run /gods-plan on this plan file to execute it task-by-task.

**Goal:** [One sentence]
**Architecture:** [2-3 sentences]
**Tech Stack:** [Key technologies and libraries]

---
~~~

## Task structure

~~~markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Step 1: Write the failing test**
```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

**Step 2: Run it and confirm it fails**
Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

**Step 3: Write the minimal implementation**
```python
def function(input):
    return expected
```

**Step 4: Run it and confirm it passes**
Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS
~~~

## Keep in mind
- Always use exact file paths
- Include the actual code, not just "add validation"
- Specify exact commands with expected output
- Build the plan from: code analysis (Phase 1), user answers (Phase 2), project conventions (CLAUDE.md)
- Write the draft to the active plan file using Edit tool
- **Don't show it to the user. Move straight to Phase 4.**
