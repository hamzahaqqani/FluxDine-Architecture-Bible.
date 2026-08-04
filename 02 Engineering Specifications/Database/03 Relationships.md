# 02 Engineering Specifications

# Database

# 03 — Relationships

---

# Document Control

| Field | Value |
|--------|-------|
| Document ID | FD-ENG-DB-003 |
| Document Name | Relationships |
| Version | 1.0 |
| Status | 🚧 Draft |
| Owner | FluxDine Engineering |
| Classification | Internal Engineering Specification |
| Depends On | 00 Database Naming Standards<br>01 Complete Database Schema Specification<br>02 Table Specifications |
| Referenced By | 04 Constraints<br>05 Index Specification<br>06 Enum Specification<br>07 Database Migration Strategy<br>08 Drizzle ORM Mapping |

---

# Dependencies

This document depends upon the following Architecture and Engineering documents.

## Core Architecture

- 04 System Architecture Blueprint
- 05 Database Architecture & Multi-Tenant Data Model
- 06 API & Service Architecture

## Database Engineering

- 00 Database Naming Standards
- 01 Complete Database Schema Specification
- 02 Table Specifications

These documents define the business domains, ownership hierarchy, database architecture, and logical tables upon which all database relationships are established.

---

# Referenced By

This document serves as the authoritative source for logical database relationships and shall be referenced by:

## Database Engineering

- 04 Constraints
- 05 Index Specification
- 06 Enum Specification
- 07 Database Migration Strategy
- 08 Drizzle ORM Mapping

## Backend Engineering

- Repository Specifications
- Service Specifications
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
| Platform Relationships | Pending |
| Identity Relationships | Pending |
| Tenant Relationships | Pending |
| Restaurant Relationships | Pending |
| Commerce Relationships | Pending |
| Customer Relationships | Pending |
| Reservation Relationships | Pending |
| Billing Relationships | Pending |
| Payment Relationships | Pending |
| Notification Relationships | Pending |
| Analytics Relationships | Pending |
| Branding Relationships | Pending |
| Feature Management Relationships | Pending |
| Shared Platform Relationships | Pending |
| Cross-Domain Relationships | Pending |
| Ownership Inheritance | Pending |
| Cascade Rules | Pending |
| Relationship Standards | Pending |
| Architecture Decision Records | Pending |
| Appendices | Pending |

---

# Purpose

The **Relationships** document is the authoritative engineering specification describing how every logical database table within the FluxDine platform connects to every other table.

Where **02 Table Specifications** defines individual tables, this document defines the relationships between them.

Every parent-child dependency, ownership relationship, junction relationship, and cross-domain dependency shall be documented here before implementation.

This specification provides the logical foundation for:

- Foreign Key Design
- Referential Integrity
- Repository Architecture
- Service Layer Dependencies
- Database Constraints
- Drizzle ORM Relations
- API Navigation
- Entity Ownership

---

# Scope

This document defines:

- Parent-child relationships
- One-to-One relationships
- One-to-Many relationships
- Many-to-Many relationships
- Junction tables
- Ownership inheritance
- Cross-domain relationships
- Relationship lifecycle
- Cascade expectations
- Referential design principles

---

# Out of Scope

This document intentionally does **not** define:

- Database columns
- SQL data types
- Foreign key SQL syntax
- Database constraints
- Check constraints
- Indexes
- Enumerations
- ORM code
- SQL migrations

These topics are documented in subsequent Engineering Specifications.

---

# Relationship Philosophy

Relationships are the foundation of the FluxDine data model.

Every relationship represents a business dependency rather than merely a database connection.

Relationships shall:

- Express real business ownership.
- Preserve data integrity.
- Support tenant isolation.
- Follow domain boundaries.
- Remain implementation independent.
- Be explicitly documented.
- Never rely on implicit assumptions.

The relationship model shall remain stable even if implementation technologies change.

---

# Relationship Design Principles

Every relationship shall follow these principles.

---

## Explicit Ownership

Every child entity shall have one clearly defined logical owner.

---

## Single Parent Rule

Each entity shall have one primary parent responsible for ownership.

---

## Domain Integrity

Relationships should remain within business domains whenever possible.

Cross-domain relationships shall be minimized and explicitly documented.

---

## Tenant Isolation

Tenant-owned entities shall never establish relationships that violate tenant boundaries.

---

## Referential Integrity

Relationships shall preserve logical consistency throughout the platform.

Orphaned records shall not exist unless explicitly allowed.

---

## Documentation First

Relationships shall be documented before implementation.

---

## Technology Independence

Relationship definitions shall remain independent of PostgreSQL, Drizzle ORM, or any future persistence technology.

---

# Relationship Types

FluxDine supports four logical relationship categories.

---

## One-to-One (1:1)

Each parent owns exactly one child.

Example:

```text
Restaurant

↓

Restaurant Settings
```

---

## One-to-Many (1:N)

A parent owns multiple child records.

Example:

```text
Restaurant

↓

Branches
```

---

## Many-to-Many (N:N)

Multiple entities relate through an explicit junction table.

Example:

```text
Users

↓

User Roles

↓

Roles
```

---

## Reference Relationship

Reference tables provide lookup data without ownership.

Example:

```text
Countries

↓

Tenants
```

Reference relationships do not imply ownership.

---

# Cardinality Standards

The following notation is used throughout this document.

| Symbol | Meaning |
|----------|---------|
| 1:1 | One-to-One |
| 1:N | One-to-Many |
| N:N | Many-to-Many |
| Optional | Child may not exist |
| Required | Child must exist |

Examples:

```text
Tenant (1)

↓

Restaurant (N)
```

```text
Order (1)

↓

Order Items (N)
```

---

# How to Read This Document

Relationships are organized according to business domains.

Each chapter begins with the parent entities before documenting every relationship belonging to that domain.

Cross-domain relationships appear later in the document after all individual domains have been defined.

Recommended reading order:

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

This order mirrors the ownership hierarchy established throughout the FluxDine Architecture Bible.

---

# Standard Relationship Specification Template

Every relationship documented in this specification shall follow the standardized template below.

---

## Relationship Name

Official business name of the relationship.

---

## Purpose

Explains why the relationship exists.

---

## Relationship Type

Values include:

- One-to-One
- One-to-Many
- Many-to-Many
- Reference

---

## Business Domain

Primary business domain responsible for the relationship.

---

## Parent Table

Logical owner of the relationship.

---

## Child Table

Dependent table.

---

## Ownership

Defines ownership responsibility.

Examples:

- Platform
- Tenant
- Restaurant
- Branch

---

## Cardinality

Defines the multiplicity of the relationship.

Examples:

- 1:1
- 1:N
- N:N

---

## Foreign Key

Logical foreign key name.

---

## Required

Whether the relationship is mandatory.

Values:

- Yes
- No

---

## Cascade Delete

Expected delete behavior.

Typical values:

- Restrict
- Cascade
- Set Null
- No Action

---

## Cascade Update

Expected update behavior.

Typical values:

- Cascade
- Restrict
- No Action

---

## Orphan Policy

Defines whether child records may exist without their parent.

Values:

- Allowed
- Not Allowed

---

## Relationship Lifecycle

Summarizes how the relationship evolves throughout the business lifecycle.

---

## Engineering Notes

Provides implementation guidance, assumptions, or architectural considerations.

---

# Table of Contents

## Chapter 1 — Platform Relationships

## Chapter 2 — Identity Relationships

## Chapter 3 — Tenant Relationships

## Chapter 4 — Restaurant Relationships

## Chapter 5 — Commerce Relationships

## Chapter 6 — Customer Relationships

## Chapter 7 — Reservation Relationships

## Chapter 8 — Billing Relationships

## Chapter 9 — Payment Relationships

## Chapter 10 — Notification Relationships

## Chapter 11 — Analytics Relationships

## Chapter 12 — Branding Relationships

## Chapter 13 — Feature Management Relationships

## Chapter 14 — Shared Platform Relationships

---

## Chapter 15 — Cross-Domain Architecture

## Chapter 16 — Ownership Inheritance

## Chapter 17 — Cascade Rules

## Chapter 18 — Relationship Naming Standards

## Chapter 19 — Engineering Rules

## Chapter 20 — Architecture Decision Records

---

# Chapter 1 — Platform Relationships

## 1.1 Purpose

The Platform Domain establishes the global reference relationships that support every application, service, and business domain within the FluxDine ecosystem.

Unlike tenant-owned relationships, Platform relationships connect globally shared reference data to business entities while preserving platform-wide consistency.

These relationships define:

- Global configuration
- Localization
- Subscription catalog
- Feature catalog
- Authorization catalog
- Platform communication resources

---

## 1.2 Platform Relationship Hierarchy

```text
Platform

├── Countries
│      ├── Tenants
│      ├── Restaurants
│      └── Customer Addresses
│
├── Currencies
│      ├── Billing Accounts
│      ├── Restaurants
│      └── Payment Transactions
│
├── Languages
│      ├── Users
│      ├── Customers
│      └── Notification Templates
│
├── Timezones
│      ├── Tenants
│      ├── Restaurants
│      └── Branches
│
├── Subscription Plans
│      └── Subscriptions
│
├── Platform Features
│      ├── Subscription Features
│      └── Tenant Features
│
├── Global Roles
│      └── Role Permissions
│
├── Global Permissions
│      └── Role Permissions
│
├── Email Templates
│      └── Notifications
│
└── Notification Templates
       └── Notifications
```

---

# Platform Relationship Specifications

---

# Relationship 1

## Relationship Name

Country Supports Tenants

---

## Purpose

Associates every tenant with an operating country.

---

## Relationship Type

Reference

---

## Business Domain

Platform

---

## Parent Table

countries

---

## Child Table

tenants

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

country_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Country Created

↓

Tenant Registered

↓

