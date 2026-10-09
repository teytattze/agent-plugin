---
name: implement
description: Implement a feature or bug fix test-first with a RED → GREEN → REVIEW → REFACTOR → REVIEW cycle, each phase run by a fresh sub-agent. Use when asked to implement, build, add, or fix behavior in a codebase that has tests or should have them, or when the user says "TDD", "test-first", or "red-green-refactor".
---

# Implement

Write the test, make it pass, get it reviewed, clean it up, get it reviewed again. You plan and orchestrate. Every phase runs in a new, fresh sub-agent, so no phase inherits the reasoning of the one before it.

## Before starting

- Run the `explore-codebase` skill with the request as its focus. Use its report to find the files, tests, and existing code the slices touch.
- Read the request and the code it touches. Read the root `AGENTS.md` and any nested ones on the path.
- Find the test command. Run the suite once. If it's already red, stop and tell the user.
- Split the work into slices: one observable behavior each. Run the cycle once per slice.

## Spawning a phase

- Spawn a new sub-agent for every phase, including every retry. Never reuse or message an earlier one.
- Give it only what it needs: the user's request, the slice, the test command, the relevant file paths, and, on a retry, the findings it must address. Don't pass your own reasoning or earlier agents' explanations.
- Tell it to report the files it changed and the suite output.
- After it returns, run the suite yourself and check the result matches what the phase requires before moving on.

## Cycle

**RED** (sub-agent)
- Write the smallest test that pins the slice's behavior from the outside. For a bug, reproduce it. Don't touch production code.
- The test must fail on an assertion about the missing behavior, not on an import, syntax, or setup error.

**GREEN** (sub-agent)
- Write the least code that makes the test pass. No extra features, no cleanup. Don't touch the tests.
- The whole suite must be green.

**REVIEW** (sub-agent, correctness)
- Give it the request, the slice, and the diff. Ask whether the tests cover what the user asked for, including edge cases and errors, and whether the code is correct. Findings only, no edits.
- Missing or wrong test: back to RED. Wrong code: back to GREEN. Clear: continue.

**REFACTOR** (sub-agent)
- Improve names, remove duplication, reuse what the codebase already has. Don't change behavior and don't weaken the tests.
- The whole suite must stay green.

**REVIEW** (sub-agent, quality)
- Have it run the `code-review` skill on the diff. Findings only, no edits.
- Real findings: back to REFACTOR with them. Clear: start the next slice.

## Rules

- No phase edits a test to make it pass. If a test is wrong, send it back to RED with the reason.
- Stop and ask the user when a finding needs a product decision, or after the same review fails twice.
- Commit only if the user asked for commits.

## Output

When every slice is done: one line per slice with the tests added, then any findings left unfixed and why.
