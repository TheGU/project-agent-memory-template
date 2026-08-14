---
type: runbook
title: Going production (local mode to GitHub mode)
description: The one-time checklist that flips the repo from local mode to GitHub mode, plus the standing CI cost-control patterns (path filters, concurrency, the run-ci label gate).
tags: [process, github, ci, release]
timestamp: 2026-08-14
---

## Summary

Local mode is day zero only. The moment the repo has a remote and an owner who reviews PRs, run
this checklist once; every step is cheap now and expensive later. After the flip, `AGENTS.md`
"GitHub mode" rules apply to every session.

## The flip

1. **Remote + protection.** Create the GitHub repo, push `main`. From now on `main` changes only
   via merged PRs: never commit to it, never push it, never force-push.
2. **Queue: notes become issues.** For every open note in `docs/QUEUE.md`: re-verify its claim
   against the current code (stale notes are the rule, not the exception), then `gh issue create`
   with the note's title and body, or drop it with a line of justification. Delete `QUEUE.md`.
   The queue is now `gh issue list`; open questions live as issues or issue comments.
3. **Sessions switch DoD.** Branches are cut from `origin/main`
   (`git fetch --prune origin && git switch -c <n>-<slug> origin/main`), named
   `<issue-number>-<slug>`, and end in push + `gh pr create` with `Closes #<n>` (see AGENTS.md
   "Definition of done - GitHub mode"). The owner merges and presses "Delete branch" immediately.
   The session-archive sweep is unchanged by the flip: every new session still deletes
   `docs/sessions/archive/` leftovers at start - the archived file's record now rides in its PR
   instead of a local merge commit.
4. **Releases: no release-notes file, ever.** Release notes are generated at tag time from merged
   PR titles and descriptions - so PR titles are written for a reader outside the branch. Add a
   tag-triggered workflow job (needs `permissions: contents: write`):

   ```yaml
   on:
     push:
       tags: ['v*']
   jobs:
     release:
       runs-on: ubuntu-latest
       permissions: { contents: write }
       steps:
         - uses: actions/checkout@v4
         - run: gh release create "$GITHUB_REF_NAME" --generate-notes --verify-tag
           env: { GH_TOKEN: ${{ github.token }} }
   ```
5. **CI: start cheap.** Every workflow you add starts with concurrency + path filters (below).
   Create the `run-ci` label now (`gh label create run-ci --description "Run PR CI for this pull
   request"`) even if you don't gate yet, so the quota lever is one edit away.
6. **Milestones (optional).** Mirror `ROADMAP.md` phases as GitHub milestones and attach issues,
   so phase progress is visible in the tracker.

## CI cost control (standing patterns, cheapest first)

- **Cancel superseded runs** - in every workflow:

  ```yaml
  concurrency:
    group: ${{ github.workflow }}-${{ github.ref }}
    cancel-in-progress: true
  ```
- **Path filters on both triggers** (module paths + the workflow file itself), so unrelated PRs
  don't run heavy jobs:

  ```yaml
  on:
    pull_request:
      paths: ['src/**', '.github/workflows/ci.yml']
    push:
      branches: [main]
      paths: ['src/**', '.github/workflows/ci.yml']
  ```
- **Cheapest runner that works.** `ubuntu-latest` bills at half the rate of `windows-latest`;
  move jobs off Windows unless they test Windows-specific behavior.
- **The big lever: the `run-ci` label gate.** When quota still hurts, gate every PR-triggered job
  so it runs only when the PR carries the `run-ci` label; pushes to `main` and tag pushes always
  run. Add `labeled` to the trigger types and the gate condition to every job:

  ```yaml
  on:
    pull_request:
      types: [opened, synchronize, reopened, labeled]
  jobs:
    test:
      if: github.event_name != 'pull_request' || contains(github.event.pull_request.labels.*.name, 'run-ci')
  ```

  Label the tip of a stacked chain, or any PR whose diff deserves CI before merge. Local
  validation stays mandatory regardless of the label. Only safe when no required status check
  would block merges on skipped jobs; a summary job with `if: always()` must treat `skipped`
  dependencies as passing.

## Pitfalls

- Migrating queue notes without re-verifying them files ghost work; several always turn out
  already done or impossible.
- Recording the PR URL in the session log, or committing after the PR exists, splits the record -
  the issue links the PR via `Closes #<n>`; let it.
- In a stacked chain, deleting a merged base branch without checking the child PR's base can
  merge the child sideways - retarget up front (AGENTS.md "Stacked runs").
