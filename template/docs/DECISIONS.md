# DECISIONS.md

Append-only, newest first. Don't reverse an Accepted decision; to dispute, raise in `BACKLOG.md`
and stop. This is the one place rationale belongs.

**An ADR records exactly one thing: a genuine fork.** A genuine fork is a choice where you can name
the alternative you rejected and the durable reason you rejected it. A deliberate deviation from
your declared baseline (`ARCHITECTURE.md`) counts as a fork. Everything else is not a decision:

- Implementing the baseline, a framework default, or the obvious approach is description, not a
  decision. Build it and write how it works in `docs/wiki/`. No ADR.
- "Do X minimally", "defer X", "X for now" is scheduling, not a decision. It is a `BACKLOG.md` item
  with a `target:`. No ADR.

If you cannot name the rejected alternative, there is no ADR. That test is the brake: the log grows
only with real forks, so it stays small by itself. Don't curate a bloated log; prevent it here.

## ADR-001 — Agent memory is tiered docs

**Accepted · [Date / Version]**
One `STATE.md` mixed lifecycles, grew unbounded, blocked concurrent sessions, and lost history
when trimmed. Split by lifecycle/scope: `AGENTS.md` (rules + gotchas + wiki read-gate, always
loaded; the wiki catalog is on-demand in `docs/wiki/index.md`); `docs/ARCHITECTURE.md` + `docs/wiki/` (semantic, by scope); `docs/DECISIONS.md`
(decisions); `docs/sessions/` (episodic, write-once; `active/` = in-flight intent + concurrency
board, one branch + file per session); `docs/ROADMAP.md` (plan + phase marker); `docs/BACKLOG.md`
(forward queue + open questions). Durable learnings are promoted out of session logs on close.
Cost: discipline — follow promotion + read-order or the wiki rots. BACKLOG conflicts are rare;
merge `main` first.
