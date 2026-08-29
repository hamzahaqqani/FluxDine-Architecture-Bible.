04 Shared Platform Services

03 — Tenant Service

Document Control

Field

Value

Document ID

FD-SPS-003

Document Name

Tenant Service

Version

1.0

Status

Approved and Locked

Owner

FluxDine Platform Architecture Team

Classification

Core Platform Service

Depends On

Shared Services Overview, Identity Service

Referenced By

HQ Platform, Restaurant Platform, Self-Service Platform, Shared Platform Services

Purpose

The Tenant Service is the root business service responsible for managing the lifecycle of restaurant businesses operating on the FluxDine platform.

It is the authoritative owner of tenant-specific business context and tenant lifecycle management.

The Tenant Service establishes and maintains the organizational boundary within which tenant-specific platform resources operate.

Conceptually:

FluxDine Platform
       ↓
Tenant Service
       ↓
Tenant
       ↓
Restaurant / Platform Resources

Every independent restaurant organization operating on FluxDine must belong to an identifiable tenant.

The Tenant Service ensures that tenant lifecycle, tenant configuration, and tenant metadata remain centrally owned and consistently managed.

Architectural Role

The Tenant Service is the root business service for tenant-specific operations.

It provides the tenant context used by other platform services.

Conceptually:

Identity Service
       ↓
Authorization Context
       ↓
Tenant Service
       ↓
Restaurant Service
       ↓
Commerce / Billing / Other Services

The Tenant Service does not replace the Identity Service.

Identity establishes the authenticated user.

The Tenant Service establishes the tenant context associated with that user or operation.

Responsibilities

The Tenant Service manages:

Tenant Registration

Tenant Activation

Tenant Suspension

Tenant Archival

Tenant Configuration

Tenant Branding

Tenant Domains

Tenant Onboarding

Tenant Feature Availability

Tenant Metadata

Tenant Subscription Association

The Tenant Service is responsible for maintaining the lifecycle and organizational context of each tenant.

Core Business Operations

Typical Tenant Service operations include:

CreateTenant()
ActivateTenant()
SuspendTenant()
ArchiveTenant()
UpdateTenantSettings()
AssignSubscription()
ConnectDomain()

These operations represent business capabilities.

Their exact API contracts and implementation details must follow the approved API and engineering specifications.

Tenant Lifecycle

The Tenant Service owns the tenant lifecycle.

A conceptual lifecycle is:

Tenant Registration
        ↓
Tenant Created
        ↓
Tenant Activation
        ↓
Active
        ↓
Suspended
        ↓
Active
        ↓
Archived

The exact valid transitions must follow the approved tenant lifecycle rules and applicable Architecture Decision Records.

Invalid lifecycle transitions must be rejected.

Tenant Registration

Tenant registration establishes a new restaurant organization within the FluxDine platform.

A tenant registration process may involve:

User / Business
      ↓
Identity
      ↓
Tenant Registration
      ↓
Tenant Created
      ↓
Onboarding
      ↓
Restaurant Configuration

Tenant creation must establish a unique tenant identity and the minimum metadata required for the tenant to operate within the platform.

Tenant creation must not bypass Identity or authorization requirements.

Tenant Activation

A tenant may become active after satisfying the conditions required by the approved onboarding and subscription workflows.

Activation makes the tenant eligible to use the applicable platform capabilities.

Activation must be an explicit business operation.

A tenant must not automatically become fully operational merely because a tenant record exists.

Tenant Suspension

The Tenant Service owns tenant suspension.

A suspended tenant remains part of the platform but may be prevented from performing some or all tenant operations depending on the reason for suspension and applicable business rules.

Suspension may occur because of:

Administrative action.

Billing or subscription state.

Security concerns.

Business policy.

Other approved platform conditions.

The exact suspension rules must follow the applicable product, billing, security, and architecture specifications.

Tenant Archival

The Tenant Service owns tenant archival.

Archival represents a controlled lifecycle state in which the tenant is no longer actively operating on the platform.

Archival must not be confused with immediate physical deletion of all tenant data.

Data retention, deletion, backup, and compliance requirements are governed by the applicable data and infrastructure architecture.

Tenant Configuration

The Tenant Service owns tenant-level configuration.

Examples may include:

Tenant settings.

Tenant metadata.

Tenant operational preferences.

Tenant-level configuration required by shared platform capabilities.

Configuration belonging specifically to restaurant operations must remain owned by the Restaurant Service.

Configuration belonging specifically to billing must remain owned by the Billing Service.

Configuration belonging specifically to themes must remain owned by the Theme Service.

The Tenant Service must not become a generic configuration database for every platform domain.

Tenant Branding

The Tenant Service owns tenant-level branding context.

