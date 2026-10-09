---
name: code-review
description: Review a diff against the repo's own conventions, for over-engineering, and for docs it makes stale. Use when asked to review changes, a branch, a PR, or a commit for conventions, simplicity, or doc drift. Does not hunt correctness bugs; the built-in /code-review does that.
---

# Code review

## Target

What to review, unless the user names something else: the current branch's changes since it left the default branch, plus uncommitted changes (`git diff $(git merge-base HEAD origin/HEAD)`). Read each changed file in full, not just the diff hunks.

## Checks

**Conventions**
- Read the root `AGENTS.md` and every nested `AGENTS.md` on the path to each changed file. Check the change against their Rule sections.
- Flag only real violations of a written rule, and quote the rule. Taste is not a finding.

**Simplicity**
- Flag code that could be deleted or done with less: things that already exist in the codebase, the standard library, or an installed dependency; abstractions with a single user; config for values that never change; dead code; duplication.
- For each finding, name what replaces it.

**Doc drift**
- Flag changes that make README.md, AGENTS.md, or PRODUCT.md wrong: setup steps, commands, paths in Structure, rules the code now breaks, a changed Solution.
- Flag changed docs that break the `project-docs` convention.
- Name the file and section that needs updating.

## Output

One line per finding, grouped under Conventions, Simplicity, and Doc drift. Write each as `file:line`, then the problem, then the fix. Leave out empty groups. If there are no findings, say so in one line.

Report only. Don't edit anything unless the user asks.
