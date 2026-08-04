Document Control
Field	Value
Document ID	FD-ENG-DB-001
Document Name	Complete Database Schema Specification
Version	1.0
Status	🔒 Draft
Owner	FluxDine Engineering
Classification	Internal
Depends On	04 System Architecture Blueprint
05 Database Architecture & Multi-Tenant Data Model
00 Database Naming Standards
Referenced By	All Database Engineering Specifications, Backend Specifications, API Specifications, Drizzle ORM Mapping
Dependencies

This document depends upon:

04 System Architecture Blueprint
05 Database Architecture & Multi-Tenant Data Model
00 Database Naming Standards
Referenced By

This document is referenced by:

02 Table Specifications
03 Relationships
04 Constraints
05 Index Specification
06 Enum Specification
07 Database Migration Strategy
08 Drizzle ORM Mapping
Backend Engineering Specifications
REST API Specification
Shared Platform Services
Product Module Specifications
Document Status
Item	Status
Schema Inventory	Draft
Entity Inventory	Draft
Domain Architecture	Draft
Tenant Model	Draft
Ownership Model	Draft
Lifecycle Model	Draft
Purpose

The Complete Database Schema Specification is the authoritative engineering specification describing the logical structure of every database schema and business entity within the FluxDine platform.

Where the Database Architecture document establishes architectural principles and multi-tenant strategy, this specification translates those principles into a complete, implementation-ready logical database model.

This document defines every business domain, every schema, every entity, their ownership, lifecycle, relationships, and responsibilities while remaining independent of physical implementation details such as column types, indexes, or migrations.

Scope

This specification defines:

Database schema organization
Business domains
Entity inventory
Entity ownership
Cross-domain relationships
Multi-tenant ownership
Unified Identity integration
Shared platform entities
Audit ownership
Entity lifecycle
Architectural rules
Engineering governance

This specification intentionally excludes:

Column definitions
SQL data types
Constraints
Indexes
Enumerations
Migration scripts
Drizzle ORM implementation details

Those topics are defined within subsequent Database Engineering Specification documents.

Database Design Philosophy

The FluxDine database is designed around the following principles:

Domain-Driven Design (DDD)
Multi-Tenant First
Unified Identity
Shared Platform Services
Tenant Isolation
Normalized Data Model
Explicit Relationships
Auditability by Design
Scalability by Design
API-First Compatibility
ORM Compatibility
Evolution Without Data Loss

Every entity introduced into the platform shall conform to these principles.

Table of Contents
Chapter 1 — Database Architecture Overview
Chapter 2 — Multi-Tenant Architecture
Chapter 3 — Database Schema Inventory
Chapter 4 — Business Domain Inventory
Chapter 5 — Platform Domain
Chapter 6 — Identity Domain
Chapter 7 — Tenant Domain
Chapter 8 — Restaurant Domain
Chapter 9 — Commerce Domain
Chapter 10 — Customer Domain
Chapter 11 — Reservation Domain
Chapter 12 — Billing Domain
Chapter 13 — Payment Domain
Chapter 14 — Notification Domain
Chapter 15 — Analytics Domain
Chapter 16 — Branding & Theme Domain
Chapter 17 — Feature Flag Domain
Chapter 18 — Shared Platform Domain
Chapter 19 — Entity Relationships
Chapter 20 — Tenant Ownership Model
Chapter 21 — Entity Lifecycle
Chapter 22 — Audit Architecture
Chapter 23 — Database Architectural Rules
Chapter 24 — Architecture Decision Records
Appendices
References
Revision History
Chapter 1 — Database Architecture Overview
1.1 Purpose

The FluxDine database is the central persistence layer supporting every application within the platform:

FluxDine HQ
Restaurant Platform
Customer Experience
Self-Service Onboarding
Shared Platform Services

The database is designed as a single logical multi-tenant system, where every business entity is associated with a tenant while maintaining strict isolation between restaurants.

1.2 Architectural Characteristics

The database architecture exhibits the following characteristics:

Single logical database
Multi-schema organization
Shared infrastructure
Tenant-aware entities
Unified identity
Explicit ownership
Domain-driven organization
Audit-ready design
Horizontal scalability
ORM-first compatibility
1.3 Architectural Layers

The logical organization of the database is:

Platform
│
├── Identity
├── Tenant
├── Restaurant
├── Commerce
├── Billing
├── Payment
├── Notification
├── Analytics
├── Branding
├── Feature Flags
└── Shared Services

Each layer encapsulates a distinct business capability and exposes well-defined relationships to other domains.

Chapter 2 — Multi-Tenant Architecture
2.1 Tenant Model

FluxDine adopts a shared-database, shared-schema multi-tenant architecture.

Every restaurant onboarded to the platform is represented as a Tenant.

All tenant-specific business data is associated with that tenant through explicit ownership relationships.

2.2 Tenant Isolation

Every tenant owns:

Restaurant
Branches
Menus
Categories
Menu Items
Customers
Orders
Reservations
Riders
Staff
Marketing Assets
Configuration
Payment Configuration
Domains
Themes
Reports

No tenant may directly access another tenant's data.

Isolation is enforced through:

Tenant identifiers
Authorization policies
Application services
Query filtering
Row ownership
2.3 Shared Platform Data

Certain entities exist independently of tenants and are shared across the entire platform.

Examples include:

Subscription Plans
Countries
Currencies
Languages
Feature Definitions
Platform Settings
System Roles
Global Permissions

These entities are managed exclusively by the FluxDine HQ platform.

Chapter 3 — Database Schema Inventory

The FluxDine platform is organized into logical schemas aligned with business capabilities.

Schema	Purpose
platform	Platform-wide configuration and HQ management
identity	Authentication, users, roles, permissions
tenant	Tenant lifecycle and ownership
restaurant	Restaurant operations and management
commerce	Orders, menus, products, pricing
reservation	Reservation management
billing	Subscriptions, invoices, billing records
payment	Payment gateways and transaction metadata
notification	Email, SMS, push notifications
analytics	Reports, KPIs, dashboards
branding	Themes, assets, domains
feature	Feature flags and entitlement management
audit	Audit logs and activity history

Each schema represents a bounded business context and communicates with others only through well-defined relationships and foreign key references.

Chapter 4 — Business Domain Inventory
4.1 Purpose

The FluxDine platform is organized around clearly defined business domains. Each domain encapsulates a cohesive set of business capabilities, owns its data, and exposes well-defined relationships to other domains. This domain-driven organization promotes modularity, maintainability, scalability, and clear ownership boundaries.

Every database entity belongs to exactly one primary business domain, even though it may participate in relationships spanning multiple domains.

4.2 Domain Architecture

The logical organization of the database domains is illustrated below.

Platform
│
├── Identity
├── Tenant
├── Restaurant
├── Commerce
├── Customer
├── Reservation
├── Billing
├── Payment
├── Notification
├── Analytics
├── Branding
├── Feature Management
└── Audit

Each domain is independently responsible for its business entities while cooperating with other domains through explicit foreign-key relationships.

