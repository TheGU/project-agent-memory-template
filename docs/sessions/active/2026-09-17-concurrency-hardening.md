---
date: 2026-09-17
branch: concurrency-hardening
status: done # active | deferred | blocked | done
scope:
  phase: docs
  backlog_items: [direct-user-request: concurrent sessions must not conflict on shared files; confirm ADRs are gone]
  modules: [template/AGENTS.md, template/README.md, template/docs/ARCHITECTURE.md, template/.claude/commands/, docs/way-of-works-report.md, RELEASE.md]
---

# Concurrency hardening + last decision-record vestiges

## Goal / Problem Statement

The owner reported two problems from running agents concurrently on template-derived projects:
every branch adds or edits the same tracking files (session history, task list, decision log,
release notes), so PRs always conflict and nobody knows when the session archive may be deleted;
and tracking decisions as numbered ADRs rots (supersession chains, code comments citing numbers
that are no longer true). The owner's other project resolves the first by having every new
session delete the archive, so each PR carries its own session file.

## Findings

- The archive-as-hand-off-buffer rule (sweep at start, close-out rides in the PR) already
  landed in commit 651065b and matches the owner's practice. ADRs were removed in e53a6e0 and
  labeled decision records in e41e629.
- Gaps that remained: "decision record" wording still in `template/AGENTS.md` Guardrails and
  `template/docs/ARCHITECTURE.md` Baseline; `template/README.md` still described the archive as
  a history log and its start-session step lacked the sweep; `/next` and `/housekeeping` steps
  lacked the sweep or would delete `.keep`; the resume-a-closed-session sentence invited a
  `git mv` out of `archive/`, which is a rename/delete conflict against every in-flight branch
  that swept; and the shared list files that concurrent branches still append to (wiki index,
  Gotchas, `QUEUE.md`, `ARCHITECTURE.md` lists) had no merge rule at the definition of done.

## What landed

- `template/AGENTS.md`: sweep leaves `.keep`; `archive/` declared write-once with the
  copy-not-move resume rule; definition of done (both modes) gains the list-file merge rule
  (union of both sides, one line where both touched the same item, never for structured files);
  Guardrails heading and example lose "decision record"; comment pointers cite a rule by name
  and page, never by code.
- `template/docs/ARCHITECTURE.md`: Baseline placeholder routes deviations to the wiki page, not
  a decision record.
- `template/README.md`: archive described as a hand-off buffer; start-session step sweeps;
  close step says the next session deletes the file.
- `template/.claude/commands/next.md`, `housekeeping.md`: sweep step present, `.keep` kept.
- Maintainer side: `docs/way-of-works-report.md` Lesson 11 sentence corrected and Lesson 12
  added; `RELEASE.md` Unreleased bullet.

## Decisions made in this session

- Kept the shared list files (index, Gotchas, queue) rather than deriving them: progressive
  disclosure needs a static catalog, and a merge rule is cheaper than a generator.
- Did not adopt the "every binding rule names what enforces it" idea from the owner's pasted
  discussion: it is doc-quality policy unrelated to conflicts or ADRs and would put a new
  obligation on every wiki page. Left for the owner to decide.

## Handoff

- Done: all edits on `concurrency-hardening`, merged to local `main`; tree clean.
- Not pushed (maintainer releases are a human action: `/release`).
- Verify with: `grep -rn "decision record" template/` (no hits) and a read of
  `template/AGENTS.md` Sessions + Definition of done.
