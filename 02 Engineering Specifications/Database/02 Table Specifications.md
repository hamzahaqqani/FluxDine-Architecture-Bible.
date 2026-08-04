# 02 Engineering Specifications

# Database

# 02 — Table Specifications

---

# Document Control

| Field | Value |
|--------|-------|
| Document ID | FD-ENG-DB-002 |
| Document Name | Table Specifications |
| Version | 1.0 |
| Status | 🔒 Draft |
| Owner | FluxDine Engineering |
| Classification | Internal |
| Depends On | 00 Database Naming Standards<br>01 Complete Database Schema Specification |
| Referenced By | 03 Relationships<br>04 Constraints<br>05 Index Specification<br>06 Enum Specification<br>07 Database Migration Strategy<br>08 Drizzle ORM Mapping |

---

# Dependencies

This document depends upon the following Engineering Specifications and Architecture documents:

## Core Architecture

- 04 System Architecture Blueprint
- 05 Database Architecture & Multi-Tenant Data Model
- 06 API & Service Architecture

## Engineering Specifications

- 00 Database Naming Standards
- 01 Complete Database Schema Specification

These documents establish the architectural principles, domain boundaries, ownership hierarchy, and naming conventions upon which this specification is built.

---

# Referenced By

The following documents shall reference this specification:

## Database Engineering

- 03 Relationships
- 04 Constraints
- 05 Index Specification
- 06 Enum Specification
- 07 Database Migration Strategy
- 08 Drizzle ORM Mapping

## Backend Engineering

- Service Specifications
- Repository Specifications
- DTO Specifications

## API Engineering

- REST API Specification
- HQ APIs
- Restaurant APIs
- Self-Service APIs
- Shared Service APIs

---

# Document Status

| Section | Status |
|---------|--------|
| Document Foundation | Draft |
| Platform Tables | Pending |
| Identity Tables | Pending |
| Tenant Tables | Pending |
| Restaurant Tables | Pending |
| Commerce Tables | Pending |
| Customer Tables | Pending |
| Reservation Tables | Pending |
| Billing Tables | Pending |
| Payment Tables | Pending |
| Notification Tables | Pending |
| Analytics Tables | Pending |
| Branding Tables | Pending |
| Feature Management Tables | Pending |
| Shared Platform Tables | Pending |
| Engineering Reference Chapters | Pending |
| Appendices | Pending |

---

# Purpose

The **Table Specifications** document is the authoritative engineering specification for every logical database table within the FluxDine platform.

While the **Complete Database Schema Specification** defines business domains, ownership, and relationships, this document defines each database table as an individual engineering artifact.

Every table included within the FluxDine platform shall have a corresponding specification in this document.

This specification provides the foundation for:

- Database implementation
- Drizzle ORM schemas
- Database migrations
- Repository layer development
- Backend services
- API development
- Engineering documentation

---

# Scope

This document specifies:

- Every database table
- Table purpose
- Business domain
- Database schema
- Ownership
- Parent-child hierarchy
- Cross-table references
- Soft delete requirements
- Audit requirements
- Lifecycle expectations
- Growth characteristics
- Implementation guidance

---

# Out of Scope

This document intentionally does **not** define:

- Database columns
- SQL data types
- Foreign key constraints
- Unique constraints
- Check constraints
- Indexes
- Enumerations
- SQL migration scripts
- Drizzle ORM implementation code

These topics are defined in subsequent Engineering Specification documents.

---

# Table Specification Philosophy

Every table within the FluxDine platform represents a business capability.

Tables are **not** created merely to store data—they model business concepts and support clearly defined responsibilities.

Each table shall:

- Represent a single business concept.
- Belong to exactly one primary business domain.
- Have one logical owner.
- Support the multi-tenant architecture where applicable.
- Participate in explicit relationships.
- Follow standardized naming conventions.
- Support auditing when required.
- Be implementation-independent.

The goal of this specification is to describe the logical intent of every table before physical implementation begins.

---

# Engineering Principles

Every table specification follows these principles:

## Single Responsibility

Each table shall represent one business concept.

---

## Explicit Ownership

Every table shall identify its logical owner.

---

## Domain-Driven Organization

Tables shall be grouped according to business domains rather than technical layers.

---

## Multi-Tenant Awareness

Tenant-owned tables shall explicitly support tenant isolation.

---

## Documentation First

Tables shall be documented before implementation.

---

## Implementation Independence

Logical table definitions shall remain independent from PostgreSQL, Drizzle ORM, or any specific database technology.

---

# How to Read This Document

This document is organized by business domain.

Each chapter introduces a domain before defining every table belonging to that domain.

Every table follows an identical specification template, allowing engineers to quickly locate implementation details and understand relationships across the platform.

The recommended reading order is:

```text
Platform

↓

Identity

↓

Tenant

↓

Restaurant

↓

Commerce

↓

Customer

↓

Reservation

↓

Billing

↓

Payment

↓

Notification

↓

Analytics

↓

Branding

↓

Feature Management

↓

Shared Platform
```

This order reflects the ownership hierarchy established within the FluxDine Architecture Bible.

---

# Standard Table Specification Template

Every table within this document shall follow the standardized specification below.

---

## Table Name

Official database table name.

---

## Purpose

Describes the business responsibility of the table.

---

## Business Domain

Identifies the domain to which the table belongs.

---

## Database Schema

Specifies the logical database schema that owns the table.

---

## Owner

Defines the logical owner of the table.

Examples:

- Platform
- Tenant
- Restaurant
- Branch

---

## Parent Entity

Identifies the immediate parent within the ownership hierarchy.

---

## Child Entities

Lists entities directly owned by this table.

---

## References

Lists tables referenced by this table.

---

## Referenced By

Lists tables that reference this table.

---

## Multi-Tenant

Indicates whether the table participates in tenant isolation.

Values:

- Yes
- No

---

## Soft Delete

Indicates whether logical deletion is supported.

Values:

- Yes
- No

---

## Audit Required

Indicates whether modifications shall generate audit records.

Values:

- Yes
- No

---

## Expected Row Growth

Describes anticipated growth.

Typical values:

- Static
- Very Low
- Low
- Medium
- High
- Very High

---

## Expected Cardinality

Provides an estimate of the number of records relative to platform usage.

Examples:

- One per platform
- One per tenant
- Many per tenant
- Millions platform-wide

---

## Lifecycle

Summarizes the lifecycle followed by records within the table.

---

## Future Expansion

Identifies planned architectural extensions.

---

## Implementation Notes

Provides engineering guidance relevant to future implementation.

---

# Table of Contents

## Chapter 1 — Platform Tables

## Chapter 2 — Identity Tables

## Chapter 3 — Tenant Tables

## Chapter 4 — Restaurant Tables

## Chapter 5 — Commerce Tables

## Chapter 6 — Customer Tables

## Chapter 7 — Reservation Tables

## Chapter 8 — Billing Tables

## Chapter 9 — Payment Tables

## Chapter 10 — Notification Tables

## Chapter 11 — Analytics Tables

## Chapter 12 — Branding Tables

## Chapter 13 — Feature Management Tables

## Chapter 14 — Shared Platform Tables

---

## Chapter 15 — Cross-Table Relationships

## Chapter 16 — Ownership Matrix

## Chapter 17 — Table Lifecycle Standards

## Chapter 18 — Table Naming Reference

## Chapter 19 — Engineering Standards

## Chapter 20 — Architecture Decision Records

---

# Chapter 1 — Platform Tables

## 1.1 Purpose

The Platform Domain contains all platform-owned database tables that define global configuration, master reference data, SaaS plans, platform capabilities, and administrative resources shared across every tenant.

Unlike tenant-owned tables, Platform tables are managed exclusively by the FluxDine HQ Platform and are never directly modified by restaurant tenants.

Platform tables form the highest level of the database ownership hierarchy and provide foundational data consumed by every other business domain.

---

## 1.2 Platform Ownership Hierarchy

```text
FluxDine Platform

├── Platform Settings
├── Countries
├── Currencies
├── Languages
├── Timezones
├── Subscription Plans
├── Platform Features
├── Global Roles
├── Global Permissions
├── Email Templates
├── Notification Templates
└── Platform Announcements
```

---

# Platform Table Specifications

---

# countries

## Purpose

Stores the master list of countries supported by the FluxDine platform.

This table serves as reference data for restaurant registration, customer addresses, billing information, localization, and taxation.

---

## Business Domain

Platform

---

## Database Schema

platform

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

None

---

## References

None

---

## Referenced By

- tenants
- restaurants
- customer_addresses
- billing_accounts

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

No

---

## Expected Row Growth

Static

---

## Expected Cardinality

Approximately 250 records.

---

## Lifecycle

Imported

↓

Active

↓

Deprecated (rare)

---

## Future Expansion

- Country-specific taxation
- Regional compliance
- Local payment availability

---

## Implementation Notes

Reference table only.

Never tenant-owned.

---

# currencies

## Purpose

Stores every currency supported by FluxDine.

Provides standardized currency definitions used throughout billing, payments, reporting, and commerce.

---

## Business Domain

Platform

---

## Database Schema

platform

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

None

---

## References

None

---

## Referenced By

- tenants
- billing_accounts
- invoices
- payment_transactions
- restaurants

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

No

---

## Expected Row Growth

Static

---

## Expected Cardinality

Approximately 180 currencies.

---

## Lifecycle

Imported

↓

Active

↓

Deprecated (rare)

---

## Future Expansion

- Exchange rate support
- Regional pricing
- Multi-currency subscriptions

---

## Implementation Notes

ISO-4217 compliant.

---

# languages

## Purpose

Defines every language supported by the platform.

Used for localization, translations, customer experience, notifications, and dashboard preferences.

---

## Business Domain

Platform

---

## Database Schema

platform

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

None

---

## References

None

---

## Referenced By

- tenants
- users
- customers
- notification_templates

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

No

---

## Expected Row Growth

Very Low

---

## Expected Cardinality

Less than 100 languages.

---

## Lifecycle

Created

↓

Active

↓

Deprecated

---

## Future Expansion

- Automatic translations
- AI localization

---

## Implementation Notes

ISO language codes shall be used.

---

# timezones

## Purpose

Stores all supported timezones used throughout the platform.

Provides consistent scheduling, reservation timing, reporting, and localization.

---

## Business Domain

Platform

---

## Database Schema

platform

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

None

---

## References

None

---

## Referenced By

- tenants
- restaurants
- branches
- users

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

No

---

## Expected Row Growth

Static

---

## Expected Cardinality

Approximately 600 timezone definitions.

---

## Lifecycle

Imported

↓

Active

---

## Future Expansion

- Daylight-saving policies
- Regional calendars

---

## Implementation Notes

Use IANA timezone identifiers.

---

# subscription_plans

## Purpose

Defines every subscription plan offered by FluxDine.

Plans determine platform capabilities, commercial offerings, and feature entitlements.

---

## Business Domain

Platform

---

## Database Schema

platform

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

- subscription_features

---

## References

None

---

## Referenced By

- subscriptions
- tenants
- billing_accounts

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Very Low

---

## Expected Cardinality

Less than 20 plans.

---

## Lifecycle

Draft

↓

Published

↓

Active

↓

Deprecated

↓

Retired

---

## Future Expansion

- Regional pricing
- Enterprise contracts
- Promotional plans

---

