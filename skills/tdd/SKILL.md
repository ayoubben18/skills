---
name: tdd
description: Test-driven development. Use when the user wants to build features or fix bugs test-first, mentions "red-green-refactor", or wants integration tests.
---

# Test-Driven Development

Every test must protect a real use case, fail for the right reason before the change, and pass because the production code now does the right thing. Work through the steps below in order, once per use case.

**Read the project's own docs first**, if they exist: domain docs such as `CONTEXT.md` or a glossary so test names match the project's domain language, any testing conventions (in `CLAUDE.md`, `AGENTS.md`, `CONTRIBUTING.md` or similar) for the framework and conventions, and the ADRs in the area you're touching. Otherwise, follow the patterns of the existing tests.

See [tests.md](tests.md) for worked examples of each step and [mocking.md](mocking.md) for where mocks belong.

## 1. Define the use case before writing the test

State the starting condition, the action, and the observable result. Ground it in a requirement, actual usage, or a reproduced bug — never in what the code happens to do. The use case is not complete until you can say concretely what breaks for the user if this behavior regresses.

## 2. Check existing coverage first

Read the nearby tests and the relevant implementation. Extend an existing test when it already exercises the scenario. Add another only when it protects a distinct failure mode.

## 3. Choose the smallest realistic test boundary

Test calculations directly, component interactions through the component, and critical application wiring through integration or E2E tests. Mock external boundaries only where necessary. Exercise the production code responsible for the behavior — never a copy, a stub of it, or a private helper reached around the interface.

When the shape of that boundary is itself in question, prefer testing through the module's public interface — the interface is the test surface. If the behavior can't be reached that way without reaching into internals, that is a design signal: the seam is in the wrong place, not a reason to test the internals.

## 4. Require meaningful red, then green

Run the test before implementing the change. Confirm it fails **because the required behavior is missing or broken** — not because of a typo, a missing import, a bad fixture or a wrong path. Implement the smallest fix, rerun until green, then refactor the code you just touched with the test as the safety net.

For a pure refactor, there is no red: establish passing behavior coverage first, then change the implementation and keep it green.

## 5. Make assertions independent of the implementation

Derive expected results from the use case, not from the code. Assert the submitted value, the visible result, the persisted state, or a meaningful interaction. Exact values belong where exactness matters, such as preserving prompt text. Multiple assertions are appropriate when they jointly prove one use case.

## 6. Challenge each test's value

For every test, ask:

- What plausible broken implementation would still pass it?
- Does another test already catch the same failure?
- Would a harmless refactor break it?

Rewrite weak assertions and remove tests with no distinct protection. For substantial changes, use a second agent to challenge the grouped findings against source evidence.

## 7. Keep execution deterministic and verification bounded

Control external data and time when needed, wait for observable outcomes instead of sleeping, and account for platform differences. Run the focused tests and the relevant integration checks.

Finish with three things stated: the behavior now protected, the evidence of red and green (the failing output, then the passing run), and any verification limits — what you could not run or could not observe.

## Autonomous runs

When no user is there to confirm (a subagent, a background or headless run), the task's stated objective and any agreed plan are the source of the use case and the boundary. If the task needs a boundary the plan never named, take the narrowest public interface that exposes the behavior and name it in your final summary so a reviewer can challenge it. The red/green evidence and verification limits from step 7 go in that summary too.
