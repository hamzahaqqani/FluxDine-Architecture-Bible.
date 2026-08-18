# ADR-041 — Tenant Isolation

## Status
Accepted

## Decision

Tenant isolation shall be enforced at API, service, and data-access boundaries.

## Consequences

Every tenant-aware operation must establish and validate tenant context.
