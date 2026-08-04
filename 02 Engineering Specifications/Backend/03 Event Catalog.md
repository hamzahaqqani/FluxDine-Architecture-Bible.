# 04 Engineering Specifications

# Backend

# 03 — Event Catalog

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-BE-003 |
| **Document Name** | Event Catalog |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | Service Specification<br>Background Job Specification<br>Queue Specification<br>Webhook Specification |
| **Referenced By** | Backend Services<br>Queue Workers<br>Webhook Dispatcher<br>Integration Services |

---

# Dependencies

This specification depends upon:

- Service Specification
- Repository Specification
- Queue Specification
- Background Job Specification
- Webhook Specification

Events provide asynchronous communication across the FluxDine platform.

---

# Referenced By

This specification is referenced by:

- Service Layer
- Queue Workers
- Background Jobs
- Notification Service
- Webhook Dispatcher
- Integration APIs
- Analytics Services

---

# Document Status

| Item | Value |
|------|-------|
| Status | Approved and Locked |
| Approval | Approved |
| Implementation | Architecture Complete |
| Last Updated | TBD |

---

# Purpose

This document defines the standard event catalog used throughout the FluxDine platform.

Events represent significant business occurrences that allow loosely coupled services to react asynchronously while maintaining consistency, scalability, and extensibility.

This specification serves as the authoritative event contract for the backend architecture.

---

# Scope

This specification defines:

- Domain Events
- Integration Events
- Event naming
- Event payload standards
- Event lifecycle
- Event publishing
- Event consumption
- Event versioning
- Engineering rules

---

# Out of Scope

This specification does not define:

- Queue implementation
- Background job scheduling
- Webhook payloads
- Message broker configuration

These topics are documented separately.

---

# Event Philosophy

Events represent facts that have already occurred.

Events shall:

- Be immutable.
- Represent completed business actions.
- Be asynchronous.
- Be versioned.
- Be auditable.
- Be idempotent.

Events shall never represent commands.

---

# Event Architecture

```
Business Operation

↓

Service Layer

↓

Database Transaction

↓

Commit

↓

Publish Event

↓

Queue

↓

Subscribers

↓

Business Reactions
```

---

# Event Types

FluxDine uses two categories of events.

## Domain Events

Internal events used within the platform.

Examples:

```
OrderCreated

ReservationConfirmed

PaymentCaptured
```

---

## Integration Events

Events intended for external systems.

Examples:

```
order.created

payment.captured

subscription.renewed
```

Integration events may trigger webhooks or third-party integrations.

---

# Event Naming Convention

Internal domain events shall use PascalCase.

Examples

```
OrderCreated

OrderAccepted

OrderCompleted

ReservationCancelled

CustomerRegistered
```

External integration events shall use lowercase dot notation.

Examples

```
order.created

order.completed

payment.refunded

restaurant.updated
```

---

# Standard Event Structure

Every event shall contain:

```json
{
  "eventId": "evt_123456",
  "eventType": "OrderCreated",
  "eventVersion": "1.0",
  "occurredAt": "2026-01-01T12:00:00Z",
  "aggregateId": "...",
  "tenantId": "...",
  "payload": {},
  "metadata": {}
}
```

---

# Event Lifecycle

Every event follows the lifecycle below.

```
Business Action

↓

Transaction Commit

↓

Event Creation

↓

Event Publication

↓

Queue

↓

Subscribers

↓

Processing Complete
```

---

# Domain Event Catalog

## Authentication

```
UserRegistered

UserActivated

UserSuspended

PasswordChanged

PasswordReset

RoleAssigned

PermissionGranted
```

---

## Tenant

```
TenantCreated

TenantActivated

TenantSuspended

TenantArchived

SubscriptionAssigned
```

---

## Restaurant

```
RestaurantCreated

RestaurantUpdated

RestaurantVerified

RestaurantActivated

RestaurantDeactivated
```

---

## Branch

```
BranchCreated

BranchActivated

BranchDeactivated

BranchTransferred
```

---

## Menu

```
MenuCreated

MenuUpdated

MenuPublished

MenuUnpublished
```

---

## Product

```
ProductCreated

ProductUpdated

ProductActivated

ProductDeactivated

ProductDeleted
```

---

## Order

```
OrderCreated

OrderAccepted

OrderRejected

OrderPreparing

OrderReady

OrderDispatched

OrderCompleted

OrderCancelled
```

---

## Reservation

```
ReservationCreated

ReservationConfirmed

ReservationSeated

ReservationCompleted

ReservationCancelled
```

---

## Customer

```
CustomerRegistered

CustomerUpdated

CustomerDeleted
```

---

## Payment

```
PaymentAuthorized

PaymentCaptured

PaymentRefunded

PaymentVoided

PaymentFailed
```

