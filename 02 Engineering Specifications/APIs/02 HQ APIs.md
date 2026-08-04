# 04 Engineering Specifications

# APIs

# 02 — HQ APIs

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-API-002 |
| **Document Name** | HQ APIs |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | 01 REST API Specification<br>Database Engineering Specifications<br>Security Architecture |
| **Referenced By** | Backend Engineering Specifications<br>API Implementation<br>Frontend Applications |

---

# Dependencies

This specification depends upon:

- 01 REST API Specification
- Database Engineering Specifications
- Security Architecture
- Identity & Access Management Architecture

All endpoint definitions contained within this document inherit the standards defined by **01 REST API Specification.md**.

---

# Referenced By

This specification is referenced by:

- Backend API Implementation
- HQ Web Dashboard
- API Test Suites
- SDK Generation
- OpenAPI Specification
- QA Test Automation

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

This document defines every REST API exposed by the FluxDine Headquarters (HQ) platform.

These APIs are used by platform administrators to manage the entire SaaS platform including tenants, subscriptions, restaurants, users, features, analytics, branding, billing, and platform configuration.

This document defines endpoint contracts only.

REST standards, authentication behavior, validation rules, pagination, response structures, versioning, and error handling are inherited from **01 REST API Specification.md**.

---

# Scope

This specification defines:

- HQ administrative APIs
- Platform management endpoints
- Identity administration APIs
- Tenant administration APIs
- Subscription administration APIs
- Restaurant administration APIs
- Administrative permissions
- Request contracts
- Response contracts
- Business rules

---

# Out of Scope

This specification does not define:

- Restaurant operational APIs
- Customer Self-Service APIs
- Integration APIs
- Webhooks
- Internal service communication

Those topics are documented separately.

---

# HQ API Philosophy

HQ APIs are designed exclusively for platform administration.

These APIs shall:

- Manage the entire SaaS platform.
- Operate across all tenants.
- Enforce platform security.
- Support administrative workflows.
- Preserve tenant isolation.
- Produce complete audit records.

---

# Authorization Model

Every HQ endpoint requires:

- Authentication
- Authorization
- Permission validation
- Audit logging

Authorization follows Role-Based Access Control (RBAC).

Typical HQ roles include:

- Platform Super Administrator
- Platform Administrator
- Support Administrator
- Finance Administrator
- Operations Administrator
- Read-Only Administrator

Permissions are defined within the Identity & Authorization specifications.

---

# Endpoint Standards

All endpoints defined within this document inherit:

- URI standards
- Request standards
- Response standards
- Pagination
- Filtering
- Sorting
- Validation
- Error handling
- Rate limiting
- Versioning

from:

**01 REST API Specification.md**

---

# Standard Endpoint Template

Every endpoint documented within this specification follows the template below.

---

## Endpoint

Endpoint URI.

---

## Purpose

Business objective.

---

## HTTP Method

GET

POST

PUT

PATCH

DELETE

---

## Authentication

Required / Public

---

## Authorization

Required role(s).

---

## Required Permissions

Platform permission(s) required.

---

## Path Parameters

Documented path variables.

---

## Query Parameters

Supported filters.

---

## Request Body

Required request payload.

---

## Validation Rules

Business validation performed.

---

## Business Rules

Business constraints.

---

## Success Response

Successful response structure.

---

## Success Status Codes

Expected HTTP success codes.

---

## Error Codes

Possible errors.

---

## Related Entities

Business entities involved.

---

## Engineering Notes

Implementation guidance.

---

# Table of Contents

## Chapter 1 — Platform Administration APIs

## Chapter 2 — Identity Management APIs

## Chapter 3 — Tenant Management APIs

## Chapter 4 — Subscription Management APIs

## Chapter 5 — Restaurant Management APIs

## Chapter 6 — Branch Management APIs

## Chapter 7 — User Management APIs

## Chapter 8 — Role & Permission APIs

## Chapter 9 — Feature Management APIs

## Chapter 10 — Branding Management APIs

## Chapter 11 — Analytics APIs

## Chapter 12 — Billing APIs

## Chapter 13 — Audit APIs

## Chapter 14 — System Configuration APIs

