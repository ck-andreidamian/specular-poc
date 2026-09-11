# 003: Iteration sync

| Field | Value |
|---|---|
| Status | specced |
| Phase | 1 |

## Problem

Every accepted turn changes both the spec and the mock. For the workspace to stay trustworthy, those changes must land as one immutable, paired version: the spec and the mock can never drift apart, and history must survive even though the POC does not browse it. This feature owns that mechanism.

## Goals

- Every accepted turn creates one paired SpecVersion + GeneratedUIVersion, both immutable (product spec Section 5).
- The spec and the mock are always in sync: the UI view reflects the latest accepted version (002).
- Full version history is retained in the data; failed turns add nothing (002).
- No history browser and no restore in the POC: the only version-facing UI is the latest change review (spec side-by-side, 001).
- If versions are ever viewed, the spec and its mock travel together (paired browsing is a property of the model).

## Non-goals

- A history/browser UI in the POC.
- Restore/rollback.
- Git-backed storage (Phase 2 candidate; also noted in 001).

## User stories

- US-1 (P1): I never see the spec and the mock disagree; each turn updates them together.
- US-2 (P1): A failed turn leaves no trace in the version history.
- US-3 (P2): Version history exists and is retained, ready for a future browser.

## Acceptance criteria

- AC-1: After N accepted turns there are exactly N paired SpecVersion + GeneratedUIVersion records; no unpaired version exists.
- AC-2: After any accepted turn, the UI view matches the latest GeneratedUIVersion.
- AC-3: A failed turn (the AC-2/AC-4 failure triggers from 001/002) adds no version records.
- AC-4: History is retained in storage (count and ordering verified via the mocked API or store in replay).
- AC-5: No history-browser or restore UI is reachable in the POC.

## Mock contract

- The mocked store holds version records (paired spec + UI versions with timestamps) and serves reads for the latest pair and history counts.
- Endpoint shapes and handler definitions are finalized in plan.md.

## Swap seam

One module boundary, for example `apps/web/src/features/versions/data/`, exporting typed functions the app imports (getLatestPair, listVersions, compareVersions). Phase 1 binds them to MSW handlers and fixtures; Phase 2 replaces the internals with real persistence behind the same functions, with no changes to the UI or its imports.

## Open questions

- Git-backed storage: Phase 2 candidate (also noted in 001).
- Whether a history browser returns after the POC (it was explicitly deferred).
