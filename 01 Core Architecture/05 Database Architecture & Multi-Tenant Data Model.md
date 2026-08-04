# Document 05 – Database Architecture & Multi-Tenant Data Model

---

# Document Control

**Document ID:** FD-ARCH-005

**Document Name:** Database Architecture & Multi-Tenant Data Model

**Version:** 1.0

**Status:** 🔒 LOCKED

**Classification:** Internal

**Owner:** FluxDine Architecture Team

**Architecture Bible Version:** 1.0

**Created:** 2026-07-31

**Last Updated:** 2026-07-31

---

## Dependencies

This document depends on:

- Document 01 – Product Requirements Document
- Document 02 – Product Technical Inventory
- Document 03 – Gap Analysis & SaaS Transformation Strategy
- Document 04 – System Architecture Blueprint

---

## Referenced By

This document serves as the architectural foundation for:

- Document 06 – API & Service Architecture
- Complete Database Schema Specification
- Drizzle ORM Schema
- Database Migrations
- Backend Services
- Repository Layer
- Authorization Layer

---

# Document Status

**Current Status**

✅ Version 1.0

🔒 LOCKED

This document is the authoritative database architecture specification for FluxDine.

All future database implementation must conform to this document.

Schema implementation details are maintained separately in:

09 Engineering Artifacts/
Database Specifications/
01 Complete Database Schema Specification.md

---


# Preface

The **Database Architecture & Multi-Tenant Data Model** is the foundational engineering specification for the FluxDine platform. It defines the architectural principles, ownership model, tenant isolation strategy, identity architecture, governance rules, and database design standards that underpin every application, service, API, and feature within the FluxDine ecosystem.

This document is intentionally focused on **architecture rather than implementation**. Its purpose is to explain **why** the database is designed in a particular way, **how** the major business domains relate to one another, and **which principles** must guide all future database decisions. It establishes the long-term architectural foundation upon which the physical schema, application services, APIs, and infrastructure will be built.

FluxDine is designed as a **multi-tenant Restaurant Commerce Platform**, where a single platform securely serves thousands of independent restaurant businesses. Every architectural decision documented here has been made with long-term scalability, tenant isolation, maintainability, security, extensibility, and operational reliability as primary objectives. The database architecture must support growth from a single restaurant to thousands of tenants without requiring structural redesign.

This document also formalizes the adoption of key architectural principles, including a shared database with tenant isolation, a unified identity system, configuration-driven customization, role-based authorization, domain-oriented organization, and an evolutionary migration strategy. Together, these principles ensure that the platform remains consistent, secure, and adaptable as new modules and capabilities are introduced.

It is important to distinguish this document from the implementation specifications used during development. While this document defines the **architectural blueprint**, the detailed database schema—including tables, columns, data types, indexes, constraints, foreign keys, and Drizzle ORM mappings—is maintained separately in the **Complete Database Schema Specification** under the Engineering Artifacts section of the Architecture Bible. This separation allows the architecture to remain stable while implementation details evolve through controlled migrations and versioning.

All database implementations, schema changes, migrations, backend services, repositories, APIs, and authorization mechanisms must conform to the principles established in this document. Any significant architectural modification must follow the FluxDine governance process through an approved Request for Comments (RFC), an Architectural Decision Record (ADR), and an update to the Architecture Bible before implementation begins.

This document serves as the **authoritative database architecture specification** for FluxDine Version 1.0 and represents the approved architectural baseline for all future development. It is intended to provide engineers, architects, AI development assistants, and future contributors with a single, consistent source of truth for understanding how the FluxDine data platform is designed and why those design decisions were made.


---

# Table of Contents

Part A
Foundation Architecture

    Chapter 1
    Database Philosophy

    Chapter 2
    Multi-Tenant Strategy

    Chapter 3
    Database Topology

    Chapter 4
    Ownership Hierarchy

    Chapter 5
    Core Entity Hierarchy

    Chapter 6
    Unified Identity System

    Chapter 7
    Authorization Model

    Chapter 8
    Tenant Isolation Rules

    Chapter 9
    Database Engineering Standards

    Chapter 10
    Database ADRs

────────────────────────────

Part B
Platform, Restaurant & Commerce Data Model

    Chapter 11
    Platform Domain

    Chapter 12
    Tenant Domain

    Chapter 13
    Branch Domain

    Chapter 14
    Identity Domain

    Chapter 15
    Restaurant Domain

    Chapter 16
    Commerce Domain

    Chapter 17
    Marketing Domain

    Chapter 18
    Billing Domain

    Chapter 19
    Configuration Domain

    Chapter 20
    Payment Gateway Framework

    Chapter 21
    Domain & Branding

    Chapter 22
    Ownership Matrix

    Chapter 23
    Domain Relationships

    Chapter 24
    Database ADRs

────────────────────────────

Part C
Physical Data Model

    Chapter 25
    Physical Data Model

    Chapter 26
    Entity Relationship Architecture

    Chapter 27
    Relationship Matrix

    Chapter 28
    Primary Keys

    Chapter 29
    Referential Integrity

    Chapter 30
    Indexing Strategy

    Chapter 31
    Unique Constraints

    Chapter 32
    Soft Deletes

    Chapter 33
    Audit Architecture

    Chapter 34
    Feature Flag Architecture

    Chapter 35
    Subscription Model

    Chapter 36
    Query Standards

    Chapter 37
    Performance Strategy

    Chapter 38
    Database Standards

    Chapter 39
    Validation Rules

    Chapter 40
    Database ADRs

────────────────────────────

