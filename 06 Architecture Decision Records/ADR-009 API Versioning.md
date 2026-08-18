# ADR-009 — API Versioning

## Status
Accepted

## Decision

Public APIs shall use explicit versioning.

Example:

`/api/v1/orders`

Breaking changes require a new API version.

## Consequences

Existing clients can continue using supported API versions while new versions evolve independently.



