# 004: Sandbox runtime

| Field | Value |
|---|---|
| Status | specced |
| Phase | 0 |

## Problem

The mock must be the application itself, not a simulation of it. The generated feature renders inside the real target app's runtime: real components, theme, API client, store, i18n. What product people see and what the developer receives are built from the same language.

This feature hosts that runtime, and it is the Phase 0 hard gate: if the sandbox cannot host the runtime, the founder re-decides before any generation work starts.

## Goals

- Host the trimmed real target runtime: allowed Kit sources, theme factory and tokens, API client, store, i18n, and the app shell that presents the application.
- Render the generated feature built into the application itself (its current state). There is no chrome "around" the feature: the mock is the application.
- Only the generated feature is interactive; the rest of the application is visual (inert) in the POC.
- The mock's API calls are served by the shared handler definitions through the transport adapter (product spec Section 9.2).
- Run fully offline, in the browser, on React 19 + MUI 7 + Emotion, with no hosted bundler service; assertable from Playwright inside the frame via `frameLocator`.
- Phase 0 spike gate: a hand-written page that uses real `KitButton` and `KitCard`, the theme, and one intercepted API call.

## Non-goals

- Full app coverage: only the runtime pieces the demo needs are trimmed.
- Pixel-level design-system fidelity guarantees (approximated and stated honestly).
- Real backend data: the target product's backend stays mocked.
- A real bundler or server-side rendering service.

## User stories

- US-1 (P1): The mock shows the application itself with the feature built into it.
- US-2 (P1): The feature's controls work against the mocked API; the rest of the app is visual.
- US-3 (P1): Everything runs offline in the browser and is assertable in automated tests.

## Acceptance criteria

- AC-1 (the Phase 0 gate): A hand-written page using real `KitButton` and `KitCard`, the theme, and one intercepted API call runs fully offline on React 19 + MUI 7 + Emotion, with no hosted bundler service.
- AC-2: Playwright can assert inside the sandbox frame via `frameLocator`.
- AC-3: The runtime hosts the application itself with the feature built in; fidelity is approximated and stated honestly.
- AC-4: Sandboxed frames work with the transport adapter (shared handler definitions; no service worker inside the frame).

## Mock contract

- The transport adapter from product spec Section 9.2 serves the mock's API calls from the shared handler definitions.
- Trim source for the mature path: the Colorkrew/quiz checkout (real Kit sources, theme factory, tokens, API client, store, i18n). The checkout is available locally.
- Endpoint shapes and handler definitions are finalized in plan.md.

## Swap seam

The runtime itself is POC infrastructure and is not swapped in Phase 2; what changes behind it is the data: the mocked handlers are replaced by the real backend behind the same contract (product spec Section 10).

## Open questions

- The exact trim list of app shell pieces for the quiz path.
- How much of the API client and store must be wired for the demo.