Part D
Migration, Governance & Lifecycle

    Chapter 41
    Migration Strategy

    Chapter 42
    SaaS Migration Plan

    Chapter 43
    Drizzle ORM Standards

    Chapter 44
    PostgreSQL & Turso Compatibility

    Chapter 45
    Data Lifecycle Management

    Chapter 46
    Backup & Recovery Strategy

    Chapter 47
    Data Retention & Archival

    Chapter 48
    Security & Compliance

    Chapter 49
    Database Versioning

    Chapter 50
    Governance & Engineering Rules

    Chapter 51
    Future Expansion Strategy

    Chapter 52
    Database ADRs

────────────────────────────
# Main Content

## Part A — Foundation Architecture

---

# Purpose

This document defines the foundational database architecture for the FluxDine SaaS platform. It establishes the principles, ownership model, tenant isolation strategy, identity system, and authorization model that every future database table, API, service, and migration must follow.

**This document is the single source of truth for all database-related engineering decisions.**

---

# Chapter 1 — Database Philosophy

## 1.1 Vision

The FluxDine database is not designed to store restaurant data.

It is designed to power an entire Restaurant Commerce Platform.

Every design decision must prioritize:

* Scalability
* Security
* Maintainability
* Performance
* Extensibility
* Tenant Isolation

---

## 1.2 Core Philosophy

The database must support:

* 1 Platform
* 10 Restaurants
* 100 Restaurants
* 1,000 Restaurants
* 10,000 Restaurants
* Millions of Customers
* Millions of Orders

without requiring structural redesign.

---

## 1.3 Design Principles

### Principle 1

One Platform

Many Restaurants

One Database Architecture.

---

### Principle 2

Every business object belongs to exactly one owner.

No orphan data.

No ambiguous ownership.

---

### Principle 3

Configuration over duplication.

Restaurants differ through configuration—not schema changes.

---

### Principle 4

Tenant Isolation is mandatory.

There must never be a query capable of leaking another tenant's data.

---

### Principle 5

Every schema decision must support future modules such as:

* POS
* Inventory
* CRM
* Loyalty
* AI Assistant
* Accounting
* Mobile Apps

without redesigning existing tables.

---

# Chapter 2 — Multi-Tenant Strategy

## 2.1 Selected Strategy

After evaluating common SaaS patterns, FluxDine adopts:

### Shared Database + Shared Schema + Tenant Isolation

```text
One Database

↓

One Schema

↓

Every Record belongs to a Tenant
```

---

## 2.2 Alternatives Considered

### Option A

One Database Per Restaurant

❌ Rejected

Reasons:

* Difficult backups
* Operational overhead
* Expensive
* Hard to maintain
* Complex analytics

---

### Option B

One Schema Per Restaurant

❌ Rejected

Reasons:

* Migration complexity
* Thousands of schemas
* Difficult deployment

---

### Option C

Shared Database

Shared Schema

Tenant ID

✅ Approved

Reasons:

* Scalable
* Easier migrations
* Simpler analytics
* Lower infrastructure cost
* Industry-proven approach (used by many SaaS platforms)

---

# Chapter 3 — Database Topology

The platform consists of one logical database divided into domains.

```text
FluxDine Database

│

├── Platform Domain

├── Tenant Domain

├── Identity Domain

├── Restaurant Domain

├── Commerce Domain

├── Marketing Domain

├── Billing Domain

├── Configuration Domain

├── Analytics Domain

└── Audit Domain
```

Each domain groups related entities while remaining within the same physical database.

---

# Chapter 4 — Ownership Hierarchy

This is the most important concept in FluxDine.

Everything ultimately belongs to a Tenant.

```text
Platform

│

└── Tenant (Restaurant)

      │

      ├── Branch

      │      │

      │      ├── Users

      │      ├── Riders

      │      ├── Orders

      │      ├── Reservations

      │      └── Reports

      │

      ├── Menu

      ├── Customers

      ├── Offers

      ├── Payments

      ├── Domains

      ├── Themes

      └── Subscription
```

---

## Ownership Rules

Every table must answer:

**Who owns this record?**

If the answer is unclear, the schema is incomplete.

---

Examples

Order

↓

Restaurant

↓

Tenant

---

Reservation

↓

Restaurant

↓

Tenant

---

Menu Item

↓

Restaurant

↓

Tenant

---

Branch

↓

Tenant

---

Customer

↓

Tenant

---

Payment Configuration

↓

Tenant

---

Theme

↓

Tenant

---

Subscription

↓

Tenant

---

# Chapter 5 — Core Entity Hierarchy

FluxDine revolves around a small set of core business entities.

```text
Platform

↓

Tenant

↓

Branch

↓

User

↓

Customer

↓

Menu

↓

Product

↓

Order

↓

Reservation
```

---

## Platform Entity

Represents FluxDine itself.

Responsible for:

* Platform Settings
* Plans
* Billing Rules
* Feature Catalog
* Global Configuration

---

## Tenant Entity

Represents one restaurant business.

This becomes the root entity for every restaurant.

Owns:

* Branches
* Users
* Menus
* Orders
* Customers
* Reservations
* Domains
* Payment Configuration
* Branding
* Subscription

---

## Branch Entity

Represents a physical restaurant location.

Belongs to:

One Tenant

Owns:

* Orders
* Staff
* Riders
* Tables
* Reports

---

## User Entity

Represents every authenticated human.

No distinction between:

Admin

Customer

Rider

Manager

HQ Staff

All are Users.

Authorization determines capabilities.

---

