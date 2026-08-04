# 04 Engineering Specifications

# APIs

# 03 — Restaurant APIs

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-API-003 |
| **Document Name** | Restaurant APIs |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | 01 REST API Specification<br>Database Engineering Specifications<br>Security Architecture |
| **Referenced By** | Restaurant Admin Dashboard<br>Branch Dashboard<br>Kitchen System<br>Order Management Services |

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

- Restaurant Admin Dashboard
- Branch Dashboard
- Kitchen Services
- Order Processing Services
- Inventory Services (Future)
- Mobile Applications

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

This document defines the REST APIs used by restaurant owners, restaurant administrators, branch administrators, managers, kitchen staff, and operational users.

These APIs manage restaurant operations including menus, categories, products, orders, reservations, customers, staff, branches, promotions, reports, and operational settings.

All REST standards, authentication, authorization, validation, response formatting, pagination, filtering, and error handling are inherited from **01 REST API Specification.md**.

---

# Scope

This specification defines:

- Restaurant operational APIs
- Branch operational APIs
- Menu management
- Product management
- Order management
- Reservation management
- Customer management
- Staff management
- Promotions
- Restaurant reporting

---

# Out of Scope

This specification does not define:

- Platform Administration APIs
- Customer Self-Service APIs
- Third-party Integration APIs
- Internal Service APIs
- Webhooks

These are documented separately.

---

# Restaurant API Philosophy

Restaurant APIs are tenant-owned operational APIs.

These APIs shall:

- Operate only within tenant boundaries.
- Enforce branch ownership.
- Support day-to-day restaurant operations.
- Produce audit records.
- Remain isolated from other tenants.

---

# Authorization Model

Restaurant APIs require:

- Authentication
- Authorization
- Tenant validation
- Branch validation
- Permission validation

Typical restaurant roles include:

- Restaurant Owner
- Restaurant Administrator
- Branch Administrator
- Manager
- Cashier
- Kitchen Staff
- Rider
- Customer Support

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

from **01 REST API Specification.md**

---

# Standard Endpoint Template

Every endpoint documented within this specification follows the standardized template defined by **01 REST API Specification.md**.

---

# Table of Contents

## Chapter 1 — Restaurant Profile APIs

## Chapter 2 — Branch Management APIs

## Chapter 3 — Menu Management APIs

## Chapter 4 — Category Management APIs

## Chapter 5 — Product Management APIs

## Chapter 6 — Modifier Management APIs

## Chapter 7 — Order Management APIs

## Chapter 8 — Reservation APIs

## Chapter 9 — Customer Management APIs

## Chapter 10 — Staff Management APIs

## Chapter 11 — Promotion APIs

## Chapter 12 — Restaurant Analytics APIs

## Chapter 13 — Restaurant Settings APIs

## Chapter 14 — Engineering Rules

## Chapter 15 — Architecture Decision Records

---

## Appendix A — Restaurant API Matrix

## Appendix B — Permission Matrix

## Appendix C — Endpoint Naming Examples

## Appendix D — Reserved Future APIs

---

## References

(To be completed in Response 2.)

---

## Revision History

(To be completed in Response 2.)

---

# Chapter 1 — Restaurant Profile APIs

## 1.1 Purpose

Restaurant Profile APIs manage restaurant information visible throughout the platform.

---

## Endpoint Group

```
/restaurant
```

---

# GET /restaurant

Returns the authenticated restaurant profile.

---

# PATCH /restaurant

Updates restaurant information.

---

## Business Rules

- Restaurant ownership validated.
- Changes are audited.
- Certain fields require verification.

---

# POST /restaurant/logo

Uploads restaurant logo.

---

# DELETE /restaurant/logo

Removes restaurant logo.

---

# POST /restaurant/banner

Uploads restaurant banner.

---

# DELETE /restaurant/banner

Removes restaurant banner.

---

# Chapter 2 — Branch Management APIs

