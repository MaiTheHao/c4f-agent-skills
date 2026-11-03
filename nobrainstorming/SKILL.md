---
name: nobrainstorming
description: Direct-work mode. Use ONLY when the user explicitly says "nobrainstorming", "làm luôn", "just do it", or "đừng nghĩ nhiều". Never self-select.
---

# Nobrainstorming

Skip the ceremony, keep the judgment. Think as well as normal, but spend effort on the change, not on exploring around it. Reply in the user's language.

Replaces the design gate and plan files for this task. Everything about correctness, quality, and honesty stays the same.

## Cut these three things

**1. Over-scanning references.**

- Start from the file/symbol the request names. Read it, its direct callers/usages, and one example of the local pattern.
- Stop reading when you can write the change. Do not tour the repo or open files "just in case".
- Search with targeted queries (a symbol, a string), not broad sweeps.

**2. Relationship overthinking.**

- Trace only dependencies the change can actually break: the symbol you modify and its direct usages.
- Do not map the whole dependency graph, enumerate hypothetical impacts, or debate architecture that the request didn't touch.
- Do not compare alternatives unless two approaches truly differ in outcome; then give the pick in one line.

**3. Pointless shell loops.**

- Run each command once with a purpose. Do not re-run the same command expecting a different result.
- Batch related checks into one command. No polling or retry loops, no `find | xargs` sweeps to "see what's there".
- If a command fails, read the error, fix the cause, then re-run. After 3 failed fixes on the same issue, stop and report.

## Keep (unchanged)

- Understand the request fully; ask when something is genuinely ambiguous and the repo can't answer it.
- Surgical changes in existing style; report unrelated problems, don't fix them.
- Verify with a real check and report real output.
- No installs, commits, pushes, or destructive actions unless asked. No weakened tests. No secrets exposed.
- If you hit real risk (schema/data migration, auth/security, shared interface, irreversible action, much larger scope than expected), say so and ask before editing. This is normal judgment, not a mode switch.

## Flow

1. One line: what you will do.
2. Read the minimum, make the change.
3. Run the check once.
4. Short report: what changed, evidence, assumptions, anything noticed.
