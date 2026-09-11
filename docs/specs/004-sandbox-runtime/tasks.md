# 004: Sandbox runtime — tasks

Gate: all tasks complete and `bun run verify` green from a clean checkout; roadmap `004-sandbox-runtime` flipped to `mocked`.

Legend: `[P]` = parallel-safe with other `[P]` tasks in the same phase (no shared files). Every implementation task starts by adding its failing test (TDD).

## Phase A — Setup

- [ ] **T001 [P] Root Bun workspace scaffold** — create `package.json` (workspaces + scripts from plan §6), `tsconfig.base.json` (strict, `moduleResolution: bundler`, `jsx: react-jsx`, `noUncheckedIndexedAccess`), `biome.json` (ignore `apps/web/src/sandbox/runtime/vendor/**`), `.gitignore` (node_modules, dist, playwright-report, test-results, *.local; keep `.omo/`); commit `plan.md` + `tasks.md` as the first commit. **Done:** `bun install` succeeds; `bun run lint` exits 0.
- [ ] **T002 [P] `packages/contracts`: quiz boot types** — package `@specular/contracts` (source-only exports `./src/index.ts`), `src/app-api.ts` with `WorkspaceVisibilityEnum`, `Workspace`, `WorkspaceOverview` copied verbatim from quiz `internalApiV2.ts:1654-1678`, `src/index.ts` barrel, `typecheck` script. **Done:** `bun --cwd packages/contracts run typecheck` passes.
- [ ] **T003 [P] `packages/mocks`: package scaffold** — `@specular/mocks` with deps `msw@2.15.0`, `@mswjs/interceptors@0.42.5`, `@specular/contracts workspace:*`; dev `vitest@5.0.0`, `typescript`; `tsconfig.json`, `vitest.config.ts` (node env), empty `src/index.ts`. **Done:** typecheck passes; `vitest run` exits 0 with no tests.
- [ ] **T004 [P] `apps/web`: Vite app + host page** — React 19 + Vite 7 + TS; Tailwind v4 + shadcn baseline (`index.css`, `lib/utils.ts` `cn`, `components.json`, `@/` alias); `vite.config.ts` with multi-entry `index.html` + stub `sandbox.html`; host `App.tsx` with `<iframe data-testid="sandbox-frame" src="sandbox.html" title="Target app preview">`; scripts `dev`, `build` (`tsc --noEmit && vite build`), `preview`, `typecheck`, `sandbox:sync`, `sandbox:verify`. **Done:** `bun --cwd apps/web run build` emits `dist/index.html` **and** `dist/sandbox.html`.
- [ ] **T005 e2e: Playwright scaffold** — `@playwright/test@1.63.0`; `playwright.config.ts` with chromium + `webServer` (build + `preview --port 4173 --strictPort`); `tests/smoke.spec.ts` asserts host page loads and the `sandbox-frame` iframe exists. **Done:** `bun --cwd e2e run test` green. Depends T004.

## Phase B — Foundational (MSW harness + trim)

- [ ] **T006 `packages/mocks`: handlers + fixtures (TDD)** — first `src/__tests__/handlers.test.ts` (red): `getResponse(appHandlers, Request('http://localhost/api/v1/app/workspaces/overview'))` → 200 + fixture deep-equal; then `src/fixtures/app.ts` (`satisfies WorkspaceOverview[]`, fixed ids/timestamps, `Acme`/2 members + `Design Team`/5 members) and `src/handlers/app.ts`; export from `index.ts`. **Done:** test green. Depends T002, T003.
- [ ] **T007 `packages/mocks`: fetch-interceptor transport (TDD)** — first `src/__tests__/fetchInterceptor.test.ts` (red): install, `await fetch(...)` resolves from handlers, dispose restores; then `src/transport/fetchInterceptor.ts` per plan §8. **Done:** test green in node; `@mswjs/interceptors` imports resolve to browser build under Vite (checked at T011 build). Depends T006.
- [ ] **T008 [P] `packages/mocks`: same-origin SW delivery skeleton** — `src/transport/browserWorker.ts` (`setupWorker(...appHandlers)`, `startBrowserWorker`); run `bunx msw init apps/web/public --save`. **Done:** typecheck passes; module exported; not wired. Depends T006.
- [ ] **T009 `apps/web`: verbatim vendor + provenance tooling** — `scripts/sandbox-trim.ts` (`sandbox:sync` copies from `$QUIZ_CHECKOUT` defaulting to the local quiz path; `sandbox:verify` hash-checks and fails on drift; regenerates `PROVENANCE.json`); copy the verbatim inventory from plan §7 into `runtime/vendor/` preserving relative structure; write `PROVENANCE.json` (quiz commit `5a2d62e`, sha256 per verbatim file). **Done:** `sandbox:verify` green; `typecheck` green. Depends T004.
- [ ] **T010 [P] `apps/web`: adapted runtime modules** — `services/api/internalApiV2.ts` (one endpoint, types from `@specular/contracts`, adapted header), `store/index.ts` (RTKQ only), `i18n.ts` (no zod, `?lang=` override, merge spike resources), spike locale files `assets/lang/specular-spike/{en,ja}.json`; add each to `PROVENANCE.json` as adapted with notes. **Done:** typecheck green; files carry the adapted-from header. Depends T009, T002.

