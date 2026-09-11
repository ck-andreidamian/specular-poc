# 0001: Bun workspaces

| Field | Value |
|---|---|
| Status | accepted |
| Date | 2026-09-12 |

## Context

The repo is a light monorepo: `apps/web`, `packages/contracts`, `packages/mocks`, and `e2e` (with `apps/server` arriving in Phase 2). It needs one workspace tool for installs, scripts, and cross-package imports.

## Decision

Use Bun workspaces.

## Consequences

- One fast tool for installs and scripts; consistent with the founder's other projects.
- Slightly less battle-tested for the Vite + Playwright monorepo flow than pnpm. If that bites, the switch is contained (workspace globs and a lockfile).
- Vite, React, MSW, Playwright, and Vitest all run on Bun.