## Implementation Notes

Plan pricing is managed separately from feature definitions.

---

# platform_features

## Purpose

Maintains the master catalog of every feature available within the FluxDine platform.

Features are referenced by subscriptions and tenant feature overrides.

---

## Business Domain

Platform

---

## Database Schema

platform

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

- subscription_features
- tenant_features

---

## References

None

---

## Referenced By

- feature_flags
- subscriptions
- tenant_features

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

100–500 features.

---

## Lifecycle

Draft

↓

Published

↓

Active

↓

Deprecated

---

## Future Expansion

- Feature dependencies
- AI capabilities
- Experimental modules

---

## Implementation Notes

This is the authoritative feature catalog for the entire platform.

---

# global_roles

## Purpose

Defines platform-wide role templates used by FluxDine HQ.

These roles govern administrative access to the HQ Platform and serve as templates for role management.

---

## Business Domain

Platform

---

## Database Schema

platform

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

None

---

## References

- global_permissions

---

## Referenced By

- user_roles

---

## Multi-Tenant

No

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Very Low

---

## Expected Cardinality

Less than 50 roles.

---

## Lifecycle

Draft

↓

Active

↓

Deprecated

↓

Archived

---

## Future Expansion

- Regional HQ roles
- Support teams
- AI administration

---

## Implementation Notes

Restaurant-specific operational roles are defined separately within the Identity Domain.

---

# global_permissions

## Purpose

Stores the master permission catalog used throughout the platform.

Permissions represent the smallest unit of authorization.

---

## Business Domain

Platform

---

## Database Schema

platform

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

None

---

## References

None

---

## Referenced By

- global_roles
- role_permissions

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

500–2,000 permissions.

---

## Lifecycle

Draft

↓

Active

↓

Deprecated

---

## Future Expansion

- Dynamic permissions
- AI-generated permission groups

---

## Implementation Notes

Permissions shall follow the naming conventions defined in **00 Database Naming Standards**.

---

# email_templates

## Purpose

Stores reusable email templates used throughout the platform.

Templates are consumed by the Notification Service for transactional and system-generated emails.

---

## Business Domain

Platform

---

## Database Schema

platform

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

None

---

## References

None

---

## Referenced By

- notifications
- email_messages

---

## Multi-Tenant

No

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

100–500 templates.

---

## Lifecycle

Draft

↓

Published

↓

Active

↓

Archived

---

## Future Expansion

- Multi-language templates
- A/B testing
- AI-assisted personalization

---

## Implementation Notes

Templates contain presentation only. Delivery metadata belongs to the Notification Domain.

---

# notification_templates

## Purpose

Stores reusable notification templates for SMS, Push, and in-app notifications.

---

## Business Domain

Platform

---

## Database Schema

platform

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

None

---

## References

None

---

## Referenced By

- notifications
- sms_messages
- push_notifications

---

## Multi-Tenant

No

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

100–500 templates.

---

## Lifecycle

Draft

↓

Published

↓

Active

↓

Archived

---

## Future Expansion

- Channel-specific personalization
- Localization
- AI-generated content

---

## Implementation Notes

Shared across all notification channels.

---

# platform_settings

## Purpose

Stores global platform configuration controlling system-wide behavior.

---

## Business Domain

Platform

---

## Database Schema

platform

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

None

---

## References

None

---

## Referenced By

Every application within the FluxDine ecosystem.

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Static

---

## Expected Cardinality

One logical configuration set.

---

## Lifecycle

Created

↓

Active

↓

Updated

---

## Future Expansion

- Feature rollout policies
- Global maintenance settings
- Platform defaults

---

## Implementation Notes

Changes to this table affect the entire platform and must be tightly controlled.

---

# platform_announcements

## Purpose

Stores announcements published by FluxDine HQ for tenants and platform users.

---

## Business Domain

Platform

---

## Database Schema

platform

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

None

---

## References

None

---

## Referenced By

- HQ Dashboard
- Restaurant Dashboard
- Self-Service Platform

---

## Multi-Tenant

No

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

Hundreds to thousands over the platform lifetime.

---

## Lifecycle

Draft

↓

Scheduled

↓

Published

↓

Expired

↓

Archived

---

## Future Expansion

- Audience targeting
- Regional announcements
- Scheduled publishing
- Rich media support

---

## Implementation Notes

Announcements may target all tenants or filtered tenant groups while remaining platform-owned.

---

# Chapter 1 Summary

The **Platform Domain** defines the global reference tables and administrative resources shared across the entire FluxDine ecosystem. These tables are exclusively managed by the FluxDine HQ Platform, remain independent of tenant ownership, and provide foundational data consumed by every other business domain. Together, they establish the platform-wide configuration, localization, subscription management, authorization, and communication infrastructure upon which the multi-tenant SaaS platform is built.

# Chapter 2 — Identity Tables

## 2.1 Purpose

The Identity Domain provides the centralized authentication and authorization infrastructure for the entire FluxDine platform.

Unlike tenant-owned business data, Identity tables maintain a unified identity model in which every person exists exactly once within the platform while allowing that individual to participate in multiple tenants through memberships and role assignments.

Identity tables are shared platform resources and implement the **Unified Identity System** defined in the Core Architecture.

---

## 2.2 Identity Ownership Hierarchy

```text
Identity

├── Users
├── User Memberships
├── Roles
├── Permissions
├── Role Permissions
├── User Roles
├── Sessions
├── Authentication Providers
├── MFA Configurations
├── Password Reset Tokens
└── Email Verification Tokens
```

---

# Identity Table Specifications

---

# users

## Purpose

Represents every authenticated individual within the FluxDine ecosystem.

A single user may simultaneously be:

- HQ Administrator
- Restaurant Owner
- Branch Administrator
- Staff Member
- Rider
- Customer
- Support Engineer

Every person exists exactly once within the platform.

---

## Business Domain

Identity

---

## Database Schema

identity

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

- user_memberships
- user_roles
- sessions
- mfa_configurations
- authentication_providers
- password_reset_tokens
- email_verification_tokens

---

## References

None

---

## Referenced By

- customers
- staff
- riders
- audit_logs
- activity_logs

---

## Multi-Tenant

No

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Very High

---

## Expected Cardinality

Millions platform-wide.

---

## Lifecycle

```text
Registered

↓

Verified

↓

Active

↓

Suspended

↓

Archived

↓

Soft Deleted
```

---

## Future Expansion

- Social authentication
- Enterprise SSO
- Passwordless authentication
- Biometric authentication

---

## Implementation Notes

The Users table is the root identity table for the entire platform and shall never contain duplicate persons.

---

# user_memberships

## Purpose

Associates platform users with tenants.

A user may belong to multiple tenants while maintaining a single platform identity.

---

## Business Domain

Identity

---

## Database Schema

identity

---

## Owner

Platform

---

## Parent Entity

users

---

## Child Entities

- user_roles

---

## References

- users
- tenants

---

## Referenced By

- authorization services

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

High

---

## Expected Cardinality

Many memberships per user.

---

## Lifecycle

```text
Invited

↓

Accepted

↓

Active

↓

Suspended

↓

Removed
```

---

## Future Expansion

- Multi-organization memberships
- Temporary memberships
- Franchise memberships

---

## Implementation Notes

Membership defines tenant participation—not permissions.

---

# roles

## Purpose

Defines reusable authorization roles.

Roles group permissions into meaningful operational responsibilities.

---

## Business Domain

Identity

---

## Database Schema

identity

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

- role_permissions
- user_roles

---

## References

None

---

## Referenced By

- user_roles

---

## Multi-Tenant

No

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

50–200 roles.

---

## Lifecycle

```text
Draft

↓

Published

↓

Active

↓

Deprecated

↓

Archived
```

---

## Future Expansion

- Custom tenant roles
- Dynamic roles
- AI-generated role templates

---

## Implementation Notes

Role definitions remain independent of user assignments.

---

# permissions

## Purpose

Defines the smallest unit of authorization within the platform.

Permissions determine exactly what operations a user may perform.

---

## Business Domain

Identity

---

## Database Schema

identity

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

None

---

## References

None

---

## Referenced By

- role_permissions

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

500–2,000 permissions.

---

## Lifecycle

```text
Draft

↓

Active

↓

Deprecated
```

---

## Future Expansion

- Attribute-based permissions
- Context-aware permissions

---

## Implementation Notes

Permissions follow the naming standards defined in **00 Database Naming Standards**.

---

# role_permissions

## Purpose

Maps permissions to authorization roles.

Implements the many-to-many relationship between Roles and Permissions.

---

## Business Domain

Identity

---

## Database Schema

identity

---

## Owner

Platform

---

## Parent Entity

roles

---

## Child Entities

None

---

## References

- roles
- permissions

---

## Referenced By

Authorization Engine

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Thousands of mappings.

---

## Lifecycle

Created

↓

Active

↓

Removed

---

## Future Expansion

- Conditional permissions
- Permission inheritance

---

## Implementation Notes

Pure relationship table.

---

# user_roles

## Purpose

Assigns authorization roles to users within a tenant.

Implements the many-to-many relationship between Users and Roles.

---

## Business Domain

Identity

---

## Database Schema

identity

---

## Owner

Platform

---

## Parent Entity

user_memberships

---

## Child Entities

None

---

## References

- users
- roles
- user_memberships

---

## Referenced By

Authorization Services

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

High

---

## Expected Cardinality

Many role assignments per tenant.

---

## Lifecycle

```text
Assigned

↓

Active

↓

Revoked
```

---

## Future Expansion

- Time-limited roles
- Emergency access
- Delegated administration

---

## Implementation Notes

Role assignments are tenant-scoped.

---

# sessions

## Purpose

Stores authenticated login sessions.

Provides session tracking, security monitoring, and logout management.

---

## Business Domain

Identity

---

## Database Schema

identity

---

## Owner

Platform

---

## Parent Entity

users

---

## Child Entities

None

---

## References

- users

---

## Referenced By

Authentication Services

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Very High

---

## Expected Cardinality

Millions of active and historical sessions.

---

## Lifecycle

```text
Created

↓

Active

↓

Expired

↓

Revoked
```

---

## Future Expansion

- Device trust
- Session analytics
- Risk scoring

---

## Implementation Notes

Session expiration policies are configurable.

---

# authentication_providers

## Purpose

Stores authentication methods available to individual users.

Supports multiple authentication providers.

---

## Business Domain

Identity

---

## Database Schema

identity

---

## Owner

Platform

---

## Parent Entity

users

---

## Child Entities

None

---

## References

- users

---

## Referenced By

Authentication Services

---

## Multi-Tenant

No

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Several providers per user.

---

## Lifecycle

```text
Connected

↓

Active

↓

Disconnected
```

---

## Future Expansion

Supported providers include:

- Google
- Microsoft
- Apple
- GitHub
- SAML
- OpenID Connect

---

## Implementation Notes

Credentials are never stored directly.

---

# mfa_configurations

## Purpose

Stores Multi-Factor Authentication configuration for platform users.

---

## Business Domain

Identity

---

## Database Schema

identity

---

## Owner

Platform

---

## Parent Entity

users

---

## Child Entities

None

---

## References

- users

---

## Referenced By

Authentication Services

---

## Multi-Tenant

No

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

One or more MFA methods per user.

---

## Lifecycle

```text
Configured

↓

Verified

↓

Enabled

↓

Disabled
```

