# 02 Engineering Specifications

# Database

# 04 — Constraints

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-DB-004 |
| **Document Name** | Constraints |
| **Version** | 1.0 |
| **Status** | Draft |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | 00 Database Naming Standards<br>01 Complete Database Schema Specification<br>02 Table Specifications<br>03 Relationships |
| **Referenced By** | 05 Index Specification<br>06 Enum Specification<br>07 Database Migration Strategy<br>08 Drizzle ORM Mapping |

---

# Dependencies

This document depends on the following engineering specifications:

- **00 Database Naming Standards.md**
- **01 Complete Database Schema Specification.md**
- **02 Table Specifications.md**
- **03 Relationships.md**

These documents collectively define the naming conventions, logical schema, entity specifications, and relationship architecture upon which database constraints are established.

---

# Referenced By

The constraints defined in this document are consumed by the following engineering specifications:

- **05 Index Specification.md**
- **06 Enum Specification.md**
- **07 Database Migration Strategy.md**
- **08 Drizzle ORM Mapping.md**

Implementation artifacts shall conform to the constraint definitions established herein.

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

The purpose of this document is to define the complete logical constraint architecture for the FluxDine database.

While **03 Relationships.md** establishes how entities relate to one another, this document specifies the integrity rules that govern those relationships and ensure the database remains accurate, consistent, secure, and reliable throughout the application lifecycle.

The constraints defined in this specification are implementation-independent and describe *what* must be enforced rather than *how* a specific database engine enforces it.

This document serves as the authoritative engineering reference for all database constraints within the FluxDine platform.

---

# Scope

This specification defines:

- Primary Key constraints
- Foreign Key constraints
- Unique constraints
- NOT NULL constraints
- Default constraints
- Check constraints
- Composite constraints
- Business constraints
- Tenant isolation constraints
- Lifecycle constraints
- Cross-domain integrity rules
- Engineering constraint principles

These constraints apply across every database domain defined within the FluxDine platform.

---

# Out of Scope

This document does **not** define:

- SQL syntax
- PostgreSQL implementation
- Drizzle ORM code
- Database migrations
- Index definitions
- Query optimization
- Repository implementation
- Service-layer validation
- API validation

Those implementation details are specified in subsequent Engineering Specification documents.

---

# Constraint Philosophy

Database constraints are the first line of defense against invalid data.

FluxDine follows a **database-first integrity philosophy**, where critical business rules are enforced at the persistence layer rather than relying solely on application logic.

Every constraint defined within this specification exists to protect one or more of the following architectural qualities:

- Data Integrity
- Referential Integrity
- Tenant Isolation
- Security
- Predictability
- Auditability
- Consistency
- Historical Accuracy

Whenever practical, invalid data shall be prevented from entering the database rather than detected after persistence.

---

# Constraint Design Principles

Every constraint defined within the FluxDine platform shall adhere to the following principles.

## Principle 1 — Explicitness

Every constraint shall have a clearly documented purpose.

Implicit or undocumented constraints are prohibited.

---

## Principle 2 — Consistency

Similar entities shall follow consistent constraint patterns throughout the database.

---

## Principle 3 — Integrity

Constraints shall preserve both logical and referential integrity at all times.

---

## Principle 4 — Tenant Isolation

No constraint shall permit cross-tenant data access or ownership.

---

## Principle 5 — Historical Preservation

Constraints shall favor preserving historical business records over destructive operations.

---

## Principle 6 — Technology Independence

Constraint definitions shall remain independent of any specific database engine or ORM framework.

---

## Principle 7 — Business Alignment

Every constraint shall represent a legitimate business rule or engineering requirement.

---

# Constraint Categories

The FluxDine platform defines the following categories of constraints.

## Primary Key Constraints

Guarantee the unique identity of every persistent entity.

---

## Foreign Key Constraints

Maintain referential integrity between related entities.

---

## Unique Constraints

Prevent duplicate values where business uniqueness is required.

---

## NOT NULL Constraints

Ensure mandatory business information is always present.

---

## Default Constraints

Automatically assign predefined values when no explicit value is supplied.

---

## Check Constraints

Validate that stored values satisfy defined business rules.

---

## Composite Constraints

Enforce uniqueness or integrity across multiple columns.

---

## Business Constraints

Protect business-specific rules that cannot be represented by simple structural constraints.

---

## Lifecycle Constraints

Restrict invalid state transitions throughout an entity's lifecycle.

---

# How to Read This Document

This specification is organized by business domain.

Each chapter defines the constraints applicable to a particular domain using a standardized engineering template.

Constraint definitions are logical specifications and shall be interpreted independently of any specific implementation technology.

Each constraint includes:

- Constraint Name
- Purpose
- Constraint Type
- Business Domain
- Table
- Columns
- Referenced Table (where applicable)
- Enforcement Rule
- Failure Behaviour
- Engineering Notes

Together, these definitions provide the authoritative integrity model for the FluxDine database.

---

# Standard Constraint Specification Template

Every constraint defined within this document shall follow the standardized structure below.

## Constraint Name

Official engineering identifier for the constraint.

---

## Purpose

Describes the business or engineering reason for the constraint.

---

## Constraint Type

One of the following:

- Primary Key
- Foreign Key
- Unique
- NOT NULL
- Default
- Check
- Composite
- Business

---

## Business Domain

The domain to which the constraint belongs.

---

## Table

Logical table affected by the constraint.

---

## Columns

Columns governed by the constraint.

---

## Referenced Table

Applicable only to foreign key constraints.

---

## Referenced Columns

Referenced primary or unique columns.

---

## Required

Indicates whether the constraint is mandatory.

---

## Enforcement Rule

Defines the integrity rule enforced by the constraint.

---

## Failure Behaviour

Expected database behavior when the constraint is violated.

---

## Engineering Notes

Additional architectural guidance for implementers.

---

# Table of Contents

## Chapter 1 — Platform Constraints

## Chapter 2 — Identity Constraints

## Chapter 3 — Tenant Constraints

## Chapter 4 — Restaurant Constraints

## Chapter 5 — Commerce Constraints

## Chapter 6 — Customer Constraints

## Chapter 7 — Reservation Constraints

## Chapter 8 — Billing Constraints

## Chapter 9 — Payment Constraints

## Chapter 10 — Notification Constraints

## Chapter 11 — Analytics Constraints

## Chapter 12 — Branding Constraints

## Chapter 13 — Feature Management Constraints

## Chapter 14 — Shared Platform Constraints

## Chapter 15 — Cross-Domain Constraints

## Chapter 16 — Tenant Isolation Rules

## Chapter 17 — Data Integrity Rules

## Chapter 18 — Lifecycle Constraints

## Chapter 19 — Engineering Rules

## Chapter 20 — Architecture Decision Records

---

# Chapter 1 — Platform Constraints

## 1.1 Purpose

The Platform Domain contains globally shared reference data and foundational entities used throughout the FluxDine ecosystem.

Unlike tenant-owned entities, Platform entities are owned by the FluxDine platform itself and are shared across all tenants. Their constraints establish the foundational integrity rules upon which every other business domain depends.

The Platform Domain is responsible for ensuring the consistency, uniqueness, and immutability of global reference data.

---

## 1.2 Platform Constraint Overview

The Platform Domain defines constraints for:

- Countries
- Currencies
- Languages
- Time Zones
- Subscription Plans
- Features
- Global Roles
- Global Permissions
- Email Templates
- Notification Templates

---

# Platform Constraint Specifications

---

# Constraint P-001

## Constraint Name

Country Primary Key

---

## Purpose

Guarantees the unique identity of every country record.

---

## Constraint Type

Primary Key

---

## Business Domain

Platform

---

## Table

countries

---

## Columns

country_id

---

## Required

Yes

---

## Enforcement Rule

Every country shall have one unique immutable identifier.

---

## Failure Behaviour

Reject insertion if duplicate identifier exists.

---

## Engineering Notes

Country identifiers shall never change after creation.

---

# Constraint P-002

## Constraint Name

Country ISO Code Uniqueness

---

## Purpose

Ensures each country has one globally unique ISO country code.

---

## Constraint Type

Unique

---

## Business Domain

Platform

---

## Table

countries

---

## Columns

iso_code

---

## Required

Yes

---

## Enforcement Rule

No two countries may share the same ISO code.

---

## Failure Behaviour

Reject duplicate ISO codes.

---

## Engineering Notes

ISO-3166 Alpha-2 codes shall be used throughout the platform.

---

# Constraint P-003

## Constraint Name

Currency Code Uniqueness

---

## Purpose

Prevents duplicate currency definitions.

---

## Constraint Type

Unique

---

## Business Domain

Platform

---

## Table

currencies

---

## Columns

currency_code

---

## Required

Yes

---

## Enforcement Rule

Each ISO-4217 currency code shall be unique.

---

## Failure Behaviour

Reject duplicate currency codes.

---

## Engineering Notes

Currency definitions are platform-managed and immutable.

---

# Constraint P-004

## Constraint Name

Language Code Uniqueness

---

## Purpose

Ensures each supported language has a unique identifier.

---

## Constraint Type

Unique

---

## Business Domain

Platform

---

## Table

languages

---

## Columns

language_code

---

## Required

Yes

---

## Enforcement Rule

Each language code shall be globally unique.

---

## Failure Behaviour

Reject duplicate language codes.

---

## Engineering Notes

Language codes shall follow ISO-639 standards.

---

# Constraint P-005

## Constraint Name

Timezone Identifier Uniqueness

---

## Purpose

Ensures every timezone is uniquely identifiable.

---

## Constraint Type

Unique

---

## Business Domain

Platform

---

## Table

timezones

---

## Columns

timezone_name

---

## Required

Yes

---

## Enforcement Rule

Timezone names shall follow the IANA Time Zone Database.

---

## Failure Behaviour

Reject duplicate timezone identifiers.

---

## Engineering Notes

Examples include:

- Asia/Karachi
- Europe/London
- America/New_York

---

# Constraint P-006

## Constraint Name

Subscription Plan Name Uniqueness

---

## Purpose

Prevents duplicate subscription plan names.

---

## Constraint Type

Unique

---

## Business Domain

Platform

---

## Table

subscription_plans

---

## Columns

plan_name

---

## Required

Yes

---

## Enforcement Rule

Every subscription plan shall have a unique internal name.

---

## Failure Behaviour

Reject duplicate plan names.

---

## Engineering Notes

This constraint governs internal identifiers only. Commercial naming is defined separately.

---

# Constraint P-007

## Constraint Name

Feature Identifier Uniqueness

---

## Purpose

Ensures every platform feature is uniquely identifiable.

---

## Constraint Type

Unique

---

## Business Domain

Platform

---

## Table

features

---

## Columns

feature_key

---

## Required

Yes

---

## Enforcement Rule

Feature keys shall be globally unique.

