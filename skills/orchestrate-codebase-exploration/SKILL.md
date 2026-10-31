---
name: orchestrate-codebase-exploration
description: Map a whole codebase by splitting it into areas and running the `explore-codebase` skill on each in parallel sub-agents, then merging their reports. Use when asked to explore, map, survey, or get oriented in a whole codebase, optionally with a focus area given as the argument.
model: claude-sonnet-5-5
effort: high
---

# Orchestrate codebase exploration

You orchestrate. Sub-agents run `agent-plugin:explore-codebase`, one area each. You merge.

## Focus

The skill's argument is the focus. Every sub-agent gets it. With no argument, explore the whole codebase evenly.

## Plan

- Read the `agent-plugin:project-docs` skill. It says where the project docs live and what each one holds.
- List the top-level layout yourself (`git ls-files`, or the directory tree), and read the root `README.md`, `AGENTS.md`, and `doc/engineering/AGENTS.md` if present.
- Split the codebase into 3–6 independent areas: by top-level directory, package, or layer. Weight the split toward the focus.

## Spawn

- Spawn one sub-agent per area, all in a single message so they run in parallel.
- Tell each one only: run the `agent-plugin:explore-codebase` skill with its area's paths and the focus as the argument, and return the skill's report.
- If an area comes back thin or contradicts another, spawn one more sub-agent for that gap. Don't loop more than once.

## Merge

Print in the conversation, or to a file only if the user names one:

1. `# Codebase: <name>`, the focus, and 2–3 lines on what the project is, its language, and stack.
2. Each area's report as returned, minus duplicates across areas.
3. `## Gaps`: areas not covered or unclear. Leave it out if there are none.

Don't add findings you didn't see reported or verify yourself.
