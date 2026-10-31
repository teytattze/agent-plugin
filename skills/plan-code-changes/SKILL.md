---
name: plan-code-changes
description: Plan a code change before writing it: map the code it touches, then write a plan of the files to change, the ordered steps, the tests, and the open questions. Read-only. Use when asked to plan, scope, or design a change, or "what would it take to" do something, before implementing.
---

# Plan changes

Plan the change. Don't make it. The argument is the request.

## Before starting

- Run the `agent-plugin:explore-codebase` skill with the request as its focus.
- Read the root `AGENTS.md` and any nested ones on the path.
- Read the code the change touches yourself. Trace the real flow end to end, and find every caller of anything you plan to change.

## Rules

- Read-only. Don't edit, commit, or install anything.
- Reuse what the codebase already has before planning anything new. Follow its existing conventions.
- Each step is one observable behavior, small enough for one `agent-plugin:implement` slice, in dependency order.
- Every claim about the code has a `path` or `path:line`.
- If an ambiguity changes the plan, ask the user before writing it. Otherwise pick the obvious default and note it under Decisions.

## Plan

Print in the conversation, or to a file only if the user names one:

```
# Plan: <request in one line>

## Goal
<what changes for the user, 1–3 lines>

## Files
- `path` — add | change | delete: what and why

## Steps
1. <behavior> — files, test that proves it

## Decisions
<defaults picked, and alternatives rejected in one line each>

## Risks
<callers, migrations, or behavior that could break>

## Open questions
<anything only the user can decide>

## Next
`agent-plugin:implement` for a few steps, `agent-plugin:multi-stages-implement` if the steps split into parallel waves.
```

Leave out empty sections.