---

## Future Expansion

- TOTP
- SMS
- Email
- Hardware Keys
- Passkeys

---

## Implementation Notes

Recovery information shall be securely managed.

---

# password_reset_tokens

## Purpose

Stores temporary password reset requests.

---

## Business Domain

Identity

---

## Database Schema

identity

---

## Owner

Platform

---

## Parent Entity

users

---

## Child Entities

None

---

## References

- users

---

## Referenced By

Password Recovery Services

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Short-lived records.

---

## Lifecycle

```text
Created

↓

Used

↓

Expired
```

---

## Future Expansion

- Recovery workflows
- Security verification

---

## Implementation Notes

Tokens shall have strict expiration policies.

---

# email_verification_tokens

## Purpose

Stores email verification requests during account registration and email change workflows.

---

## Business Domain

Identity

---

## Database Schema

identity

---

## Owner

Platform

---

## Parent Entity

users

---

## Child Entities

None

---

## References

- users

---

## Referenced By

Registration Services

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Short-lived records.

---

## Lifecycle

```text
Created

↓

Verified

↓

Expired
```

---

## Future Expansion

- Multi-email verification
- Organization invitations

---

## Implementation Notes

Verification tokens shall be single-use and automatically expire.

---

# Chapter 2 Summary

The **Identity Domain** establishes the unified identity and authorization foundation for the FluxDine platform. These tables ensure that every authenticated individual exists only once while supporting multi-tenant memberships, flexible role-based access control, secure authentication, and extensible identity management. Together, they provide the security backbone consumed by every application and service across the FluxDine ecosystem.

# Chapter 3 — Tenant Tables

## 3.1 Purpose

The Tenant Domain represents the core of the FluxDine SaaS architecture.

Every restaurant business that subscribes to FluxDine becomes a **Tenant**. The Tenant Domain manages tenant onboarding, configuration, subscription assignment, branding, integrations, payment gateway configuration, feature entitlements, API access, and tenant-specific operational settings.

All tenant-owned business data originates from this domain and ultimately traces its ownership back to a single Tenant.

---

## 3.2 Tenant Ownership Hierarchy

```text
Tenant

├── Tenant Settings
├── Tenant Domains
├── Tenant Branding
├── Tenant Features
├── Tenant Payment Gateways
├── Tenant Integrations
├── Tenant Subscription
├── Tenant Usage Metrics
├── Tenant API Keys
├── Tenant Audit Settings
└── Tenant Invitations
```

---

# Tenant Table Specifications

---

# tenants

## Purpose

Represents every restaurant organization subscribed to the FluxDine platform.

The Tenant table is the root entity for all tenant-owned business data and establishes the security, ownership, and isolation boundary for the platform.

---

## Business Domain

Tenant

---

## Database Schema

tenant

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

- tenant_settings
- tenant_domains
- tenant_branding
- tenant_features
- tenant_payment_gateways
- tenant_integrations
- tenant_subscriptions
- tenant_usage_metrics
- tenant_api_keys
- tenant_audit_settings
- tenant_invitations
- restaurants

---

## References

- subscription_plans

---

## Referenced By

- user_memberships
- restaurants
- customers
- orders
- reservations
- billing_accounts
- payment_transactions
- audit_logs

---

## Multi-Tenant

No

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Thousands to hundreds of thousands of tenants.

---

## Lifecycle

```text
Registered

↓

Verified

↓

Provisioned

↓

Trial

↓

Active

↓

Suspended

↓

Cancelled

↓

Archived

↓

Soft Deleted
```

---

## Future Expansion

- Franchise Organizations
- Parent Organizations
- Multi-Brand Groups
- White Label Organizations

---

## Implementation Notes

The Tenant table is the root aggregate for every restaurant customer within FluxDine. All tenant-owned records must ultimately reference this table either directly or indirectly.

---

# tenant_settings

## Purpose

Stores operational settings and preferences specific to a tenant.

---

## Business Domain

Tenant

---

## Database Schema

tenant

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

None

---

## References

- tenants
- languages
- currencies
- timezones

---

## Referenced By

Restaurant Platform

Self-Service Platform

Shared Services

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Static

---

## Expected Cardinality

One record per tenant.

---

## Lifecycle

```text
Created

↓

Active

↓

Updated
```

---

## Future Expansion

- AI preferences
- Regional configuration
- Operational defaults
- Notification defaults

---

## Implementation Notes

Stores configurable business settings without affecting platform-wide defaults.

---

# tenant_domains

## Purpose

Stores every internet domain associated with a tenant.

Supports custom domains and FluxDine-managed subdomains.

---

## Business Domain

Tenant

---

## Database Schema

tenant

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

None

---

## References

- tenants

---

## Referenced By

Branding

SEO

SSL Services

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

One or more domains per tenant.

---

## Lifecycle

```text
Added

↓

Verification Pending

↓

Verified

↓

Active

↓

Expired

↓

Removed
```

---

## Future Expansion

- Multi-domain routing
- Domain aliases
- International domains

---

## Implementation Notes

Each domain shall belong to exactly one tenant.

---

# tenant_branding

## Purpose

Stores tenant-specific branding configuration.

---

## Business Domain

Tenant

---

## Database Schema

tenant

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

- themes
- logos
- brand_assets

---

## References

- tenants

---

## Referenced By

Restaurant Platform

Customer Experience

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Static

---

## Expected Cardinality

One branding configuration per tenant.

---

## Lifecycle

```text
Created

↓

Active

↓

Updated
```

---

## Future Expansion

- Brand versions
- Seasonal branding
- Brand packages

---

## Implementation Notes

Contains branding configuration only. Asset storage is handled by the Branding Domain.

---

# tenant_features

## Purpose

Stores tenant-specific feature overrides.

Overrides supplement the subscription-defined feature availability.

---

## Business Domain

Tenant

---

## Database Schema

tenant

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

None

---

## References

- tenants
- platform_features

---

## Referenced By

Feature Management Services

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Multiple feature assignments per tenant.

---

## Lifecycle

```text
Enabled

↓

Modified

↓

Disabled
```

---

## Future Expansion

- Time-based activation
- Beta enrollment
- Promotional features

---

## Implementation Notes

Only stores overrides. Master feature definitions remain platform-owned.

---

# tenant_payment_gateways

## Purpose

Stores payment gateway configurations owned by a tenant.

---

## Business Domain

Tenant

---

## Database Schema

tenant

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

None

---

## References

- tenants
- payment_providers

---

## Referenced By

Payment Service

Commerce

Billing

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

Multiple gateways per tenant.

---

## Lifecycle

```text
Configured

↓

Verified

↓

Enabled

↓

Disabled
```

---

## Future Expansion

- Multi-provider routing
- Failover providers
- Regional gateways

---

## Implementation Notes

Sensitive credentials shall be encrypted and managed securely.

---

# tenant_integrations

## Purpose

Stores third-party integrations configured by tenants.

---

## Business Domain

Tenant

---

## Database Schema

tenant

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

None

---

## References

- tenants

---

## Referenced By

Shared Platform Services

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Several integrations per tenant.

---

## Lifecycle

```text
Configured

↓

Connected

↓

Active

↓

Disconnected
```

---

## Future Expansion

Supported integrations include:

- Google Analytics
- Meta Pixel
- Google Maps
- Twilio
- Mailgun
- WhatsApp Business
- Zapier

---

## Implementation Notes

Integration credentials shall never be stored in plain text.

---

# tenant_subscriptions

## Purpose

Represents the subscription assigned to a tenant.

Maintains the relationship between the tenant and the Platform Billing system.

---

## Business Domain

Tenant

---

## Database Schema

tenant

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

None

---

## References

- tenants
- subscriptions

---

## Referenced By

Billing Services

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Very Low

---

## Expected Cardinality

One active subscription per tenant.

---

## Lifecycle

```text
Trial

↓

Active

↓

Past Due

↓

Suspended

↓

Cancelled

↓

Expired
```

---

## Future Expansion

- Multiple concurrent subscriptions
- Add-on subscriptions
- Marketplace subscriptions

---

## Implementation Notes

This table references the Billing Domain and should not duplicate billing information.

---

# tenant_usage_metrics

## Purpose

Stores tenant resource consumption and usage statistics.

---

## Business Domain

Tenant

---

## Database Schema

tenant

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

None

---

## References

- tenants

---

## Referenced By

Analytics

Billing

HQ Platform

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

No

---

## Expected Row Growth

High

---

## Expected Cardinality

Continuous growth throughout tenant lifetime.

---

## Lifecycle

```text
Generated

↓

Updated

↓

Archived
```

---

## Future Expansion

- AI usage
- API usage
- Storage usage
- Queue usage

---

## Implementation Notes

Optimized for reporting rather than transactional operations.

---

# tenant_api_keys

## Purpose

Stores API credentials issued to tenants.

---

## Business Domain

Tenant

---

## Database Schema

tenant

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

None

---

## References

- tenants

---

## Referenced By

API Gateway

Integration Services

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

Several API keys per tenant.

---

## Lifecycle

```text
Issued

↓

Active

↓

Rotated

↓

Revoked
```

---

## Future Expansion

- Scoped API keys
- Read-only keys
- Temporary keys

---

## Implementation Notes

Secret values shall be stored securely and never exposed after creation.

---

# tenant_audit_settings

## Purpose

Defines tenant-specific audit and data retention policies.

---

## Business Domain

Tenant

---

## Database Schema

tenant

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

None

---

## References

- tenants

---

## Referenced By

Audit Service

Compliance Services

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Static

---

## Expected Cardinality

One record per tenant.

---

## Lifecycle

```text
Configured

↓

Active

↓

Updated
```

---

## Future Expansion

- Custom retention periods
- Compliance profiles
- Regional data policies

---

## Implementation Notes

Policies must remain compatible with platform-wide compliance requirements.

---

# tenant_invitations

## Purpose

Stores invitations sent to prospective tenant users before account activation.

---

## Business Domain

Tenant

---

## Database Schema

tenant

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

None

---

## References

- tenants
- users (optional after acceptance)

---

## Referenced By

Onboarding Services

Identity Services

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Multiple invitations per tenant.

---

## Lifecycle

```text
Created

↓

Pending

↓

Accepted

↓

Expired

↓

Cancelled
```

---

## Future Expansion

- Bulk invitations
- Role-based invitations
- Scheduled invitations

---

## Implementation Notes

Invitation records provide a complete onboarding history and should be retained for auditing purposes.

---

# Chapter 3 Summary

The **Tenant Domain** establishes the ownership boundary for every restaurant organization within FluxDine. These tables define tenant identity, operational configuration, branding, domains, integrations, payment gateway configuration, feature entitlements, API access, subscriptions, and governance. Together they form the central control layer from which all tenant-owned business resources inherit their ownership and security context, making the Tenant Domain the cornerstone of FluxDine's multi-tenant SaaS architecture.

# Chapter 4 — Restaurant Tables

## 4.1 Purpose

The Restaurant Domain models the operational structure of every restaurant onboarded to the FluxDine platform.

While the Tenant Domain represents the restaurant business as a SaaS customer, the Restaurant Domain models the day-to-day operations of that business. It defines restaurants, branches, operational schedules, delivery coverage, physical tables, staff, riders, and restaurant-specific configuration.

