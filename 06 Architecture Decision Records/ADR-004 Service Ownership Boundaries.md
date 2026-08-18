# ADR-004 — Service Ownership Boundaries

## Status
Accepted

## Context

FluxDine contains multiple business capabilities that must remain independently maintainable.

## Decision

Every major business capability shall have one authoritative owner.

Examples:

Identity → Identity Service  
Tenant → Tenant Service  
Restaurant → Restaurant Service  
Commerce → Commerce Service  
Billing → Billing Service  
Payment → Payment Service  
Notification → Notification Service  
Email → Email Service  
Analytics → Analytics Service

## Consequences

Business logic shall not be duplicated across services.


