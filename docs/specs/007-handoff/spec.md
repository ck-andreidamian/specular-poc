# 007: Handoff

| Field | Value |
|---|---|
| Status | specced |
| Phase | 1 |

## Problem

Handoff is where the work leaves the workspace. The generated feature plus its full spec context is pushed into the dedicated sandbox repo as a branch or PR, so the implementing AI picks it up with everything attached. The action lives in the top bar, and the real product repo is never touched.

## Goals

- Top-bar action: shows what will be pushed (a file list and a summary), then pushes.
- Pushes the generated feature code, the spec-context file, and the fixture/mock module behind one import seam (product spec Section 10).
- Branch/PR only, in the sandbox repo, under the specular namespace; never main.
- After success, a handed-off marker remains in the project (subtle; the handoff is still an operation, not an entity).
- On failure, the UI never claims success; an error with retry is shown.

## Non-goals

- Reviewing or merging PRs from the workspace.
- Multiple sandbox repos or target selection.
- Any mutation of the real product repo or the local checkout.

## User stories

- US-1 (P1): I trigger handoff from the top bar and see exactly what will be pushed.
- US-2 (P1): I confirm and get the branch/PR link plus the pushed file list.
- US-3 (P1): If the push fails, I get an error with retry and no false success.
- US-4 (P2): After success, the project shows a subtle handed-off marker.

## Acceptance criteria

- AC-1 (product AC-6): confirming handoff pushes to the dedicated sandbox repo via `GITHUB_MODE=replay`; the created ref matches `^refs/heads/specular/` and is not main; the pushed tree matches the Section 10 layout.
- AC-2 (product AC-7): a scenario trigger fails the push; an error state with retry is shown; the UI does not claim a successful push; no ref outside the namespace is created or updated.
- AC-3: before pushing, the panel shows the file list and a summary (the pre-push review).
- AC-4: after success, the handed-off marker is visible in the project.
- AC-5: preflight (`tsc --noEmit` plus `biome check` in a temp worktree) blocks the push on failure and reports it.

## Mock contract

- Phase 1: the push is simulated with `GITHUB_MODE=replay`; fixtures provide the branch/PR result and the failure scenario.
- Git operations stay server-side in Phase 2; endpoint shapes and handler definitions are finalized in plan.md.

## Swap seam

One module boundary, for example `apps/web/src/features/handoff/data/`, exporting typed functions the UI imports (prepareHandoff, pushHandoff, getHandoffResult). Phase 1 binds them to MSW handlers and fixtures; Phase 2 replaces the internals with the real GitHub client behind the same functions, with no changes to the UI or its imports.

## Open questions

- Where exactly the handed-off marker appears (project row on the home, workspace top bar, or both).