Every operational entity ultimately belongs to a Tenant through the Restaurant hierarchy.

---

## 4.2 Restaurant Ownership Hierarchy

```text
Tenant
│
└── Restaurant
      │
      ├── Branches
      │      ├── Business Hours
      │      ├── Delivery Zones
      │      ├── Seating Areas
      │      ├── Restaurant Tables
      │      ├── Kitchen Stations
      │      ├── Staff
      │      ├── Riders
      │      └── Rider Assignments
      │
      └── Restaurant Settings
```

---

# Restaurant Table Specifications

---

# restaurants

## Purpose

Represents the primary restaurant operated by a tenant.

A restaurant acts as the operational root for menus, branches, customers, orders, reservations, staff, and reports.

---

## Business Domain

Restaurant

---

## Database Schema

restaurant

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

- branches
- restaurant_settings
- menus
- customers
- orders
- reservations
- reports

---

## References

- tenants

---

## Referenced By

- branches
- menus
- customers
- orders
- reservations
- analytics

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

One restaurant per tenant.

---

## Lifecycle

```text
Created

↓

Configured

↓

Active

↓

Suspended

↓

Archived

↓

Soft Deleted
```

---

## Future Expansion

- Restaurant Groups
- Franchise Networks
- Shared Brands

---

## Implementation Notes

Every tenant owns exactly one logical restaurant. Multi-location businesses are modeled through Branches.

---

# branches

## Purpose

Represents physical operating locations belonging to a restaurant.

Each branch operates independently while remaining under the ownership of a single restaurant.

---

## Business Domain

Restaurant

---

## Database Schema

restaurant

---

## Owner

Restaurant

---

## Parent Entity

restaurants

---

## Child Entities

- business_hours
- delivery_zones
- seating_areas
- restaurant_tables
- kitchen_stations
- staff
- riders
- rider_assignments

---

## References

- restaurants

---

## Referenced By

- orders
- reservations
- customers
- menus

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

One to hundreds of branches per restaurant.

---

## Lifecycle

```text
Created

↓

Configured

↓

Operational

↓

Closed

↓

Archived
```

---

## Future Expansion

- Regional Branches
- Franchise Branches
- Dark Kitchens
- Pickup Locations

---

## Implementation Notes

Branch-level settings override restaurant defaults where applicable.

---

# business_hours

## Purpose

Defines the operating schedule for each branch.

---

## Business Domain

Restaurant

---

## Database Schema

restaurant

---

## Owner

Branch

---

## Parent Entity

branches

---

## Child Entities

None

---

## References

- branches

---

## Referenced By

- reservations
- orders
- customer_portal

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

Multiple schedules per branch.

---

## Lifecycle

```text
Configured

↓

Active

↓

Updated
```

---

## Future Expansion

- Holiday Hours
- Seasonal Hours
- Special Events
- Emergency Closures

---

## Implementation Notes

Supports recurring weekly schedules and date-specific overrides.

---

# delivery_zones

## Purpose

Defines delivery service areas for each branch.

---

## Business Domain

Restaurant

---

## Database Schema

restaurant

---

## Owner

Branch

---

## Parent Entity

branches

---

## Child Entities

None

---

## References

- branches

---

## Referenced By

- orders
- checkout
- delivery_pricing

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Several delivery zones per branch.

---

## Lifecycle

```text
Created

↓

Active

↓

Modified

↓

Archived
```

---

## Future Expansion

- Polygon Zones
- Dynamic Pricing
- Driver Coverage
- Zone Priorities

---

## Implementation Notes

Supports future GIS integration.

---

# seating_areas

## Purpose

Represents logical dining areas within a branch.

---

## Business Domain

Restaurant

---

## Database Schema

restaurant

---

## Owner

Branch

---

## Parent Entity

branches

---

## Child Entities

- restaurant_tables

---

## References

- branches

---

## Referenced By

- reservations

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

Several seating areas per branch.

---

## Lifecycle

```text
Created

↓

Active

↓

Archived
```

---

## Future Expansion

- Floor Maps
- Outdoor Areas
- VIP Rooms
- Rooftop Seating

---

## Implementation Notes

Supports visual floor planning in future releases.

---

# restaurant_tables

## Purpose

Represents physical dining tables within a branch.

---

## Business Domain

Restaurant

---

## Database Schema

restaurant

---

## Owner

Branch

---

## Parent Entity

seating_areas

---

## Child Entities

None

---

## References

- seating_areas
- branches

---

## Referenced By

- reservations
- qr_ordering

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Dozens to hundreds of tables per branch.

---

## Lifecycle

```text
Created

↓

Available

↓

Unavailable

↓

Archived
```

---

## Future Expansion

- QR Ordering
- Smart Tables
- Table Sensors

---

## Implementation Notes

Designed to support dine-in ordering and reservation assignment.

---

# kitchen_stations

## Purpose

Represents operational kitchen workstations.

---

## Business Domain

Restaurant

---

## Database Schema

restaurant

---

## Owner

Branch

---

## Parent Entity

branches

---

## Child Entities

None

---

## References

- branches

---

## Referenced By

- kitchen_display_system
- order_preparation

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

Several stations per branch.

---

## Lifecycle

```text
Created

↓

Operational

↓

Inactive

↓

Archived
```

---

## Future Expansion

- Kitchen Routing
- Preparation Queues
- Smart Displays

---

## Implementation Notes

Supports future Kitchen Display System (KDS) integration.

---

# staff

## Purpose

Represents restaurant employees operating within a branch.

Staff identities reference the centralized Users table.

---

## Business Domain

Restaurant

---

## Database Schema

restaurant

---

## Owner

Branch

---

## Parent Entity

branches

---

## Child Entities

None

---

## References

- branches
- users

---

## Referenced By

- orders
- reservations
- audit_logs

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Dozens to hundreds of staff members per restaurant.

---

## Lifecycle

```text
Invited

↓

Active

↓

Suspended

↓

Terminated

↓

Archived
```

---

## Future Expansion

- Shift Scheduling
- Payroll Integration
- Performance Reviews

---

## Implementation Notes

Business information is stored here; authentication remains within the Identity Domain.

---

# riders

## Purpose

Represents delivery personnel assigned to restaurant branches.

---

## Business Domain

Restaurant

---

## Database Schema

restaurant

---

## Owner

Branch

---

## Parent Entity

branches

---

## Child Entities

- rider_assignments

---

## References

- branches
- users

---

## Referenced By

- deliveries
- orders

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Several riders per branch.

---

## Lifecycle

```text
Registered

↓

Available

↓

Busy

↓

Offline

↓

Archived
```

---

## Future Expansion

- Fleet Management
- GPS Tracking
- Route Optimization

---

## Implementation Notes

Authentication is managed through the Identity Domain.

---

# rider_assignments

## Purpose

Tracks rider assignments to branches and deliveries.

Provides historical assignment records.

---

## Business Domain

Restaurant

---

## Database Schema

restaurant

---

## Owner

Branch

---

## Parent Entity

riders

---

## Child Entities

None

---

## References

- riders
- branches

---

## Referenced By

Delivery Management

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

High

---

## Expected Cardinality

Thousands of assignments over time.

---

## Lifecycle

```text
Assigned

↓

Accepted

↓

Completed

↓

Closed
```

---

## Future Expansion

- Multi-Branch Assignments
- Shift-Based Assignments
- AI Dispatch

---

## Implementation Notes

Maintains complete rider assignment history.

---

# restaurant_settings

## Purpose

Stores restaurant-wide operational configuration.

---

## Business Domain

Restaurant

---

## Database Schema

restaurant

---

## Owner

Restaurant

---

## Parent Entity

restaurants

---

## Child Entities

None

---

## References

- restaurants

---

## Referenced By

Restaurant Platform

Customer Experience

Order Processing

Reservation Services

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Static

---

## Expected Cardinality

One record per restaurant.

---

## Lifecycle

```text
Created

↓

Configured

↓

Updated
```

---

## Future Expansion

- Operational Policies
- Reservation Defaults
- Delivery Defaults
- AI Configuration

---

## Implementation Notes

Contains restaurant-specific operational settings. Branch-level settings may override these defaults.

---

# Chapter 4 Summary

The **Restaurant Domain** models the operational structure of every restaurant within the FluxDine platform. These tables define restaurants, branches, schedules, delivery coverage, dining infrastructure, staff, riders, and operational configuration. Together they establish the business hierarchy that supports order processing, reservations, delivery management, and day-to-day restaurant operations while maintaining strict tenant ownership and isolation.

# Chapter 5 — Commerce & Customer Tables

## 5.1 Purpose

The Commerce and Customer Domains together power the customer-facing business operations of the FluxDine platform.

The Commerce Domain manages menus, ordering, pricing, checkout, and sales transactions, while the Customer Domain manages customer profiles, addresses, loyalty, preferences, and engagement.

Together these domains represent the operational heart of the Restaurant Platform and Customer Experience application.

---

## 5.2 Commerce Ownership Hierarchy

```text
Restaurant
│
├── Menus
│     ├── Menu Categories
│     │      ├── Menu Items
│     │      │      ├── Item Variants
│     │      │      ├── Item Options
│     │      │      └── Item Add-ons
│
├── Customers
│     ├── Addresses
│     ├── Preferences
│     ├── Favorites
│     ├── Loyalty Accounts
│     └── Reward Points
│
├── Carts
│     ├── Cart Items
│
└── Orders
      ├── Order Items
      ├── Coupons
      ├── Discounts
      ├── Taxes
      └── Fees
```

---

# Commerce Table Specifications

---

# menus

## Purpose

Represents the primary customer-facing menu offered by a restaurant.

Menus organize products into categories and define item availability for one or more branches.

---

## Business Domain

Commerce

---

## Database Schema

commerce

---

## Owner

Restaurant

---

## Parent Entity

restaurants

---

## Child Entities

- menu_categories

---

## References

- restaurants

---

## Referenced By

- menu_categories
- branches

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

Multiple menus per restaurant.

---

## Lifecycle

```text
Draft

↓

Published

↓

Active

↓

Archived
```

---

## Future Expansion

- Seasonal Menus
- Time-Based Menus
- Delivery Menus
- QR Menus

---

## Implementation Notes

Supports multiple menus assigned to different branches or service channels.

---

# menu_categories

## Purpose

Groups menu items into logical customer-facing categories.

---

## Business Domain

Commerce

---

## Database Schema

commerce

---

## Owner

Menu

---

## Parent Entity

menus

---

## Child Entities

- menu_items

---

## References

- menus

---

## Referenced By

- menu_items

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Dozens of categories per menu.

---

## Lifecycle

```text
Created

↓

Published

↓

Archived
```

---

## Future Expansion

- Nested Categories
- Promotional Categories
- AI Categories

---

## Implementation Notes

Categories define customer navigation only.

---

# menu_items

## Purpose

Represents individual products available for ordering.

---

## Business Domain

Commerce

---

## Database Schema

commerce

---

## Owner

Category

---

## Parent Entity

menu_categories

---

## Child Entities

- item_variants
- item_options
- item_addons

---

## References

- menu_categories

---

## Referenced By

- cart_items
- order_items
- favorites

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

High

---

## Expected Cardinality

Hundreds to thousands of products.

---

## Lifecycle

```text
Draft

↓

Published

↓

Available

↓

Unavailable

↓

Archived
```

---

## Future Expansion

