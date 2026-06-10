# ADR 0001: Monorepo and Runtime Choices

## Status

Accepted

## Context

The first milestone needs a working proxy path, resilience primitives, and a coordinator skeleton without fragmenting the codebase too early.

## Decision

- Use an npm workspace monorepo for the TypeScript runtime packages.
- Keep observability helpers in Python where ecosystem tooling is strongest.
- Start with an HTTP-first proxy runtime before adding gRPC or lower-level network protocols.
- Isolate resilience and coordination into separate packages so they can evolve independently.

## Consequences

- Faster bootstrap for local development and CI.
- Clear package boundaries for future protocol and control-plane work.
- A straightforward migration path toward additional runtimes and deployment integrations.

