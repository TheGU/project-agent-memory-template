---
description: Cleanup + reflection session; sweep clutter, fix docs drift, promote recurring lessons
---

Run a cleanup and reflection session: remove what accumulated, fix documentation drift, and
promote recurring lessons. Run quarterly, or whenever clutter is noticeable (docs contradicting
code, scratch files piling up, gotchas outgrowing AGENTS.md).

This is a full session per AGENTS.md - Sessions: a queue item, a branch, a session file, closed
per the current mode's definition of done. Local mode: reflection reads the git log instead of
merged PRs, and the staleness pass covers `QUEUE.md` notes instead of open issues.

Steps:
1. Open the session: queue item titled "Housekeeping YYYY-MM", branch cut from the mainline,
   session file from TEMPLATE.
2. Sweep clutter:
   - `docs/sessions/archive/`: delete anything present (normally already empty - every new
     session sweeps it at start; the files are merged history, recoverable from git).
   - Scratch/temp directories: list candidates in the session file and confirm with the owner
     before deleting - they may hold captures still referenced elsewhere. Never auto-delete.
   - Branches and worktrees: `git fetch --prune`, delete local branches whose PRs are merged,
     remove leftover worktrees.
3. Documentation drift pass (ground truth is code + passing tests):
   - Grep the docs for references to files, routes, settings, or queue items that no longer
     exist; fix or remove.
   - Spot-check each wiki page's key claims against the code it describes; fix the doc where it
     disagrees, delete sections describing removed behavior.
   - AGENTS.md gotchas: an entry that is no longer always-true gets deleted; one that grew past a
     few lines moves into its topical wiki page (with an index line in `docs/wiki/index.md`).
4. Reflection:
   - Skim the work merged since the last housekeeping (GitHub mode: `gh pr list --state merged`
     plus closed issues; local mode: `git log`). Look for repeated mistakes, repeated review
     comments, or recurring friction; each one becomes a gotcha line, a wiki paragraph, a skill
     update, or a new queue item.
   - Check the queue for staleness; propose closures to the owner with a one-line reason rather
     than closing unilaterally, and update items that drifted from reality.
5. Close per the AGENTS.md definition of done for the current mode. The output is doc edits,
   deletions, and filed queue items; keep the diff easy to review and put anything needing an
   owner decision in the PR description (or the report, local mode).
