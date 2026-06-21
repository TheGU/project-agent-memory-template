# Release Notes

Changelog for **agent-template**. Each release publishes the `template/` directory as
`agent-template-<version>.zip` on the [Releases](../../releases) page, built by
`.github/workflows/release.yml` when a `v*` tag is pushed.

Newest first. The `release` skill stamps the **Unreleased** section with the chosen version and
date; the release workflow uses that section as the GitHub release notes.

## Unreleased

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