---

## Failure Behaviour

Reject duplicate feature keys.

---

## Engineering Notes

Feature keys are permanent engineering identifiers and shall never be reused.

---

# Constraint P-008

## Constraint Name

Global Role Name Uniqueness

---

## Purpose

Ensures every global role has a unique name.

---

## Constraint Type

Unique

---

## Business Domain

Platform

---

## Table

global_roles

---

## Columns

role_name

---

## Required

Yes

---

## Enforcement Rule

Role names shall be unique across the platform.

---

## Failure Behaviour

Reject duplicate role names.

---

## Engineering Notes

Roles define authorization templates and shall remain immutable once referenced.

---

# Constraint P-009

## Constraint Name

Permission Key Uniqueness

---

## Purpose

Ensures every permission is uniquely identifiable.

---

## Constraint Type

Unique

---

## Business Domain

Platform

---

## Table

global_permissions

---

## Columns

permission_key

---

## Required

Yes

---

## Enforcement Rule

Permission keys shall be globally unique.

---

## Failure Behaviour

Reject duplicate permission keys.

---

## Engineering Notes

Permission keys are stable engineering identifiers used throughout authorization services.

---

# Constraint P-010

## Constraint Name

Notification Template Identifier

---

## Purpose

Ensures every notification template has a unique identifier.

---

## Constraint Type

Unique

---

## Business Domain

Platform

---

## Table

notification_templates

---

## Columns

template_key

---

## Required

Yes

---

## Enforcement Rule

Template keys shall be unique across the platform.

---

## Failure Behaviour

Reject duplicate template identifiers.

---

## Engineering Notes

Templates may evolve in content, but their engineering identifiers remain constant.

---

# Chapter 2 — Identity Constraints

## 2.1 Purpose

The Identity Domain governs platform authentication, authorization, and user identity.

Constraints within this domain ensure secure authentication, unique identities, role integrity, and controlled access to tenant resources.

---

## 2.2 Identity Constraint Overview

The Identity Domain defines constraints for:

- Users
- Sessions
- User Memberships
- Authentication Providers
- MFA Configurations
- Password Reset Tokens
- Email Verification Tokens
- Roles
- Permissions

---

# Identity Constraint Specifications

---

# Constraint I-001

## Constraint Name

User Email Uniqueness

---

## Purpose

Ensures every platform user has one unique email address.

---

## Constraint Type

Unique

---

## Business Domain

Identity

---

## Table

users

---

## Columns

email

---

## Required

Yes

---

## Enforcement Rule

A single email address may be associated with only one user account.

---

## Failure Behaviour

Reject duplicate email addresses.

---

## Engineering Notes

Email addresses shall be normalized before uniqueness evaluation.

---

# Constraint I-002

## Constraint Name

User Primary Key

---

## Purpose

Provides a unique immutable identifier for every platform user.

---

## Constraint Type

Primary Key

---

## Business Domain

Identity

---

## Table

users

---

## Columns

user_id

---

## Required

Yes

---

## Enforcement Rule

Every user shall possess one immutable identifier.

---

## Failure Behaviour

Reject duplicate identifiers.

---

## Engineering Notes

User identifiers shall never be modified after creation.

---

# Constraint I-003

## Constraint Name

Session Foreign Key

---

## Purpose

Associates authentication sessions with valid users.

---

## Constraint Type

Foreign Key

---

## Business Domain

Identity

---

## Table

sessions

---

## Columns

user_id

---

## Referenced Table

users

---

## Referenced Columns

user_id

---

## Required

Yes

---

## Enforcement Rule

Every session shall belong to an existing user.

---

## Failure Behaviour

Reject sessions referencing nonexistent users.

---

## Engineering Notes

Deleting a user shall invalidate all active sessions.

---

# Constraint I-004

## Constraint Name

Authentication Provider Uniqueness

---

## Purpose

Prevents duplicate authentication provider registrations for the same user.

---

## Constraint Type

Composite Unique

---

## Business Domain

Identity

---

## Table

authentication_providers

---

## Columns

user_id

provider_name

provider_account_id

---

## Required

Yes

---

## Enforcement Rule

A user may register a provider account only once.

---

## Failure Behaviour

Reject duplicate provider registrations.

---

## Engineering Notes

Supports OAuth, OpenID Connect, SAML, and future authentication mechanisms.

---

# Constraint I-005

## Constraint Name

Single Active MFA Configuration

---

## Purpose

Ensures a user has only one active multi-factor authentication configuration per authentication method.

---

## Constraint Type

Composite Unique

---

## Business Domain

Identity

---

## Table

mfa_configurations

---

## Columns

user_id

mfa_method

---

## Required

No

---

## Enforcement Rule

Only one active MFA configuration may exist for a specific authentication method.

---

## Failure Behaviour

Reject duplicate active MFA configurations.

---

## Engineering Notes

Future versions may allow multiple devices under one method while preserving a single active configuration.

---

# Chapter 2 Summary

The Platform and Identity constraints establish the foundational integrity model for the entire FluxDine platform. They ensure that global reference data remains unique and immutable while protecting user identities, authentication records, authorization structures, and security-sensitive information. All subsequent business domains inherit and depend upon the integrity guarantees established within these foundational constraints.

# Chapter 3 — Tenant Constraints

## 3.1 Purpose

The Tenant Domain establishes the integrity rules governing tenant-owned resources within the FluxDine SaaS platform.

Every customer organization onboarded to the platform is represented by a Tenant. All operational entities ultimately inherit ownership from a tenant. Therefore, constraints within this domain primarily protect:

- Tenant uniqueness
- Tenant ownership
- SaaS isolation
- Branding
- Domain ownership
- Feature assignments
- API security
- Payment gateway configuration
- Subscription linkage

These constraints form the foundation of FluxDine's multi-tenant architecture.

---

## 3.2 Tenant Constraint Overview

The Tenant Domain defines constraints for:

- Tenants
- Tenant Settings
- Tenant Domains
- Tenant Branding
- Tenant Features
- Tenant Payment Gateways
- Tenant Integrations
- Tenant Subscriptions
- Tenant Usage Metrics
- Tenant API Keys
- Tenant Audit Settings
- Tenant Invitations

---

# Tenant Constraint Specifications

---

# Constraint T-001

## Constraint Name

Tenant Primary Key

---

## Purpose

Guarantees the unique identity of every tenant.

---

## Constraint Type

Primary Key

---

## Business Domain

Tenant

---

## Table

tenants

---

## Columns

tenant_id

---

## Required

Yes

---

## Enforcement Rule

Every tenant shall possess one immutable unique identifier.

---

## Failure Behaviour

Reject duplicate tenant identifiers.

---

## Engineering Notes

Tenant identifiers shall never change after creation.

---

# Constraint T-002

## Constraint Name

Tenant Slug Uniqueness

---

## Purpose

Ensures every tenant has a globally unique slug.

---

## Constraint Type

Unique

---

## Business Domain

Tenant

---

## Table

tenants

---

## Columns

tenant_slug

---

## Required

Yes

---

## Enforcement Rule

No two tenants may share the same slug.

---

## Failure Behaviour

Reject duplicate tenant slugs.

---

## Engineering Notes

Tenant slugs are used in routing, APIs, and internal references.

---

# Constraint T-003

## Constraint Name

Tenant Settings Foreign Key

---

## Purpose

Associates settings with an existing tenant.

---

## Constraint Type

Foreign Key

---

## Business Domain

Tenant

---

## Table

tenant_settings

---

## Columns

tenant_id

---

## Referenced Table

tenants

---

## Referenced Columns

tenant_id

---

## Required

Yes

---

## Enforcement Rule

Every settings record shall reference a valid tenant.

---

## Failure Behaviour

Reject orphan settings records.

---

## Engineering Notes

Each tenant owns exactly one settings record.

---

# Constraint T-004

## Constraint Name

Single Tenant Settings Record

---

## Purpose

Prevents multiple settings records for the same tenant.

---

## Constraint Type

Unique

---

## Business Domain

Tenant

---

## Table

tenant_settings

---

## Columns

tenant_id

---

## Required

Yes

---

## Enforcement Rule

A tenant may have only one settings record.

---

## Failure Behaviour

Reject duplicate tenant settings.

---

## Engineering Notes

Implements the one-to-one relationship defined in the Relationships specification.

---

# Constraint T-005

## Constraint Name

Tenant Domain Uniqueness

---

## Purpose

Ensures that every custom domain belongs to only one tenant.

---

## Constraint Type

Unique

---

## Business Domain

Tenant

---

## Table

tenant_domains

---

## Columns

domain_name

---

## Required

Yes

---

## Enforcement Rule

A custom domain shall not be assigned to multiple tenants.

---

## Failure Behaviour

Reject duplicate domain assignments.

---

## Engineering Notes

Prevents cross-tenant domain conflicts.

---

# Constraint T-006

## Constraint Name

Tenant Branding Foreign Key

---

## Purpose

Associates branding with a valid tenant.

---

## Constraint Type

Foreign Key

---

## Business Domain

Tenant

---

## Table

tenant_branding

---

## Columns

tenant_id

---

## Referenced Table

tenants

---

## Referenced Columns

tenant_id

---

## Required

Yes

---

## Enforcement Rule

Branding records shall reference an existing tenant.

---

## Failure Behaviour

Reject orphan branding records.

---

## Engineering Notes

Branding is tenant-owned and isolated.

---

# Constraint T-007

## Constraint Name

Tenant Feature Assignment Integrity

---

## Purpose

Ensures feature assignments reference both a valid tenant and a valid platform feature.

---

## Constraint Type

Composite Foreign Key

---

## Business Domain

Tenant

---

## Table

tenant_features

---

## Columns

tenant_id

feature_id

---

## Referenced Tables

tenants

features

---

## Referenced Columns

tenant_id

feature_id

---

## Required

Yes

---

## Enforcement Rule

Every tenant feature assignment shall reference an existing tenant and an existing feature.

---

## Failure Behaviour

Reject invalid feature assignments.

---

## Engineering Notes

Feature overrides supplement subscription entitlements.

---

# Constraint T-008

## Constraint Name

Tenant Payment Gateway Integrity

---

## Purpose

Ensures every configured payment gateway belongs to an existing tenant.

---

## Constraint Type

Foreign Key

---

## Business Domain

Tenant

---

## Table

tenant_payment_gateways

---

## Columns

tenant_id

---

## Referenced Table

tenants

---

## Referenced Columns

tenant_id

---

## Required

Yes

---

## Enforcement Rule

Payment gateway configurations shall reference a valid tenant.

---

## Failure Behaviour

Reject orphan gateway configurations.

---

## Engineering Notes

Supports multiple gateway configurations per tenant while preserving ownership.

---

# Constraint T-009

## Constraint Name

Tenant API Key Uniqueness

---

## Purpose

