# AGENTS.md

Read first, every session. Rules, not explanation - the *why* behind a rule lives in the
"Decision record" section of the relevant `docs/wiki/` page.

## Modes: local now, GitHub as soon as possible

This repo runs in one of two modes. **GitHub mode is the destination state**; local mode exists
only for day zero, before the repo has a remote and an owner reviewing PRs. Flip early - the
longer local mode lasts, the more queue notes and merged-without-review history you must migrate
later. The flip is a checklist: `docs/wiki/going-production.md`.

- **Local mode** (no remote yet): the work queue is `docs/QUEUE.md` (issue-shaped notes); sessions
  merge to `main` locally; never push anywhere.
- **GitHub mode** (remote + owner exist): the work queue is GitHub Issues (`gh issue list`);
  sessions end in a pushed branch + PR; the owner merges; `main` changes only via merged PRs.

## Docs - read in this order, scoped to your task (don't survey the tree)
1. `AGENTS.md` - rules + wiki read-gate (this file; catalog is `docs/wiki/index.md`).
2. The work queue - GitHub mode: `gh issue list`, then read the issue you pick IN FULL including
   comments (`gh issue view <n> --comments`) - later comments routinely supersede the body.
   Local mode: `docs/QUEUE.md`.
3. `docs/ROADMAP.md` - current phase + its scope/done-criteria.
4. `docs/sessions/active/` - list it; read any session whose scope overlaps yours.
5. `docs/ARCHITECTURE.md` - only the section for your module.
6. `docs/wiki/` - open `docs/wiki/index.md` to find the page your task triggers, then read it.
7. Then the code - orient via module `README.md`s and tests.

Ground truth is code + passing tests. If a doc disagrees, fix the doc. Any PR (or local-mode
session) that changes a flow, contract, schema, service, port, or auth behavior must update the
affected `docs/wiki/` page in the same PR - a stale doc is a review blocker.

## Sessions
- **Every unit of work starts from a queue item** - a GitHub issue (GitHub mode) or a `QUEUE.md`
  note (local mode). One item = one branch = one session file. In GitHub mode, if no issue exists,
  create it first (`gh issue create`); if `gh` is unavailable or unauthenticated, do not push or
  run `gh` - suggest ready-to-paste issue/PR text and wait for the number.
- Branch naming: GitHub mode `<issue-number>-<slug>`; local mode `<date>-<slug>`. Cut every
  branch explicitly from the mainline, never from whatever HEAD a checkout or reused worktree
  happens to have. GitHub mode: `git fetch --prune origin && git switch -c <n>-<slug> origin/main`.
  This one rule is what makes the post-merge "Delete branch" button always safe.
- Worktrees (optional, for parallel sessions): one per item, created with its branch in one step
  (`git worktree add <path> -b <n>-<slug> origin/main`); removing it is part of the definition of
  done. Never reuse a leftover worktree - its HEAD is stale by definition.
- Start: copy `docs/sessions/TEMPLATE.md` to `docs/sessions/active/YYYY-MM-DD-<slug>.md`, fill
  `scope`. Declaring scope claims the item - don't edit the queue to claim. Read `active/` first
  to avoid overlap.
- **Commit freely** - small green checkpoints as you go; keep the handoff current so a stop at any
  point resumes from the file alone.
- **Never push `main`. Never force-push. Never add self-attribution trailers** (`Co-Authored-By:
  Claude`, "Generated with Claude Code", etc.) to commits or PRs. In GitHub mode, pushing the
  session branch to origin is how the PR gets made; in local mode, never push at all.
- **Finish, then commit - never pause to ask permission to commit.** Committing and the close
  steps are part of completing a unit of work: do them autonomously, then report what landed for
  async review. Only stop mid-task for a genuine *unsettled decision* - never for
  commit/checkpoint permission.

### Definition of done - GitHub mode
1. Commit your work; tests + lint/typecheck green.
2. `git fetch origin`, merge `origin/main` into your branch, resolve conflicts, re-verify green.
3. Promote learnings (table below).
4. Write the session close-out (final summary, validation results, next step: owner review), set
   `status: done`, move the session file to `docs/sessions/archive/`, and commit - BEFORE opening
   the PR, so the close-out rides inside it.
5. Push the branch. `gh pr create` referencing the issue (`Closes #<n>`).
6. Do not record the PR URL in the session log (the issue links the PR), and do not commit to the
   branch after the PR exists (review follow-ups excepted).
7. If the work ran in a worktree, remove it; the primary checkout returns to `main`. Report what
   landed.

The PR awaits owner review - **never merge it yourself**; merging is what closes the issue.
Whoever merges presses "Delete branch" immediately. PR creation is the terminal condition of a
session: local green tests alone do not end it. If validation, push, or PR creation fails, keep
the session active, log the blocker, fix in-scope problems, and retry. Do **not** open the PR if
done-criteria are unmet, tests are red, or a new decision surfaced that wasn't settled at session
start - then stop and ask. Settle decisions in plan mode at session start, not before the close.

