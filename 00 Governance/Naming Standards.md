# 00 Governance

# Naming Standards

---

# Document Control

| Field | Value |
|---|---|
| Document ID | FD-GOV-009 |
| Document Name | Naming Standards |
| Version | 1.0 |
| Status | Approved and Locked |
| Owner | FluxDine Architecture Team |
| Classification | Governance Standard |

---

# Purpose

This document defines naming conventions for FluxDine architecture, code, APIs, databases, services, documentation, and infrastructure.

Consistent naming improves discoverability and reduces ambiguity.

---

# General Naming Principle

Names should be:

- Clear
- Explicit
- Consistent
- Domain-oriented
- Unambiguous

Avoid unnecessary abbreviations.

---

# Services

Services use Pascal Case in documentation:

```text
Identity Service
Tenant Service
Restaurant Service
Commerce Service
Billing Service
Payment Service
Notification Service
Email Service
Analytics Service
Domain Service
Theme Service
Feature Flag Service
Audit Service
Logging Service
Monitoring Service
File Storage Service
Search Service
```

---

# Applications

Application names should describe their business purpose.

Examples:

```text
HQ Platform
Restaurant Platform
Self-Service Platform
Customer Platform
Rider Platform
```

---

# Database Tables

Database tables should use:

```text
snake_case
```

Examples:

```text
users
tenant_members
restaurants
menu_items
orders
order_items
payment_transactions
audit_events
```

---

# Database Columns

Database columns use:

```text
snake_case
```

Examples:

```text
created_at
updated_at
tenant_id
restaurant_id
current_period_end
```

---

# Primary Keys

Primary key columns should normally use:

```text
id
```

Foreign keys should use:

```text
<entity>_id
```

Examples:

```text
tenant_id
restaurant_id
customer_id
order_id
```

---

# API Endpoints

API resource paths should use plural nouns.

Example:

```text
/api/v1/restaurants
/api/v1/orders
/api/v1/customers
```

Avoid verbs in resource paths where HTTP semantics already express the operation.

---

# API Versioning

API versions use:

```text
v1
v2
v3
```

Example:

```text
/api/v1/orders
```

---

# JSON Fields

JSON fields use:

```text
camelCase
```

Example:

```json
{
  "restaurantId": "...",
  "createdAt": "...",
  "currentPeriodEnd": "..."
}
```

---

# TypeScript / JavaScript

Use:

### Variables and functions

```text
camelCase
```

### Classes and types

```text
PascalCase
```

### Constants

Use descriptive naming consistent with the project's TypeScript conventions.

---

# React Components

React components use:

```text
PascalCase
```

Examples:

```text
OrderCard
RestaurantDashboard
PaymentForm
ThemePreview
```

---

# Hooks

React hooks use:

```text
use<Name>
```

Examples:

```text
useAuth
useTenant
useOrders
useReservations
```

---

# Files

File naming shall follow the surrounding project convention.

Architecture Markdown files use descriptive names.

---

# Events

Domain events should use past-tense terminology where possible.

Examples:

```text
OrderCreated
OrderConfirmed
PaymentSucceeded
PaymentFailed
SubscriptionActivated
RestaurantLaunched
```

Events describe something that happened.

---

# Commands

Commands describe an intended action.

Examples:

```text
CreateOrder
ActivateSubscription
PublishTheme
VerifyDomain
```

---

# State Names

States should be:

- Explicit
- Mutually understandable
- Consistent

Examples:

```text
Pending
Upcoming
Active
Fulfilled
Canceled
```

---

# Environment Variables

Environment variables should use uppercase snake case.

Examples:

```text
DATABASE_URL
API_BASE_URL
CRON_SECRET
PAYMENT_PROVIDER_KEY
```

Secrets must never be committed to Git.

---

# Git Branches

Branches should use descriptive prefixes.

Examples:

```text
feature/
fix/
refactor/
docs/
chore/
hotfix/
```

Example:

```text
feature/payment-gateway-abstraction
```

---

# Git Commits

Commits follow Conventional Commits.

Examples:

```text
feat: add reservation lifecycle
fix: resolve payment callback issue
docs: add payment service architecture
refactor: simplify order calculation
test: add reservation integration tests
chore: update dependencies
```

---

# ADR Naming

ADR files use:

```text
ADR-### <Decision Name>.md
```

Example:

```text
ADR-018 Payment Gateway Abstraction.md
```

---

# Architecture Folder Naming

Top-level architecture sections use:

```text
NN Section Name/
```

Example:

```text
04 Shared Platform Services/
05 Development Standards/
06 Architecture Decision Records/
```

---

# Naming Governance

When a naming conflict appears:

1. Prefer the System Glossary.
2. Prefer established domain terminology.
3. Avoid introducing synonyms.
4. Update affected documentation.
5. Create an ADR if the naming decision has architectural significance.

---

# Naming Principle

> **Names are part of the architecture. Choose names that communicate ownership, purpose, and intent.**