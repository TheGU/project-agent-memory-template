# ARCHITECTURE.md

The map. Read only the section for your module. Link to module `README.md`s; don't duplicate.

## What it is
[Provide a brief 1-2 sentence description of the system and its primary value proposition / purpose.]

## Baseline (the default this system implements)
[Name the standard, spec, or convention this system implements by default: the domain standard, the
framework defaults, the obvious approach for your stack. This is the reference point that makes
"standard vs. fork" decidable. Implementing the baseline is description (write it in `docs/wiki/`),
not a decision; only a deliberate deviation from it earns an ADR. Without a named baseline, every
implementation choice looks like a fork and `DECISIONS.md` bloats.]

## Stack (why → DECISIONS.md)
| Layer | Choice |
|------|--------|
| Language | [e.g., TypeScript / Python / Go] |
| Runtime / API | [e.g., Bun / Elysia / Node.js] |
| DB | [e.g., PostgreSQL / SQLite / MySQL] |
| ORM / Driver | [e.g., Drizzle / Prisma / SQLALchemy] |
| Tenancy | [e.g., Shared schema with tenant_id / Single-tenant] |
| Frontend | [e.g., SvelteKit / React / Next.js / Vanilla HTML] |
| Deploy | [e.g., Modular monolith, single deployable / Containerized / Serverless] |

## Module layout
```
/apps/web            [E.g., SvelteKit frontend]
/apps/api            [E.g., API server gateway]
/modules/tenancy     [E.g., Tenant isolation & roles]
/modules/[module_a]  [E.g., Component A responsibility]
/modules/[module_b]  [E.g., Component B responsibility]
/packages/[pkg_a]    [E.g., Shared utility package]
/db                  [E.g., DB Schema and migrations]
/docs                [E.g., Documentation, session plans, and wiki logs]
```
Modules talk only through their port interface; each owns a `README.md`.

## Core entities
- **[Entity A]** — [Key business entity, its unique keys, invariants, and constraints]
- **[Entity B]** — [Key business entity, its unique keys, invariants, and constraints]

## Flow
[Input / Action trigger] → [Module / Component] → [Update database / State change] → [Result / Output / Event notification]

## Where to look
- [Feature A / Utility A] → `[path/to/feature_or_utility]`
- [Feature B / Utility B] → `[path/to/feature_or_utility]`
- What's next / in flight → `BACKLOG.md` / `session/active/`
