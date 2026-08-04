# 04 Engineering Specifications

# Database

# 06 — Enum Specification

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-DB-006 |
| **Document Name** | Enum Specification |
| **Version** | 1.0 |
| **Status** | Draft |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | 00 Database Naming Standards<br>01 Complete Database Schema Specification<br>02 Table Specifications<br>03 Relationships<br>04 Constraints<br>05 Index Specification |
| **Referenced By** | 07 Database Migration Strategy<br>08 Drizzle ORM Mapping |

---

# Dependencies

This specification depends upon the following Database Engineering documents:

- 00 Database Naming Standards
- 01 Complete Database Schema Specification
- 02 Table Specifications
- 03 Relationships
- 04 Constraints
- 05 Index Specification

These documents collectively define the entities, relationships, constraints, and indexing strategies upon which the business enumerations are defined.

---

# Referenced By

This specification is consumed by:

- 07 Database Migration Strategy
- 08 Drizzle ORM Mapping

Backend services, APIs, frontend validation, and database implementations shall use the enum definitions specified in this document.

---

# Document Status

| Item | Value |
|------|-------|
| Status | Draft |
| Approval | Pending |
| Implementation | Not Started |
| Last Updated | TBD |

---

# Purpose

The purpose of this document is to define the complete business enumeration model for the FluxDine platform.

Enumerations establish controlled vocabularies that represent business states, classifications, lifecycle stages, configuration values, and operational behaviors.

This document is the authoritative source for every logical enum used throughout the platform.

---

# Scope

This specification defines:

- Platform enums
- Identity enums
- Tenant enums
- Restaurant enums
- Commerce enums
- Customer enums
- Reservation enums
- Billing enums
- Payment enums
- Notification enums
- Analytics enums
- Branding enums
- Feature Management enums
- Shared Platform enums
- Cross-domain enum rules

---

# Out of Scope

This document does not define:

- PostgreSQL ENUM types
- Drizzle ORM enum declarations
- TypeScript enums
- API serialization formats
- UI display labels
- Localization

Implementation-specific definitions are documented separately.

---

# Enum Philosophy

Enumerations represent controlled business values.

Rather than allowing unrestricted text values, every business state shall be represented by a documented enumeration.

Enums provide:

- Data consistency
- Validation
- Predictable business logic
- Easier reporting
- API consistency
- Safer migrations

Every enum shall have documented business meaning.

---

# Enum Design Principles

## Principle 1 — Business Driven

Enums shall represent business concepts rather than implementation details.

---

## Principle 2 — Stable

Existing enum values shall not be renamed without an approved Architecture Decision Record.

---

## Principle 3 — Explicit

Every allowed value shall be explicitly documented.

---

## Principle 4 — Predictable

Enums shall have deterministic meanings across every system component.

---

## Principle 5 — Extensible

Future values shall be introduced without breaking existing implementations.

---

## Principle 6 — Technology Independent

This specification defines logical enums only.

Implementation-specific enum types belong to later engineering specifications.

---

# Enum Categories

FluxDine recognizes the following logical enum categories.

- Status Enums
- Lifecycle Enums
- Classification Enums
- Configuration Enums
- Type Enums
- Visibility Enums
- Security Enums
- Financial Enums
- Operational Enums
- Platform Enums

---

# Standard Enum Specification Template

Every enum defined within this specification shall use the standardized engineering template below.

---

## Enum Name

Official engineering identifier.

---

## Business Domain

Owning business domain.

---

## Purpose

Business meaning of the enum.

---

## Database Type

Logical enum.

---

## Allowed Values

Complete list of valid values.

---

## Default Value

Default business value.

---

## Lifecycle

How values evolve over time.

---

## Business Rules

Rules governing valid usage.

---

## Usage

Where the enum is used throughout the platform.

---

## Validation Rules

Validation requirements.

---

## Future Extension

Guidance for introducing additional values.

---

