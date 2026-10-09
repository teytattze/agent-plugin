---
name: explore-codebase
description: Map what a codebase contains by fanning out Haiku sub-agents to explore it, then write a findings report. Covers what exists and where, not why it was built that way or how it works internally. Use when asked to explore, map, survey, or get oriented in a codebase, optionally with a focus area given as the argument.
---

# Explore codebase

You orchestrate. Sub-agents explore. You write the report.

Answer **what** exists and **where**: modules, entry points, commands, data models, config, dependencies, tests. Leave out **why** it was built that way and **how** it works step by step.

## Focus

The skill's argument is the focus. Every sub-agent gets it and reports anything touching it in more detail. With no argument, explore the whole codebase evenly.

## Plan

- List the top-level layout yourself (`git ls-files`, or the directory tree), and read the root `README.md` and `AGENTS.md` if present.
- Split the codebase into 3–6 independent areas: by top-level directory, package, or layer. Weight the split toward the focus.

## Spawn

- Spawn one sub-agent per area, all in a single message so they run in parallel. Use `model: "haiku"` and `effort: "medium"`.
- Give each one only: its area's paths, the focus, and these instructions:
  - Read-only. Don't edit, run builds, or install anything.
  - Report what exists in the area: each notable file or directory with `path` and one line on what it is. Name entry points, public APIs, commands, data models, config, external dependencies, and tests.
  - Report anything matching the focus in more detail.
  - Facts only, each with a `path` or `path:line`. No reasoning about why, no walkthroughs of how.
- If an area comes back thin or contradicts another, spawn one more sub-agent for that gap. Don't loop more than once.

## Report

Merge the sub-agent results. Drop duplicates and anything without a path. Don't add findings you didn't see reported or verify yourself.

```
# Codebase: <name>
Focus: <argument, or "none">

## Overview
<2–3 lines: what the project is, language, stack>

## Layout
- `path/` — what it is

## Entry points
## Key components
## Data & config
## Dependencies
## Tests

## Focus: <argument>
<findings on the focus, each with a path>

## Gaps
<areas not covered or unclear>
```

Leave out empty sections. Print the report in the conversation. Write it to a file only if the user names one.
