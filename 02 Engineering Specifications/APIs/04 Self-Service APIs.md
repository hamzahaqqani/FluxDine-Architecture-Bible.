# 04 Engineering Specifications

# APIs

# 04 — Self-Service APIs

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-API-004 |
| **Document Name** | Self-Service APIs |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | 01 REST API Specification<br>Database Engineering Specifications<br>Security Architecture |
| **Referenced By** | Customer Web Application<br>Customer Mobile Applications<br>Restaurant Ordering Platform |

---

# Dependencies

This specification depends upon:

- 01 REST API Specification
- Database Engineering Specifications
- Security Architecture
- Identity & Authorization Architecture

All endpoint definitions inherit the engineering standards defined within **01 REST API Specification.md**.

---

# Referenced By

This specification is referenced by:

- Customer Web Application
- Customer Mobile Applications
- Customer Portal
- Online Ordering Platform
- Order Tracking Services

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

This document defines every customer-facing REST API used by the FluxDine Self-Service platform.

These APIs allow customers to browse restaurants, view menus, place orders, make reservations, manage profiles, track deliveries, submit reviews, redeem loyalty rewards, and manage their personal accounts.

All REST standards, authentication, authorization, validation, response formatting, pagination, filtering, sorting, and error handling are inherited from **01 REST API Specification.md**.

---

# Scope

This specification defines:

- Customer authentication
- Customer profile management
- Restaurant discovery
- Menu browsing
- Shopping cart
- Checkout
- Orders
- Reservations
- Favorites
- Reviews
- Loyalty
- Notifications
- Customer settings

---

# Out of Scope

This specification does not define:

- Headquarters APIs
- Restaurant Administration APIs
- Integration APIs
- Internal Service APIs
- Webhooks

These topics are documented separately.

---

# Self-Service API Philosophy

Self-Service APIs provide secure, intuitive, customer-centric access to restaurant services.

These APIs shall:

- Operate only on customer-owned resources.
- Respect tenant isolation.
- Protect customer privacy.
- Deliver consistent user experiences.
- Support mobile and web applications.
- Generate audit records where appropriate.

---

# Authorization Model

Self-Service APIs require:

- Authentication where applicable.
- Customer ownership validation.
- Tenant validation.
- Resource authorization.

Public APIs remain accessible without authentication where explicitly documented.

---

# Endpoint Standards

All endpoint behavior inherits:

- URI Standards
- Request Standards
- Response Standards
- Validation
- Pagination
- Filtering
- Sorting
- Versioning
- Error Handling

from **01 REST API Specification.md**.

---

# Standard Endpoint Template

Every endpoint documented within this specification follows the standardized template defined by **01 REST API Specification.md**.

---

# Table of Contents

## Chapter 1 — Authentication APIs

## Chapter 2 — Customer Profile APIs

## Chapter 3 — Restaurant Discovery APIs

## Chapter 4 — Menu Browsing APIs

## Chapter 5 — Shopping Cart APIs

## Chapter 6 — Checkout APIs

## Chapter 7 — Order APIs

## Chapter 8 — Reservation APIs

## Chapter 9 — Favorites APIs

## Chapter 10 — Review APIs

## Chapter 11 — Loyalty APIs

## Chapter 12 — Notification APIs

## Chapter 13 — Customer Settings APIs

## Chapter 14 — Engineering Rules

## Chapter 15 — Architecture Decision Records

---

## Appendix A — Self-Service API Matrix

## Appendix B — Permission Matrix

## Appendix C — Endpoint Naming Examples

## Appendix D — Reserved Future APIs

---

## References

---

## Revision History

---

# Chapter 1 — Authentication APIs

## Endpoint Group

```
/auth
```

### Endpoints

```
POST   /auth/register
POST   /auth/login
POST   /auth/logout
POST   /auth/refresh
POST   /auth/forgot-password
POST   /auth/reset-password
POST   /auth/verify-email
POST   /auth/resend-verification
POST   /auth/change-password
```

### Business Rules

- Email verification required.
- Password policy enforced.
- Refresh tokens securely managed.
- Failed login attempts monitored.

---

# Chapter 2 — Customer Profile APIs

## Endpoint Group

```
/profile
```

### Endpoints