# Chapter 6 — Unified Identity System

## Architectural Decision

AD-001

Approved.

FluxDine uses one Users table.

Current System

```text
Users

Admins

Riders
```

Future

```text
Users

↓

Role

↓

Permissions
```

---

## User Types

The Users table supports:

Customer

Head Admin

Branch Admin

Rider

HQ Owner

HQ Admin

Support Staff

Sales Staff

Finance Staff

Future roles can be added without creating new tables.

---

## User Ownership

Every user belongs to exactly one Tenant, except HQ users.

```text
HQ Users

↓

Platform

Restaurant Users

↓

Tenant
```

---

## Benefits

Single Authentication

Single Authorization

Simpler APIs

Simpler Auditing

Cleaner Relationships

Future-proof

---

# Chapter 7 — Authorization Model

Authentication answers:

Who are you?

Authorization answers:

What can you do?

---

## Authentication Layer

Responsible for:

Identity

Password

Session

JWT / Cookies

MFA (Future)

---

## Authorization Layer

Responsible for:

Permissions

Roles

Tenant Access

Branch Access

Feature Access

Subscription Access

---

## Authorization Hierarchy

```text
Platform

↓

HQ Owner

↓

HQ Admin

↓

Support

↓

Sales

↓

Finance

↓

Tenant

↓

Head Admin

↓

Branch Admin

↓

Rider

↓

Customer
```

---

## Branch Scoping

Branch Admins

Can access

Only assigned branches.

---

Riders

Can access

Only assigned deliveries.

---

Customers

Can access

Only their own orders.

---

HQ Staff

Can access

Platform-level data according to role.

---

## Principle of Least Privilege

Every role receives only the minimum permissions required to perform its responsibilities.

No role receives unnecessary access.

---

# Chapter 8 — Tenant Isolation Rules

Tenant isolation is the highest-priority security requirement in FluxDine.

---

## Rule 1

Every tenant-owned table must contain a tenant identifier.

---

## Rule 2

Every tenant query must include tenant filtering.

---

## Rule 3

Platform tables must never mix with tenant-owned business data.

---

## Rule 4

Cross-tenant joins are prohibited unless explicitly executed by authorized HQ services.

---

## Rule 5

Tenant isolation must be enforced at:

* Service layer
* API layer
* Authorization layer
* Database query layer

---

# Chapter 9 — Database Engineering Standards

These standards apply to every future table.

---

## Naming Convention

Tables

Plural

Examples:

users

orders

branches

menus

products

---

Primary Keys

id

---

Foreign Keys

tenant_id

branch_id

user_id

order_id

---

Timestamp Columns

created_at

updated_at

deleted_at (Soft Delete)

---

Boolean Columns

Prefix with:

is_

Examples

is_active

is_verified

is_deleted

---

Status Columns

Use ENUM or constrained values rather than multiple boolean flags where appropriate.

Examples:

order_status

reservation_status

subscription_status

---

## Soft Deletes

Business records should generally use soft deletion where recovery or auditing is valuable.

---

## Auditability

Critical business tables must include:

* created_by
* updated_by
* timestamps

where appropriate.

---

# Chapter 10 — Architectural Decisions (Database)

## AD-007

Shared Database

Shared Schema

Tenant Isolation

Status:

Approved

---

## AD-008

Unified Identity System

Status:

Approved

---

## AD-009

Tenant is the Root Business Entity

Status:

Approved

---

## AD-010

Every Business Object Has One Owner

Status:

Approved

---

## AD-011

Authorization is Role-Based and Tenant-Aware

Status:

Approved

---

## AD-012

Configuration over Schema Customization

Status:

Approved

---

# Part A Summary

This part establishes the **foundation of the FluxDine data model**. It defines the database philosophy, selects the multi-tenant strategy, establishes the ownership hierarchy, formalizes the unified identity system, and sets mandatory engineering standards that every future schema and migration must follow. These principles are now the architectural baseline for all subsequent database design work.

---

---


## Part B — Platform, Restaurant & Commerce Data Model

---

# Purpose

Part B defines the **business domains**, **major entities**, and **ownership boundaries** of the FluxDine database. Rather than specifying every table and column, it establishes the logical data model that will later be translated into Drizzle schemas, migrations, ERDs, and APIs.

Every future table must belong to one of the domains defined in this document.

---

# Chapter 11 — Platform Domain

The Platform Domain represents **FluxDine itself**, not any individual restaurant.

Platform data is global and is owned exclusively by FluxDine.

```text
Platform
│
├── Plans
├── Global Settings
├── Feature Catalog
├── Platform Users
├── System Announcements
├── Audit Configuration
├── Billing Rules
├── Email Templates
├── Notification Templates
└── Platform Configuration
```

---

## Ownership

Owner:

```text
FluxDine Platform
```

Never:

```text
Restaurant
```

---

## Responsibilities

The Platform Domain manages:

* Subscription plans
* Global feature catalogue
* Platform-wide configuration
* HQ administration
* Global notifications
* Billing rules
* Email templates
* Default themes
* System maintenance
* Global analytics

---

## Platform Entities

Future entities include:

| Entity                | Purpose                     |
| --------------------- | --------------------------- |
| Platform              | Root platform configuration |
| Plan                  | SaaS subscription plans     |
| Feature               | Master feature catalogue    |
| HQ User               | Internal FluxDine staff     |
| Platform Settings     | Global configuration        |
| Announcement          | Platform announcements      |
| Email Template        | Global email templates      |
| Notification Template | Notification templates      |
| Audit Policy          | Global auditing rules       |