Relationship Active
```

---

## Engineering Notes

Countries are global reference records and may not be deleted while referenced.

---

# Relationship 2

## Relationship Name

Country Supports Restaurants

---

## Purpose

Defines the operating country for restaurants.

---

## Relationship Type

Reference

---

## Parent Table

countries

---

## Child Table

restaurants

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

country_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Supports localization, taxation, and compliance.

---

# Relationship 3

## Relationship Name

Currency Used By Billing Accounts

---

## Purpose

Associates billing accounts with their operating currency.

---

## Relationship Type

Reference

---

## Business Domain

Platform

---

## Parent Table

currencies

---

## Child Table

billing_accounts

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

currency_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Currencies remain immutable after billing activation.

---

# Relationship 4

## Relationship Name

Currency Used By Restaurants

---

## Purpose

Defines the operating currency used by restaurants.

---

## Relationship Type

Reference

---

## Parent Table

currencies

---

## Child Table

restaurants

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

currency_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Engineering Notes

Provides pricing consistency across restaurant operations.

---

# Relationship 5

## Relationship Name

Language Assigned To Users

---

## Purpose

Associates users with their preferred language.

---

## Relationship Type

Reference

---

## Business Domain

Platform

---

## Parent Table

languages

---

## Child Table

users

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

language_id

---

## Required

No

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Engineering Notes

Language preference is optional and user-configurable.

---

# Relationship 6

## Relationship Name

Language Used By Customers

---

## Purpose

Stores customer language preference.

---

## Relationship Type

Reference

---

## Parent Table

languages

---

## Child Table

customers

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

language_id

---

## Required

No

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

# Relationship 7

## Relationship Name

Timezone Assigned To Tenants

---

## Purpose

Defines the default operating timezone for tenants.

---

## Relationship Type

Reference

---

## Parent Table

timezones

---

## Child Table

tenants

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

timezone_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

# Relationship 8

## Relationship Name

Timezone Assigned To Branches

---

## Purpose

Allows each branch to operate in its local timezone.

---

## Relationship Type

Reference

---

## Parent Table

timezones

---

## Child Table

branches

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

timezone_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

# Relationship 9

## Relationship Name

Subscription Plan Defines Subscriptions

---

## Purpose

Associates subscriptions with the plan purchased by the tenant.

---

## Relationship Type

One-to-Many

---

## Business Domain

Platform

---

## Parent Table

subscription_plans

---

## Child Table

subscriptions

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

subscription_plan_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Historical subscriptions retain their plan association even if the plan is retired.

---

# Relationship 10

## Relationship Name

Platform Feature Included In Subscription

---

## Purpose

Associates platform capabilities with subscription plans.

---

## Relationship Type

Many-to-Many

---

## Business Domain

Platform

---

## Parent Table

features

---

## Child Table

subscription_features

---

## Ownership

Platform

---

## Cardinality

N:N

---

## Junction Table

subscription_features

---

## Foreign Key

feature_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

A feature may appear in many subscription plans, and a subscription plan may include many features.

---

# Relationship 11

## Relationship Name

Platform Feature Assigned To Tenant

---

## Purpose

Defines tenant-specific feature overrides.

---

## Relationship Type

Many-to-Many

---

## Business Domain

Platform

---

## Parent Table

features

---

## Child Table

tenant_features

---

## Ownership

Platform

---

## Cardinality

N:N

---

## Junction Table

tenant_features

---

## Foreign Key

feature_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Engineering Notes

Overrides supplement, but do not replace, subscription feature assignments.

---

# Relationship 12

## Relationship Name

Global Role Contains Permissions

---

## Purpose

Associates permissions with reusable platform roles.

---

## Relationship Type

Many-to-Many

---

## Business Domain

Platform

---

## Parent Table

global_roles

---

## Child Table

role_permissions

---

## Ownership

Platform

---

## Cardinality

N:N

---

## Junction Table

role_permissions

---

## Foreign Key

role_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Engineering Notes

Roles remain independent from user assignments.

---

# Relationship 13

## Relationship Name

Global Permission Assigned To Roles

---

## Purpose

Associates permissions with authorization roles.

---

## Relationship Type

Many-to-Many

---

## Parent Table

global_permissions

---

## Child Table

role_permissions

---

## Ownership

Platform

---

## Cardinality

N:N

---

## Junction Table

role_permissions

---

## Foreign Key

permission_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Engineering Notes

Permissions are reusable across multiple roles.

---

# Relationship 14

## Relationship Name

Email Template Generates Notifications

---

## Purpose

Associates reusable email templates with outbound notifications.

---

## Relationship Type

One-to-Many

---

## Business Domain

Platform

---

## Parent Table

email_templates

---

## Child Table

notifications

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

email_template_id

---

## Required

No

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Engineering Notes

Notifications may also originate from dynamically generated content.

---

# Relationship 15

## Relationship Name

Notification Template Generates Notifications

---

## Purpose

Associates notification templates with outbound notification records.

---

## Relationship Type

One-to-Many

---

## Business Domain

Platform

---

## Parent Table

notification_templates

---

## Child Table

notifications

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

notification_template_id

---

## Required

No

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Engineering Notes

Supports SMS, Push, Email, and future notification channels.

---

# Chapter 2 — Identity Relationships

## 2.1 Purpose

The Identity Domain establishes the relationships required to support the Unified Identity System.

These relationships ensure that every authenticated individual exists only once while allowing secure participation across multiple tenants, roles, authentication methods, and sessions.

Identity relationships form the security backbone of the FluxDine platform.

---

## 2.2 Identity Relationship Hierarchy

```text
Users

├── User Memberships
├── User Roles
├── Sessions
├── Authentication Providers
├── MFA Configurations
├── Password Reset Tokens
└── Email Verification Tokens

Roles

└── User Roles

Permissions

└── Role Permissions
```

---

# Identity Relationship Specifications

---

# Relationship 16

## Relationship Name

User Owns Memberships

---

## Purpose

Associates users with one or more tenant memberships.

---

## Relationship Type

One-to-Many

---

## Business Domain

Identity

---

## Parent Table

users

---

## Child Table

user_memberships

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

user_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
User Created

↓

Membership Created

↓

Membership Active

↓

Membership Removed
```

---

## Engineering Notes

Supports users belonging to multiple tenants while maintaining a single platform identity.

---

# Relationship 17

## Relationship Name

User Owns Sessions

---

## Purpose

Tracks authenticated sessions for each user.

---

## Relationship Type

One-to-Many

---

## Parent Table

users

---

## Child Table

sessions

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

user_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Engineering Notes

Sessions are automatically removed when a user is permanently deleted.

---

# Relationship 18

## Relationship Name

User Owns Authentication Providers

---

## Purpose

Associates external authentication methods with a user account.

---

## Relationship Type

One-to-Many

---

## Parent Table

users

---

## Child Table

authentication_providers

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

user_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Engineering Notes

Supports multiple authentication providers per user.

---

# Relationship 19

## Relationship Name

User Owns MFA Configurations

---

## Relationship Type

One-to-Many

---

## Parent Table

users

---

## Child Table

mfa_configurations

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

user_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

# Relationship 20

## Relationship Name

User Owns Password Reset Tokens

---

## Relationship Type

One-to-Many

---

## Parent Table

users

---

## Child Table

password_reset_tokens

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

user_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

# Relationship 21

## Relationship Name

User Owns Email Verification Tokens

---

## Relationship Type

One-to-Many

---

## Parent Table

users

---

## Child Table

email_verification_tokens

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

user_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

# Relationship 22

## Relationship Name

Role Assigned To Users

---

## Purpose

Assigns authorization roles to users through a junction table.

---

## Relationship Type

Many-to-Many

---

## Business Domain

Identity

---

## Parent Table

roles

---

## Child Table

user_roles

---

## Ownership

Platform

---

## Cardinality

N:N

---

## Junction Table

user_roles

---

## Foreign Key

role_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Engineering Notes

User role assignments are tenant-scoped through user memberships.

---

# Relationship 23

## Relationship Name

Permission Assigned To Roles

---

## Purpose

Associates permissions with authorization roles.

---

## Relationship Type

Many-to-Many

---

## Parent Table

permissions

---

## Child Table

role_permissions

---

## Ownership

Platform

---

## Cardinality

N:N

---

## Junction Table

role_permissions

---

## Foreign Key

permission_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Engineering Notes

Role-permission mappings implement the platform's role-based access control (RBAC) model.

---

# Phase 2 Summary

Phase 2 establishes the foundational relationships for the **Platform** and **Identity** domains. These relationships define how global reference data, subscription catalogs, authorization resources, and unified identities connect to the rest of the FluxDine platform. Together they form the core relational infrastructure upon which all tenant-owned business domains are built.

# Chapter 3 — Tenant Relationships

## 3.1 Purpose

The Tenant Domain defines the ownership boundary of the FluxDine multi-tenant architecture.

Every restaurant business onboarded to FluxDine is represented by a Tenant. All tenant-owned resources originate from the Tenant entity either directly or indirectly through ownership inheritance.

These relationships establish:

- Tenant ownership
- SaaS isolation
- Configuration hierarchy
- Feature assignment
- Branding
- Integrations
- Payment gateway ownership
- Subscription linkage
- API security

The Tenant Domain is the root of every customer-owned data hierarchy.

---

## 3.2 Tenant Relationship Hierarchy

```text
Tenant

├── Tenant Settings
├── Tenant Domains
├── Tenant Branding
├── Tenant Features
├── Tenant Payment Gateways
├── Tenant Integrations
├── Tenant Subscriptions
├── Tenant Usage Metrics
├── Tenant API Keys
├── Tenant Audit Settings
├── Tenant Invitations
└── Restaurants
```

---

# Tenant Relationship Specifications

---

# Relationship 24

## Relationship Name

Tenant Owns Restaurants

---

## Purpose

Defines the primary ownership relationship between a tenant and its restaurant.

Every restaurant belongs to exactly one tenant.

---

## Relationship Type

One-to-Many

---

## Business Domain

Tenant

---

## Parent Table

tenants

---

## Child Table

restaurants

---

## Ownership

Tenant

---

## Cardinality

1:N

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Tenant Created

↓

Restaurant Created

↓

Restaurant Active

↓

Restaurant Archived
```

---

## Engineering Notes

This relationship establishes the root ownership boundary for every operational entity in the platform.

---

# Relationship 25

## Relationship Name

Tenant Owns Settings

---

## Purpose

Associates operational settings with a tenant.

---

## Relationship Type

One-to-One

---

## Business Domain

Tenant

---

## Parent Table

tenants

---

## Child Table

tenant_settings

---

## Ownership

Tenant

---

## Cardinality

1:1

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Tenant Created

↓

Settings Created

↓

Settings Updated
```

---

## Engineering Notes

Each tenant shall have exactly one settings record.

---

# Relationship 26

## Relationship Name

Tenant Owns Domains

---

## Purpose

Associates internet domains with a tenant.

---

## Relationship Type

One-to-Many

---

## Business Domain

Tenant

---

## Parent Table

tenants

---

## Child Table

tenant_domains

---

## Ownership

Tenant

---

## Cardinality

1:N

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Domain Added

↓

Verification

↓

Active

↓

