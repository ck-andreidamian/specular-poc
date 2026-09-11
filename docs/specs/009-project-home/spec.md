# 009: Projects home

| Field | Value |
|---|---|
| Status | specced |
| Phase | 1 |

## Problem

Both entry paths, importing a mature repo and creating from scratch, need a front door. The home is where projects live: a simple list plus the two entry actions, so an evaluator can run both demo paths in one narrative and re-open any project.

## Goals

- A minimal home: a list of projects, plus two entry actions: Import repo and Create from scratch.
- Opening a project enters its workspace (chat on the left; UI | Specification switcher; handoff in the top bar).
- Re-opening a project restores it intact: conversation, spec, and mock.

## Non-goals

- Search, folders, or project management beyond the list.
- Multi-user or sharing.
- Deleting or archiving projects.

## User stories

- US-1 (P1): I land on a home that lists my projects.
- US-2 (P1): I start either path from here: import or create from scratch.
- US-3 (P1): I open a project and land in its workspace where I left it.

## Acceptance criteria

- AC-1: the home lists projects and provides both entry actions.
- AC-2: opening a project enters its workspace with the conversation, spec, and mock restored.
- AC-3: after an import or a create, the new project appears in the list.

## Mock contract

- Fixtures: a seeded project list for the demo. No network calls in the POC.
- Endpoint shapes and handler definitions are finalized in plan.md.

## Swap seam

One module boundary, for example `apps/web/src/features/projects/data/`, exporting typed functions the UI imports (listProjects, openProject). Phase 1 binds them to MSW handlers and fixtures; Phase 2 replaces the internals with the real backend behind the same functions, with no changes to the UI or its imports.

## Open questions

- What each project row shows (name only, versus name plus last-updated or path).
