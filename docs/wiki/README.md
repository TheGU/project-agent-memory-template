# docs/wiki/

Scoped how-it-works knowledge, loaded only when working that area (keeps it out of always-loaded
files while staying findable).

- One topic per page (`auth.md`, `api-testing.md`).
- Reachable two ways: a trigger in `AGENTS.md` wiki index, and a link from the module `README.md`.
- Created/updated by the promotion step on session close; update the page when the code it
  describes changes.

Placement: global always-true → `AGENTS.md` Gotchas. Scoped → here. A decision (why) →
`DECISIONS.md`. One-off task detail → the session file.

Page format:
```markdown
# <Topic>
**When you need this:** <trigger>
## Summary
<the essential thing, 1–3 sentences>
## Steps / details
<commands, config, sequence>
## Pitfalls
<mistakes that cost time>
```