4.3 Domain Responsibilities
Domain	Primary Responsibility
Platform	FluxDine HQ administration and platform configuration
Identity	Authentication, authorization, users, roles, permissions
Tenant	Restaurant tenancy and lifecycle management
Restaurant	Restaurant operational structure
Commerce	Ordering, menus, products, pricing
Customer	Customer profiles and engagement
Reservation	Reservation management
Billing	Subscription lifecycle and invoices
Payment	Payment gateway configuration and transaction metadata
Notification	Email, SMS, Push and notification history
Analytics	Reports, dashboards and KPIs
Branding	Themes, domains and branding assets
Feature Management	Feature flags and entitlement management
Audit	Audit logs and historical activities
4.4 Domain Ownership Principles

Every domain shall satisfy the following principles:

Single business responsibility
Explicit ownership
Independent evolution
Controlled cross-domain relationships
Tenant-aware where applicable
Auditability
Consistent lifecycle management
Chapter 5 — Platform Domain
5.1 Purpose

The Platform Domain contains entities used exclusively by the FluxDine HQ application. These entities represent platform-wide configuration and are never owned by individual restaurants.

The Platform Domain serves as the control plane for the entire SaaS platform.

5.2 Ownership

Owned by:

FluxDine Platform

Never owned by:

Tenant
Restaurant
Branch
Customer
5.3 Primary Entities

The Platform Domain consists of the following entities:

Platform

├── Platform Settings
├── Countries
├── Currencies
├── Languages
├── Timezones
├── Subscription Plans
├── Platform Features
├── Global Permissions
├── Global Roles
├── System Configurations
├── Maintenance Windows
├── Support Categories
├── Announcement Templates
├── Email Templates
├── Notification Templates
└── Platform Announcements
5.4 Entity Responsibilities
Platform Settings

Stores global platform configuration shared across all tenants.

Countries

Master list of supported countries.

Currencies

Supported billing and payment currencies.

Languages

Supported localization languages.

Timezones

Reference timezone catalogue.

Subscription Plans

Defines every subscription plan offered by FluxDine.

This entity is managed exclusively by HQ.

Restaurants reference plans but never modify them.

Platform Features

Master catalog of every feature available within FluxDine.

Examples include:

Reservations
Coupons
Loyalty
Multi-Branch
Online Payments
Analytics
AI Assistant
QR Ordering
Global Permissions

Master permission catalogue.

Examples:

restaurant.create

restaurant.update

order.view

billing.manage

tenant.delete
Global Roles

Defines platform-wide role templates.

Examples:

Super Administrator
Support Engineer
Billing Manager
Platform Administrator

Restaurant-specific roles are managed separately within the Identity Domain.

Email Templates

Reusable system email templates.

Notification Templates

Reusable notification templates.

Platform Announcements

Platform-wide announcements visible to selected audiences.

5.5 Platform Relationships

The Platform Domain is referenced by:

Tenant Domain
Billing Domain
Identity Domain
Notification Domain
Analytics Domain

It does not depend upon restaurant-owned entities.

Chapter 6 — Identity Domain
6.1 Purpose

The Identity Domain provides authentication and authorization services across the entire FluxDine ecosystem.

It implements the Unified Identity System approved in ADR-001, ensuring that every human user exists only once within the platform regardless of the number of restaurants or roles they possess.

6.2 Ownership

Identity entities are shared platform resources.

Individual user memberships are tenant-owned.

6.3 Identity Architecture
User

│

├── Credentials

├── Sessions

├── Roles

├── Permissions

├── Role Assignments

├── User Memberships

├── Authentication Providers

└── MFA Configuration
6.4 Primary Entities
Users

Represents every authenticated person within the FluxDine ecosystem.

A user may simultaneously be:

Customer
Restaurant Owner
Branch Manager
Rider
HQ Administrator
Support Agent

The User entity is never duplicated.

User Memberships

Associates users with tenants.

A user may belong to multiple restaurants.

Each membership defines:

Tenant
Assigned role
Status
Effective permissions
Roles

Defines collections of permissions.

Examples:

Head Admin
Branch Admin
Rider
Customer
Support Agent
Permissions

Represents atomic authorization capabilities.

Role Assignments

Maps users to roles within a specific tenant.

Sessions

Tracks authenticated login sessions.

Authentication Providers

Supports multiple authentication mechanisms.

Initially supported:

Email & Password

Future support:

Google
Microsoft
Apple
GitHub
MFA Configuration

Stores user multi-factor authentication preferences.

6.5 Identity Relationships

The Identity Domain connects to:

Tenant Domain
Customer Domain
Restaurant Domain
Billing Domain
Audit Domain

Every authenticated action throughout the platform ultimately references a User.

6.6 Identity Principles
One User record per person.
Unlimited tenant memberships.
Unlimited role assignments.
Tenant-scoped permissions.
Centralized authentication.
Decentralized authorization.

---

# Chapter 7 — Tenant Domain

## 7.1 Purpose

The Tenant Domain represents the core of FluxDine's SaaS architecture. Every restaurant that joins the platform becomes a **Tenant**. The Tenant Domain is responsible for managing the lifecycle, ownership, subscription, configuration, and operational boundaries of each restaurant organization.

The Tenant is the highest-level business entity owned by a customer of FluxDine.

---

## 7.2 Domain Responsibilities

The Tenant Domain is responsible for:

* Tenant registration
* Tenant activation
* Subscription association
* Restaurant ownership
* Domain ownership
* Feature entitlement
* Branding ownership
* Payment gateway configuration
* Platform configuration
* Tenant lifecycle
* Tenant status management

---

## 7.3 Domain Ownership

Owned By:

* FluxDine Customer (Restaurant Owner)

Managed By:

* FluxDine HQ
* Self-Service Platform
* Restaurant Head Administrator

---

## 7.4 Primary Entities

```text
Tenant Domain

├── Tenants
├── Tenant Settings
├── Tenant Domains
├── Tenant Branding
├── Tenant Feature Flags
├── Tenant Payment Gateways
├── Tenant Integrations
├── Tenant Subscription
├── Tenant Usage Metrics
├── Tenant Invitations
├── Tenant API Keys
└── Tenant Audit Settings
```

---

## 7.5 Entity Responsibilities

### Tenants

Represents one restaurant business onboarded to FluxDine.

This entity acts as the root owner for nearly every restaurant-specific record in the platform.

A Tenant owns:

* Restaurants
* Branches
* Users (through memberships)
* Menus
* Orders
* Customers
* Reservations
* Riders
* Reports
* Themes
* Domains
* Payment Configuration

---

### Tenant Settings

Stores operational configuration.

Examples include:

* Timezone
* Currency
* Language
* Date format
* Order preferences
* Reservation settings
* Notification preferences

---

### Tenant Domains

Represents every domain associated with a tenant.

Examples:

```text
restaurant.com

www.restaurant.com

orders.restaurant.com

restaurant.fluxdine.app
```

Each domain belongs to exactly one tenant.

---

### Tenant Branding

Stores restaurant-specific branding.

Examples:

* Logo
* Favicon
* Color palette
* Typography
* Theme configuration
* Email branding
* Invoice branding

---

### Tenant Feature Flags