---

# Chapter 12 — Tenant Domain

The Tenant Domain is the heart of FluxDine.

Everything that belongs to a restaurant ultimately belongs to a Tenant.

```text
Tenant
│
├── Restaurant Profile
├── Subscription
├── Branches
├── Users
├── Branding
├── Domains
├── Payment Configuration
├── Themes
├── Features
└── Analytics
```

---

## Definition

A Tenant represents:

> One restaurant business operating on the FluxDine platform.

Examples:

* Pizza Hut Lahore
* Burger Lab
* KFC Riyadh
* Local Café

Each is a completely isolated tenant.

---

## Tenant Responsibilities

A Tenant owns:

* Brand
* Business Identity
* Menu
* Orders
* Customers
* Staff
* Riders
* Domains
* Payment gateways
* Theme
* Subscription
* Reports
* Reservations

---

## Tenant Lifecycle

```text
Registration

↓

Trial

↓

Subscription

↓

Active

↓

Suspended (Optional)

↓

Cancelled

↓

Archived
```

Every tenant follows this lifecycle.

---

## Tenant Configuration

Each tenant has configurable settings such as:

* Timezone
* Currency
* Language
* Tax Rules
* Service Areas
* Ordering Preferences
* Reservation Rules
* Branding
* Email Settings
* SMS Settings
* Notification Preferences

No custom code should be required.

---

# Chapter 13 — Branch Domain

Branches represent the physical operating locations of a restaurant.

```text
Tenant

↓

Branch

↓

Operations
```

---

## Branch Responsibilities

Each branch manages:

* Kitchen
* Orders
* Riders
* Reservations
* Tables
* Staff
* Reports
* Inventory (Future)

---

## Ownership

```text
Tenant

↓

Branch
```

A branch can never exist independently.

---

## Branch Relationships

Branch connects to:

* Orders
* Reservations
* Staff
* Riders
* Reports
* Customers (through orders)
* Menu Availability

---

## Future Expansion

Future modules:

* Kitchen Display System
* Inventory
* Stock
* Suppliers
* Workforce Scheduling
* Branch Performance Metrics

No redesign should be necessary.

---

# Chapter 14 — Identity Domain

The Identity Domain centralizes every authenticated person.

```text
Users

↓

Authentication

↓

Authorization
```

---

## Unified Users

One Users table supports:

* Customer
* Head Admin
* Branch Admin
* Rider
* HQ Owner
* HQ Admin
* Support
* Finance
* Sales

Future roles:

* Franchise Manager
* Kitchen Manager
* Accountant
* Marketing Manager

without schema changes.

---

## Identity Relationships

Every authenticated user has:

* Profile
* Credentials
* Role
* Permissions
* Session
* Audit History

---

## Authentication Components

Future entities:

* User
* Session
* Refresh Token
* MFA Device
* Password Reset
* Login History
* Email Verification
* API Key (Future)

---

# Chapter 15 — Restaurant Domain

The Restaurant Domain contains operational business data.

```text
Restaurant

↓

Menu

↓

Products

↓

Categories

↓

Offers

↓

Reservations

↓

News

↓

Customers
```

---

## Core Modules

### Menu Management

Owns:

* Categories
* Products
* Availability
* Pricing
* Images

---

### Offers

Owns:

* Promotions
* Coupons
* Discounts
* Campaigns

---

### Reservations

Owns:

* Booking
* Table
* Guest Count
* Schedule

---

### News

Owns:

* Restaurant updates
* Promotions
* Events

---

### Customer Profile

Owns:

* Addresses
* Preferences
* Saved Orders
* Favorites (Future)

---

# Chapter 16 — Commerce Domain

Commerce drives revenue.

Everything related to ordering belongs here.

```text
Customer

↓

Cart

↓

Checkout

↓

Payment

↓

Order

↓

Delivery
```

---

## Commerce Components

* Shopping Cart
* Cart Items
* Checkout
* Orders
* Order Items
* Payments
* Refunds (Future)
* Tips (Future)
* Taxes
* Delivery Charges

---

## Order Lifecycle

```text
Draft

↓

Pending Payment

↓

Confirmed

↓

Preparing

↓

Ready

↓

Out for Delivery

↓

Delivered

↓

Completed
```

Alternative paths:

```text
Cancelled

Failed

Refunded
```

---

## Ownership

Every order belongs to:

```text
Tenant

↓

Branch

↓

Customer
```

---

## Commerce Relationships

Order connects to:

* Customer
* Branch
* Rider
* Payment
* Products
* Discounts
* Taxes

---

# Chapter 17 — Marketing Domain

Marketing helps restaurants grow.

Future entities:

* Newsletter
* Subscriber
* Campaign
* Coupon
* Promotion
* Referral
* Loyalty (Future)
* Gift Cards (Future)

---

## Marketing Goals

* Customer retention
* Repeat orders
* Email marketing
* Promotions
* Analytics

---

# Chapter 18 — Billing Domain

The Billing Domain manages the commercial relationship between FluxDine and its tenants.

It is separate from restaurant customer payments.

---

## Responsibilities

* Subscription
* Invoice
* Payment
* Plan
* Renewal
* Upgrade
* Downgrade
* Trial
* Credits (Future)

---

## Billing Lifecycle

```text
Trial

↓

Active

↓

Renewal

↓

Upgrade

↓

Downgrade

↓

Grace Period

↓

Suspended

↓

Cancelled
```

---

## Ownership

```text
Platform

↓

Tenant Subscription
```