Ensures every API key is globally unique.

---

## Constraint Type

Unique

---

## Business Domain

Tenant

---

## Table

tenant_api_keys

---

## Columns

api_key

---

## Required

Yes

---

## Enforcement Rule

API keys shall never be duplicated.

---

## Failure Behaviour

Reject duplicate API keys.

---

## Engineering Notes

API keys are security credentials and shall be cryptographically generated.

---

# Constraint T-010

## Constraint Name

Tenant Invitation Email Uniqueness

---

## Purpose

Prevents duplicate active invitations for the same email within a tenant.

---

## Constraint Type

Composite Unique

---

## Business Domain

Tenant

---

## Table

tenant_invitations

---

## Columns

tenant_id

email

status

---

## Required

Yes

---

## Enforcement Rule

Only one active invitation may exist for a specific email address within the same tenant.

---

## Failure Behaviour

Reject duplicate active invitations.

---

## Engineering Notes

Expired or revoked invitations do not prevent issuing a new invitation.

---

# Chapter 3 Summary

The Tenant constraints establish the integrity rules that enforce FluxDine's multi-tenant architecture. They ensure every tenant-owned resource maintains a valid ownership chain, prevent cross-tenant conflicts, enforce one-to-one and one-to-many relationships, and protect critical business identifiers such as domains, API keys, and feature assignments. These constraints collectively provide the foundation for secure tenant isolation and SaaS scalability.

# Chapter 4 — Restaurant Constraints

## 4.1 Purpose

The Restaurant Domain governs the operational integrity of restaurants and their physical branches within the FluxDine platform.

While the Tenant Domain establishes ownership, the Restaurant Domain ensures that operational resources remain consistent, correctly associated, and structurally valid throughout their lifecycle.

These constraints protect:

- Restaurant ownership
- Branch hierarchy
- Business hours
- Delivery zones
- Seating areas
- Restaurant tables
- Kitchen stations
- Staff assignments
- Rider assignments
- Menu ownership

---

## 4.2 Restaurant Constraint Overview

The Restaurant Domain defines constraints for:

- Restaurants
- Restaurant Settings
- Branches
- Business Hours
- Delivery Zones
- Seating Areas
- Restaurant Tables
- Kitchen Stations
- Staff
- Riders
- Rider Assignments
- Menus

---

# Restaurant Constraint Specifications

---

# Constraint R-001

## Constraint Name

Restaurant Primary Key

---

## Purpose

Guarantees the unique identity of every restaurant.

---

## Constraint Type

Primary Key

---

## Business Domain

Restaurant

---

## Table

restaurants

---

## Columns

restaurant_id

---

## Required

Yes

---

## Enforcement Rule

Every restaurant shall possess one immutable unique identifier.

---

## Failure Behaviour

Reject duplicate restaurant identifiers.

---

## Engineering Notes

Restaurant identifiers shall never change after creation.

---

# Constraint R-002

## Constraint Name

Restaurant Tenant Ownership

---

## Purpose

Ensures every restaurant belongs to an existing tenant.

---

## Constraint Type

Foreign Key

---

## Business Domain

Restaurant

---

## Table

restaurants

---

## Columns

tenant_id

---

## Referenced Table

tenants

---

## Referenced Columns

tenant_id

---

## Required

Yes

---

## Enforcement Rule

A restaurant cannot exist without a valid tenant.

---

## Failure Behaviour

Reject orphan restaurant records.

---

## Engineering Notes

This establishes the root ownership chain for all restaurant operations.

---

# Constraint R-003

## Constraint Name

Restaurant Settings Uniqueness

---

## Purpose

Ensures each restaurant has only one settings record.

---

## Constraint Type

Unique

---

## Business Domain

Restaurant

---

## Table

restaurant_settings

---

## Columns

restaurant_id

---

## Required

Yes

---

## Enforcement Rule

One restaurant may have only one settings record.

---

## Failure Behaviour

Reject duplicate settings.

---

## Engineering Notes

Implements the one-to-one relationship defined in the Relationships specification.

---

# Constraint R-004

## Constraint Name

Branch Restaurant Ownership

---

## Purpose

Ensures every branch belongs to an existing restaurant.

---

## Constraint Type

Foreign Key

---

## Business Domain

Restaurant

---

## Table

branches

---

## Columns

restaurant_id

---

## Referenced Table

restaurants

---

## Referenced Columns

restaurant_id

---

## Required

Yes

---

## Enforcement Rule

Every branch shall reference a valid restaurant.

---

## Failure Behaviour

Reject orphan branch records.

---

## Engineering Notes

Branches inherit ownership through the restaurant.

---

# Constraint R-005

## Constraint Name

Branch Code Uniqueness

---

## Purpose

Prevents duplicate branch codes within the same restaurant.

---

## Constraint Type

Composite Unique

---

## Business Domain

Restaurant

---

## Table

branches

---

## Columns

restaurant_id

branch_code

---

## Required

Yes

---

## Enforcement Rule

A branch code shall be unique within its restaurant.

---

## Failure Behaviour

Reject duplicate branch codes.

---

## Engineering Notes

Different restaurants may reuse identical branch codes.

---

# Constraint R-006

## Constraint Name

Business Hours Branch Integrity

---

## Purpose

Ensures business hours belong to an existing branch.

---

## Constraint Type

Foreign Key

---

## Business Domain

Restaurant

---

## Table

business_hours

---

## Columns

branch_id

---

## Referenced Table

branches

---

## Referenced Columns

branch_id

---

## Required

Yes

---

## Enforcement Rule

Business hours shall reference a valid branch.

---

## Failure Behaviour

Reject orphan schedules.

---

## Engineering Notes

Business hours are branch-specific.

---

# Constraint R-007

## Constraint Name

Delivery Zone Integrity

---

## Purpose

Ensures delivery zones belong to an existing branch.

---

## Constraint Type

Foreign Key

---

## Business Domain

Restaurant

---

## Table

delivery_zones

---

## Columns

branch_id

---

## Referenced Table

branches

---

## Referenced Columns

branch_id

---

## Required

Yes

---

## Enforcement Rule

Every delivery zone shall reference a valid branch.

---

## Failure Behaviour

Reject orphan delivery zones.

---

## Engineering Notes

Delivery zones cannot be shared between branches.

---

# Constraint R-008

## Constraint Name

Seating Area Branch Integrity

---

## Purpose

Ensures seating areas belong to an existing branch.

---

## Constraint Type

Foreign Key

---

## Business Domain

Restaurant

---

## Table

seating_areas

---

## Columns

branch_id

---

## Referenced Table

branches

---

## Referenced Columns

branch_id

---

## Required

Yes

---

## Enforcement Rule

Every seating area shall reference a valid branch.

---

## Failure Behaviour

Reject orphan seating areas.

---

## Engineering Notes

Each seating area belongs exclusively to one branch.

---

# Constraint R-009

## Constraint Name

Restaurant Table Number Uniqueness

---

## Purpose

Prevents duplicate table numbers within the same seating area.

---

## Constraint Type

Composite Unique

---

## Business Domain

Restaurant

---

## Table

restaurant_tables

---

## Columns

seating_area_id

table_number

---

## Required

Yes

---

## Enforcement Rule

Table numbers shall be unique within a seating area.

---

## Failure Behaviour

Reject duplicate table numbers.

---

## Engineering Notes

Different seating areas may reuse identical table numbers.

---

# Constraint R-010

## Constraint Name

Kitchen Station Branch Integrity

---

## Purpose

Ensures kitchen stations belong to an existing branch.

---

## Constraint Type

Foreign Key

---

## Business Domain

Restaurant

---

## Table

kitchen_stations

---

## Columns

branch_id

---

## Referenced Table

branches

---

## Referenced Columns

branch_id

---

## Required

Yes

---

## Enforcement Rule

Kitchen stations shall reference a valid branch.

---

## Failure Behaviour

Reject orphan kitchen stations.

---

## Engineering Notes

Supports future Kitchen Display System (KDS) integration.

---

# Constraint R-011

## Constraint Name

Staff Branch Integrity

---

## Purpose

Ensures staff members belong to an existing branch.

---

## Constraint Type

Foreign Key

---

## Business Domain

Restaurant

---

## Table

staff

---

## Columns

branch_id

---

## Referenced Table

branches

---

## Referenced Columns

branch_id

---

## Required

Yes

---

## Enforcement Rule

Staff shall always be assigned to a valid branch.

---

## Failure Behaviour

Reject orphan staff records.

---

## Engineering Notes

Authentication is managed separately by the Identity Domain.

---

# Constraint R-012

## Constraint Name

Rider Branch Integrity

---

## Purpose

Ensures riders belong to an existing branch.

---

## Constraint Type

Foreign Key

---

## Business Domain

Restaurant

---

## Table

riders

---

## Columns

branch_id

---

## Referenced Table

branches

---

## Referenced Columns

branch_id

---

## Required

Yes

---

## Enforcement Rule

Every rider shall reference a valid branch.

---

## Failure Behaviour

Reject orphan rider records.

---

## Engineering Notes

Supports future rider transfer workflows while maintaining ownership.

---

# Constraint R-013

## Constraint Name

Rider Assignment Integrity

---

## Purpose

Ensures rider assignments reference valid riders and branches.

---

## Constraint Type

Composite Foreign Key

---

## Business Domain

Restaurant

---

## Table

rider_assignments

---

## Columns

rider_id

branch_id

---

## Referenced Tables

riders

branches

---

## Referenced Columns

rider_id

branch_id

---

## Required

Yes

---

## Enforcement Rule

Every assignment shall reference an existing rider and an existing branch.

---

## Failure Behaviour

Reject invalid assignments.

---

## Engineering Notes

Assignment history shall remain immutable after completion.

---

# Constraint R-014

## Constraint Name

Menu Restaurant Ownership

---

## Purpose

Ensures every menu belongs to an existing restaurant.

---

## Constraint Type

Foreign Key

---

## Business Domain

Restaurant

---

## Table

menus

---

## Columns

restaurant_id

---

## Referenced Table

restaurants

---

## Referenced Columns

restaurant_id

---

## Required

Yes

---

## Enforcement Rule

Menus shall reference a valid restaurant.

---

## Failure Behaviour

Reject orphan menus.

---

## Engineering Notes

Menus inherit ownership from the restaurant and may later be associated with one or more branches.

---

# Chapter 4 Summary

The Restaurant constraints establish the integrity rules governing restaurant operations throughout the FluxDine platform. They ensure that every operational entity—including branches, schedules, delivery zones, seating infrastructure, staff, riders, and menus—maintains valid ownership, correct hierarchical relationships, and consistent business identifiers. These constraints preserve operational integrity while supporting scalable multi-branch restaurant management.

# Chapter 5 — Commerce Constraints

## 5.1 Purpose

