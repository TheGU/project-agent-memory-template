---
description: Dependency + CVE refresh session; apply safe updates in one PR, file migration items for the rest
---

Run a dependency and security refresh session: collect every update and vulnerability signal,
apply the safe updates in one session, and turn the risky ones into migration queue items. Run
monthly, or immediately when a Dependabot or CVE alert arrives.

This is a full session per AGENTS.md - Sessions: a queue item, a branch, a session file, closed
per the current mode's definition of done. Local mode: the queue item is a `QUEUE.md` note and
the GitHub-side signals (Dependabot, `gh`) are unavailable - run the local audit tools only.

Steps:
1. Open the session: queue item titled "Tech refresh YYYY-MM" (plus the triggering alert if any),
   branch cut from the mainline, session file from TEMPLATE.
2. Collect signals; record every finding in the session file:
   - GitHub mode: `gh api "repos/{owner}/{repo}/dependabot/alerts?state=open"` (security alerts
     plus their fixed versions).
   - The stack's dependency audit tooling: `bundler-audit` (Ruby), `npm audit` / `pnpm audit`
     (Node), `pip-audit` (Python), `cargo audit` (Rust), `govulncheck` (Go), as applicable.
   - The stack's outdated-package listing (`bundle outdated`, `npm outdated`,
     `pip list --outdated`, `cargo outdated`, ...) - available updates, not just vulnerable ones.
   - Container image, when a Dockerfile exists: `trivy image <image>` if trivy is installed, else
     `docker scout cves`; if neither is available, note the skip. Separately check whether the
     base image tag has a newer patch release.
   - EOL horizon: check endoflife.date for the language runtime, framework, database, and base
     image; flag anything within about 6 months of end of support.
3. Classify each finding:
   - **Apply now**: patch/minor bumps, and security fixes within the current major.
   - **Migration item, do not apply**: major version bumps and EOL/EOS items. One queue item per
     migration with the breaking-change sketch and the date that makes it urgent; reference the
     alert. (GitHub mode: `gh issue create`.)
   - **Dismiss with justification**: not exploitable here (dev-only dependency, unused code
     path). Write the reasoning in the session file; GitHub mode: dismiss the Dependabot alert
     via `gh api -X PATCH` with a `dismissed_reason`.
4. Apply the "apply now" set, respecting the repo's AGENTS.md gotchas about lockfiles, frozen
   images, or pinned toolchains.
5. Validate: full test suite + lint green; re-run the step 2 audits to confirm they come back
   clean; rebuild and boot the container if the Dockerfile or a native dependency changed.
6. Close per the AGENTS.md definition of done for the current mode. The PR description (or local
   merge report) lists each bump as `package old -> new` with the alert or CVE it resolves, and
   links the migration items it filed.
