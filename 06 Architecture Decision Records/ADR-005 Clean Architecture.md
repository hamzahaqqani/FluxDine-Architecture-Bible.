# ADR-005 — Clean Architecture

## Status
Accepted

## Decision

FluxDine applications and services shall follow Clean Architecture principles.

Dependencies shall flow toward business logic rather than infrastructure.

## Consequences

- Controllers remain thin.
- Business logic resides in services/use cases.
- Persistence remains behind repositories.
- Infrastructure remains replaceable.


