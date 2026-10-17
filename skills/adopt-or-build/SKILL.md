---
name: adopt-or-build
description: Before building a feature or fix, search for open-source libraries that already solve it and decide per piece whether to adopt one or build it. Use when the user is scoping a feature or bug, asks "is there a library for this", "should we build this ourselves", or wants to avoid reinventing something.
---

# Adopt or Build

Every piece of a feature gets one verdict: **adopt** a library or **build** it. Adopt only a **healthy** library that removes real work. Build when no healthy library exists, or when the piece is small enough that a dependency costs more than the code.

The repo's own rules win. Read `AGENTS.md`, `CLAUDE.md` and `CONTRIBUTING.md` for banned or preferred dependencies, licence policy and supported runtimes.

## 1. Scope the feature

Break the feature or bug into **pieces**: the separate capabilities it needs (for example parsing, retries, rendering, scheduling). Note the constraints: language, framework, runtime (Node, browser, edge, mobile), the project's licence, and the bundle-size budget if one exists.

Done when every piece has a one-line description and you can say which pieces are hard and which are trivial.

## 2. Check what the repo already has

For each piece, look for existing code that already covers it:

- dependencies in the manifest (`package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`), including what they offer beyond what the repo uses today
- internal helpers and shared packages

Done when every piece is marked **covered** (with the dependency or `file:line`) or **open**. A covered piece reuses what's there. Adding a second library for a job the repo already does is the redundancy this skill exists to prevent.

## 3. Find candidates

For each open piece, search the package registry, GitHub and the framework's own docs and ecosystem lists. Collect up to three candidates per piece, starting with the ones the ecosystem treats as the default.

Done when every open piece has its candidates listed, or a note that the search found none.

## 4. Check health

A library is **healthy** when it passes every check below. Gather the numbers, never estimate them:

| Check | Bar | How |
| --- | --- | --- |
| Maintained | release or commit in the last 6 months | `gh repo view <owner/repo> --json pushedAt,isArchived`, `npm view <pkg> time` |
| Community | more than one active contributor in the last year, and maintainers reply to issues | `gh api repos/<owner/repo>/contributors`, the latest issues and PRs |
| Used | steady downloads or dependents for its ecosystem | registry download stats, GitHub "Used by" |
| Safe | no open high or critical advisories | `npm audit`, GitHub advisories, osv.dev |
| Licence | compatible with the project's licence | `gh repo view --json licenseInfo` |
| Fits | supports the project's runtime, module format and types | its docs and `package.json` |

Done when every candidate has a pass or fail on each check, with the number or link behind it. A candidate that fails any check is out.

## 5. Weigh the abstraction

For each healthy candidate, compare the work it removes against what it costs:

- **Coverage:** which parts of the piece it handles, and what glue code remains.
- **Friction:** where its design fights the requirements or the codebase's patterns.
- **Weight:** install and bundle size, and the dependencies it pulls in.

Adopt when the library handles the hard part and the glue is smaller than the code it replaces. Build when the piece takes a short function to write, or when the glue would match the library's size.

Done when every piece has a verdict with a one-sentence reason.

## 6. Report

Send one table, then stop:

```
| Piece | Verdict | Library | Why |
| --- | --- | --- | --- |
| retries | adopt | p-retry 6.2 | handles backoff and abort; 2 contributors active, last release 3 weeks ago |
| CSV export | build | none | 15 lines with the built-in stream API; candidates unmaintained since 2023 |
| date parsing | covered | date-fns (already installed) | `parseISO` covers the formats |
```

Under the table, list what adopting changes: packages to install, files the glue code goes in. Install packages only after the user agrees.

Done when every piece from step 1 appears in the table.