## Chapter 15 — Engineering Rules

## Chapter 16 — Architecture Decision Records

---

## Appendix A — HQ API Matrix

## Appendix B — Permission Matrix

## Appendix C — Endpoint Naming Examples

## Appendix D — Reserved Future APIs

---

## References

(To be completed in Response 3.)

---

## Revision History

(To be completed in Response 3.)

---

# Chapter 1 — Platform Administration APIs

## 1.1 Purpose

Platform Administration APIs manage global platform resources that are not owned by any tenant.

These endpoints are accessible only to authorized HQ administrators.

---

## Endpoint Group

```
/platform
```

---

# GET /platform

## Purpose

Returns platform information and operational status.

---

## Authentication

Required

---

## Authorization

Platform Administrator

---

## Required Permission

platform.read

---

## Query Parameters

None

---

## Success Response

```json
{
  "success": true,
  "data": {
    "platform": {}
  }
}
```

---

## Business Rules

- Returns global platform metadata.
- Tenant-specific information is excluded.

---

## Related Entities

- Platform

---

# PATCH /platform

## Purpose

Updates global platform configuration.

---

## Authentication

Required

---

## Authorization

Platform Super Administrator

---

## Required Permission

platform.update

---

## Request Body

Platform configuration object.

---

## Business Rules

- Configuration changes are audited.
- Critical settings require elevated permissions.

---

## Related Entities

- Platform
- Audit Log

---

# GET /platform/health

## Purpose

Returns operational health information for the platform.

---

## Required Permission

platform.health.read

---

## Business Rules

- Read-only endpoint.
- Intended for administrative dashboards.

---

# GET /platform/statistics

## Purpose

Returns high-level platform statistics.

---

## Required Permission

platform.analytics.read

---

## Business Rules

Statistics include:

- Total tenants
- Total restaurants
- Total users
- Total orders
- Active subscriptions

---

# Chapter 2 — Identity Management APIs

## 2.1 Purpose

Identity Management APIs manage platform identities used by HQ administrators.

These endpoints do not manage restaurant users or customers.

---

## Endpoint Group

```
/identity
```

---

# GET /identity/users

## Purpose

Returns HQ administrative users.

---

## Authentication

Required

---

## Required Permission

identity.read

---

## Query Parameters

- page
- pageSize
- status
- search
- role

---

## Business Rules

- Supports pagination.
- Supports filtering.
- Soft-deleted users excluded.

---

## Related Entities

- User
- Role

---

# POST /identity/users

## Purpose

Creates a new HQ administrator.

---

## Required Permission

identity.create

---

## Request Body

Administrative user information.

---

## Business Rules

- Email must be unique.
- Password policy enforced.
- Role assignment validated.

---

# GET /identity/users/{userId}

## Purpose

Returns details of a specific HQ administrator.

---

## Required Permission

identity.read

---

# PATCH /identity/users/{userId}

## Purpose

Updates administrator information.

---

## Required Permission

identity.update

---

# DELETE /identity/users/{userId}

## Purpose

Soft deletes an administrator.

---

## Required Permission

identity.delete

---

## Business Rules

- Super Administrators cannot be deleted.
- Operation is audited.

---

# Chapter 3 — Tenant Management APIs

## 3.1 Purpose

Tenant Management APIs manage SaaS tenants throughout the platform lifecycle.

---

## Endpoint Group

```
/tenants
```

---

# GET /tenants

## Purpose

Returns all tenants.

---

## Required Permission

tenant.read

---

## Query Parameters

- page
- pageSize
- status
- subscription
- search

---

## Business Rules

- Paginated.
- Filterable.
- Ordered by creation date by default.

---

## Related Entities

- Tenant
- Subscription

---

# POST /tenants

## Purpose

Creates a new tenant.

---

## Required Permission

tenant.create

---

## Business Rules

- Tenant slug must be unique.
- Default configuration initialized.
- Subscription assigned.

---

# GET /tenants/{tenantId}

Returns tenant details.

---

# PATCH /tenants/{tenantId}

Updates tenant information.

---

# DELETE /tenants/{tenantId}

Archives tenant.

Deletion is logical rather than physical.

---

# POST /tenants/{tenantId}/suspend

