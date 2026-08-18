# ADR-018 — Payment Gateway Abstraction

## Status
Accepted

## Decision

Payment providers shall be accessed through a shared Payment Gateway Abstraction.

The Payment Service shall remain gateway-agnostic.

## Rationale

New providers can be introduced without rewriting payment business logic.

## Consequences

Gateway-specific implementation remains behind the abstraction layer.