Restaurant customers never interact with this domain.

---

# Chapter 19 — Configuration Domain

Everything configurable belongs here.

Not hardcoded.

---

## Configuration Categories

### Branding

* Logo
* Colors
* Fonts
* Icons

---

### Theme

* Layout
* Components
* Homepage
* Typography

---

### Payments

* Stripe
* PayPal
* Future providers

---

### Localization

* Language
* Currency
* Timezone
* Date format

---

### Restaurant Rules

* Delivery radius
* Reservation limits
* Working hours
* Holidays

---

### Notifications

* Email
* SMS
* Push (Future)
* WhatsApp (Future)

---

# Chapter 20 — Payment Gateway Framework

## Architectural Decision

Restaurants own their payment accounts.

FluxDine never stores or controls restaurant funds.

---

## Payment Abstraction

Restaurant Platform

↓

Payment Service

↓

Gateway Adapter

↓

Payment Provider

```text
Restaurant

↓

Payment Service

├── Stripe Adapter
├── PayPal Adapter
├── Future Adapter
└── Sandbox Adapter
```

---

## Version 1

Supported providers:

* Stripe
* PayPal

---

## Future Providers

The architecture supports adding:

* Square
* Razorpay
* Paymob
* Paystack
* Mollie
* Adyen
* Checkout.com
* Authorize.Net
* Amazon Pay
* Apple Pay
* Google Pay

without changing restaurant code.

---

## Gateway Ownership

Each tenant stores its own:

* Provider
* API credentials (encrypted)
* Webhook configuration
* Currency
* Supported payment methods
* Sandbox/Production mode

---

## Payment Rules

Restaurant code must never communicate directly with:

* Stripe SDK
* PayPal SDK

Instead:

```text
Restaurant

↓

Payment Service

↓

Adapter

↓

Gateway
```

---

## Security Standards

Payment credentials:

* Encrypted at rest
* Never logged
* Never exposed to clients
* Rotatable without downtime

---

# Chapter 21 — Domain & Branding Model

Every tenant owns its brand identity.

---

## Domain Ownership

Each tenant may have:

* FluxDine subdomain
* Custom domain
* Future multiple domains

Examples:

```text
restaurant.fluxdine.app

pizza.example.com

orders.restaurant.com
```

---

## Branding Ownership

Each tenant controls:

* Logo
* Colors
* Typography
* Favicon
* Social links
* SEO metadata

without affecting other tenants.

---

# Chapter 22 — Data Ownership Matrix

| Domain        | Owner             | Tenant Scoped |
| ------------- | ----------------- | ------------- |
| Platform      | FluxDine          | ❌             |
| Tenant        | Restaurant        | ✅             |
| Branch        | Restaurant        | ✅             |
| Identity      | Platform + Tenant | ✅             |
| Restaurant    | Restaurant        | ✅             |
| Commerce      | Restaurant        | ✅             |
| Marketing     | Restaurant        | ✅             |
| Configuration | Restaurant        | ✅             |
| Billing       | Platform          | Partial       |
| Analytics     | Restaurant        | ✅             |
| Audit         | Platform + Tenant | ✅             |

---

# Chapter 23 — Domain Relationships

```text
Platform
│
├── Plans
├── Features
├── HQ Users
│
└── Tenant
      │
      ├── Branches
      ├── Users
      ├── Customers
      ├── Menu
      ├── Orders
      ├── Reservations
      ├── Offers
      ├── Payments
      ├── Domains
      ├── Themes
      ├── Analytics
      └── Subscription
```

---

# Chapter 24 — Architectural Decisions

## AD-013

Platform data is globally owned by FluxDine.

Status:

Approved

---

## AD-014

Every restaurant operates as one Tenant.

Status:

Approved

---

## AD-015

Every Branch belongs to one Tenant.

Status:

Approved

---

## AD-016

Commerce entities are tenant-scoped.

Status:

Approved

---

## AD-017

Payment processing must use the Payment Service abstraction.

Status:

Approved

---

## AD-018

Configuration replaces custom development wherever possible.

Status:

Approved

---

# Part B Summary

Part B establishes the **logical business domains** of the FluxDine platform and defines the ownership boundaries that separate platform-level data from tenant-specific data. It introduces the Platform, Tenant, Branch, Identity, Restaurant, Commerce, Marketing, Billing, and Configuration domains, along with the Payment Gateway Framework and branding model. Together with Part A, these chapters provide the conceptual blueprint from which the physical database schema, Drizzle models, and migration strategy will be designed.

---

---

## Part C — Physical Data Model, Relationships & Performance Architecture

---

# Purpose

Parts A and B established the **database philosophy** and the **logical business domains**.

Part C defines the **physical architecture rules** that every database table, relationship, index, constraint, and query must follow. It serves as the engineering blueprint for implementing the database in Drizzle ORM and PostgreSQL-compatible systems.

---

# Chapter 25 — Physical Data Model

## Philosophy

The physical database model translates the business architecture into an implementation-ready schema.

The design goals are:

* High performance
* Simplicity
* Tenant safety
* Easy migrations
* Extensibility
* Referential integrity

---

## Entity Classification

Every table must belong to exactly one of the following categories:

```text
Platform Tables

↓

Tenant Tables

↓

Operational Tables

↓

Transaction Tables

↓

Configuration Tables

↓

Audit Tables

↓

Analytics Tables
```

---

## Table Families

### Platform Family

Examples

* plans
* features
* platform_settings
* platform_users

---

### Tenant Family

Examples

* tenants
* subscriptions
* domains
* themes