## Engineering Notes

Implementation guidance.

---

## Related ADRs

Architecture Decision Records governing the enum.

---

# Table of Contents

## Chapter 1 — Platform Enums

## Chapter 2 — Identity Enums

## Chapter 3 — Tenant Enums

## Chapter 4 — Restaurant Enums

## Chapter 5 — Commerce Enums

## Chapter 6 — Customer Enums

## Chapter 7 — Reservation Enums

## Chapter 8 — Billing Enums

## Chapter 9 — Payment Enums

## Chapter 10 — Notification Enums

## Chapter 11 — Analytics Enums

## Chapter 12 — Branding Enums

## Chapter 13 — Feature Management Enums

## Chapter 14 — Shared Platform Enums

## Chapter 15 — Cross-Domain Enum Rules

## Chapter 16 — Enum Versioning Strategy

## Chapter 17 — Engineering Rules

## Chapter 18 — Architecture Decision Records

---

## Appendix A — Complete Enum Matrix

## Appendix B — State Transition Matrix

## Appendix C — Deprecated Enum Values

## Appendix D — Reserved Future Enums

---

## References

(To be completed in the final response.)

---

## Revision History

(To be completed in the final response.)

---

# Chapter 1 — Platform Enums

## 1.1 Purpose

Platform enums define globally shared business values used throughout the FluxDine platform. These values are platform-owned and are not tenant-specific.

---

## Platform Enum Strategy

Platform enums shall:

- Remain globally consistent.
- Be centrally managed.
- Be reusable across all business domains.
- Rarely change.

---

### Enum: Environment

**Purpose**

Identifies the deployment environment.

**Allowed Values**

- development
- testing
- staging
- production

**Default Value**

development

---

### Enum: Language Status

**Purpose**

Defines whether a language is available for platform use.

**Allowed Values**

- active
- inactive
- deprecated

**Default Value**

active

---

### Enum: Currency Status

**Purpose**

Defines whether a currency is available.

**Allowed Values**

- active
- inactive

**Default Value**

active

---

### Enum: Country Status

**Purpose**

Defines supported countries.

**Allowed Values**

- supported
- unsupported

**Default Value**

supported

---

### Enum: Time Zone Status

**Purpose**

Defines whether a time zone is available.

**Allowed Values**

- active
- deprecated

**Default Value**

active

---

# Chapter 2 — Identity Enums

## 2.1 Purpose

Identity enums govern authentication, authorization, account lifecycle, sessions, and security.

---

### Enum: User Status

**Allowed Values**

- invited
- pending_verification
- active
- suspended
- locked
- archived

**Default Value**

pending_verification

---

### Enum: Membership Status

**Allowed Values**

- pending
- active
- suspended
- removed

**Default Value**

pending

---

### Enum: Session Status

**Allowed Values**

- active
- expired
- revoked

**Default Value**

active

---

### Enum: MFA Method

**Allowed Values**

- authenticator_app
- sms
- email
- backup_codes

---

### Enum: Authentication Provider

**Allowed Values**

- local
- google
- microsoft
- github

**Default Value**

local

---

### Enum: Token Status

**Allowed Values**

- active
- consumed
- expired

**Default Value**

active

---

# Chapter 3 — Tenant Enums

## 3.1 Purpose

Tenant enums define the operational lifecycle of customer organizations.

---

### Enum: Tenant Status

**Allowed Values**

- onboarding
- active
- suspended
- cancelled
- archived

**Default Value**

onboarding

---

### Enum: Domain Status

**Allowed Values**

- pending_verification
- verified
- failed
- disabled

**Default Value**

pending_verification

---

### Enum: Subscription Status

**Allowed Values**

- trial
- active
- past_due
- suspended
- cancelled
- expired

**Default Value**

trial

---

### Enum: Integration Status

**Allowed Values**

- pending
- connected
- disconnected
- failed

**Default Value**

pending

---

