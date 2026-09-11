# 005: Repository import

| Field | Value |
|---|---|
| Status | specced |
| Phase | 1 |

## Problem

Import is the seam where a mature repository becomes a project. The analysis produces the mocked version of the repository (frontend plus mocked backend) and the design-system harness (tokens, component inventory, rules) that generation works from. In the POC the analysis is fully mocked: fixture results derived from the real repository, so import is deterministic and offline.

## Goals

- A repo picker: a small mocked list of repositories (Colorkrew/quiz among them); pick one to import.
- An import screen shows the analysis results: detected tokens, the component inventory, and a mocked-version summary; then the project opens.
- The analysis is fully mocked in the POC: fixture results derived from the real repository; nothing is parsed at runtime.
- Import creates the project's mocked version (frontend plus mocked backend behind the shared handlers) and produces the generation harness (product spec Section 7, D-16). For existing projects this harness is detected and approximate by nature.
- Import is where design-system detection lands: tokens, the component inventory, and the usage rules.

## Non-goals

- Real GitHub API access or repository cloning.
- Real parsing or analysis at runtime (deferred; post-POC candidate).
- Auth, repository permissions, or private repositories.

## User stories

- US-1 (P1): I pick Colorkrew/quiz from the repository list and start an import.
- US-2 (P1): I see the analysis results (detected tokens, component inventory, mocked-version summary) before opening the project.
- US-3 (P1): After import, the project opens ready for generation, with the harness in place.

## Acceptance criteria

- AC-1 (product AC-8): importing the quiz fixture shows a mocked repo analysis with detected tokens and a component inventory.
- AC-2: the picker lists mocked repositories; selecting one runs the import deterministically (replay).
- AC-3: the import screen shows the analysis results and leads into the project workspace.
- AC-4: the import produces the generation harness (Section 7): the inventory, tokens, and rules.

## Mock contract

- Fixtures: the mocked repository list; the mocked analysis result for quiz (derived from the real checkout); the mocked-version summary. No network calls in the POC.
- Endpoint shapes and handler definitions are finalized in plan.md.

## Swap seam

One module boundary, for example `apps/web/src/features/import/data/`, exporting typed functions the UI imports (listRepositories, importRepository, getAnalysis). Phase 1 binds them to MSW handlers and fixtures; Phase 2 can replace the internals with real analysis behind the same functions, with no changes to the UI or its imports.

## Open questions

- Post-POC: partially real detection (parse tokens and the component inventory from a checkout).
- Whether the mocked repository list should include additional entries.
