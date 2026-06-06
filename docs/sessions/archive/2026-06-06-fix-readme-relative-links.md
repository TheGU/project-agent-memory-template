---
date: 2026-06-06
branch: main
status: done # active | deferred | blocked | done
scope:
  phase: docs
  backlog_items: [direct-user-request: fix non-portable absolute links in `README.md`]
  modules: [README.md]
---

# Fix README links to be repository-relative

## Goal / Problem Statement

Replace machine-specific `file:///...` links in `README.md` with repository-relative markdown links so the documentation works on GitHub and in any clone location.

## Scope

Update `README.md` only. Keep the change surgical and preserve the existing structure/content.

## Task Breakdown

- [x] Replace `file:///...` links in `README.md` with relative links
- [x] Verify no `file:///` links remain in `README.md`

## Log (append-only)

- 00:00 — Read `AGENTS.md`, `docs/BACKLOG.md`, `docs/ROADMAP.md`, and checked `docs/sessions/active/`; confirmed there is no overlapping active session.
- 00:00 — Confirmed `README.md` still contains 12 absolute `file:///...` links, which are not portable across GitHub or other clone locations.
- 00:00 — Replaced all absolute `file:///...` links in `README.md` with repository-relative `./...` links.
- 00:00 — Verified with `grep` that `README.md` no longer contains any `file:///` links.
- 00:00 — Ran `git --no-pager diff --check`; no diff errors, only existing LF/CRLF working-copy warnings from the repository.

## Handoff (keep current — what the next agent reads)

- Done: Converted all absolute README links to repository-relative paths.
- Half-done / how to continue: Nothing.
- Next: Optional follow-up is to scan other docs for machine-specific `file:///` links if you want the same portability elsewhere.
- New gotchas: None.
- Verify with: `grep` for `file:///` in `README.md` and `git --no-pager diff --check`
