# ADR-048 — API Idempotency

## Status
Accepted

## Decision

Retry-sensitive operations shall support idempotency.

Critical examples include:

- Payment
- Refund
- Order creation
- Subscription activation

## Consequences

Distributed retries can occur without unintentionally duplicating business operations.


