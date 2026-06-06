---
date: 2026-06-06
branch: main
status: done # active | deferred | blocked | done
scope:
  phase: docs
  backlog_items: [direct-user-request: document the `/next` Claude command in `README.md`]
  modules: [README.md, .claude/commands/next.md]
---

# Document `/next` Claude command in README

## Goal / Problem Statement

Add `README.md` documentation that explains how to use the custom Claude `/next` command defined in `.claude/commands/next.md`.

## Scope

Update `README.md` only to describe what `/next` does, how to invoke it, and that adopters should copy the `.claude/` directory.

## Task Breakdown

- [x] Add `/next` command usage docs to `README.md`
- [x] Verify the documentation matches `.claude/commands/next.md`

## Log (append-only)

- 00:00 — Read `AGENTS.md`, `docs/BACKLOG.md`, `docs/ROADMAP.md`, and checked `docs/sessions/active/`; confirmed there is no overlapping active session.
- 00:00 — Identified the task as a direct user request to document the custom Claude `/next` command in `README.md`.
- 00:00 — Updated `README.md` to document `.claude/commands/next.md`, including what `/next` does, example usage, and the need to copy `.claude/` during adoption.
- 00:00 — Re-triaged the placeholder backlog while closing; no deferred items target the current phase, so no backlog changes were needed.
- 00:00 — Ran `git --no-pager diff --check`; no diff errors, only existing LF/CRLF warnings from the repository working copy.

## Handoff (keep current — what the next agent reads)

- Done: Added `/next` documentation to `README.md` and updated the adoption guide to include `.claude/`.
- Half-done / how to continue: Nothing.
- Next: Optional follow-up is documenting any future `.claude/commands/` files in the same section.
- New gotchas: None.
- Verify with: `git --no-pager diff --check`