Tenant branding may include organizational identity information required across the platform.

However, detailed visual theme configuration remains the responsibility of the Theme Service.

Conceptually:

Tenant Service
      ↓
Tenant Identity / Branding Context
      ↓
Theme Service
      ↓
Visual Theme Configuration

The two responsibilities must remain distinct.

Tenant Domains

The Tenant Service participates in tenant domain ownership and association.

Typical tenant domain operations include:

ConnectDomain()

However, technical domain operations such as:

DNS validation.

SSL provisioning.

Domain verification.

DNS configuration.

are handled by the Domain Service.

Therefore:

Tenant Service
      ↓
Tenant Domain Association
      ↓
Domain Service
      ↓
DNS / SSL / Verification

The Tenant Service owns the tenant's relationship with the domain.

The Domain Service owns the technical domain-management capabilities.

Tenant Onboarding

The Tenant Service participates in the tenant onboarding lifecycle.

A conceptual onboarding flow is:

Business Registration
        ↓
Identity
        ↓
Plan Selection
        ↓
Payment / Billing
        ↓
Tenant Created
        ↓
Tenant Configuration
        ↓
Restaurant Configuration
        ↓
Theme Configuration
        ↓
Payment Gateway Configuration
        ↓
Domain Configuration
        ↓
Launch

The Tenant Service owns the tenant lifecycle portion of this process.

It does not own every individual step.

Each step must remain owned by its appropriate service.

Tenant Feature Availability

The Tenant Service maintains tenant-level feature availability context where required.

Feature availability may depend on:

Tenant state.

Subscription state.

Platform configuration.

Feature flags.

Business rules.

The Feature Flag Service remains the authoritative owner of feature flag evaluation and feature-toggle infrastructure.

Therefore:

Tenant
  ↓
Subscription / Eligibility Context
  ↓
Feature Flag Service
  ↓
Feature Availability

Tenant Service must not duplicate Feature Flag Service logic.

Tenant and Identity Relationship

Identity and Tenant are separate architectural concepts.

The Identity Service answers:

Who is the authenticated user?

The Tenant Service answers:

Which tenant context does this operation belong to?

Conceptually:

User
 ↓
Identity Service
 ↓
Authenticated Identity
 ↓
Tenant Context
 ↓
Tenant Service
 ↓
Authorization
 ↓
Tenant Resource

The Identity Service owns users, authentication, authorization, roles, and permissions.

The Tenant Service owns tenant lifecycle, tenant configuration, and tenant metadata.

Neither service should absorb the other's authoritative responsibilities.

Tenant and Restaurant Relationship

A tenant represents the restaurant organization operating on FluxDine.

The Restaurant Service manages restaurant operational data.

Conceptually:

Tenant
   ↓
Restaurant
   ↓
Branches
   ↓
Menus / Reservations / Offers / Operations

The Tenant Service owns the organizational boundary.

The Restaurant Service owns restaurant operational management.

The Tenant Service must not directly own restaurant operational data.

Tenant and Billing Relationship

The Tenant Service is the root owner of the tenant lifecycle.

The Billing Service owns SaaS subscription management.

Conceptually:

Tenant Service
      ↓
Tenant
      ↓
Billing Service
      ↓
Subscription

The Tenant Service may associate a tenant with its subscription context.

However, Billing Service owns:

Plans.

Trials.

Subscriptions.

Renewals.

Invoices.

Billing history.

Subscription lifecycle.

The Tenant Service must not duplicate billing business logic.

Tenant and Payment Relationship

The Tenant Service does not process payments directly.

Payment processing belongs to the Payment Service.

Conceptually:

Tenant / Billing
       ↓
Billing Service
       ↓
Payment Service
       ↓
Payment Gateway Abstraction
       ↓
Payment Provider

The Tenant Service must never communicate directly with external payment providers.

Tenant Isolation

Tenant isolation is a foundational requirement of FluxDine.

Every tenant-specific operation must execute within the correct tenant context.

Conceptually:

Tenant A
   ↓
Tenant A Resources

Tenant B
   ↓
Tenant B Resources

The platform must prevent unauthorized cross-tenant access.

A tenant identifier supplied by an untrusted client must never independently establish authorization.

Tenant context must be established and validated through trusted server-side mechanisms.

Tenant Context

Tenant context should be available to services handling tenant-scoped operations.

Conceptually:

Request
   ↓
Authentication
   ↓
Authorization
   ↓
Tenant Context
   ↓
Business Service
   ↓
Tenant-Scoped Resource

Services must validate that the authenticated identity is authorized to operate within the requested tenant context.

Data Ownership

The Tenant Service exclusively owns:

Tenant records.

Tenant lifecycle state.

Tenant metadata.

Tenant configuration.

