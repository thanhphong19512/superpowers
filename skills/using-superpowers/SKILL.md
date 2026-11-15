---
name: using-superpowers
description: Use when starting any conversation - establishes how to find and use skills, requiring skill invocation before ANY response including clarifying questions
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, ignore this skill.
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>
If you think there is even a 1% chance a skill might apply to what you are doing, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.

This is not negotiable. You cannot rationalize your way out of this.
</EXTREMELY-IMPORTANT>

## The Rule

**Invoke relevant or requested skills BEFORE any response or action** — including clarifying questions, exploring the codebase, or checking files. If it turns out wrong for the situation, you don't have to use it.

**Before entering plan mode:** if you haven't already brainstormed, invoke the brainstorming skill first.

Then announce "Using [skill] to [purpose]" and follow the skill exactly. If it has a checklist, create a todo per item.

## Skill Priority

When multiple skills apply, process skills come first — they set the approach, then implementation skills (frontend-design, etc.) carry it out. Brainstorming and systematic-debugging are Superpowers' most common process skills, but the rule holds for any of them.

- "Let's build X" (large) → superpowers:brainstorming first, then implementation skills.
- "Fix this bug" → superpowers:systematic-debugging first, then domain skills.
- Writing or changing feature code → superpowers:spec-driven-development.

## Size the Work First

Ceremony scales with the work. Decide the size before choosing a workflow,
and say it in one line ("Small change, doing it directly").

**Small or medium — do it yourself, directly.** One feature or bug, a
handful of files, one service or module, the intent is clear from the
request or one question. No design doc, no plan file, no subagents, no
review dispatch. Ask at most the one question that changes what gets
built, then implement, verify by running it, and report.

**Large — spec, plan, subagents.** Use the full workflow
(superpowers:brainstorming → design doc → superpowers:writing-plans →
superpowers:subagent-driven-development or superpowers:executing-plans)
only when at least one is true:
- the work runs through many dependent steps that must land in order;
- it crosses services, modules, or repos with contracts between them
  (APIs, schemas, events, migrations);
- several people or agents must work from the same written agreement;
- your human partner asks for a spec, a plan, or subagents.

Start subagents only for large work, for genuinely independent parallel
investigations (superpowers:dispatching-parallel-agents), or when your
human partner asks. Never spawn a subagent for work you can finish in
this session.

If small work grows into large mid-way, stop, say so, and step up.

## Development Model

Work is organized around features, not process. Each feature keeps a
living spec (`docs/features/<feature>.md`) that is the source of truth
across edits. There is no test-driven development: verify behavior by
running it, and write tests only for recurring issues and `[locked]`
(sensitive) behaviors. See superpowers:spec-driven-development.

## Test Scope

Run only the tests in scope of the change: the test file(s), test name, or
package that covers the code you touched, plus the guard tests of the
feature you changed. Use the runner's filter (`pytest path::name`,
`npm test -- path`, `go test ./pkg/...`, `cargo test name`,
`dotnet test --filter`).

**Never run the full test suite unless your human partner asks for it.**
This holds everywhere — after a task, before a commit, before finishing a
branch, in worktree setup, in a final review. If you think a full run is
warranted (shared code with many callers, a dependency upgrade, a
migration), say so and let them decide. A build or type check is not a
test suite and may always be run.

## Code Comments

Comment only what the code cannot say:
- **why**, when it is not obvious: a workaround, a constraint from an
  external system, a non-obvious invariant, an ordering that matters;
- **technical or business decisions made while coding**: a rule, a
  threshold, a trade-off you chose, and its reason (e.g.
  `// Round half-up: finance reconciles against the bank's rounding`).

Never comment what the code already shows: restating the line, naming
the function's steps, narrating a change ("added X", "fixed Y"), or
labeling sections. Prefer a clearer name over a comment. Match the
existing comment density of the file.

## Red Flags

These thoughts mean STOP—you're rationalizing:

| Thought | Reality |
|---------|---------|
| "This is just a simple question" | Questions are tasks. Check for skills. |
| "I need more context first" | Skill check comes BEFORE clarifying questions. |
| "Let me explore the codebase first" | Skills tell you HOW to explore. Check first. |
| "I can check git/files quickly" | Files lack conversation context. Check for skills. |
| "Let me gather information first" | Skills tell you HOW to gather information. |
| "This doesn't need a formal skill" | If a skill exists, use it. |
| "I remember this skill" | Skills evolve. Read current version. |
| "This doesn't count as a task" | Action = task. Check for skills. |
| "The skill is overkill" | Check it; then size the work. Small work skips spec, plan, and subagents. |
| "Let me run the full suite to be safe" | Run the in-scope tests. A full suite needs your human partner's request. |
| "I'll write a plan just to be safe" | A plan for small work is waste. Plans are for multi-step, cross-service work. |
| "I'll just do this one thing first" | Check BEFORE doing anything. |
| "This feels productive" | Undisciplined action wastes time. Skills prevent this. |
| "I know what that means" | Knowing the concept ≠ using the skill. Invoke it. |

## Platform Adaptation

If your harness appears here, read its reference file for special instructions:

- Claude Code: `references/claude-code-tools.md`
- Codex: `references/codex-tools.md`
- Pi: `references/pi-tools.md`
- Antigravity: `references/antigravity-tools.md`
- Hermes Agent: `references/hermes-tools.md`
- Muse: `references/muse-tools.md`

## User Instructions

User instructions (CLAUDE.md, AGENTS.md, GEMINI.md, etc, direct requests) take precedence over skills, which in turn override default behavior. Only skip skill workflows or instructions when your human partner has explicitly told you to.