## 2.1 Purpose

Branch APIs manage restaurant branches owned by the authenticated tenant.

---

## Endpoint Group

```
/branches
```

---

# GET /branches

Returns restaurant branches.

---

# POST /branches

Creates branch.

---

# GET /branches/{branchId}

Returns branch details.

---

# PATCH /branches/{branchId}

Updates branch.

---

# DELETE /branches/{branchId}

Archives branch.

---

# POST /branches/{branchId}/activate

Activates branch.

---

# POST /branches/{branchId}/deactivate

Temporarily closes branch.

---

## Business Rules

- Restaurant ownership validated.
- Branch codes remain unique.
- Branch administrators are notified of status changes.

---

# Chapter 3 — Menu Management APIs

## 3.1 Purpose

Menu APIs manage restaurant menus.

---

## Endpoint Group

```
/menus
```

---

# GET /menus

Returns menus.

---

# POST /menus

Creates menu.

---

# GET /menus/{menuId}

Returns menu details.

---

# PATCH /menus/{menuId}

Updates menu.

---

# DELETE /menus/{menuId}

Archives menu.

---

# POST /menus/{menuId}/publish

Publishes menu.

---

# POST /menus/{menuId}/unpublish

Unpublishes menu.

---

## Business Rules

- Published menus become customer visible.
- Draft menus remain internal.
- Menu publishing is audited.

---

# Chapter 4 — Category Management APIs

## 4.1 Purpose

Category APIs organize menu products.

---

## Endpoint Group

```
/categories
```

---

# GET /categories

Returns menu categories.

---

# POST /categories

Creates category.

---

# GET /categories/{categoryId}

Returns category.

---

# PATCH /categories/{categoryId}

Updates category.

---

# DELETE /categories/{categoryId}

Archives category.

---

# POST /categories/reorder

Reorders categories.

---

## Business Rules

- Category names must be unique within the same menu.
- Ordering affects customer menu display.

---

# Chapter 5 — Product Management APIs

## 5.1 Purpose

Product APIs manage food and beverage items.

---

## Endpoint Group

```
/products
```

---

# GET /products

Returns products.

Supports:

- Pagination
- Filtering
- Search
- Category filtering

---

# POST /products

Creates product.

---

# GET /products/{productId}

Returns product.

---

# PATCH /products/{productId}

Updates product.

---

# DELETE /products/{productId}

Archives product.

---

# POST /products/{productId}/activate

Activates product.

---

# POST /products/{productId}/deactivate

Makes product unavailable.

---

# POST /products/{productId}/duplicate

Duplicates product.

---

## Business Rules

- Product SKU shall remain unique.
- Product availability is branch-aware.
- Menu publication updates customer visibility.

---

# Chapter 6 — Modifier Management APIs

## 6.1 Purpose

Modifier Management APIs manage product modifiers, modifier groups, and customer customization options.

Modifiers allow restaurants to offer configurable products while maintaining pricing and inventory consistency.

---

## Endpoint Group

```
/modifiers
```

---

# GET /modifiers

Returns modifier groups.

---

# POST /modifiers

Creates modifier group.

---

# GET /modifiers/{modifierId}

Returns modifier group details.

---

# PATCH /modifiers/{modifierId}

Updates modifier group.

---

# DELETE /modifiers/{modifierId}

Archives modifier group.

---

# POST /modifiers/{modifierId}/options

Adds modifier option.

---

# PATCH /modifiers/options/{optionId}

Updates modifier option.

---

# DELETE /modifiers/options/{optionId}

Archives modifier option.

---

## Business Rules

- Modifier groups belong to a restaurant.
- Modifier options support additional pricing.
- Product assignments are validated.
- Changes affect future orders only.

---

# Chapter 7 — Order Management APIs

## 7.1 Purpose

Order Management APIs manage the complete restaurant order lifecycle from creation through fulfillment.

---

## Endpoint Group

