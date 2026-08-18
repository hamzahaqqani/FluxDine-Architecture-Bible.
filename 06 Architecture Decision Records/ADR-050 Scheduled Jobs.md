# ADR-050 — Scheduled Jobs

## Status
Accepted

## Decision

Time-based platform operations shall execute through reliable server-side scheduled jobs.

Client-side polling shall not be the authoritative mechanism for business lifecycle transitions.

## Consequences

Business state transitions continue even when no user has an application open.


