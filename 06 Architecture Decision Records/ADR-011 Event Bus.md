# ADR-011 — Shared Event Bus

## Status
Accepted

## Decision

Cross-service domain events shall be distributed through a shared Event Bus abstraction.

## Consequences

Services publish events without requiring direct synchronous coupling to every consumer.


