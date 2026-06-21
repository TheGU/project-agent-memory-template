---
name: release
description: Cut a new template release. Determines the next version (or uses one you pass), stamps RELEASE.md, tags the commit, and pushes main + tag to trigger the GitHub release build (zips template/ to the Releases page). Use when publishing a new version of the template.
---

# Release the template

Automates publishing a new version of the template. The artifact (the `template/` folder zipped as
`agent-template-<version>.zip`) is built by `.github/workflows/release.yml`, which runs **only** when
a `v*` tag is pushed. This skill produces that tag.

**Runs autonomously, end to end - no confirmation prompts.** It assumes the release content is
already committed on `main`; it only stamps `RELEASE.md`, commits that, tags, and pushes. It does
not sweep up unrelated uncommitted changes.

## Parameter

- **version** (optional) - an explicit version like `v1.4.0` (a leading `v` is added if missing).
  - If given: use it as the tag verbatim. **Do not** look up the latest release.
  - If omitted: find the latest version and bump the **patch** number.

## Steps

1. **Safety checks.** Ensure you are on `main` and up to date: `git fetch && git status -sb`.
   Proceed automatically.

2. **Decide the version.**
   - If a version parameter was provided: `VERSION=<param>` (prefix `v` if missing), then go to step 3.
   - Otherwise find the latest released version and bump patch:
     ```bash
     LATEST=$(gh release list --limit 1 --json tagName -q '.[0].tagName' 2>/dev/null \
              || git tag --list 'v*' --sort=-v:refname | head -1)
     ```
     - If `LATEST` is empty (no releases yet) -> `VERSION=v0.1.0`.
     - Else parse `vMAJOR.MINOR.PATCH` and increment PATCH (e.g. `v0.3.2` -> `v0.3.3`).
   - Validate `VERSION` matches `v<major>.<minor>.<patch>` and proceed.

3. **Stamp RELEASE.md.** Rename the `## Unreleased` heading to `## <VERSION> - <YYYY-MM-DD>`
   (`date +%F`) and insert a fresh empty `## Unreleased` section above it. If the section has no
   bullets, summarise changes since the last tag: `git log <LATEST>..HEAD --oneline`.

4. **Commit.** `git add RELEASE.md && git commit -m "Release <VERSION>"`.

5. **Tag.** `git tag -a <VERSION> -m "<VERSION>"`.

6. **Push.** `git push --follow-tags` (pushes the new `main` commit and the tag together, triggering
   the release build).

7. **Verify.** The tag push starts the release workflow. Watch it and confirm the zip is attached:
   `gh run watch`, then `gh release view <VERSION>`.

## Notes

- Pass an explicit version for a **major/minor** release - the auto path only bumps patch.
- The published release notes come from this version's section in `RELEASE.md` (the workflow reads
  it), falling back to auto-generated notes if that section is empty.
- Never bypass hooks or signing, and never force-push. If a push is rejected, stop and report.
