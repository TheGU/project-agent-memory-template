# AGENTS.md

Read first, every session. Rules, not explanation — the *why* is in `docs/DECISIONS.md`.

## Docs — read in this order, scoped to your task (don't survey the tree)
1. `AGENTS.md` — rules + wiki read-gate (this file; catalog is `docs/wiki/index.md`).
2. `docs/BACKLOG.md` — next tasks + open questions; pick work here.
3. `docs/ROADMAP.md` — current phase + its scope/done-criteria.
4. `docs/sessions/active/` — list it; read any session whose scope overlaps yours.
5. `docs/ARCHITECTURE.md` — only the section for your module.
6. `docs/wiki/` — open `docs/wiki/index.md` to find the page your task triggers, then read it.
7. `docs/DECISIONS.md` — only if near a logged decision.
8. Then the code — orient via module `README.md`s and tests.

Ground truth is code + passing tests. If a doc disagrees, fix the doc.

## Sessions
- One git branch + one session file per unit of work.
- Start: copy `docs/sessions/TEMPLATE.md` → `docs/sessions/active/YYYY-MM-DD-<slug>.md`, fill
  `scope`. Declaring scope claims a BACKLOG item — don't edit BACKLOG to claim. Read `active/`
  first to avoid overlap.
- One git branch per session; **commit freely** — use git fully in development. Commit small green
  checkpoints as you go and keep the handoff current, so a stop at any point resumes from the file alone.
- **Never `git push`** — all git work stays local; origin is never touched. (Also never add
  self-attribution trailers to commits.)
- **Git definition-of-done for a session** (at close, all local): commit your work → merge `main`
  into your working branch → resolve any conflicts → merge the working branch back into `main`.
- **Finish, then commit — never pause to ask permission to commit.** Committing is part of completing
  a unit of work: do it and the close steps autonomously, then report what landed for async review.
  Stopping mid-task to ask "should I commit?" forces an expensive full-context reload later just to do
  one trivial thing. Only stop mid-task for a genuine *unsettled decision* — never for commit/checkpoint
  permission. If the owner wants changes after review, they open a new session.
- Close when done-criteria are met and tests + typecheck are green. Do all of: the git DoD above,
  promote learnings (below), **re-triage the deferred backlog** (every deferred item must carry a
  `target:`; promote any whose target is the now-current phase/gate into the active queue), set
  `status: done`, move the session to `docs/sessions/archive/`, report what landed. Always merge on
  close — never wait for review. Adding a new ADR is normal close work, not a reason to wait.
- Do **not** merge only if: done-criteria are unmet, tests/typecheck are red, or a new decision
  surfaces that wasn't settled at session start — then stop and ask. Settle decisions in plan mode at
  session start, not before the merge.

## Promote on close (required)
| Learning | Goes to |
|---|---|
| Global, always-true gotcha/rule | `AGENTS.md` → Gotchas |
| Scoped how-it-works | `docs/wiki/<topic>.md` (full frontmatter) + a line in `docs/wiki/index.md` |
| Genuine fork (you can name the rejected alternative) or deviation from the baseline | `docs/DECISIONS.md` (new ADR; gate in its header) |
| Structure/module change | `docs/ARCHITECTURE.md` |
| Done items / new follow-ups / answered questions | `docs/BACKLOG.md` |
| Deferred item whose `target:` is now current | `docs/BACKLOG.md` (active queue) |

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


## Wiki — scoped how-it-works, loaded on demand
The catalog lives in `docs/wiki/index.md` (not inline here — keeps this always-loaded file lean).
Open it (read-order step 6) to find the page for your task, then open the page.
**Read-gate:** open a page before *substantial* work in its area — a new capability, a changed
flow/contract/schema, a refactor, or when you're unsure or hit a block.
**Skip it** for a small, well-understood change (tweak a step, copy/label, an obvious-cause bugfix).
Prefer reading the one relevant section over guessing.
When you create a page: add full frontmatter (see `docs/wiki/README.md`) and a line in
`docs/wiki/index.md`.


## Agent Skills Index — open a SKILL.md only when the task needs it
Same **read-gate** as the wiki above: open the matching SKILL.md when starting new or substantial
work in its area, or when unsure / blocked — not for a small, well-understood adjustment. The
`use when…` cues below scope *which* skill is relevant, not *whether* to read it; the gate decides that.
> [!NOTE]
> If your IDE or agent tool already has built-in skill/tool detection that automatically loads `.agents/skills`, you can delete this index section to save context.

- **[better-auth-best-practices](.agents/skills/better-auth-best-practices/SKILL.md)**: Configure Better Auth server/client, DB adapters, sessions, plugins, and env vars. Use when mentioning Better Auth, `auth.ts`, or setting up TypeScript authentication.
- **[better-auth-security-best-practices](.agents/skills/better-auth-security-best-practices/SKILL.md)**: Rate limiting, secrets, CSRF, trusted origins, secure cookies, OAuth token encryption, IP tracking, audit logging. Use to secure/harden Better Auth.
- **[create-auth-skill](.agents/skills/create-auth-skill/SKILL.md)**: Scaffold auth in TS/JS apps. Framework detection, route handlers, OAuth providers, auth UI pages. Use when adding login/sign-up.
- **[elysiajs](.agents/skills/elysiajs/SKILL.md)**: Build type-safe, high-performance backends with ElysiaJS.
- **[email-and-password-best-practices](.agents/skills/email-and-password-best-practices/SKILL.md)**: Email verification, password reset flows, password policies, custom hashing. Use for credential authentication.
- **[organization-best-practices](.agents/skills/organization-best-practices/SKILL.md)**: Multi-tenant organizations, membership/invitations, custom roles/permissions (RBAC), and teams.
- **[svelte-code-writer](.agents/skills/svelte-code-writer/SKILL.md)**: Svelte 5/SvelteKit doc lookup and code analysis. MUST be used when creating/modifying `.svelte` components or `.svelte.ts`/`.svelte.js` modules.
- **[svelte-core-bestpractices](.agents/skills/svelte-core-bestpractices/SKILL.md)**: Reactivity runes, props, events, snippets, each blocks, styles, context. MUST be used when writing or editing Svelte code.
- **[two-factor-authentication-best-practices](.agents/skills/two-factor-authentication-best-practices/SKILL.md)**: Configure TOTP authenticator apps, OTP via email/SMS, backup codes, trusted devices.
