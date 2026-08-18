# ADR-003 — Database per Service

## Status
Accepted

## Context

Shared databases create tight coupling between services and make independent evolution difficult.

## Decision

Each Shared Platform Service shall own its own database.

## Rationale

Database ownership provides clear service boundaries and prevents cross-service persistence coupling.

## Consequences

- Services cannot directly access another service's database.
- Cross-service data access uses APIs or events.
- Distributed workflows require explicit coordination.