Suspends tenant access.

---

# POST /tenants/{tenantId}/activate

Reactivates suspended tenant.

---

# POST /tenants/{tenantId}/archive

Archives tenant.

---

# Chapter 4 — Subscription Management APIs

## 4.1 Purpose

Subscription APIs manage platform subscriptions assigned to tenants.

---

## Endpoint Group

```
/subscriptions
```

---

# GET /subscriptions

Returns subscription catalog.

---

# GET /subscriptions/{subscriptionId}

Returns subscription details.

---

# POST /subscriptions

Creates subscription.

---

# PATCH /subscriptions/{subscriptionId}

Updates subscription.

---

# DELETE /subscriptions/{subscriptionId}

Archives subscription.

---

# POST /subscriptions/{subscriptionId}/assign

Assigns subscription to tenant.

---

# POST /subscriptions/{subscriptionId}/cancel

Cancels subscription.

---

# POST /subscriptions/{subscriptionId}/renew

Renews subscription.

---

# Chapter 5 — Restaurant Management APIs

## 5.1 Purpose

Restaurant APIs allow HQ administrators to manage restaurants across all tenants.

---

## Endpoint Group

```
/restaurants
```

---

# GET /restaurants

Returns all restaurants.

Supports:

- Pagination
- Filtering
- Sorting
- Search

---

# POST /restaurants

Creates restaurant.

---

# GET /restaurants/{restaurantId}

Returns restaurant details.

---

# PATCH /restaurants/{restaurantId}

Updates restaurant.

---

# DELETE /restaurants/{restaurantId}

Archives restaurant.

---

# POST /restaurants/{restaurantId}/activate

Activates restaurant.

---

# POST /restaurants/{restaurantId}/deactivate

Deactivates restaurant.

---

# POST /restaurants/{restaurantId}/verify

Marks restaurant as verified.

---

# POST /restaurants/{restaurantId}/transfer

Transfers restaurant ownership.

---

# Chapter 6 — Branch Management APIs

## 6.1 Purpose

Branch Management APIs allow Headquarters administrators to manage restaurant branches across all tenants.

These endpoints provide centralized control over branch creation, activation, operational settings, and lifecycle management.

---

## Endpoint Group

```
/branches
```

---

# GET /branches

## Purpose

Returns all restaurant branches.

---

## Required Permission

branch.read

---

## Query Parameters

- page
- pageSize
- tenantId
- restaurantId
- status
- city
- search

---

## Business Rules

- Supports pagination.
- Supports filtering.
- Returns branches across all tenants based on administrator permissions.

---

# POST /branches

## Purpose

Creates a new restaurant branch.

---

## Required Permission

branch.create

---

## Business Rules

- Parent restaurant must exist.
- Branch code must be unique within the restaurant.
- Default operational settings are initialized.

---

# GET /branches/{branchId}

Returns branch details.

---

# PATCH /branches/{branchId}

Updates branch information.

---

# DELETE /branches/{branchId}

Archives branch.

Physical deletion is prohibited.

---

# POST /branches/{branchId}/activate

Activates branch.

---

# POST /branches/{branchId}/deactivate

Temporarily disables branch.

---

# POST /branches/{branchId}/transfer

Transfers branch ownership to another restaurant within the same tenant where permitted.

---

# Chapter 7 — User Management APIs

## 7.1 Purpose

User Management APIs manage tenant-level users across the entire FluxDine platform.

These APIs allow HQ administrators to supervise users without directly managing customer accounts.

---

## Endpoint Group

```
/users
```

---

# GET /users

## Purpose

Returns platform users.

---

## Required Permission

user.read

---

## Query Parameters

- page
- pageSize
- tenantId
- role
- status
- search

---

## Business Rules

- Supports pagination.
- Supports filtering.
- Customer accounts are excluded.

---

# POST /users

## Purpose

Creates a tenant user.

---

## Required Permission

user.create

---

## Business Rules

- Email must be unique.
- Assigned tenant must exist.
- Initial role required.

---

# GET /users/{userId}

Returns user details.

---

# PATCH /users/{userId}

Updates user information.

---

# DELETE /users/{userId}

Soft deletes user.