Tenant-level branding context.

Tenant domain associations.

Tenant onboarding state where applicable.

Tenant feature availability context where applicable.

Tenant-to-subscription association where applicable.

Other services must not directly modify Tenant Service data.

Data That Tenant Service Does Not Own

The Tenant Service does not own:

User identities.

Credentials.

Passwords.

Sessions.

Roles.

Permissions.

Orders.

Carts.

Payments.

Invoices.

Restaurant operational data.

Menu data.

Reservations.

Notification delivery.

Analytics data.

Technical DNS infrastructure.

Theme implementation details.

Those responsibilities belong to their respective services.

Service Boundaries

The Tenant Service owns:

Tenant Lifecycle
Tenant Configuration
Tenant Metadata
Tenant Context

It does not own:

Authentication
Restaurant Operations
Orders
Payments
Notifications

The service must maintain these boundaries throughout implementation.

Public APIs

Typical APIs may include:

Create Tenant
Get Tenant
Update Tenant
Activate Tenant
Suspend Tenant
Archive Tenant
Get Tenant Configuration
Update Tenant Configuration
Assign Subscription
Connect Domain
Get Tenant Status

The exact API paths, HTTP methods, request contracts, response contracts, authorization requirements, and versioning must follow the FluxDine API Standards and Engineering Specifications.

Published Events

The Tenant Service may publish tenant lifecycle events including:

TenantCreated
TenantActivated
TenantSuspended
TenantArchived
TenantConfigurationUpdated
TenantBrandingUpdated
TenantDomainConnected
TenantSubscriptionAssigned

Events must be published through the approved Event Bus.

Event payloads must contain only the information required by consumers.

Sensitive information must not be unnecessarily included in events.

Consumed Events

The Tenant Service may consume events from other platform services where required.

Potential examples include:

SubscriptionActivated
SubscriptionCancelled
PaymentStatusChanged
DomainVerified

The exact event contracts must follow the approved Event Architecture and applicable service specifications.

Consumed events must not transfer ownership of another service's business data to Tenant Service.

Event Processing

Tenant Service event processing must support:

Retry.

Duplicate delivery.

Idempotency.

Failure handling.

Eventual consistency where applicable.

A failed event must not result in an invalid tenant lifecycle state.

Database Ownership

The Tenant Service follows the Database-per-Service architecture.

Its tenant database is exclusively owned by the Tenant Service.

No other service may:

Query the Tenant Service database directly.

Write directly to Tenant tables.

Modify Tenant Service records outside approved interfaces.

Depend on internal Tenant database schemas.

Cross-service interaction must occur through:

API
Event
Approved Abstraction

Security

The Tenant Service must enforce tenant isolation and tenant-level authorization requirements.

Security controls must include:

Server-side tenant validation.

Authorization checks.

Tenant boundary enforcement.

Protection against cross-tenant access.

Input validation.

Secure handling of tenant metadata.

Auditability of administrative tenant operations.

Security requirements must follow the platform Security Architecture.

Authorization

Tenant-level operations must require appropriate authorization.

Conceptually:

Identity
   ↓
Role
   ↓
Permission
   ↓
Tenant Context
   ↓
Tenant Authorization
   ↓
Operation

Authorization must not rely solely on:

Client-side state.

URL parameters.

Request-body tenant IDs.

Local storage.

UI visibility.

The server remains the authoritative security boundary.

Audit Integration

Important tenant lifecycle operations must generate audit records.

Examples include:

TenantCreated
TenantActivated
TenantSuspended
TenantArchived
TenantConfigurationUpdated
TenantBrandingUpdated
TenantDomainConnected
TenantSubscriptionAssigned

The Tenant Service produces the relevant business/security information.

The Audit Service owns centralized audit storage and retrieval.

Notifications

The Tenant Service does not directly implement notification delivery.

When tenant lifecycle events require communication:

Tenant Service
      ↓
Event / Notification Request
      ↓
Notification Service
      ↓
Email Service / Other Delivery Channel

Notification delivery remains centralized.

Observability

The Tenant Service must provide appropriate operational visibility.

It should support:

Structured logging.

Metrics.

Health checks.

Error reporting.

Tenant lifecycle monitoring.

Operational alerts.

The Logging Service owns centralized logging.

The Monitoring Service owns centralized monitoring.

The Audit Service owns audit records.

The Analytics Service owns analytical reporting.

These responsibilities must remain separate.

Reliability

Tenant lifecycle operations must be reliable and consistent.

Critical operations should be:

Transactionally safe where applicable.

Idempotent where applicable.

Recoverable.

Auditable.

Observable.

The service must prevent partially completed tenant lifecycle transitions from producing inconsistent platform state.

Scalability

