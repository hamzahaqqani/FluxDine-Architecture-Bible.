# 04 Engineering Specifications

# APIs

# 08 — Error Code Catalog

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-API-008 |
| **Document Name** | Error Code Catalog |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | 01 REST API Specification<br>02 HQ APIs<br>03 Restaurant APIs<br>04 Self-Service APIs<br>05 Shared Service APIs<br>06 Integration APIs |
| **Referenced By** | All REST APIs<br>Frontend Applications<br>SDKs<br>API Documentation<br>Monitoring Systems |

---

# Dependencies

This specification depends upon:

- 01 REST API Specification
- All API Specifications
- Security Architecture
- Database Engineering Specifications

Every API error returned by the platform shall conform to this specification.

---

# Referenced By

This specification is referenced by:

- HQ APIs
- Restaurant APIs
- Self-Service APIs
- Shared Service APIs
- Integration APIs
- Webhook Processing
- SDKs
- Client Applications

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

This document defines the standardized error catalog used throughout the FluxDine platform.

It establishes consistent error codes, HTTP status mappings, error response formats, and engineering standards for communicating failures to API consumers.

---

# Scope

This specification defines:

- Error response format
- Error categories
- Error code naming
- HTTP status mapping
- Validation errors
- Authentication errors
- Authorization errors
- Business rule errors
- Infrastructure errors
- Integration errors
- Engineering rules

---

# Out of Scope

This specification does not define:

- Business workflows
- Exception handling implementation
- Logging architecture

Those topics are documented separately.

---

# Error Philosophy

Every error returned by the platform shall:

- Be predictable.
- Be machine readable.
- Be human understandable.
- Be version independent.
- Preserve security.
- Support troubleshooting.

---

# Standard Error Response

Every API error shall follow the structure below.

```json
{
  "success": false,
  "error": {
    "code": "AUTH-001",
    "message": "Authentication required.",
    "details": [],
    "correlationId": "xxxxxxxx",
    "timestamp": "2026-01-01T12:00:00Z"
  }
}
```

---

# Error Code Format

Standard format:

```
CATEGORY-NNN
```

Examples

```
AUTH-001

AUTH-005

VAL-003

ORDER-014

PAY-008
```

---

# Error Categories

| Prefix | Category |
|---------|----------|
| AUTH | Authentication |
| PERM | Authorization |
| VAL | Validation |
| TENANT | Tenant |
| USER | User |
| REST | Restaurant |
| BRANCH | Branch |
| MENU | Menu |
| PRODUCT | Product |
| MODIFIER | Modifier |
| ORDER | Order |
| RES | Reservation |
| CUSTOMER | Customer |
| PAYMENT | Payment |
| BILL | Billing |
| SUB | Subscription |
| FILE | File Management |
| MEDIA | Media |
| NOTIFY | Notification |
| SEARCH | Search |
| CONFIG | Configuration |
| WEBHOOK | Webhooks |
| INTEGRATION | Integrations |
| SYSTEM | Platform |
| DB | Database |
| NETWORK | Network |

---

# HTTP Status Mapping

| HTTP Status | Usage |
|-------------|-------|
| 400 | Invalid Request |
| 401 | Authentication Required |
| 403 | Permission Denied |
| 404 | Resource Not Found |
| 409 | Conflict |
| 410 | Resource Removed |
| 422 | Validation Failed |
| 429 | Rate Limited |
| 500 | Internal Error |
| 503 | Service Unavailable |

---

# Authentication Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| AUTH-001 | 401 | Authentication required |
| AUTH-002 | 401 | Invalid credentials |
| AUTH-003 | 401 | Invalid access token |
| AUTH-004 | 401 | Token expired |
| AUTH-005 | 401 | Refresh token expired |
| AUTH-006 | 401 | Account locked |
| AUTH-007 | 403 | Email not verified |
| AUTH-008 | 403 | Multi-factor authentication required |

---

# Authorization Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| PERM-001 | 403 | Permission denied |
| PERM-002 | 403 | Role required |
| PERM-003 | 403 | Tenant access denied |
| PERM-004 | 403 | Branch access denied |
| PERM-005 | 403 | Resource ownership required |

---

# Validation Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| VAL-001 | 422 | Validation failed |
| VAL-002 | 422 | Required field missing |
| VAL-003 | 422 | Invalid format |
| VAL-004 | 422 | Invalid value |
| VAL-005 | 422 | Duplicate value |
| VAL-006 | 422 | Invalid enum value |
| VAL-007 | 422 | Maximum length exceeded |
| VAL-008 | 422 | Minimum value violated |

---

# Tenant Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| TENANT-001 | 404 | Tenant not found |
| TENANT-002 | 403 | Tenant suspended |
| TENANT-003 | 403 | Subscription expired |
| TENANT-004 | 409 | Tenant already exists |

---

# Restaurant Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| REST-001 | 404 | Restaurant not found |
| REST-002 | 403 | Restaurant inactive |
| REST-003 | 409 | Restaurant already verified |

---

# Branch Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| BRANCH-001 | 404 | Branch not found |
| BRANCH-002 | 409 | Branch already active |
| BRANCH-003 | 409 | Branch already inactive |

---

# Menu & Product Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| MENU-001 | 404 | Menu not found |
| MENU-002 | 409 | Menu already published |
| PRODUCT-001 | 404 | Product not found |
| PRODUCT-002 | 409 | Product unavailable |
| MODIFIER-001 | 404 | Modifier not found |