---

# POST /users/{userId}/activate

Activates user account.

---

# POST /users/{userId}/deactivate

Suspends user account.

---

# POST /users/{userId}/reset-password

Initiates password reset workflow.

---

# POST /users/{userId}/unlock

Unlocks user account after security lockout.

---

# Chapter 8 — Role & Permission APIs

## 8.1 Purpose

Role & Permission APIs manage authorization throughout the FluxDine platform.

These endpoints support Role-Based Access Control (RBAC).

---

## Endpoint Groups

```
/roles

/permissions
```

---

# GET /roles

Returns platform roles.

---

# POST /roles

Creates role.

---

# GET /roles/{roleId}

Returns role details.

---

# PATCH /roles/{roleId}

Updates role.

---

# DELETE /roles/{roleId}

Archives role.

---

# GET /permissions

Returns available permissions.

---

# GET /permissions/{permissionId}

Returns permission details.

---

# POST /roles/{roleId}/permissions

Assigns permissions to role.

---

# DELETE /roles/{roleId}/permissions/{permissionId}

Removes permission from role.

---

## Business Rules

- System roles cannot be deleted.
- Permission inheritance follows the Authorization Architecture.
- Changes generate audit records.

---

# Chapter 9 — Feature Management APIs

## 9.1 Purpose

Feature Management APIs control platform capabilities available to tenants and subscriptions.

---

## Endpoint Group

```
/features
```

---

# GET /features

Returns platform feature catalog.

---

# POST /features

Creates feature.

---

# GET /features/{featureId}

Returns feature details.

---

# PATCH /features/{featureId}

Updates feature.

---

# DELETE /features/{featureId}

Archives feature.

---

# POST /features/{featureId}/enable

Enables feature.

---

# POST /features/{featureId}/disable

Disables feature.

---

# POST /features/{featureId}/assign

Assigns feature to subscription or tenant.

---

# DELETE /features/{featureId}/assignments/{assignmentId}

Removes feature assignment.

---

## Business Rules

- Feature assignments respect subscription limitations.
- Disabled features are unavailable to assigned tenants.
- Platform default features cannot be removed.

---

# Chapter 10 — Branding Management APIs

## 10.1 Purpose

Branding APIs manage tenant branding assets and visual configuration.

---

## Endpoint Group

```
/branding
```

---

# GET /branding

Returns branding configuration.

---

# PATCH /branding

Updates branding configuration.

---

# POST /branding/logo

Uploads tenant logo.

---

# DELETE /branding/logo

Removes logo.

---

# POST /branding/favicon

Uploads favicon.

---

# DELETE /branding/favicon

Removes favicon.

---

# POST /branding/theme

Updates theme configuration.

---

# POST /branding/domain

Registers custom domain.

---

# POST /branding/domain/verify

Verifies custom domain ownership.

---

# DELETE /branding/domain

Removes custom domain.

---

## Business Rules

- Uploaded assets are validated.
- Custom domains require successful verification.
- Branding updates generate audit records.
- Tenant branding remains isolated.

---

# Chapter 11 — Analytics APIs

## 11.1 Purpose

Analytics APIs provide Headquarters administrators with platform-wide operational, financial, and business intelligence data.

These APIs expose aggregated analytics across all tenants while enforcing authorization and audit requirements.

---

## Endpoint Group

```
/analytics
```

---

# GET /analytics/dashboard

## Purpose

Returns platform dashboard metrics.

---

## Required Permission

analytics.dashboard.read

---

## Business Rules

Returns aggregated metrics including:

- Active tenants
- Active restaurants
- Active subscriptions
- Daily orders
- Monthly revenue
- Platform health

---

# GET /analytics/orders

Returns platform-wide order analytics.

Supports:

- Date filtering
- Restaurant filtering
- Tenant filtering
- Status filtering

---

# GET /analytics/revenue

Returns revenue analytics.

Supports:

- Daily
- Weekly
- Monthly
- Yearly reporting

---

# GET /analytics/customers

Returns customer growth metrics.

---

# GET /analytics/restaurants

Returns restaurant performance metrics.

---

# GET /analytics/subscriptions

Returns subscription analytics.

---

# GET /analytics/export