Stores enabled features for the tenant.

Examples

```text
Reservations

Coupons

Loyalty

QR Ordering

AI Assistant

Multi Branch

Gift Cards
```

Feature availability depends on:

* Subscription
* Manual overrides
* Beta participation

---

### Tenant Payment Gateways

Stores configured payment providers.

Initial providers:

* Stripe
* PayPal

Future providers:

* Square
* Adyen
* Razorpay
* Paymob

No payment credentials are stored within restaurant entities.

All gateway configuration belongs to the Tenant.

---

### Tenant Integrations

Represents third-party integrations.

Examples:

* Google Maps
* Meta Pixel
* Google Analytics
* Twilio
* Mailgun
* WhatsApp Business API

---

### Tenant Subscription

Represents the active subscription assigned to a tenant.

References:

* Billing Domain
* Platform Subscription Plans

---

### Tenant Usage Metrics

Stores platform usage information.

Examples:

* Orders this month
* Active users
* Storage usage
* Email usage
* API usage

---

### Tenant Invitations

Supports inviting staff members.

Invitation lifecycle:

```text
Pending

↓

Accepted

↓

Expired

↓

Cancelled
```

---

### Tenant API Keys

Stores tenant-specific API credentials.

Examples:

* Public API key
* Secret API key
* Webhook secret

---

### Tenant Audit Settings

Defines audit retention policies.

Examples:

* Log retention
* GDPR preferences
* Export permissions

---

## 7.6 Tenant Relationships

```text
Tenant

├── owns → Restaurant

├── owns → Branches

├── owns → Customers

├── owns → Orders

├── owns → Reservations

├── owns → Menus

├── owns → Branding

├── owns → Domains

├── owns → Payment Configuration

└── references → Subscription Plan
```

---

## 7.7 Tenant Lifecycle

```text
Registration

↓

Verification

↓

Provisioning

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
```

Every entity owned by a tenant follows this lifecycle.

---

# Chapter 8 — Restaurant Domain

## 8.1 Purpose

The Restaurant Domain models the operational structure of a restaurant after onboarding.

While the Tenant Domain represents the SaaS customer relationship, the Restaurant Domain represents the day-to-day business operations of that tenant.

This domain powers the Restaurant Platform (Applications 2 and 3).

---

## 8.2 Domain Responsibilities

Responsible for:

* Restaurant profile
* Branch hierarchy
* Staff hierarchy
* Riders
* Business hours
* Delivery zones
* Service areas
* Kitchen configuration
* Tables
* Restaurant settings

---

## 8.3 Primary Entities

```text
Restaurant Domain

├── Restaurants
├── Branches
├── Branch Settings
├── Business Hours
├── Delivery Zones
├── Tables
├── Kitchen Stations
├── Staff
├── Riders
├── Rider Assignments
├── Restaurant Settings
└── Operating Schedule
```

---

## 8.4 Restaurants

Represents the primary restaurant owned by a tenant.

Every tenant owns exactly one logical restaurant.

Multi-location restaurants are represented using Branches.

---

## 8.5 Branches

Represents physical operating locations.

Each branch maintains:

* Address
* Contact information
* Hours
* Delivery availability
* Assigned staff
* Assigned riders
* Menu availability

---

## 8.6 Staff

Represents restaurant employees.

Examples:

* Head Admin
* Branch Admin
* Cashier
* Kitchen Manager
* Chef

Staff identities reference the Identity Domain.

No duplicate user records are created.

---

## 8.7 Riders

Represents delivery personnel.

A rider may be assigned to:

* one branch
* multiple branches (future expansion)

Each rider references:

* User
* Branch
* Availability
* Status

---

## 8.8 Restaurant Relationships

```text
Restaurant

├── contains → Branches

├── contains → Staff

├── contains → Riders

├── contains → Delivery Zones

├── contains → Tables

├── contains → Business Hours

├── contains → Settings

└── references → Tenant
```

---

## 8.9 Operational Ownership

Restaurant entities belong to:

Tenant

↓

Restaurant

↓

Branch

↓

Operational Resources

This hierarchy shall never be violated.

---

# Chapter 9 — Commerce Domain

## 9.1 Purpose

The Commerce Domain powers every customer-facing commercial activity within FluxDine.

It represents the operational heart of the ordering platform.

Every customer interaction ultimately passes through this domain.

---

## 9.2 Responsibilities

Responsible for:

* Menus
* Categories
* Products
* Product Variants
* Add-ons
* Carts
* Orders
* Discounts
* Coupons
* Taxes
* Fees
* Checkout

---

## 9.3 Primary Entities

```text
Commerce Domain

├── Menus
├── Menu Categories
├── Menu Items
├── Item Variants
├── Item Options
├── Item Add-ons
├── Carts
├── Cart Items
├── Orders
├── Order Items
├── Discounts
├── Coupons
├── Taxes
├── Fees
├── Delivery Charges
└── Checkout Sessions
```

---

## 9.4 Menu Hierarchy

```text
Menu

↓

Category

↓

Menu Item

↓

Variant

↓

Add-ons
```

This hierarchy supports unlimited menu complexity while maintaining a normalized data model.

---

## 9.5 Cart Model

A cart belongs to:

* one customer
* one restaurant
* one tenant

A cart contains:

* cart items
* quantities
* modifiers
* pricing snapshot

Only one active cart may exist per customer per restaurant.

---

## 9.6 Order Model

An order represents a finalized purchase.

Each order references:

* Customer
* Branch
* Restaurant
* Tenant
* Payment
* Delivery
* Reservation (optional)

Orders maintain immutable pricing snapshots to preserve historical accuracy.

---

## 9.7 Order Lifecycle

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

Alternative terminal states:

```text
Cancelled

Refunded

Failed
```

---

## 9.8 Commerce Relationships

```text
Restaurant

↓

Menu

↓

Category

↓

Menu Item

↓

Cart

↓

Order

↓

Payment
```

---

## 9.9 Commerce Ownership

Every commerce entity ultimately belongs to:

```text
Tenant

↓

Restaurant

↓

Branch

↓

Commerce Entity
```

No commerce entity may exist independently of a tenant.

---

## 9.10 Commerce Design Principles

The Commerce Domain adheres to the following principles:

* Immutable historical orders
* Version-safe pricing
* Normalized product catalog
* Tenant isolation
* Branch-aware inventory
* Snapshot-based order history
* Auditability
* Horizontal scalability
* Extensible pricing model
* Payment-provider independence through the Payment Service abstraction

---

Chapter 10 — Customer Domain
10.1 Purpose

The Customer Domain represents every customer who interacts with a restaurant through the FluxDine platform. It manages customer identity within the business context, customer preferences, addresses, loyalty, saved payment preferences, communication preferences, and historical interactions.

Unlike platform users, the Customer Domain models the commercial relationship between a customer and a tenant.

10.2 Domain Responsibilities

The Customer Domain is responsible for:

Customer profiles
Customer addresses
Customer favorites
Customer preferences
Customer loyalty
Customer reward points
Customer communication preferences
Customer order history
Customer segmentation
10.3 Domain Ownership