Removed
```

---

## Engineering Notes

A domain shall belong to only one tenant.

---

# Relationship 27

## Relationship Name

Tenant Owns Branding

---

## Purpose

Associates branding configuration with a tenant.

---

## Relationship Type

One-to-One

---

## Business Domain

Tenant

---

## Parent Table

tenants

---

## Child Table

tenant_branding

---

## Ownership

Tenant

---

## Cardinality

1:1

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Branding defines tenant identity across customer-facing applications.

---

# Relationship 28

## Relationship Name

Tenant Owns Feature Overrides

---

## Purpose

Associates feature overrides with a tenant.

---

## Relationship Type

One-to-Many

---

## Business Domain

Tenant

---

## Parent Table

tenants

---

## Child Table

tenant_features

---

## Ownership

Tenant

---

## Cardinality

1:N

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Overrides supplement subscription features without modifying platform defaults.

---

# Relationship 29

## Relationship Name

Tenant Owns Payment Gateway Configurations

---

## Purpose

Associates configured payment gateways with a tenant.

---

## Relationship Type

One-to-Many

---

## Business Domain

Tenant

---

## Parent Table

tenants

---

## Child Table

tenant_payment_gateways

---

## Ownership

Tenant

---

## Cardinality

1:N

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Gateway Added

↓

Verification

↓

Active

↓

Disabled
```

---

## Engineering Notes

Supports multiple payment gateways per tenant.

---

# Relationship 30

## Relationship Name

Tenant Owns Integrations

---

## Purpose

Associates third-party integrations with a tenant.

---

## Relationship Type

One-to-Many

---

## Business Domain

Tenant

---

## Parent Table

tenants

---

## Child Table

tenant_integrations

---

## Ownership

Tenant

---

## Cardinality

1:N

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Each integration belongs exclusively to one tenant.

---

# Relationship 31

## Relationship Name

Tenant Owns Subscription Mapping

---

## Purpose

Associates a tenant with its active subscription record.

---

## Relationship Type

One-to-One

---

## Business Domain

Tenant

---

## Parent Table

tenants

---

## Child Table

tenant_subscriptions

---

## Ownership

Tenant

---

## Cardinality

1:1

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Tenant Registered

↓

Trial Started

↓

Subscription Activated

↓

Subscription Expired
```

---

## Engineering Notes

This relationship links the operational tenant to the Billing Domain without duplicating subscription information.

---

# Relationship 32

## Relationship Name

Tenant Owns Usage Metrics

---

## Purpose

Associates resource usage statistics with a tenant.

---

## Relationship Type

One-to-Many

---

## Business Domain

Tenant

---

## Parent Table

tenants

---

## Child Table

tenant_usage_metrics

---

## Ownership

Tenant

---

## Cardinality

1:N

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Supports billing, analytics, and capacity planning.

---

# Relationship 33

## Relationship Name

Tenant Owns API Keys

---

## Purpose

Associates issued API credentials with a tenant.

---

## Relationship Type

One-to-Many

---

## Business Domain

Tenant

---

## Parent Table

tenants

---

## Child Table

tenant_api_keys

---

## Ownership

Tenant

---

## Cardinality

1:N

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Key Issued

↓

Active

↓

Rotated

↓

Revoked
```

---

## Engineering Notes

API keys are scoped to a single tenant.

---

# Relationship 34

## Relationship Name

Tenant Owns Audit Settings

---

## Purpose

Associates audit configuration with a tenant.

---

## Relationship Type

One-to-One

---

## Business Domain

Tenant

---

## Parent Table

tenants

---

## Child Table

tenant_audit_settings

---

## Ownership

Tenant

---

## Cardinality

1:1

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Defines tenant-specific retention and auditing policies.

---

# Relationship 35

## Relationship Name

Tenant Owns Invitations

---

## Purpose

Associates onboarding invitations with a tenant.

---

## Relationship Type

One-to-Many

---

## Business Domain

Tenant

---

## Parent Table

tenants

---

## Child Table

tenant_invitations

---

## Ownership

Tenant

---

## Cardinality

1:N

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Invitation Created

↓

Pending

↓

Accepted

↓

Expired
```

---

## Engineering Notes

Invitation history is retained for auditing and onboarding analytics.

---

# Chapter 3 Summary

The **Tenant Domain** defines the primary ownership boundary of the FluxDine SaaS platform. Every operational resource ultimately inherits ownership from a tenant through these relationships. By establishing clear one-to-one and one-to-many relationships for configuration, branding, subscriptions, integrations, payment gateways, API access, and restaurant ownership, this chapter enforces tenant isolation, simplifies authorization, and provides the foundation for all downstream business domains. It is the cornerstone of FluxDine's multi-tenant relational architecture.

# Chapter 4 — Restaurant Relationships

## 4.1 Purpose

The Restaurant Domain defines the operational hierarchy of every restaurant within the FluxDine platform.

While the Tenant Domain establishes business ownership, the Restaurant Domain organizes the day-to-day operational structure of that business through restaurants, branches, business hours, delivery zones, seating areas, physical tables, kitchen stations, staff, riders, and restaurant configuration.

These relationships define how operational resources are organized and inherited throughout the Restaurant Platform.

---

## 4.2 Restaurant Relationship Hierarchy

```text
Restaurant

├── Branches
│
├── Restaurant Settings
│
├── Menus
│
└── Branch
      │
      ├── Business Hours
      ├── Delivery Zones
      ├── Seating Areas
      │      └── Restaurant Tables
      ├── Kitchen Stations
      ├── Staff
      ├── Riders
      └── Rider Assignments
```

---

# Restaurant Relationship Specifications

---

# Relationship 36

## Relationship Name

Restaurant Owns Branches

---

## Purpose

Defines the operational relationship between a restaurant and its physical branches.

---

## Relationship Type

One-to-Many

---

## Business Domain

Restaurant

---

## Parent Table

restaurants

---

## Child Table

branches

---

## Ownership

Restaurant

---

## Cardinality

1:N

---

## Foreign Key

restaurant_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Restaurant Created

↓

Branch Created

↓

Operational

↓

Archived
```

---

## Engineering Notes

A restaurant may operate one or many branches, but every branch belongs to exactly one restaurant.

---

# Relationship 37

## Relationship Name

Restaurant Owns Settings

---

## Purpose

Associates operational configuration with a restaurant.

---

## Relationship Type

One-to-One

---

## Business Domain

Restaurant

---

## Parent Table

restaurants

---

## Child Table

restaurant_settings

---

## Ownership

Restaurant

---

## Cardinality

1:1

---

## Foreign Key

restaurant_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Each restaurant has exactly one configuration record.

---

# Relationship 38

## Relationship Name

Branch Owns Business Hours

---

## Purpose

Defines the operating schedule for each branch.

---

## Relationship Type

One-to-Many

---

## Business Domain

Restaurant

---

## Parent Table

branches

---

## Child Table

business_hours

---

## Ownership

Branch

---

## Cardinality

1:N

---

## Foreign Key

branch_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Branch Created

↓

Schedule Configured

↓

Updated
```

---

## Engineering Notes

Supports recurring schedules and holiday overrides.

---

# Relationship 39

## Relationship Name

Branch Owns Delivery Zones

---

## Purpose

Associates delivery coverage areas with a branch.

---

## Relationship Type

One-to-Many

---

## Business Domain

Restaurant

---

## Parent Table

branches

---

## Child Table

delivery_zones

---

## Ownership

Branch

---

## Cardinality

1:N

---

## Foreign Key

branch_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Zone Created

↓

Active

↓

Modified

↓

Archived
```

---

## Engineering Notes

Multiple delivery zones may exist per branch.

---

# Relationship 40

## Relationship Name

Branch Owns Seating Areas

---

## Purpose

Organizes physical dining spaces within a branch.

---

## Relationship Type

One-to-Many

---

## Business Domain

Restaurant

---

## Parent Table

branches

---

## Child Table

seating_areas

---

## Ownership

Branch

---

## Cardinality

1:N

---

## Foreign Key

branch_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Area Created

↓

Operational

↓

Archived
```

---

## Engineering Notes

Supports indoor, outdoor, VIP, and future dining layouts.

---

# Relationship 41

## Relationship Name

Seating Area Owns Restaurant Tables

---

## Purpose

Associates physical dining tables with a seating area.

---

## Relationship Type

One-to-Many

---

## Business Domain

Restaurant

---

## Parent Table

seating_areas

---

## Child Table

restaurant_tables

---

## Ownership

Seating Area

---

## Cardinality

1:N

---

## Foreign Key

seating_area_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Table Added

↓

Available

↓

Unavailable

↓

Archived
```

---

## Engineering Notes

Supports table assignment and QR ordering.

---

# Relationship 42

## Relationship Name

Branch Owns Kitchen Stations

---

## Purpose

Associates kitchen workstations with a branch.

---

## Relationship Type

One-to-Many

---

## Business Domain

Restaurant

---

## Parent Table

branches

---

## Child Table

kitchen_stations

---

## Ownership

Branch

---

## Cardinality

1:N

---

## Foreign Key

branch_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Station Created

↓

Operational

↓

Inactive

↓

Archived
```

---

## Engineering Notes

Supports future Kitchen Display System (KDS) routing.

---

# Relationship 43

## Relationship Name

Branch Employs Staff

---

## Purpose

Associates restaurant employees with their operating branch.

---

## Relationship Type

One-to-Many

---

## Business Domain

Restaurant

---

## Parent Table

branches

---

## Child Table

staff

---

## Ownership

Branch

---

## Cardinality

1:N

---

## Foreign Key

branch_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Staff Invited

↓

Active

↓

Suspended

↓

Archived
```

---

## Engineering Notes

Authentication is managed separately through the Identity Domain.

---

# Relationship 44

## Relationship Name

Branch Owns Riders

---

## Purpose

Associates delivery personnel with a branch.

---

## Relationship Type

One-to-Many

---

## Business Domain

Restaurant

---

## Parent Table

branches

---

## Child Table

riders

---

## Ownership

Branch

---

## Cardinality

1:N

---

## Foreign Key

branch_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Rider Registered

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

## Engineering Notes

Riders may later support branch transfers while maintaining historical assignment records.

---

# Relationship 45

## Relationship Name

Rider Owns Rider Assignments

---

## Purpose

Maintains the assignment history for each rider.

---

## Relationship Type

One-to-Many

---

## Business Domain

Restaurant

---

## Parent Table

riders

---

## Child Table

rider_assignments

---

## Ownership

Rider

---

## Cardinality

1:N

---

## Foreign Key

rider_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Assignment Created

↓

Accepted

↓

Completed

↓

Closed
```

---

## Engineering Notes

Preserves historical rider assignment records for reporting and operational analytics.

---

# Relationship 46

## Relationship Name

Branch Receives Rider Assignments

---

## Purpose

Associates rider assignments with the branch where deliveries originate.

---

## Relationship Type

One-to-Many

---

## Business Domain

Restaurant

---

## Parent Table

branches

---

## Child Table

rider_assignments

---

## Ownership

Branch

---

## Cardinality

1:N

---

## Foreign Key

branch_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Assignment Created

↓

Delivery Executed

↓

Assignment Archived
```

---

## Engineering Notes

Allows historical reporting of rider activity by branch.

---

# Relationship 47

## Relationship Name

Restaurant Owns Menus

---

## Purpose

Associates customer-facing menus with a restaurant.

---

## Relationship Type

One-to-Many

---