### Enum: API Key Status

**Allowed Values**

- active
- revoked
- expired

**Default Value**

active

---

# Chapter 4 — Restaurant Enums

## 4.1 Purpose

Restaurant enums govern restaurant operations, staffing, seating, and delivery.

---

### Enum: Restaurant Status

**Allowed Values**

- onboarding
- active
- temporarily_closed
- permanently_closed

**Default Value**

onboarding

---

### Enum: Branch Status

**Allowed Values**

- active
- temporarily_closed
- permanently_closed

**Default Value**

active

---

### Enum: Staff Status

**Allowed Values**

- active
- on_leave
- suspended
- terminated

**Default Value**

active

---

### Enum: Rider Status

**Allowed Values**

- offline
- available
- assigned
- delivering
- unavailable

**Default Value**

offline

---

### Enum: Kitchen Station Status

**Allowed Values**

- active
- maintenance
- offline

**Default Value**

active

---

### Enum: Restaurant Table Status

**Allowed Values**

- available
- reserved
- occupied
- cleaning
- unavailable

**Default Value**

available

---

# Chapter 5 — Commerce Enums

## 5.1 Purpose

Commerce enums define the lifecycle of menus, carts, orders, and fulfillment.

---

### Enum: Menu Status

**Allowed Values**

- draft
- active
- archived

**Default Value**

draft

---

### Enum: Menu Item Status

**Allowed Values**

- available
- unavailable
- discontinued

**Default Value**

available

---

### Enum: Cart Status

**Allowed Values**

- active
- checked_out
- abandoned
- expired

**Default Value**

active

---

### Enum: Order Status

**Allowed Values**

- pending
- confirmed
- preparing
- ready
- out_for_delivery
- completed
- cancelled

**Default Value**

pending

---

### Enum: Order Type

**Allowed Values**

- delivery
- pickup
- dine_in

**Default Value**

delivery

---

### Enum: Fulfillment Method

**Allowed Values**

- restaurant_delivery
- self_pickup
- third_party_delivery

**Default Value**

restaurant_delivery

---

# Chapter 6 — Customer Enums

## 6.1 Purpose

Customer enums define the business states governing customer profiles, addresses, loyalty participation, communication preferences, and customer engagement throughout the FluxDine platform.

These enumerations ensure consistent customer lifecycle management across ordering, reservations, marketing, and support services.

---

## Customer Enum Strategy

Customer enums shall:

- Represent customer lifecycle states.
- Support customer personalization.
- Enable loyalty management.
- Maintain communication preferences.
- Remain tenant-aware while preserving customer consistency.

---

### Enum: Customer Status

**Purpose**

Defines the operational state of a customer account.

**Allowed Values**

- guest
- registered
- active
- suspended
- blocked
- archived

**Default Value**

guest

---

### Enum: Address Type

**Purpose**

Classifies customer addresses.

**Allowed Values**

- home
- office
- other

**Default Value**

home

---

### Enum: Loyalty Tier

**Purpose**

Represents the customer's loyalty level.

**Allowed Values**

- bronze
- silver
- gold
- platinum

**Default Value**

bronze

---

### Enum: Loyalty Transaction Type

**Purpose**

Defines loyalty point activity.

**Allowed Values**

- earned
- redeemed
- adjusted
- expired

**Default Value**

earned

---

### Enum: Communication Preference

**Purpose**

Defines preferred communication channels.

**Allowed Values**

- email
- sms
- push_notification
- whatsapp

**Default Value**

email

---

### Enum: Device Type

**Purpose**

Identifies registered customer devices.

**Allowed Values**

- android
- ios
- web

**Default Value**

web

---

# Chapter 7 — Reservation Enums

## 7.1 Purpose

Reservation enums govern reservation lifecycle management, seating allocation, waitlists, and reservation history.

These enums provide consistent reservation state management throughout the platform.

---

## Reservation Enum Strategy

