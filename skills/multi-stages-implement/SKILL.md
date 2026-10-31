---
name: multi-stages-implement
description: Implement a large feature in stages: a fresh sub-agent designs the architecture, a 3-agent panel reviews it until all pass, then the work runs as waves of parallel tasks, each task through the `implement` skill and each wave through `code-review`. Use when a request is too big for one `implement` run, or when the user says "multi-stage", "waves", or "plan then implement".
---

# Multi-stages implement

You orchestrate. Sub-agents design, review, and implement. Every step runs in a new, fresh sub-agent; never reuse or message an earlier one.

## Before starting

- Run the `agent-plugin:orchestrate-codebase-exploration` skill with the request as its focus.
- Read the root `AGENTS.md` and any nested ones on the path.
- Find the test command. Run the suite once. If it's already red, stop and tell the user.

## Design

**ARCHITECTURE** (sub-agent)
- Give it the request, the explore report, and the `AGENTS.md` paths. Ask for a design that follows the codebase's existing conventions and reuses what already exists: the files to add or change, and how they fit together.

**PANEL** (3 sub-agents, in parallel)
- Give each the request and the design. Ask for a verdict, `PASS` or `FAIL`, with findings for a `FAIL`. No edits.
- Any `FAIL`: spawn a new ARCHITECTURE sub-agent with the design and all findings, then a new panel. Loop until all three pass.

## Waves

Break the passed design into a sequence of waves. Each wave is a set of tasks that:
- can run in parallel: no task needs another task in the same wave, and no two tasks touch the same files;
- depend only on earlier waves.

For each wave, in order:

**IMPLEMENT** (1 sub-agent per task, in parallel)
- Give each the request, the design, its task, the test command, and the relevant file paths. Have it run the `agent-plugin:implement` skill on its task and report the files it changed and the suite output.
- When all return, run the suite yourself. If it's red, send the failure to a new sub-agent for the task that caused it.

**REVIEW** (sub-agent)
- Have it run the `agent-plugin:code-review` skill on the wave's changes, then address every finding and report what it changed.
- Spawn a new review sub-agent after each fix. Loop until it reports no findings, then start the next wave.

## Rules

- Stop and ask the user when a finding needs a product decision, or after the same loop fails three times.
- Commit only if the user asked for commits.

## Output

When every wave is done: one line per wave with its tasks and the tests added, then any findings left unfixed and why.
