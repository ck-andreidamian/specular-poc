# 002: Generation and the live mock

| Field | Value |
|---|---|
| Status | specced |
| Phase | 1 |

## Problem

The spec becomes credible only when people can see it. Generation turns the structured spec into a running mock of the application with the feature applied, on every accepted turn. The UI view always shows the current state of the application; there is no separate preview mode and no diff view for the mock: the live mock is the artifact.

## Goals

- Every accepted turn produces one validated envelope (product spec Section 5) and refreshes the mock.
- The generation turn works from the project's harness (product spec Section 7): the allowed Kit inventory, theme tokens, the usage rules, and the API seam.
- The UI view shows the current state of the application with the feature applied: the application itself, hosted by the trimmed real runtime (004), with the feature built into it, rendered live in the sandbox.
- While a turn is generating, the previous mock stays visible with an "updating the mock" indicator; the view swaps when the new version validates.
- On invalid output (invalid envelope or broken TSX), the last valid mock stays on screen, an error with a Retry control appears, and no broken version is recorded as valid.
- No diff view for the mock: the mock always shows the new state. The spec keeps its side-by-side change review.
- The first accepted turn produces the first mock.

## Non-goals

- Real backend data: the target product's backend stays mocked (product spec Section 9.2).
- Pixel-level design-system fidelity guarantees (approximated and stated honestly).
- Static-analysis enforcement (008) and handoff (007).
- A change/diff view for the mock.

## User stories

- US-1 (P1): I send a message and the mock updates to the new state of the application with the feature applied.
- US-2 (P1): While it updates, I still see the previous state, with a clear "updating" indication.
- US-3 (P1): When an update fails, I keep the last good mock and get an error with Retry.
- US-4 (P1): I switch to the UI view and see the application as it looks with the feature applied.
- US-5 (P1): I never compare diffs of the mock; it always shows the current state.
- US-6 (P2): I can interact with the mock as far as the provided handlers allow.

## Acceptance criteria

- AC-1 (product AC-3): An accepted message refreshes the mocked UI in the sandbox and records a paired SpecVersion and GeneratedUIVersion.
- AC-2 (product AC-4): A scenario trigger returns an invalid envelope or invalid TSX; the previous mock stays visible; an error state with Retry appears; no broken version is recorded as valid; retry recovers.
- AC-3: During generation the previous mock remains visible with an updating indicator (observable in the DOM).
- AC-4 (adapted from product AC-5): After a second message, the mock shows the new state; the spec change is reviewable side-by-side; no diff UI exists for the mock.
- AC-5: The mock renders the application with the feature applied, using real Kit components and theme tokens in the sandbox.

## Mock contract

Phase 1 is mocked; endpoint shapes and handler definitions are finalized in plan.md. This section pins behaviors.

- Generation: an accepted chat turn produces its generation envelope from fixtures (replay mode, keyed by request hash plus prompt version).
- Scenario triggers: invalid envelope and invalid TSX, for AC-2.
- The mock's own API calls are served by the shared handler definitions through the transport adapter (product spec Section 9.2).
- Sandbox assertions use Playwright `frameLocator` (product spec Section 13).
- The sandbox runtime and its shell are provided by 004-sandbox-runtime; this spec depends on it.

## Swap seam

One module boundary, for example `apps/web/src/features/generation/data/`, exporting typed functions the UI imports (requestGeneration, getVersions, getLatestMock). Phase 1 binds them to MSW handlers and fixtures; Phase 2 replaces the internals with the real backend behind the same functions, with no changes to the UI or its imports.

## Open questions

- Placement and styling of the "updating the mock" indicator (chat status vs over the preview).
- Interactivity scope of the mock: which interactions are live (backed by handlers) vs inert chrome.
- Phase 2: skip regeneration when a change cannot affect the mock, or always regenerate (the Phase 1 rule is every accepted turn).