Reservation enums shall:

- Define reservation lifecycle.
- Support seating workflows.
- Enable waitlist processing.
- Preserve reservation history.

---

### Enum: Reservation Status

**Purpose**

Represents the lifecycle state of a reservation.

**Allowed Values**

- pending
- upcoming
- active
- fulfilled
- cancelled
- no_show

**Default Value**

pending

---

### Enum: Reservation Event Type

**Purpose**

Defines historical reservation events.

**Allowed Values**

- created
- confirmed
- modified
- checked_in
- completed
- cancelled

**Default Value**

created

---

### Enum: Waitlist Status

**Purpose**

Defines customer waitlist progression.

**Allowed Values**

- waiting
- notified
- seated
- cancelled
- expired

**Default Value**

waiting

---

### Enum: Seating Assignment Status

**Purpose**

Defines seating allocation state.

**Allowed Values**

- pending
- assigned
- occupied
- released

**Default Value**

pending

---

# Chapter 8 — Billing Enums

## 8.1 Purpose

Billing enums define subscription billing, invoice lifecycle, refunds, credits, and financial processing.

These enumerations ensure accounting consistency across the platform.

---

## Billing Enum Strategy

Billing enums shall:

- Represent financial document states.
- Support recurring subscriptions.
- Preserve immutable accounting history.
- Maintain refund workflows.

---

### Enum: Billing Account Status

**Purpose**

Represents billing account availability.

**Allowed Values**

- active
- suspended
- closed

**Default Value**

active

---

### Enum: Subscription Status

**Purpose**

Defines subscription lifecycle.

**Allowed Values**

- trial
- active
- past_due
- suspended
- cancelled
- expired

**Default Value**

trial

---

### Enum: Invoice Status

**Purpose**

Represents invoice lifecycle.

**Allowed Values**

- draft
- issued
- paid
- overdue
- void

**Default Value**

draft

---

### Enum: Invoice Type

**Purpose**

Classifies invoices.

**Allowed Values**

- subscription
- adjustment
- manual

**Default Value**

subscription

---

### Enum: Refund Status

**Purpose**

Defines refund workflow.

**Allowed Values**

- requested
- approved
- processing
- completed
- rejected

**Default Value**

requested

---

### Enum: Credit Note Status

**Purpose**

Represents credit note lifecycle.

**Allowed Values**

- issued
- applied
- cancelled

**Default Value**

issued

---

# Chapter 9 — Payment Enums

## 9.1 Purpose

Payment enums govern the shared Payment Service and Payment Gateway Abstraction Layer.

These enums remain provider-independent while supporting multiple payment gateways and merchant accounts.

---

## Payment Enum Strategy

Payment enums shall:

- Remain gateway-independent.
- Support multiple payment providers.
- Preserve payment history.
- Enable reconciliation.
- Support asynchronous payment workflows.

---

### Enum: Payment Status

**Purpose**

Represents payment transaction lifecycle.

**Allowed Values**

- pending
- authorized
- captured
- failed
- refunded
- cancelled

**Default Value**

pending

---

### Enum: Payment Method

**Purpose**

Identifies customer payment methods.

**Allowed Values**

- card
- digital_wallet
- bank_transfer
- cash_on_delivery

**Default Value**

card

---

### Enum: Payment Provider

**Purpose**

Identifies the payment gateway used.

**Allowed Values**

- stripe
- creem
- paypal
- manual

**Default Value**

manual

---

### Enum: Payment Transaction Type

**Purpose**

Classifies payment transactions.

**Allowed Values**

- authorization
- capture
- refund
- reversal

**Default Value**

authorization

---

### Enum: Settlement Status

**Purpose**

Represents settlement processing.

**Allowed Values**

- pending
- settled
- failed

**Default Value**

pending

---

### Enum: Reconciliation Status

**Purpose**

Represents reconciliation progress.

**Allowed Values**

- pending
- matched
- exception

