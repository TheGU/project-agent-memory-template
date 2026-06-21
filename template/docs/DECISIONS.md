# DECISIONS.md

Append-only, newest first. Don't reverse an Accepted decision; to dispute, raise in `BACKLOG.md`
and stop. This is the one place rationale belongs.

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
