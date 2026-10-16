---
name: issue-to-pr
description: Take an issue, ticket or plain request through root cause, scope, fix, PR, green checks and resolved review comments, then merge on the user's approval. Use when the user hands over a ticket, bug report or feature idea to implement, or says "take this to a PR", "fix this issue", "ship this".
---

# Issue to PR

One issue in, one **green** PR out: every check passing, every review thread resolved, merged only on the user's yes.

The repo's own rules win on specifics. Read `AGENTS.md`, `CLAUDE.md` and `CONTRIBUTING.md` first for the base branch, validation commands, PR format and review process.

## 1. Read the issue

The input is a ticket URL or number, a pasted prompt, or a loose idea. Fetch tickets in full (`gh issue view <n> --json title,body,comments,labels`), including attached screenshots. Done when you can state the issue in one sentence and say whether it's a **bug** or a **feature**.

## 2. Find the root cause

- **Bug:** reproduce it, then trace it to the line that produces it. Fix at the source, not where the symptom shows.
- **Feature:** find where it belongs: the modules it extends and the existing patterns it should follow.

Done when you can point to `file:line` and say why it causes the issue, or where the feature plugs in.

## 3. Fix the scope

List every file and behaviour the change touches, every consumer the root cause reaches (UI, API, jobs, other packages), and what is deliberately left out. Done when nothing in the diff will surprise a reviewer.

## 4. Explain, then build

Tell the user in a few lines: the issue, the root cause, the scope, the plan. Then carry on without waiting, unless the acceptance criteria are ambiguous; ask about those once.

Pick the **base branch** before branching: the one the user's recent PRs target (`gh pr list --author @me --state all --limit 20 --json baseRefName`), or the one the repo docs name. If neither gives a clear answer, ask.

Work in a fresh branch or worktree off the base, leaving the user's checkout and uncommitted changes untouched. Write a test that fails without the fix where one fits, implement in the surrounding style, and run the repo's lint, typecheck and tests. Done when they pass locally.

## 5. Open the PR

Commit, push and open the PR against the base branch, using the repo's PR template if it has one. The body covers the root cause, the fix, how you verified it, anything not verified, and `Closes #<n>` when there's a ticket.

## 6. Watch until green

Watch the checks in the background (`gh pr checks <n> --watch`) until none is pending. On a failure, read the log, fix it, push, and watch again. Done when every required check passes.

## 7. Clear the reviews

Run the repo's PR-review skill if it has one (look in `.claude/skills`, `.agents/skills` and the docs). Separately, wait for automated reviewers to post: CodeRabbit, Copilot, Greptile, or any bot that commented on the repo's recent PRs or has a config file in the repo.

For each comment, fix it or reply with why not, then resolve the thread. Each push restarts checks and re-triggers bots, so go back to step 6. Done when checks are green, every thread is resolved, and no reviewer has posted since your last push.

## 8. Merge on approval

Ask the user, in a few lines: PR link, root cause, fix, checks, review status. Merge only on an explicit yes in this conversation, using the repo's merge method (squash, rebase or merge commit, whichever its history shows). Confirm the PR shows `MERGED` and the ticket closed, then delete the branch or worktree.
