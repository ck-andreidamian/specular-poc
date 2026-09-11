# Tech Stack

Ratified stack decisions and open decisions. Not a wishlist.

## Settled

| Decision | Choice | Source |
|---|---|---|
| specular frontend | React + Vite + TypeScript + shadcn/ui | D-5 |
| Target-backend mocking | Shared MSW-style handler definitions + one transport adapter (delivery per sandbox spike) | D-6, D-14 |
| LLM access | Configurable OpenAI-compatible provider, server-side keys, streaming | D-7 |
| specular backend | Thin real backend: LLM orchestration, envelope validation, GitHub ops, persistence; integrated in Phase 2 | D-1 |
| Acceptance harness | Playwright driving the browser; sandbox assertions via frameLocator; offline replay on both axes | spec §13, D-8 |
| Development workflow | Mock-first: everything mocked in Phase 1, including the LLM/chat; fixtures double as replay data | D-14 |
| Workspace tooling | Bun workspaces for the apps/ + packages/ layout | ADR 0001 |

## Open decisions

| Item | Notes | Decide when |
|---|---|---|
| Backend runtime | e.g. Node or Bun | Phase 2 planning |
| LLM provider + default model | Product spec open question 1 | Phase 2 planning |
| Persistence store | SQLite + explicit seed fixture recommended (product spec open question 5) | Phase 2 planning |

New decisions are recorded as amendments to the decision log in the [product spec](./specular.md) or as [ADRs](../adr/README.md).
