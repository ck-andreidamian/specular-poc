# Roadmap

The single source of truth for product progress and how we iterate.

## MVP

Prove the full loop end to end: conversation to structured spec, spec to live mocked UI, UI to handoff. Run it on two paths in one demo: importing the mature Colorkrew/quiz repo and a from-scratch scaffold. Build it mock-first per D-14: in Phase 1 everything is served from mocks, with no server process and no real LLM or GitHub calls; the thin backend swaps in later behind the same contract.

## Phases

| Phase | Goal | Exit criteria |
|---|---|---|
| Phase 0 — Foundation | Sandbox spike (hard gate) + repo scaffold + MSW harness skeleton | Spike proven: a hand-written page using real KitButton + KitCard, the theme, and one intercepted API call, running fully offline and assertable from Playwright inside the frame via frameLocator; repo structure in place; AGENTS.md, roadmap, and specs index live. |
| Phase 1 — UI MVP (mock-first) | The full loop built against MSW handlers and fixtures: no server process, no real LLM calls, no real GitHub calls. | An evaluator can complete the full loop unaided on both paths against mocks: describe in chat, browse the spec as a wiki, see the mock refresh, review changes, and hand off; generated UI renders in the sandbox using real Kit components; error and retry paths are observable. |
| Phase 2 — Backend integration | Thin backend (LLM proxy + streaming, envelope validation, GitHub ops, persistence) swapped in behind the contract. | Features flip to `integrated`; both replay axes wired; acceptance criteria green offline. |
| Phase 3 — Demo hardening | Five-minute demo run on replay, both paths. | Determinism (artifact IDs + DOM snapshot) stable across runs. |

## Feature catalog

| Feature | Purpose | Spec | Phase | Status |
|---|---|---|---|---|
| 001-conversation | Chat produces and refines a persisted, versioned structured spec; browsable as a wiki with side-by-side (before vs after) change review | `../specs/001-conversation/spec.md` | 1 | specced |
| 002-generation | Every accepted turn refreshes the mocked UI: a validated Generation Contract envelope rendered live as TSX in the sandbox | `../specs/002-generation/spec.md` | 1 | specced |
| 003-iteration-sync | Spec and mock stay paired and versioned across turns (paired, immutable versions); history retained but not browsable in the POC | `../specs/003-iteration-sync/spec.md` | 1 | specced |
| 004-sandbox-runtime | Trimmed real target runtime (allowed Kit sources, theme, API client, store, i18n, preview shell) hosting the generated page; the Phase 0 spike gate | `../specs/004-sandbox-runtime/spec.md` | 0 | specced |
| 005-import | Import the mature repo (Colorkrew/quiz) with mocked analysis: mocked version of the repo + detected tokens and components; produces the generation harness | `../specs/005-import/spec.md` | 1 | specced |
| 006-scaffold | From-scratch project scaffold: name-and-go creation with a small kit (card, button, text field), tokens, and a mocked backend | `../specs/006-scaffold/spec.md` | 1 | specced |
| 007-handoff | Push generated feature + spec context to the dedicated sandbox repo as a branch/PR from the workspace top bar; a handed-off marker remains after success | `../specs/007-handoff/spec.md` | 1 | specced |
| 008-enforcement | Static analysis flags allowlist violations, raw MUI usage, hex/px literals, hardcoded strings; flagged versions still render the mock but are not marked valid | `../specs/008-enforcement/spec.md` | 1 | specced |
| 009-project-home | Projects home: the project list plus the two entry actions (import repo, create from scratch); opens a project's workspace | `../specs/009-project-home/spec.md` | 1 | specced |

Turn a Spec cell into a link when that spec file exists.

## Feature lifecycle

One status ladder describes where each feature stands:

- **planned** — listed here, no spec yet.
- **draft** — `spec.md` being written.
- **specced** — spec reviewed and approved.
- **mocked** — UI works against MSW; acceptance criteria observable.
- **integrated** — real backend behind the same contract.
- **done** — acceptance criteria pass in offline replay.
- **cut** — deferred or removed; row kept for history.

## Iteration loop

1. Pick the next feature from the catalog.
2. Carve `docs/specs/NNN-slug/spec.md` from the product spec (just-in-time — never create empty spec folders).
3. Review and approve the spec.
4. When building starts, add `plan.md` and `tasks.md` to that feature folder.
5. Build (agents).
6. Flip status to `mocked` when the UI works against mocks.
7. In Phase 2, flip to `integrated`.
8. Flip to `done` when acceptance criteria pass in replay.

Update the catalog table on every status change.

## Rules

- This file is the ONLY place feature statuses are tracked. `docs/specs/index.md` must not duplicate statuses.
- Phases describe build order; the ladder describes state.
- Keep rows for cut features.

Product spec: [product spec](./specular.md). Specs index: [specs index](../specs/index.md). Tech stack: [tech stack](./tech-stack.md).
