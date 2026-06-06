---
description: Continue development by picking the next task per AGENTS.md (announce the pick first)
argument-hint: "[optional: a specific backlog item or task to work]"
---

Continue development. Follow AGENTS.md strictly.

1. Read AGENTS.md, then `docs/BACKLOG.md` → `docs/ROADMAP.md` → list `docs/sessions/active/`
   (read any session whose scope overlaps, to avoid collision).
2. Decide the work and TELL ME what you picked + why BEFORE coding:
   - If `$ARGUMENTS` is non-empty, work that.
   - Else, first pick any deferred BACKLOG item whose `target:` is the now-current phase/gate.
   - Else the next unchecked item in the current phase's "Next —" section.
   - If two candidates are reasonable, ask me which (don't pick silently).
3. Open a session: branch + copy `docs/sessions/TEMPLATE.md` to
   `docs/sessions/active/<date>-<slug>.md` and fill `scope` (declaring scope CLAIMS the item —
   don't edit BACKLOG to claim).
4. If a decision that wasn't settled at session start surfaces, STOP and ask me in plan mode —
   don't guess.
5. Test-first incl. bad paths. Run tests/typecheck per AGENTS.md instruction.
6. On close (done-criteria met + green): merge `main` into the branch, merge the branch to `main`
   LOCALLY (NEVER push), promote learnings, re-triage the deferred backlog (promote any item whose
   `target:` is now current), set `status: done`, archive the session, and report what landed.