```
/orders
```

---

# GET /orders

Returns restaurant orders.

Supports:

- Pagination
- Filtering
- Status filtering
- Branch filtering
- Customer filtering
- Date filtering

---

# GET /orders/{orderId}

Returns order details.

---

# PATCH /orders/{orderId}

Updates order information.

---

# POST /orders/{orderId}/accept

Accepts order.

---

# POST /orders/{orderId}/reject

Rejects order.

---

# POST /orders/{orderId}/prepare

Marks order as preparing.

---

# POST /orders/{orderId}/ready

Marks order as ready.

---

# POST /orders/{orderId}/dispatch

Assigns order for delivery or pickup.

---

# POST /orders/{orderId}/complete

Completes order.

---

# POST /orders/{orderId}/cancel

Cancels order.

---

## Business Rules

- Order state transitions follow the Order Lifecycle.
- Completed orders cannot be modified.
- Status changes generate audit records.
- Customer notifications are triggered automatically.

---

# Chapter 8 — Reservation APIs

## 8.1 Purpose

Reservation APIs manage restaurant reservations and table assignments.

---

## Endpoint Group

```
/reservations
```

---

# GET /reservations

Returns reservations.

---

# POST /reservations

Creates reservation.

---

# GET /reservations/{reservationId}

Returns reservation details.

---

# PATCH /reservations/{reservationId}

Updates reservation.

---

# POST /reservations/{reservationId}/confirm

Confirms reservation.

---

# POST /reservations/{reservationId}/seat

Marks reservation as seated.

---

# POST /reservations/{reservationId}/complete

Completes reservation.

---

# POST /reservations/{reservationId}/cancel

Cancels reservation.

---

## Business Rules

- Reservation availability is validated.
- Table conflicts are prevented.
- Reservation lifecycle follows Reservation Architecture.

---

# Chapter 9 — Customer Management APIs

## 9.1 Purpose

Customer Management APIs provide restaurant administrators with access to customer information relevant to their restaurant.

---

## Endpoint Group

```
/customers
```

---

# GET /customers

Returns restaurant customers.

---

# GET /customers/{customerId}

Returns customer profile.

---

# GET /customers/{customerId}/orders

Returns customer order history.

---

# GET /customers/{customerId}/reservations

Returns reservation history.

---

# GET /customers/{customerId}/loyalty

Returns loyalty information.

---

# PATCH /customers/{customerId}/notes

Updates internal customer notes.

---

## Business Rules

- Restaurants may only access their own customers.
- Customer personal data follows Privacy Policy.
- Internal notes remain invisible to customers.

---

# Chapter 10 — Staff Management APIs

## 10.1 Purpose

Staff Management APIs manage restaurant staff and operational roles.

---

## Endpoint Group

```
/staff
```

---

# GET /staff

Returns restaurant staff.

---

# POST /staff

Creates staff member.

---

# GET /staff/{staffId}

Returns staff details.

---

# PATCH /staff/{staffId}

Updates staff information.

---

# DELETE /staff/{staffId}

Archives staff member.

---

# POST /staff/{staffId}/activate

Activates staff account.

---

# POST /staff/{staffId}/deactivate

Deactivates staff account.

---

# POST /staff/{staffId}/assign-role

Assigns operational role.

---

## Business Rules

- Staff belong to one tenant.
- Branch assignments are validated.
- Role changes generate audit records.

---

# Chapter 11 — Promotion APIs

## 11.1 Purpose

Promotion APIs manage offers, discounts, promotional campaigns, and coupons.

---

## Endpoint Group

```
/promotions
```

---

# GET /promotions

Returns promotions.

---

# POST /promotions

Creates promotion.

---

# GET /promotions/{promotionId}

Returns promotion details.

---

# PATCH /promotions/{promotionId}

Updates promotion.

---

# DELETE /promotions/{promotionId}

Archives promotion.

---