The Commerce Domain governs the integrity of customer ordering operations throughout the FluxDine platform.

These constraints ensure that products, menus, shopping carts, orders, and purchased items remain logically consistent, historically accurate, and fully traceable.

The Commerce Domain is responsible for protecting:

- Menu organization
- Product hierarchy
- Product configuration
- Customer carts
- Checkout integrity
- Order processing
- Order history
- Historical pricing
- Product availability

---

## 5.2 Commerce Constraint Overview

The Commerce Domain defines constraints for:

- Menus
- Menu Categories
- Menu Items
- Item Variants
- Item Options
- Item Add-ons
- Carts
- Cart Items
- Orders
- Order Items

---

# Commerce Constraint Specifications

---

# Constraint C-001

## Constraint Name

Menu Category Ownership

---

## Purpose

Ensures every menu category belongs to an existing menu.

---

## Constraint Type

Foreign Key

---

## Business Domain

Commerce

---

## Table

menu_categories

---

## Columns

menu_id

---

## Referenced Table

menus

---

## Referenced Columns

menu_id

---

## Required

Yes

---

## Enforcement Rule

Every category shall reference a valid menu.

---

## Failure Behaviour

Reject orphan categories.

---

## Engineering Notes

Categories inherit restaurant ownership through their parent menu.

---

# Constraint C-002

## Constraint Name

Menu Item Category Integrity

---

## Purpose

Ensures every menu item belongs to a valid category.

---

## Constraint Type

Foreign Key

---

## Business Domain

Commerce

---

## Table

menu_items

---

## Columns

category_id

---

## Referenced Table

menu_categories

---

## Referenced Columns

category_id

---

## Required

Yes

---

## Enforcement Rule

Every menu item shall reference an existing category.

---

## Failure Behaviour

Reject orphan menu items.

---

## Engineering Notes

Menu items cannot exist independently of a category.

---

# Constraint C-003

## Constraint Name

Menu Item SKU Uniqueness

---

## Purpose

Prevents duplicate product identifiers within the same restaurant.

---

## Constraint Type

Composite Unique

---

## Business Domain

Commerce

---

## Table

menu_items

---

## Columns

restaurant_id

sku

---

## Required

No

---

## Enforcement Rule

When used, a SKU shall be unique within the owning restaurant.

---

## Failure Behaviour

Reject duplicate SKUs.

---

## Engineering Notes

SKU values are optional but must be unique if assigned.

---

# Constraint C-004

## Constraint Name

Item Variant Integrity

---

## Purpose

Ensures every product variant belongs to an existing menu item.

---

## Constraint Type

Foreign Key

---

## Business Domain

Commerce

---

## Table

item_variants

---

## Columns

menu_item_id

---

## Referenced Table

menu_items

---

## Referenced Columns

menu_item_id

---

## Required

Yes

---

## Enforcement Rule

Variants shall reference a valid parent product.

---

## Failure Behaviour

Reject orphan variants.

---

## Engineering Notes

Variants inherit pricing and ownership from the parent item.

---

# Constraint C-005

## Constraint Name

Item Option Integrity

---

## Purpose

Ensures configurable options belong to an existing menu item.

---

## Constraint Type

Foreign Key

---

## Business Domain

Commerce

---

## Table

item_options

---

## Columns

menu_item_id

---

## Referenced Table

menu_items

---

## Referenced Columns

menu_item_id

---

## Required

Yes

---

## Enforcement Rule

Options shall reference an existing menu item.

---

## Failure Behaviour

Reject orphan options.

---

## Engineering Notes

Options may be required or optional depending on business configuration.

---

# Constraint C-006

## Constraint Name

Item Add-on Integrity

---

## Purpose

Ensures add-ons belong to an existing menu item.

---

## Constraint Type

Foreign Key

---

## Business Domain

Commerce

---

## Table

item_addons

---

## Columns

menu_item_id

---

## Referenced Table

menu_items

---

## Referenced Columns

menu_item_id

---

## Required

Yes

---

## Enforcement Rule

Every add-on shall reference a valid menu item.

---

## Failure Behaviour

Reject orphan add-ons.

---

## Engineering Notes

Add-ons inherit ownership from the parent product.

---

# Constraint C-007

## Constraint Name

Cart Customer Integrity

---

## Purpose

Ensures every shopping cart belongs to an existing customer.

---

## Constraint Type

Foreign Key

---

## Business Domain

Commerce

---

## Table

carts

---

## Columns

customer_id

---

## Referenced Table

customers

---

## Referenced Columns

customer_id

---

## Required

Yes

---

## Enforcement Rule

Every cart shall belong to a valid customer.

---

## Failure Behaviour

Reject orphan carts.

---

## Engineering Notes

Guest checkout implementations may create temporary customer records.

---

# Constraint C-008

## Constraint Name

Single Active Cart

---

## Purpose

Prevents multiple active shopping carts for the same customer within the same restaurant.

---

## Constraint Type

Composite Unique

---

## Business Domain

Commerce

---

## Table

carts

---

## Columns

customer_id

restaurant_id

cart_status

---

## Required

Yes

---

## Enforcement Rule

Only one active cart may exist for a customer in a restaurant.

---

## Failure Behaviour

Reject creation of another active cart.

---

## Engineering Notes

Completed or expired carts are excluded from this constraint.

---

# Constraint C-009

## Constraint Name

Cart Item Integrity

---

## Purpose

Ensures cart items belong to both a valid cart and a valid menu item.

---

## Constraint Type

Composite Foreign Key

---

## Business Domain

Commerce

---

## Table

cart_items

---

## Columns

cart_id

menu_item_id

---

## Referenced Tables

carts

menu_items

---

## Referenced Columns

cart_id

menu_item_id

---

## Required

Yes

---

## Enforcement Rule

Cart items shall reference an existing cart and product.

---

## Failure Behaviour

Reject invalid cart items.

---

## Engineering Notes

Product availability shall also be validated during checkout.

---

# Constraint C-010

## Constraint Name

Order Restaurant Integrity

---

## Purpose

Ensures every order belongs to an existing restaurant.

---

## Constraint Type

Foreign Key

---

## Business Domain

Commerce

---

## Table

orders

---

## Columns

restaurant_id

---

## Referenced Table

restaurants

---

## Referenced Columns

restaurant_id

---

## Required

Yes

---

## Enforcement Rule

Orders shall reference a valid restaurant.

---

## Failure Behaviour

Reject orphan orders.

---

## Engineering Notes

Restaurant ownership is immutable after order creation.

---

# Constraint C-011

## Constraint Name

Order Customer Integrity

---

## Purpose

Ensures every order belongs to an existing customer.

---

## Constraint Type

Foreign Key

---

## Business Domain

Commerce

---

## Table

orders

---

## Columns

customer_id

---

## Referenced Table

customers

---

## Referenced Columns

customer_id

---

## Required

Yes

---

## Enforcement Rule

Orders shall reference a valid customer.

---

## Failure Behaviour

Reject orphan orders.

---

## Engineering Notes

Historical customer deletion shall never invalidate completed orders.

---

# Constraint C-012

## Constraint Name

Order Item Integrity

---

## Purpose

Ensures every purchased item belongs to a valid order.

---

## Constraint Type

Foreign Key

---

## Business Domain

Commerce

---

## Table

order_items

---

## Columns

order_id

---

## Referenced Table

orders

---

## Referenced Columns

order_id

---

## Required

Yes

---

## Enforcement Rule

Order items shall reference an existing order.

---

## Failure Behaviour

Reject orphan order items.

---

## Engineering Notes

Order items become immutable after order confirmation.

---

# Chapter 6 — Customer Constraints

## 6.1 Purpose

The Customer Domain governs the integrity of customer profiles, preferences, addresses, loyalty, and communication settings.

These constraints ensure customer information remains accurate while supporting personalization, repeat business, and long-term customer relationships.

---

## 6.2 Customer Constraint Overview

The Customer Domain defines constraints for:

- Customers
- Customer Addresses
- Customer Preferences
- Customer Favorites
- Loyalty Accounts
- Loyalty Transactions
- Reward Points
- Customer Devices
- Communication Preferences

---

# Customer Constraint Specifications

---

# Constraint CU-001

## Constraint Name

Customer Primary Key

---

## Purpose

Guarantees the unique identity of every customer.

---

## Constraint Type

Primary Key

---

## Business Domain

Customer

---

## Table

customers

---

## Columns

customer_id

---

## Required

Yes

---

## Enforcement Rule

Every customer shall possess one immutable identifier.

---

## Failure Behaviour

Reject duplicate identifiers.

---

## Engineering Notes

Customer identity persists across orders and reservations.

---

# Constraint CU-002

## Constraint Name

Customer Email Uniqueness Per Tenant

---

## Purpose

Prevents duplicate customer accounts within the same tenant.

---

## Constraint Type

Composite Unique

---

## Business Domain

Customer

---

## Table

customers

---

## Columns

tenant_id

email

---

## Required

No

---

## Enforcement Rule

If an email address is provided, it shall be unique within the owning tenant.

---

## Failure Behaviour

Reject duplicate customer email addresses within the same tenant.

---

## Engineering Notes

Different tenants may register customers using the same email address.

---

# Constraint CU-003

## Constraint Name

Customer Address Integrity

---

## Purpose

Ensures addresses belong to existing customers.

---

## Constraint Type

Foreign Key

---

## Business Domain

Customer

---

## Table

customer_addresses

---

## Columns

customer_id

---

## Referenced Table

customers

---

## Referenced Columns

customer_id

---

## Required

Yes

---

## Enforcement Rule

Every address shall reference a valid customer.

---

## Failure Behaviour

Reject orphan addresses.

---

## Engineering Notes

Customers may own multiple addresses.

---

# Constraint CU-004

## Constraint Name

Single Customer Preferences Record

---

## Purpose

Ensures every customer has at most one preference profile.

---

## Constraint Type

Unique

---

## Business Domain

Customer

---

## Table

customer_preferences

---

## Columns

customer_id

---

## Required

Yes

---

## Enforcement Rule

Only one preference record may exist per customer.

---

## Failure Behaviour

Reject duplicate preference records.

---

## Engineering Notes

Implements the one-to-one relationship defined in the Relationships specification.

---

# Constraint CU-005

## Constraint Name

Loyalty Account Uniqueness

---

## Purpose

Ensures customers possess at most one loyalty account.

---

## Constraint Type

Unique

---

## Business Domain

Customer

---

## Table

loyalty_accounts

---

## Columns

customer_id

---

## Required

No

---

## Enforcement Rule

Only one loyalty account may exist per customer.

---

## Failure Behaviour

Reject duplicate loyalty accounts.

---

## Engineering Notes

Participation in loyalty programs remains optional.

---

# Constraint CU-006

## Constraint Name

Communication Preferences Integrity

---

## Purpose