---

### Restaurant Family

Examples

* branches
* categories
* menu_items
* offers
* reservations

---

### Commerce Family

Examples

* carts
* cart_items
* orders
* order_items
* payments

---

### Identity Family

Examples

* users
* sessions
* refresh_tokens
* login_history

---

### Audit Family

Examples

* audit_logs
* activity_logs
* system_events

---

# Chapter 26 — Entity Relationship Architecture

The following hierarchy represents the primary ownership model.

```text
Platform
│
├── Plans
├── Features
├── HQ Users
│
└── Tenant
      │
      ├── Subscription
      ├── Branches
      │      │
      │      ├── Orders
      │      ├── Reservations
      │      ├── Riders
      │      └── Staff
      │
      ├── Customers
      ├── Menu
      │      ├── Categories
      │      └── Products
      │
      ├── Offers
      ├── Domains
      ├── Themes
      ├── Analytics
      └── Payment Configuration
```

---

## Relationship Principles

Every relationship must satisfy:

* One owner
* Explicit foreign key
* Cascading rules defined
* Tenant safety
* No circular dependencies

---

# Chapter 27 — Relationship Matrix

| Parent   | Child         | Cardinality           |
| -------- | ------------- | --------------------- |
| Platform | Tenant        | One → Many            |
| Tenant   | Branch        | One → Many            |
| Tenant   | User          | One → Many            |
| Tenant   | Customer      | One → Many            |
| Tenant   | Category      | One → Many            |
| Category | Menu Item     | One → Many            |
| Tenant   | Offer         | One → Many            |
| Tenant   | Reservation   | One → Many            |
| Tenant   | Domain        | One → Many            |
| Tenant   | Theme         | One → One             |
| Branch   | Order         | One → Many            |
| Customer | Order         | One → Many            |
| Order    | Order Item    | One → Many            |
| Order    | Payment       | One → One (initially) |
| User     | Session       | One → Many            |
| User     | Login History | One → Many            |

---

## Relationship Rules

No child entity may exist without its parent unless explicitly designed for global platform use.

---

# Chapter 28 — Primary Key Strategy

## Standard

Every table will use a single immutable primary key.

```text
id
```

---

## Characteristics

Primary keys must be:

* Globally unique
* Immutable
* Never reused
* Independent of business logic

---

## Foreign Keys

Every relationship uses explicit foreign keys.

Examples

```text
tenant_id

branch_id

user_id

customer_id

order_id

reservation_id

payment_id
```

---

## Composite Keys

Avoid composite primary keys.

Composite unique constraints may be used where appropriate.

Example:

```text
tenant_id + domain
```

---

# Chapter 29 — Referential Integrity

Every foreign key relationship must explicitly define its behavior.

---

## Delete Policies

### Restrict

Used when deleting a parent would corrupt business history.

Examples

* Orders
* Payments
* Audit Logs

---

### Cascade

Used only when child records have no independent value.

Examples

* Cart Items
* Sessions
* Temporary Tokens

---

### Set Null

Used when historical records must survive.

Example

Deleted rider

↓

Historical delivery remains.

---

## Rule

Cascade deletes should be used sparingly.

Business history should generally be preserved.

---

# Chapter 30 — Indexing Strategy

Performance is designed into the schema from day one.

---

## Primary Indexes

Every table

```text
PRIMARY KEY(id)
```

---

## Tenant Index

Every tenant-owned table

```text
INDEX(tenant_id)
```

Mandatory.

---

## Branch Index

Operational tables

```text
INDEX(branch_id)
```

---

## Customer Index

Commerce

```text
INDEX(customer_id)
```

---

## Order Indexes

```text
tenant_id

branch_id

customer_id

status

created_at
```

---

## Reservation Indexes

```text
tenant_id

branch_id

reservation_date

status
```

---

## User Indexes

```text
email

tenant_id

role

branch_id
```

---

## Search Indexes

Future:

* Full-text search
* Product search
* Customer search

---

# Chapter 31 — Unique Constraints

Uniqueness protects business integrity.

---

Examples

Tenant slug

Must be unique.

---

Restaurant domain

Unique.

---

Email

Unique within the intended scope (platform-wide or tenant-scoped, depending on authentication policy defined later).

---

Branch code

Unique inside one tenant.

---

SKU

Unique within one tenant.

---

Coupon code

Unique within one tenant.

---

# Chapter 32 — Soft Delete Strategy

Business data should rarely be permanently deleted.

---

Tables supporting soft deletion include:

* Users
* Customers
* Products
* Categories
* Offers
* Reservations
* Branches

---

Excluded

Orders

Payments

Audit Logs

These are immutable historical records.

---

Deleted records should include:

```text
deleted_at

deleted_by

delete_reason (optional)
```

---

# Chapter 33 — Audit Architecture

Every critical business action must be traceable.

---

Auditable Actions

* Login
* Logout
* Password Reset
* Order Creation
* Payment
* Refund
* Reservation Update
* Menu Changes
* Staff Changes
* Configuration Changes
* Subscription Changes

---

Audit Record

Contains

* Actor
* Timestamp
* Action
* Entity
* Entity ID
* Previous Value (where appropriate)
* New Value (where appropriate)
* IP Address (where appropriate)
* User Agent (where appropriate)

---

Audit logs are immutable.

---

# Chapter 34 — Feature Flag Architecture

Features are configuration-driven.

Not code-driven.

---

Hierarchy

```text
Platform

↓

Plan

↓

Tenant

↓

User (Future)
```

---

Evaluation Order

