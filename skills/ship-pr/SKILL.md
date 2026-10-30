---
name: ship-pr
description: "Ship the current changes as a GitHub PR: branch, commit, push, and open or update the PR. Use when asked to ship, open, raise, or create a PR."
---

# Ship PR

1. On the default branch? Create a branch first (`claude/<short-slug>`).
2. Commit uncommitted changes in Conventional Commits style (`feat:`, `fix:`, `chore:`), matching `git log`. Stage files by name, never secrets.
3. Push with `git push -u origin HEAD`.
4. Write the body with the `write-pr-body` skill, covering the whole branch (`git diff $(git merge-base HEAD origin/HEAD)`), not just the last commit.
5. No PR for the branch yet (`gh pr view`)? `gh pr create --base <default> --title "<commit-style title>" --body-file <file>`. One exists? `gh pr edit --body-file <file>`.
6. Reply with the PR URL.

Never force-push, merge, or skip hooks unless asked.