Ensures communication preferences belong to existing customers.

---

## Constraint Type

Foreign Key

---

## Business Domain

Customer

---

## Table

communication_preferences

---

## Columns

customer_id

---

## Referenced Table

customers

---

## Referenced Columns

customer_id

---

## Required

Yes

---

## Enforcement Rule

Communication preferences shall reference a valid customer.

---

## Failure Behaviour

Reject orphan communication preference records.

---

## Engineering Notes

Supports compliance with privacy regulations and customer consent requirements.

---

# Chapter 5 Summary

The Commerce and Customer constraints establish the integrity rules governing online ordering and customer management within the FluxDine platform. These constraints ensure products, carts, orders, customer profiles, loyalty programs, and communication preferences remain structurally valid, historically accurate, and securely associated with their owning business entities. Together, they provide the foundation for reliable customer experiences and transaction processing.

# Chapter 7 — Reservation Constraints

## 7.1 Purpose

The Reservation Domain governs the integrity of restaurant reservations, seating assignments, reservation history, and waiting lists.

These constraints ensure reservations remain valid throughout their lifecycle while preserving historical seating information and preventing scheduling conflicts.

The Reservation Domain protects:

- Reservation ownership
- Table assignments
- Reservation lifecycle
- Waitlist integrity
- Seating allocation
- Reservation history

---

## 7.2 Reservation Constraint Overview

The Reservation Domain defines constraints for:

- Reservations
- Reservation Tables
- Reservation Events
- Waitlists

---

# Reservation Constraint Specifications

---

# Constraint RS-001

## Constraint Name

Reservation Customer Integrity

---

## Purpose

Ensures every reservation belongs to an existing customer.

---

## Constraint Type

Foreign Key

---

## Business Domain

Reservation

---

## Table

reservations

---

## Columns

customer_id

---

## Referenced Table

customers

---

## Referenced Columns

customer_id

---

## Required

Yes

---

## Enforcement Rule

Reservations shall reference an existing customer.

---

## Failure Behaviour

Reject orphan reservation records.

---

## Engineering Notes

Guest reservations shall be associated with a temporary or guest customer record.

---

# Constraint RS-002

## Constraint Name

Reservation Branch Integrity

---

## Purpose

Ensures every reservation belongs to a valid restaurant branch.

---

## Constraint Type

Foreign Key

---

## Business Domain

Reservation

---

## Table

reservations

---

## Columns

branch_id

---

## Referenced Table

branches

---

## Referenced Columns

branch_id

---

## Required

Yes

---

## Enforcement Rule

Reservations shall reference an existing branch.

---

## Failure Behaviour

Reject orphan reservations.

---

## Engineering Notes

Reservations cannot be transferred between branches.

---

# Constraint RS-003

## Constraint Name

Reservation Table Integrity

---

## Purpose

Ensures assigned tables exist before reservation assignment.

---

## Constraint Type

Composite Foreign Key

---

## Business Domain

Reservation

---

## Table

reservation_tables

---

## Columns

reservation_id

table_id

---

## Referenced Tables

reservations

restaurant_tables

---

## Referenced Columns

reservation_id

table_id

---

## Required

Yes

---

## Enforcement Rule

Every reservation table assignment shall reference an existing reservation and restaurant table.

---

## Failure Behaviour

Reject invalid assignments.

---

## Engineering Notes

Supports reservations spanning multiple tables.

---

# Constraint RS-004

## Constraint Name

Reservation Event Integrity

---

## Purpose

Ensures every reservation event belongs to an existing reservation.

---

## Constraint Type

Foreign Key

---

## Business Domain

Reservation

---

## Table

reservation_events

---

## Columns

reservation_id

---

## Referenced Table

reservations

---

## Referenced Columns

reservation_id

---

## Required

Yes

---

## Enforcement Rule

Reservation events shall reference an existing reservation.

---

## Failure Behaviour

Reject orphan reservation events.

---

## Engineering Notes

Reservation history is immutable.

---

# Constraint RS-005

## Constraint Name

Waitlist Branch Integrity

---

## Purpose

Ensures waitlist entries belong to an existing branch.

---

## Constraint Type

Foreign Key

---

## Business Domain

Reservation

---

## Table

waitlists

---

## Columns

branch_id

---

## Referenced Table

branches

---

## Referenced Columns

branch_id

---

## Required

Yes

---

## Enforcement Rule

Waitlist entries shall reference an existing branch.

---

## Failure Behaviour

Reject orphan waitlist records.

---

## Engineering Notes

Waitlists remain independent from confirmed reservations.

---

# Chapter 8 — Billing Constraints

## 8.1 Purpose

The Billing Domain governs subscription billing, invoices, credits, and refunds.

These constraints ensure financial records remain complete, auditable, and historically accurate.

---

## 8.2 Billing Constraint Overview

The Billing Domain defines constraints for:

- Billing Accounts
- Subscriptions
- Invoices
- Invoice Items
- Credit Notes
- Refund Requests

---

# Billing Constraint Specifications

---

# Constraint B-001

## Constraint Name

Billing Account Tenant Integrity

---

## Purpose

Ensures every billing account belongs to an existing tenant.

---

## Constraint Type

Foreign Key

---

## Business Domain

Billing

---

## Table

billing_accounts

---

## Columns

tenant_id

---

## Referenced Table

tenants

---

## Referenced Columns

tenant_id

---

## Required

Yes

---

## Enforcement Rule

Billing accounts shall reference valid tenants.

---

## Failure Behaviour

Reject orphan billing accounts.

---

## Engineering Notes

One billing account exists per tenant.

---

# Constraint B-002

## Constraint Name

Subscription Billing Account Integrity

---

## Purpose

Ensures subscriptions belong to valid billing accounts.

---

## Constraint Type

Foreign Key

---

## Business Domain

Billing

---

## Table

subscriptions

---

## Columns

billing_account_id

---

## Referenced Table

billing_accounts

---

## Referenced Columns

billing_account_id

---

## Required

Yes

---

## Enforcement Rule

Subscriptions shall reference existing billing accounts.

---

## Failure Behaviour

Reject orphan subscriptions.

---

## Engineering Notes

Historical subscriptions remain preserved after expiration.

---

# Constraint B-003

## Constraint Name

Invoice Subscription Integrity

---

## Purpose

Ensures invoices belong to existing subscriptions.

---

## Constraint Type

Foreign Key

---

## Business Domain

Billing

---

## Table

invoices

---

## Columns

subscription_id

---

## Referenced Table

subscriptions

---

## Referenced Columns

subscription_id

---

## Required

Yes

---

## Enforcement Rule

Invoices shall reference valid subscriptions.

---

## Failure Behaviour

Reject orphan invoices.

---

## Engineering Notes

Invoices are immutable financial records.

---

# Constraint B-004

## Constraint Name

Invoice Item Integrity

---

## Purpose

Ensures invoice items belong to existing invoices.

---

## Constraint Type

Foreign Key

---

## Business Domain

Billing

---

## Table

invoice_items

---

## Columns

invoice_id

---

## Referenced Table

invoices

---

## Referenced Columns

invoice_id

---

## Required

Yes

---

## Enforcement Rule

Invoice items shall reference valid invoices.

---

## Failure Behaviour

Reject orphan invoice items.

---

## Engineering Notes

Invoice totals shall equal the sum of invoice items.

---

# Constraint B-005

## Constraint Name

Credit Note Invoice Integrity

---

## Purpose

Ensures credit notes reference existing invoices.

---

## Constraint Type

Foreign Key

---

## Business Domain

Billing

---

## Table

credit_notes

---

## Columns

invoice_id

---

## Referenced Table

invoices

---

## Referenced Columns

invoice_id

---

## Required

Yes

---

## Enforcement Rule

Credit notes shall reference valid invoices.

---

## Failure Behaviour

Reject orphan credit notes.

---

## Engineering Notes

Credit notes preserve historical accounting records.

---

# Constraint B-006

## Constraint Name

Refund Request Invoice Integrity

---

## Purpose

Ensures refund requests belong to existing invoices.

---

## Constraint Type

Foreign Key

---

## Business Domain

Billing

---

## Table

refund_requests

---

## Columns

invoice_id

---

## Referenced Table

invoices

---

## Referenced Columns

invoice_id

---

## Required

Yes

---

## Enforcement Rule

Refund requests shall reference valid invoices.

---

## Failure Behaviour

Reject orphan refund requests.

---

## Engineering Notes

Refund approval workflow is enforced by the application layer.

---

# Chapter 9 — Payment Constraints

## 9.1 Purpose

The Payment Domain governs payment gateway abstraction, merchant accounts, payment transactions, settlements, reconciliation, and provider webhooks.

These constraints ensure payment processing remains provider-independent while preserving financial integrity.

---

## 9.2 Payment Constraint Overview

The Payment Domain defines constraints for:

- Payment Providers
- Gateway Configurations
- Merchant Accounts
- Payment Transactions
- Payment Intents
- Refund Transactions
- Settlement Records
- Reconciliation Records
- Webhook Events

---

# Payment Constraint Specifications

---

# Constraint PM-001

## Constraint Name

Gateway Configuration Integrity

---

## Purpose

Ensures gateway configurations reference valid payment providers.

---

## Constraint Type

Foreign Key

---

## Business Domain

Payment

---

## Table

gateway_configurations

---

## Columns

payment_provider_id

---

## Referenced Table

payment_providers

---

## Referenced Columns

payment_provider_id

---

## Required

Yes

---

## Enforcement Rule

Gateway configurations shall reference existing payment providers.

---

## Failure Behaviour

Reject orphan gateway configurations.

---

## Engineering Notes

Supports provider-independent payment abstraction.

---

# Constraint PM-002

## Constraint Name

Merchant Account Integrity

---

## Purpose

Ensures merchant accounts belong to valid gateway configurations.

---

## Constraint Type

Foreign Key

---

## Business Domain

Payment

---

## Table

merchant_accounts

---

## Columns

gateway_configuration_id

---

## Referenced Table

gateway_configurations

---

## Referenced Columns

gateway_configuration_id

---

## Required

Yes

---

## Enforcement Rule

Merchant accounts shall reference existing gateway configurations.

---

## Failure Behaviour

Reject orphan merchant accounts.

---

## Engineering Notes

Supports multiple merchant accounts per gateway configuration.

---

# Constraint PM-003

## Constraint Name

Payment Transaction Merchant Integrity

---

## Purpose

Ensures every payment transaction belongs to an existing merchant account.

---

## Constraint Type

Foreign Key

---

## Business Domain

Payment

---

## Table

payment_transactions

---

## Columns

merchant_account_id

---

## Referenced Table

merchant_accounts

---

## Referenced Columns

merchant_account_id

---

## Required

Yes

---

## Enforcement Rule

Payment transactions shall reference valid merchant accounts.

---

## Failure Behaviour