- AI Recommendations
- Nutritional Data
- Inventory Integration

---

## Implementation Notes

Historical orders shall maintain product snapshots independently of future menu updates.

---

# item_variants

## Purpose

Represents purchasable product variations.

Examples:

- Small
- Medium
- Large

---

## Business Domain

Commerce

---

## Database Schema

commerce

---

## Owner

Menu Item

---

## Parent Entity

menu_items

---

## Child Entities

None

---

## References

- menu_items

---

## Referenced By

- cart_items
- order_items

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Multiple variants per item.

---

## Lifecycle

```text
Created

↓

Available

↓

Archived
```

---

## Future Expansion

- Dynamic Pricing
- Regional Variants

---

## Implementation Notes

Variants inherit the parent menu item.

---

# item_options

## Purpose

Defines configurable product options.

Examples:

- Spice Level
- Cooking Preference
- Drink Choice

---

## Business Domain

Commerce

---

## Database Schema

commerce

---

## Owner

Menu Item

---

## Parent Entity

menu_items

---

## Child Entities

None

---

## References

- menu_items

---

## Referenced By

- cart_items
- order_items

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Several options per product.

---

## Lifecycle

```text
Created

↓

Available

↓

Archived
```

---

## Future Expansion

- Nested Options
- Conditional Options

---

## Implementation Notes

Options represent customer choices that do not necessarily affect price.

---

# item_addons

## Purpose

Defines optional purchasable add-ons.

Examples:

- Extra Cheese
- Extra Sauce
- Drinks

---

## Business Domain

Commerce

---

## Database Schema

commerce

---

## Owner

Menu Item

---

## Parent Entity

menu_items

---

## Child Entities

None

---

## References

- menu_items

---

## Referenced By

- cart_items
- order_items

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

High

---

## Expected Cardinality

Many add-ons per product.

---

## Lifecycle

```text
Created

↓

Available

↓

Archived
```

---

## Future Expansion

- Bundle Offers
- Combo Builder

---

## Implementation Notes

Supports multiple add-ons per menu item.

---

# carts

## Purpose

Represents a customer's active shopping cart.

---

## Business Domain

Commerce

---

## Database Schema

commerce

---

## Owner

Customer

---

## Parent Entity

customers

---

## Child Entities

- cart_items

---

## References

- customers
- restaurants
- branches

---

## Referenced By

- checkout_sessions

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

No

---

## Expected Row Growth

Very High

---

## Expected Cardinality

One active cart per customer per restaurant.

---

## Lifecycle

```text
Created

↓

Active

↓

Checked Out

↓

Expired
```

---

## Future Expansion

- Shared Carts
- Saved Carts
- AI Cart Recovery

---

## Implementation Notes

Historical purchases are stored in Orders, not Carts.

---

# cart_items

## Purpose

Stores individual products contained within a shopping cart.

---

## Business Domain

Commerce

---

## Database Schema

commerce

---

## Owner

Cart

---

## Parent Entity

carts

---

## Child Entities

None

---

## References

- carts
- menu_items
- item_variants

---

## Referenced By

Checkout

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

No

---

## Expected Row Growth

Very High

---

## Expected Cardinality

Multiple items per cart.

---

## Lifecycle

```text
Added

↓

Updated

↓

Removed

↓

Checked Out
```

---

## Future Expansion

- Bundle Support
- Personalized Pricing

---

## Implementation Notes

Cart contents remain mutable until checkout.

---

# orders

## Purpose

Represents finalized customer purchases.

Orders are immutable business records.

---

## Business Domain

Commerce

---

## Database Schema

commerce

---

## Owner

Tenant

---

## Parent Entity

restaurants

---

## Child Entities

- order_items

---

## References

- customers
- branches
- payment_transactions

---

## Referenced By

Analytics

Billing

Reporting

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Very High

---

## Expected Cardinality

Millions platform-wide.

---

## Lifecycle

```text
Draft

↓

Pending

↓

Confirmed

↓

Preparing

↓

Ready

↓

Assigned

↓

Out For Delivery

↓

Delivered

↓

Completed
```

---

## Future Expansion

- Scheduled Orders
- Group Orders
- Marketplace Orders

---

## Implementation Notes

Orders are immutable after confirmation.

---

# order_items

## Purpose

Stores purchased products within an order.

Maintains historical pricing snapshots.

---

## Business Domain

Commerce

---

## Database Schema

commerce

---

## Owner

Order

---

## Parent Entity

orders

---

## Child Entities

None

---

## References

- orders

---

## Referenced By

Analytics

Reporting

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Very High

---

## Expected Cardinality

Multiple items per order.

---

## Lifecycle

Same as Order.

---

## Future Expansion

- Kitchen Routing
- Inventory Allocation

---

## Implementation Notes

Product data shall be snapshotted at purchase time.

---

# Customer Table Specifications

---

# customers

## Purpose

Represents a customer's business relationship with a tenant.

---

## Business Domain

Customer

---

## Database Schema

customer

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

- customer_addresses
- customer_preferences
- customer_favorites
- loyalty_accounts

---

## References

- users
- tenants

---

## Referenced By

Orders

Reservations

Notifications

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Very High

---

## Expected Cardinality

Millions platform-wide.

---

## Lifecycle

```text
Guest

↓

Registered

↓

Verified

↓

Active

↓

VIP

↓

Inactive

↓

Archived
```

---

## Future Expansion

- Corporate Customers
- Customer Groups

---

## Implementation Notes

One platform user may have multiple customer records across different tenants.

---

# customer_addresses

## Purpose

Stores customer delivery addresses.

---

## Business Domain

Customer

---

## Database Schema

customer

---

## Owner

Customer

---

## Parent Entity

customers

---

## Child Entities

None

---

## References

- customers

---

## Referenced By

Orders

Checkout

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

No

---

## Expected Row Growth

High

---

## Expected Cardinality

Multiple addresses per customer.

---

## Lifecycle

Created → Active → Archived

---

## Future Expansion

- Geocoding
- Delivery Validation

---

## Implementation Notes

Supports home, office, and custom address types.

---

# customer_preferences

## Purpose

Stores customer-specific ordering and communication preferences.

---

## Business Domain

Customer

---

## Database Schema

customer

---

## Owner

Customer

---

## Parent Entity

customers

---

## References

- customers

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

No

---

## Expected Cardinality

One record per customer.

---

## Implementation Notes

Contains non-transactional preference data.

---

# customer_favorites

## Purpose

Stores products bookmarked by customers for quick reordering.

---

## Business Domain

Customer

---

## Database Schema

customer

---

## Owner

Customer

---

## Parent Entity

customers

---

## References

- customers
- menu_items

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

No

---

## Expected Row Growth

High

---

## Implementation Notes

Supports personalized customer experiences.

---

# loyalty_accounts

## Purpose

Represents a customer's loyalty membership within a restaurant.

---

## Business Domain

Customer

---

## Database Schema

customer

---

## Owner

Customer

---

## Parent Entity

customers

---

## Child Entities

- loyalty_transactions
- reward_points

---

## References

- customers

---

## Referenced By

Checkout

Marketing

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Implementation Notes

Supports future loyalty program expansion.

---

# loyalty_transactions

## Purpose

Stores every loyalty point earning and redemption event.

---

## Business Domain

Customer

---

## Database Schema

customer

---

## Owner

Loyalty Account

---

## Parent Entity

loyalty_accounts

---

## References

- loyalty_accounts

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Very High

---

## Implementation Notes

Provides a complete loyalty audit trail.

---

# reward_points

## Purpose

Maintains the customer's current loyalty point balance.

---

## Business Domain

Customer

---

## Database Schema

customer

---

## Owner

Loyalty Account

---

## Parent Entity

loyalty_accounts

---

## References

- loyalty_accounts

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Cardinality

One record per loyalty account.

---

## Implementation Notes

Represents the current balance only; history belongs in `loyalty_transactions`.

---

# customer_devices

## Purpose

Stores devices registered by customers for authentication, security, and push notifications.

---

## Business Domain

Customer

---

## Database Schema

customer

---

## Owner

Customer

---

## Parent Entity

customers

---

## References

- customers

---

## Referenced By

Notification Services

Authentication Services

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

No

---

## Expected Row Growth

High

---

## Implementation Notes

Supports trusted devices and push notification registration.

---

# communication_preferences

## Purpose

Stores customer consent and communication preferences.

---

## Business Domain

Customer

---

## Database Schema

customer

---

## Owner

Customer

---

## Parent Entity

customers

---

## References

- customers

---

## Referenced By

Notification Services

Marketing Services

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Cardinality

One record per customer.

---

## Implementation Notes

Supports compliance with privacy regulations and marketing consent management.

---

# Chapter 5 Summary

The **Commerce and Customer Domains** collectively power the customer journey across the FluxDine platform. Commerce tables manage menus, products, carts, and orders, while Customer tables maintain customer relationships, preferences, loyalty, and engagement. Together they provide the transactional foundation for online ordering, customer retention, and personalized dining experiences, all while preserving strict tenant isolation and historical data integrity.

# Chapter 6 — Reservation, Billing & Payment Tables

## 6.1 Purpose

The Reservation, Billing, and Payment Domains together govern the financial and reservation operations of the FluxDine platform.

These domains ensure that restaurant reservations, SaaS subscriptions, customer payments, and financial transactions remain independent while working together through well-defined relationships.

This separation aligns with the architectural principles established in:

- 05 Database Architecture & Multi-Tenant Data Model
- 06 API & Service Architecture
- Payment Gateway Abstraction Layer

---

## 6.2 Domain Ownership Hierarchy

```text
Reservation

├── Reservations
├── Reservation Tables
├── Waitlists
└── Reservation Events

Billing

├── Billing Accounts
├── Subscriptions
├── Invoices
├── Invoice Items
├── Credit Notes
└── Refund Requests

Payment

├── Payment Providers
├── Gateway Configurations
├── Merchant Accounts
├── Payment Transactions
├── Payment Intents
├── Payment Methods
├── Payment Tokens
├── Refund Transactions
├── Settlement Records
├── Reconciliation Records
└── Webhook Events
```

---

# Reservation Table Specifications

---

# reservations

## Purpose

Represents customer table reservations.

Supports reservation lifecycle management, table allocation, customer arrival tracking, and reservation history.

---

## Business Domain

Reservation

---

## Database Schema

reservation

---

## Owner

Tenant

---

## Parent Entity

branches

---

## Child Entities

- reservation_tables
- reservation_events

---

## References

- customers
- branches

---

## Referenced By

- notifications
- analytics

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

High

---

## Expected Cardinality

Thousands to millions.

---

## Lifecycle

```text
Pending

↓

Confirmed

↓

Upcoming

↓

Active

↓

Completed
```

Alternative States

```text
Cancelled

No Show

Expired
```

---

## Future Expansion

- Group Reservations
- Waiting Queue Integration
- AI Capacity Planning

---

## Implementation Notes

Reservation status management follows the architecture defined in Document 05.

---

# reservation_tables

## Purpose

Maps reservations to one or more restaurant tables.

---

## Business Domain

Reservation

---

## Database Schema

reservation

---

## Owner

Reservation

---

## Parent Entity

reservations

---

## Child Entities

None

---

## References

- reservations
- restaurant_tables

---

## Referenced By

Reservation Engine

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Expected Cardinality