## Business Domain

Restaurant

---

## Parent Table

restaurants

---

## Child Table

menus

---

## Ownership

Restaurant

---

## Cardinality

1:N

---

## Foreign Key

restaurant_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Menu Created

↓

Published

↓

Active

↓

Archived
```

---

## Engineering Notes

Menus belong to the restaurant and may later be assigned to one or more branches depending on operational requirements.

---

# Chapter 4 Summary

The **Restaurant Domain** establishes the operational hierarchy beneath each restaurant. These relationships organize branches, schedules, delivery zones, dining infrastructure, kitchen operations, staff, riders, and menus into a coherent ownership model. Together they ensure that operational resources inherit ownership from the restaurant while remaining logically grouped by branch, providing the structural foundation for ordering, reservations, delivery management, and day-to-day restaurant operations across the FluxDine platform.

# Chapter 5 — Commerce Relationships

## 5.1 Purpose

The Commerce Domain defines the relationships that govern the complete online ordering lifecycle within the FluxDine platform.

These relationships organize menus, products, customer carts, and finalized orders into a structured commerce hierarchy while preserving tenant isolation and historical accuracy.

The Commerce Domain is responsible for:

- Menu organization
- Product hierarchy
- Customer carts
- Checkout
- Order history
- Product customization

---

## 5.2 Commerce Relationship Hierarchy

```text
Restaurant

└── Menus
      │
      ├── Menu Categories
      │      │
      │      ├── Menu Items
      │      │      ├── Item Variants
      │      │      ├── Item Options
      │      │      └── Item Add-ons
      │
      ├── Carts
      │      └── Cart Items
      │
      └── Orders
             └── Order Items
```

---

# Commerce Relationship Specifications

---

# Relationship 48

## Relationship Name

Restaurant Owns Menus

---

## Purpose

Associates customer-facing menus with a restaurant.

---

## Relationship Type

One-to-Many

---

## Business Domain

Commerce

---

## Parent Table

restaurants

---

## Child Table

menus

---

## Ownership

Restaurant

---

## Cardinality

1:N

---

## Foreign Key

restaurant_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Restaurant Created

↓

Menu Created

↓

Published

↓

Archived
```

---

## Engineering Notes

Menus inherit ownership from the restaurant and may later be assigned to one or more branches.

---

# Relationship 49

## Relationship Name

Menu Owns Categories

---

## Purpose

Organizes menu items into logical customer-facing categories.

---

## Relationship Type

One-to-Many

---

## Business Domain

Commerce

---

## Parent Table

menus

---

## Child Table

menu_categories

---

## Ownership

Menu

---

## Cardinality

1:N

---

## Foreign Key

menu_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Menu Created

↓

Category Added

↓

Published

↓

Archived
```

---

## Engineering Notes

Categories cannot exist without a menu.

---

# Relationship 50

## Relationship Name

Category Owns Menu Items

---

## Purpose

Associates products with a menu category.

---

## Relationship Type

One-to-Many

---

## Business Domain

Commerce

---

## Parent Table

menu_categories

---

## Child Table

menu_items

---

## Ownership

Category

---

## Cardinality

1:N

---

## Foreign Key

category_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Category Created

↓

Product Added

↓

Published

↓

Unavailable

↓

Archived
```

---

## Engineering Notes

A product belongs to one category at a time.

---

# Relationship 51

## Relationship Name

Menu Item Owns Variants

---

## Purpose

Associates purchasable variants with a product.

---

## Relationship Type

One-to-Many

---

## Business Domain

Commerce

---

## Parent Table

menu_items

---

## Child Table

item_variants

---

## Ownership

Menu Item

---

## Cardinality

1:N

---

## Foreign Key

menu_item_id

---

## Required

No

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Product Created

↓

Variant Added

↓

Available

↓

Archived
```

---

## Engineering Notes

Variants inherit pricing behavior from the parent product.

---

# Relationship 52

## Relationship Name

Menu Item Owns Options

---

## Purpose

Associates configurable options with a menu item.

---

## Relationship Type

One-to-Many

---

## Business Domain

Commerce

---

## Parent Table

menu_items

---

## Child Table

item_options

---

## Ownership

Menu Item

---

## Cardinality

1:N

---

## Foreign Key

menu_item_id

---

## Required

No

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Option Created

↓

Available

↓

Archived
```

---

## Engineering Notes

Options represent customer selections that may or may not affect pricing.

---

# Relationship 53

## Relationship Name

Menu Item Owns Add-ons

---

## Purpose

Associates purchasable add-ons with menu items.

---

## Relationship Type

One-to-Many

---

## Business Domain

Commerce

---

## Parent Table

menu_items

---

## Child Table

item_addons

---

## Ownership

Menu Item

---

## Cardinality

1:N

---

## Foreign Key

menu_item_id

---

## Required

No

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Add-on Created

↓

Available

↓

Archived
```

---

## Engineering Notes

Supports multiple add-ons per product.

---

# Relationship 54

## Relationship Name

Customer Owns Carts

---

## Purpose

Associates shopping carts with customers.

---

## Relationship Type

One-to-Many

---

## Business Domain

Commerce

---

## Parent Table

customers

---

## Child Table

carts

---

## Ownership

Customer

---

## Cardinality

1:N

---

## Foreign Key

customer_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Cart Created

↓

Active

↓

Checked Out

↓

Expired
```

---

## Engineering Notes

Historical carts may be retained for analytics.

---

# Relationship 55

## Relationship Name

Cart Owns Cart Items

---

## Purpose

Associates products with a customer's shopping cart.

---

## Relationship Type

One-to-Many

---

## Business Domain

Commerce

---

## Parent Table

carts

---

## Child Table

cart_items

---

## Ownership

Cart

---

## Cardinality

1:N

---

## Foreign Key

cart_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Item Added

↓

Modified

↓

Removed

↓

Checked Out
```

---

## Engineering Notes

Cart items remain editable until checkout.

---

# Relationship 56

## Relationship Name

Menu Item Referenced By Cart Items

---

## Purpose

Associates products with shopping cart entries.

---

## Relationship Type

Reference

---

## Business Domain

Commerce

---

## Parent Table

menu_items

---

## Child Table

cart_items

---

## Ownership

Menu Item

---

## Cardinality

1:N

---

## Foreign Key

menu_item_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Menu items cannot be physically deleted while referenced by active carts.

---

# Relationship 57

## Relationship Name

Restaurant Owns Orders

---

## Purpose

Associates customer orders with a restaurant.

---

## Relationship Type

One-to-Many

---

## Business Domain

Commerce

---

## Parent Table

restaurants

---

## Child Table

orders

---

## Ownership

Restaurant

---

## Cardinality

1:N

---

## Foreign Key

restaurant_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Order Created

↓

Confirmed

↓

Completed
```

---

## Engineering Notes

Orders are immutable business records.

---

# Relationship 58

## Relationship Name

Branch Processes Orders

---

## Purpose

Associates orders with the branch responsible for fulfillment.

---

## Relationship Type

One-to-Many

---

## Business Domain

Commerce

---

## Parent Table

branches

---

## Child Table

orders

---

## Ownership

Branch

---

## Cardinality

1:N

---

## Foreign Key

branch_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Supports multi-branch restaurant operations.

---

# Relationship 59

## Relationship Name

Customer Places Orders

---

## Purpose

Associates customer purchases with the customer account.

---

## Relationship Type

One-to-Many

---

## Business Domain

Commerce

---

## Parent Table

customers

---

## Child Table

orders

---

## Ownership

Customer

---

## Cardinality

1:N

---

## Foreign Key

customer_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Customer Registered

↓

Order Placed

↓

Order Completed
```

---

## Engineering Notes

Guest checkout may create a temporary customer record.

---

# Relationship 60

## Relationship Name

Order Owns Order Items

---

## Purpose

Associates purchased products with an order.

---

## Relationship Type

One-to-Many

---

## Business Domain

Commerce

---

## Parent Table

orders

---

## Child Table

order_items

---

## Ownership

Order

---

## Cardinality

1:N

---

## Foreign Key

order_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Order Created

↓

Items Added

↓

Order Confirmed

↓

Order Completed
```

---

## Engineering Notes

Order items become immutable once the order is confirmed.

---

# Relationship 61

## Relationship Name

Menu Item Referenced By Order Items

---

## Purpose

Provides historical linkage between purchased products and the originating menu item.

---

## Relationship Type

Reference

---

## Business Domain

Commerce

---

## Parent Table

menu_items

---

## Child Table

order_items

---

## Ownership

Menu Item

---

## Cardinality

1:N

---

## Foreign Key

menu_item_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Product Purchased

↓

Historical Snapshot Stored

↓

Relationship Preserved
```

---

## Engineering Notes

Order items store a snapshot of product details at purchase time to preserve historical accuracy even if the menu item changes later.

---

# Chapter 6 — Customer Relationships

## 6.1 Purpose

The Customer Domain manages relationships associated with customer identity, preferences, loyalty, communication, and engagement.

These relationships establish a persistent customer profile for each tenant while supporting personalized experiences and long-term customer retention.

---

## 6.2 Customer Relationship Hierarchy

```text
Customer

