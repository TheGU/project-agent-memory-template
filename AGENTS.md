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
- Close when done-criteria are met and tests + typecheck are green. Do all of: merge `main` into your
  branch, merge the branch to `main`, promote learnings (below), set `status: done`, move the session
  to `docs/sessions/archive/`, report what landed. Always merge on close — never wait for review.
  Adding a new ADR is normal close work, not a reason to wait. (Standing authorization to merge to
  `main`.)
- Do **not** merge only if: done-criteria are unmet, tests/typecheck are red, or a new decision
  surfaces that wasn't settled at session start — then stop and ask. Settle decisions in plan mode at
  session start, not before the merge.

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


## Wiki index — read a page only when the task needs its depth
**Read-gate:** open the page before *substantial* work in its area — a new capability, 
a changed flow/contract/schema, a refactor, or when you're unsure or hit a block. 
**Skip it** for a small, well-understood change (tweak a step, copy/label, an obvious-cause bugfix). 
When unsure, skim the headings and read only the section you touch; prefer reading over guessing. 
Add a line when you create a page.
_None yet — e.g. `auth → docs/wiki/auth.md`._


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