Multiple table assignments per reservation.

---

## Lifecycle

Assigned

↓

Modified

↓

Released

---

## Future Expansion

- Dynamic Table Merging
- AI Seating Optimization

---

## Implementation Notes

Supports multi-table reservations.

---

# waitlists

## Purpose

Stores customers waiting for table availability.

---

## Business Domain

Reservation

---

## Database Schema

reservation

---

## Owner

Branch

---

## Parent Entity

branches

---

## Child Entities

None

---

## References

- customers
- branches

---

## Referenced By

Reservation Engine

---

## Multi-Tenant

Yes

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Lifecycle

Waiting

↓

Notified

↓

Seated

↓

Expired

---

## Future Expansion

- SMS Notifications
- AI Wait Prediction

---

## Implementation Notes

Independent from confirmed reservations.

---

# reservation_events

## Purpose

Stores immutable reservation history.

---

## Business Domain

Reservation

---

## Database Schema

reservation

---

## Owner

Reservation

---

## Parent Entity

reservations

---

## Child Entities

None

---

## References

- reservations

---

## Referenced By

Audit

Analytics

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Very High

---

## Lifecycle

Append Only

---

## Implementation Notes

Every reservation state transition creates an event.

---

# Billing Table Specifications

---

# billing_accounts

## Purpose

Represents the financial account for each tenant.

---

## Business Domain

Billing

---

## Database Schema

billing

---

## Owner

Tenant

---

## Parent Entity

tenants

---

## Child Entities

- subscriptions
- invoices

---

## References

- tenants

---

## Referenced By

Billing Service

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Expected Cardinality

One billing account per tenant.

---

## Lifecycle

Created

↓

Active

↓

Suspended

↓

Closed

---

## Future Expansion

- Multiple Billing Accounts
- Corporate Billing

---

## Implementation Notes

Represents the tenant's commercial relationship with FluxDine.

---

# subscriptions

## Purpose

Represents SaaS subscriptions purchased by tenants.

---

## Business Domain

Billing

---

## Database Schema

billing

---

## Owner

Billing Account

---

## Parent Entity

billing_accounts

---

## Child Entities

- invoices

---

## References

- billing_accounts
- subscription_plans

---

## Referenced By

Payment Service

Analytics

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Lifecycle

```text
Trial

↓

Active

↓

Past Due

↓

Suspended

↓

Cancelled

↓

Expired
```

---

## Future Expansion

- Add-on Modules
- Enterprise Contracts

---

## Implementation Notes

Business rules are defined in the Billing Domain.

---

# invoices

## Purpose

Represents invoices generated for subscription billing.

---

## Business Domain

Billing

---

## Database Schema

billing

---

## Owner

Billing Account

---

## Parent Entity

subscriptions

---

## Child Entities

- invoice_items

---

## References

- subscriptions

---

## Referenced By

Payments

Accounting

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Lifecycle

Generated

↓

Issued

↓

Paid

↓

Overdue

↓

Void

---

## Future Expansion

- Credit Invoices
- Tax Invoices

---

## Implementation Notes

Invoices are immutable financial records.

---

# invoice_items

## Purpose

Represents line items within an invoice.

---

## Business Domain

Billing

---

## Database Schema

billing

---

## Owner

Invoice

---

## Parent Entity

invoices

---

## References

- invoices

---

## Referenced By

Reporting

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Medium

---

## Implementation Notes

Maintains historical billing breakdown.

---

# credit_notes

## Purpose

Represents credits issued against invoices.

---

## Business Domain

Billing

---

## Database Schema

billing

---

## Owner

Billing Account

---

## Parent Entity

billing_accounts

---

## References

- invoices

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Lifecycle

Issued

↓

Applied

↓

Closed

---

## Future Expansion

- Promotional Credits
- Manual Adjustments

---

# refund_requests

## Purpose

Tracks refund requests for subscription billing.

---

## Business Domain

Billing

---

## Database Schema

billing

---

## Owner

Billing Account

---

## References

- invoices
- payment_transactions

---

## Audit Required

Yes

---

## Lifecycle

Requested

↓

Approved

↓

Rejected

↓

Processed

---

## Implementation Notes

Financial approval workflow only.

---

# Payment Table Specifications

---

# payment_providers

## Purpose

Master catalog of supported payment gateways.

---

## Business Domain

Payment

---

## Database Schema

payment

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

- gateway_configurations

---

## References

None

---

## Referenced By

Tenant Payment Gateways

---

## Multi-Tenant

No

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Cardinality

Few records.

---

## Future Expansion

- Stripe
- PayPal
- Square
- Adyen
- Checkout.com

---

## Implementation Notes

Platform reference table.

---

# gateway_configurations

## Purpose

Stores provider-specific gateway configuration.

---

## Business Domain

Payment

---

## Database Schema

payment

---

## Owner

Tenant

---

## Parent Entity

payment_providers

---

## References

- payment_providers
- tenants

---

## Referenced By

Payment Service

---

## Multi-Tenant

Yes

---

## Audit Required

Yes

---

## Implementation Notes

Sensitive credentials must be encrypted.

---

# merchant_accounts

## Purpose

Represents merchant accounts registered with payment providers.

---

## Business Domain

Payment

---

## Database Schema

payment

---

## Owner

Tenant

---

## References

- gateway_configurations

---

## Referenced By

Payment Transactions

---

## Implementation Notes

Stores provider-issued merchant identifiers only.

---

# payment_transactions

## Purpose

Represents every payment processed by FluxDine.

Supports:

- Orders
- Subscriptions
- Future Marketplace Services

---

## Business Domain

Payment

---

## Database Schema

payment

---

## Owner

Tenant

---

## Parent Entity

merchant_accounts

---

## Child Entities

- payment_intents
- refund_transactions
- settlement_records

---

## References

- orders
- invoices
- merchant_accounts

---

## Referenced By

Reporting

Analytics

Billing

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Very High

---

## Lifecycle

```text
Created

↓

Pending

↓

Authorized

↓

Captured

↓

Completed
```

Alternative States

```text
Failed

Cancelled

Refunded
```

---

## Future Expansion

- Split Payments
- Marketplace Payments

---

## Implementation Notes

Immutable financial transaction.

---

# payment_intents

## Purpose

Represents pending payment authorization.

---

## Business Domain

Payment

---

## Database Schema

payment

---

## Owner

Payment Transaction

---

## References

- payment_transactions

---

## Lifecycle

Created

↓

Authorized

↓

Captured

↓

Expired

---

## Implementation Notes

Used by providers supporting two-step authorization.

---

# payment_methods

## Purpose

Stores reusable customer payment methods.

---

## Business Domain

Payment

---

## Database Schema

payment

---

## Owner

Customer

---

## References

- customers

---

## Multi-Tenant

Yes

---

## Audit Required

Yes

---

## Implementation Notes

Never stores raw card data.

---

# payment_tokens

## Purpose

Stores provider-issued payment tokens.

---

## Business Domain

Payment

---

## Database Schema

payment

---

## Owner

Customer

---

## References

- payment_methods

---

## Implementation Notes

PCI-compliant token storage only.

---

# refund_transactions

## Purpose

Represents payment refunds.

---

## Business Domain

Payment

---

## Database Schema

payment

---

## Owner

Payment Transaction

---

## References

- payment_transactions

---

## Lifecycle

Requested

↓

Approved

↓

Processed

↓

Completed

---

## Implementation Notes

Maintains immutable refund history.

---

# settlement_records

## Purpose

Tracks settlement between payment provider and merchant.

---

## Business Domain

Payment

---

## Database Schema

payment

---

## Owner

Payment Transaction

---

## References

- payment_transactions

---

## Lifecycle

Pending

↓

Settled

↓

Failed

---

## Implementation Notes

Supports reconciliation.

---

# reconciliation_records

## Purpose

Tracks reconciliation between provider reports and platform transactions.

---

## Business Domain

Payment

---

## Database Schema

payment

---

## Owner

Platform

---

## References

- settlement_records

---

## Audit Required

Yes

---

## Implementation Notes

Financial reporting support.

---

# webhook_events

## Purpose

Stores incoming payment provider webhook events.

---

## Business Domain

Payment

---

## Database Schema

payment

---

## Owner

Platform

---

## References

- payment_providers

---

## Referenced By

Payment Processing

Audit

---

## Expected Row Growth

Very High

---

## Lifecycle

Received

↓

Validated

↓

Processed

↓

Archived

---

## Implementation Notes

Stores webhook metadata and processing history only.

---

# Chapter 6 Summary

The **Reservation, Billing, and Payment Domains** provide the operational foundation for table reservations, SaaS billing, and financial processing within FluxDine. Reservation tables manage customer seating and booking workflows, Billing tables govern the commercial relationship between tenants and the platform, and Payment tables implement the gateway abstraction architecture through provider-independent payment processing. Together these domains ensure reliable reservation management, subscription lifecycle control, secure financial transactions, and complete auditability while preserving strict tenant isolation and financial integrity.

# Chapter 7 — Notification, Analytics, Branding, Feature Management & Shared Platform Tables

## 7.1 Purpose

This chapter defines the supporting infrastructure tables that enable communication, analytics, branding, feature management, and cross-cutting platform services across the FluxDine ecosystem.

Unlike operational business tables (Orders, Reservations, Customers, etc.), these tables provide shared capabilities consumed by every application and service within the platform.

These domains are designed to be highly reusable, loosely coupled, and horizontally scalable.

---

## Domain Overview

```text
Notification

├── Notifications
├── Notification Templates
├── Notification Queue
├── Email Messages
├── SMS Messages
└── Push Notifications

Analytics

├── Dashboard Metrics
├── KPI Definitions
├── KPI Snapshots
├── Reports
└── Usage Metrics

Branding

├── Themes
├── Theme Presets
├── Brand Assets
├── Logos
├── Domains
└── SEO Settings

Feature Management

├── Features
├── Feature Flags
├── Subscription Features
├── Tenant Features
└── Feature Usage

Shared Platform

├── Files
├── Audit Logs
├── Activity Logs
├── System Logs
├── Monitoring Events
├── Background Jobs
├── Scheduled Tasks
├── Search Indexes
├── Webhook Registrations
└── API Keys
```

---

# Notification Table Specifications

---

# notifications

## Purpose

Represents every logical notification generated by the platform before channel delivery.

---

## Business Domain

Notification

---

## Database Schema

notification

---

## Owner

Platform

---

## Parent Entity

Platform

---

## Child Entities

- email_messages
- sms_messages
- push_notifications

---

## References

- notification_templates

---

## Referenced By

Notification Service

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

Yes

---

## Expected Row Growth

Very High

---

## Lifecycle

```text
Created

↓

Queued

↓

Sent

↓

Delivered

↓

Archived
```

---

## Future Expansion

- In-App Messaging
- Multi-Channel Delivery
- AI Personalization

---

## Implementation Notes

Acts as the parent record for every outbound communication.

---

# notification_templates

## Purpose

Stores reusable templates for emails, SMS, push notifications, and in-app messaging.

---

## Business Domain

Notification

---

## Database Schema

notification

---

## Owner

Platform

---

## References

None

---

## Referenced By

notifications

---

## Multi-Tenant

No

---

## Soft Delete

Yes

---

## Audit Required

Yes

---

## Expected Row Growth

Low

---

## Lifecycle

Draft

