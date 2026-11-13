---
name: git-commit
description: Stage intended changes only, generate a Conventional Commit message, and commit safely when invoked via /commit.
---

# Git Commit

## Hard rules

- Never commit secrets (.env, keys, tokens, credentials). Scan before staging/committing; stop and alert if found.
- Never run destructive git ops (reset --hard, force push, amend) or modify git config unless explicitly requested. Never force-push main/master.
- Never bypass hooks (--no-verify) unless explicitly requested. If a hook fails: fix, re-review, create a fresh commit.
- Stage only intended files/hunks; preserve unrelated staging; never blindly `git add -A`.
- If scope is unclear, secrets found, or unrelated changes can't be safely separated: stop and ask.

## Workflow

1. Inspect: `git status`, `git diff`, `git diff --staged`, `git log --oneline -n 5`.
2. Identify logical boundaries and potential secrets.
3. Stage selectively.
4. Re-review staged diff + secret scan.
5. Commit using Conventional Commit (type/scope inferred from diff; header ≤72 chars; imperative lowercase; `!` or `BREAKING CHANGE:` for breaking; issue refs if known).
6. Verify commit result and remaining tree state.

## On failure

- Hook/test failure: fix, re-review, new commit. Never amend. Never claim success.
- Unexpected repo state: stop and clarify.

## Report

Commit hash, message, validation outcome, remaining changes, unresolved issues.