├── Customer Addresses
├── Customer Preferences
├── Customer Favorites
├── Loyalty Accounts
│      ├── Loyalty Transactions
│      └── Reward Points
├── Customer Devices
└── Communication Preferences
```

---

# Customer Relationship Specifications

---

# Relationship 62

## Relationship Name

Customer Owns Addresses

---

## Purpose

Associates delivery addresses with a customer.

---

## Relationship Type

One-to-Many

---

## Business Domain

Customer

---

## Parent Table

customers

---

## Child Table

customer_addresses

---

## Ownership

Customer

---

## Cardinality

1:N

---

## Foreign Key

customer_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Supports multiple saved delivery addresses.

---

# Relationship 63

## Relationship Name

Customer Owns Preferences

---

## Purpose

Stores customer-specific ordering and communication preferences.

---

## Relationship Type

One-to-One

---

## Business Domain

Customer

---

## Parent Table

customers

---

## Child Table

customer_preferences

---

##Ownership

Customer

---

## Cardinality

1:1

---

## Foreign Key

customer_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Each customer has one preference profile.

---

# Relationship 64

## Relationship Name

Customer Owns Favorites

---

## Purpose

Associates favorite menu items with a customer.

---

## Relationship Type

One-to-Many

---

## Business Domain

Customer

---

## Parent Table

customers

---

## Child Table

customer_favorites

---

## Ownership

Customer

---

## Cardinality

1:N

---

## Foreign Key

customer_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Supports personalized ordering experiences.

---

# Relationship 65

## Relationship Name

Customer Owns Loyalty Account

---

## Purpose

Associates a loyalty account with a customer.

---

## Relationship Type

One-to-One

---

## Business Domain

Customer

---

## Parent Table

customers

---

## Child Table

loyalty_accounts

---

## Ownership

Customer

---

## Cardinality

1:1

---

## Foreign Key

customer_id

---

## Required

No

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Allowed

---

## Engineering Notes

Not every customer participates in a loyalty program.

---

# Relationship 66

## Relationship Name

Loyalty Account Owns Transactions

---

## Purpose

Stores loyalty earning and redemption history.

---

## Relationship Type

One-to-Many

---

## Business Domain

Customer

---

## Parent Table

loyalty_accounts

---

## Child Table

loyalty_transactions

---

## Ownership

Loyalty Account

---

## Cardinality

1:N

---

## Foreign Key

loyalty_account_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Provides a complete audit trail for loyalty activity.

---

# Relationship 67

## Relationship Name

Loyalty Account Owns Reward Points

---

## Purpose

Associates the current point balance with a loyalty account.

---

## Relationship Type

One-to-One

---

## Business Domain

Customer

---

## Parent Table

loyalty_accounts

---

## Child Table

reward_points

---

## Ownership

Loyalty Account

---

## Cardinality

1:1

---

## Foreign Key

loyalty_account_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Historical point movements remain in loyalty_transactions.

---

# Relationship 68

## Relationship Name

Customer Owns Devices

---

## Purpose

Associates registered devices with customers.

---

## Relationship Type

One-to-Many

---

## Business Domain

Customer

---

## Parent Table

customers

---

## Child Table

customer_devices

---

## Ownership

Customer

---

## Cardinality

1:N

---

## Foreign Key

customer_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Supports trusted devices and push notifications.

---

# Relationship 69

## Relationship Name

Customer Owns Communication Preferences

---

## Purpose

Stores communication consent and notification preferences.

---

## Relationship Type

One-to-One

---

## Business Domain

Customer

---

## Parent Table

customers

---

## Child Table

communication_preferences

---

## Ownership

Customer

---

## Cardinality

1:1

---

## Foreign Key

customer_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Supports privacy regulations and customer consent management.

---

# Phase 5 Summary

Phase 5 defines the logical relationships for the **Commerce** and **Customer** domains, covering the complete journey from menu organization and product configuration through shopping carts, order processing, and long-term customer engagement. These relationships establish the transactional core of the Restaurant Platform while ensuring historical integrity, customer personalization, and consistent ownership throughout the ordering lifecycle.

# Chapter 7 — Reservation Relationships

## 7.1 Purpose

The Reservation Domain defines the logical relationships that govern table reservations, seating assignments, waiting lists, and reservation history within the FluxDine platform.

These relationships support the complete reservation lifecycle while maintaining historical integrity and ensuring that reservation data remains isolated within the owning tenant.

The Reservation Domain is responsible for:

- Customer reservations
- Table allocation
- Waitlist management
- Reservation history
- Reservation analytics

---

## 7.2 Reservation Relationship Hierarchy

```text
Customer
      │
      └── Reservations
              │
              ├── Reservation Tables
              └── Reservation Events

Branch
      │
      └── Waitlists
```

---

# Reservation Relationship Specifications

---

# Relationship 70

## Relationship Name

Customer Creates Reservations

---

## Purpose

Associates reservations with the customer who created them.

---

## Relationship Type

One-to-Many

---

## Business Domain

Reservation

---

## Parent Table

customers

---

## Child Table

reservations

---

## Ownership

Customer

---

## Cardinality

1:N

---

## Foreign Key

customer_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Customer Registered

↓

Reservation Created

↓

Reservation Completed
```

---

## Engineering Notes

Historical reservations remain preserved even if customer accounts are later deactivated.

---

# Relationship 71

## Relationship Name

Branch Receives Reservations

---

## Purpose

Associates reservations with the branch where dining will occur.

---

## Relationship Type

One-to-Many

---

## Business Domain

Reservation

---

## Parent Table

branches

---

## Child Table

reservations

---

## Ownership

Branch

---

## Cardinality

1:N

---

## Foreign Key

branch_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Reservation Created

↓

Confirmed

↓

Completed
```

---

## Engineering Notes

Reservations belong to a single operating branch.

---

# Relationship 72

## Relationship Name

Reservation Owns Reservation Tables

---

## Purpose

Associates reserved dining tables with a reservation.

---

## Relationship Type

One-to-Many

---

## Business Domain

Reservation

---

## Parent Table

reservations

---

## Child Table

reservation_tables

---

## Ownership

Reservation

---

## Cardinality

1:N

---

## Foreign Key

reservation_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Reservation Confirmed

↓

Tables Assigned

↓

Reservation Completed
```

---

## Engineering Notes

Supports assigning one or multiple physical tables to a reservation.

---

# Relationship 73

## Relationship Name

Restaurant Table Assigned To Reservation

---

## Purpose

Associates physical restaurant tables with reservation assignments.

---

## Relationship Type

Reference

---

## Business Domain

Reservation

---

## Parent Table

restaurant_tables

---

## Child Table

reservation_tables

---

## Ownership

Restaurant Table

---

## Cardinality

1:N

---

## Foreign Key

table_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Maintains historical seating assignments.

---

# Relationship 74

## Relationship Name

Reservation Owns Reservation Events

---

## Purpose

Stores the complete lifecycle history of every reservation.

---

## Relationship Type

One-to-Many

---

## Business Domain

Reservation

---

## Parent Table

reservations

---

## Child Table

reservation_events

---

## Ownership

Reservation

---

## Cardinality

1:N

---

## Foreign Key

reservation_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Created

↓

Confirmed

↓

Upcoming

↓

Active

↓

Completed
```

---

## Engineering Notes

Every significant reservation state change creates an immutable event.

---

# Relationship 75

## Relationship Name

Branch Owns Waitlists

---

## Purpose

Associates waiting lists with restaurant branches.

---

## Relationship Type

One-to-Many

---

## Business Domain

Reservation

---

## Parent Table

branches

---

## Child Table

waitlists

---

## Ownership

Branch

---

## Cardinality

1:N

---

## Foreign Key

branch_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Relationship Lifecycle

```text
Customer Added

↓

Waiting

↓

Seated

↓

Removed
```

---

## Engineering Notes

Waitlists operate independently of confirmed reservations.

---

# Chapter 8 — Billing Relationships

## 8.1 Purpose

The Billing Domain manages the commercial relationship between tenants and the FluxDine platform.

These relationships govern subscriptions, invoicing, credits, and refund requests.

---

## 8.2 Billing Relationship Hierarchy

```text
Tenant

↓

Billing Account

├── Subscriptions
│      └── Invoices
│              ├── Invoice Items
│              ├── Credit Notes
│              └── Refund Requests
```

---

# Billing Relationship Specifications

---

# Relationship 76

## Relationship Name

Tenant Owns Billing Account

---

## Purpose

Associates every tenant with a billing account.

---

## Relationship Type

One-to-One

---

## Business Domain

Billing

---

## Parent Table

tenants

---

## Child Table

billing_accounts

---

## Ownership

Tenant

---

## Cardinality

1:1

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Every tenant has exactly one billing account.

---

# Relationship 77

## Relationship Name

Billing Account Owns Subscriptions

---

## Purpose

Associates subscriptions with billing accounts.

---

## Relationship Type

One-to-Many

---

## Parent Table

billing_accounts

---

## Child Table

subscriptions

---

## Ownership

Billing Account

---

## Cardinality

1:N

---

## Foreign Key

billing_account_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Supports historical subscription records.

---

# Relationship 78

## Relationship Name

Subscription Owns Invoices

---

## Purpose

Associates invoices with subscription billing cycles.

---

## Relationship Type

One-to-Many

---

## Business Domain

Billing

---

## Parent Table

subscriptions

---

## Child Table

invoices

---

## Ownership

Subscription

---

## Cardinality

1:N

---

## Foreign Key

subscription_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Invoices are immutable financial documents.

---

# Relationship 79

## Relationship Name

Invoice Owns Invoice Items

---

## Purpose

Associates billing line items with invoices.

---

## Relationship Type

One-to-Many

---

## Parent Table

invoices

---

## Child Table

invoice_items

---

## Ownership

Invoice

---

## Cardinality

1:N

---

## Foreign Key

invoice_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Invoice totals are derived from invoice items.

---

# Relationship 80

## Relationship Name

Invoice Owns Credit Notes

---

## Purpose

Associates credit notes with invoices.

---

## Relationship Type

One-to-Many

---

## Business Domain

Billing

---

## Parent Table

invoices

---

## Child Table

credit_notes

---

## Ownership

Invoice

---

## Cardinality

1:N

---

## Foreign Key

invoice_id

---

## Required

No

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Allowed

---

## Engineering Notes

Credit notes adjust invoice balances while preserving financial history.

---

# Relationship 81

## Relationship Name

Invoice Owns Refund Requests

---

## Purpose

Associates refund requests with invoices.

---

## Relationship Type

One-to-Many

---

## Business Domain

Billing

---

## Parent Table

invoices

---

## Child Table

refund_requests

---

## Ownership

Invoice

---

## Cardinality

1:N

---

## Foreign Key

invoice_id

---

## Required

No

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Allowed

---

## Engineering Notes

Refund requests follow an approval workflow before payment processing.

---

# Chapter 9 — Payment Relationships

## 9.1 Purpose

The Payment Domain implements the Payment Gateway Abstraction Layer and governs all payment processing relationships.

These relationships remain provider-independent while supporting multiple payment gateways and merchant accounts.

---

## 9.2 Payment Relationship Hierarchy

```text
Payment Providers

└── Gateway Configurations

        └── Merchant Accounts

                └── Payment Transactions

                        ├── Payment Intents
                        ├── Refund Transactions
                        ├── Settlement Records
                        │       └── Reconciliation Records
                        └── Webhook Events
