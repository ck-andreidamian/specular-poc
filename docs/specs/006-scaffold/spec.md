# 006: From-scratch scaffold

| Field | Value |
|---|---|
| Status | specced |
| Phase | 1 |

## Problem

Not every project starts from a mature repository. The from-scratch path creates a brand-new project with the minimum it needs to be designed against: a small design system (a few components plus tokens and rules), a mocked backend, and the same generation loop. It proves the product is not quiz-specific.

## Goals

- Create from scratch, name it and go: enter a project name; the scaffold and its mocked backend are provisioned; a brief summary, then the workspace opens.
- The scaffold carries a small kit (card, button, text field), tokens, and usage rules, so generated UI is constrained the same way as in an imported project.
- The scaffold produces its generation harness (product spec Section 7, D-16); as a new project, its harness is fully known and owned by specular.
- The canonical demo: a workspace overview card showing member count with an "Invite member" button (2 members, workspace "Acme").

## Non-goals

- Template galleries or scaffold customization in the POC.
- Real backends (the scaffold's backend is mocked).
- Importing or cloning repositories (that is 005).

## User stories

- US-1 (P1): I create a project by naming it; the scaffold is provisioned and the workspace opens.
- US-2 (P1): The scaffold's small kit constrains generated UI the same way an imported project's harness does.
- US-3 (P1): The from-scratch path demonstrates the same loop as the quiz path, on a smaller surface.

## Acceptance criteria

- AC-1 (product AC-9): creating a project from scratch provisions the minimal scaffold and its mocked backend, and generation targets it.
- AC-2: the scaffold includes the small kit (card, button, text field), tokens, and the usage rules.
- AC-3: the create flow is name-and-go: name, provision, summary, workspace.
- AC-4: the scaffold produces a generation harness (Section 7, D-16).

## Mock contract

- Fixtures: the scaffold's small kit sources, tokens, and its mocked backend handlers (the workspace overview with 2 members, "Acme").
- Endpoint shapes and handler definitions are finalized in plan.md.

## Swap seam

One module boundary, for example `apps/web/src/features/scaffold/data/`, exporting typed functions the UI imports (createProject, getScaffold). Phase 1 binds them to MSW handlers and fixtures; Phase 2 replaces the internals with real provisioning behind the same functions, with no changes to the UI or its imports.

## Open questions

- Which exact components beyond card, button, and text field the scaffold ships with.