# POST /promotions/{promotionId}/activate

Activates promotion.

---

# POST /promotions/{promotionId}/deactivate

Deactivates promotion.

---

## Business Rules

- Promotion validity dates are validated.
- Promotions may be limited by branch.
- Expired promotions cannot be activated.

---

# Chapter 12 — Restaurant Analytics APIs

## 12.1 Purpose

Restaurant Analytics APIs provide operational reporting and business intelligence for restaurant owners and managers.

---

## Endpoint Group

```
/analytics
```

---

# GET /analytics/dashboard

Returns restaurant dashboard metrics.

---

# GET /analytics/orders

Returns order analytics.

---

# GET /analytics/revenue

Returns revenue reports.

---

# GET /analytics/customers

Returns customer analytics.

---

# GET /analytics/products

Returns product performance.

---

# GET /analytics/branches

Returns branch performance.

---

# GET /analytics/export

Exports restaurant reports.

Supported formats:

- CSV
- XLSX
- PDF

---

## Business Rules

- Analytics are tenant isolated.
- Reports support configurable date ranges.
- Export operations generate audit records.

---

# Chapter 13 — Restaurant Settings APIs

## 13.1 Purpose

Restaurant Settings APIs manage operational settings and restaurant configuration.

---

## Endpoint Group

```
/settings
```

---

# GET /settings

Returns restaurant settings.

---

# PATCH /settings

Updates restaurant settings.

---

# GET /settings/hours

Returns business hours.

---

# PATCH /settings/hours

Updates operating hours.

---

# GET /settings/delivery

Returns delivery configuration.

---

# PATCH /settings/delivery

Updates delivery configuration.

---

# GET /settings/taxes

Returns tax configuration.

---

# PATCH /settings/taxes

Updates tax configuration.

---

# GET /settings/payments

Returns payment configuration.

---

# PATCH /settings/payments

Updates payment configuration.

---

## Business Rules

- Critical configuration changes are audited.
- Business hour updates affect order availability.
- Payment configuration follows Payment Architecture.

---

# Chapter 14 — Engineering Rules

## 14.1 Purpose

Engineering Rules define the mandatory standards governing Restaurant APIs.

---

## Rule RAPI-001

Every Restaurant API shall comply with **01 REST API Specification.md**.

---

## Rule RAPI-002

Every endpoint shall enforce tenant ownership validation.

---

## Rule RAPI-003

Branch-scoped operations shall validate branch ownership.

---

## Rule RAPI-004

Restaurant APIs shall never expose resources belonging to another tenant.

---

## Rule RAPI-005

Business validation shall execute before persistence.

---

## Rule RAPI-006

Every data modification shall generate an audit record.

---

## Rule RAPI-007

Order and reservation lifecycle transitions shall follow the documented business lifecycle specifications.

---

## Rule RAPI-008

Breaking API changes require API versioning.

---

## Rule RAPI-009

Endpoint behavior shall remain independent of database implementation.

---

## Rule RAPI-010

This document is the authoritative Restaurant API specification for the FluxDine platform.

---

# Chapter 15 — Architecture Decision Records

## ADR-RAPI-001

Restaurant APIs are tenant-owned operational APIs.

---

## ADR-RAPI-002

Restaurant APIs inherit all engineering standards defined in **01 REST API Specification.md**.

---

## ADR-RAPI-003

Tenant isolation shall be enforced for every endpoint.

---

## ADR-RAPI-004

Operational events shall generate audit records.

---

## ADR-RAPI-005

Business lifecycle transitions shall be enforced by the application.

---

## ADR-RAPI-006

Administrative and operational APIs remain separated.

---

## ADR-RAPI-007

Restaurant APIs shall remain backward compatible within supported API versions.

---

## ADR-RAPI-008

Endpoint contracts shall remain independent of implementation technology.

---

## ADR-RAPI-009

All endpoint groups shall follow the standardized endpoint template.

---