Reject orphan payment transactions.

---

## Engineering Notes

Transactions are immutable after successful authorization.

---

# Constraint PM-004

## Constraint Name

Payment Transaction External Reference

---

## Purpose

Prevents duplicate transaction identifiers returned by payment providers.

---

## Constraint Type

Unique

---

## Business Domain

Payment

---

## Table

payment_transactions

---

## Columns

provider_transaction_id

---

## Required

Yes

---

## Enforcement Rule

Each provider transaction identifier shall be globally unique.

---

## Failure Behaviour

Reject duplicate provider transaction identifiers.

---

## Engineering Notes

Ensures idempotent payment processing.

---

# Constraint PM-005

## Constraint Name

Webhook Event Idempotency

---

## Purpose

Prevents duplicate webhook processing.

---

## Constraint Type

Unique

---

## Business Domain

Payment

---

## Table

webhook_events

---

## Columns

provider_event_id

---

## Required

Yes

---

## Enforcement Rule

Each provider event shall be processed only once.

---

## Failure Behaviour

Reject duplicate webhook events.

---

## Engineering Notes

Critical for reliable asynchronous payment processing.

---

# Chapter 6 Summary

The Reservation, Billing, and Payment constraints establish the integrity rules governing reservations, subscription billing, and payment processing across the FluxDine platform. They ensure reservation history remains consistent, financial records remain immutable and auditable, and payment processing maintains provider independence while enforcing strict referential integrity and idempotent transaction handling.

# Chapter 10 — Notification Constraints

## 10.1 Purpose

The Notification Domain governs the integrity of all communications generated by the FluxDine platform.

Notifications are created by business events originating from multiple domains, including Commerce, Reservations, Billing, Authentication, and Platform Administration.

These constraints ensure notifications are reliable, traceable, idempotent, and historically preserved.

The Notification Domain protects:

- Notification ownership
- Template integrity
- Delivery records
- Queue processing
- Channel consistency
- Delivery history

---

## 10.2 Notification Constraint Overview

The Notification Domain defines constraints for:

- Notifications
- Notification Templates
- Email Messages
- SMS Messages
- Push Notifications
- Notification Queue

---

# Notification Constraint Specifications

---

# Constraint N-001

## Constraint Name

Notification Template Integrity

---

## Purpose

Ensures every notification references a valid template when template-based generation is used.

---

## Constraint Type

Foreign Key

---

## Business Domain

Notification

---

## Table

notifications

---

## Columns

notification_template_id

---

## Referenced Table

notification_templates

---

## Referenced Columns

notification_template_id

---

## Required

No

---

## Enforcement Rule

If a template is specified, it shall reference an existing notification template.

---

## Failure Behaviour

Reject invalid template references.

---

## Engineering Notes

Dynamic notifications may not require templates.

---

# Constraint N-002

## Constraint Name

Email Message Integrity

---

## Purpose

Ensures email messages belong to valid notifications.

---

## Constraint Type

Foreign Key

---

## Business Domain

Notification

---

## Table

email_messages

---

## Columns

notification_id

---

## Referenced Table

notifications

---

## Referenced Columns

notification_id

---

## Required

Yes

---

## Enforcement Rule

Email messages shall reference existing notifications.

---

## Failure Behaviour

Reject orphan email messages.

---

## Engineering Notes

Delivery status shall remain historically preserved.

---

# Constraint N-003

## Constraint Name

SMS Message Integrity

---

## Purpose

Ensures SMS messages belong to valid notifications.

---

## Constraint Type

Foreign Key

---

## Business Domain

Notification

---

## Table

sms_messages

---

## Columns

notification_id

---

## Referenced Table

notifications

---

## Referenced Columns

notification_id

---

## Required

Yes

---

## Enforcement Rule

SMS messages shall reference existing notifications.

---

## Failure Behaviour

Reject orphan SMS messages.

---

# Constraint N-004

## Constraint Name

Push Notification Integrity

---

## Purpose

Ensures push notifications belong to valid notification records.

---

## Constraint Type

Foreign Key

---

## Business Domain

Notification

---

## Table

push_notifications

---

## Columns

notification_id

---

## Referenced Table

notifications

---

## Referenced Columns

notification_id

---

## Required

Yes

---

## Enforcement Rule

Push notifications shall reference existing notifications.

---

## Failure Behaviour

Reject orphan push notifications.

---

# Constraint N-005

## Constraint Name

Notification Queue Integrity

---

## Purpose

Ensures queued notification jobs reference valid notifications.

---

## Constraint Type

Foreign Key

---

## Business Domain

Notification

---

## Table

notification_queue

---

## Columns

notification_id

---

## Referenced Table

notifications

---

## Referenced Columns

notification_id

---

## Required

Yes

---

## Enforcement Rule

Queue entries shall reference existing notifications.

---

## Failure Behaviour

Reject orphan queue entries.

---

## Engineering Notes

Processed queue records may later be archived.

---

# Chapter 11 — Analytics Constraints

## 11.1 Purpose

The Analytics Domain governs reporting, KPI calculation, dashboard metrics, and platform analytics.

Analytics data is derived from operational domains but never replaces them as the source of truth.

---

## 11.2 Analytics Constraint Overview

The Analytics Domain defines constraints for:

- KPI Definitions
- KPI Snapshots
- Reports
- Dashboard Metrics
- Usage Metrics

---

# Analytics Constraint Specifications

---

# Constraint A-001

## Constraint Name

KPI Snapshot Integrity

---

## Purpose

Ensures KPI snapshots belong to existing KPI definitions.

---

## Constraint Type

Foreign Key

---

## Business Domain

Analytics

---

## Table

kpi_snapshots

---

## Columns

kpi_definition_id

---

## Referenced Table

kpi_definitions

---

## Referenced Columns

kpi_definition_id

---

## Required

Yes

---

## Enforcement Rule

Snapshots shall reference valid KPI definitions.

---

## Failure Behaviour

Reject orphan KPI snapshots.

---

# Constraint A-002

## Constraint Name

Report Metric Integrity

---

## Purpose

Ensures dashboard metrics belong to valid reports.

---

## Constraint Type

Foreign Key

---

## Business Domain

Analytics

---

## Table

dashboard_metrics

---

## Columns

report_id

---

## Referenced Table

reports

---

## Referenced Columns

report_id

---

## Required

Yes

---

## Enforcement Rule

Dashboard metrics shall reference existing reports.

---

## Failure Behaviour

Reject orphan dashboard metrics.

---

# Constraint A-003

## Constraint Name

Usage Metric Tenant Integrity

---

## Purpose

Ensures tenant usage metrics belong to existing tenants.

---

## Constraint Type

Foreign Key

---

## Business Domain

Analytics

---

## Table

usage_metrics

---

## Columns

tenant_id

---

## Referenced Table

tenants

---

## Referenced Columns

tenant_id

---

## Required

Yes

---

## Enforcement Rule

Usage metrics shall reference valid tenants.

---

## Failure Behaviour

Reject orphan usage metrics.

---

# Chapter 12 — Branding Constraints

## 12.1 Purpose

The Branding Domain governs tenant identity, visual assets, themes, and public domains.

These constraints ensure branding assets remain isolated between tenants.

---

## 12.2 Branding Constraint Overview

The Branding Domain defines constraints for:

- Themes
- Logos
- Brand Assets
- Domains
- SEO Settings

---

# Branding Constraint Specifications

---

# Constraint BR-001

## Constraint Name

Theme Branding Integrity

---

## Purpose

Ensures themes belong to existing tenant branding.

---

## Constraint Type

Foreign Key

---

## Business Domain

Branding

---

## Table

themes

---

## Columns

tenant_branding_id

---

## Referenced Table

tenant_branding

---

## Referenced Columns

tenant_branding_id

---

## Required

Yes

---

## Enforcement Rule

Themes shall reference valid tenant branding.

---

## Failure Behaviour

Reject orphan themes.

---

# Constraint BR-002

## Constraint Name

Domain Name Uniqueness

---

## Purpose

Prevents duplicate public domains across the platform.

---

## Constraint Type

Unique

---

## Business Domain

Branding

---

## Table

domains

---

## Columns

domain_name

---

## Required

Yes

---

## Enforcement Rule

Every public domain shall be globally unique.

---

## Failure Behaviour

Reject duplicate domains.

---

# Constraint BR-003

## Constraint Name

SEO Settings Integrity

---

## Purpose

Ensures SEO configuration belongs to an existing domain.

---

## Constraint Type

Foreign Key

---

## Business Domain

Branding

---

## Table

seo_settings

---

## Columns

domain_id

---

## Referenced Table

domains

---

## Referenced Columns

domain_id

---

## Required

Yes

---

## Enforcement Rule

SEO settings shall reference valid domains.

---

## Failure Behaviour

Reject orphan SEO settings.

---

# Chapter 13 — Feature Management Constraints

## 13.1 Purpose

The Feature Management Domain governs platform capabilities and subscription feature availability.

These constraints ensure feature assignments remain consistent across subscriptions and tenants.

---

## 13.2 Feature Constraint Overview

The Feature Management Domain defines constraints for:

- Features
- Feature Flags
- Subscription Features
- Tenant Features
- Feature Usage

---

# Feature Constraint Specifications

---

# Constraint F-001

## Constraint Name

Feature Key Uniqueness

---

## Purpose

Ensures every platform feature has a globally unique engineering identifier.

---

## Constraint Type

Unique

---

## Business Domain

Feature Management

---

## Table

features

---

## Columns

feature_key

---

## Required

Yes

---

## Enforcement Rule

Feature keys shall remain globally unique.

---

## Failure Behaviour

Reject duplicate feature keys.

---

# Constraint F-002

## Constraint Name

Subscription Feature Integrity

---

## Purpose

Ensures feature assignments reference existing subscription plans and platform features.

---

## Constraint Type

Composite Foreign Key

---

## Business Domain

Feature Management

---

## Table

subscription_features

---

## Columns

subscription_plan_id

feature_id

---

## Referenced Tables

subscription_plans

features

---

## Referenced Columns

subscription_plan_id

feature_id

---

## Required

Yes

---

## Enforcement Rule

Assignments shall reference existing plans and features.

---

## Failure Behaviour

Reject invalid assignments.

---

# Constraint F-003

## Constraint Name

Tenant Feature Integrity

---

## Purpose

Ensures tenant feature overrides reference valid tenants and platform features.

---

## Constraint Type

Composite Foreign Key

---

## Business Domain

Feature Management

---

## Table

tenant_features

---

## Columns

tenant_id

feature_id

---

## Referenced Tables

tenants

features

---

## Referenced Columns

tenant_id

feature_id

---

## Required

Yes

---

## Enforcement Rule

Overrides shall reference existing tenants and features.

---

## Failure Behaviour

Reject invalid overrides.

---