### Definition of done - local mode
Commit your work, merge `main` into your branch, resolve conflicts, verify green, merge the
branch back into `main` locally, promote learnings, archive the session, report what landed.
Never wait for review in local mode; if the owner wants changes, they open a new session.

### Stacked runs (GitHub mode): chained PRs for a long multi-issue effort
For one long effort split across several related issues, don't race independent branches into
conflicts - run an owner-approved stacked chain (record the approval in the issues involved; this
is the only exception to cutting every branch from `origin/main`):
- Cut each issue branch from the previous issue branch's tip; open its PR with
  `gh pr create --base <previous-branch>`. Each PR then shows only its own issue's diff.
- The owner merges bottom-up, deleting each branch after its merge.
- **Caution: GitHub does not reliably retarget the next PR when a base branch is deleted** - a
  child PR can silently merge sideways into its sibling. Retarget child PRs to their final base
  up front, or verify the retarget after every bottom-up merge before deleting the branch.

## Promote on close (required)
| Learning | Goes to |
|---|---|
| Global, always-true gotcha/rule | `AGENTS.md` -> Gotchas |
| Scoped how-it-works | `docs/wiki/<topic>.md` (full frontmatter) + a line in `docs/wiki/index.md` |
| Genuine fork (you can name the rejected alternative) or deviation from the baseline | a "Decision record" entry in the relevant `docs/wiki/<topic>.md` page (gate below) |
| Structure/module change | `docs/ARCHITECTURE.md` |
| New follow-ups / answered questions | a GitHub issue or issue comment (GitHub mode); `docs/QUEUE.md` (local mode) |
| Release-worthy fix or feature | the PR title and description - they become the release notes at tag time, so write them for a reader outside the branch |

**Decision-record gate:** record only a genuine fork - a choice where you can name the
alternative you rejected and the durable reason - or a deliberate deviation from the baseline
named in `ARCHITECTURE.md`. Implementing the baseline or a framework default is description
(wiki body), not a decision; "defer X" is scheduling (queue item), not a decision. Give each
entry a dated heading (`### Decision: <slug> (YYYY-MM-DD)`) so references stay greppable.

## Coding Guidelines
- **Think Before Coding**: Don't assume or hide confusion. State assumptions explicitly, present tradeoffs, push back on overcomplication, and ask if unclear.
- **Simplicity First**: Write the minimum code that solves the problem. No speculative features, single-use abstractions, or unrequested flexibility.
- **Surgical Changes**: Touch only what you must. Match existing style, don't refactor/reformat unrelated code, and clean up imports/variables orphaned by your edits.
- **Goal-Driven Execution**: Define success criteria and write tests first. Break multi-step tasks into verifiable steps: `1. [Step] -> verify: [check]`.

### Comments
The code already says what it does. A comment earns its place only by stating a constraint the
code cannot show, or by saving the reader a trip to another file.
- Never cite issue numbers, PR numbers, plan decision codes, session context, or tool version
  numbers. All of those rot, and `git blame` plus the issue tracker already carry them.
- Durable design rationale goes in `docs/wiki/`, not in a comment block.
- When a comment starts growing into an essay, write the wiki paragraph instead and leave at most
  one pointer line behind.

## Guardrails (never break without a decision record; to dispute, raise a queue item and stop)
- [Example: One stack: TS monorepo, modular monolith, one Postgres. No microservices, broker, or second language without a decision record.]
- [Example: Every domain row has `tenant_id` if multi-tenant; RLS enforced; no cross-tenant query outside marked operator paths.]
- [Example: Per-tenant behaviour is config with defaults, never hardcoded (primitive vs policy).]
- Finish a phase (done-criteria + tests) before starting the next.
- [Add your project-specific guardrails here (e.g. data invariants, safety boundaries, etc.)]

## Conventions
- Modules talk via ports; each has a `README.md` (responsibility, public interface, wiki links).
- Tests are the correctness contract; verify by running them; add a failing test before a fix.
- Merge `main` into your branch before opening the PR (or before merging back, local mode).
- Link, don't duplicate.

## Gotchas (global, always-true only)
_None yet._


## Wiki - scoped how-it-works, loaded on demand
The catalog lives in `docs/wiki/index.md` (not inline here - keeps this always-loaded file lean).
Open it (read-order step 6) to find the page for your task, then open the page.
**Read-gate:** open a page before *substantial* work in its area - a new capability, a changed
flow/contract/schema, a refactor, or when you're unsure or hit a block.
**Skip it** for a small, well-understood change (tweak a step, copy/label, an obvious-cause bugfix).
Prefer reading the one relevant section over guessing.
When you create a page: add full frontmatter (see `docs/wiki/README.md`) and a line in
`docs/wiki/index.md`.


## Agent Skills Index - open a SKILL.md only when the task needs it
Same **read-gate** as the wiki above: open the matching SKILL.md when starting new or substantial
work in its area, or when unsure / blocked - not for a small, well-understood adjustment. The
`use when...` cues below scope *which* skill is relevant, not *whether* to read it; the gate decides that.
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
