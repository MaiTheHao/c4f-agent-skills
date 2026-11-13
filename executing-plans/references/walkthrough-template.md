# Walkthrough Template

Path: `local/agents/walkthroughs/<same filename as the plan>`. Repo-relative paths. Create the directory if missing. Write it in the user's language; keep keys in English.

````markdown
# [Feature Name] Walkthrough

**Plan:** `local/agents/plans/<filename>`

## Summary

[Goal and outcome]

## Progress

- [x] U1: [Name] - Verify passed
- [ ] U2: [Name] - pending

## Changes Made

| File           | Status                       | Unit | Description |
| :------------- | :--------------------------- | :--- | :---------- |
| `path/to/file` | CREATED / MODIFIED / DELETED | U1   | [Change]    |

## Verification Evidence

Per unit: the command actually run, relevant output, exit status.

```bash
# U1: command
# relevant output
```

## Baseline vs Final

- Baseline: [result]
- Final regression: [result]
- Pre-existing failures: [list or None]

## Deviations

- [dated entry: blocker, affected decision, user go-ahead] or None

## Out-of-Scope Observations

- [unrelated findings or None]

## Acceptance

- [x] AC1: [criterion] - [evidence]
- [x] ACn: full regression - [result vs Baseline]
````