Exports analytics reports.

Supported formats:

- CSV
- XLSX
- PDF

---

## Business Rules

- Export operations are audited.
- Large reports execute asynchronously.
- Data visibility follows administrator permissions.

---

# Chapter 12 — Billing APIs

## 12.1 Purpose

Billing APIs manage subscriptions, invoices, settlements, and platform financial operations.

---

## Endpoint Group

```
/billing
```

---

# GET /billing/invoices

Returns invoices.

---

# GET /billing/invoices/{invoiceId}

Returns invoice details.

---

# POST /billing/invoices

Creates invoice.

---

# PATCH /billing/invoices/{invoiceId}

Updates invoice.

---

# POST /billing/invoices/{invoiceId}/pay

Records invoice payment.

---

# POST /billing/invoices/{invoiceId}/void

Voids invoice.

---

# GET /billing/subscriptions

Returns subscription billing information.

---

# POST /billing/refunds

Creates refund request.

---

# GET /billing/payments

Returns payment transactions.

---

## Business Rules

- Financial operations generate audit records.
- Invoice numbering follows billing standards.
- Payment processing follows Payment Architecture.

---

# Chapter 13 — Audit APIs

## 13.1 Purpose

Audit APIs provide controlled access to platform audit records.

Audit information supports compliance, troubleshooting, and security investigations.

---

## Endpoint Group

```
/audit
```

---

# GET /audit/logs

Returns audit logs.

Supports:

- Pagination
- Filtering
- Date ranges
- User filtering
- Entity filtering

---

# GET /audit/logs/{auditId}

Returns audit record.

---

# GET /audit/entities/{entityType}/{entityId}

Returns entity audit history.

---

# GET /audit/users/{userId}

Returns user audit history.

---

# GET /audit/export

Exports audit records.

---

## Business Rules

- Audit records are immutable.
- Audit records cannot be modified.
- Export operations require elevated permissions.
- Sensitive information shall be protected.

---

# Chapter 14 — System Configuration APIs

## 14.1 Purpose

System Configuration APIs manage global platform configuration.

Only Platform Super Administrators may modify system configuration.

---

## Endpoint Group

```
/configuration
```

---

# GET /configuration

Returns platform configuration.

---

# PATCH /configuration

Updates platform configuration.

---

# GET /configuration/security

Returns security configuration.

---

# PATCH /configuration/security

Updates security configuration.

---

# GET /configuration/features

Returns global feature configuration.

---

# PATCH /configuration/features

Updates global feature configuration.

---

# GET /configuration/maintenance

Returns maintenance mode configuration.

---

# POST /configuration/maintenance/enable

Enables maintenance mode.

---

# POST /configuration/maintenance/disable

Disables maintenance mode.

---

## Business Rules

- Configuration changes are audited.
- Critical configuration changes require elevated permissions.
- Maintenance mode shall not interrupt active administrative sessions without warning.

---

# Chapter 15 — Engineering Rules

## 15.1 Purpose

Engineering Rules define the mandatory standards governing every Headquarters API.

---

## Rule HQAPI-001

Every endpoint shall comply with **01 REST API Specification.md**.

---

## Rule HQAPI-002

Every HQ endpoint shall require authentication.

---

## Rule HQAPI-003

Authorization shall be enforced using Role-Based Access Control (RBAC).

---

## Rule HQAPI-004

Every endpoint shall validate permissions before executing business logic.

---

## Rule HQAPI-005

Every data modification shall generate an audit record.

---

## Rule HQAPI-006

All responses shall use the standardized response structure.

---

## Rule HQAPI-007

Platform administrators shall never bypass tenant isolation except where explicitly authorized.

---

## Rule HQAPI-008

Breaking endpoint changes require API versioning.

---

## Rule HQAPI-009

Deprecated endpoints shall follow the API Versioning Specification.

---

## Rule HQAPI-010

This document is the authoritative endpoint specification governing Headquarters APIs within the FluxDine platform.

---

# Chapter 16 — Architecture Decision Records

## ADR-HQ-001

Headquarters APIs are reserved exclusively for platform administration.

---

## ADR-HQ-002

