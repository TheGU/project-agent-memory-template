# Way-of-works modernization report

Rationale record for the 2026-08-14 template changes. Distilled from studying a real project that
transformed a dev-state repo into a production-state repo, and from the debt a second project
accrued by adopting this template's earlier dev-state conventions.

The core insight: the studied project paid one full session to delete 1809 lines of a
release-notes file, refile 26 backlog items as issues (each needing re-verification against code
because they had rotted), rewrite an 826-line doc that was mostly wrong, and sweep issue-number
comments out of code. Every one of those costs was avoidable by adopting the production
conventions on day one. The template's job is to make day one already production-shaped.

## Lesson 1: The work queue belongs in the issue tracker, not a file

A backlog file rots. Items get done but stay listed, stale claims accumulate, and the eventual
migration forces re-verifying every item against code (several always turn out already delivered
or impossible). GitHub Issues never need migration, carry discussion, close automatically from PR
merges, and are the natural place owners and customers answer questions.

Template change: read-order step 2 is the issue queue (`gh issue list`; read the chosen issue in
full including comments - later comments routinely supersede the body). `BACKLOG.md` is replaced
by `QUEUE.md`, a thin local-mode holding pen of issue-shaped notes that convert mechanically at
the GitHub flip and are deleted with the file.

## Lesson 2: Never create a release-notes file

Release notes are generated at tag time from merged PR titles and descriptions
(`gh release create --generate-notes`). This makes PR titles a deliverable: write them for a
reader outside the branch.

Template change: the consumer template ships no release-notes file; the `going-production` wiki
page carries the tag-triggered release job. (This maintainer repo keeps its own `RELEASE.md`
because its release workflow reads the Unreleased section; that is a maintainer-side mechanism,
not shipped content.)

## Lesson 3: A central decisions file is a record, not a retrieval system

An append-only ADR log grows into a monolith nobody re-reads (one adopting project's hit ~100
entries; another 37 KB). The knowledge is only useful when it surfaces at task time, which is
what the wiki read-gate already does.

Template change: `DECISIONS.md` is removed. The gate survives unchanged (write rationale only
for a genuine fork with a named rejected alternative, or a deliberate deviation from the
baseline), but there is no record format: the decision is folded into the relevant
`docs/wiki/<topic>.md` page - and into any config or scaffold it touches - so the docs simply
state the new truth, with the rejected alternative named inline where it saves re-litigating.
Nobody should need a decision label to know the database is Postgres; the compose file and the
architecture page already say so.

## Lesson 4: Comment hygiene from day one

Issue numbers, PR numbers, plan decision codes, session context, and tool version numbers in code
comments all rot; `git blame` and the tracker already carry them. Skipping this rule costs a
dedicated sweep session later. The rule now in `template/AGENTS.md`: a comment earns its place
only by stating a constraint the code cannot show, or by saving the reader a trip to another
file; durable rationale goes to the wiki; an essay-comment becomes a wiki paragraph plus at most
one pointer line.

## Lesson 5: Branch and worktree discipline that makes cleanup free

- Cut every branch explicitly from `origin/main`
  (`git fetch --prune origin && git switch -c <n>-<slug> origin/main`), never from whatever HEAD
  a checkout or reused worktree happens to have. This one rule makes the post-merge "Delete
  branch" button always safe.
- One worktree per issue, created with the branch in one step, removed as part of the definition
  of done. Never reuse a leftover worktree; its HEAD is stale by definition.
- Whoever merges deletes the branch immediately; `git fetch --prune` at session start flags the
  dead locals. Hygiene comes from the ritual, not from cleanup scripts.

## Lesson 6: Stacked PRs for multi-issue chains

For a long effort split across related issues, racing independent branches into conflicts is the
failure mode. Instead: owner-approved stacked runs - cut each issue branch from the previous
branch's tip, open each PR with `--base <previous-branch>`, owner merges bottom-up.

Correction from this template owner's own history (2026-06-15): GitHub does NOT reliably
auto-retarget the next PR when a base branch is deleted - a child PR can merge sideways into its
sibling. Retarget child PRs to their final base up front, or verify the retarget after every
bottom-up merge before deleting the branch. The caution is written into the template's Stacked
runs section.

## Lesson 7: The session close-out rides inside the PR

Write the close-out (final summary, validation results, "awaiting owner review"), archive the
session file, and commit BEFORE `gh pr create`, so the PR contains its own record. Do not write
the PR URL into the session log (the issue links the PR via `Closes #<n>`); do not commit to the
branch after the PR exists (review follow-ups excepted). PR creation is the terminal condition of
an issue session; local green tests alone do not end it.

## Lesson 8: CI cost control patterns

Cheapest first: concurrency groups with cancel-in-progress; path filters on both `pull_request`
and `push` triggers (module paths plus the workflow file itself); the cheapest runner that works
(ubuntu bills at half of windows); and, when quota still hurts, the `run-ci` label gate - every
PR-triggered job gated on
`if: github.event_name != 'pull_request' || contains(github.event.pull_request.labels.*.name, 'run-ci')`
with `types: [opened, synchronize, reopened, labeled]`, while pushes to main and tag pushes
always run. Only safe when no required status check blocks merges on skipped jobs. All captured
in the `going-production` wiki page so the quota response is prepared, not improvised.

## Lesson 9: Make the dev-to-production transition a checklist, not a project

Local mode (never push, merge to main locally) is right for day zero; what was missing is the
flip. `template/docs/wiki/going-production.md` now lists it: remote + PR-only main, queue notes
verified and refiled as issues, DoD switch, release workflow, `run-ci` label, optional
milestones.

## Lesson 10: Docs stay current or block review

Two standing rules now in the template: any PR that changes a flow, contract, schema, service,
port, or auth behavior must update the affected wiki page in the same PR (a stale doc is a review
blocker); and when migrating or importing a doc, verify every claim against code first - never
ship carried-over prose unaudited (the studied project's 826-line guide documented directories
and class names that did not exist; the honest rewrite was 89 lines).

## Lesson 11: Maintenance is a session type, and the session archive is a hand-off buffer

Two recurring needs get first-class skills instead of ad-hoc effort. `/tech-refresh` (monthly, or
immediately on a security alert): collect every signal (Dependabot alerts, the stack's audit
tools, outdated-package listings, a container scan, the EOL horizon), apply patch/minor updates
in one session, and turn major bumps and EOL items into migration queue items - never silent
upgrades. `/housekeeping` (quarterly, or on noticeable drift): sweep clutter, fix docs drift
against code, and reflect over recently merged work for recurring mistakes worth promoting.

Retention insight behind the archive sweep: a session file is irreplaceable mid-flight
(interrupt-proof plan, progress, and hand-off state, which issue comments carry badly) and
redundant the moment the session closes - promote-on-close moved the learnings, the PR diff is
the change record, and git history retains the file itself. So the archive is a hand-off buffer,
not a library: the close still moves the file to `docs/sessions/archive/` so the record rides in
the PR's Files changed tab, and every new session deletes whatever sits there as part of its
first commit (`git log --diff-filter=D -- docs/sessions/archive` recovers any file; restore one
into `active/` to resume or fix a closed session). The archive stays near-empty automatically,
with no retention policy to remember and no wisdom lost.

## Non-goals

- The session file system (`docs/sessions/` + TEMPLATE.md) stays: it complements issues
  (issues = what/why, sessions = how/handoff).
- The wiki read-gate and promote-on-close table stay as designed; the studied project converged
  on the same design independently, which is evidence it works.
- `ROADMAP.md` stays: phases with done-criteria are direction, not a queue; in GitHub mode they
  can be mirrored as milestones.