Owned by:

Tenant

Shared with:

Commerce Domain
Reservation Domain
Notification Domain
Analytics Domain
10.4 Primary Entities
Customer Domain

├── Customers
├── Customer Profiles
├── Customer Addresses
├── Customer Preferences
├── Customer Favorites
├── Customer Loyalty Accounts
├── Loyalty Transactions
├── Reward Points
├── Customer Tags
├── Customer Notes
├── Customer Devices
├── Communication Preferences
└── Customer Activity
10.5 Entity Responsibilities
Customers

Represents a customer relationship within a tenant.

A Customer references:

User
Tenant

This allows one platform user to become a customer of multiple restaurants while maintaining independent customer records for each tenant.

Customer Profiles

Stores customer-specific business information.

Examples:

Display name
Birthday
Preferred language
Marketing preferences
Customer Addresses

Stores delivery addresses.

Each customer may own multiple addresses.

Examples:

Home
Office
Apartment
Other
Customer Preferences

Stores ordering preferences.

Examples:

Preferred payment method
Preferred branch
Preferred delivery option
Dietary preferences
Customer Favorites

Stores bookmarked menu items.

Examples:

Favorite meals
Favorite restaurants
Frequently reordered items
Customer Loyalty Accounts

Represents the customer's loyalty membership within a tenant.

Loyalty Transactions

Tracks every loyalty event.

Examples:

Points earned
Points redeemed
Bonus campaigns
Manual adjustments
Reward Points

Maintains the customer's current point balance.

Customer Tags

Allows restaurants to classify customers.

Examples:

VIP

Frequent Buyer

Corporate

Inactive

High Value

First Time Customer
Customer Notes

Private notes created by restaurant staff.

Customer Devices

Stores registered customer devices.

Used for:

Push notifications
Device recognition
Security
Communication Preferences

Stores communication consent.

Examples:

Email
SMS
Push
Marketing
Transactional notifications
Customer Activity

Maintains customer engagement history.

Examples:

Login
Order
Reservation
Review
Loyalty redemption
10.6 Customer Relationships
Tenant

↓

Customers

├── Addresses

├── Preferences

├── Loyalty

├── Favorites

├── Activity

└── Orders
10.7 Customer Lifecycle
Guest

↓

Registered

↓

Verified

↓

Active

↓

VIP (optional)

↓

Inactive

↓

Archived
10.8 Customer Design Principles

The Customer Domain follows these principles:

One customer per tenant per user
Independent loyalty programs
Unlimited addresses
Unlimited order history
Privacy by design
GDPR compliance
Marketing consent tracking
Complete activity history
Chapter 11 — Reservation Domain
11.1 Purpose

The Reservation Domain manages restaurant reservations from booking through fulfillment while supporting restaurant capacity planning and customer scheduling.

The reservation system is independent of the order system but may optionally integrate with commerce.

11.2 Domain Responsibilities

Responsible for:

Reservations
Reservation tables
Reservation seating
Reservation schedules
Capacity planning
Waitlists
Reservation policies
11.3 Primary Entities
Reservation Domain

├── Reservations
├── Reservation Tables
├── Reservation Assignments
├── Reservation Waitlists
├── Reservation Policies
├── Seating Areas
├── Reservation Notes
└── Reservation Events
11.4 Reservations

Represents a customer reservation.

Each reservation references:

Tenant
Restaurant
Branch
Customer
Assigned Table (optional)
11.5 Reservation Tables

Represents physical dining tables.

Examples:

Table Number
Capacity
Area
Availability
11.6 Reservation Assignments

Maps reservations to physical tables.

Supports:

Single table
Multiple joined tables
Future table reassignment
11.7 Reservation Waitlists

Maintains waiting customers when capacity is exceeded.

11.8 Reservation Policies

Stores configurable reservation rules.

Examples:

Maximum party size
Advance booking window
Cancellation window
Reservation duration
No-show timeout
11.9 Seating Areas

Logical restaurant areas.

Examples:

Indoor
Outdoor
Rooftop
VIP
Private Room
11.10 Reservation Events

Tracks reservation history.

Examples:

Created
Confirmed
Modified
Checked In
Seated
Completed
Cancelled
11.11 Reservation Relationships
Restaurant

↓

Branch

↓

Seating Area

↓

Table

↓

Reservation

↓

Customer
11.12 Reservation Lifecycle
Pending

↓

Confirmed

↓

Upcoming

↓

Checked In

↓

Seated

↓

Completed

Alternative terminal states:

Cancelled

No Show

Expired
11.13 Reservation Design Principles
Tenant ownership
Branch awareness
Table assignment flexibility
Immutable reservation history
Capacity-aware scheduling
Event-driven lifecycle
Optional order integration
Chapter 12 — Billing Domain
12.1 Purpose

The Billing Domain manages the commercial relationship between FluxDine and its tenants.

Unlike the Commerce Domain, which processes restaurant sales, the Billing Domain manages SaaS subscriptions, invoicing, recurring billing, and platform revenue.

12.2 Domain Responsibilities

Responsible for:

Tenant subscriptions
Billing accounts
Invoices
Invoice items
Usage billing
Credits
Discounts
Refunds
Tax records
12.3 Domain Ownership

Owned by:

FluxDine HQ

Referenced by:

Tenant Domain
Payment Domain
Analytics Domain
12.4 Primary Entities
Billing Domain

├── Billing Accounts
├── Subscriptions
├── Subscription Plans
├── Subscription Features
├── Invoices
├── Invoice Items
├── Credits
├── Credit Transactions
├── Discounts
├── Refunds
├── Tax Records
└── Billing Events
12.5 Billing Accounts

Represents the financial account associated with a tenant.

Each tenant owns one billing account.

12.6 Subscriptions

Represents the active SaaS subscription.

References:

Tenant
Platform Plan
12.7 Subscription Features

Represents the effective feature entitlement inherited from the assigned subscription plan, with support for tenant-specific overrides through the Feature Management Domain.

12.8 Invoices

Represents billing documents generated by FluxDine.

Invoices are immutable after issuance.

12.9 Invoice Items

Represents individual invoice line items.

Examples:

Monthly subscription
One-time setup fee
Usage charges
Additional services
12.10 Credits

Stores available billing credits.

Examples:

Promotional credit
Manual adjustment
Service recovery credit
12.11 Credit Transactions

Maintains complete history of credit usage.

12.12 Discounts

Represents subscription discounts.

Examples:

Promotional campaigns
Coupon codes
Lifetime discounts
Manual discounts
12.13 Refunds

Tracks subscription refunds.

Every refund references:

Invoice
Payment Transaction
Refund Reason
12.14 Tax Records

Stores taxation information applicable to invoices.

12.15 Billing Events

Captures lifecycle events.

Examples:

Trial Started
Subscription Activated
Renewal
Upgrade
Downgrade
Payment Failed
Cancellation
Refund Issued
12.16 Billing Relationships
Tenant

↓

Billing Account

↓

Subscription

↓

Invoice

↓

Invoice Items

↓

Payment
12.17 Subscription Lifecycle
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
12.18 Billing Design Principles

