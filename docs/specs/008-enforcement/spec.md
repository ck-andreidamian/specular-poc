# 008: Enforcement

| Field | Value |
|---|---|
| Status | specced |
| Phase | 1 |

## Problem

Constraints only matter if they are checked. Enforcement computes, from static analysis of the generated code, whether it respects the project's harness: the component allowlist, theme tokens, and the i18n rule. Flags come from the analyzer, never from the model's own claims.

## Goals

- Static analysis of every accepted generation: forbidden imports, raw MUI usage, hex/px literals, hardcoded user-facing strings.
- Violations render and flag: the mock still renders; the affected version is flagged and is not marked valid.
- Flags surface as a violation summary on the affected version (count plus files), expandable to per-file, line-referenced details.
- The flags come from the analyzer, not from LLM self-report, and are triggered deterministically in replay.

## Non-goals

- Auto-fixing violations in the POC.
- Design-system authoring or editing.
- Runtime or behavioral testing of generated code.

## User stories

- US-1 (P1): When generated code violates the harness, I see exactly what and where, on the version it belongs to.
- US-2 (P1): A violating version is not marked valid, even though the mock renders.
- US-3 (P1): I can trust the flags because they are computed, not self-reported.

## Acceptance criteria

- AC-1 (product AC-11): static analysis flags a forbidden import, raw MUI usage, a hex or px literal, and a hardcoded user-facing string; the flags come from the analyzer, not LLM self-report, and are triggered deterministically in replay.
- AC-2: a flagged version renders the mock and is not marked valid.
- AC-3: the violation summary shows a count plus files, expandable to line-referenced details.
- AC-4: violations are attached to the version they belong to (its paired SpecVersion and GeneratedUIVersion).

## Mock contract

- Fixtures: a violation scenario (the deliberate demo violation) with analyzer output, deterministic in replay.
- The analyzer runs wherever generation runs: in the mocked pipeline in Phase 1, in the backend in Phase 2. Endpoint shapes and handler definitions are finalized in plan.md.

## Swap seam

One module boundary, for example `apps/web/src/features/enforcement/data/`, exporting typed functions the UI imports (getFlags, getFlagDetails). Phase 1 binds them to MSW handlers and fixtures; Phase 2 replaces the internals with the real analyzer behind the same functions, with no changes to the UI or its imports.

## Open questions

- Whether severity levels are needed in the POC at all (currently one class of violation).
