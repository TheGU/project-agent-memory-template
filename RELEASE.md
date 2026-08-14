# Release Notes

Changelog for **agent-template**. Each release publishes the `template/` directory as
`agent-template-<version>.zip` on the [Releases](../../releases) page, built by
`.github/workflows/release.yml` when a `v*` tag is pushed.

Newest first. The `release` skill stamps the **Unreleased** section with the chosen version and
date; the release workflow uses that section as the GitHub release notes.

## Unreleased

- Production way-of-works: the template now defines two modes - local mode (day zero, no remote)
  and GitHub mode (the destination state: issue-first, branch per issue cut from `origin/main`,
  push + PR with `Closes #<n>`, owner merges and deletes the branch). A new wiki page
  `going-production.md` is the one-time flip checklist plus CI cost-control patterns (path
  filters, concurrency, the `run-ci` label gate, tag-time `gh release create --generate-notes`).
- `docs/BACKLOG.md` replaced by `docs/QUEUE.md`: issue-shaped local-mode notes that convert
  mechanically to GitHub Issues at the flip; in GitHub mode the queue is the issue tracker.
- `docs/DECISIONS.md` removed: a decision (same gate - a genuine fork with a named rejected
  alternative, or a baseline deviation) is folded into the relevant `docs/wiki/` page and any
  config it touches, so the docs state the current truth where the read-gate surfaces it;
  rejected alternatives are named inline only where they matter.
- New comment-hygiene rule in `AGENTS.md` (no issue/PR numbers, session context, or tool versions
  in code comments), stacked-PR guidance with the up-front-retarget caution, close-out-before-PR
  session rules, and the docs-stay-current review-blocker rule.
- Recurring maintenance skills: `/tech-refresh` (dependency + CVE refresh - monthly or on a
  security alert; patch/minor updates applied in one session, major bumps and EOL items filed as
  migration queue items) and `/housekeeping` (quarterly clutter sweep, docs-drift fixes, and
  reflection over recently merged work), listed in a new `AGENTS.md` "Recurring maintenance"
  section.
- Session-archive retention: the archive is a hand-off buffer. Close still moves the session file
  to `docs/sessions/archive/` so the record rides in the PR (or the local merge commit); every
  new session deletes archive leftovers as part of its first commit - git history retains the
  files (`git log --diff-filter=D`), and a closed session is resumed by restoring its file into
  `active/`.
- Rationale: `docs/way-of-works-report.md` (maintainer side).

## v0.0.2 - 2026-06-21

- ADR gate: `DECISIONS.md` now records only a genuine fork (a choice with a named rejected
  alternative) or a deliberate deviation from the baseline; standard work goes to the wiki and
  deferrals to the backlog. Prevents the decision log from bloating.
- `ARCHITECTURE.md` gains a Baseline prompt (the standard-vs-fork reference point); `AGENTS.md`,
  `README.md`, and `docs/wiki/README.md` align to the gate.

## v0.0.1 - 2026-06-21

- Restructure: the shippable template moved under `template/`; the repo root now holds maintainer
  control (`AGENTS.md`, `CLAUDE.md`), the release CI, and maintenance `docs/sessions/`.
- Wiki adopts OKF-style YAML frontmatter with an on-demand `docs/wiki/index.md` catalog.
- Add `.github/workflows/release.yml`: zips `template/` and publishes a GitHub Release on `v*` tags.
- Add the `release` skill to automate version bump, RELEASE.md stamping, tagging, and push.