The Billing Domain follows these principles:

One billing account per tenant
Immutable invoices
Complete financial audit trail
Plan-driven feature entitlement
Usage-aware billing support
Provider-independent payment processing
Separation of billing and payment concerns

---

Chapter 13 — Payment Domain
13.1 Purpose

The Payment Domain manages payment gateway configuration, payment transaction metadata, webhook processing, reconciliation, and payment lifecycle management across the FluxDine platform.

The Payment Domain is intentionally separated from the Billing Domain and Commerce Domain.

Commerce owns restaurant sales.
Billing owns SaaS subscriptions.
Payment owns payment processing infrastructure.

This separation allows FluxDine to support multiple payment providers through a common abstraction layer while maintaining provider independence.

13.2 Domain Responsibilities

The Payment Domain is responsible for:

Payment Gateway Management
Gateway Configuration
Payment Transactions
Payment Intents
Refund Transactions
Webhook Processing
Payment Events
Reconciliation
Payment Methods
Payment Tokens
13.3 Ownership

Platform-Owned

Payment Providers
Provider Capabilities

Tenant-Owned

Gateway Configuration
Merchant Credentials
Webhook Secrets
Payment Settings

Commerce-Owned

Orders

Billing-Owned

Subscription Payments
13.4 Primary Entities
Payment Domain

├── Payment Providers
├── Payment Gateway Configurations
├── Merchant Accounts
├── Payment Transactions
├── Payment Intents
├── Refund Transactions
├── Payment Methods
├── Payment Tokens
├── Webhook Events
├── Reconciliation Records
├── Settlement Records
├── Payment Logs
└── Payment Events
13.5 Entity Responsibilities
Payment Providers

Represents supported payment gateways.

Initial providers:

Stripe
PayPal

Future providers:

Square
Adyen
Razorpay
Paymob
Authorize.Net
Checkout.com
Payment Gateway Configurations

Stores tenant-specific payment gateway settings.

Each tenant may configure:

Stripe
PayPal
Additional future providers

Each configuration belongs to exactly one tenant.

Merchant Accounts

Represents the merchant account registered with a payment provider.

Examples:

Stripe Connected Account
PayPal Business Account
Payment Transactions

Represents every payment attempt processed through the platform.

Transactions may originate from:

Restaurant Orders
Subscription Billing
Future Marketplace Services
Payment Intents

Represents pending payment authorization before capture.

Used primarily by providers supporting two-step payment flows.

Refund Transactions

Represents refund operations.

Every refund references:

Original transaction
Refund reason
Refund amount
Provider response
Payment Methods

Stores reusable customer payment methods where supported.

Examples:

Card
PayPal Account
Apple Pay
Google Pay

Sensitive payment information shall never be stored directly within the FluxDine database.

Payment Tokens

Stores provider-issued payment tokens only.

Raw card information is never persisted.

Webhook Events

Stores incoming webhook payload metadata.

Examples:

Payment Succeeded
Payment Failed
Refund Created
Subscription Renewed
Reconciliation Records

Used for financial reconciliation between:

FluxDine
Merchant
Payment Provider
Settlement Records

Tracks settlement status.

Examples:

Pending
Settled
Failed
Reversed
Payment Logs

Stores operational logs related to payment processing.

Payment Events

Captures the payment lifecycle.

Examples:

Created
Authorized
Captured
Failed
Refunded
Cancelled
13.6 Payment Relationships
Tenant
      │
      ▼
Gateway Configuration
      │
      ▼
Merchant Account
      │
      ▼
Payment Transaction
      │
      ├── Payment Intent
      ├── Refund
      ├── Webhook Events
      └── Settlement
13.7 Payment Lifecycle
Created

↓

Pending

↓

Authorized

↓

Captured

↓

Completed

Alternative terminal states

Failed

Cancelled

Expired

Refunded

Partially Refunded
13.8 Payment Architecture Principles

The Payment Domain follows these principles:

Gateway abstraction
Provider independence
PCI-aware design
Tokenized payment information
Immutable transaction history
Webhook-driven synchronization
Event-based processing
Tenant isolation
Chapter 14 — Notification Domain
14.1 Purpose

The Notification Domain manages all outbound communications generated by the FluxDine platform.

It provides a centralized communication service shared by every application.

14.2 Responsibilities

Responsible for:

Email
SMS
Push Notifications
In-App Notifications
Notification Templates
Notification Preferences
Delivery Tracking
Notification History
14.3 Primary Entities
Notification Domain

├── Notifications
├── Notification Templates
├── Notification Channels
├── Notification Preferences
├── Email Messages
├── SMS Messages
├── Push Notifications
├── Delivery Attempts
├── Notification Queue
└── Notification Events
14.4 Entity Responsibilities
Notifications

Represents logical notifications before channel delivery.

Notification Templates

Reusable templates.

Examples:

Welcome Email
Password Reset
Order Confirmation
Reservation Confirmation
Invoice Generated
Notification Channels

Supported delivery channels.

Examples:

Email
SMS
Push
In-App
Notification Preferences

Stores user communication preferences.

Email Messages

Email delivery records.

SMS Messages

SMS delivery records.

Push Notifications

Mobile notification records.

Delivery Attempts

Tracks retries and delivery status.

Notification Queue

Pending outbound notifications awaiting processing.

Notification Events

Lifecycle history.

Examples:

Queued
Sent
Delivered
Failed
Retried
14.5 Notification Relationships
Notification

├── Template

├── Channel

├── Recipient

├── Delivery

└── Events
14.6 Notification Principles
Shared platform service
Event-driven delivery
Template-driven messaging
Retry capable
Channel independent
Complete delivery history
Chapter 15 — Analytics Domain
15.1 Purpose

The Analytics Domain provides reporting, KPIs, operational metrics, business intelligence, and dashboard data across the entire FluxDine ecosystem.

It consumes data from every business domain without owning operational records.

15.2 Responsibilities

Responsible for:

KPIs
Dashboard metrics
Sales reports
Customer analytics
Restaurant analytics
Platform analytics
Billing analytics
Operational metrics
15.3 Primary Entities
Analytics Domain

├── Dashboard Metrics
├── KPI Definitions
├── KPI Snapshots
├── Sales Reports
├── Customer Reports
├── Restaurant Reports
├── Billing Reports
├── Platform Reports
├── Usage Metrics
├── Aggregated Statistics
└── Scheduled Reports
15.4 Entity Responsibilities
Dashboard Metrics

Precomputed dashboard values.

KPI Definitions

Defines measurable business indicators.

Examples:

Monthly Revenue
Orders per Day
Average Order Value
Customer Retention
Conversion Rate
KPI Snapshots

Historical KPI measurements.

Sales Reports

Restaurant sales reporting.

Customer Reports

Customer behavior analysis.

Restaurant Reports

Operational reporting.

Billing Reports

Subscription revenue analysis.

Platform Reports

Platform-wide operational metrics.

Usage Metrics

Tenant usage statistics.

Aggregated Statistics

Precomputed analytical summaries.

Scheduled Reports

Automatically generated reports.

15.5 Analytics Relationships
Analytics

▲

