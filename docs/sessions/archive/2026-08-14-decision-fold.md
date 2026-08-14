# 2026-08-14 - Decisions fold into pages, not "Decision record" entries

Branch `decision-fold`, follow-up to `process-modernization` (same day). Status: done.

## Problem

The modernization session replaced the central `DECISIONS.md` with dated "Decision record"
entries inside wiki pages. Owner review corrected the concept: that still creates labeled
decision artifacts people must cross-reference. The intent is that a decision is folded into
the affected page (and any config or scaffold it touches) so the docs simply state the current
truth - e.g. "the database is Postgres" in the architecture page and compose file, not a
"Decision: use postgres" entry anywhere. The rejected alternative is named inline, in prose,
only where that saves a future reader from re-litigating the choice.

## What changed

- `template/AGENTS.md`: intro pointer, promote-on-close row, and the gate paragraph rewritten -
  the gate still decides whether rationale is written at all (genuine fork or baseline
  deviation), but there is no record format; fold into the page and touched config, no labels,
  headings, or central log.
- `template/docs/wiki/README.md`: Placement paragraph aligned (fold into the topical page's
  prose; no decision headings).
- `template/docs/ARCHITECTURE.md`: Stack heading pointer no longer names "Decision record"
  sections.
- `template/README.md`: doc-set bullet, close checklist item, and bootstrap prompt aligned.
- `docs/way-of-works-report.md`: Lesson 3 rewritten to the corrected meaning.
- `RELEASE.md`: Unreleased bullet reworded.

## Validation

`grep -rn "Decision record\|Decision: <slug>" template/ docs/ RELEASE.md` returns no hits after
the change; diff-added lines contain no em-dash/arrow/emoji.
