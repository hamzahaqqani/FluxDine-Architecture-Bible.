# ADR Register

# FluxDine Architecture Decision Record Register

---

## Purpose

This register provides the authoritative index of all Architecture Decision Records within the FluxDine Architecture Bible.

ADR documents capture significant architectural decisions, their rationale, consequences, and affected areas.

---

## ADR Status Definitions

| Status | Meaning |
|---|---|
| Proposed | Decision is under consideration |
| Accepted | Decision is approved and authoritative |
| Superseded | Replaced by a later ADR |
| Deprecated | No longer recommended |
| Rejected | Decision was considered and rejected |

---

## Register

| ADR | Title | Status |
|---|---|---|
| ADR-000 | Architecture Decision Record Template | Accepted |
| ADR-001 | FluxDine Architecture Bible | Accepted |
| ADR-002 | Multi-Tenant SaaS Architecture | Accepted |
| ADR-003 | Database per Service | Accepted |
| ADR-004 | Service Ownership Boundaries | Accepted |
| ADR-005 | Clean Architecture | Accepted |
| ADR-006 | Feature-First Code Organization | Accepted |
| ADR-007 | API-First Architecture | Accepted |
| ADR-008 | REST API Standard | Accepted |
| ADR-009 | API Versioning | Accepted |
| ADR-010 | Event-Driven Architecture | Accepted |
| ADR-011 | Shared Event Bus | Accepted |
| ADR-012 | Centralized Identity Service | Accepted |
| ADR-013 | Centralized Tenant Service | Accepted |
| ADR-014 | Restaurant Service Ownership | Accepted |
| ADR-015 | Commerce Service | Accepted |
| ADR-016 | Billing Service | Accepted |
| ADR-017 | Centralized Payment Service | Accepted |
| ADR-018 | Payment Gateway Abstraction | Accepted |
| ADR-019 | Centralized Notification Service | Accepted |
| ADR-020 | Centralized Email Service | Accepted |
| ADR-021 | Centralized Analytics Service | Accepted |
| ADR-022 | Domain Service | Accepted |
| ADR-023 | Theme Service | Accepted |
| ADR-024 | Feature Flag Service | Accepted |
| ADR-025 | Audit Service | Accepted |
| ADR-026 | Logging Service | Accepted |
| ADR-027 | Monitoring Service | Accepted |
| ADR-028 | Centralized File Storage Service | Accepted |
| ADR-029 | Centralized Search Service | Accepted |
| ADR-030 | Multi-Application Platform | Accepted |
| ADR-031 | Self-Service SaaS Architecture | Accepted |
| ADR-032 | Registration and Email Verification | Accepted |
| ADR-033 | Plan Selection | Accepted |
| ADR-034 | Trial Management | Accepted |
| ADR-035 | Onboarding Wizard | Accepted |
| ADR-036 | Restaurant Configuration | Accepted |
| ADR-037 | Restaurant Payment Gateway Configuration | Accepted |
| ADR-038 | Domain Configuration | Accepted |
| ADR-039 | Theme Configuration | Accepted |
| ADR-040 | Restaurant Launch Workflow | Accepted |
| ADR-041 | Tenant Isolation | Accepted |
| ADR-042 | Authorization Model | Accepted |
| ADR-043 | Repository Pattern | Accepted |
| ADR-044 | Shared Design System | Accepted |
| ADR-045 | Centralized File Asset Management | Accepted |
| ADR-046 | Centralized Observability | Accepted |
| ADR-047 | Audit Immutability | Accepted |
| ADR-048 | API Idempotency | Accepted |
| ADR-049 | Background Processing | Accepted |
| ADR-050 | Scheduled Jobs | Accepted |
| ADR-051 | AI-Assisted Development | Accepted |
| ADR-052 | Architecture as AI Context | Accepted |
| ADR-053 | Infrastructure Independence | Accepted |
| ADR-054 | Independent Service Evolution | Accepted |

---

# ADR Statistics

| Category | Count |
|---|---:|
| Governance / Architecture | 11 |
| Shared Services | 18 |
| Security / Data | 5 |
| Product Workflows | 10 |
| Engineering / AI | 7 |
| Infrastructure / Operations | 5 |
| **Total ADRs** | **55** |

---

# Maintenance Rules

New ADRs shall be created when:

- A significant architectural decision is made.
- An existing architectural boundary changes.
- A major technology decision is introduced.
- A significant security decision is made.
- A previous ADR is superseded.

Existing ADRs shall not be silently rewritten to hide historical decisions.

When an accepted decision changes, a new ADR should normally supersede the previous decision.

---

# Authority

The ADR Register and individual ADR documents together form the authoritative architectural decision history of FluxDine.

The Architecture Bible remains the authoritative implementation reference, while ADRs explain the decisions behind that architecture.