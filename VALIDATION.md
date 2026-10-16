# Present-snapshot validation and reconciliation

Assessment date: 2026-10-16. Historical Phase 9 evidence has not been rewritten.

## Exact package delta

Historical `package.json` SHA-256: `f37ce13a22a35a9144a5e3fb9e435e0109a8ae3e8497576e17264842c155b61a`.

Present SHA-256: `51015eaf0495f73e211959cf646d06c41556646e1ffde8c1ff023c0b80c5af55`.

Removing only the `demo:reset` script entry (`tsx scripts/reset-showcase.ts`) and its separating comma exactly reproduces the historic byte hash. This is stronger evidence than an inferred semantic diff. The command is intentional showcase tooling: the Visual Apex documentation describes deterministic showcase resets. Its script rejects non-development/test mode and non-`relay_dev`/`relay_test` database names. Adding a package script does not alter production startup or dependency declarations; executing the reset remains a deliberate local action and was not part of this validation.

The canonical pnpm lockfile still matches its historical critical hash. An isolated copy of the ten explicit workspace/package/lock inputs passed `pnpm install --lockfile-only --frozen-lockfile --ignore-scripts`, exit 0. An earlier offline attempt failed because registry policy metadata was not cached; it was not a lock mismatch. The successful check used network metadata and did not install dependencies or rewrite the canonical lockfile. Installed pnpm is 11.25.0; metadata specifies 11.19.0.

## Current checks

| Check | Result | Scope |
| --- | --- | --- |
| Test database | Fresh isolated PostgreSQL 15, loopback-only, documented port 55433 | Only `relay_test`; existing dev/production state was not reset |
| Migration | PASS | Existing `db:migrate` command on the new empty database |
| Vitest | **26 tests / 12 files passed**, exit 0 | Existing smoke, integration, tenancy and concurrency suite |
| TypeScript | PASS, exit 0 | Existing `pnpm typecheck` |
| Build | PASS, exit 0 | Existing `pnpm build` for API/worker/web |
| Historical critical hashes | 17 match / 18; package delta explained above | No historical manifest refreshed |

An initial `db:reset` attempt on the pristine database failed because its down-migration expected `schema_migrations`. The documented migration command then initialized the fresh database and the full suite passed. No canonical reset-script change was needed or made. The isolated database was stopped after validation. Original Phase 9 Docker, backup/restore and restart tests were not rerun; current tests are not a replacement for those historical results.

## Boundaries

Passing tests do not certify cloud/scale/customer deployment or exactly-once external effects. Runtime service roles use BYPASSRLS. Local dependency/security and rights evidence remains bounded; no fresh RELAY dependency-cleanliness claim is made. Private logs, SQL data and test storage are not exported.
