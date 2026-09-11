# Architecture Decision Records

Durable records of architectural decisions that outlive a single feature.

## When to write one

Write an ADR when a decision changes how we build (architecture, contracts,
tooling) and will outlive the feature that triggered it. If it only affects one
feature, keep it in that feature's spec instead.

## Naming

`NNNN-title-with-dashes.md` (4-digit sequential number, lowercase hyphenated
title; e.g. `0001-use-msw-for-mocks.md`). Numbers are never reused.

## Statuses

- proposed
- accepted
- rejected
- superseded

Superseded ADRs are kept in place with a link to the replacement.

## Relation to the product spec

Settled product decisions live as the D-log in `../product/specular.md`
(D-1..D-14). ADRs are for new post-restructure technical decisions not already
covered there.