↓

Published

↓

Archived

---

## Implementation Notes

Supports localization and template versioning.

---

# notification_queue

## Purpose

Represents notifications waiting to be processed by the Notification Service.

---

## Business Domain

Notification

---

## Database Schema

notification

---

## Owner

Platform

---

## References

- notifications

---

## Referenced By

Background Jobs

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

No

---

## Expected Row Growth

Very High

---

## Implementation Notes

Temporary operational queue.

---

# email_messages

## Purpose

Stores outbound email delivery records.

---

## Business Domain

Notification

---

## Database Schema

notification

---

## Owner

Notification

---

## References

- notifications

---

## Multi-Tenant

Yes

---

## Audit Required

Yes

---

## Implementation Notes

Stores delivery status only.

---

# sms_messages

## Purpose

Stores outbound SMS delivery history.

---

## Business Domain

Notification

---

## Database Schema

notification

---

## Owner

Notification

---

## References

- notifications

---

## Multi-Tenant

Yes

---

## Audit Required

Yes

---

# push_notifications

## Purpose

Stores mobile push notification delivery history.

---

## Business Domain

Notification

---

## Database Schema

notification

---

## Owner

Notification

---

## References

- notifications

---

## Multi-Tenant

Yes

---

## Audit Required

Yes

---

# Analytics Table Specifications

---

# dashboard_metrics

## Purpose

Stores precomputed dashboard metrics for high-performance reporting.

---

## Business Domain

Analytics

---

## Database Schema

analytics

---

## Owner

Platform

---

## References

None

---

## Referenced By

HQ Dashboard

Restaurant Dashboard

---

## Multi-Tenant

Yes

---

## Soft Delete

No

---

## Audit Required

No

---

## Expected Row Growth

High

---

## Implementation Notes

Read-optimized analytical data.

---

# kpi_definitions

## Purpose

Defines platform-wide Key Performance Indicators.

---

## Business Domain

Analytics

---

## Database Schema

analytics

---

## Owner

Platform

---

## References

None

---

## Referenced By

kpi_snapshots

---

## Expected Cardinality

Hundreds of KPIs.

---

## Implementation Notes

Master KPI catalog.

---

# kpi_snapshots

## Purpose

Stores historical KPI measurements.

---

## Business Domain

Analytics

---

## Database Schema

analytics

---

## Owner

Platform

---

## References

- kpi_definitions

---

## Expected Row Growth

Very High

---

## Implementation Notes

Immutable historical records.

---

# reports

## Purpose

Represents generated reports across the platform.

---

## Business Domain

Analytics

---

## Database Schema

analytics

---

## Owner

Platform

---

## References

Multiple Business Domains

---

## Multi-Tenant

Yes

---

## Audit Required

Yes

---

## Future Expansion

- Scheduled Reports
- AI Reports

---

# usage_metrics

## Purpose

Stores aggregated usage statistics.

---

## Business Domain

Analytics

---

## Database Schema

analytics

---

## Owner

Platform

---

## Expected Row Growth

Very High

---

## Implementation Notes

Supports platform analytics and subscription reporting.

---

# Branding Table Specifications

---

# themes

## Purpose

Represents visual themes available to tenants.

---

## Business Domain

Branding

---

## Database Schema

branding

---

## Owner

Tenant

---

## References

- tenant_branding

---

## Referenced By

Customer Experience

---

## Multi-Tenant

Yes

---

## Audit Required

Yes

---

## Future Expansion

- Marketplace Themes
- AI Theme Generator

---

# theme_presets

## Purpose

Stores reusable design templates.

---

## Business Domain

Branding

---

## Database Schema

branding

---

## Owner

Platform

---

## References

None

---

## Referenced By

themes

---

# brand_assets

## Purpose

Stores uploaded branding resources.

---

## Business Domain

Branding

---

## Database Schema

branding

---

## Owner

Tenant

---

## References

- tenants

---

## Referenced By

themes

logos

---

## Multi-Tenant

Yes

---

# logos

## Purpose

Stores restaurant logo assets.

---

## Business Domain

Branding

---

## Database Schema

branding

---

## Owner

Tenant

---

## References

- brand_assets

---

# domains

## Purpose

Stores customer-facing internet domains.

---

## Business Domain

Branding

---

## Database Schema

branding

---

## Owner

Tenant

---

## References

- tenant_domains

---

## Future Expansion

- Multi-Domain Routing
- Regional Domains

---

# seo_settings

## Purpose

Stores SEO metadata for customer-facing websites.

---

## Business Domain

Branding

---

## Database Schema

branding

---

## Owner

Tenant

---

## References

- domains

---

## Future Expansion

- AI SEO
- Structured Data

---

# Feature Management Table Specifications

---

# features

## Purpose

Master catalog of all platform capabilities.

---

## Business Domain

Feature Management

---

## Database Schema

feature

---

## Owner

Platform

---

## Child Entities

- feature_flags
- subscription_features
- tenant_features

---

## Implementation Notes

Authoritative feature catalog.

---

# feature_flags

## Purpose

Controls runtime feature activation.

---

## Business Domain

Feature Management

---

## Database Schema

feature

---

## Owner

Platform

---

## References

- features

---

## Future Expansion

- Canary Releases
- Percentage Rollouts

---

# subscription_features

## Purpose

Maps subscription plans to platform features.

---

## Business Domain

Feature Management

---

## Database Schema

feature

---

## Owner

Platform

---

## References

- subscription_plans
- features

---

# tenant_features

## Purpose

Stores tenant-specific feature overrides.

---

## Business Domain

Feature Management

---

## Database Schema

feature

---

## Owner

Tenant

---

## References

- tenants
- features

---

# feature_usage

## Purpose

Tracks feature adoption and utilization.

---

## Business Domain

Feature Management

---

## Database Schema

feature

---

## Owner

Platform

---

## Expected Row Growth

Very High

---

# Shared Platform Table Specifications

---

# files

## Purpose

Stores metadata for uploaded files.

---

## Business Domain

Shared Platform

---

## Database Schema

shared

---

## Owner

Platform

---

## References

Multiple Business Domains

---

## Future Expansion

- Object Storage
- CDN Integration

---

# audit_logs

## Purpose

Stores immutable audit records.

---

## Business Domain

Shared Platform

---

## Database Schema

shared

---

## Owner

Platform

---

## Expected Row Growth

Very High

---

## Implementation Notes

Append-only architecture.

---

# activity_logs

## Purpose

Stores user activity history.

---

## Business Domain

Shared Platform

---

## Database Schema

shared

---

## Owner

Platform

---

# system_logs

## Purpose

Stores application and infrastructure logs.

---

## Business Domain

Shared Platform

---

## Database Schema

shared

---

## Owner

Platform

---

# monitoring_events

## Purpose

Stores monitoring and observability events.

---

## Business Domain

Shared Platform

---

## Database Schema

shared

---

## Owner

Platform

---

# background_jobs

## Purpose

Represents asynchronous jobs executed by the platform.

---

## Business Domain

Shared Platform

---

## Database Schema

shared

---

## Owner

Platform

---

## Future Expansion

- Distributed Workers
- Priority Queues

---

# scheduled_tasks

## Purpose

Stores recurring scheduled platform tasks.

---

## Business Domain

Shared Platform

---

## Database Schema

shared

---

## Owner

Platform

---

## Examples

- Subscription Renewal
- Reservation Status Updates
- Daily Reports
- Cleanup Jobs

---

# search_indexes

## Purpose

Stores searchable indexes for full-text search.

---

## Business Domain

Shared Platform

---

## Database Schema

shared

---

## Owner

Platform

---

# webhook_registrations

## Purpose

Stores outbound webhook subscriptions.

---

## Business Domain

Shared Platform

---

## Database Schema

shared

---

## Owner

Platform

---

# api_keys

## Purpose

Stores platform-issued API credentials.

---

## Business Domain

Shared Platform

---

## Database Schema

shared

---

## Owner

Platform

---

## Future Expansion

- Scoped API Keys
- Temporary Keys
- Service Accounts

---

# Chapter 7 Summary

The **Notification, Analytics, Branding, Feature Management, and Shared Platform Domains** provide the cross-cutting services that support every application within the FluxDine ecosystem. These tables enable platform-wide communication, reporting, tenant branding, feature governance, auditing, observability, search, file management, and background processing. Together they form the shared service layer that underpins the operational and technical capabilities of the entire multi-tenant SaaS platform while remaining independent of individual business domains.


# Chapter 8 — Cross-Table Relationships

## 8.1 Purpose

This chapter defines the logical relationships between database tables across all business domains.

While **03 Relationships.md** will later define every individual foreign key, cardinality, and referential constraint, this chapter establishes the high-level relationship model that governs the FluxDine database architecture.

It serves as the bridge between logical table specifications and detailed relational implementation.

---

## 8.2 Relationship Philosophy

Every table relationship within FluxDine shall adhere to the following principles:

- Explicit ownership
- Single source of truth
- Referential integrity
- Tenant isolation
- Domain-driven organization
- Loose coupling
- Predictable navigation
- Implementation independence

Relationships shall always support the ownership hierarchy established within the Core Architecture.

---

## 8.3 Platform Relationship Hierarchy

```text
Platform

↓

Identity

↓

Tenant

↓

Restaurant

↓

Branch

↓

Commerce

↓

Customer

↓

Reservation

↓

Billing

↓

Payment

↓

Notification

↓

Analytics

↓

Branding

↓

Feature Management

↓

Shared Platform
```

Every business relationship ultimately traces back to the Platform.

---

## 8.4 Domain Relationship Matrix

| Domain | Primary Parent | Major Child Domains |
|----------|----------------|---------------------|
| Platform | None | Identity, Tenant, Billing, Feature |
| Identity | Platform | Authentication |
| Tenant | Platform | Restaurant, Branding, Features |
| Restaurant | Tenant | Commerce, Reservation |
| Commerce | Restaurant | Orders |
| Customer | Tenant | Loyalty |
| Reservation | Restaurant | Reservation Events |
| Billing | Tenant | Subscription |
| Payment | Billing / Commerce | Transactions |
| Notification | Platform | Email, SMS, Push |
| Analytics | Platform | Reports |
| Branding | Tenant | Themes |
| Feature | Platform | Tenant Features |
| Shared Platform | Platform | Audit, Logs |

---

# Chapter 9 — Ownership Matrix

## 9.1 Purpose

The Ownership Matrix defines the authoritative owner for every major database table.

Ownership determines:

- Access control
- Tenant isolation
- Security boundaries
- Cascade behavior
- Auditing responsibility
- Administrative authority

No table shall exist without a clearly documented owner.

---

## 9.2 Ownership Hierarchy

```text
Platform

↓

Tenant

↓

Restaurant

↓

Branch

↓

Business Resources
```

---

## 9.3 Ownership Matrix

| Table | Owner |
|---------|-------|
| users | Platform |
| user_memberships | Platform |
| tenants | Platform |
| restaurants | Tenant |
| branches | Restaurant |
| menus | Restaurant |
| menu_categories | Menu |
| menu_items | Category |
| customers | Tenant |
| carts | Customer |
| orders | Tenant |
| reservations | Tenant |
| subscriptions | Billing Account |
| invoices | Billing Account |
| payment_transactions | Tenant |
| notifications | Platform |
| dashboard_metrics | Platform |
| themes | Tenant |
| feature_flags | Platform |
| audit_logs | Platform |