```

---

# Payment Relationship Specifications

---

# Relationship 82

## Relationship Name

Payment Provider Owns Gateway Configurations

---

## Purpose

Associates supported payment providers with tenant gateway configurations.

---

## Relationship Type

One-to-Many

---

## Business Domain

Payment

---

## Parent Table

payment_providers

---

## Child Table

gateway_configurations

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

payment_provider_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Supports multiple tenant configurations for the same provider.

---

# Relationship 83

## Relationship Name

Gateway Configuration Owns Merchant Accounts

---

## Relationship Type

One-to-Many

---

## Parent Table

gateway_configurations

---

## Child Table

merchant_accounts

---

## Ownership

Gateway Configuration

---

## Cardinality

1:N

---

## Foreign Key

gateway_configuration_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

# Relationship 84

## Relationship Name

Merchant Account Owns Payment Transactions

---

## Relationship Type

One-to-Many

---

## Parent Table

merchant_accounts

---

## Child Table

payment_transactions

---

## Ownership

Merchant Account

---

## Cardinality

1:N

---

## Foreign Key

merchant_account_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Engineering Notes

Every payment transaction belongs to exactly one merchant account.

---

# Relationship 85

## Relationship Name

Payment Transaction Owns Payment Intent

---

## Relationship Type

One-to-One

---

## Parent Table

payment_transactions

---

## Child Table

payment_intents

---

## Ownership

Payment Transaction

---

## Cardinality

1:1

---

## Foreign Key

payment_transaction_id

---

## Required

No

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Engineering Notes

Only providers supporting two-step authorization create payment intents.

---

# Relationship 86

## Relationship Name

Payment Transaction Owns Refund Transactions

---

## Relationship Type

One-to-Many

---

## Parent Table

payment_transactions

---

## Child Table

refund_transactions

---

## Ownership

Payment Transaction

---

## Cardinality

1:N

---

## Foreign Key

payment_transaction_id

---

## Required

No

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

# Relationship 87

## Relationship Name

Payment Transaction Owns Settlement Records

---

## Relationship Type

One-to-Many

---

## Parent Table

payment_transactions

---

## Child Table

settlement_records

---

## Ownership

Payment Transaction

---

## Cardinality

1:N

---

## Foreign Key

payment_transaction_id

---

## Required

No

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

# Relationship 88

## Relationship Name

Settlement Record Owns Reconciliation Records

---

## Relationship Type

One-to-Many

---

## Parent Table

settlement_records

---

## Child Table

reconciliation_records

---

## Ownership

Settlement Record

---

## Cardinality

1:N

---

## Foreign Key

settlement_record_id

---

## Required

No

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

# Relationship 89

## Relationship Name

Payment Provider Generates Webhook Events

---

## Purpose

Associates inbound webhook events with the originating payment provider.

---

## Relationship Type

One-to-Many

---

## Business Domain

Payment

---

## Parent Table

payment_providers

---

## Child Table

webhook_events

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

payment_provider_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Engineering Notes

Webhook events provide asynchronous updates for payment lifecycle changes and are processed independently of payment transactions.

---

# Phase 6 Summary

Phase 6 defines the relationships for the **Reservation**, **Billing**, and **Payment** domains. These relationships establish the complete reservation lifecycle, tenant billing hierarchy, and payment gateway abstraction architecture. Together they provide the relational foundation for reservation management, subscription billing, financial transactions, reconciliation, and provider-independent payment processing while preserving auditability and historical integrity throughout the FluxDine platform.

# Chapter 10 — Notification Relationships

## 10.1 Purpose

The Notification Domain defines the logical relationships responsible for delivering communications throughout the FluxDine platform.

Notifications are generated by business events originating from multiple domains, including Commerce, Reservations, Billing, Authentication, and Platform Administration.

This domain provides a centralized communication architecture that supports multiple delivery channels while maintaining a single logical notification record.

The Notification Domain is responsible for:

- Email notifications
- SMS notifications
- Push notifications
- Notification templates
- Delivery queue management
- Notification history

---

## 10.2 Notification Relationship Hierarchy

```text
Notification Templates

        │

        ▼

Notifications

├── Email Messages
├── SMS Messages
├── Push Notifications
└── Notification Queue
```

---

# Notification Relationship Specifications

---

# Relationship 90

## Relationship Name

Notification Template Generates Notifications

---

## Purpose

Associates reusable templates with notifications.

---

## Relationship Type

One-to-Many

---

## Business Domain

Notification

---

## Parent Table

notification_templates

---

## Child Table

notifications

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

notification_template_id

---

## Required

No

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Orphan Policy

Allowed

---

## Engineering Notes

Notifications may originate from reusable templates or dynamically generated content.

---

# Relationship 91

## Relationship Name

Notification Owns Email Messages

---

## Purpose

Associates logical notifications with email delivery records.

---

## Relationship Type

One-to-One

---

## Business Domain

Notification

---

## Parent Table

notifications

---

## Child Table

email_messages

---

## Ownership

Notification

---

## Cardinality

1:1

---

## Foreign Key

notification_id

---

## Required

No

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Engineering Notes

Only notifications delivered through email create email message records.

---

# Relationship 92

## Relationship Name

Notification Owns SMS Messages

---

## Purpose

Associates logical notifications with SMS delivery records.

---

## Relationship Type

One-to-One

---

## Business Domain

Notification

---

## Parent Table

notifications

---

## Child Table

sms_messages

---

## Ownership

Notification

---

## Cardinality

1:1

---

## Foreign Key

notification_id

---

## Required

No

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Engineering Notes

Only SMS notifications create SMS message records.

---

# Relationship 93

## Relationship Name

Notification Owns Push Notifications

---

## Purpose

Associates logical notifications with mobile push delivery records.

---

## Relationship Type

One-to-One

---

## Business Domain

Notification

---

## Parent Table

notifications

---

## Child Table

push_notifications

---

## Ownership

Notification

---

## Cardinality

1:1

---

## Foreign Key

notification_id

---

## Required

No

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Engineering Notes

Supports mobile applications and future desktop push channels.

---

# Relationship 94

## Relationship Name

Notification Owns Queue Entry

---

## Purpose

Associates outbound notifications with asynchronous processing queues.

---

## Relationship Type

One-to-One

---

## Business Domain

Notification

---

## Parent Table

notifications

---

## Child Table

notification_queue

---

## Ownership

Notification

---

## Cardinality

1:1

---

## Foreign Key

notification_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Orphan Policy

Not Allowed

---

## Engineering Notes

Queue entries are temporary operational records processed by the Notification Service.

---

# Chapter 11 — Analytics Relationships

## 11.1 Purpose

The Analytics Domain defines the relationships supporting reporting, KPI calculation, business intelligence, and platform analytics.

Unlike operational domains, Analytics primarily references business entities rather than owning them.

---

## 11.2 Analytics Relationship Hierarchy

```text
KPI Definitions

        │

        ▼

KPI Snapshots

Reports

        │

        ▼

Dashboard Metrics

Usage Metrics
```

---

# Analytics Relationship Specifications

---

# Relationship 95

## Relationship Name

KPI Definition Owns KPI Snapshots

---

## Purpose

Stores historical measurements for defined KPIs.

---

## Relationship Type

One-to-Many

---

## Business Domain

Analytics

---

## Parent Table

kpi_definitions

---

## Child Table

kpi_snapshots

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

kpi_definition_id

---

## Required

Yes

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Engineering Notes

Historical KPI values are immutable.

---

# Relationship 96

## Relationship Name

Report Generates Dashboard Metrics

---

## Purpose

Associates generated reports with dashboard metrics.

---

## Relationship Type

One-to-Many

---

## Parent Table

reports

---

## Child Table

dashboard_metrics

---

## Ownership

Platform

---

## Cardinality

1:N

---

## Foreign Key

report_id

---

## Required

No

---

## Cascade Delete

Restrict

---

## Cascade Update

Cascade

---

## Engineering Notes

Reports may generate multiple dashboard metrics.

---

# Relationship 97

## Relationship Name

Tenant Generates Usage Metrics

---

## Purpose

Associates tenant activity with usage statistics.

---

## Relationship Type

One-to-Many

---

## Business Domain

Analytics

---

## Parent Table

tenants

---

## Child Table

usage_metrics

---

## Ownership

Tenant

---

## Cardinality

1:N

---

## Foreign Key

tenant_id

---

## Required

Yes

---

## Cascade Delete

Cascade

---

## Cascade Update

Cascade

---

## Engineering Notes

Usage metrics support billing, reporting, and capacity planning.

---

# Chapter 12 — Branding Relationships

## 12.1 Purpose

The Branding Domain defines the relationships that control each tenant's visual identity and customer-facing web presence.

---

## 12.2 Branding Relationship Hierarchy

```text
Tenant Branding

├── Themes
├── Logos
├── Brand Assets

Tenant Domains

└── Domains
        └── SEO Settings
```

---

# Branding Relationship Specifications

---

# Relationship 98

## Relationship Name

Tenant Branding Owns Themes

---

## Relationship Type

One-to-Many

---

## Parent Table

tenant_branding

---

## Child Table

themes

---

## Ownership

Tenant

---

## Cardinality

1:N

---

## Foreign Key

tenant_branding_id

---

# Relationship 99

## Relationship Name

Theme Owns Logos

---

## Relationship Type

One-to-Many

---

## Parent Table

themes

---

## Child Table

logos

---

## Ownership

Theme

---

## Cardinality

1:N

---

## Foreign Key

theme_id

---

# Relationship 100

## Relationship Name

Theme Owns Brand Assets

---

## Relationship Type

One-to-Many

---

## Parent Table

themes

---

## Child Table

brand_assets

---

## Ownership

Theme

---

## Cardinality

1:N

---

## Foreign Key

theme_id

---

# Relationship 101

## Relationship Name

Tenant Domain Owns Domains

---

## Relationship Type

One-to-Many

---

## Parent Table

tenant_domains

---

## Child Table

domains

---

## Ownership

Tenant

---

## Cardinality

1:N

---

## Foreign Key

tenant_domain_id

---

# Relationship 102

## Relationship Name

Domain Owns SEO Settings

---

## Relationship Type

One-to-One

---

## Parent Table

domains

---

## Child Table

seo_settings

---

## Ownership

Domain

---

## Cardinality

1:1

---

## Foreign Key

domain_id

---

## Engineering Notes

Each customer-facing domain maintains one SEO configuration.

---

# Chapter 13 — Feature Management Relationships

## 13.1 Purpose

The Feature Management Domain controls feature availability across the entire SaaS platform.

It separates platform capabilities from subscription entitlements and tenant-specific overrides.

---

## 13.2 Feature Relationship Hierarchy

```text
Features

├── Feature Flags
├── Subscription Features
├── Tenant Features
└── Feature Usage
```

---

# Feature Management Relationship Specifications

---

# Relationship 103

## Relationship Name

Feature Owns Feature Flags

---

## Relationship Type

One-to-Many

---

## Parent Table

features

---

## Child Table

feature_flags

---

## Cardinality

1:N

---

## Foreign Key

feature_id

---

# Relationship 104

## Relationship Name

Feature Assigned To Subscription Plans

---

## Relationship Type

Many-to-Many

---

## Parent Table

features

---

## Child Table

subscription_features

---

## Junction Table

subscription_features

---

## Cardinality

N:N

---

# Relationship 105

## Relationship Name

Feature Assigned To Tenants

---

## Relationship Type

Many-to-Many

---

## Parent Table

features

---

## Child Table

tenant_features

---

## Junction Table

tenant_features

---

## Cardinality

N:N

---

# Relationship 106

## Relationship Name

Feature Generates Usage Records

---

## Relationship Type

One-to-Many

---

## Parent Table

features

---

## Child Table

feature_usage

---

## Cardinality

1:N

---

## Foreign Key

feature_id

---

# Chapter 14 — Shared Platform Relationships

## 14.1 Purpose

The Shared Platform Domain contains infrastructure-level relationships used across all business domains.

These services provide common capabilities rather than owning business processes.

---

## 14.2 Shared Platform Relationship Hierarchy

```text
Platform

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

