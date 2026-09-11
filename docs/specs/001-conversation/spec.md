# 001: Conversation to structured spec

| Field | Value |
|---|---|
| Status | specced |
| Phase | 1 |

## Problem

The conversation is where a feature is born in specular. A product person talks through what they want, and the agent turns that talk into a structured specification.

The structured spec is what carries the feature forward: into the mock, and later into handoff. That means the agent has to turn product talk into a spec that is always current, always referenceable, and never hand-maintained. The human looks at it when needed and changes it only by talking to the agent.

## Goals

- A chat panel on the left of the project screen, always available; the single conversation for the project.
- The agent drafts and refines the structured spec (problem, goals, user stories, acceptance criteria) on every accepted turn.
- When a description is vague, the agent asks targeted clarifying questions before updating the spec.
- The spec is browsable as a wiki: sections as pages, in the Specification view under the top UI | Specification switcher.
- Spec changes are reviewable like a GitHub PR, adapted: side-by-side "before vs after" per section while browsing.
- The spec is persisted, versioned, and re-openable; full change history is retained.
- The spec is AI-authored only: the human changes it by telling the agent, referencing sections as needed.

## Non-goals

- Direct human editing of spec text.
- Multiple conversations per project (one thread per project in this phase).
- Approval gates or sign-off workflows.
- Git-backed spec storage (candidate for Phase 2; see Open questions).

## User stories

- US-1 (P1): I send one message describing my feature and get a structured spec (problem, goals, at least one user story, acceptance criteria) that is persisted.
- US-2 (P1): I continue the conversation and the spec is updated in the same turn; the spec and the mock stay in sync.
- US-3 (P1): When my description is vague, the agent asks targeted questions before updating the spec.
- US-4 (P1): I browse the spec as a wiki whenever I need to check or reference it.
- US-5 (P1): After a change, I review what the agent changed, side by side, per section.
- US-6 (P1): To change the spec, I reference the relevant part and tell the agent; I never edit text myself.
- US-7 (P2): I reach the Specification view at any time from the top switcher.

## Acceptance criteria

- AC-1 (product AC-1): Creating a project and sending one message yields a persisted spec containing problem, goals, at least one user story, and acceptance criteria.
- AC-2 (product AC-2): A scenario trigger makes the LLM return an error; a visible error appears with a Retry control, the previous spec is unchanged, and retry recovers.
- AC-3: The spec is browsable as a wiki: its sections are individually reachable while the conversation continues.
- AC-4 (product AC-5 basis): After a second message changes the spec, the changed sections can be compared side-by-side (before vs after) and the change is retained in history.
- AC-5: With a scripted vague description, the agent responds with a clarifying question and does not rewrite the spec until answered (deterministic in replay).
- AC-6: Every accepted turn records a new SpecVersion paired with a GeneratedUIVersion (product spec Section 5).
- AC-7: Re-opening the project restores the conversation and the spec intact.

## Mock contract

Phase 1 is mocked; endpoint shapes and handler definitions are finalized in plan.md. This section pins behaviors.

- Chat turn: user message in, streamed assistant reply out (scripted streaming fixtures in Phase 1, both replay axes).
- Scripted scenarios: a normal reply, an LLM error (AC-2 trigger), and a vague-description interview (AC-5 trigger).
- Spec state: current spec document plus version history, with a compare capability for before/after per section.
- Fixtures: a canonical demo conversation for the quiz "result sharing" feature ("Onboarding Basics", score 8/10), and one for the scaffold path ("Acme" workspace, invite member).

## Swap seam

One module boundary, for example `apps/web/src/features/conversation/data/`, exporting typed functions the UI imports (sendMessage, getConversation, getSpec, listSpecVersions, compareSpecSections). Phase 1 binds them to MSW handlers and fixtures; Phase 2 replaces the internals with the real backend behind the same functions, with no changes to the UI or its imports.

## Open questions

- Spec history storage: app-store records in Phase 1; git-backed (wiki-style) storage candidate for Phase 2?
- Wiki page granularity: one page per spec section, or nested pages?
- Side-by-side review presentation: dedicated review mode vs inline comparison while browsing.
- Streaming granularity in Phase 1 fixtures: token-level vs chunked.