---

## 9.4 Ownership Principles

Every table shall satisfy the following rules:

### Single Owner

Each table has exactly one logical owner.

---

### Ownership Never Changes

Ownership is immutable after record creation.

---

### Child Ownership

Children inherit ownership from their parent entity.

---

### Platform Exception

Platform-owned tables are globally shared and are not tenant-owned.

---

# Chapter 10 — Table Lifecycle Standards

## 10.1 Purpose

This chapter standardizes the lifecycle expectations for every database table.

Although individual business tables may introduce additional workflow states, all persistent entities shall follow the same high-level lifecycle model.

---

## 10.2 Standard Lifecycle

```text
Created

↓

Active

↓

Updated

↓

Archived

↓

Soft Deleted

↓

Hard Deleted
```

Hard deletion is reserved exclusively for administrative maintenance and legal compliance processes.

---

## 10.3 Lifecycle Principles

Every table shall:

- Support traceability.
- Preserve historical integrity.
- Favor soft deletion.
- Record lifecycle transitions.
- Maintain audit history.

---

## 10.4 Domain Extensions

Business domains may extend the lifecycle.

Examples include:

### Orders

```text
Draft

↓

Pending

↓

Confirmed

↓

Preparing

↓

Ready

↓

Completed
```

### Reservations

```text
Pending

↓

Confirmed

↓

Upcoming

↓

Active

↓

Completed
```

### Subscriptions

```text
Trial

↓

Active

↓

Past Due

↓

Cancelled
```

---

# Chapter 11 — Table Naming Reference

## 11.1 Purpose

This chapter summarizes the naming conventions applied throughout the database.

The authoritative rules remain defined in:

**00 Database Naming Standards.md**

This chapter provides a quick engineering reference.

---

## 11.2 Naming Rules

Tables shall:

- use lowercase
- use snake_case
- use plural nouns
- avoid abbreviations
- describe business concepts
- remain technology independent

---

## 11.3 Examples

| Correct | Incorrect |
|-----------|-----------|
| users | User |
| menu_items | menuItem |
| payment_transactions | payments |
| reservation_events | reservationHistory |
| customer_addresses | address |

---

## 11.4 Schema Naming

Approved schemas include:

```text
platform
identity
tenant
restaurant
commerce
customer
reservation
billing
payment
notification
analytics
branding
feature
shared
```

---

# Chapter 12 — Engineering Standards

## 12.1 Purpose

This chapter defines mandatory engineering standards for every database table.

These standards ensure consistency across database implementation, migrations, ORM mappings, repositories, and APIs.

---

## 12.2 Mandatory Standards

Every table shall:

- Follow Database Naming Standards.
- Have a documented business purpose.
- Belong to exactly one business domain.
- Have one logical owner.
- Support tenant isolation where applicable.
- Define lifecycle expectations.
- Support auditing where required.
- Remain implementation independent.
- Be documented before implementation.

---

## 12.3 Database Design Principles

The FluxDine database shall emphasize:

- High cohesion
- Low coupling
- Domain-driven organization
- Referential integrity
- Predictable ownership
- Horizontal scalability
- Event-driven extensibility
- Backward compatibility

---

## 12.4 Documentation Requirements

Before implementation, every new table shall have:

- Table Specification
- Relationship Specification
- Constraint Specification
- Index Specification
- Enum Specification (if applicable)
- Migration Plan
- Drizzle ORM Mapping

No undocumented table shall be introduced into the production schema.

---

# Chapter 13 — Architecture Decision Records

The following architectural decisions govern the Table Specifications document.

---

## ADR-TS-001

Every business table shall have exactly one logical owner.

---

## ADR-TS-002

Every table shall belong to exactly one business domain.

---

## ADR-TS-003

Platform-owned tables shall remain globally shared.

---

## ADR-TS-004

Tenant-owned tables shall enforce tenant isolation.

---

## ADR-TS-005

Soft deletion shall be preferred over physical deletion.

---

## ADR-TS-006

Historical business records shall remain immutable.

---

## ADR-TS-007

Every significant business table shall support auditing.

---

## ADR-TS-008

Documentation shall precede implementation.

---

## ADR-TS-009

Table specifications are the authoritative source for logical table design.

---

## ADR-TS-010

Physical database implementation shall conform to the specifications defined in this document.

---

# Engineering Reference Summary

This chapter concludes the engineering guidance for the **Table Specifications** document.

Combined with the domain-specific table definitions presented in the preceding chapters, these standards establish the complete logical specification for every database table within the FluxDine platform.

Subsequent Engineering Specifications—including **03 Relationships**, **04 Constraints**, **05 Index Specification**, **06 Enum Specification**, **07 Database Migration Strategy**, and **08 Drizzle ORM Mapping**—shall build directly upon the table definitions established in this document without redefining business ownership or table responsibilities.


---

# Appendices

---

# Appendix A — Complete Table Inventory

The following inventory represents the complete logical database table catalog defined by the FluxDine Architecture Bible Version 1.0.

This inventory serves as the master reference for all subsequent engineering specifications and implementation artifacts.

---

## Platform Domain

- countries
- currencies
- languages
- timezones
- subscription_plans
- platform_features
- global_roles
- global_permissions
- email_templates
- notification_templates
- platform_settings
- platform_announcements

---

## Identity Domain

- users
- user_memberships
- roles
- permissions
- role_permissions
- user_roles
- sessions
- authentication_providers
- mfa_configurations
- password_reset_tokens
- email_verification_tokens

---

## Tenant Domain

- tenants
- tenant_settings
- tenant_domains
- tenant_branding
- tenant_features
- tenant_payment_gateways
- tenant_integrations
- tenant_subscriptions
- tenant_usage_metrics
- tenant_api_keys
- tenant_audit_settings
- tenant_invitations

---

## Restaurant Domain

- restaurants
- branches
- business_hours
- delivery_zones
- seating_areas
- restaurant_tables
- kitchen_stations
- staff
- riders
- rider_assignments
- restaurant_settings

---

## Commerce Domain

- menus
- menu_categories
- menu_items
- item_variants
- item_options
- item_addons
- carts
- cart_items
- orders
- order_items

---

## Customer Domain

- customers
- customer_addresses
- customer_preferences
- customer_favorites
- loyalty_accounts
- loyalty_transactions
- reward_points
- customer_devices
- communication_preferences

---

## Reservation Domain

- reservations
- reservation_tables
- waitlists
- reservation_events

---

## Billing Domain

- billing_accounts
- subscriptions
- invoices
- invoice_items
- credit_notes
- refund_requests

---

## Payment Domain

- payment_providers
- gateway_configurations
- merchant_accounts
- payment_transactions
- payment_intents
- payment_methods
- payment_tokens
- refund_transactions
- settlement_records
- reconciliation_records
- webhook_events

---

## Notification Domain

- notifications
- notification_templates
- notification_queue
- email_messages
- sms_messages
- push_notifications

---

## Analytics Domain

- dashboard_metrics
- kpi_definitions
- kpi_snapshots
- reports
- usage_metrics

---

## Branding Domain

- themes
- theme_presets
- brand_assets
- logos
- domains
- seo_settings

---

## Feature Management Domain

- features
- feature_flags
- subscription_features
- tenant_features
- feature_usage

---

## Shared Platform Domain

- files
- audit_logs
- activity_logs
- system_logs
- monitoring_events
- background_jobs
- scheduled_tasks
- search_indexes
- webhook_registrations
- api_keys

---

# Appendix B — Estimated Growth Characteristics

The following estimates guide future infrastructure planning and capacity forecasting.

| Domain | Growth |
|----------|---------|
| Platform | Static |
| Identity | Very High |
| Tenant | Medium |
| Restaurant | Medium |
| Commerce | Very High |
| Customer | Very High |
| Reservation | High |
| Billing | Medium |
| Payment | Very High |
| Notification | Very High |
| Analytics | Extremely High |
| Branding | Low |
| Feature Management | Low |
| Shared Platform | Extremely High |

---

## Highest Growth Tables

Expected to exceed millions of records:

- users
- sessions
- orders
- order_items
- customers
- reservations
- payment_transactions
- notifications
- audit_logs
- activity_logs
- monitoring_events
- background_jobs

These tables should be considered primary candidates for:

- Partitioning
- Archival
- Read optimization
- Background maintenance
- Horizontal scaling

---

# Appendix C — Reserved Future Tables

The following tables are intentionally reserved for future platform evolution.

Documenting them now minimizes disruptive schema changes in later releases.

---

## Artificial Intelligence

- ai_assistants
- ai_conversations
- ai_prompts
- ai_recommendations
- ai_order_predictions

---

## Inventory

- inventory_items
- inventory_transactions
- suppliers
- purchase_orders
- stock_adjustments

---

## Kitchen Display System (KDS)

- kitchen_orders
- kitchen_workstations
- preparation_events

---

## Marketplace

- marketplace_orders
- marketplace_merchants
- marketplace_commissions

---

## Gift Cards

- gift_cards
- gift_card_transactions

---

## Loyalty Expansion

- loyalty_campaigns
- loyalty_rewards
- customer_badges

---

## Delivery Fleet

- vehicles
- rider_locations
- delivery_routes

---

## Workforce Management

- employee_shifts
- attendance
- payroll_profiles

---

## AI Analytics

- predictive_models
- anomaly_detection
- forecasting_results

---

These tables are **reserved** and shall not be implemented until formally introduced through approved Architecture Decision Records.

---

# References

This document should be read together with the following FluxDine Architecture Bible documents.

---

## Core Architecture

- 04 System Architecture Blueprint
- 05 Database Architecture & Multi-Tenant Data Model
- 06 API & Service Architecture
- 07 Security Architecture
- 08 Infrastructure Architecture

---

## Engineering Specifications

- 00 Database Naming Standards
- 03 Relationships
- 04 Constraints
- 05 Index Specification
- 06 Enum Specification
- 07 Database Migration Strategy
- 08 Drizzle ORM Mapping

---

## Architecture Decision Records

- ADR Register
- Database ADRs
- Security ADRs
- Infrastructure ADRs

---

# Revision History

| Version | Date | Author | Description |
|----------|------|---------|-------------|
| 0.1 | Initial Draft | FluxDine Engineering | Document foundation created |
| 0.5 | Domain Specifications | FluxDine Engineering | Platform, Identity, Tenant, Restaurant, Commerce, Customer, Reservation, Billing, Payment, Notification, Analytics, Branding, Feature Management, and Shared Platform tables documented |
| 0.9 | Engineering References | FluxDine Engineering | Ownership matrix, lifecycle standards, naming references, engineering standards, and ADRs completed |
| 1.0 | Final Release | FluxDine Engineering | Initial implementation-ready logical table specification approved and locked |

---

# Document Completion Summary

This document defines the complete logical specification for every database table within the FluxDine platform.

It establishes:

- The complete table inventory
- Business ownership
- Domain organization
- Parent-child hierarchy
- Lifecycle expectations
- Growth characteristics
- Engineering standards
- Future expansion strategy

This specification intentionally excludes implementation details such as:

- Database columns
- Data types
- Foreign keys
- Constraints
- Indexes
- Enums
- SQL migrations
- Drizzle ORM schemas

These topics are defined in the subsequent Engineering Specification documents.

---
