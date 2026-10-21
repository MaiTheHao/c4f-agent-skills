---
name: refactoring-code
description: Review or refactor existing code for readability, maintainability, duplication, coupling, and structural quality without changing behavior. Use for requests to review, clean up, simplify, optimize, tidy, or refactor code, including code smells and SOLID. Not for new features, bug fixes, or runtime profiling.
---

# Refactoring Code

Improve structure with the smallest change that removes a concrete problem. Preserve behavior.

## 1. Priority

When rules conflict: behavior > repo conventions > cohesion/coupling > SOLID > readability > performance > style.

- **Behavior frozen:** Preserve APIs, return values, error types/messages, side effects, data formats, ordering, and concurrency. If a change alters these, report it separately; do not apply it as a refactor.
- **Repo conventions:** Check `CODING_STANDARDS.md`, `skill code-convention`, `docs/`, linter configuration, tests, and neighboring files. Ignore findings that follow intentional repo conventions or are already enforced by tooling.
- **Scope:** Touch only requested files and necessary lines. Remove only imports/variables orphaned by your changes. Report unrelated issues without fixing them.
- **Abstraction:** Add a layer, interface, or pattern only when it removes real duplication/branching in 2+ places or follows an existing seam. Prefer simple functions over speculative abstractions.
- **Verification:** Run tests before and after. Without tests, limit changes to mechanical refactors and document manual checks.

## 2. Workflow

1. **Mode:** Review/analyze/check → report only. Refactor/clean/simplify/optimize → apply changes. If unclear, report first and ask once. For broad changes spanning many files or ~200+ changed lines, report and obtain approval first.
2. **Inspect:** Read target files fully; trace public-symbol callers and review relevant tests, dependencies, and neighboring code.
3. **Baseline:** Run available tests, lint, and type checks. Record commands and results.
4. **Analyze:** For multiple files, map imports and dependency direction first; identify cycles and layer violations.
5. **Prioritize:** High = risky structure, cycles, duplicated logic that has diverged, or major testing barriers. Medium = comprehension and maintainability issues. Low = naming and nesting. Fix safe High/Medium findings; report Low findings unless trivial.
6. **Refactor:** Apply one change at a time; rerun relevant tests after each. Revert a change if verification fails.
7. **Report:** Use the output format below.

## 3. Code Smells

Thresholds are investigation triggers, not automatic defects. Report only concrete problems that affect comprehension, testing, or changeability.

| Smell | Signal | Typical fix |
|---|---|---|
| Poor naming | Generic or unclear names requiring explanation | Use domain-specific names |
| Long function | ~30+ lines or mixed responsibilities | Extract cohesive functions |
| Deep nesting | >3 levels | Guard clauses, early returns |
| Long parameters/data clumps | >4 params, or 3+ parameters repeatedly grouped in 3+ places | Parameter object |
| Boolean flag | Selects materially different behavior | Split functions; retain orthogonal flags such as `dryRun` |
| Duplication | Same logic in 2+ places changing for the same reason | Extract shared logic |
| Repeated switches | Same type-based branching in 2+ places | Lookup map or polymorphism |
| Feature envy | Method relies primarily on another object's data | Move behavior where appropriate |
| Primitive obsession | Repeated validation/formatting of a domain string or integer | Dedicated type when justified |
| Message chains | Traverses 3+ objects, e.g. `a.b().c().d()` | Encapsulate the traversal |
| Middle man | Unit only delegates | Remove unnecessary indirection |
| Speculative generality | Unused parameters, unnecessary interfaces, dead hooks | Remove unnecessary code |
| Refused bequest | Subclass ignores or overrides most inherited behavior | Prefer composition |
| Redundant comments | Comments merely restate code | Improve naming or structure; retain rationale |
| Shotgun surgery/divergent change | Related changes span many files, or one file serves unrelated reasons | Reorganize by change responsibility |

Do not change swallowed exceptions or leaked low-level errors: report them because fixes may alter error semantics.

## 4. Structural Review

- **Cohesion:** A unit should have a clear purpose and related behavior/state. Split by responsibility, not line count alone.
- **Coupling:** Identify import cycles, reversed dependency direction, reach-through chains, and shared mutable state. Fix only when an appropriate seam exists.
- **SOLID:** Apply only when a concrete problem exists:
  - SRP: one unit changes for unrelated reasons.
  - OCP: every new variant requires modifying the same branching logic.
  - LSP: subtype violates the base contract.
  - ISP: implementors must provide unused methods.
  - DIP: high-level logic depends on concrete infrastructure despite an existing abstraction or a real testing need.
- **Patterns:** Use Strategy for repeated switches, Factory/Builder for genuinely complex construction, or Observer for multiple dependents when justified. Follow existing repo patterns. Prefer a well-named function when sufficient.

## 5. Constraints

- Never expand scope or change behavior under the label of refactoring.
- Do not add speculative abstractions or unrelated cleanup.
- Do not fix issues already handled by repository tooling unless requested.
- If tests are absent, make only mechanical changes such as renaming, extracting functions, or introducing guard clauses; document manual checks.
- If safe behavior preservation cannot be established, report the issue instead of applying the change.

## 6. Report Format

# Summary
[Overall state and work completed in 1–2 sentences.]

# Findings
| # | Severity | Location | Smell | Evidence | Fix | Status |
|---|---|---|---|---|---|---|
| 1 | High | `path/to/file:line` | [Smell] | [Concrete evidence] | [Minimal fix] | Applied / Reported |

# Changes
- `path/to/file`: [Change and rationale]

# Left Alone
- [Skipped issue and reason, or None]

# Verification
- Before: `[command]` — [result]
- After: `[command]` — [result]
- Manual checks / limitations: [details or None]