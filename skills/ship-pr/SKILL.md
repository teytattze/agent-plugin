---
name: ship-pr
description: "Ship the current changes as a GitHub PR: branch, commit, push, and open or update the PR. Use when asked to ship, open, raise, or create a PR."
---

# Ship PR

1. On the default branch? Create a branch first (`agent-plugin/<short-slug>`).
2. Commit uncommitted changes in Conventional Commits style (`feat:`, `fix:`, `chore:`), matching `git log`. Stage files by name, never secrets.
3. Bump the version if needed. Skip if the repo has no version file (`.claude-plugin/plugin.json`, `package.json`, `pyproject.toml`, `Cargo.toml`, ...), the branch already changes it, or it changes no shipped behavior (docs, tests, CI only). Otherwise use semver from the branch's commits: breaking → major (minor while below 1.0), `feat:` → minor, `fix:` → patch. Commit it on its own as `chore: bump version to <x.y.z>`.
4. Rebase onto the default branch (`git fetch origin && git rebase origin/<default>`), then push with `git push -u origin HEAD`. Already pushed before the rebase? Use `--force-with-lease`.
5. Write the body with the `write-pr-body` skill, covering the whole branch (`git diff $(git merge-base HEAD origin/HEAD)`), not just the last commit.
6. No PR for the branch yet (`gh pr view`)? `gh pr create --base <default> --title "<commit-style title>" --body-file <file>`. One exists? `gh pr edit --body-file <file>`.
7. Reply with the PR URL.

Never plain force-push, merge, or skip hooks unless asked.
