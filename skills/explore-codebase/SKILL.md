---
name: explore-codebase
description: Explore one path in a codebase and report what it contains and where, with a path on every finding. Covers what exists, not why it was built that way or how it works internally. Use when asked to explore a single directory, package, or file set, optionally with a focus. For a whole codebase, use `orchestrate-codebase-exploration`.
model: claude-haiku-5-5
effort: medium
---

# Explore codebase

Explore one path yourself. Don't spawn sub-agents.

The argument is the path (or paths) to explore, optionally followed by a focus. No path given: use the current directory.

## Rules

- Read-only. Don't edit, run builds, or install anything.
- Stay inside the given path. Note anything outside it only as a dependency.
- Facts only, each with a `path` or `path:line`. No reasoning about why, no walkthroughs of how.

## Report

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

Leave out empty sections.
