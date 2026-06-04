# AGENTS.md

Read first, every session. Rules, not explanation — the *why* is in `docs/DECISIONS.md`.

## Docs — read in this order, scoped to your task (don't survey the tree)
1. `AGENTS.md` — rules + wiki index (this file).
2. `docs/BACKLOG.md` — next tasks + open questions; pick work here.
3. `docs/ROADMAP.md` — current phase + its scope/done-criteria.
4. `docs/sessions/active/` — list it; read any session whose scope overlaps yours.
5. `docs/ARCHITECTURE.md` — only the section for your module.
6. `docs/wiki/` — pages your task triggers (index below).
7. `docs/DECISIONS.md` — only if near a logged decision.
8. Then the code — orient via module `README.md`s and tests.

Ground truth is code + passing tests. If a doc disagrees, fix the doc.

## Sessions
- One git branch + one session file per unit of work.
- Start: copy `docs/sessions/TEMPLATE.md` → `docs/sessions/active/YYYY-MM-DD-<slug>.md`, fill
  `scope`. Declaring scope claims a BACKLOG item — don't edit BACKLOG to claim. Read `active/`
  first to avoid overlap.
- Work resumably: commit small green checkpoints; keep the handoff current as you go, so a stop
  at any point resumes from the file alone.
- Close (merged to main): promote learnings (below), set `status: done`, move to
  `docs/sessions/archive/`.

## Promote on close (required)
| Learning | Goes to |
|---|---|
| Global, always-true gotcha/rule | `AGENTS.md` → Gotchas |
| Scoped how-it-works | `docs/wiki/<topic>.md` + a trigger below |
| Durable decision | `docs/DECISIONS.md` (new ADR) |
| Structure/module change | `docs/ARCHITECTURE.md` |
| Done items / new follow-ups / answered questions | `docs/BACKLOG.md` |

## Coding Guidelines
- **Think Before Coding**: Don't assume or hide confusion. State assumptions explicitly, present tradeoffs, push back on overcomplication, and ask if unclear.
- **Simplicity First**: Write the minimum code that solves the problem. No speculative features, single-use abstractions, or unrequested flexibility.
- **Surgical Changes**: Touch only what you must. Match existing style, don't refactor/reformat unrelated code, and clean up imports/variables orphaned by your edits.
- **Goal-Driven Execution**: Define success criteria and write tests first. Break multi-step tasks into verifiable steps: `1. [Step] → verify: [check]`.

## Guardrails (never break without a new ADR; to dispute, raise in BACKLOG and stop)
- [Example: One stack: TS monorepo, modular monolith, one Postgres. No microservices, broker, or second language without an ADR.]
- [Example: Every domain row has `tenant_id` if multi-tenant; RLS enforced; no cross-tenant query outside marked operator paths.]
- [Example: Per-tenant behaviour is config with defaults, never hardcoded (primitive vs policy).]
- Finish a phase (done-criteria + tests) before starting the next.
- [Add your project-specific guardrails here (e.g. data invariants, safety boundaries, etc.)]

## Conventions
- Modules talk via ports; each has a `README.md` (responsibility, public interface, wiki links).
- Tests are the correctness contract; verify by running them; add a failing test before a fix.
- Merge `main` into your branch before merging back.
- Link, don't duplicate.

## Gotchas (global, always-true only)
_None yet._

## Wiki index (read the page before working its area; add a line when you create one)
_None yet — e.g. `auth → docs/wiki/auth.md`._
