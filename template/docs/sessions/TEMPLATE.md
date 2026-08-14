---
date: YYYY-MM-DD
issue: <GitHub issue #, or QUEUE.md note title in local mode>
branch: <n>-<slug>
status: active # active | deferred | blocked | done
scope:
  phase: <phase>
  modules: [<dirs you will edit>]
---

# <title>

Copy to `active/YYYY-MM-DD-<slug>.md`, fill `scope`, read other `active/` files first. On close:
promote learnings (AGENTS.md table), write the close-out below, set `status: done`, move to
`../archive/`, and commit - BEFORE `gh pr create` (GitHub mode) so the close-out rides inside the
PR (the archive is a hand-off buffer; the next session deletes it at start). Do not record the PR URL here (the issue links it) and do not commit after the PR exists.
Local mode: merge back to `main` locally instead of opening a PR.

## Goal / Problem Statement

<the one outcome / problem statement>

## Scope

<scope>

## Task Breakdown

- [ ] <task>

## Log (append-only)

- HH:MM - <did / decided / found>

## Handoff (keep current - what the next agent reads)

- Done: <+ commit refs>
- Half-done / how to continue: <state + next step>
- Next: <single next thing>
- New gotchas: <promote on close>
- Verify with: <commands/tests>

## Close-out (write before opening the PR)

- What landed: <summary>
- Validation: <test/lint results, what was NOT verifiable and why>
- Next step: owner review