```text
Platform

↓

Subscription Plan

↓

Tenant Override

↓

Temporary Override
```

---

Examples

Restaurant can:

Enable

Reservations

Disable

Newsletter

Enable

Rider Module

without deployment.

---

# Chapter 35 — Subscription Data Model

Subscription data is separate from restaurant operations.

---

Relationships

```text
Platform

↓

Plan

↓

Tenant

↓

Subscription

↓

Billing History
```

---

Subscription contains

* Plan
* Status
* Trial
* Renewal
* Billing Cycle
* Next Invoice
* Cancellation Date
* Grace Period

---

Restaurant operations never depend on hardcoded plans.

Everything is configuration-based.

---

# Chapter 36 — Query Standards

Every query must be:

* Secure
* Predictable
* Tenant-aware
* Indexed
* Efficient

---

Required Rule

Every tenant query must filter by:

```sql
tenant_id
```

Never optional.

---

Branch Queries

Must include

```sql
branch_id
```

where appropriate.

---

Customer Queries

Must include authenticated customer ownership.

---

No query may expose another tenant's data.

---

# Chapter 37 — Performance Strategy

The database is optimized for:

* Read-heavy workloads
* High order volume
* Concurrent restaurant operations

---

Optimization Strategy

* Proper indexing
* Pagination
* Lazy loading
* Query optimization
* Background processing
* Materialized summaries (future)
* Read replicas (future)

---

Avoid

* N+1 queries
* Large table scans
* Cross-tenant joins
* Excessive eager loading

---

# Chapter 38 — Database Standards

## Required Columns

Every tenant-owned table should include, where applicable:

```text
id

tenant_id

created_at

updated_at
```

Operational tables may additionally include:

```text
created_by

updated_by
```

Soft-deletable tables may include:

```text
deleted_at

deleted_by
```

---

## Naming Standards

Tables

Plural

Columns

snake_case

Foreign Keys

```text
tenant_id

branch_id

user_id

order_id
```

Indexes

```text
idx_table_column
```

Unique Constraints

```text
uq_table_column
```

Foreign Keys

```text
fk_table_parent
```

---

# Chapter 39 — Engineering Validation Rules

Before any migration is approved, it must satisfy:

✅ Tenant ownership defined

✅ Foreign keys defined

✅ Delete behavior defined

✅ Indexes defined

✅ Unique constraints defined

✅ Audit requirements evaluated

✅ Soft delete policy evaluated

✅ Naming standards followed

✅ Performance reviewed

✅ Security reviewed

---

# Chapter 40 — Architectural Decisions

## AD-019

Every table belongs to one domain.

Status:

Approved

---

## AD-020

Every tenant-owned table requires a `tenant_id`.

Status:

Approved

---

## AD-021

Business history is preserved by default.

Status:

Approved

---

## AD-022

Indexes are part of the architecture, not an optimization added later.

Status:

Approved

---

## AD-023

Feature availability is configuration-driven.

Status:

Approved

---

## AD-024

Every schema change must pass the Engineering Validation Checklist before implementation.

Status:

Approved

---

# Part C Summary

Part C transforms the conceptual model into an implementation-oriented physical design. It defines how entities relate, how integrity is enforced, how indexing and constraints support performance, how feature flags and subscriptions are represented, and the engineering standards that every future migration must satisfy. Together with Parts A and B, this provides the architectural foundation needed to design the detailed schema and migration strategy in the final part of Document 05.

---

---

## Part D — Migration Strategy, Lifecycle & Governance

---

# Purpose

Part D defines how the FluxDine database evolves over time. It establishes migration principles, compatibility rules, data lifecycle management, backup strategy, governance, and engineering processes that ensure the database remains reliable as the platform scales.

---

# Chapter 41 — Migration Strategy

## Philosophy

FluxDine follows an **evolutionary migration strategy**.

We evolve the schema without unnecessary rewrites.

---

## Migration Principles

1. Prefer additive changes over destructive changes.
2. Never break production tenants unnecessarily.
3. Preserve historical data.
4. Support rollback where practical.
5. Every migration must be idempotent.
6. Every migration must be reviewed before execution.

---

## Migration Workflow

```text
Architecture Bible
        ↓
Schema Specification
        ↓
Drizzle Schema
        ↓
Migration Generation
        ↓
Code Review
        ↓
Staging Validation
        ↓
Production Deployment
```

No migration should bypass this workflow.

---

# Chapter 42 — Current Schema to SaaS Migration

FluxDine already has a functioning single-restaurant application.

The migration strategy is:

> **Evolve, Don't Rewrite**

---

## Preserve Existing Assets

Retain and adapt:

* Orders
* Order Items
* Categories
* Menu Items
* Reservations
* Branches
* Offers
* News
* Newsletter Subscribers
* Customer Accounts

---

## Refactor

Transform into SaaS-aware entities:

* Admins → Users
* Riders → Users
* Settings → Tenant Configuration
* Payment Settings → Payment Configuration
* Email Settings → Notification Configuration

---

## Introduce

New SaaS entities:

* Tenant
* Subscription
* Plan
* Feature
* Domain
* Theme
* Feature Flags
* Audit Logs
* Platform Settings

---

# Chapter 43 — Drizzle ORM Standards

Drizzle ORM is the canonical ORM for FluxDine.

---

## Standards

* One schema definition per table.
* Relationships defined explicitly.
* Reusable enum definitions.
* Shared column helpers where appropriate.
* Avoid duplicated schema definitions.

---

## Migration Rules

