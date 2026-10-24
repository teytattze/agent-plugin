---
name: pr
description: Write a pull request body with Summary, Evidence, and Merge Danger sections. Use when opening a PR, writing or updating a PR description, or when asked for a PR body.
---

# PR

Fill this template. No preamble, brief prose. Use the domain terms from `GLOSSARY.md` if the repo has one.

```markdown
## Summary

<the smallest diagram, tree, or diff sketch that makes the point>

## Evidence

- **Before:** <screenshot, output, or failing test run>
  **After:** <screenshot, output, or passing test run>

## Merge Danger

**Door:** <one-way or two-way>

<optional: why>

**Blast Radius:** <one word>

<optional: what could break>
```

## Summary

Pick the smallest view that makes the point. Usually one, sometimes a few, never all.

| Point | View |
|---|---|
| Logic or an algorithm | Pseudocode |
| Runtime control flow | Call tree |
| UI structure | Component tree, with the state and module boundaries that matter |
| File responsibility or a broad refactor | Shallow file tree, one comment per entry |
| Interaction or data flow between parts | Mermaid diagram |
| A change to a shape that already exists | `diff` of that view |
| Mostly new code, or a copyable target shape | The whole block |

A `diff` keeps the view's shape and marks what changes:

```diff
 submitForm
   createSession
     persistPrompt
+    expandSkillMention
     launchAgent
```

Put each view next to the line it supports. Keep only the calls, files, props, and states needed to make the point.

## Evidence

Show it works, before and after.
- Best: screenshots, when the change is visual and the environment can take them.
- Next: test results or console output. Show the test that failed and now passes, as pseudocode.

## Merge Danger

- **Door**: two-way if a revert undoes it cheaply; one-way if it's destructive or hard to reverse.
- **Blast Radius**: everything the change could affect, such as consumers, layout shift, or mobile.
