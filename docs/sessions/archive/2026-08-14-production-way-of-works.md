---
date: 2026-08-14
branch: process-modernization
status: done # active | deferred | blocked | done
scope:
  phase: docs
  backlog_items: [direct-user-request: bake production way-of-works into the template]
  modules: [template/AGENTS.md, template/docs/, template/README.md, template/.claude/commands/next.md, docs/way-of-works-report.md, RELEASE.md]
---

# Bake the production way-of-works into the template

## Goal / Problem Statement

A project that adopted this template later had to run a dedicated "process modernization" session
to become production-ready: its backlog file had rotted (items refiled as GitHub issues only
after re-verification), its release-notes file had grown to 1809 lines before deletion, its
decision log had become an unread monolith, and issue-number comments had to be swept out of the
code. A second adopting project accrued the same debt. The template should make day one already
production-shaped so the transformation session never has to happen.

The change spec is `docs/way-of-works-report.md` (Lessons 1-10), distilled from studying the
modernized project's AGENTS.md and its two modernization session logs.

## What landed

- `template/AGENTS.md`: rewritten around two modes. Local mode (day zero, no remote: QUEUE.md
  queue, local merge DoD, never push) and GitHub mode (the destination state: issue-first, one
  issue = one branch = one session, branch cut explicitly from `origin/main`, optional
  worktree-per-issue removed at close, close-out committed before `gh pr create`, push + PR with
  `Closes #<n>`, owner merges and deletes the branch, PR creation as the terminal condition).
  Added: comment-hygiene section, stacked-runs section with the up-front-retarget caution
  (GitHub does not reliably retarget child PRs when a base branch is deleted), the
  docs-stay-current review-blocker rule, and the decision-record gate (unchanged test, new
  destination: dated "Decision record" entries in topical wiki pages).
- `template/docs/BACKLOG.md` deleted; `template/docs/QUEUE.md` added (issue-shaped local-mode
  notes, explicitly temporary, converted to issues at the flip after re-verification).
- `template/docs/DECISIONS.md` deleted; wiki README "Placement" now routes forks to a dated
  "Decision record" entry in the topical page.
- `template/docs/wiki/going-production.md` added (flip checklist + CI cost control: concurrency,
  path filters, ubuntu runners, the run-ci label gate with the exact condition, tag-time
  `gh release create --generate-notes` job) and indexed in `template/docs/wiki/index.md`.
- `template/docs/ROADMAP.md` kept (call below); its queue reference now points at issues/QUEUE.md
  and suggests milestones in GitHub mode.
- `template/docs/ARCHITECTURE.md`, `template/docs/sessions/TEMPLATE.md` (new Close-out section,
  issue frontmatter key, close-before-PR instructions), `template/.claude/commands/next.md`, and
  `template/README.md` (structure tree, workflow diagram, adoption guide, bootstrap prompt)
  aligned; grep proves no `BACKLOG`/`DECISIONS.md` references remain under `template/`.
- Maintainer side: `docs/way-of-works-report.md` (rationale record, genericized), `RELEASE.md`
  Unreleased entry.

## Decisions made in this session

- **ROADMAP.md: kept.** Phases with done-criteria are direction, not a queue; they work in both
  modes and map to GitHub milestones after the flip. Dropping it would have merged direction into
  the queue, which is the lifecycle-mixing this template exists to prevent.
- **Queue file named QUEUE.md, not retained as BACKLOG.md.** The rename marks the semantic
  change: not a phase-structured backlog with target tags, but a thin holding pen of
  issue-shaped notes that dies at the flip.
- **CI cost control folded into going-production.md** rather than a second wiki page: same
  trigger moment (repo grows a remote/owner/quota), one page reads better.
- The template ships no `.github/` workflows, so the release job and CI patterns are documented
  in the wiki page as paste-ready YAML instead of scaffold files (per the spec: do not invent a
  heavy workflow scaffold).

## Handoff

- Done: all edits committed on `process-modernization`, merged to local `main`; tree clean.
- Not pushed anywhere (maintainer releases are a human action: tag + `git push --follow-tags`).
- Next: owner review of local `main`; cut `v0.0.3` when satisfied.
- Verify with: `grep -rn "BACKLOG\|DECISIONS" template/` (no hits) and a read of
  `template/AGENTS.md`.
