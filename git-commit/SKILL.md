---
name: git-commit
description: Analyze diffs, securely stage changes, generate Conventional Commit messages, and execute git commits when requested or invoked via /commit.
---

# Git Commit

## 1. Safety Rules (Mandatory)

- **Secrets:** Never commit `.env`, private keys, tokens, credentials, or other secrets. Scan staged and unstaged changes before committing. If a secret is detected, stop and alert the user.
- **Destructive actions:** Never use `git reset --hard`, force operations, or force-push unless explicitly requested. Never force-push to `main`/`master`.
- **Git config:** Never modify `git config`.
- **Hooks:** Never bypass hooks with `--no-verify` unless explicitly requested. If a hook fails, fix the cause and create a new commit; never amend.
- **Scope:** Preserve unrelated changes. Stage only intended files; never blindly stage all changes when unrelated or sensitive files may be present.

## 2. Conventional Commit Format

`<type>[optional scope][!]: <description>`

- Keep the header under 72 characters.
- Use lowercase, imperative, present-tense descriptions without trailing periods.
- Use one logical change per commit.
- Breaking changes: append `!` to the type/scope or include `BREAKING CHANGE: <details>` in the footer.
- Add issue references, e.g. `Closes #123`, when supported by context.

| Type | Use |
|---|---|
| `feat` | New functionality |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting; no logic changes |
| `refactor` | Code restructuring without behavior changes |
| `perf` | Performance improvement |
| `test` | Test changes |
| `build` | Build or dependency changes |
| `ci` | CI configuration or scripts |
| `chore` | Maintenance |
| `revert` | Revert a previous commit |

Choose the type and scope from the actual diff, not assumptions.

## 3. Workflow

1. **Inspect:** Run `git status`, `git diff`, `git diff --staged`, and `git log --oneline -n 5`.
2. **Review:** Understand staged, unstaged, and untracked changes. Identify logical commit boundaries and potential secrets.
3. **Stage:** Stage only intended files or hunks. Preserve unrelated existing staging.
4. **Validate:** Review the final staged diff and scan staged content for secrets.
5. **Generate:** Infer type, scope, summary, breaking changes, and issue references from evidence.
6. **Commit:** Execute `git commit` with the generated message.
7. **Verify:** Check the commit result and remaining working-tree changes.

Do not commit if the intended scope is unclear, secrets are detected, or unrelated changes cannot be safely separated. Ask the user when necessary.

## 4. Failure Handling

- **Hook failure:** Diagnose, fix, re-review, and create a fresh commit.
- **Tests or validation fail:** Report the failure; do not claim success.
- **Unexpected staging or repository state:** Stop before committing and clarify.
- **Commit succeeds:** Report the commit hash, message, and any remaining changes.

## 5. Output

When invoked, perform the workflow rather than merely suggesting a message, provided repository access is available.

Keep the final report concise: commit hash, Conventional Commit message, validation outcome, and unresolved issues. Never claim an action succeeded unless verified.