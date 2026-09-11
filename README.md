# specular

A workspace where product people design features by talking to an AI. Features
render as live, dev-ready mocked UI built from a real project's code and design
system. Handoff pushes the feature plus its spec context into a dedicated sandbox
repo, where a developer implements the backend and replaces the mocked interface.

## Start here

- [Product spec](docs/product/specular.md)
- [Roadmap](docs/product/roadmap.md)
- [Tech stack](docs/product/tech-stack.md)

## Repository layout

- `docs/`: product spec, roadmap, tech stack, feature specs, ADRs
- `apps/`: `web` (specular frontend) and `server` (thin backend, Phase 2)
- `packages/`: `contracts` (shared types) and `mocks` (MSW handlers + fixtures)
- `e2e/`: Playwright acceptance harness

Status: POC, developed mock-first: Phase 1 runs entirely on mocks (no backend, no real LLM). See the roadmap.