The Tenant Service must support the growth of FluxDine from a small number of tenants to a large multi-tenant platform.

The architecture must support:

Horizontal scaling.

High availability.

Large tenant counts.

Efficient tenant resolution.

Reliable tenant lifecycle processing.

Distributed application deployment.

Scaling strategies must not compromise tenant isolation.

Background Jobs

Background processing may be used for tenant-related operations where appropriate.

Examples may include:

Tenant lifecycle maintenance.

Tenant archival processing.

Tenant configuration synchronization.

Other approved tenant maintenance operations.

Background jobs must execute with explicit tenant context where tenant-scoped work is performed.

They must never process tenant data without establishing the correct tenant boundary.

Administrative Operations

Administrative users may perform elevated tenant operations.

Administrative tenant operations must still be:

Authenticated.

Authorized.

Tenant-aware where applicable.

Audited.

Observable.

Administrative privileges must not bypass architectural service boundaries.

Existing FluxDine Implementation

The existing FluxDine application contains functionality related to restaurant accounts, administration, authentication, configuration, and other tenant-like concepts.

During transformation, the existing implementation must be inspected and mapped against the Tenant Service architecture.

The existing code should be:

Inspected
   ↓
Understood
   ↓
Mapped to Tenant Responsibilities
   ↓
Preserved where compatible
   ↓
Refactored where necessary
   ↓
Validated

Existing implementation must not automatically be assumed to represent the final tenant architecture.

The goal is to evolve useful existing functionality into the approved FluxDine platform architecture rather than unnecessarily discarding working functionality.

Engineering Rules

The Tenant Service is the root service for tenant lifecycle management.

Every tenant must have a clearly defined lifecycle.

Tenant lifecycle state must have a single authoritative owner.

Tenant configuration must have a clearly defined ownership boundary.

Tenant metadata must be owned by the Tenant Service.

Tenant isolation is mandatory.

Tenant authorization must be enforced server-side.

Client-provided tenant identifiers must never independently establish authorization.

Tenant Service must not implement authentication.

Tenant Service must not implement payment processing.

Tenant Service must not implement restaurant operational logic.

Tenant Service must not directly access another service's database.

Other services must not directly modify Tenant Service data.

Tenant domain association belongs to Tenant Service while technical DNS/SSL operations belong to Domain Service.

SaaS subscription lifecycle belongs to Billing Service.

Payment processing belongs to Payment Service.

Notification delivery belongs to Notification Service.

Tenant lifecycle operations must be auditable.

Tenant operations must be idempotent where applicable.

Tenant lifecycle events must use the approved Event Bus.

This document is the authoritative Tenant Service specification.

Architecture Decision Records

The Tenant Service architecture follows these approved principles:

Every business capability belongs to exactly one service.

The Tenant Service is the root service for tenant lifecycle management.

Tenant lifecycle ownership is centralized.

Tenant isolation is mandatory across the platform.

Tenant Service remains independent from authentication.

Tenant Service remains independent from restaurant operational logic.

Tenant Service remains independent from payment processing.

Billing owns SaaS subscription lifecycle.

Payment Service owns payment processing.

Domain Service owns technical domain infrastructure.

Notification Service owns outbound communication.

Tenant events are published through the shared Event Bus.

Tenant data follows the approved database ownership model.

Cross-service database access is prohibited.

Tenant authorization must be enforced server-side.

Quality Attributes

Attribute

Objective

Reliability

Consistent tenant lifecycle management

Availability

High availability of tenant context

Scalability

Support large numbers of tenants

Security

Strong tenant isolation

Performance

Low-latency tenant resolution

Auditability

Complete tenant lifecycle traceability

Extensibility

Support future tenant capabilities

Maintainability

Independent tenant lifecycle evolution

Service Interaction Model

The Tenant Service participates in the platform service architecture:

                    Identity Service
                           │
                           ▼
                    Authorization
                           │
                           ▼
                    Tenant Service
                    │      │      │
                    │      │      └──────► Billing Service
                    │      │
                    │      └─────────────► Domain Service
                    │
                    ▼
              Restaurant Service
                    │
                    ▼
              Commerce Service
                    │
          ┌─────────┼──────────┐
          ▼         ▼          ▼
      Payment   Notification Analytics
       Service     Service     Service

Each service communicates through well-defined interfaces and maintains its own ownership boundaries.

References

Shared Services Overview

Identity Service

Restaurant Service

Billing Service

Payment Service

Domain Service

Feature Flag Service

Audit Service

Database Architecture

Multi-Tenant Architecture

API Standards

Security Architecture

Event Architecture

Implementation Roadmap

Revision History

Version

Date

Author

Description

1.0

Initial Release

FluxDine Platform Architecture Team

Approved as the authoritative Tenant Service specification