---

## Subscription

```
SubscriptionCreated

SubscriptionRenewed

SubscriptionCancelled

SubscriptionExpired
```

---

## Notification

```
EmailQueued

EmailSent

SMSQueued

SMSSent

PushNotificationSent
```

---

# Event Publishing Rules

Events shall only be published:

- After successful transaction commit.
- From the Service Layer.
- Once per business action.

Repositories shall never publish events.

---

# Event Subscribers

Typical subscribers include:

```
Notification Service

Analytics Service

Webhook Dispatcher

Queue Workers

Reporting Service

Audit Service

Search Indexer
```

Subscribers shall remain independent.

---

# Event Ordering

Ordering is guaranteed only within the same aggregate.

Example:

```
OrderCreated

↓

OrderAccepted

↓

OrderPreparing

↓

OrderReady

↓

OrderCompleted
```

Global ordering is not guaranteed.

---

# Event Versioning

Events shall include:

```
eventVersion
```

Breaking payload changes require a new event version.

---

# Event Idempotency

Every event shall include a globally unique identifier.

Subscribers shall ignore duplicate event deliveries.

---

# Event Retention

Published events shall be retained according to platform retention policies.

Retention periods may vary by:

- Event category
- Compliance requirements
- Audit requirements

---

# Event Security

Events shall:

- Respect tenant isolation.
- Exclude sensitive information.
- Avoid transmitting secrets.
- Encrypt sensitive payloads where required.

---

# Event Error Handling

Subscriber failures shall not affect the originating transaction.

Failed processing shall:

- Be logged.
- Be retried where applicable.
- Generate operational alerts.

---

# Engineering Rules

## Rule EVT-001

Events represent completed business facts.

---

## Rule EVT-002

Events shall be immutable.

---

## Rule EVT-003

Events shall only be published after successful transaction commit.

---

## Rule EVT-004

Repositories shall never publish events.

---

## Rule EVT-005

Events shall include globally unique identifiers.

---

## Rule EVT-006

Event payloads shall remain backward compatible within supported versions.

---

## Rule EVT-007

Subscribers shall process events idempotently.

---

## Rule EVT-008

Events shall respect tenant isolation.

---

## Rule EVT-009

Business logic shall not exist within event handlers.

---

## Rule EVT-010

This document is the authoritative Event Catalog for the FluxDine platform.

---

# Architecture Decision Records

## ADR-EVT-001

FluxDine adopts event-driven architecture for asynchronous workflows.

---

## ADR-EVT-002

Domain events are published only after transaction commit.

---

## ADR-EVT-003

Repositories never publish events.

---

## ADR-EVT-004

Events remain immutable after publication.

---

## ADR-EVT-005

Subscribers process events independently.

---

## ADR-EVT-006

Events include explicit version information.

---

## ADR-EVT-007

Duplicate event processing is prevented through idempotency.

---

## ADR-EVT-008

Integration events remain separate from domain events.

---

## ADR-EVT-009

Event contracts remain implementation independent.

---

## ADR-EVT-010

This document is the authoritative Event Catalog for the FluxDine platform.

---

# Appendix A — Event Matrix

| Domain | Event Count |
|----------|------------:|
| Authentication | 7 |
| Tenant | 5 |
| Restaurant | 5 |
| Branch | 4 |
| Menu | 4 |
| Product | 5 |
| Order | 8 |
| Reservation | 5 |
| Customer | 3 |
| Payment | 5 |
| Subscription | 4 |
| Notification | 5 |

---

# Appendix B — Publisher Matrix

| Publisher | Event Categories |
|------------|------------------|
| AuthService | Authentication |
| TenantService | Tenant |
| RestaurantService | Restaurant |
| MenuService | Menu |
| ProductService | Product |
| OrderService | Orders |
| ReservationService | Reservations |
| PaymentService | Payments |
| SubscriptionService | Subscriptions |

---

# Appendix C — Subscriber Matrix

| Subscriber | Responsibilities |
|------------|------------------|
| Notification Service | Customer communications |
| Analytics Service | Business metrics |
| Audit Service | Audit trail generation |
| Webhook Dispatcher | External notifications |
| Search Indexer | Search synchronization |
| Queue Workers | Asynchronous processing |

---

# Appendix D — Reserved Future Events

Future event domains may include:

```
Inventory*

Supplier*

Kitchen*

Marketplace*

Fleet*

Workforce*

AIRecommendation*

Loyalty*
```

---

# References

- Service Specification
- Repository Specification
- Background Job Specification
- Queue Specification
- Webhook Specification
- REST API Specification
- Integration APIs

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | Initial Release | FluxDine Engineering | Approved as the authoritative Event Catalog for the FluxDine platform |