## Phase C — Spike (the gate)

- [ ] **T011 `apps/web`: sandbox shell + entry + transport bootstrap** — real `sandbox.html`; `src/sandbox/main.tsx` (install transport → i18n lang → render); `bootstrap.ts` calling `installFetchInterceptorTransport(appHandlers)`; `SandboxApp.tsx` (Redux Provider + `ThemeProvider(createMuiTheme())` + `CssBaseline` + `RouterProvider`); `feature/FeatureRoute.tsx` rendering a placeholder. **Done:** frame loads with no console errors; `build` still emits both entries; interceptor logs no unmatched boot request. Depends T007, T009, T010.
- [ ] **T012 `apps/web`: spike page TDD + route mount** — write the e2e gate spec first (T013 content) and watch it fail; implement `spikes/WorkspaceOverviewSpike.tsx` (cards from `useGetWorkspacesOverviewQuery`, `KitButton` refresh, `t()` strings, token-only `sx`, testids); mount in `FeatureRoute`. **Done:** page renders fixture list; button refetches. Depends T006, T009, T011.
- [ ] **T013 e2e: 004 gate spec** — `tests/004-sandbox-spike.spec.ts` with all six assertions from plan §10 (fixture-in-frame, second intercepted request after click, theme computed styles, frameLocator-only interaction, `navigator.serviceWorker.controller === null` in frame, zero external requests). **Done:** `bun --cwd e2e run test` green; this is AC-1/AC-2/AC-4 evidence. Depends T005, T012.

## Phase D — Verification

- [ ] **T014 quality gates across the workspace (no new features)** — run `bun run typecheck`, `bun run lint`, `bun run test:unit`; fix all fallout; confirm root scripts match plan §6. **Done:** all green; no biome suppressions added outside `vendor`. Depends T006–T013.
- [ ] **T015 [P] offline + determinism + provenance audit** — grep fixtures/spike for `Date.now`, `Math.random`, `crypto.randomUUID` (must be absent); confirm all fonts/lang assets are local; run `sandbox:verify`; run the e2e suite once with `CI=true`; record evidence (test output + e2e report). **Done:** audit passes; evidence saved for T017. Depends T013.
- [ ] **T016 final gate review from a clean checkout** — `rm -rf node_modules && bun install && bun run verify` (plus one-time `bunx playwright install chromium`); manual `bun run dev` check that the frame shows the app; confirm each AC-1/AC-2/AC-3/AC-4 has an artifact. **Done:** `bun run verify` exits 0. Depends T014, T015.
- [ ] **T017 docs + status flip** — add `004-sandbox-runtime` to `docs/specs/index.md`; update `AGENTS.md` Commands (dev/build/test/verify); flip roadmap row to `mocked` with evidence link/notes; record any deviations discovered. **Done:** docs consistent; roadmap updated last. Depends T016.

## Parallel waves

| Wave | Tasks |
|---|---|
| 1 | T001 |
| 2 | T002, T003, T004 |
| 3 | T005, T006, T009 |
| 4 | T007, T008, T010 |
| 5 | T011 |
| 6 | T012 |
| 7 | T013 |
| 8 | T014, T015 |
| 9 | T016 |
| 10 | T017 |