* Generate migrations through Drizzle.
* Never manually edit production migrations unless formally reviewed.
* Version control every migration.

---

# Chapter 44 — PostgreSQL & Turso Compatibility

FluxDine is designed to remain database-portable.

---

## Current State

Development:

* Turso (SQLite compatible)

---

## Target

Production architecture should remain compatible with PostgreSQL.

---

## Design Rules

* Avoid vendor-specific SQL when practical.
* Prefer portable data types.
* Keep ORM abstractions clean.
* Minimize database-specific business logic.

---

# Chapter 45 — Data Lifecycle Management

Every type of data has a lifecycle.

---

## Operational Data

Examples:

* Carts
* Sessions
* Tokens

Retention:

Short-lived.

---

## Business Data

Examples:

* Orders
* Reservations
* Customers

Retention:

Long-term.

---

## Financial Data

Examples:

* Payments
* Invoices
* Billing History

Retention:

Long-term according to legal and business requirements.

---

## Audit Data

Retention:

Long-term.

Never modified.

---

# Chapter 46 — Backup & Recovery Strategy

Backups are mandatory.

---

## Principles

* Automated backups.
* Verified restore procedures.
* Secure storage.
* Encrypted backups.
* Documented recovery process.

---

## Recovery Objectives

Architecture should support defining:

* Recovery Time Objective (RTO)
* Recovery Point Objective (RPO)

Specific values will be determined during infrastructure planning.

---

# Chapter 47 — Data Retention & Archival

Not all data should remain in primary operational tables indefinitely.

---

## Active Data

Frequently accessed.

Examples:

* Active Orders
* Current Customers
* Current Menu

---

## Historical Data

Examples:

* Completed Orders
* Closed Reservations
* Audit Logs

May be archived while remaining accessible for reporting.

---

## Purge Policy

Temporary data such as:

* Expired sessions
* Password reset tokens
* Verification tokens

may be purged automatically after defined retention periods.

---

# Chapter 48 — Security & Compliance

Database security is a shared responsibility across the application and infrastructure.

---

## Requirements

* Encryption in transit.
* Encryption at rest where supported.
* Principle of least privilege.
* Secret management.
* Credential rotation.
* Audit logging.
* Secure backups.

---

## Sensitive Data

Sensitive information (for example, payment credentials and API secrets) must be encrypted and never exposed to client applications or logs.

---

# Chapter 49 — Database Versioning

The database is versioned alongside the application.

---

## Rules

* Every schema change receives a migration.
* Every migration has a version.
* No undocumented schema changes.
* Architecture Bible changes precede implementation.

---

## Version Flow

```text
Architecture
      ↓
Schema Specification
      ↓
Migration
      ↓
Application Release
```

---

# Chapter 50 — Governance & Engineering Rules

The database architecture is governed by the Architecture Bible.

---

## Change Management

Every significant database change should answer:

* Why is the change needed?
* Does it affect tenant isolation?
* Does it affect performance?
* Does it require an ADR?
* Does it require a schema specification update?
* Does it require a migration?

---

## Engineering Checklist

Before implementation:

* Architecture approved.
* ADR created (if applicable).
* Schema specification updated.
* Migration reviewed.
* Tests written.
* Performance considered.
* Security reviewed.

---

# Chapter 51 — Future Expansion Strategy

The architecture is intentionally designed to support future capabilities without redesign.

Potential future modules include:

* Inventory Management
* Kitchen Display System (KDS)
* POS Integration
* CRM
* Loyalty Program
* Gift Cards
* Marketplace Integrations
* AI Recommendations
* Multi-brand Restaurant Groups
* Mobile Applications
* Public APIs
* Plugin Ecosystem

Each new module must integrate through the established domain model and tenant ownership rules.

---

# Chapter 52 — Database Architectural Decisions

## AD-025

Adopt an evolutionary migration strategy.

Status: Approved

---

## AD-026

Preserve existing business data during SaaS transformation.

Status: Approved

---

## AD-027

Use Drizzle ORM as the canonical schema definition layer.

Status: Approved

---

## AD-028

Maintain PostgreSQL portability while supporting Turso during development.

Status: Approved

---

## AD-029

Architecture documentation precedes schema implementation.

Status: Approved

---

## AD-030

Schema specifications are maintained separately from architectural documentation.

Status: Approved

---

# Part D Summary

Part D defines how the FluxDine database will evolve from the current single-restaurant application into a scalable multi-tenant SaaS platform. It establishes migration principles, ORM standards, database portability, lifecycle management, backup and recovery, governance, and long-term expansion strategy. Together with Parts A, B, and C, it completes the architectural specification for the FluxDine database while intentionally separating long-lived architectural decisions from the detailed schema specification.

---

────────────────────────────

# Appendices

Appendix A
Entity Relationship Diagrams (ERD)

Appendix B
Database Standards Reference

Appendix C
Architectural Decision References

Appendix D
Future Database Modules

────────────────────────────

# Glossary

Platform

Tenant

Branch

Commerce

User

Customer

Subscription

Feature Flag

Audit Log

Domain

Theme

Configuration

Billing

Repository

Migration

Schema

ADR

────────────────────────────

# References

Document 01 – Product Requirements Document

Document 02 – Product Technical Inventory

Document 03 – Gap Analysis & SaaS Transformation Strategy

Document 04 – System Architecture Blueprint

Document 06 – API & Service Architecture

Complete Database Schema Specification

ADR-001 → ADR-030

────────────────────────────

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | YYYY-MM-DD | FluxDine Architecture Team | Initial approved and locked release |