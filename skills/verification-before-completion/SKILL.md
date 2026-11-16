---
name: verification-before-completion
description: Use when about to claim code works, is fixed, or passes, before committing or creating PRs - evidence proportional to the change, and never claiming more than was checked
---

# Verification Before Completion

## Overview

**Core principle:** Don't claim what you didn't check. Check in proportion
to the change.

## The Rule

```
NO "IT WORKS / IT'S FIXED / TESTS PASS" WITHOUT EVIDENCE FROM THIS SESSION
```

Scale the check to the change:

| Change | Enough evidence |
|--------|-----------------|
| Docs, comments, copy, config values, renames the compiler checks | Re-read the diff. No command needed. |
| Small code change | The build/type check, or one run of the changed behavior. |
| Behavior change or bug fix | Run the changed behavior (or the reproduction) and read the output; in-scope tests. |
| Locked behavior, recurring issue, large work | Guards + in-scope tests, and the evidence named in the report. |

When you could not verify something, say so plainly instead of implying
it works. Keep the report short: what changed, what you ran, the result.

## The Gate Function

```
BEFORE claiming code works, is fixed, or passes:

1. IDENTIFY: What is the cheapest check that proves this claim?
2. RUN it fresh, and READ the output and exit code
3. CLAIM exactly what it showed — no wider
```

## Common Failures

| Claim | Requires | Not Sufficient |
|-------|----------|----------------|
| In-scope tests pass | Filtered test command output: 0 failures, and the claim names the scope | Previous run, "should pass", "all tests pass" without having run all |
| Linter clean | Linter output: 0 errors | Partial check, extrapolation |
| Build succeeds | Build command: exit 0 | Linter passing, logs look good |
| Bug fixed | Test original symptom: passes | Code changed, assumed fixed |
| Guard test works | Seen failing on broken code, passing on fixed | Test passes once |
| Feature works | Ran the changed behavior, read output | Code compiles, diff looks right |
| Agent completed | VCS diff shows changes | Agent reports "success" |
| Requirements met | Line-by-line checklist | Tests passing |

## Red Flags - STOP

- Saying code "should work" / "probably works" instead of checking
- About to commit/push/PR without the check the change needs
- Trusting agent success reports without looking at the diff
- Claiming more than you checked (e.g. "all tests pass" after an in-scope run)
- Running heavy checks for a doc-only or trivial change

## Rationalization Prevention

| Excuse | Reality |
|--------|---------|
| "Should work now" | RUN the verification |
| "I'm confident" | Confidence ≠ evidence |
| "Linter passed" | Linter ≠ compiler |
| "Agent said success" | Verify independently |
| "Partial check is enough" | The in-scope run is the right check; claim exactly that scope, nothing wider |
| "I should run the full suite to be sure" | Full suite only when your human partner asks. Run the in-scope tests. |
| "Let me run everything to be thorough" | Proportional evidence. Over-checking a small change wastes the user's time. |

## Key Patterns

**Tests:**
```
✅ [Run pytest tests/billing/] [See: 34/34 pass] "billing tests pass (34/34)"
❌ "Should pass now" / "Looks correct" / "All tests pass" after a scoped run
❌ Running the full suite nobody asked for
```

**Guard tests (locked behaviors, recurring issues only):**
```
✅ Write → Run (pass) → Revert fix → Run (MUST FAIL) → Restore → Run (pass)
❌ "I've written a guard test" (without seeing it fail)
```

**Features (spec-driven):**
```
✅ Run each changed spec behavior → read output → spec file matches code → "F1–F3 verified"
❌ "Implemented per spec" (without running it)
```

**Build:**
```
✅ [Run build] [See: exit 0] "Build passes"
❌ "Linter passed" (linter doesn't check compilation)
```

**Requirements:**
```
✅ Re-read plan → Create checklist → Verify each → Report gaps or completion
❌ "Tests pass, phase complete"
```

**Agent delegation:**
```
✅ Agent reports success → Check VCS diff → Verify changes → Report actual state
❌ Trust agent report
```

## When To Apply

Before claiming code works, is fixed, or passes; before committing,
opening a PR, or marking a task complete; and when accepting a
subagent's "done". Not for conversation, explanations, or doc-only edits
beyond re-reading the diff.