Administrative endpoints inherit the REST engineering standards defined in **01 REST API Specification.md**.

---

## ADR-HQ-003

Every HQ endpoint shall enforce authentication and authorization.

---

## ADR-HQ-004

Administrative actions shall generate audit records.

---

## ADR-HQ-005

Business validation shall execute before persistence.

---

## ADR-HQ-006

Tenant isolation shall remain enforced throughout Headquarters operations.

---

## ADR-HQ-007

Endpoint contracts shall remain independent of database implementation.

---

## ADR-HQ-008

Administrative APIs shall remain backward compatible within supported API versions.

---

## ADR-HQ-009

All endpoint catalogs shall follow the standardized endpoint template.

---

## ADR-HQ-010

This document is the authoritative Headquarters API specification for the FluxDine platform.

---

# Appendix A — HQ API Matrix

| Domain | Endpoint Group | CRUD | Administrative Actions |
|---------|----------------|:----:|:----------------------:|
| Platform | /platform | ✓ | ✓ |
| Identity | /identity | ✓ | ✓ |
| Tenant | /tenants | ✓ | ✓ |
| Subscription | /subscriptions | ✓ | ✓ |
| Restaurant | /restaurants | ✓ | ✓ |
| Branch | /branches | ✓ | ✓ |
| User | /users | ✓ | ✓ |
| Role | /roles | ✓ | ✓ |
| Feature | /features | ✓ | ✓ |
| Branding | /branding | ✓ | ✓ |
| Analytics | /analytics | Read | Export |
| Billing | /billing | ✓ | ✓ |
| Audit | /audit | Read | Export |
| Configuration | /configuration | Read/Update | ✓ |

---

# Appendix B — Permission Matrix

| Module | Read | Create | Update | Delete | Special Actions |
|---------|:----:|:------:|:------:|:------:|:---------------:|
| Platform | ✓ | — | ✓ | — | Health, Statistics |
| Identity | ✓ | ✓ | ✓ | ✓ | Reset Password |
| Tenant | ✓ | ✓ | ✓ | Archive | Suspend, Activate |
| Restaurant | ✓ | ✓ | ✓ | Archive | Verify, Transfer |
| Branch | ✓ | ✓ | ✓ | Archive | Activate, Transfer |
| Users | ✓ | ✓ | ✓ | Soft Delete | Unlock |
| Roles | ✓ | ✓ | ✓ | Archive | Assign Permissions |
| Features | ✓ | ✓ | ✓ | Archive | Assign |
| Branding | ✓ | Upload | ✓ | Remove | Verify Domain |
| Billing | ✓ | ✓ | ✓ | Void | Refund |
| Audit | ✓ | — | — | — | Export |
| Configuration | ✓ | — | ✓ | — | Maintenance |

---

# Appendix C — Endpoint Naming Examples

Endpoints shall follow the URI standards defined in **01 REST API Specification.md**.

### Examples

```text
GET    /tenants

POST   /tenants

GET    /tenants/{tenantId}

PATCH  /tenants/{tenantId}

DELETE /tenants/{tenantId}

POST   /tenants/{tenantId}/activate

POST   /restaurants/{restaurantId}/verify

POST   /features/{featureId}/assign

GET    /analytics/dashboard

GET    /billing/invoices
```

---

## Naming Principles

- Lowercase URIs
- Plural resource names
- Hyphen-separated words
- Resource-oriented paths
- Stable endpoint contracts

---

# Appendix D — Reserved Future APIs

Future Headquarters API groups may include:

## Inventory Management

```
/inventory
```

---

## Supplier Management

```
/suppliers
```

---

## Kitchen Display System

```
/kitchen
```

---

## Marketplace Management

```
/marketplace
```

---

## Workforce Management

```
/workforce
```

---

## Fleet Management

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
- 03 Restaurant APIs
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
| 0.5 | Core Administrative APIs | FluxDine Engineering | Platform, identity, tenant, subscription, restaurant, branch, user, role, feature, and branding APIs completed |
| 0.8 | Administrative Services | FluxDine Engineering | Analytics, billing, audit, and system configuration APIs completed |
| 1.0 | Final Release | FluxDine Engineering | Approved as the authoritative Headquarters API specification |