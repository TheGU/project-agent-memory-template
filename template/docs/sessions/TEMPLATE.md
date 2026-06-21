---
date: YYYY-MM-DD
branch: <branch>
status: active # active | deferred | blocked | done
scope:
  phase: <phase>
  backlog_items: [<claimed item(s)>]
  modules: [<dirs you will edit>]
---

# <title>

Copy to `active/YYYY-MM-DD-hhmm-<slug>.md`, fill `scope`, read other `active/` files first. On close:
promote (AGENTS.md), set `status: done`, move to `../archive/`.

## Goal / Problem Statement

<the one outcome / problem statement>

## Scope

<scope>

## Task Breakdown

- [ ] <task>

## Log (append-only)

- HH:MM — <did / decided / found>

## Handoff (keep current — what the next agent reads)

- Done: <+ commit refs>
- Half-done / how to continue: <state + next step>
- Next: <single next thing>
- New gotchas: <promote on close>
- Verify with: <commands/tests>