**Default Value**

pending

---

### Enum: Webhook Processing Status

**Purpose**

Tracks webhook processing.

**Allowed Values**

- received
- processing
- processed
- failed

**Default Value**

received

---

# Chapter 10 — Notification Enums

## 10.1 Purpose

Notification enums define message generation, delivery processing, communication channels, and notification lifecycle.

These enums ensure reliable communication across email, SMS, push notifications, and future messaging channels.

---

## Notification Enum Strategy

Notification enums shall:

- Support asynchronous processing.
- Enable delivery tracking.
- Preserve notification history.
- Support multiple communication channels.

---

### Enum: Notification Status

**Purpose**

Represents notification lifecycle.

**Allowed Values**

- pending
- queued
- sending
- delivered
- failed
- cancelled

**Default Value**

pending

---

### Enum: Notification Channel

**Purpose**

Defines the delivery channel.

**Allowed Values**

- email
- sms
- push_notification
- whatsapp
- in_app

**Default Value**

email

---

### Enum: Delivery Status

**Purpose**

Tracks delivery outcome.

**Allowed Values**

- pending
- sent
- delivered
- bounced
- failed

**Default Value**

pending

---

### Enum: Queue Status

**Purpose**

Represents notification queue processing.

**Allowed Values**

- waiting
- processing
- completed
- failed

**Default Value**

waiting

---

### Enum: Notification Priority

**Purpose**

Defines processing priority.

**Allowed Values**

- low
- normal
- high
- critical

**Default Value**

normal

---

# Chapter 11 — Analytics Enums

## 11.1 Purpose

Analytics enums define the standardized classifications used throughout the FluxDine reporting and business intelligence platform.

These enumerations provide consistent definitions for KPIs, reports, metrics, aggregations, and analytical processing while ensuring that analytics remains a read-model and never becomes the operational source of truth.

---

## Analytics Enum Strategy

Analytics enums shall:

- Standardize reporting.
- Classify business metrics.
- Support dashboard generation.
- Enable historical analysis.
- Remain independent from operational business logic.

---

### Enum: KPI Type

**Purpose**

Classifies Key Performance Indicators.

**Allowed Values**

- operational
- financial
- customer
- marketing
- system

**Default Value**

operational

---

### Enum: KPI Frequency

**Purpose**

Defines KPI calculation intervals.

**Allowed Values**

- real_time
- hourly
- daily
- weekly
- monthly

**Default Value**

daily

---

### Enum: Report Type

**Purpose**

Defines report classifications.

**Allowed Values**

- operational
- financial
- analytical
- audit
- executive

**Default Value**

operational

---

### Enum: Metric Type

**Purpose**

Defines measurement categories.

**Allowed Values**

- count
- percentage
- currency
- duration
- ratio

**Default Value**

count

---

### Enum: Aggregation Method

**Purpose**

Defines statistical aggregation.

**Allowed Values**

- sum
- average
- minimum
- maximum
- count

**Default Value**

count

---

# Chapter 12 — Branding Enums

## 12.1 Purpose

Branding enums define the business classifications governing tenant branding, themes, domains, SEO, and brand assets.

These enums ensure consistent branding behavior throughout every tenant website.

---

## Branding Enum Strategy

Branding enums shall:

- Standardize tenant branding.
- Support domain management.
- Enable theme configuration.
- Preserve branding consistency.

---

### Enum: Theme Status

**Purpose**

Defines theme lifecycle.

**Allowed Values**

- draft
- active
- archived

**Default Value**

draft

---

### Enum: Theme Mode

**Purpose**

Defines theme appearance.

**Allowed Values**

- light
- dark
- auto

**Default Value**

light

---

### Enum: Domain Verification Status

**Purpose**

Represents custom domain verification.

**Allowed Values**

- pending
- verified
- failed

**Default Value**

pending

---

### Enum: Brand Asset Type

**Purpose**

