# Release Notes

Changelog for **agent-template**. Each release publishes the `template/` directory as
`agent-template-<version>.zip` on the [Releases](../../releases) page, built by
`.github/workflows/release.yml` when a `v*` tag is pushed.

Newest first. The `release` skill stamps the **Unreleased** section with the chosen version and
date; the release workflow uses that section as the GitHub release notes.

## Unreleased

- Restructure: the shippable template moved under `template/`; the repo root now holds maintainer
  control (`AGENTS.md`, `CLAUDE.md`), the release CI, and maintenance `docs/sessions/`.
- Wiki adopts OKF-style YAML frontmatter with an on-demand `docs/wiki/index.md` catalog.
- Add `.github/workflows/release.yml`: zips `template/` and publishes a GitHub Release on `v*` tags.
- Add the `release` skill to automate version bump, RELEASE.md stamping, tagging, and push.
