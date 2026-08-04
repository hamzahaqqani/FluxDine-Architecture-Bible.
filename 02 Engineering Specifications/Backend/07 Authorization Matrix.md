# 04 Engineering Specifications

# Backend

# 07 — Authorization Matrix

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-BE-007 |
| **Document Name** | Authorization Matrix |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | Service Specification<br>REST API Specification<br>Security Architecture<br>Identity & Authorization Architecture |
| **Referenced By** | HQ APIs<br>Restaurant APIs<br>Self-Service APIs<br>Services<br>Repositories |

---

# Dependencies

This specification depends upon:

- Security Architecture
- Identity & Authorization Architecture
- REST API Specification
- Service Specification
- Repository Specification

Authorization shall be enforced consistently throughout every layer of the FluxDine platform.

---

# Referenced By

This specification is referenced by:

- HQ APIs
- Restaurant APIs
- Self-Service APIs
- Background Jobs
- Queue Workers
- Integration APIs
- Webhook Authorization

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

This document defines the authorization model used throughout the FluxDine platform.

Authorization determines which authenticated identities are permitted to perform specific actions on platform resources.

The platform uses **Role-Based Access Control (RBAC)** with resource ownership, tenant isolation, and permission-based authorization.

---

# Scope

This specification defines:

- Platform roles
- Permission model
- Resource ownership
- Authorization hierarchy
- Role inheritance
- Authorization matrix
- Permission naming
- Engineering rules

---

# Out of Scope

This specification does not define:

- Authentication
- Identity management
- JWT implementation
- Session management

These are covered by the Security Architecture.

---

# Authorization Philosophy

Authorization shall:

- Follow least privilege.
- Enforce tenant isolation.
- Protect resource ownership.
- Be evaluated server-side.
- Remain independent of UI visibility.
- Never trust client-side authorization.

---

# Authorization Architecture

```
Authenticated User

↓

Tenant Validation

↓

Role Resolution

↓

Permission Resolution

↓

Resource Ownership Validation

↓

Business Rule Validation

↓

Access Granted / Denied
```

---

# Authorization Layers

Every protected operation shall validate:

1. Authentication
2. Tenant Access
3. Resource Ownership
4. Role
5. Permission
6. Business Rules

Failure at any stage terminates execution.

---

# RBAC Model

FluxDine uses **Role-Based Access Control (RBAC)**.

```
User

↓

Assigned Role(s)

↓

Permissions

↓

Protected Resources
```

Roles grant permissions.

Permissions authorize operations.

---

# Platform Roles

## Headquarters

- Platform Super Administrator
- Platform Administrator
- Platform Support
- Platform Finance
- Platform Analyst

---

## Tenant

- Restaurant Owner
- Restaurant Administrator

---

## Branch

- Branch Administrator
- Branch Manager
- Cashier
- Kitchen Staff
- Rider

---

## Customer

- Registered Customer
- Guest Customer

---

# Permission Naming Convention

Permissions follow:

```
<resource>.<action>
```

Examples

```
restaurant.read

restaurant.create

restaurant.update

restaurant.delete

order.accept

order.cancel

reservation.confirm

menu.publish

payment.refund
```

---

# Standard Permission Actions

Standard CRUD actions:

```
read

create

update

delete
```

Operational actions:

```
approve

reject

publish

unpublish

activate

deactivate

assign

transfer

refund

capture

cancel

complete

restore

archive

export

import
```

---

# Resource Ownership

Authorization shall validate ownership.

Examples:

Customer

```
Customer

↓

Own Orders

Own Reservations

Own Profile
```

Restaurant

```
Restaurant

↓

Own Branches

Own Menus

Own Products

Own Orders
```

Platform

```
Platform

↓

All Tenants
```

---

# Tenant Isolation

Tenant-owned resources shall never be accessible across tenants.

Every request shall validate:

```
Current Tenant

=

Resource Tenant
```

Failure results in authorization denial.

---

# Role Hierarchy

```
Platform Super Administrator

↓

Platform Administrator

↓

Restaurant Owner

↓

Restaurant Administrator

↓

Branch Administrator

↓

Branch Manager

↓

Cashier

↓

Kitchen Staff

↓

Rider

↓

Customer
```

Higher roles inherit lower-level permissions only where explicitly configured.

---

# Authorization Matrix

| Resource | Super Admin | Platform Admin | Restaurant Owner | Restaurant Admin | Branch Admin | Manager | Cashier | Kitchen | Rider | Customer |
|-----------|:-----------:|:--------------:|:----------------:|:----------------:|:------------:|:-------:|:-------:|:-------:|:-----:|:--------:|
| Platform | ✓ | ✓ | — | — | — | — | — | — | — | — |
| Tenant | ✓ | ✓ | — | — | — | — | — | — | — | — |
| Restaurant | ✓ | ✓ | ✓ | ✓ | Read | Read | — | — | — | — |
| Branch | ✓ | ✓ | ✓ | ✓ | ✓ | Read | — | — | — | — |
| Menu | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | Read | Read | — | Read |
| Product | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | Read | Read | — | Read |
| Orders | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | Own |
| Reservations | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | Read | — | Own |
| Customers | ✓ | ✓ | ✓ | ✓ | Read | Read | Limited | — | — | Own |
| Payments | ✓ | Finance | Read | Read | — | — | — | — | — | Own |
| Reports | ✓ | ✓ | ✓ | ✓ | Read | Read | — | — | — | — |
| Settings | ✓ | ✓ | ✓ | ✓ | Limited | — | — | — | — | Own |