---

# Order Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| ORDER-001 | 404 | Order not found |
| ORDER-002 | 409 | Invalid order state |
| ORDER-003 | 409 | Order already completed |
| ORDER-004 | 409 | Order already cancelled |
| ORDER-005 | 422 | Order cannot be modified |

---

# Reservation Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| RES-001 | 404 | Reservation not found |
| RES-002 | 409 | Reservation conflict |
| RES-003 | 422 | Reservation unavailable |

---

# Customer Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| CUSTOMER-001 | 404 | Customer not found |
| CUSTOMER-002 | 403 | Customer access denied |

---

# Payment & Billing Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| PAYMENT-001 | 402 | Payment declined |
| PAYMENT-002 | 409 | Payment already captured |
| PAYMENT-003 | 422 | Invalid payment method |
| BILL-001 | 404 | Invoice not found |
| BILL-002 | 409 | Invoice already paid |

---

# File & Media Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| FILE-001 | 413 | File too large |
| FILE-002 | 415 | Unsupported file type |
| MEDIA-001 | 422 | Media processing failed |

---

# Integration Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| INTEGRATION-001 | 503 | Provider unavailable |
| INTEGRATION-002 | 504 | Provider timeout |
| INTEGRATION-003 | 401 | Invalid integration credentials |

---

# Webhook Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| WEBHOOK-001 | 401 | Invalid signature |
| WEBHOOK-002 | 400 | Invalid payload |
| WEBHOOK-003 | 409 | Duplicate event |

---

# System Errors

| Code | HTTP | Description |
|------|:----:|-------------|
| SYSTEM-001 | 500 | Internal server error |
| SYSTEM-002 | 503 | Service unavailable |
| SYSTEM-003 | 429 | Rate limit exceeded |
| DB-001 | 500 | Database unavailable |
| NETWORK-001 | 504 | Network timeout |

---

# Engineering Rules

## Rule ERR-001

Every API error shall follow the standard error response.

---

## Rule ERR-002

Error codes shall remain immutable once released.

---

## Rule ERR-003

Error messages shall be human readable.

---

## Rule ERR-004

Sensitive implementation details shall never be exposed.

---

## Rule ERR-005

HTTP status codes shall accurately reflect the error condition.

---

## Rule ERR-006

Correlation IDs shall be included for server-side errors.

---

## Rule ERR-007

Every error shall be logged according to the Logging Architecture.

---

## Rule ERR-008

Breaking error changes require API versioning.

---

## Rule ERR-009

Client applications shall rely on error codes rather than error messages.

---

## Rule ERR-010

This document is the authoritative Error Code Catalog for the FluxDine platform.

---

# Architecture Decision Records

## ADR-ERR-001

All APIs use a standardized error response envelope.

---

## ADR-ERR-002

Error codes remain stable across supported API versions.

---

## ADR-ERR-003

Error messages prioritize clarity while avoiding sensitive implementation details.

---

## ADR-ERR-004

Error categories group related failures using standardized prefixes.

---

## ADR-ERR-005

HTTP status codes follow standard REST semantics.

---

## ADR-ERR-006

Correlation IDs support distributed troubleshooting.

---

## ADR-ERR-007

Every error is auditable.

---

## ADR-ERR-008

Error contracts remain independent of implementation technology.

---

## ADR-ERR-009

Client integrations should depend on error codes instead of localized messages.

---

## ADR-ERR-010

This document is the authoritative Error Code Catalog for the FluxDine platform.

---

# Appendix A — Error Category Matrix

| Category | Prefix | Example |
|----------|--------|----------|
| Authentication | AUTH | AUTH-001 |
| Authorization | PERM | PERM-001 |
| Validation | VAL | VAL-001 |
| Tenant | TENANT | TENANT-001 |
| Restaurant | REST | REST-001 |
| Order | ORDER | ORDER-001 |
| Payment | PAYMENT | PAYMENT-001 |
| Integration | INTEGRATION | INTEGRATION-001 |
| System | SYSTEM | SYSTEM-001 |

---

# Appendix B — HTTP Status Matrix

| Status | Typical Categories |
|--------:|--------------------|
| 400 | Request Format |
| 401 | Authentication |
| 403 | Authorization |
| 404 | Missing Resources |
| 409 | Business Conflicts |
| 422 | Validation |
| 429 | Rate Limiting |
| 500 | Internal Errors |
| 503 | Service Availability |

---

# Appendix C — Error Naming Examples

```text
AUTH-001

VAL-004

ORDER-003

PAYMENT-002

WEBHOOK-001

SYSTEM-001
```

---

# Appendix D — Reserved Future Error Categories

Future prefixes may include:

```
AI

INVENTORY

SUPPLIER

WORKFORCE

FLEET

MARKETPLACE

KDS

LOYALTY
```

---

# References

- 01 REST API Specification
- 02 HQ APIs
- 03 Restaurant APIs
- 04 Self-Service APIs
- 05 Shared Service APIs
- 06 Integration APIs
- 07 Webhook Specification
- 09 API Versioning

- Security Architecture
- Database Engineering Specifications

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | Initial Release | FluxDine Engineering | Approved as the authoritative Error Code Catalog for the FluxDine platform |