# Agent Template

An opinionated, docs-first **way-of-works scaffold** for AI-agent-assisted software projects:
tiered memory (`AGENTS.md` + `docs/`), a session/branch workflow, a scoped wiki, and a curated set
of agent skills. Clone-and-go.

## Use it

Two ways to get the template:

- **Download (recommended):** grab the latest `agent-template-vX.Y.Z.zip` from the
  [Releases](../../releases) page and unzip it - the contents are your new project skeleton.
- **Copy the folder:** copy this repo's [`template/`](./template) directory into a new project.

Then open it and fill in the placeholders (start with `AGENTS.md`, `docs/ROADMAP.md`, and
`docs/ARCHITECTURE.md`).

## Repo layout

| Path | What it is |
|------|------------|
| `template/` | The shippable template - **this is what you use.** |
| `README.md` | This file - about the template repo. |
| `CLAUDE.md` | Way-of-works for **maintaining** the template (for agents/maintainers). |
| `.github/workflows/release.yml` | CI: on a `v*` tag, zips `template/` and publishes a Release. |
| `LICENSE` | Repo license. |

## Maintaining / releasing

See [`CLAUDE.md`](./CLAUDE.md). In short: tag `vX.Y.Z`, `git push --follow-tags`, and CI publishes
the zip to the Releases page.
