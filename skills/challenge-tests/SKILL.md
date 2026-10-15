---
name: challenge-tests
description: Audit existing tests for gaps, weak assertions, redundancy and bloat. Use when the user wants to challenge, review or audit a test suite, asks whether tests actually protect anything, or wants to find missing coverage.
---

# Challenge Tests

A test earns its place by catching a **mutant** — a plausible broken implementation — that no other test catches, in the fewest lines that prove its use case. Challenge every test in scope against that bar, then name the use cases nothing protects.

Read the project's testing conventions first, if any (`CLAUDE.md`, `AGENTS.md`, `CONTRIBUTING.md`, ADRs), so verdicts match the house style.

## 1. Fix the scope

Name the test files and the production code they exercise. Done when every test in scope maps to the code it claims to protect.

## 2. Map each test to its use case

For each test, state the starting condition, the action, the observable result, and what breaks for the user if it regresses. Ground it in a requirement, real usage or a past bug. A test whose use case you cannot state is a **delete** candidate.

## 3. Challenge each test

Run every test through these pillars and record each one it fails:

- **Mutant** — name the simplest broken implementation that still passes (`() => ""`, summing only the first item, skipping the save). Where it's cheap, prove it: apply the mutant, run the test, revert, and confirm `git diff` is clean.
- **Duplicate** — another test already kills the same mutants.
- **Brittle** — a harmless refactor breaks it: it asserts internal calls, private helpers, or mocks of the project's own modules instead of observable results.
- **Wrong boundary** — it exercises a copy, a stub or a private helper rather than the production code responsible, or mocks past the system edge (external APIs, time, randomness, sometimes the DB or filesystem).
- **Self-fulfilling** — the expected value is recomputed the way the code computes it, so it passes by construction. Expected values come from the use case.
- **Flaky** — it depends on the wall clock, sleeps, ordering or shared state rather than controlled time and observable outcomes.
- **Verbose** — setup, fixtures or assertions that don't bear on the use case; near-identical tests that belong in one table-driven test; comments restating the code.

Done when every test in scope has a verdict: **keep**, **rewrite**, **merge** or **delete**, with the failing pillars and evidence (`file:line`, the mutant).

## 4. Find the gaps

List the behaviours in scope from requirements, public interfaces, error paths and bug history. Each one is either covered (name the test) or a **gap** (state its use case and the mutant nothing kills). Done when every public behaviour is accounted for.

## 5. Report

Lead with the gaps and the tests that protect nothing, ranked by what breaks for the user. Then a table of every test: verdict, failing pillars, evidence, and the proposed rewrite or merge in one line. End with verification limits — mutants you reasoned about but didn't run.

Apply changes only when the user asks. When applying, every rewrite and new test passes the same pillars, and every gap's test goes red against its mutant before it goes green.