Classifies branding assets.

**Allowed Values**

- logo
- favicon
- banner
- background
- icon

**Default Value**

logo

---

### Enum: SEO Status

**Purpose**

Defines SEO configuration status.

**Allowed Values**

- draft
- published
- archived

**Default Value**

draft

---

# Chapter 13 — Feature Management Enums

## 13.1 Purpose

Feature Management enums define the lifecycle, visibility, rollout, and availability of platform features.

These enums support subscription entitlements, tenant overrides, and controlled feature releases.

---

## Feature Management Enum Strategy

Feature enums shall:

- Control feature availability.
- Support progressive rollout.
- Enable subscription management.
- Preserve platform stability.

---

### Enum: Feature Status

**Purpose**

Represents feature lifecycle.

**Allowed Values**

- development
- testing
- active
- deprecated

**Default Value**

development

---

### Enum: Feature Visibility

**Purpose**

Defines feature exposure.

**Allowed Values**

- public
- internal
- experimental

**Default Value**

public

---

### Enum: Feature Flag Status

**Purpose**

Defines runtime availability.

**Allowed Values**

- enabled
- disabled

**Default Value**

disabled

---

### Enum: Feature Assignment Type

**Purpose**

Defines feature assignment source.

**Allowed Values**

- subscription
- tenant_override
- platform_default

**Default Value**

platform_default

---

### Enum: Feature Usage Status

**Purpose**

Tracks feature utilization.

**Allowed Values**

- active
- inactive

**Default Value**

active

---

# Chapter 14 — Shared Platform Enums

## 14.1 Purpose

Shared Platform enums define common business classifications used across all domains of the FluxDine platform.

These enums support auditing, logging, monitoring, file management, background processing, and shared infrastructure services.

---

## Shared Platform Enum Strategy

Shared Platform enums shall:

- Be reusable.
- Remain technology-independent.
- Support infrastructure services.
- Maintain operational consistency.

---

### Enum: Background Job Status

**Purpose**

Defines background processing lifecycle.

**Allowed Values**

- pending
- running
- completed
- failed
- cancelled

**Default Value**

pending

---

### Enum: Job Priority

**Purpose**

Defines execution priority.

**Allowed Values**

- low
- normal
- high
- critical

**Default Value**

normal

---

### Enum: Log Severity

**Purpose**

Classifies log importance.

**Allowed Values**

- debug
- information
- warning
- error
- critical

**Default Value**

information

---

### Enum: Audit Action

**Purpose**

Defines recorded audit operations.

**Allowed Values**

- create
- update
- delete
- restore
- login
- logout

**Default Value**

create

---

### Enum: File Status

**Purpose**

Represents managed file lifecycle.

**Allowed Values**

- uploading
- available
- archived
- deleted

**Default Value**

uploading

---

### Enum: Monitoring Status

**Purpose**

Represents monitored system health.

**Allowed Values**

- healthy
- degraded
- unavailable

**Default Value**

healthy

---

# Chapter 15 — Cross-Domain Enum Rules

## 15.1 Purpose

Cross-Domain Enum Rules establish engineering standards governing enum usage across multiple business domains.

These rules ensure consistency, interoperability, and long-term maintainability throughout the FluxDine platform.

---

## Rule CDE-001

Every enum shall belong to exactly one owning business domain.

---

## Rule CDE-002

Enum values shall represent business concepts rather than implementation details.

---

## Rule CDE-003

Enum values shall be immutable after production release.

Breaking changes require an approved Architecture Decision Record (ADR).

---

## Rule CDE-004

Enum names shall follow the naming standards defined in **00 Database Naming Standards.md**.

---

## Rule CDE-005

Default values shall represent the initial business state of an entity.

---

## Rule CDE-006

Enum values shall be lowercase using snake_case formatting.

Examples:

- pending_verification
- out_for_delivery
- cash_on_delivery

---

## Rule CDE-007

