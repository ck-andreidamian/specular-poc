# specular

A workspace where product people design features by talking to an AI. Features
render as live, dev-ready mocked UI built from a real project's code and design
system. Handoff pushes the feature plus its spec context into a dedicated sandbox
repo, where a developer implements the backend and replaces the mocked interface.

## Where things live

| Path | What it is |
|---|---|
| `docs/product/specular.md` | Product spec: what and why, scope, acceptance criteria, decision log D-1..D-14 |
| `docs/product/roadmap.md` | MVP definition, phases, feature catalog + statuses (single source of truth for progress) |
| `docs/product/tech-stack.md` | Settled stack + open decisions |
| `docs/specs/` | One folder per feature (`NNN-slug/spec.md`; `plan.md` + `tasks.md` added when building); `index.md` holds the conventions |
| `docs/adr/` | Architecture decision records for decisions that outlive a single feature |
| `apps/web/` | specular frontend (Phase 0+) |
| `apps/server/` | Thin backend (Phase 2) |
| `packages/contracts/` | API types/schema shared by web, server, mocks, and e2e |
| `packages/mocks/` | MSW handler definitions + fixtures (the replay data) |
| `e2e/` | Playwright acceptance harness; tests map to the product spec's AC list |
| `.omo/` | Session-local agent plans; gitignored, not part of the product |

## How we work

Non-negotiables:

- **Mock-first (D-14).** Phase 1 runs with no server and no external calls: the
  frontend uses MSW handlers behind one client seam, and everything, including the
  LLM/chat, is served from fixtures. The real backend is swapped in behind the
  same contract in Phase 2.
- **Spec-first.** No feature work without its `docs/specs/NNN-slug/spec.md`.
  Specs are carved just-in-time, never upfront.
- **Contracts are the source of truth.** Shared types and schema live in
  `packages/contracts/`. Mocks and (later) the backend both implement them. Mocks
  are written as the real contract, never throwaway stubs.
- **Fixtures are replay data.** The same fixtures serve dev iteration, acceptance
  tests, and the demo. Keep them deterministic.
- **Status discipline.** Update `docs/product/roadmap.md` whenever a feature
  changes state. Ladder: planned → draft → specced → mocked → integrated → done;
  or cut.

## Feature workflow

1. Pick the next feature from the roadmap catalog.
2. Carve its spec just-in-time.
3. Review and approve it.
4. Add `plan.md` + `tasks.md` when building.
5. Build.
6. Flip the status on the roadmap.

Detail lives in the roadmap's iteration loop.

## Commands

None yet; Phase 0 adds dev/test/build commands here.

## Definition of done

A feature is done when its acceptance criteria pass offline with both replay
axes. Determinism is scoped to artifact IDs plus a DOM snapshot (see product spec
§12-13).

## Agent notes

Claude Code reads CLAUDE.md, which imports this file. Keep this file small and
push detail into docs/.
