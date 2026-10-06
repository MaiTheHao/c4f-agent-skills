---
name: refactoring-code
description: Review and improve the structure of existing code without changing its behavior. Finds code smells, tangled responsibilities, tight coupling, duplication, long or deeply nested functions, and unclear names, then reports or applies minimal, safe fixes. Make sure to use this skill whenever the user asks to refactor, clean up, tidy, simplify, optimize, or review the readability, maintainability, or quality of a file or a group of related files, including phrases like "code smell", "SOLID", "too messy", "hard to read", "review this code", "tối ưu code", "dọn code", or "refactor file này", even if they never say "refactor". Not for new features (use brainstorming), bug fixes, or runtime profiling.
---

# Refactoring Code

Improve structure, keep behavior. Make the smallest change that removes a real, nameable problem.

## Rules

When rules conflict, the earlier one wins: behavior > repo conventions > coupling and cohesion > SOLID > readability > performance > style.

1. **Behavior is frozen.** Keep public APIs, return values, error types and messages, side effects, data formats, ordering, and concurrency semantics. A fix that needs any change is not a refactor: report it separately and do not apply it.
2. **Repo conventions beat this skill.** Check `CODING_STANDARDS.md`, `CONTRIBUTING.md`, `docs/`, linter config, and neighboring files. Drop any finding the repo's own convention endorses.
3. **Surgical edits.** Touch only the requested files and only the lines the fix needs. Mention unrelated problems instead of fixing them. Remove only imports and variables your own change orphaned.
4. **No speculative abstraction.** Add an interface, layer, wrapper, or pattern only when it removes duplication or branching that exists today in two or more places, or when the repo already uses that seam. Three similar lines beat a premature abstraction.
5. **Verify.** Run tests before and after. With no tests, limit changes to mechanical refactors (rename, extract function, guard clause) and list manual checks.

## Workflow

1. **Pick the mode.** *Review* (report only) when the user says review, analyze, or check. *Refactor* (apply) when they say refactor, clean, optimize, or fix. If unclear, report first and ask once. If the work spans many files or about 200+ changed lines, report first and get approval.
2. **Read.** Read every target file fully, grep callers of its public symbols, and read its tests and neighbors.
3. **Baseline.** Run tests, lint, and type-check, and note the results.
4. **Detect.** Use the checklists below. For multiple files, map imports and dependency direction first and flag cycles and layer violations before per-file issues.
5. **Rank.** *High*: blocks testing or makes changes risky (cycles, god class, repeated switches, duplicated logic that has diverged). *Medium*: hurts comprehension. *Low*: naming and nesting. Fix safe High and Medium findings; list Low ones unless trivial.
6. **Apply** one refactor at a time and rerun tests after each. Revert any step that fails.
7. **Report** in the format below.

## Smell checklist

Thresholds say where to look, not what is wrong. Flag only when you can state what is hard to read, test, or change.

| Smell | Flag when | Fix |
|---|---|---|
| Mysterious name | Needs a comment to explain, or is generic (`data`, `info`, `helper`, `manager`) | Rename to the project's domain term |
| Long function | Over ~30 lines, or mixes levels (validate, compute, persist) | Extract by responsibility; name each piece for what it does |
| Deep nesting | More than 3 levels | Guard clauses and early returns; keep the happy path unindented |
| Long parameter list / data clumps | More than 4 params, or the same 3+ params travel together in 3+ places | Parameter object |
| Boolean flag | A bool parameter switches between materially different behaviors | Split into separate functions (keep orthogonal toggles like `dryRun`) |
| Duplicated code | Same logic (not just similar text) in 2+ places that change for the same reason | Extract and call from both; leave look-alikes that change for different reasons |
| Repeated switches | Same `switch` or `if` cascade on a type in 2+ places | Polymorphism or a lookup map |
| Feature envy | A method uses another object's data more than its own | Move it onto that object |
| Primitive obsession | A string or int stands for a domain concept whose validation or formatting is repeated | Small dedicated type |
| Message chains | `a.b().c().d()` walks through 3+ objects | Hide the walk behind one method |
| Middle man | A class or function only delegates | Remove it, call the target directly |
| Speculative generality | Unused params, single-implementation interfaces with no seam, hooks nobody calls | Delete |
| Refused bequest | A subclass overrides or ignores most of what it inherits | Composition instead of inheritance |
| What-comments | Comment restates the code | Rename or extract; keep comments that explain why |
| Shotgun surgery / divergent change | `git log` shows one change touching many files, or one file changing for unrelated reasons | Group what changes together; split what changes apart |

Skip anything the linter, formatter, or type checker already enforces. Report swallowed exceptions or leaked low-level errors, but do not change them, since that alters error semantics.

## Structure checklist

- **Cohesion:** Can you describe the unit's purpose without "and"? Do its methods share the same fields? If not, split by responsibility, not by line count.
- **Coupling:** Look for import cycles, domain code importing infrastructure, reach-through chains, and shared mutable state across modules. Report these; fix only where a seam already exists. Do not decouple things that change together for the same reason.
- **SOLID, only when it causes real pain:**
  - SRP: one file changes for two unrelated reasons.
  - OCP: every new variant forces an edit to the same type switch.
  - LSP: a subtype throws, narrows inputs, or ignores the base contract.
  - ISP: implementors stub methods they do not use.
  - DIP: high-level code imports a concrete low-level detail, and the repo has an abstraction for it elsewhere or tests must mock it.
- **Patterns:** Use one only to fix a smell above (Strategy for repeated switches, Builder or Factory for many optional variants, Observer for notifying several dependents). Follow the shape of any existing pattern in the repo. A well-named function often beats a pattern.

## Report format

```markdown
### Summary
[1-2 sentences: overall state and what was done]

### Findings
| # | Severity | Location | Smell | Evidence | Fix | Status |
|---|---|---|---|---|---|---|
| 1 | High | `OrderService.java:40-120` | Repeated switches | Same `switch(type)` at L45 and L98 | Replace with `Map<Type, Handler>` | Applied |

### Changes
- `path/to/file`: [what changed and why]

### Left alone
- [Skipped item and the reason: repo convention, behavior change, no tests, out of scope]

### Verification
- Before: [command] -> [result]
- After: [command] -> [result]
```