Consumes

│

Commerce

Customer

Restaurant

Billing

Payment

Reservation

Notification

Analytics never owns business transactions.

15.6 Analytics Principles
Read-optimized
Historical
Immutable snapshots
Aggregation focused
Tenant-aware
Platform-aware
Performance optimized
Chapter 16 — Branding & Theme Domain
16.1 Purpose

The Branding Domain enables every tenant to create a unique branded customer experience while using the shared FluxDine platform.

This domain separates branding concerns from restaurant operations.

16.2 Responsibilities

Responsible for:

Themes
Brand Assets
Logos
Color Palettes
Typography
Domains
Favicons
SEO Configuration
Social Metadata
16.3 Primary Entities
Branding Domain

├── Themes
├── Theme Presets
├── Brand Assets
├── Logos
├── Favicons
├── Color Palettes
├── Typography
├── Domains
├── SEO Settings
├── Social Metadata
└── Branding History
16.4 Entity Responsibilities
Themes

Represents the active visual theme.

Theme Presets

Reusable design templates.

Brand Assets

Stores uploaded branding resources.

Logos

Restaurant logo assets.

Favicons

Browser icons.

Color Palettes

Primary, secondary, accent colors.

Typography

Font configuration.

Domains

Represents customer-facing domains.

Examples:

restaurant.com
www.restaurant.com
orders.restaurant.com
SEO Settings

Metadata controlling search engine visibility.

Social Metadata

Open Graph and social sharing configuration.

Branding History

Version history for branding changes.

16.5 Branding Relationships
Tenant

↓

Branding

├── Theme

├── Assets

├── Domains

├── SEO

└── Social Metadata
16.6 Branding Principles
Complete tenant ownership
Theme isolation
Independent branding
Version history
Domain independence
SEO-aware
Extensible theme engine

---

# Chapter 17 — Feature Management Domain

## 17.1 Purpose

The Feature Management Domain controls the capabilities available across the FluxDine platform. It provides a centralized framework for enabling, disabling, and governing features at the platform, subscription, tenant, and future user levels.

Feature Management is a core component of the SaaS architecture and is independent of business logic. Every application—HQ Platform, Restaurant Platform, Customer Experience, and Self-Service Onboarding—relies on this domain to determine feature availability.

---

## 17.2 Domain Responsibilities

The Feature Management Domain is responsible for:

- Master Feature Catalog
- Feature Categories
- Subscription Feature Mapping
- Tenant Feature Overrides
- Feature Flags
- Beta Feature Management
- Experimental Features
- Feature Usage Tracking
- Feature Lifecycle Management

---

## 17.3 Ownership

### Platform-Owned

- Feature Catalog
- Feature Categories
- Feature Definitions
- Feature Dependencies

### Subscription-Owned

- Plan Feature Mapping

### Tenant-Owned

- Feature Overrides
- Beta Participation
- Experimental Access

---

## 17.4 Primary Entities

```text
Feature Domain

├── Features
├── Feature Categories
├── Feature Flags
├── Feature Dependencies
├── Subscription Features
├── Tenant Features
├── Beta Features
├── Experimental Features
├── Feature Usage
└── Feature Events
```

---

## 17.5 Entity Responsibilities

### Features

Master catalog of every feature supported by FluxDine.

Examples:

- Reservations
- Coupons
- Loyalty
- QR Ordering
- AI Assistant
- Multi Branch
- Online Payments
- Gift Cards
- Table Ordering

---

### Feature Categories

Logical grouping of features.

Examples:

- Commerce
- Marketing
- Restaurant Operations
- Billing
- AI
- Reporting
- Customer Experience

---

### Feature Flags

Represents runtime feature toggles.

Feature flags allow controlled rollout without requiring deployment.

---

### Feature Dependencies

Defines relationships between features.

Examples:

```
Online Payments

↓

Payment Gateway Configuration

↓

Stripe
```

or

```
Loyalty

↓

Customer Accounts
```

---

### Subscription Features

Defines which features are available for each subscription plan.

---

### Tenant Features

Represents tenant-specific overrides.

Examples:

- Manual enablement
- Manual disablement
- Promotional access

---

### Beta Features

Features available only to selected tenants.

---

### Experimental Features

Internal development features.

Accessible only by FluxDine HQ.

---

### Feature Usage

Stores feature utilization metrics.

Examples:

- Last Used
- Total Usage
- Active Users
- Usage Frequency

---

### Feature Events

Tracks feature lifecycle.

Examples:

- Enabled
- Disabled
- Upgraded
- Downgraded
- Expired

---

## 17.6 Feature Relationships

```text
Platform

↓

Subscription Plan

↓

Feature

↓

Tenant Feature

↓

Application Module
```

---

## 17.7 Feature Architecture Principles

The Feature Domain follows these principles:

- Platform-managed
- Runtime configurable
- Subscription-aware
- Tenant-aware
- Extensible
- Backward compatible
- Version independent
- Audit capable

---

# Chapter 18 — Shared Platform Domain

## 18.1 Purpose

The Shared Platform Domain contains platform services used by every application within the FluxDine ecosystem.

Unlike business domains, these entities do not belong to a single module. Instead, they provide cross-cutting capabilities that support the entire SaaS platform.

The Shared Platform Domain represents the technical foundation of FluxDine.

---

## 18.2 Responsibilities

Responsible for:

- Identity Integration
- File Storage
- Search
- Audit
- Logging
- Monitoring
- Configuration
- Background Jobs
- Event Processing
- System Health

---

## 18.3 Primary Entities

```text
Shared Platform Domain

├── Files
├── Media Assets
├── Audit Logs
├── Activity Logs
├── System Logs
├── Monitoring Events
├── Background Jobs
├── Scheduled Tasks
├── Search Indexes
├── Search Documents
├── API Keys
├── Webhook Registrations
├── System Events
├── Event Queue
└── Health Checks
```

---

## 18.4 Entity Responsibilities

### Files

Stores uploaded files.

Examples:

- Images
- PDFs
- Documents
- CSV
- Reports

---

### Media Assets

Represents media used throughout the platform.

Examples:

- Product Images
- Logos
- Hero Banners
- Gallery Images

---

### Audit Logs

Maintains immutable audit history.

Every business domain contributes records to the Audit Domain.

---

### Activity Logs

Stores user activity.

Examples:

- Login
- Logout
- Order Creation
- Reservation Update

---

### System Logs

Captures application diagnostics.

Examples:

- Errors
- Warnings
- Exceptions
- Performance

---

### Monitoring Events

Stores infrastructure monitoring data.

Examples:

- CPU
- Memory
- Queue Depth
- Response Time

---

### Background Jobs

Represents asynchronous work.

Examples:

- Email Processing
- Payment Sync
- Reservation Updates
- Analytics Aggregation

---

### Scheduled Tasks

Platform automation.

Examples:

- Subscription Renewal
- Cleanup
- Backups
- Daily Reports

---

### Search Indexes

Defines searchable datasets.

---

### Search Documents

Optimized search records.

---

### API Keys

Stores secure API credentials.

---

### Webhook Registrations

Represents outgoing webhook subscriptions.

