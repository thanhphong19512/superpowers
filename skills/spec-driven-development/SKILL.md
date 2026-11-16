---
name: spec-driven-development
description: Use when changing what a feature does (its observable behavior), or when a project keeps feature specs in docs/features/ - keeps the spec true, verifies by running it, and writes tests only for recurring issues and locked (sensitive) behaviors. Not needed for refactors, typo fixes, styling, or edits that change no behavior.
---

# Spec-Driven Development

## Overview

The feature is the unit of work, not the process. Every feature has a
living spec that says what it does. Code is changed to match the spec;
the spec is changed when your human partner changes their mind. Behavior
is proven by running it, not by ceremony.

**Scale to the work.**
- **No behavior change** (refactor, rename, typo, styling, internal
  cleanup, a bug fix that restores the spec'd behavior): touch no spec.
  Just make the change and verify it.
- **Small or medium behavior change:** update the one or two spec lines
  that changed, if the feature has a spec. If it has none and the project
  has no `docs/features/`, do not create one unless the behavior is
  sensitive enough to lock or your human partner asks.
- **Large, multi-step or cross-service work:** full specs, design doc,
  and plan (see the sizing rules in superpowers:using-superpowers).

**Never commit unless your human partner asks**, or a large-work plan's
steps say to. Leave changes in the working tree and say what changed.

**Core principle:** The spec says what must stay true. Tests guard only
what has broken before or must never break. Everything else is verified
by running the feature and reading the result.

There is no test-first rule. Do not write a failing test before the code,
and do not write tests to satisfy a process.

## The Feature Spec

One file per feature, kept up to date across every change:
`docs/features/<feature-name>.md` (your human partner's location wins).
It describes the feature as it is now, not the history of how it got there.

```markdown
# <Feature name>

**Purpose:** one or two lines: who uses it and why.

## Behaviors

- F1: <observable behavior, with exact values> 
- F2: <...>
- F3 [locked]: <behavior that must stay exactly like this> — guard: `tests/path::test_name`

## Out of scope

- <what this feature deliberately does not do>

## Known issues

- I1: <symptom> — cause: <root cause> — fixed in <commit> — occurrences: 1
- I2: <symptom> — cause: <...> — occurrences: 2 — guard: `tests/path::test_name`
```

Rules for the spec:

- **Behaviors are observable.** What a user, caller, or another module can
  see: inputs, outputs, side effects, exact values, error cases. Not how
  the code is structured.
- **IDs are stable.** Never renumber. A removed behavior is deleted, and
  its ID is not reused.
- **The spec changes first.** When a request changes what a feature does,
  update the affected behaviors before touching code, and tell your human
  partner which IDs changed.
- **Small is fine.** A three-line spec that is true beats a long one that
  drifted. Spec only the features you touch; never backfill the whole
  codebase.

## When a Test Is Required

Tests exist for exactly two reasons. If neither applies, do not write one.

**1. A recurring issue.** An issue is recurring when:
- the Known issues list already has it (same symptom or same root cause)
  and it came back, or
- your human partner says it came back ("again", "lại bị", "still"), or
- it is a regression of something that worked before and broke because of
  an unrelated change.

When a bug worth remembering is fixed (non-obvious root cause, user-visible,
or likely to return) and the feature has a spec, record it under Known
issues with `occurrences: 1` and no test. The second time, write the
guard test, bump `occurrences`, and add `guard:`. Typos and one-off slips
are not recorded.

**2. A locked behavior.** A behavior is locked when getting it wrong is
expensive and it must survive many later edits exactly:
- money, billing, quotas, pricing, rounding
- authentication, authorization, permissions, secrets handling
- data integrity: migrations, deletion, deduplication, sync, persistence
  formats, public API contracts other systems depend on
- exact values your human partner insists on, or anything they call
  sensitive, critical, or "must not change"

Mark it `[locked]` in the spec and give it a guard test. Propose a lock
when you see one of these; your human partner can decline it. Never lock
silently, never unlock silently.

**Not test-worthy:** UI layout and copy, glue code, straightforward CRUD,
refactors that keep behavior, prototypes and spikes, configuration,
one-off scripts, anything verified by running it once.

## Writing a Guard Test

A guard test is written after the code works, but it must be proven able
to fail:

```
1. Write the test against the spec's behavior (literal expected values
   taken from the spec, never computed by the code under test).
2. Run it: it passes.
3. Break it on purpose: revert the fix, or mutate the guarded behavior.
4. Run it: it MUST fail, for the reason the spec names.
5. Restore the code. Run it: it passes.
```

A guard test that never failed guards nothing. Record the step-4 output
in your report.

Test the behavior, not the implementation: one guard per locked behavior
or recurring issue, as narrow as the behavior allows, through the real
components. For mocks, expected values, and what makes a test worth
keeping, read `references/writing-good-tests.md`.

## The Loop

For each change to a feature's behavior:

1. **Read the spec**, if the feature has one. Create one only per the
   scaling rules above.
2. **Update the spec** lines for what changes. Note which `[locked]`
   behaviors the change touches.
3. **Implement** the change. Comment only what the code cannot show: the
   why behind a non-obvious choice, and technical or business decisions
   made while coding (a rule, threshold, or trade-off and its reason).
   No comments that restate the code or narrate the change.
4. **Verify by running it.** Use the cheapest evidence that proves each
   changed behavior: run the feature, call the endpoint, run the CLI, open
   the page, run the build and type checker. Read the output.
5. **Run the in-scope tests:** the feature's guards (its `guard:` tests,
   by name or file) and any existing test file covering the code you
   touched. Never the full suite unless your human partner asks. Run every guard, whether or not
   you think the change touched them. A guard that fails means the change
   broke a locked behavior or brought back a known issue. Stop and fix the
   code, never the guard, unless the spec itself changed that behavior,
   with your human partner's approval.
6. **Add guards** only for the reasons above.
7. **Report** briefly what changed and how you verified it. Commit only if
   asked (code and spec together).

## Changing a Locked Behavior

A `[locked]` behavior changes only when your human partner asks for that
change by name or approves it after you point it out. Then: update the
spec line, update its guard test to the new values, prove the guard fails
against the old code, and say in your final message which locked
behaviors changed.

If a requested change would alter a locked behavior as a side effect,
stop and ask before implementing.

## Red Flags

| Thought | Reality |
|---------|---------|
| "Let me write the failing test first" | Not this workflow. Build it, run it, read the output. |
| "More tests can't hurt" | Every test is maintenance. Only recurring issues and locked behaviors earn one. |
| "A non-obvious bug, not worth recording" | If it could come back, one line in Known issues is how you'll know it recurred. |
| "Small refactor, let me update the spec" | No behavior change, no spec change. |
| "I'll commit this now" | Only when asked. |
| "The guard is failing, I'll update the expected value" | A failing guard is the point. Fix the code unless the spec changed. |
| "I'll update the spec later" | When behavior changes, the spec line changes with the code. |
| "This feels sensitive, I'll lock it quietly" | Propose locks out loud; your human partner decides. |
| "Small change, but I'll write a plan and dispatch subagents" | Small and medium work is done directly. Plans are for multi-step, cross-service work. |
| "I'll add a comment explaining what this does" | If the code shows it, delete the comment. Comment the why and the decisions. |
| "It compiles, so it works" | Run the behavior you changed and read the result. |
| "The guard passes, it must be fine" | Did you see it fail when broken? If not, it guards nothing. |
