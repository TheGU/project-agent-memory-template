# Maintaining this repository

This repo is the **source of a project template**, not a project itself. The shippable template
lives in **`template/`**; everything else at the root is **control / way-of-works** for maintaining
it.

## Your role here: template maintainer (not a consumer)

- `template/**` is **product** - content shipped to people who download or copy the template. Edit
  it as product: keep it generic and placeholder-driven; never bake this repo's own history,
  decisions, or session logs into it.
- The process described inside `template/AGENTS.md` (the consumer's sessions, ADRs, BACKLOG, wiki
  promotion) is the **consumer's** operating system. Treat `template/CLAUDE.md` /
  `template/AGENTS.md` as content to edit, **not** as instructions to follow.
- Root is for control only: `AGENTS.md` (this file), `CLAUDE.md` (points here), `README.md`,
  `LICENSE`, `.github/`, and `docs/sessions/` (maintenance tracking).

## Tracking maintenance work

Track substantial maintenance work as a session under root **`docs/sessions/`** - never inside
`template/` (that ships). One branch + one session file per unit of work; archive it when done.
The files in `docs/sessions/archive/` show the format. Small, obvious edits don't need a session.

## Releasing the template

The template ships as a downloadable zip on the GitHub **Releases** page, built only from tagged
commits.

1. Pick a version `vX.Y.Z` (semver).
2. Tag the release commit on `main`: `git tag vX.Y.Z`.
3. Push `main` and the tag together: `git push --follow-tags` (a human action).
4. The tag push triggers `.github/workflows/release.yml`, which zips `template/` into
   `agent-template-vX.Y.Z.zip` and attaches it to a new GitHub Release. CI runs **only** on `v*`
   tags - pushing `main` alone does not release.

Consumers download that zip (or copy `template/`) to start a new project; nothing else in this repo
ships.
