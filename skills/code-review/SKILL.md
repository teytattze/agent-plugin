---
name: code-review
description: Review a diff for correctness bugs, security holes, the repo's own conventions, over-engineering, and docs it makes stale. Use when asked to review changes, a branch, a PR, or a commit.
---

# Code review

## Target

What to review, unless the user names something else: the current branch's changes since it left the default branch, plus uncommitted changes (`git diff $(git merge-base HEAD origin/HEAD)`). Read each changed file in full, not just the diff hunks.

## Checks

Read the `project-docs` skill first. It says which file owns which rules.

You orchestrate. Spawn every sub-agent below in a single message so they run in parallel. Give each one the diff command from Target, the changed files it covers, and its check's bullets. Each reads its files in full and reports in the Output line format.

**Correctness** (1 sub-agent)
- Flag logic errors, unhandled edge cases or errors, and callers the change breaks. Name the input that triggers each.

**Security** (1 sub-agent)
- Flag injection, missing auth, unvalidated untrusted input, and leaked secrets. Name how each is exploited.

**Conventions** (you, plus 1 sub-agent per topic)
- Read the root `AGENTS.md` and every nested `AGENTS.md` on the path to each changed file. Check the change against their Rule sections yourself.
- Read the root `doc/engineering/AGENTS.md` and, in a monorepo, the one in each changed file's app, if they exist. A topic applies when a changed file matches a glob in its Paths column.
- Give each topic's sub-agent its rule file (plus the app's file of the same name, if there is one, which wins inside that app) and only the changed files that match its globs. It checks that topic's rules only.
- Flag only real violations of a written rule, and quote the rule. Taste is not a finding. Drop findings that don't quote a rule.

**Simplicity** (1 sub-agent)
- Flag code that could be deleted or done with less: things that already exist in the codebase, the standard library, or an installed dependency; abstractions with a single user; config for values that never change; dead code; duplication.
- For each finding, name what replaces it.

**Doc drift** (1 sub-agent)
- Read the `project-docs` skill and every README.md, AGENTS.md, PRODUCT.md, and `doc/engineering/` file the change could affect.
- Flag changes that make README.md, AGENTS.md, PRODUCT.md, or a `doc/engineering/` file wrong: setup steps, commands, paths in Structure, rules the code now breaks, a changed Solution.
- Flag changed docs that break the `project-docs` convention.
- Name the file and section that needs updating.

## Output

Merge your findings with the sub-agents' and drop duplicates. One line per finding, grouped under Correctness, Security, Conventions, Simplicity, and Doc drift. Write each as `file:line`, then the problem, then the fix. Leave out empty groups. If there are no findings, say so in one line.

Report only. Don't edit anything unless the user asks.
