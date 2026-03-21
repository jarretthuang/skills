---
name: github-pr-iteration
description: Handle GitHub PR workflows using the gh CLI for both new PR creation and existing PR iteration. Use when asked to open a PR, address review comments, fix CI, apply requested changes on the same branch, push follow-up commits, or close out review threads with minimal replies.
---

# GitHub PR Iteration

## Overview

Use this skill to reliably run both PR creation and follow-up iteration without branch/PR churn. Keep changes focused, test-backed, and easy to review.

For solution design and implementation quality, pair this with the `coding-principles` skill.

## Workflow

1. Identify whether this is **new PR creation** or **existing PR iteration**.
2. If this is iteration work, identify the target PR explicitly before checking state. Prefer a PR number or PR URL when available. Use head branch and issue linkage only as fallback hints to find the right PR, not as equally reliable selectors. Do not rely on bare `gh pr view` / `gh pr list` to discover the target from whatever branch happens to be checked out. After the target branch or PR context is verified, bare `gh pr view` is fine.
3. Check issue state too: for GitHub iteration work, prefer working from a GitHub issue unless the user, repo workflow, or task context makes that unnecessary (for example, "quick fix, don't need an issue number", issues are disabled, or the team uses PR-only/external-ticket workflows). If an active issue does not exist and creating one fits the repo workflow, create one with a reasonable summary and a concise bullet-list description.
4. Pull latest remote changes first (`git fetch`, `git pull --rebase` when needed).
5. Collect context:
   - new PR: summarize scope and prepare description
   - existing PR: collect latest review comments and CI failures
4. Critically validate comments before changing code (especially bot/AI comments).
5. Implement only applicable fixes on the same branch.
6. Add or update lean tests for changed behavior.
7. Run targeted checks locally first, then broader checks as needed.
8. Commit with a plain human message and push.
9. If this is a new PR, make sure the PR title starts with the issue/ticket number when one exists (for example, `[123] Fix dropdown focus trap`). Then run `gh pr create` and verify the returned PR URL/number.
10. If this is an existing PR, keep the title aligned with that issue/ticket-number convention when applicable, and verify updates landed on the active PR (`gh pr view` / checks).
11. If new commits changed scope/behavior, update the PR description so it reflects the latest state before/after pushing.
12. Reply on addressed review comments with minimal wording (`fixed` / `done`).
13. Summarize what changed, what was tested, and any remaining risk.

## Command Patterns

```bash
# PR creation + checks
 gh pr create --base <base-branch> --head <branch> --title "<title>" --body "<body>"
 gh pr view <pr-number|url|branch> --repo <owner/repo> --json number,url,state
 gh pr view --repo <owner/repo> --json number,url,state                              # okay after the target branch/PR context is already verified
 gh pr list --repo <owner/repo> --head <branch> --state open --json number,url,state   # fallback when PR number/URL is not available and branch context is unambiguous
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

## PR Description Format

- Follow `pr-description-template.md` in this skill folder when creating or updating PR descriptions.
- Start PR titles with the issue/ticket number when one exists, using `[123] Title` format.
- Use the **New feature** format for feature work.
- Use the **Bug fix** format for bug work.
- Keep the **Testing** block brief: summarize test types and covered behavior, not full logs.

## Guardrails

- For GitHub iteration work, prefer having an active issue number tied to the work when it fits the repo workflow.
- If an active issue does not exist yet, create one with a concise summary and bullet-list description only when that matches the repo's workflow and the user has not explicitly waived it.
- For existing PR iteration, stay on the same branch and same PR only while that PR is still open.
- PR base default: target the repository’s active default base branch (e.g., `main`, `master`, or equivalent) when creating PRs; do not target another feature/fix branch unless the user explicitly requests stacked PRs.
- Keep PR descriptions up-to-date during iteration (especially after new commits that alter scope, behavior, tests, or risk).
- If that PR is already merged or closed, do not keep pushing to the old PR branch. Start from the current base branch, create a new branch, and open a new PR for follow-up work.
- Before pushing follow-up commits or sending a PR link to the user, re-check that the PR is still open and is the correct review target.
- Create a new PR only when the task is explicitly new PR work (not follow-up on an active PR).
- Do not commit planning/design markdown files unless explicitly requested.
- Prefer minimal diffs that directly resolve comments or failures.
- Always report what was run locally before pushing.