---

### System Events

Represents events generated across the platform.

---

### Event Queue

Central event processing queue.

---

### Health Checks

Tracks operational health.

Examples:

- Database
- Cache
- Queue
- Email Provider
- Payment Provider

---

## 18.5 Shared Platform Relationships

```text
Every Business Domain

↓

Shared Platform Services

├── Audit

├── Logging

├── Monitoring

├── Files

├── Events

├── Search

└── Background Jobs
```

---

## 18.6 Shared Platform Principles

- Shared by every application
- Independent from business logic
- Horizontally scalable
- Event-driven
- Highly available
- Observable
- Secure
- Extensible

---

# Chapter 19 — Entity Relationships

## 19.1 Purpose

The Entity Relationship Model defines how every business entity within the FluxDine platform is connected.

Rather than documenting individual foreign keys (covered in later Engineering Specifications), this chapter defines the logical ownership hierarchy and relationship principles that govern the entire data model.

---

## 19.2 Top-Level Ownership Hierarchy

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

This hierarchy forms the foundation of every ownership and access decision throughout the platform.

---

## 19.3 Complete Logical Relationship Model

```text
Platform
│
├── Subscription Plans
├── Features
├── Countries
├── Currencies
├── Permissions
└── Roles
        │
        ▼
Tenant
│
├── Tenant Settings
├── Domains
├── Branding
├── Subscription
├── Payment Gateway
├── Integrations
├── Feature Overrides
└── API Keys
        │
        ▼
Restaurant
│
├── Branches
├── Staff
├── Riders
├── Menus
├── Customers
├── Reservations
├── Reports
└── Settings
        │
        ▼
Commerce
│
├── Categories
├── Menu Items
├── Variants
├── Add-ons
├── Cart
├── Orders
└── Payments
```

---

## 19.4 Cross-Domain Relationships

Several domains interact while maintaining clear ownership boundaries.

### Identity

Referenced by every authenticated entity.

---

### Billing

Referenced by:

- Tenant
- Payment
- Analytics

---

### Notification

Consumes events from:

- Commerce
- Reservation
- Billing
- Customer
- Identity

---

### Analytics

Consumes data from every operational domain without owning business records.

---

### Feature Management

Controls runtime capabilities across every application.

---

### Shared Platform Services

Provides infrastructure support to all domains.

---

## 19.5 Relationship Principles

Every relationship shall satisfy the following principles:

### Explicit Ownership

Every entity shall have exactly one logical owner.

---

### No Circular Ownership

Entities may reference one another but ownership shall never become circular.

---

### Tenant Isolation

Relationships may never cross tenant boundaries unless explicitly owned by the Platform.

---

### Referential Integrity

Every relationship shall be enforced through documented foreign keys defined in subsequent engineering specifications.

---

### Auditability

Every business relationship shall support complete historical traceability.

---

# Chapter 20 — Tenant Ownership Model

## 20.1 Purpose

The Tenant Ownership Model defines how ownership flows throughout the entire database.

It is the most important architectural rule within the FluxDine platform because it guarantees multi-tenant isolation and establishes the security boundary for every business entity.

---

## 20.2 Ownership Hierarchy

```text
FluxDine Platform
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
        ▼
Business Resources
```

---

## 20.3 Ownership Rules

### Rule 1 — Single Root Owner

Every tenant-owned entity ultimately belongs to exactly one Tenant.

---

### Rule 2 — Platform-Owned Entities

Platform entities have no tenant owner.

Examples:

- Subscription Plans
- Countries
- Permissions
- Roles
- Feature Catalog

---

### Rule 3 — Restaurant Ownership

Every restaurant belongs to one Tenant.

---

### Rule 4 — Branch Ownership

Every branch belongs to one Restaurant.

---

### Rule 5 — Commerce Ownership

Every commerce entity ultimately belongs to one Tenant through its Restaurant and Branch hierarchy.

---

### Rule 6 — Customer Ownership

A customer record belongs to one Tenant while referencing a shared User identity.

---

### Rule 7 — Shared Identity

Users are platform-wide entities.

Memberships connect Users to Tenants.

---

### Rule 8 — Shared Services

Infrastructure entities may be shared while maintaining tenant context where required.

---

## 20.4 Ownership Matrix

| Entity | Owner |
|---------|-------|
| Subscription Plan | Platform |
| Feature | Platform |
| User | Platform |
| Tenant | Platform |
| Restaurant | Tenant |
| Branch | Restaurant |
| Customer | Tenant |
| Menu | Restaurant |
| Order | Tenant |
| Reservation | Tenant |
| Theme | Tenant |
| Domain | Tenant |
| Payment Gateway | Tenant |
| Invoice | Billing Account |
| Payment Transaction | Tenant/Billing Context |
| Audit Log | Platform (tenant-aware) |

---

## 20.5 Ownership Principles

The FluxDine ownership model is governed by the following principles:

- Single ownership hierarchy
- Complete tenant isolation
- Explicit parent-child relationships
- Platform-controlled shared resources
- Referential integrity
- Horizontal scalability
- Security by ownership
- Auditability by design

---
# Chapter 21 — Entity Lifecycle

## 21.1 Purpose

Every entity within the FluxDine platform follows a defined lifecycle from creation through archival. Establishing standardized lifecycle states ensures consistency across business domains, simplifies business logic, enables predictable reporting, and supports auditing throughout the platform.

Each business domain may extend its lifecycle with domain-specific states, but all entities shall adhere to the lifecycle principles defined in this chapter.

---

## 21.2 Lifecycle Philosophy

The lifecycle of every entity shall be:

- Predictable
- Traceable
- Auditable
- Recoverable
- Extensible
- Tenant-aware
- Backward compatible

Lifecycle transitions shall never result in data loss.

---

## 21.3 Universal Entity Lifecycle

Every persistent business entity shall progress through the following lifecycle.

```text
Created

↓

Active

↓

Updated (0..N)

↓

Suspended (Optional)

↓

Archived

↓

Soft Deleted

↓

Hard Deleted (Administrative Only)
```

---

## 21.4 Lifecycle Definitions

### Created

The entity has been successfully created.

---

### Active

The entity is operational and available for normal business operations.

---

### Updated

The entity has undergone one or more modifications while remaining active.

---

### Suspended

The entity is temporarily unavailable but retained.

Examples:

- Suspended Tenant
- Disabled User
- Inactive Payment Gateway

---

### Archived

The entity is no longer operational but remains available for historical reporting.

---

### Soft Deleted

The entity is logically deleted but retained for recovery and audit purposes.

---

### Hard Deleted

Permanent removal from the database.

Hard deletion shall only occur:

- when legally permitted
- after retention requirements expire
- through administrative maintenance processes

---

## 21.5 Domain-Specific Lifecycles

Individual domains extend the universal lifecycle.

Examples:

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

Assigned

↓

Out For Delivery

↓

Delivered

↓

Completed
```

---

### Reservations

```text
Pending

↓

Confirmed

↓

Upcoming

↓

Checked In

↓

Seated

↓

Completed
```

---

### Subscriptions

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

### Payments

```text
Created

