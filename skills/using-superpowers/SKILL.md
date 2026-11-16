---
name: using-superpowers
description: Use when starting any conversation - establishes how to size work and when to use skills; small and medium changes are done directly without process
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, ignore this skill.
</SUBAGENT-STOP>

## The Rule

**Size the work first, then pick skills.** Reading files, git history, or
the code you are about to change is always allowed before deciding — you
cannot size work you have not looked at.

- **Small or medium work:** just do it. Skills are optional references
  here; use one only when it clearly fits (a stubborn bug →
  superpowers:systematic-debugging). No announcements, no todo lists, no
  ceremony.
- **Large work, or your human partner names a skill or workflow:** invoke
  the matching skills and follow them.

Invoke a skill when it clearly matches the task, not "just in case."

## Skill Map

- Large new feature or project, or a request too vague to build →
  superpowers:brainstorming, then superpowers:writing-plans.
- A bug that is not obvious, or a fix that already failed once →
  superpowers:systematic-debugging.
- Changing feature behavior → superpowers:spec-driven-development's rules
  (spec, guards, tests) apply; for small changes, that means updating a
  spec line only if behavior changed.

## Size the Work First

Ceremony scales with the work.

**Small or medium — do it yourself, directly.** One feature or bug, a
handful of files, one service or module, the intent is clear from the
request or one question. No design doc, no plan file, no subagents, no
review dispatch, no worktree, no integration menu, no commit unless
asked. Ask at most the one question that changes what gets built, then
implement, verify proportionately, and report briefly.

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

| Thought | Reality |
|---------|---------|
| "Let me load a skill just in case" | Only when it clearly fits. Small work needs none. |
| "I'll announce the skill and make a todo list" | Not for small or medium work. Just do it. |
| "Let me run the full suite to be safe" | Run the in-scope tests. A full suite needs your human partner's request. |
| "I'll write a plan just to be safe" | A plan for small work is waste. Plans are for multi-step, cross-service work. |
| "I'll commit this for them" | Commit only when asked, or when a large-work plan's steps say so. |
| "This is large, but I'll wing it" | Large or cross-service work gets the spec, plan, and subagent workflow. |
| "I remember this skill" | Skills evolve. When you use one, read the current version. |

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