## ADR-RAPI-010

This document is the authoritative Restaurant API specification for the FluxDine platform.

---

# Appendix A — Restaurant API Matrix

| Domain | Endpoint Group | CRUD | Operational Actions |
|---------|----------------|:----:|:-------------------:|
| Restaurant | /restaurant | Read/Update | Logo, Banner |
| Branch | /branches | ✓ | Activate, Deactivate |
| Menu | /menus | ✓ | Publish |
| Category | /categories | ✓ | Reorder |
| Product | /products | ✓ | Duplicate, Activate |
| Modifier | /modifiers | ✓ | Manage Options |
| Order | /orders | Read/Update | Lifecycle |
| Reservation | /reservations | ✓ | Confirm, Seat |
| Customer | /customers | Read | Notes |
| Staff | /staff | ✓ | Role Assignment |
| Promotion | /promotions | ✓ | Activate |
| Analytics | /analytics | Read | Export |
| Settings | /settings | Read/Update | Configuration |

---

# Appendix B — Permission Matrix

| Module | Read | Create | Update | Delete | Special Actions |
|---------|:----:|:------:|:------:|:------:|:---------------:|
| Restaurant | ✓ | — | ✓ | — | Branding |
| Branch | ✓ | ✓ | ✓ | Archive | Activate |
| Menu | ✓ | ✓ | ✓ | Archive | Publish |
| Category | ✓ | ✓ | ✓ | Archive | Reorder |
| Product | ✓ | ✓ | ✓ | Archive | Duplicate |
| Modifier | ✓ | ✓ | ✓ | Archive | Options |
| Order | ✓ | — | ✓ | Cancel | Accept, Complete |
| Reservation | ✓ | ✓ | ✓ | Cancel | Confirm |
| Customer | ✓ | — | Notes | — | History |
| Staff | ✓ | ✓ | ✓ | Archive | Assign Role |
| Promotion | ✓ | ✓ | ✓ | Archive | Activate |
| Analytics | ✓ | — | — | — | Export |
| Settings | ✓ | — | ✓ | — | Hours, Delivery |

---

# Appendix C — Endpoint Naming Examples

```text
GET    /menus

POST   /menus

GET    /products/{productId}

PATCH  /products/{productId}

POST   /orders/{orderId}/accept

POST   /orders/{orderId}/ready

POST   /reservations/{reservationId}/confirm

GET    /analytics/dashboard

PATCH  /settings/hours
```

---

# Appendix D — Reserved Future APIs

Future Restaurant API groups may include:

## Inventory

```
/inventory
```

---

## Suppliers

```
/suppliers
```

---

## Kitchen Display System

```
/kitchen
```

---

## Loyalty

```
/loyalty
```

---

## Workforce

```
/workforce
```

---

## Fleet

```
/fleet
```

---

Future endpoint groups shall follow the engineering standards defined by this specification.

---

# References

This specification shall be read together with:

## API Engineering

- 01 REST API Specification
- 02 HQ APIs
- 04 Self-Service APIs
- 05 Shared Service APIs
- 06 Integration APIs
- 07 Webhook Specification
- 08 Error Code Catalog
- 09 API Versioning

---

## Architecture

- System Architecture Blueprint
- Security Architecture
- Identity & Authorization Architecture
- Database Engineering Specifications

---

## Governance

- Architecture Principles
- Documentation Standards
- Architecture Decision Record (ADR) Register

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 0.1 | Initial Draft | FluxDine Engineering | Document structure established |
| 0.5 | Restaurant Operations | FluxDine Engineering | Restaurant, branch, menu, category, product, modifier, order, reservation, customer, staff, and promotion APIs completed |
| 0.8 | Reporting & Configuration | FluxDine Engineering | Analytics, settings, engineering rules, ADRs, appendices, and governance completed |
| 1.0 | Final Release | FluxDine Engineering | Approved as the authoritative Restaurant API specification |