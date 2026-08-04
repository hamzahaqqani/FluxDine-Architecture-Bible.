````md id="fd-sps-005"
# 04 Shared Platform Services

# 05 — Commerce Service

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-SPS-005 |
| **Document Name** | Commerce Service |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Platform Architecture Team |
| **Classification** | Core Platform Service |
| **Depends On** | Shared Services Overview |
| **Referenced By** | Restaurant Platform<br>Customer Platform<br>HQ Platform |

---

# Purpose

The Commerce Service provides centralized commerce orchestration across the entire FluxDine platform.

It is the single authoritative owner of:

- Order Lifecycle
- Shopping Cart
- Checkout
- Pricing Calculations
- Taxes
- Discounts
- Promotions
- Service Charges
- Delivery Charges
- Order Totals

No other service shall implement commerce logic independently.

---

# Responsibilities

The Commerce Service owns:

- Shopping Cart
- Checkout
- Order Creation
- Order Validation
- Pricing Engine
- Discount Engine
- Promotion Engine
- Coupon Validation
- Tax Calculation
- Delivery Fee Calculation
- Service Charge Calculation
- Order Total Calculation

---

# Out of Scope

The Commerce Service does **not** own:

- Authentication
- Tenant Lifecycle
- Restaurant Registry
- Payment Processing
- Billing
- Menu Management
- Inventory
- Notifications
- Analytics

---

# Service Boundaries

The Commerce Service owns:

- Commerce Database
- Commerce APIs
- Commerce Events
- Commerce Business Rules
- Pricing Engine

Other services consume published APIs only.

---

# Primary Consumers

The Commerce Service is consumed by:

- Restaurant Platform
- Customer Platform
- HQ Platform
- Payment Service
- Notification Service
- Analytics Service
- Search Service

---

# Public APIs

Typical APIs include:

- Create Cart
- Update Cart
- Calculate Pricing
- Validate Coupon
- Apply Promotion
- Calculate Taxes
- Calculate Delivery Fee
- Checkout
- Create Order
- Cancel Order

APIs shall be versioned and documented.

---

# Published Events

The Commerce Service publishes events including:

```text
CartCreated

CartUpdated

CheckoutStarted

OrderCreated

OrderConfirmed

OrderCancelled

CouponApplied

PromotionApplied

PricingCalculated
```

---

# Consumed Events

The Commerce Service consumes events including:

```text
RestaurantActivated

PaymentSucceeded

PaymentFailed

MenuUpdated

SubscriptionActivated
```

---

# Data Ownership

The Commerce Service exclusively owns:

- Shopping Carts
- Checkout Sessions
- Pricing Rules
- Discounts
- Promotions
- Coupons
- Tax Calculations
- Delivery Charges
- Service Charges
- Order Totals

No other service may modify commerce data directly.

---

# Security

The Commerce Service shall enforce:

- Tenant Isolation
- Restaurant Ownership Validation
- Customer Authorization
- Secure Checkout
- Pricing Integrity
- Complete Audit Logging

Every commerce operation shall validate authorization before execution.

---

# Scalability

The Commerce Service shall support:

- Millions of Shopping Carts
- Millions of Orders
- High Checkout Volume
- Global Restaurant Operations
- Horizontal Scaling
- High Availability

---

# Engineering Rules

- The Commerce Service is the single source of truth for commerce workflows.
- Pricing calculations shall execute exclusively within the Commerce Service.
- Discounts, promotions, taxes, and service charges shall never be calculated by client applications.
- The Commerce Service shall never execute payment transactions.
- Payment authorization belongs exclusively to the Payment Service.
- Commerce data shall never be modified through another service's database.
- Commerce lifecycle changes shall publish domain events.
- Every commerce operation shall generate an audit record.
- Commerce APIs shall remain backward compatible.
- Commerce operations shall be idempotent where applicable.
- This document is the authoritative Commerce Service specification.

---

# Architecture Decision Records

- Commerce orchestration is centralized into a dedicated platform service.
- Shopping cart ownership belongs exclusively to the Commerce Service.
- Pricing calculations remain independent from payment execution.
- Discounts and promotions are evaluated within the Commerce Service.
- Order creation precedes payment authorization.
- Commerce events are published through the shared Event Bus.
- Commerce data follows the Database-per-Service architecture.
- Future pricing strategies shall extend the Pricing Engine without changing service ownership.
- Payment execution remains delegated to the Payment Service.
- This document is the authoritative Commerce Service specification.

---

# Quality Attributes

| Attribute | Objective |
|-----------|-----------|
| Reliability | Consistent commerce execution |
| Availability | High commerce service uptime |
| Scalability | Millions of concurrent orders |
| Security | Protected pricing and checkout |
| Performance | Low-latency pricing calculations |
| Auditability | Complete commerce traceability |
| Extensibility | Support future commerce models |
| Maintainability | Independent service evolution |

---

# References

- Shared Services Overview
- Restaurant Service
- Payment Service
- Billing Service
- Restaurant Platform Architecture
- Event Catalog
- REST API Specification
- Monitoring Specification

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | Initial Release | FluxDine Platform Architecture Team | Approved as the authoritative Commerce Service specification |
````
