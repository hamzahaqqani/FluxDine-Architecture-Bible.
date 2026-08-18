# ADR-028 — Centralized File Storage Service

## Status
Accepted

## Decision

Binary files and object storage shall be centralized through the File Storage Service.

Business services shall store file references and metadata rather than binary content in their own databases.

## Consequences

Storage providers can be changed behind an abstraction.