↓

Authorized

↓

Captured

↓

Completed
```

---

## 21.6 Lifecycle Principles

- No destructive updates.
- Historical records remain immutable where required.
- Lifecycle transitions are auditable.
- Domain-specific states extend, rather than replace, the universal lifecycle.
- Soft deletion is preferred over physical deletion.

---

# Chapter 22 — Audit Architecture

## 22.1 Purpose

Auditability is a foundational architectural principle of FluxDine. Every significant business operation shall generate an immutable audit record to ensure accountability, traceability, operational transparency, and regulatory compliance.

---

## 22.2 Audit Philosophy

Audit logging shall be:

- Automatic
- Immutable
- Centralized
- Tenant-aware
- User-aware
- Time-aware
- Searchable

---

## 22.3 Audit Sources

Audit records originate from every business domain.

Examples:

- Identity
- Tenant
- Restaurant
- Commerce
- Reservation
- Billing
- Payment
- Notification
- Analytics
- Branding
- Feature Management

---

## 22.4 Audit Events

Examples include:

- Login
- Logout
- Restaurant Created
- Branch Created
- Menu Updated
- Order Created
- Order Cancelled
- Reservation Confirmed
- Payment Completed
- Subscription Renewed
- Feature Enabled
- Domain Connected
- Theme Updated

---

## 22.5 Audit Record Model

Every audit record shall capture:

- Event Identifier
- Event Type
- Timestamp
- Tenant Identifier (where applicable)
- User Identifier
- Entity Type
- Entity Identifier
- Action Performed
- Previous State (where applicable)
- New State (where applicable)
- Source Application
- Client IP
- User Agent
- Correlation Identifier

---

## 22.6 Audit Retention

Audit records shall never be modified.

Retention policies shall be configurable according to:

- Legal requirements
- Customer agreements
- Platform policy

---

## 22.7 Audit Principles

- Immutable history
- Centralized storage
- Complete traceability
- Tenant isolation
- Cross-domain visibility
- Efficient querying

---

# Chapter 23 — Database Architectural Rules

## 23.1 Purpose

This chapter defines the mandatory engineering rules governing every database object within the FluxDine platform.

These rules supersede implementation preferences and shall be followed across all environments.

---

## 23.2 Architecture Rules

### Rule 1 — Multi-Tenant First

Every business entity shall explicitly support multi-tenancy unless designated as a platform-owned entity.

---

### Rule 2 — Unified Identity

Every authenticated person shall be represented by a single User record.

---

### Rule 3 — Tenant Isolation

No tenant-owned entity may reference another tenant's data.

---

### Rule 4 — Explicit Ownership

Every entity shall have one clearly defined logical owner.

---

### Rule 5 — Normalization

The database shall remain normalized unless documented performance considerations justify denormalization.

---

### Rule 6 — Immutable History

Historical business records shall never be overwritten.

---

### Rule 7 — Soft Delete

Logical deletion shall be preferred over physical deletion.

---

### Rule 8 — Auditability

Every significant business operation shall generate an audit record.

---

### Rule 9 — Gateway Independence

Payment providers shall be accessed exclusively through the Payment Service abstraction.

---

### Rule 10 — Domain Separation

Business domains shall remain independent while interacting through explicit relationships.

---

### Rule 11 — Documentation First

Database changes shall be documented before implementation.

---

### Rule 12 — Migration First

Schema evolution shall occur exclusively through approved database migrations.

---

## 23.3 Compliance

Every database change shall comply with:

- Database Naming Standards
- Schema Specification
- Table Specifications
- Relationship Specifications
- Constraint Specifications
- Migration Standards
- Drizzle ORM Standards

---

# Chapter 24 — Architecture Decision Records

The following Architecture Decision Records govern the Complete Database Schema Specification.

---

## ADR-127 — Domain-Driven Database Organization

Database schemas shall be organized according to business domains rather than technical layers.

---

## ADR-128 — Shared Database Multi-Tenant Architecture

FluxDine shall use a shared database with strict tenant isolation.

---

## ADR-129 — Unified Identity

Every person shall exist exactly once within the platform.

---

## ADR-130 — Tenant Ownership Hierarchy

Every tenant-owned entity shall trace its ownership back to a single Tenant.

---

## ADR-131 — Platform-Owned Reference Data

Global configuration data shall remain platform-owned.

---

## ADR-132 — Shared Platform Services

Cross-cutting services shall remain independent of business domains.

---

## ADR-133 — Gateway Abstraction

Payment providers shall be accessed exclusively through the Payment Service abstraction.

---

## ADR-134 — Immutable Audit History

Audit records shall never be modified after creation.

---

## ADR-135 — Soft Delete Policy

Logical deletion shall be the default deletion strategy.

---

## ADR-136 — Documentation Before Implementation

Engineering specifications shall be updated before implementation begins.

---

## ADR-137 — Migration-Based Schema Evolution

All schema changes shall be introduced through version-controlled migrations.

---

## ADR-138 — Explicit Relationship Ownership

Relationships shall always have documented ownership boundaries.

---

## ADR-139 — Event-Driven Architecture

Cross-domain communication should favor events over tight coupling where appropriate.

---

## ADR-140 — Database as the Platform Foundation

The database architecture defined in this document is the authoritative logical data model for the FluxDine platform and serves as the foundation for all ORM mappings, APIs, backend services, and future platform evolution.

---

# Appendices

## Appendix A — Complete Domain Inventory

- Platform
- Identity
- Tenant
- Restaurant
- Commerce
- Customer
- Reservation
- Billing
- Payment
- Notification
- Analytics
- Branding
- Feature Management
- Shared Platform

---

## Appendix B — Entity Ownership Hierarchy

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

## Appendix C — Cross-Domain Dependency Summary

| Domain | Depends On |
|---------|------------|
| Identity | Platform |
| Tenant | Platform |
| Restaurant | Tenant |
| Commerce | Restaurant |
| Customer | Identity, Tenant |
| Reservation | Restaurant |
| Billing | Platform, Tenant |
| Payment | Billing, Commerce |
| Notification | All Business Domains |
| Analytics | All Business Domains |
| Branding | Tenant |
| Feature Management | Platform |
| Shared Platform | All Business Domains |

---

## Appendix D — Database Engineering Checklist

Before implementation:

- Database Naming Standards approved
- Schema Specification approved
- Relationships documented
- Constraints documented
- Indexes documented
- Migration strategy prepared
- Drizzle mapping prepared

---

# References

## Core Architecture

- 04 System Architecture Blueprint
- 05 Database Architecture & Multi-Tenant Data Model
- 06 API & Service Architecture
- 07 Security Architecture
- 08 Infrastructure Architecture

---

## Engineering Specifications

- 00 Database Naming Standards
- 02 Table Specifications
- 03 Relationships
- 04 Constraints
- 05 Index Specification
- 06 Enum Specification
- 07 Database Migration Strategy
- 08 Drizzle ORM Mapping

---

# Revision History

| Version | Date | Author | Changes |
|----------|------|---------|----------|
| 1.0 | 2026 | FluxDine Engineering | Initial Release |

---