Enums shall not contain localized or user-facing display text.

Localization shall be handled separately by the presentation layer.

---

## Rule CDE-008

Enums shall remain technology-independent.

This specification defines logical business enums only.

---

## Rule CDE-009

Shared enums shall be reused wherever business meaning is identical.

Duplicate enums representing the same business concept are prohibited.

---

## Rule CDE-010

Every new enum introduced into the platform shall be documented within this specification before implementation.

---

# Chapter 16 — Enum Versioning Strategy

## 16.1 Purpose

The Enum Versioning Strategy defines the engineering principles governing the evolution of business enumerations throughout the FluxDine platform.

Enumerations represent contractual business values shared across the database, backend services, APIs, frontend applications, integrations, and reporting systems. Consequently, changes to enums must be carefully controlled to preserve backward compatibility and maintain long-term architectural stability.

---

## 16.2 Versioning Objectives

The enum versioning strategy shall ensure:

- Backward compatibility
- Stable APIs
- Predictable migrations
- Safe feature evolution
- Cross-service consistency
- Long-term maintainability

---

## 16.3 Versioning Principles

### Principle 1

Existing enum values shall never be renamed after production release.

---

### Principle 2

Existing enum values shall never change business meaning.

---

### Principle 3

New values may be appended only when business requirements justify the addition.

---

### Principle 4

Deprecated values shall remain documented until completely removed from every supported platform version.

---

### Principle 5

Breaking enum changes require an approved Architecture Decision Record (ADR).

---

### Principle 6

Every enum modification shall be reflected in:

- Database migrations
- API documentation
- Backend validation
- Frontend validation
- Drizzle ORM mappings
- Integration documentation

---

## 16.4 Enum Evolution Lifecycle

Every enum follows the lifecycle below:

1. Proposed
2. Approved
3. Implemented
4. Production
5. Deprecated (if required)
6. Removed (future major release only)

---

## 16.5 Deprecation Policy

Deprecated enum values:

- Shall remain documented.
- Shall not be assigned to newly created records.
- Shall remain readable by existing systems.
- Shall be removed only after successful migration.

---

# Chapter 17 — Engineering Rules

## 17.1 Purpose

These engineering rules define the mandatory standards governing every business enumeration within the FluxDine platform.

---

# Engineering Rules

## Rule ERE-001

Every business enum shall be documented within this specification.

---

## Rule ERE-002

Every enum shall belong to exactly one business domain.

---

## Rule ERE-003

Enum values shall use lowercase snake_case formatting.

---

## Rule ERE-004

Enums shall represent business concepts rather than implementation details.

---

## Rule ERE-005

Duplicate enums representing identical business concepts are prohibited.

---

## Rule ERE-006

Every enum shall define a documented default value whenever applicable.

---

## Rule ERE-007

Business validation shall use documented enum values only.

---

## Rule ERE-008

Enums shall remain technology-independent within this specification.

---

## Rule ERE-009

Breaking enum modifications require architecture approval.

---

## Rule ERE-010

This document is the authoritative engineering specification for all logical business enumerations within the FluxDine platform.

---

# Chapter 18 — Architecture Decision Records

The following Architecture Decision Records govern the business enumeration strategy.

---

## ADR-E-001

Every business state shall be represented by a documented enumeration.

---

## ADR-E-002

Enums shall represent business meaning rather than implementation details.

---

## ADR-E-003

Existing enum values shall remain backward compatible after production release.

---

## ADR-E-004

Enum modifications shall follow the documented versioning strategy.

---

## ADR-E-005

Duplicate business enums shall be eliminated through shared reusable definitions.

---

## ADR-E-006

Platform-wide enums shall be centrally governed.

---

## ADR-E-007

Enums shall remain independent of programming language implementations.

---

## ADR-E-008

Deprecated enum values shall remain documented until complete migration.

---

## ADR-E-009

