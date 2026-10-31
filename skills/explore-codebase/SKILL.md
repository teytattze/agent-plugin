---
name: explore-codebase
description: Map what a codebase contains by splitting it into areas, exploring each in a parallel Haiku sub-agent, and merging their reports, with a path on every finding. Covers what exists and where, not why it was built that way or how it works internally. Use when asked to explore, map, survey, or get oriented in a codebase, optionally with a focus area given as the argument.
model: claude-sonnet-5-5
effort: high
---

# Explore codebase

You orchestrate. Sub-agents explore one area each. You merge.

## Focus

The skill's argument is the focus. Every sub-agent gets it. With no argument, explore the whole codebase evenly.

## Plan

- Read the `agent-plugin:project-docs` skill. It says where the project docs live and what each one holds.
- List the top-level layout yourself (`git ls-files`, or the directory tree), and read the root `README.md`, `AGENTS.md`, and `doc/engineering/AGENTS.md` if present.
- Split the codebase into 3–6 independent areas: by top-level directory, package, or layer. Weight the split toward the focus.

## Spawn

- Spawn one sub-agent per area, all in a single message so they run in parallel. Use `model: "haiku"` and `effort: "medium"`.
- Give each one only: its area's paths, the focus, and these instructions:
  - Explore only your paths, yourself. Don't spawn sub-agents. Note anything outside your paths only as a dependency.
  - Read-only. Don't edit, run builds, or install anything.
  - Facts only, each with a `path` or `path:line`. No reasoning about why, no walkthroughs of how.
  - Return this report, leaving out empty sections:

    ```
    ## Area: <path>

    - `path` — one line on what it is

    ### Entry points
    ### Public APIs & commands
    ### Data models & config
    ### External dependencies
    ### Tests
    ### Docs
    <the area's `AGENTS.md` and `doc/engineering/` files>

    ### Focus: <focus>
    <anything touching the focus, in more detail>
    ```
- If an area comes back thin or contradicts another, spawn one more sub-agent for that gap. Don't loop more than once.

## Merge

Print in the conversation, or to a file only if the user names one:

1. `# Codebase: <name>`, the focus, and 2–3 lines on what the project is, its language, and stack.
2. Each area's report as returned, minus duplicates across areas.
3. `## Gaps`: areas not covered or unclear. Leave it out if there are none.

Don't add findings you didn't see reported or verify yourself.
