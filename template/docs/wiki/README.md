# docs/wiki/

Scoped how-it-works knowledge, loaded only when a task needs it (keeps detail out of the
always-loaded `AGENTS.md` while staying findable). Format follows OKF (Open Knowledge Format):
markdown + YAML frontmatter, with an on-demand `index.md` for progressive disclosure.

- One topic per page (`auth.md`, `api-testing.md`).
- Discoverable via `index.md` (the catalog) — reached from `AGENTS.md` read-order step 6 and from
  module `README.md`s.
- Created/updated by the promotion step on session close; update the page **and** its `index.md`
  line when the code it describes changes.

## Page format

A UTF-8 markdown file: a YAML frontmatter block, then a structured body. Favor headings, lists,
tables, and fenced code over prose — structure aids both human reading and agent retrieval.

```markdown
---
type: <how-to | reference | runbook | concept>   # REQUIRED  - kind of page
title: <Display name>                             # Recommended
description: <One-line summary>                    # Recommended - this is the index + read-gate trigger
resource: <Canonical URI/path it documents>        # Optional - omit for abstract topics
tags: [<tag>, <tag>]                               # Optional
timestamp: <ISO 8601, last meaningful change>      # Recommended
---

## Summary
<the essential thing, 1-3 sentences>

## Steps / details
<commands, config, sequence>

## Pitfalls
<mistakes that cost time>
```

- **Required:** `type` (free-form; consumers tolerate unknown values).
- **Recommended, in priority order:** `title`, `description`, `timestamp`. `description` is the
  single source for the page's `index.md` line and its read-gate trigger - keep them in sync.
- Don't reject a page for missing optional fields, unknown `type`, extra keys, or broken
  cross-links (a broken link may just mark not-yet-written knowledge).

## index.md (the catalog)

`index.md` lists every page with its `description`, grouped by area, so a human or agent can see
what exists before opening anything (progressive disclosure). No frontmatter. Entry form:

```markdown
# <Area>
* [Title](topic.md) - description (mirrors the page's frontmatter `description`)
```

## Cross-links

Link related pages with standard relative markdown links (`auth.md` within the wiki,
`../ARCHITECTURE.md` to siblings). A link asserts a relationship; its kind is conveyed by the
surrounding prose. Broken links are allowed.

## Placement

Global always-true -> `AGENTS.md` Gotchas. Scoped how-it-works -> here. A genuine fork (why) ->
a "Decision record" entry in the topical page here (only a real choice with a rejected
alternative, or a deviation from the baseline; dated heading, e.g.
`### Decision: <slug> (YYYY-MM-DD)`, so references stay greppable). One-off task detail -> the
session file.