# Shared Platform Relationship Specifications

---

# Relationship 107

## Relationship Name

Background Job Owns Scheduled Tasks

---

## Relationship Type

One-to-Many

---

## Parent Table

background_jobs

---

## Child Table

scheduled_tasks

---

## Cardinality

1:N

---

## Foreign Key

background_job_id

---

# Relationship 108

## Relationship Name

Tenant Registers Webhooks

---

## Relationship Type

One-to-Many

---

## Parent Table

tenants

---

## Child Table

webhook_registrations

---

## Cardinality

1:N

---

## Foreign Key

tenant_id

---

# Relationship 109

## Relationship Name

Tenant Owns API Keys

---

## Relationship Type

One-to-Many

---

## Parent Table

tenants

---

## Child Table

api_keys

---

## Cardinality

1:N

---

## Foreign Key

tenant_id

---

# Relationship 110

## Relationship Name

Platform Generates Audit Logs

---

## Relationship Type

Reference

---

## Parent Table

platform

---

## Child Table

audit_logs

---

## Cardinality

1:N

---

## Engineering Notes

Audit logs may reference entities from any business domain while remaining centrally managed.

---

# Relationship 111

## Relationship Name

Platform Generates Activity Logs

---

## Relationship Type

Reference

---

## Parent Table

platform

---

## Child Table

activity_logs

---

## Cardinality

1:N

---

# Relationship 112

## Relationship Name

Platform Generates Monitoring Events

---

## Relationship Type

Reference

---

## Parent Table

platform

---

## Child Table

monitoring_events

---

## Cardinality

1:N

---

# Relationship 113

## Relationship Name

Platform Stores Files

---

## Relationship Type

Reference

---

## Parent Table

platform

---

## Child Table

files

---

## Cardinality

1:N

---

## Engineering Notes

Files may be referenced by multiple business domains while remaining managed through the Shared Platform.

---

# Phase 7 Summary

Phase 7 completes the remaining operational relationship domains: **Notification**, **Analytics**, **Branding**, **Feature Management**, and **Shared Platform**. These relationships define the cross-cutting services that support every application within the FluxDine ecosystem, including communication, reporting, branding, feature governance, auditing, monitoring, file management, and background processing. Together with the previous phases, they complete the domain-level relationship specifications for all major business areas of the platform.

# Chapter 15 — Cross-Domain Relationships

15.1 Purpose

Explain the purpose.

Example:

The previous chapters define relationships within individual business domains.

This chapter explains how those domains interact while preserving domain boundaries.

It does not redefine relationships already documented elsewhere.

15.2 Domain Communication Overview

Keep your architecture diagram.

Identity

↓

Tenant

↓

Restaurant

↓

Commerce

↓

Payment

↓

Notification

↓

Analytics

No changes.

15.3 Domain Reference Matrix

Now replace Relationship 114–126 with a table.

Example:

Source Domain	Target Domain	Existing Relationship	Reference
Identity	Tenant	User Membership	Relationship 16
Tenant	Restaurant	Tenant Owns Restaurants	Relationship 24
Restaurant	Commerce	Restaurant Owns Menus	Relationship 47
Customer	Commerce	Customer Places Orders	Relationship 59
Customer	Reservation	Customer Creates Reservations	Relationship 70
Commerce	Payment	Order Creates Payment Transaction	Relationship 122
Billing	Payment	Invoice Creates Payment Transaction	Relationship 123
Commerce	Notification	Business Events Generate Notifications	Relationship 124
Commerce	Analytics	Business Events Generate Analytics	Relationship 125
Shared Platform	Multiple Domains	Shared Files	Relationship 126

Notice:

No duplicate definitions.

No repeated templates.

Just references.

15.4 Communication Rules

Then keep your rules.

Rule 1

Relationships must remain explicit.

---

Rule 2

No circular dependencies.

---

Rule 3

Shared Platform services never own business entities.

---

Rule 4

Analytics is read-only.

---

Rule 5

Notifications reference entities without owning them.

---

Rule 6

Business ownership never crosses tenant boundaries.
15.5 Dependency Flow

Keep a larger architecture diagram.

Platform

│

├── Identity

│

├── Tenant

│     │

│     ├── Restaurant

│     │      │

│     │      ├── Commerce

│     │      ├── Reservation

│     │      └── Branding

│     │

│     ├── Billing

│     │      │

│     │      └── Payment

│     │

│     └── Feature Management

│

├── Notification

│

└── Analytics
15.6 Engineering Principles

Finish with principles like:

Domains communicate only through documented relationships.
Parent domains do not depend on child domains.
Cross-domain communication must preserve tenant isolation.
Circular dependencies are prohibited.
Analytics consumes operational data but never owns it.
Shared Platform provides infrastructure services without owning business entities.
---

# Phase 8 Summary

Phase 8 defines the **cross-domain relationship architecture** that connects the previously isolated business domains into a unified platform. It establishes how Identity, Tenant, Restaurant, Commerce, Reservation, Billing, Payment, Notification, Analytics, and Shared Platform interact while preserving domain boundaries, ownership rules, and tenant isolation. These relationships complete the logical connectivity of the FluxDine database and provide the architectural bridge between domain-specific relationships and the engineering governance chapters that follow.

# Chapter 16 — Ownership Inheritance

## 16.1 Purpose

Ownership inheritance defines how ownership propagates through relationships within the FluxDine database.

Rather than assigning ownership individually to every table, ownership is inherited from parent entities according to the platform's architectural hierarchy.

This ensures:

- Consistent authorization
- Tenant isolation
- Predictable data access
- Simplified security
- Referential consistency

Ownership inheritance is one of the core architectural principles of the FluxDine platform.

---

# 16.2 Ownership Hierarchy

Every persistent business entity ultimately inherits ownership from the Platform.

```text
Platform
    │
    ▼
Tenant
    │
    ▼
Restaurant
    │
    ▼
Branch
    │
    ├──────────────┐
    ▼              ▼
Commerce     Reservation
    │              │
    ▼              ▼
Payment     Notification
```

---

# 16.3 Ownership Levels

## Platform Ownership

Platform-owned entities belong to the FluxDine platform itself.

Examples:

- countries
- currencies
- languages
- subscription_plans
- global_roles
- global_permissions
- payment_providers
- notification_templates
- features

Platform-owned records are globally shared.

---

## Tenant Ownership

Tenant-owned entities belong to one customer organization.

Examples

- restaurants
- tenant_settings
- tenant_domains
- tenant_branding
- tenant_features
- billing_accounts

A tenant may never access another tenant's records.

---

## Restaurant Ownership

Restaurant-owned entities belong to one restaurant.

Examples

- branches
- menus
- restaurant_settings

Ownership is inherited from the tenant.

---

## Branch Ownership

Branch-owned entities belong to one restaurant branch.

Examples

- business_hours
- delivery_zones
- seating_areas
- restaurant_tables
- staff
- riders

---

## Business Entity Ownership

Business entities inherit ownership automatically.

Examples

- orders
- reservations
- customers
- carts
- notifications

---

# 16.4 Ownership Inheritance Rules

## Rule 1

Every entity has exactly one logical owner.

---

## Rule 2

Ownership never changes after creation.

---

## Rule 3

Child entities inherit ownership from their parent.

---

## Rule 4

Ownership inheritance shall never cross tenant boundaries.

---

## Rule 5

Reference tables do not transfer ownership.

---

## Rule 6

Shared Platform tables remain Platform-owned.

---

# Chapter 17 — Cascade Rules

## 17.1 Purpose

Cascade rules define the expected behavior when parent records are updated or deleted.

These rules describe architectural expectations only.

Actual database implementation is specified in **04 Constraints.md**.

---

# 17.2 Cascade Philosophy

FluxDine favors:

- Historical preservation
- Restrictive deletion
- Immutable business records
- Explicit cleanup

Automatic deletion shall be used only where appropriate.

---

# 17.3 Delete Behaviors

The following delete behaviors are recognized.

| Behavior | Description |
|-----------|-------------|
| Restrict | Parent cannot be deleted |
| Cascade | Child deleted automatically |
| Set Null | Child retained without parent |
| No Action | Application manages behavior |

---

# 17.4 Standard Cascade Matrix

| Parent | Child | Delete Behavior |
|----------|-------|----------------|
| Tenant | Restaurant | Restrict |
| Restaurant | Branch | Restrict |
| Branch | Orders | Restrict |
| Order | Order Items | Restrict |
| Customer | Customer Preferences | Cascade |
| Customer | Customer Devices | Cascade |
| Menu | Menu Categories | Cascade |
| Menu Category | Menu Items | Restrict |
| Reservation | Reservation Events | Cascade |
| Reservation | Reservation Tables | Cascade |
| Invoice | Invoice Items | Cascade |
| Payment Transaction | Payment Intent | Cascade |
| Payment Transaction | Refund Transactions | Restrict |
| Notification | Email Message | Cascade |
| Notification | SMS Message | Cascade |
| Notification | Push Notification | Cascade |

---

# 17.5 Update Rules

Primary identifiers should rarely change.

Where updates occur:

- Foreign keys cascade.
- Business ownership remains unchanged.
- Historical integrity is preserved.

---

# Chapter 18 — Relationship Naming Standards

## 18.1 Purpose

This chapter standardizes naming conventions for every logical relationship.

---

# 18.2 Foreign Key Naming

Foreign keys shall follow:

```text
<parent_table>_id
```

Examples

```text
tenant_id

restaurant_id

branch_id

menu_id

customer_id

reservation_id

order_id

invoice_id

payment_transaction_id
```

---

# 18.3 Junction Table Naming

Junction tables use plural business names.

Examples

```text
user_roles

role_permissions

subscription_features

tenant_features

reservation_tables
```

---

# 18.4 Relationship Names

Relationship names should describe business meaning.

Preferred examples

```text
Tenant Owns Restaurants

Customer Places Orders

Reservation Owns Events

Order Contains Items

Feature Assigned To Subscription
```

Avoid technical names.

---

# 18.5 Navigation Naming

Repository and ORM navigation should mirror business terminology.

Examples

```text
tenant.restaurants

restaurant.branches

branch.orders

order.orderItems

customer.reservations
```

---

# Chapter 19 — Engineering Rules

## 19.1 Purpose

This chapter establishes mandatory engineering rules governing database relationships.

---

# 19.2 Mandatory Rules

Every relationship shall:

- Have one logical parent.
- Have one documented business purpose.
- Follow domain boundaries.
- Support tenant isolation.
- Be implementation independent.
- Be documented before implementation.

---

