---
name: github-pr-iteration
description: Handle GitHub PR iteration loops using the gh CLI. Use when asked to address review comments, fix CI on an existing PR, apply requested changes on the same branch, push follow-up commits, or close out review threads with minimal replies.
---

# GitHub PR Iteration

## Overview

Use this skill to reliably execute follow-up work on an existing PR without creating branch/PR churn. Keep changes focused, test-backed, and easy to review.

## Workflow

1. Identify the active PR and current branch.
2. Pull latest remote changes first (`git fetch`, `git pull --rebase` when needed).
3. Collect latest review comments and CI failures.
4. Critically validate comments before changing code (especially bot/AI comments).
5. Implement only applicable fixes on the same branch.
6. Add or update lean tests for changed behavior.
7. Run targeted checks locally first, then broader checks as needed.
8. Commit with a plain human message and push.
9. If publishing PR work, explicitly create the PR (do not stop at the suggested URL output).
10. Verify the PR exists by checking the returned PR URL/number (or `gh pr view`).
11. Reply on addressed review comments with minimal wording (`fixed` / `done`).
12. Summarize what changed, what was tested, and any remaining risk.

## Command Patterns

```bash
# PR + checks
 gh pr create --base <base-branch> --head <branch> --title "<title>" --body "<body>"
 gh pr view --repo <owner/repo> --json number,url,state

 gh pr checks <pr-number> --repo <owner/repo>
 gh run list --repo <owner/repo> --branch <branch> --limit 10
 gh run view <run-id> --repo <owner/repo> --log-failed

# Review comments
 gh api repos/<owner>/<repo>/pulls/<pr-number>/comments --paginate

# Safe branch sync before work
 git fetch --all --prune
 git pull --rebase
```

## Review Comment Policy

- Treat review comments as hypotheses, not truth.
- Confirm with code context before applying changes.
- If a comment is outdated or no longer applicable, skip it.
- Keep thread replies minimal:
  - use `fixed` or `done` for applicable addressed comments
  - otherwise do not add noisy replies

## CI Triage Heuristics

- `npm ci` lock mismatch: regenerate lockfile from latest branch state and re-run `npm ci`.
- Type/lint/test failures: run the same command locally, patch smallest root cause, rerun.
- If CI and local differ, check Node/npm versions and workflow environment assumptions.

## Guardrails

- Stay on the same branch for the same active PR.
- Do not create a new PR for follow-up iteration.
- Do not commit planning/design markdown files unless explicitly requested.
- Prefer minimal diffs that directly resolve comments or failures.
- Always report what was run locally before pushing.
