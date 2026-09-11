# Feature Specs

Feature specs are carved just-in-time from the product spec. This file holds the
conventions and catalogs the specs that exist.

## Conventions

- One folder per feature: `NNN-slug/` (three-digit sequential number, lowercase
  hyphenated slug; e.g. `001-conversation`).
- Files: `spec.md` always (once carved). `plan.md` and `tasks.md` are added to
  the folder when the feature starts building.
- Just-in-time rule: no empty spec folders. A folder exists only once its spec is
  written.
- Statuses are tracked ONLY in `../product/roadmap.md`. Do not duplicate feature
  statuses here.
- The feature list with phases and statuses lives in `../product/roadmap.md`.
  This index only catalogs specs that exist.

## Spec shape

- Status / Phase header
- Problem
- Goals and non-goals
- User stories + acceptance criteria (reference the product spec's AC numbers
  where applicable)
- Mock contract (endpoints, handler behavior, fixtures needed)
- Swap seam (the single module boundary the backend will replace)
- Open questions

## Existing specs

- [001-conversation](./001-conversation/spec.md) — chat that produces and refines the structured spec; wiki browsing and side-by-side change review
- [002-generation](./002-generation/spec.md) — every accepted turn refreshes the mock; validated envelopes rendered live in the sandbox
- [003-iteration-sync](./003-iteration-sync/spec.md) — paired spec + mock versions per accepted turn; history retained, no browser in the POC
- [005-import](./005-import/spec.md) — repo picker and mocked analysis; produces the mocked version and the generation harness
- [006-scaffold](./006-scaffold/spec.md) — name-and-go creation with a small kit and a mocked backend
- [007-handoff](./007-handoff/spec.md) — top-bar handoff: push the feature + spec context as a branch/PR; handed-off marker after success
- [008-enforcement](./008-enforcement/spec.md) — analyzer flags violations with file/line details; flagged versions render but stay invalid
- [009-project-home](./009-project-home/spec.md) — project list and the two entry actions; opens a workspace