```
GET    /profile
PATCH  /profile
DELETE /profile
POST   /profile/avatar
DELETE /profile/avatar

GET    /profile/addresses
POST   /profile/addresses
PATCH  /profile/addresses/{addressId}
DELETE /profile/addresses/{addressId}
```

### Business Rules

- Customers access only their own profile.
- Address ownership validated.
- Profile updates are audited.

---

# Chapter 3 — Restaurant Discovery APIs

## Endpoint Group

```
/restaurants
```

### Endpoints

```
GET /restaurants

GET /restaurants/{restaurantId}

GET /restaurants/featured

GET /restaurants/nearby

GET /restaurants/search

GET /restaurants/categories

GET /restaurants/{restaurantId}/branches
```

### Business Rules

- Only published restaurants returned.
- Supports filtering.
- Supports search.
- Supports pagination.

---

# Chapter 4 — Menu Browsing APIs

## Endpoint Group

```
/menus
```

### Endpoints

```
GET /menus/{menuId}

GET /menus/{menuId}/categories

GET /products

GET /products/{productId}

GET /products/search
```

### Business Rules

- Only published menus visible.
- Product availability respects branch configuration.
- Hidden products excluded.

---

# Chapter 5 — Shopping Cart APIs

## Endpoint Group

```
/cart
```

### Endpoints

```
GET    /cart

POST   /cart/items

PATCH  /cart/items/{itemId}

DELETE /cart/items/{itemId}

DELETE /cart

POST   /cart/apply-coupon

DELETE /cart/coupon
```

### Business Rules

- Cart belongs to authenticated customer.
- Product availability validated.
- Coupon eligibility verified.
- Price recalculated after every modification.

---

# Chapter 6 — Checkout APIs

## Endpoint Group

```
/checkout
```

### Endpoints

```
POST /checkout

POST /checkout/validate

POST /checkout/payment

POST /checkout/confirm
```

### Business Rules

- Cart validation required.
- Payment authorization required.
- Inventory validation performed where applicable.
- Successful checkout creates order.

---

# Chapter 7 — Order APIs

## Endpoint Group

```
/orders
```

### Endpoints

```
GET /orders

GET /orders/{orderId}

POST /orders/{orderId}/cancel

GET /orders/{orderId}/tracking

GET /orders/history
```

### Business Rules

- Customers access only their own orders.
- Cancellation follows Order Policy.
- Tracking reflects live order status.

---

# Chapter 8 — Reservation APIs

## Endpoint Group

```
/reservations
```

### Endpoints

```
GET /reservations

POST /reservations

GET /reservations/{reservationId}

PATCH /reservations/{reservationId}

POST /reservations/{reservationId}/cancel
```

### Business Rules

- Reservation availability validated.
- Restaurant operating hours respected.
- Customers manage only their own reservations.

---

# Chapter 9 — Favorites APIs

## Endpoint Group

```
/favorites
```

### Endpoints

```
GET /favorites

POST /favorites/restaurants

DELETE /favorites/restaurants/{restaurantId}

POST /favorites/products

DELETE /favorites/products/{productId}
```

### Business Rules

- Favorites belong to customer.
- Duplicate favorites prohibited.

---

# Chapter 10 — Review APIs

## Endpoint Group

```
/reviews
```

### Endpoints

```
GET /reviews

POST /reviews

PATCH /reviews/{reviewId}

DELETE /reviews/{reviewId}
```

### Business Rules

- Reviews require completed orders.
- Customers edit only their own reviews.
- Moderation policies apply.

---

# Chapter 11 — Loyalty APIs

## Endpoint Group

```
/loyalty
```

### Endpoints

```
GET /loyalty

GET /loyalty/history

POST /loyalty/redeem

GET /loyalty/rewards
```

### Business Rules

- Loyalty belongs to customer.
- Reward eligibility validated.
- Redemption transactions audited.

---

# Chapter 12 — Notification APIs

## Endpoint Group

```
/notifications
```

### Endpoints

```
GET /notifications

PATCH /notifications/{notificationId}/read

PATCH /notifications/read-all

DELETE /notifications/{notificationId}
```

### Business Rules

- Customers access only their notifications.
- Read status synchronized across devices.

---

# Chapter 13 — Customer Settings APIs

## Endpoint Group

```
/settings
```

### Endpoints

```
GET /settings

PATCH /settings

PATCH /settings/preferences

PATCH /settings/privacy

PATCH /settings/notifications

PATCH /settings/language
```