---

# Permission Matrix

## Restaurant

```
restaurant.read

restaurant.create

restaurant.update

restaurant.delete

restaurant.activate

restaurant.deactivate
```

---

## Branch

```
branch.read

branch.create

branch.update

branch.delete

branch.transfer
```

---

## Menu

```
menu.read

menu.create

menu.update

menu.publish

menu.unpublish
```

---

## Product

```
product.read

product.create

product.update

product.delete

product.activate

product.deactivate
```

---

## Orders

```
order.read

order.create

order.update

order.accept

order.reject

order.prepare

order.ready

order.dispatch

order.complete

order.cancel
```

---

## Reservations

```
reservation.read

reservation.create

reservation.update

reservation.confirm

reservation.cancel

reservation.complete
```

---

## Users

```
user.read

user.create

user.update

user.delete

user.activate

user.deactivate

user.assign-role
```

---

## Payments

```
payment.read

payment.capture

payment.refund

payment.void
```

---

# Authorization Evaluation

Every protected request follows:

```
Authenticate

↓

Resolve Role

↓

Resolve Permissions

↓

Validate Tenant

↓

Validate Ownership

↓

Validate Business Rules

↓

Execute Operation
```

---

# Permission Inheritance

Permissions may be inherited through role hierarchy.

Example:

Restaurant Owner

↓

Restaurant Administrator

↓

Branch Administrator

↓

Manager

↓

Cashier

Permission inheritance shall be explicit.

---

# Background Jobs

Background Jobs execute using Service Accounts.

Service Accounts shall receive only the permissions required to complete assigned work.

---

# API Authorization

Every protected API endpoint shall declare:

- Required Authentication
- Required Permission
- Required Ownership
- Required Tenant

These requirements shall be enforced by the Service Layer.

---

# Engineering Rules

## Rule AUTH-001

Authorization shall be enforced server-side.

---

## Rule AUTH-002

Every protected resource shall require explicit permission.

---

## Rule AUTH-003

Tenant isolation shall always be enforced.

---

## Rule AUTH-004

Resource ownership shall be validated where applicable.

---

## Rule AUTH-005

Permissions shall follow standardized naming conventions.

---

## Rule AUTH-006

Background Jobs shall execute using Service Accounts.

---

## Rule AUTH-007

Authorization shall remain independent of UI visibility.

---

## Rule AUTH-008

Repositories shall not perform authorization.

---

## Rule AUTH-009

Business rules shall execute after authorization.

---

## Rule AUTH-010

This document is the authoritative Authorization Matrix for the FluxDine platform.

---

# Architecture Decision Records

## ADR-AUTH-001

FluxDine adopts Role-Based Access Control (RBAC).

---

## ADR-AUTH-002

Permissions are assigned to roles rather than users.

---

## ADR-AUTH-003

Tenant isolation is mandatory.

---

## ADR-AUTH-004

Authorization is enforced by the Service Layer.

---

## ADR-AUTH-005

Repositories remain authorization agnostic.

---

## ADR-AUTH-006

Permission names follow the `<resource>.<action>` convention.

---

## ADR-AUTH-007

Service Accounts receive least-privilege permissions.

---

## ADR-AUTH-008

Ownership validation complements RBAC.

---

## ADR-AUTH-009

Authorization remains independent of transport protocols.

---

## ADR-AUTH-010

This document is the authoritative Authorization Matrix for the FluxDine platform.

---

# Appendix A — Role Inventory

| Category | Roles |
|----------|-------|
| Platform | Super Administrator, Administrator, Support, Finance, Analyst |
| Restaurant | Owner, Restaurant Administrator |
| Branch | Branch Administrator, Manager, Cashier, Kitchen Staff, Rider |
| Customer | Registered Customer, Guest |

---

# Appendix B — Standard Permission Actions

```text
read
create
update
delete
activate
deactivate
approve
reject
publish
unpublish
assign
transfer
refund
capture
cancel
complete
restore
archive
import
export
```

---

# Appendix C — Permission Naming Examples

```text
restaurant.read

branch.update

menu.publish

product.activate

order.complete

reservation.confirm

payment.refund

user.assign-role
```

---

# Appendix D — Reserved Future Roles

Future roles may include:

```text
Inventory Manager

Supplier Manager

Marketplace Manager

Fleet Manager

Workforce Manager

Loyalty Manager

AI Operations Manager
```

---

# References

- Security Architecture
- Identity & Authorization Architecture
- REST API Specification
- Service Specification
- Repository Specification
- Queue Specification
- Event Catalog

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | Initial Release | FluxDine Engineering | Approved as the authoritative Authorization Matrix for the FluxDine platform |