Cross-domain enum consistency shall be maintained through this specification.

---

## ADR-E-010

This document is the authoritative source for logical business enumerations within the FluxDine platform.

---

# Appendix A — Complete Enum Matrix

| Domain | Status | Lifecycle | Classification | Configuration | Type | Operational |
|---------|:------:|:---------:|:--------------:|:-------------:|:----:|:-----------:|
| Platform | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Identity | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Tenant | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Restaurant | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Commerce | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Customer | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Reservation | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Billing | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Payment | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Notification | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Analytics | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Branding | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Feature Management | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Shared Platform | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |

---

# Appendix B — State Transition Matrix

This appendix summarizes the primary lifecycle enums defined throughout the platform.

| Enum | Initial State | Terminal States |
|------|---------------|-----------------|
| User Status | pending_verification | archived |
| Tenant Status | onboarding | archived |
| Restaurant Status | onboarding | permanently_closed |
| Order Status | pending | completed, cancelled |
| Reservation Status | pending | fulfilled, cancelled, no_show |
| Invoice Status | draft | paid, void |
| Payment Status | pending | captured, refunded, cancelled, failed |
| Notification Status | pending | delivered, failed, cancelled |
| Background Job Status | pending | completed, cancelled, failed |

---

## Engineering Note

Detailed state transition rules are defined within the corresponding business domain specifications.

---

# Appendix C — Deprecated Enum Values

Deprecated enum values shall remain documented until all supported platform versions no longer reference them.

The following table is intentionally maintained even when empty.

| Enum | Deprecated Value | Replacement | Removal Version |
|------|------------------|-------------|-----------------|
| None | — | — | — |

---

## Deprecation Rules

- Deprecated values shall not be assigned to new records.
- Existing records shall remain readable.
- Removal requires an approved Architecture Decision Record.
- Migrations shall precede physical removal.

---

# Appendix D — Reserved Future Enums

The following enum groups are reserved for future platform capabilities.

---

## Inventory

Examples include:

- Inventory Status
- Stock Movement Type
- Purchase Order Status
- Supplier Status

---

## Kitchen Display System (KDS)

Examples include:

- Kitchen Order Status
- Preparation Status
- Kitchen Queue Status
- Station Assignment Status

---

## Marketplace

Examples include:

- Marketplace Order Status
- Marketplace Settlement Status
- Marketplace Commission Status
- Merchant Verification Status

---

## Artificial Intelligence

Examples include:

- AI Conversation Status
- AI Recommendation Type
- AI Model Status
- AI Processing Status

---

## Workforce Management

Examples include:

- Attendance Status
- Shift Status
- Leave Status
- Payroll Status

---

## Fleet Management

Examples include:

- Vehicle Status
- Driver Assignment Status
- Route Status
- Delivery Route State

---

Future enums shall be introduced only through approved Architecture Decision Records and documented within future revisions of this specification.

---

# References

This specification should be read together with the following FluxDine Architecture Bible documents.

## Database Engineering

- 00 Database Naming Standards
- 01 Complete Database Schema Specification
- 02 Table Specifications
- 03 Relationships
- 04 Constraints
- 05 Index Specification
- 07 Database Migration Strategy
- 08 Drizzle ORM Mapping

---

## Core Architecture

- System Architecture Blueprint
- Database Architecture & Multi-Tenant Data Model
- API & Service Architecture
- Security Architecture
- Infrastructure Architecture

---

## Governance

- Architecture Principles
- Documentation Standards
- ADR Register

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 0.1 | Initial Draft | FluxDine Engineering | Document structure established |
| 0.5 | Domain Enumerations | FluxDine Engineering | Platform through Shared Platform enum definitions completed |
| 0.8 | Governance | FluxDine Engineering | Cross-domain rules, versioning strategy, engineering rules, and ADRs completed |
| 1.0 | Final Release | FluxDine Engineering | Approved as the authoritative business enumeration specification |

---