### Business Rules

- Settings belong to authenticated customer.
- Privacy preferences immediately enforced.

---

# Chapter 14 — Engineering Rules

## Rule SSAPI-001

Every endpoint shall comply with **01 REST API Specification.md**.

---

## Rule SSAPI-002

Customers shall only access resources they own unless an endpoint is explicitly public.

---

## Rule SSAPI-003

Tenant isolation shall be enforced throughout every request.

---

## Rule SSAPI-004

Public endpoints shall expose only published resources.

---

## Rule SSAPI-005

Business validation shall execute before persistence.

---

## Rule SSAPI-006

Every order and reservation operation shall follow the documented lifecycle specifications.

---

## Rule SSAPI-007

Breaking endpoint changes require API versioning.

---

## Rule SSAPI-008

Endpoint contracts shall remain independent of database implementation.

---

## Rule SSAPI-009

Customer privacy shall be preserved throughout every operation.

---

## Rule SSAPI-010

This document is the authoritative Self-Service API specification for the FluxDine platform.

---

# Chapter 15 — Architecture Decision Records

## ADR-SSAPI-001

Self-Service APIs are customer-facing APIs.

---

## ADR-SSAPI-002

Self-Service APIs inherit all engineering standards from **01 REST API Specification.md**.

---

## ADR-SSAPI-003

Customers may only access resources they own.

---

## ADR-SSAPI-004

Public resources shall expose only published data.

---

## ADR-SSAPI-005

Operational lifecycle rules shall be enforced by the application.

---

## ADR-SSAPI-006

Customer data privacy is mandatory.

---

## ADR-SSAPI-007

Endpoint contracts remain implementation independent.

---

## ADR-SSAPI-008

Customer APIs remain backward compatible within supported API versions.

---

## ADR-SSAPI-009

Every endpoint group follows the standardized endpoint template.

---

## ADR-SSAPI-010

This document is the authoritative Self-Service API specification for the FluxDine platform.

---

# Appendix A — Self-Service API Matrix

| Domain | Endpoint Group | CRUD | Special Actions |
|---------|----------------|:----:|:---------------:|
| Authentication | /auth | ✓ | Login, Verify |
| Profile | /profile | ✓ | Avatar |
| Restaurants | /restaurants | Read | Search |
| Menus | /menus | Read | Browse |
| Cart | /cart | ✓ | Coupon |
| Checkout | /checkout | Create | Payment |
| Orders | /orders | Read | Tracking |
| Reservations | /reservations | ✓ | Cancel |
| Favorites | /favorites | ✓ | Save |
| Reviews | /reviews | ✓ | Moderate |
| Loyalty | /loyalty | Read | Redeem |
| Notifications | /notifications | Read/Update | Read |
| Settings | /settings | Read/Update | Preferences |

---

# Appendix B — Permission Matrix

| Module | Public | Authenticated | Owner Required |
|---------|:------:|:-------------:|:--------------:|
| Authentication | ✓ | — | — |
| Restaurants | ✓ | — | — |
| Menus | ✓ | — | — |
| Cart | — | ✓ | ✓ |
| Checkout | — | ✓ | ✓ |
| Orders | — | ✓ | ✓ |
| Reservations | — | ✓ | ✓ |
| Profile | — | ✓ | ✓ |
| Reviews | — | ✓ | ✓ |
| Loyalty | — | ✓ | ✓ |
| Notifications | — | ✓ | ✓ |
| Settings | — | ✓ | ✓ |

---

# Appendix C — Endpoint Naming Examples

```text
POST   /auth/login

GET    /restaurants

GET    /products/{productId}

POST   /cart/items

POST   /checkout

GET    /orders/{orderId}/tracking

POST   /reservations

PATCH  /settings/preferences
```

---

# Appendix D — Reserved Future APIs

Future Self-Service API groups may include:

```
/wallet

/gift-cards

/subscriptions

/chat

/referrals

/ai-assistant
```

---

# References

- 01 REST API Specification
- 02 HQ APIs
- 03 Restaurant APIs
- 05 Shared Service APIs
- 06 Integration APIs
- 07 Webhook Specification
- 08 Error Code Catalog
- 09 API Versioning

- System Architecture Blueprint
- Security Architecture
- Database Engineering Specifications

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | Initial Release | FluxDine Engineering | Approved as the authoritative Self-Service API specification |