---
description: Continue development by picking the next task per AGENTS.md (announce the pick first)
argument-hint: "[optional: an issue number, queue note, or task to work]"
---

Continue development. Follow AGENTS.md strictly.

1. Read AGENTS.md, then the work queue (GitHub mode: `gh issue list`, and read the picked issue
   in full including comments; local mode: `docs/QUEUE.md`), then `docs/ROADMAP.md`, then list
   `docs/sessions/active/` (read any session whose scope overlaps, to avoid collision).
2. Decide the work and TELL ME what you picked + why BEFORE coding:
   - If `$ARGUMENTS` is non-empty, work that.
   - Else pick the next queue item that fits the current ROADMAP phase (deferred items whose
     defer-until condition is now current go first).
   - If two candidates are reasonable, ask me which (don't pick silently).
3. Open a session: cut the branch from the mainline (GitHub mode: `git fetch --prune origin &&
   git switch -c <n>-<slug> origin/main`) + copy `docs/sessions/TEMPLATE.md` to
   `docs/sessions/active/<date>-<slug>.md` and fill `scope` (declaring scope CLAIMS the item -
   don't edit the queue to claim).
4. If a decision that wasn't settled at session start surfaces, STOP and ask me in plan mode -
   don't guess.
5. Test-first incl. bad paths. Run tests/typecheck per AGENTS.md instruction.
6. On close (done-criteria met + green): follow the AGENTS.md definition of done for the current
   mode - GitHub mode: merge `origin/main` in, promote learnings, commit the close-out, push the
   branch, `gh pr create` with `Closes #<n>`, and stop for owner review; local mode: merge the
   branch to `main` LOCALLY (NEVER push), promote learnings, archive the session, and report
   what landed.