# Chapter 14 — Shared Platform Constraints

## 14.1 Purpose

The Shared Platform Domain provides infrastructure services used across every business domain.

These constraints govern common platform resources while remaining independent of operational ownership.

---

## 14.2 Shared Platform Constraint Overview

The Shared Platform Domain defines constraints for:

- Audit Logs
- Activity Logs
- Files
- Background Jobs
- Scheduled Tasks
- API Keys
- Webhook Registrations
- Monitoring Events

---

# Shared Platform Constraint Specifications

---

# Constraint SP-001

## Constraint Name

Scheduled Task Integrity

---

## Purpose

Ensures scheduled tasks belong to valid background jobs.

---

## Constraint Type

Foreign Key

---

## Business Domain

Shared Platform

---

## Table

scheduled_tasks

---

## Columns

background_job_id

---

## Referenced Table

background_jobs

---

## Referenced Columns

background_job_id

---

## Required

Yes

---

## Enforcement Rule

Scheduled tasks shall reference existing background jobs.

---

## Failure Behaviour

Reject orphan scheduled tasks.

---

# Constraint SP-002

## Constraint Name

Tenant API Key Integrity

---

## Purpose

Ensures API keys belong to existing tenants.

---

## Constraint Type

Foreign Key

---

## Business Domain

Shared Platform

---

## Table

api_keys

---

## Columns

tenant_id

---

## Referenced Table

tenants

---

## Referenced Columns

tenant_id

---

## Required

Yes

---

## Enforcement Rule

API keys shall reference valid tenants.

---

## Failure Behaviour

Reject orphan API keys.

---

# Constraint SP-003

## Constraint Name

Webhook Registration Integrity

---

## Purpose

Ensures webhook registrations belong to existing tenants.

---

## Constraint Type

Foreign Key

---

## Business Domain

Shared Platform

---

## Table

webhook_registrations

---

## Columns

tenant_id

---

## Referenced Table

tenants

---

## Referenced Columns

tenant_id

---

## Required

Yes

---

## Enforcement Rule

Webhook registrations shall reference valid tenants.

---

## Failure Behaviour

Reject orphan webhook registrations.

---

## Engineering Notes

Webhook endpoints shall additionally satisfy application-level validation and security requirements.

---

# Chapter 7 Summary

The Notification, Analytics, Branding, Feature Management, and Shared Platform constraints establish the integrity rules for the platform's cross-cutting services. Together, they ensure reliable communication, trustworthy analytics, consistent branding, controlled feature availability, and secure infrastructure services while preserving tenant isolation and maintaining the integrity of shared platform resources.

# Chapter 15 — Cross-Domain Constraints

## 15.1 Purpose

The Cross-Domain Constraints define integrity rules that span multiple business domains within the FluxDine platform.

While previous chapters specify constraints within individual domains, this chapter governs interactions between domains to ensure that business operations remain consistent, secure, and fully isolated across the SaaS architecture.

These constraints protect:

- Cross-domain referential integrity
- Tenant ownership propagation
- Payment linkage
- Notification generation
- Analytics data collection
- Shared platform interactions

---

## 15.2 Cross-Domain Constraint Overview

The Cross-Domain Constraints govern interactions between:

- Identity ↔ Tenant
- Tenant ↔ Restaurant
- Restaurant ↔ Commerce
- Restaurant ↔ Reservation
- Commerce ↔ Payment
- Billing ↔ Payment
- Business Domains ↔ Notification
- Business Domains ↔ Analytics
- Shared Platform ↔ All Domains

---

# Cross-Domain Constraint Specifications

---

# Constraint XD-001

## Constraint Name

User Membership Tenant Integrity

---

## Purpose

Ensures every user membership references both a valid user and a valid tenant.

---

## Constraint Type

Composite Foreign Key

---

## Business Domain

Cross-Domain

---

## Table

user_memberships

---

## Columns

user_id

tenant_id

---

## Referenced Tables

users

tenants

---

## Referenced Columns

user_id

tenant_id

---

## Required

Yes

---

## Enforcement Rule

Every membership shall reference an existing user and an existing tenant.

---

## Failure Behaviour

Reject invalid user membership records.

---

## Engineering Notes

Supports users belonging to multiple tenants while maintaining strict tenant isolation.

---

# Constraint XD-002

## Constraint Name

Restaurant Tenant Ownership Integrity

---

## Purpose

Ensures every restaurant references a valid tenant.

---

## Constraint Type

Foreign Key

---

## Business Domain

Cross-Domain

---

## Table

restaurants

---

## Columns

tenant_id

---

## Referenced Table

tenants

---

## Referenced Columns

tenant_id

---

## Required

Yes

---

## Enforcement Rule

Restaurants shall reference existing tenants.

---

## Failure Behaviour

Reject orphan restaurant records.

---

## Engineering Notes

This is the root ownership constraint for every operational entity.

---

# Constraint XD-003

## Constraint Name

Order Payment Integrity

---

## Purpose

Ensures payment transactions reference existing customer orders.

---

## Constraint Type

Foreign Key

---

## Business Domain

Cross-Domain

---

## Table

payment_transactions

---

## Columns

order_id

---

## Referenced Table

orders

---

## Referenced Columns

order_id

---

## Required

No

---

## Enforcement Rule

When an order payment exists, it shall reference an existing order.

---

## Failure Behaviour

Reject orphan payment transactions.

---

## Engineering Notes

Cash-on-delivery orders may legitimately have no associated payment transaction.

---

# Constraint XD-004

## Constraint Name

Invoice Payment Integrity

---

## Purpose

Ensures subscription payment transactions reference existing invoices.

---

## Constraint Type

Foreign Key

---

## Business Domain

Cross-Domain

---

## Table

payment_transactions

---

## Columns

invoice_id

---

## Referenced Table

invoices

---

## Referenced Columns

invoice_id

---

## Required

No

---

## Enforcement Rule

Subscription payments shall reference valid invoices.

---

## Failure Behaviour

Reject orphan subscription payments.

---

## Engineering Notes

Commerce payments and subscription payments remain logically independent while using the same Payment Service.

---

# Constraint XD-005

## Constraint Name

Notification Source Integrity

---

## Purpose

Ensures notifications reference valid originating business entities.

---

## Constraint Type

Business Constraint

---

## Business Domain

Cross-Domain

---

## Table

notifications

---

## Columns

entity_type

entity_id

---

## Required

No

---

## Enforcement Rule

When a notification references a business entity, the referenced entity shall exist within its respective domain.

---

## Failure Behaviour

Reject notifications referencing nonexistent business entities.

---

## Engineering Notes

Supports polymorphic notification sources across multiple business domains.

---

# Constraint XD-006

## Constraint Name

Analytics Source Integrity

---

## Purpose

Ensures analytics records reference valid operational entities.

---

## Constraint Type

Business Constraint

---

## Business Domain

Cross-Domain

---

## Table

dashboard_metrics

usage_metrics

reports

---

## Columns

entity_type

entity_id

---

## Required

No

---

## Enforcement Rule

Analytics records shall reference existing operational entities when entity references are stored.

---

## Failure Behaviour

Reject invalid analytical references.

---

## Engineering Notes

Analytics remains a read-model and shall never become the operational source of truth.

---

# Constraint XD-007

## Constraint Name

Shared File Reference Integrity

---

## Purpose

Ensures shared platform files reference existing business entities.

---

## Constraint Type

Business Constraint

---

## Business Domain

Cross-Domain

---

## Table

files

---

## Columns

entity_type

entity_id

---

## Required

No

---

## Enforcement Rule

Referenced entities shall exist before file associations are created.

---

## Failure Behaviour

Reject invalid file associations.

---

## Engineering Notes

Supports reusable media assets across restaurants, menu items, themes, users, and customers.

---

# Constraint XD-008

## Constraint Name

Cross-Domain Tenant Consistency

---

## Purpose

Ensures all cross-domain references remain within the same tenant boundary.

---

## Constraint Type

Business Constraint

---

## Business Domain

Cross-Domain

---

## Table

Multiple

---

## Columns

tenant_id

---

## Required

Yes

---

## Enforcement Rule

A business entity shall never reference an entity owned by another tenant unless explicitly designated as a platform-owned entity.

---

## Failure Behaviour

Reject cross-tenant references.

---

## Engineering Notes

This is the primary integrity rule enforcing SaaS tenant isolation.

---

# Cross-Domain Constraint Principles

The following principles govern every cross-domain constraint:

### Principle 1

Cross-domain constraints shall never violate tenant isolation.

---

### Principle 2

Every cross-domain reference shall be explicitly documented.

---

### Principle 3

Platform-owned entities may be referenced by all tenants.

---

### Principle 4

Tenant-owned entities shall never reference resources belonging to another tenant.

---

### Principle 5

Operational domains shall not bypass ownership inheritance.

---

### Principle 6

Payment transactions shall reference either operational commerce records or subscription billing records, but never both simultaneously.

---

### Principle 7

Notifications may reference business entities without owning them.

---

### Principle 8

Analytics may consume operational data without becoming the authoritative source of truth.

---

# Chapter 15 Summary

The Cross-Domain constraints establish the integrity rules governing interactions between business domains throughout the FluxDine platform. They ensure that cross-domain references remain valid, tenant boundaries are never violated, payment processing remains consistent across commerce and billing, and shared platform services interact safely with operational domains. Together, these constraints preserve architectural consistency while supporting a scalable, multi-tenant SaaS environment.

# Chapter 16 — Tenant Isolation Rules

## 16.1 Purpose

Tenant isolation is the cornerstone of the FluxDine multi-tenant SaaS architecture.

These rules define the mandatory constraints that ensure every tenant's data remains logically, operationally, and securely isolated from every other tenant while allowing the platform to operate as a shared infrastructure.

Tenant isolation constraints apply to every persistent business entity unless explicitly designated as a Platform-owned entity.

---

## 16.2 Tenant Isolation Objectives

The tenant isolation model shall guarantee:

- Complete data separation
- Ownership consistency
- Secure authorization
- Referential integrity
- Predictable query boundaries
- Regulatory compliance
- SaaS scalability

---

# Tenant Isolation Rules

## Rule TI-001

Every tenant-owned entity shall contain a valid ownership path that ultimately resolves to one and only one tenant.

---

## Rule TI-002

Tenant ownership shall never change after entity creation.

Ownership transfer shall require an approved migration process.

---

## Rule TI-003

No foreign key relationship shall reference an entity belonging to another tenant unless the referenced entity is Platform-owned.

---

## Rule TI-004

Queries executed against tenant-owned entities shall always be constrained by tenant ownership.

---

## Rule TI-005

Platform-owned reference tables may be shared across all tenants.

Examples include:

- countries
- currencies
- languages
- payment_providers
- subscription_plans
- features

---

## Rule TI-006

