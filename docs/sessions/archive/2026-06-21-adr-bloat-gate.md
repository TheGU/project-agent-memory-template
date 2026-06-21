---
date: 2026-06-21
branch: maint/adr-bloat-gate
status: done # active | deferred | blocked | done
scope:
  phase: docs
  backlog_items: [direct-user-request: prevent DECISIONS.md ADR bloat in consumer projects]
  modules: [template/docs/DECISIONS.md, template/docs/ARCHITECTURE.md, template/AGENTS.md, template/README.md, template/docs/wiki/README.md]
---

# Add an ADR gate to the template so DECISIONS.md cannot bloat

## Goal / Problem Statement

A project that adopted this template grew its `DECISIONS.md` to ~100 ADRs. Root cause: the agent
treated implementing the standard/default as a decision to record, so every "do X minimally / defer
X / X for now" became an ADR. The template shipped no rule for what earns an ADR, so the generator
ran unbounded. Capture the core thinking (generic, no project context) that prevents this: turn off
the generator at the source rather than curate the output.

## The two levers (general, not project-specific)

1. Declare a baseline (the standard/default the system implements) so "standard vs. fork" is
   decidable. Without it, every implementation choice looks like a fork.
2. ADR gate: an ADR records only a genuine fork (a choice with a named rejected alternative) or a
   deliberate deviation from the baseline. Standard work is described in the wiki; a deferral is a
   backlog item with a `target:`.

## Scope

Edit template docs only; keep it generic and placeholder-driven. No curation/archive process (that
manages bloat instead of preventing it).

## Task Breakdown

- [x] `template/docs/DECISIONS.md` - add the ADR gate to the header (genuine fork only).
- [x] `template/docs/ARCHITECTURE.md` - add a "Baseline" prompt as the reference point.
- [x] `template/AGENTS.md` - sharpen the Promote-on-close row from "Durable decision" to "genuine
      fork / deviation from the baseline".
- [x] `template/README.md` - align the promotion list label and the bootstrap prompt (seed only
      genuine forks; declare the baseline in ARCHITECTURE).
- [x] `template/docs/wiki/README.md` - align the placement guide ("a genuine fork (why) ->
      DECISIONS.md").
- [x] Verify no other template file primes unconditional ADR creation.

## Log (append-only)

- 00:00 - Read root + template `AGENTS.md`, the affected docs, and the archived session format;
  confirmed no overlapping active session.
- 00:00 - Grepped `template/` for `ADR|DECISIONS|decision`; identified every place that primes ADR
  creation (DECISIONS header, AGENTS promote table, ARCHITECTURE stack note, README promotion list +
  bootstrap prompt, wiki placement guide).
- 00:00 - Applied the gate across those five files; left the ADR-001 example as the model of a
  genuine fork (rejected: single `STATE.md`).

## Handoff (keep current - what the next agent reads)

- Done: ADR gate added across the five template docs above, committed on `maint/adr-bloat-gate`
  and merged into local `main`. Not pushed (push to `main` is a human action).
- Half-done / how to continue: none.
- Next: none required. Optional - when this template next ships, mention the ADR gate in RELEASE
  notes.
- New gotchas: none.
- Verify with: read `template/docs/DECISIONS.md` header and `template/docs/ARCHITECTURE.md`
  "Baseline" section; grep `template/` for `decision`/`ADR` to confirm no unconditional ADR prompts
  remain.