## 19.3 Referential Integrity Rules

Relationships shall never create:

- orphan records
- circular ownership
- hidden dependencies
- duplicate ownership

---

## 19.4 Multi-Tenant Rules

Tenant-owned entities:

- cannot reference another tenant's records
- cannot inherit ownership from multiple tenants
- cannot bypass tenant boundaries

---

## 19.5 Engineering Principles

Relationship design emphasizes:

- High cohesion
- Low coupling
- Domain-driven design
- Explicit ownership
- Historical integrity
- Scalability
- Predictability

---

# Chapter 20 — Architecture Decision Records

The following ADRs govern relationship design throughout the FluxDine platform.

---

## ADR-R-001

Every relationship shall have one logical parent.

---

## ADR-R-002

Every entity shall have one logical owner.

---

## ADR-R-003

Ownership inheritance shall be deterministic.

---

## ADR-R-004

Reference tables shall not transfer ownership.

---

## ADR-R-005

Tenant isolation shall never be violated.

---

## ADR-R-006

Business records shall favor Restrict over Cascade deletion.

---

## ADR-R-007

Many-to-many relationships shall always use explicit junction tables.

---

## ADR-R-008

Cross-domain relationships shall be explicitly documented.

---

## ADR-R-009

Circular relationship dependencies are prohibited.

---

## ADR-R-010

Relationship specifications are the authoritative source for logical database relationships.

---

# Phase 9 Summary

Phase 9 establishes the engineering governance for all database relationships within the FluxDine platform. It defines ownership inheritance, cascade behavior, relationship naming conventions, mandatory engineering rules, and the architectural decisions that govern relational design. Together, these chapters ensure that every relationship documented in this specification follows a consistent, secure, scalable, and implementation-independent model, providing the governance foundation for **04 Constraints.md**, **05 Index Specification.md**, and **08 Drizzle ORM Mapping.md**.

---


# Appendix A — Complete Relationship Matrix

The following matrix provides a consolidated view of every logical relationship defined within the FluxDine platform.

This appendix serves as a high-level engineering reference and should be used alongside the detailed relationship specifications contained in Chapters 1–20.

---

## Platform Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| countries | tenants | 1:N |
| countries | restaurants | 1:N |
| currencies | billing_accounts | 1:N |
| currencies | restaurants | 1:N |
| languages | users | 1:N |
| languages | customers | 1:N |
| timezones | tenants | 1:N |
| timezones | branches | 1:N |
| subscription_plans | subscriptions | 1:N |
| features | subscription_features | N:N |
| features | tenant_features | N:N |
| global_roles | role_permissions | N:N |
| global_permissions | role_permissions | N:N |
| email_templates | notifications | 1:N |
| notification_templates | notifications | 1:N |

---

## Identity Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| users | user_memberships | 1:N |
| users | sessions | 1:N |
| users | authentication_providers | 1:N |
| users | mfa_configurations | 1:N |
| users | password_reset_tokens | 1:N |
| users | email_verification_tokens | 1:N |
| roles | user_roles | N:N |
| permissions | role_permissions | N:N |

---

## Tenant Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| tenants | restaurants | 1:N |
| tenants | tenant_settings | 1:1 |
| tenants | tenant_domains | 1:N |
| tenants | tenant_branding | 1:1 |
| tenants | tenant_features | 1:N |
| tenants | tenant_payment_gateways | 1:N |
| tenants | tenant_integrations | 1:N |
| tenants | tenant_subscriptions | 1:1 |
| tenants | tenant_usage_metrics | 1:N |
| tenants | tenant_api_keys | 1:N |
| tenants | tenant_audit_settings | 1:1 |
| tenants | tenant_invitations | 1:N |

---

## Restaurant Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| restaurants | branches | 1:N |
| restaurants | restaurant_settings | 1:1 |
| restaurants | menus | 1:N |
| branches | business_hours | 1:N |
| branches | delivery_zones | 1:N |
| branches | seating_areas | 1:N |
| seating_areas | restaurant_tables | 1:N |
| branches | kitchen_stations | 1:N |
| branches | staff | 1:N |
| branches | riders | 1:N |
| riders | rider_assignments | 1:N |
| branches | rider_assignments | 1:N |

---

## Commerce Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| menus | menu_categories | 1:N |
| menu_categories | menu_items | 1:N |
| menu_items | item_variants | 1:N |
| menu_items | item_options | 1:N |
| menu_items | item_addons | 1:N |
| customers | carts | 1:N |
| carts | cart_items | 1:N |
| restaurants | orders | 1:N |
| branches | orders | 1:N |
| customers | orders | 1:N |
| orders | order_items | 1:N |

---

## Customer Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| customers | customer_addresses | 1:N |
| customers | customer_preferences | 1:1 |
| customers | customer_favorites | 1:N |
| customers | loyalty_accounts | 1:1 |
| loyalty_accounts | loyalty_transactions | 1:N |
| loyalty_accounts | reward_points | 1:1 |
| customers | customer_devices | 1:N |
| customers | communication_preferences | 1:1 |

---

## Reservation Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| customers | reservations | 1:N |
| branches | reservations | 1:N |
| reservations | reservation_tables | 1:N |
| restaurant_tables | reservation_tables | 1:N |
| reservations | reservation_events | 1:N |
| branches | waitlists | 1:N |

---

## Billing Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| tenants | billing_accounts | 1:1 |
| billing_accounts | subscriptions | 1:N |
| subscriptions | invoices | 1:N |
| invoices | invoice_items | 1:N |
| invoices | credit_notes | 1:N |
| invoices | refund_requests | 1:N |

---

## Payment Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| payment_providers | gateway_configurations | 1:N |
| gateway_configurations | merchant_accounts | 1:N |
| merchant_accounts | payment_transactions | 1:N |
| payment_transactions | payment_intents | 1:1 |
| payment_transactions | refund_transactions | 1:N |
| payment_transactions | settlement_records | 1:N |
| settlement_records | reconciliation_records | 1:N |
| payment_providers | webhook_events | 1:N |

---

## Notification Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| notification_templates | notifications | 1:N |
| notifications | email_messages | 1:1 |
| notifications | sms_messages | 1:1 |
| notifications | push_notifications | 1:1 |
| notifications | notification_queue | 1:1 |

---

## Analytics Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| kpi_definitions | kpi_snapshots | 1:N |
| reports | dashboard_metrics | 1:N |
| tenants | usage_metrics | 1:N |

---

## Branding Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| tenant_branding | themes | 1:N |
| themes | logos | 1:N |
| themes | brand_assets | 1:N |
| tenant_domains | domains | 1:N |
| domains | seo_settings | 1:1 |

---

## Feature Management Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| features | feature_flags | 1:N |
| features | subscription_features | N:N |
| features | tenant_features | N:N |
| features | feature_usage | 1:N |

---

## Shared Platform Domain

| Parent | Child | Relationship |
|----------|-------|--------------|
| background_jobs | scheduled_tasks | 1:N |
| tenants | webhook_registrations | 1:N |
| tenants | api_keys | 1:N |

---

# Appendix B — Junction Tables

The following tables implement many-to-many relationships throughout the platform.

| Junction Table | Parent Entities | Purpose |
|----------------|-----------------|---------|
| user_memberships | users ↔ tenants | Multi-tenant membership |
| user_roles | users ↔ roles | User authorization |
| role_permissions | roles ↔ permissions | RBAC permissions |
| subscription_features | subscription_plans ↔ features | Plan feature mapping |
| tenant_features | tenants ↔ features | Tenant feature overrides |
| reservation_tables | reservations ↔ restaurant_tables | Multi-table reservations |

---

# Appendix C — Domain Dependency Graph

The logical dependency hierarchy for the FluxDine platform is illustrated below.

```text
Platform
│
├── Identity
│
├── Tenant
│     │
│     ├── Restaurant
│     │      │
│     │      ├── Commerce
│     │      ├── Reservation
│     │      └── Branding
│     │
│     ├── Billing
│     │      │
│     │      └── Payment
│     │
│     ├── Feature Management
│     │
│     └── Shared Platform
│
├── Notification
│
└── Analytics
```

### Dependency Principles

- Parent domains shall never depend on child domains.
- Dependencies shall flow downward.
- Circular dependencies are prohibited.
- Cross-domain communication shall occur only through documented relationships.
- Shared Platform services may support all domains but shall not own business entities.

---

# Appendix D — Reserved Future Relationships

The following relationships are reserved for future platform capabilities.

## Inventory

```text
Suppliers

↓

Purchase Orders

↓

Inventory Items

↓

Inventory Transactions
```

---

## Kitchen Display System (KDS)

```text
Kitchen Stations

↓

Kitchen Orders

↓

Preparation Events
```

---

## Marketplace

```text
Marketplace Merchants

↓

Marketplace Orders

↓

Marketplace Settlements
```

---

## Artificial Intelligence

```text
Customers

↓

AI Recommendations

↓

AI Conversations

↓

AI Analytics
```

---

## Workforce Management

```text
Staff

↓

Shifts

↓

Attendance

↓

Payroll Profiles
```

---

## Delivery Fleet

```text
Riders

↓

Vehicles

↓

Routes

↓

GPS Tracking
```

---

These future relationships shall be introduced only through approved Architecture Decision Records (ADRs).

---

# References

This document should be read together with the following FluxDine Architecture Bible documents.

## Core Architecture

- 04 System Architecture Blueprint
- 05 Database Architecture & Multi-Tenant Data Model
- 06 API & Service Architecture
- 07 Security Architecture
- 08 Infrastructure Architecture

## Database Engineering

- 00 Database Naming Standards
- 01 Complete Database Schema Specification
- 02 Table Specifications
- 04 Constraints
- 05 Index Specification
- 06 Enum Specification
- 07 Database Migration Strategy
- 08 Drizzle ORM Mapping

## Architecture Governance

- ADR Register
- Architecture Principles
- Documentation Standards

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 0.1 | Initial Draft | FluxDine Engineering | Document structure established |
| 0.4 | Domain Relationships | FluxDine Engineering | Platform through Shared Platform relationships completed |
| 0.8 | Cross-Domain & Governance | FluxDine Engineering | Cross-domain relationships, ownership, cascade rules, engineering standards, and ADRs completed |
| 1.0 | Final Release | FluxDine Engineering | Relationship specification approved for implementation |

---

# Document Completion Summary

This document defines the complete logical relationship model for the FluxDine platform.

It establishes:

- Parent-child ownership
- One-to-one relationships
- One-to-many relationships
- Many-to-many relationships
- Junction tables
- Cross-domain relationships
- Ownership inheritance
- Cascade expectations
- Relationship naming conventions
- Engineering governance

This document intentionally excludes:

- SQL foreign key definitions
- Constraint implementation
- Index design
- ORM relationship code
- Migration scripts

These implementation details are defined in subsequent Engineering Specification documents.

---