Tenant-owned entities shall never reference another tenant's API keys, domains, branding, subscriptions, payment gateways, or operational records.

---

## Rule TI-007

Deletion of a tenant shall not automatically cascade into destructive deletion of historical business records.

Historical preservation policies shall be applied.

---

## Rule TI-008

Every tenant-owned record shall remain independently auditable.

---

# Chapter 17 — Data Integrity Rules

## 17.1 Purpose

Data integrity rules establish universal constraints that protect the correctness, consistency, and reliability of information stored throughout the FluxDine platform.

These rules apply across all business domains.

---

# Data Integrity Rules

## Rule DI-001

Every table shall define one immutable primary key.

---

## Rule DI-002

Every foreign key shall reference an existing parent record.

---

## Rule DI-003

Orphan records are prohibited unless explicitly documented.

---

## Rule DI-004

Business identifiers shall be unique within their defined scope.

---

## Rule DI-005

Mandatory business information shall not contain NULL values.

---

## Rule DI-006

Check constraints shall enforce valid business values.

---

## Rule DI-007

Composite uniqueness shall be enforced wherever business uniqueness depends upon multiple attributes.

---

## Rule DI-008

Deleted business records shall preserve referential integrity.

---

## Rule DI-009

Historical financial records shall remain immutable.

---

## Rule DI-010

Constraint validation shall occur before persistence whenever possible.

---

# Chapter 18 — Lifecycle Constraints

## 18.1 Purpose

Lifecycle constraints define the permitted state transitions for major business entities.

These rules ensure business processes follow valid progression paths while preventing inconsistent or impossible state changes.

---

# Lifecycle Constraint Rules

## Rule LC-001

Entity lifecycle transitions shall follow documented state models.

---

## Rule LC-002

Completed business transactions shall become immutable.

Examples include:

- Orders
- Invoices
- Payment Transactions
- Reservation Events

---

## Rule LC-003

Cancelled entities shall preserve historical records.

---

## Rule LC-004

Soft deletion shall be preferred over physical deletion for operational entities.

---

## Rule LC-005

Archived entities shall remain queryable for reporting and auditing.

---

## Rule LC-006

Lifecycle transitions shall not violate referential integrity.

---

## Rule LC-007

Business entities shall never regress to invalid previous states unless explicitly supported by documented business workflows.

---

## Rule LC-008

Lifecycle validation shall be enforced consistently across all application services.

---

# Chapter 19 — Engineering Rules

## 19.1 Purpose

Engineering rules define the mandatory standards that every database implementation shall follow.

These rules ensure consistency between database design, application services, migrations, APIs, and ORM mappings.

---

# Engineering Rules

## Rule ER-001

Every constraint defined within this specification shall be uniquely identified and documented.

---

## Rule ER-002

Constraint names shall follow the naming standards defined in **00 Database Naming Standards.md**.

---

## Rule ER-003

Constraint implementation shall remain consistent across development, testing, staging, and production environments.

---

## Rule ER-004

Database constraints shall complement—not replace—application-level validation.

---

## Rule ER-005

Constraint changes shall be introduced only through controlled database migrations.

---

## Rule ER-006

Breaking changes to constraints require an approved Architecture Decision Record (ADR).

---

## Rule ER-007

Constraint implementation shall remain technology-independent wherever possible.

---

## Rule ER-008

Constraint definitions within this document are the authoritative source for database integrity requirements.

---

# Chapter 20 — Architecture Decision Records

The following Architecture Decision Records govern the design and implementation of database constraints throughout the FluxDine platform.

---

## ADR-C-001

Every persistent entity shall define exactly one primary key.

---

## ADR-C-002

Foreign key relationships shall preserve referential integrity at all times.

---

## ADR-C-003

Tenant isolation shall never be compromised by database constraints.

---

## ADR-C-004

Business uniqueness shall be enforced using unique or composite unique constraints.

---

## ADR-C-005

Historical financial data shall remain immutable after completion.

---

## ADR-C-006

Soft deletion shall be preferred for operational business entities.

---

## ADR-C-007

Platform-owned reference data shall remain globally shared and immutable.

---

## ADR-C-008

Cross-domain constraints shall explicitly preserve ownership inheritance.

---

## ADR-C-009

Constraint implementation shall remain independent of any specific database engine or ORM framework.

---

## ADR-C-010

This document is the authoritative engineering specification for logical database constraints within the FluxDine platform.

---

# Phase 9 Summary

Phase 9 establishes the governance framework for database constraints across the FluxDine platform. It defines mandatory tenant isolation rules, universal data integrity principles, lifecycle constraints, engineering implementation standards, and the Architecture Decision Records (ADRs) that guide future database evolution. Together, these chapters ensure that every constraint defined in this specification is implemented consistently, preserves data integrity, enforces SaaS tenant isolation, and aligns with the long-term architectural vision of the FluxDine platform.


---

# Appendix A — Complete Constraint Matrix

The following matrix provides a consolidated overview of the major constraint categories defined throughout this specification.

| Domain | Primary Keys | Foreign Keys | Unique | Composite | Check | Business |
|----------|-------------:|-------------:|--------:|----------:|------:|----------:|
| Platform | ✓ | ✓ | ✓ | — | ✓ | ✓ |
| Identity | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Tenant | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Restaurant | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Commerce | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Customer | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Reservation | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Billing | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Payment | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Notification | ✓ | ✓ | ✓ | — | ✓ | ✓ |
| Analytics | ✓ | ✓ | ✓ | — | ✓ | ✓ |
| Branding | ✓ | ✓ | ✓ | — | ✓ | ✓ |
| Feature Management | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Shared Platform | ✓ | ✓ | ✓ | — | ✓ | ✓ |

---

## Constraint Coverage Summary

The FluxDine platform defines the following logical constraint categories:

- Primary Key Constraints
- Foreign Key Constraints
- Unique Constraints
- Composite Unique Constraints
- Composite Foreign Key Constraints
- NOT NULL Constraints
- Default Constraints
- Check Constraints
- Business Constraints
- Lifecycle Constraints
- Cross-Domain Constraints
- Tenant Isolation Constraints

Every persistent entity shall be governed by one or more of these constraint categories.

---

# Appendix B — Composite Constraints

The following composite constraints are used throughout the platform to enforce business uniqueness and multi-column integrity.

| Table | Composite Columns | Purpose |
|--------|-------------------|---------|
| authentication_providers | user_id + provider_name + provider_account_id | Prevent duplicate provider registrations |
| mfa_configurations | user_id + mfa_method | One active MFA configuration per method |
| branches | restaurant_id + branch_code | Unique branch code within a restaurant |
| restaurant_tables | seating_area_id + table_number | Unique table number within a seating area |
| menu_items | restaurant_id + sku | Unique SKU within a restaurant |
| carts | customer_id + restaurant_id + cart_status | One active cart per customer per restaurant |
| cart_items | cart_id + menu_item_id | Valid product assignment within a cart |
| customers | tenant_id + email | Unique customer email within a tenant |
| tenant_features | tenant_id + feature_id | Unique tenant feature assignment |
| subscription_features | subscription_plan_id + feature_id | Unique subscription feature assignment |
| reservation_tables | reservation_id + table_id | Prevent duplicate table assignments |
| rider_assignments | rider_id + branch_id | Unique rider assignment |
| user_memberships | user_id + tenant_id | Unique tenant membership |

---

## Composite Constraint Design Principles

Composite constraints shall:

- Represent genuine business uniqueness.
- Avoid redundant single-column constraints.
- Support tenant isolation.
- Remain stable over time.
- Be documented before implementation.

---

# Appendix C — Business Rules

The following high-level business rules are enforced through database constraints and application services.

## Ownership Rules

- Every business entity has exactly one logical owner.
- Ownership is inherited through the entity hierarchy.
- Ownership never crosses tenant boundaries.
- Ownership is immutable after creation unless an approved migration process is executed.

---

## Referential Integrity Rules

- Parent records shall exist before child records are created.
- Orphan records are prohibited unless explicitly documented.
- Historical references shall remain valid throughout the entity lifecycle.

---

## Financial Integrity Rules

- Invoices are immutable.
- Payment transactions are immutable after successful authorization.
- Credit notes preserve accounting history.
- Refund requests reference existing invoices.
- Financial history shall never be physically deleted.

---

## Security Rules

- Authentication credentials shall remain unique.
- API keys shall be globally unique.
- Notification templates shall be platform-managed.
- Payment providers shall remain platform-owned.

---

## Tenant Isolation Rules

- Tenant-owned entities cannot reference another tenant's entities.
- Platform-owned reference data may be shared.
- Every query shall respect tenant boundaries.
- Cross-tenant relationships are prohibited unless explicitly documented.

---

## Operational Rules

- Orders cannot exist without customers and restaurants.
- Reservations require valid branches.
- Menus require restaurants.
- Branches require restaurants.
- Restaurants require tenants.

---

# Appendix D — Reserved Future Constraints

The following constraint groups are reserved for future platform expansion.

## Inventory

Future constraints will govern:

- Supplier ownership
- Purchase orders
- Inventory items
- Inventory transactions
- Warehouse locations

---

## Kitchen Display System (KDS)

Future constraints will govern:

- Kitchen stations
- Kitchen queues
- Preparation workflows
- Station assignments

---

## Marketplace

Future constraints will govern:

- Marketplace merchants
- Marketplace settlements
- Marketplace commissions
- Marketplace payouts

---

## Artificial Intelligence

Future constraints will govern:

- AI conversations
- AI recommendations
- AI prompts
- AI-generated analytics

---

## Workforce Management

Future constraints will govern:

- Employee schedules
- Attendance
- Leave management
- Payroll integration

---

## Fleet Management

Future constraints will govern:

- Delivery vehicles
- GPS tracking
- Route optimization
- Driver assignments

---

All future constraints shall be introduced through approved Architecture Decision Records (ADRs) before implementation.

---

# References

This document should be read together with the following FluxDine Architecture Bible documents.

## Core Architecture

- 04 System Architecture Blueprint
- 05 Database Architecture & Multi-Tenant Data Model
- 06 API & Service Architecture
- 07 Security Architecture
- 08 Infrastructure Architecture

---

## Database Engineering Specifications

- 00 Database Naming Standards
- 01 Complete Database Schema Specification
- 02 Table Specifications
- 03 Relationships
- 05 Index Specification
- 06 Enum Specification
- 07 Database Migration Strategy
- 08 Drizzle ORM Mapping

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
| 0.4 | Domain Constraints | FluxDine Engineering | Platform through Shared Platform constraint specifications completed |
| 0.8 | Governance | FluxDine Engineering | Cross-domain constraints, tenant isolation, lifecycle rules, engineering rules, and ADRs completed |
| 1.0 | Final Release | FluxDine Engineering | Constraint specification approved as the authoritative database integrity standard |

---
