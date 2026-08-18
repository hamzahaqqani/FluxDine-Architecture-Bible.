# ADR-010 — Event-Driven Architecture

## Status
Accepted

## Decision

FluxDine shall use asynchronous domain events for appropriate cross-service communication.

## Rationale

Events reduce direct coupling and allow independent service evolution.

## Consequences

- Events require defined contracts.
- Consumers must be idempotent.
- Event failures require retry/recovery mechanisms.
- Event ownership must remain explicit.


