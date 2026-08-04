# 04 Engineering Specifications

# Backend

# 08 — DTO Specification

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-BE-008 |
| **Document Name** | DTO Specification |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | REST API Specification<br>Service Specification<br>Repository Specification |
| **Referenced By** | All REST APIs<br>Service Layer<br>Controllers<br>Integration APIs |

---

# Dependencies

This specification depends upon:

- REST API Specification
- Service Specification
- Repository Specification
- Error Code Catalog

DTOs define the standardized data contracts exchanged between the API Layer and the Service Layer.

---

# Referenced By

This specification is referenced by:

- HQ APIs
- Restaurant APIs
- Self-Service APIs
- Shared Service APIs
- Integration APIs
- Controllers
- Services

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

This document defines the Data Transfer Object (DTO) standards used throughout the FluxDine platform.

DTOs provide stable, validated, implementation-independent contracts between application layers while preventing direct exposure of persistence models.

This document serves as the authoritative DTO specification for the backend architecture.

---

# Scope

This specification defines:

- DTO architecture
- DTO organization
- DTO naming conventions
- Request DTOs
- Response DTOs
- Validation DTOs
- Mapping rules
- Serialization
- Versioning
- Engineering rules

---

# Out of Scope

This specification does not define:

- Database entities
- ORM models
- API endpoints
- Repository implementation

These topics are documented separately.

---

# DTO Philosophy

DTOs shall:

- Represent API contracts.
- Hide persistence models.
- Be immutable after creation.
- Support validation.
- Remain independent of ORM implementation.
- Remain stable across supported API versions.

DTOs shall never contain business logic.

---

# DTO Architecture

```
Client

↓

Request DTO

↓

Controller

↓

Service

↓

Repository

↓

Database
```

Response flow

```
Database

↓

Repository

↓

Entity

↓

Mapper

↓

Response DTO

↓

Client
```

---

# DTO Categories

FluxDine uses four primary DTO categories.

## Request DTO

Represents incoming client data.

Examples

```
CreateOrderRequestDTO

UpdateMenuRequestDTO

CreateReservationRequestDTO
```

---

## Response DTO

Represents outgoing API responses.

Examples

```
OrderResponseDTO

CustomerResponseDTO

RestaurantResponseDTO
```

---

## Query DTO

Represents filtering and pagination.

Examples

```
OrderSearchDTO

RestaurantFilterDTO

PaginationDTO
```

---

## Internal DTO

Represents data exchanged between internal services.

Examples

```
PaymentGatewayDTO

NotificationDTO

WebhookPayloadDTO
```

Internal DTOs shall never be exposed directly through public APIs.

---

# DTO Naming Convention

Every DTO shall use the suffix:

```
DTO
```

Examples

```
CreateOrderRequestDTO

UpdateCustomerRequestDTO

OrderResponseDTO

RestaurantSummaryDTO

PaymentGatewayDTO
```

---

# DTO Organization

DTOs shall be organized by domain.

Example

```
dto/

├── auth/
├── tenant/
├── restaurant/
├── menu/
├── product/
├── customer/
├── order/
├── reservation/
├── payment/
├── notification/
└── shared/
```

Each domain owns its DTOs.

---

# Request DTO Standards

Request DTOs shall:

- Validate incoming data.
- Define required fields.
- Reject unknown fields where appropriate.
- Avoid persistence concerns.

Example

```
CreateOrderRequestDTO

↓

Customer ID

Branch ID

Items

Payment Method

Notes
```

---

# Response DTO Standards

Response DTOs shall:

- Contain only client-visible data.
- Exclude sensitive information.
- Be serialization friendly.
- Support backward compatibility.

Sensitive fields shall never appear in Response DTOs.

---

# Query DTO Standards

Query DTOs standardize:

- Pagination
- Filtering
- Sorting
- Search

Typical fields

```
page

pageSize

sortBy

sortOrder

search
```

---

# Validation

Validation occurs at the Request DTO layer.

Examples

- Required fields
- Length validation
- Enum validation
- Numeric validation
- Date validation
- Email validation
- Phone validation

Business validation belongs in the Service Layer.

---

# Mapping

DTOs shall be converted using dedicated mappers.

Example

```
Request DTO

↓

Mapper

↓

Entity

↓

Repository
```

Response

```
Entity

↓

Mapper

↓

Response DTO
```

Controllers shall not perform mapping logic.

---

# Serialization

DTO serialization shall:

- Produce JSON.
- Use camelCase property names.
- Exclude null fields where appropriate.
- Exclude internal implementation details.

Serialization behavior shall remain consistent across APIs.

---

# Versioning

DTO changes follow the API Versioning Specification.

Non-breaking changes

- Add optional field
- Add metadata

Breaking changes

- Remove field
- Rename field
- Change field type

Breaking DTO changes require a new API version.

---

# Entity Isolation

Entities shall never be returned directly by controllers.

Correct flow

```
Entity

↓

Mapper

↓

Response DTO
```

Incorrect flow

```
Entity

↓

Controller

↓

Client
```

---

# Sensitive Data

DTOs shall never expose:

- Passwords
- Password hashes
- Secrets
- Access tokens
- Refresh tokens
- API keys
- Internal identifiers
- Encryption keys

Sensitive information remains internal.

---

# Reuse Strategy

Shared DTOs may be reused when appropriate.

Examples

```
PaginationDTO

AddressDTO

MoneyDTO

AuditDTO

FileDTO
```

Business-specific DTOs shall remain domain-specific.

---

# Engineering Rules

## Rule DTO-001

Controllers shall only accept Request DTOs.

---

## Rule DTO-002

Controllers shall only return Response DTOs.

---

## Rule DTO-003

Entities shall never be exposed directly.

---

## Rule DTO-004

DTOs shall not contain business logic.

---

## Rule DTO-005

Business validation belongs in the Service Layer.

---

## Rule DTO-006

DTO mapping shall be performed by dedicated mappers.

---

## Rule DTO-007

Sensitive information shall never appear in Response DTOs.

---

## Rule DTO-008

DTOs shall remain independent of ORM implementation.

---

## Rule DTO-009

Breaking DTO changes require API versioning.

---

## Rule DTO-010

This document is the authoritative DTO Specification for the FluxDine platform.

---

# Architecture Decision Records

## ADR-DTO-001

DTOs separate API contracts from persistence models.

---

## ADR-DTO-002

Entities are never exposed directly.

---

## ADR-DTO-003

Dedicated mappers perform DTO conversion.

---

## ADR-DTO-004

Validation begins at the Request DTO layer.

---

## ADR-DTO-005

Business validation belongs exclusively in services.

---

## ADR-DTO-006

Response DTOs exclude sensitive information.

---

## ADR-DTO-007

DTOs remain implementation independent.

---

## ADR-DTO-008

DTO contracts remain backward compatible within supported API versions.

---

## ADR-DTO-009

Every domain owns its DTOs.

---

## ADR-DTO-010

This document is the authoritative DTO Specification for the FluxDine platform.

---

# Appendix A — Standard DTO Inventory

| Domain | Typical DTOs |
|----------|--------------|
| Authentication | LoginRequestDTO, LoginResponseDTO |
| Tenant | CreateTenantRequestDTO, TenantResponseDTO |
| Restaurant | RestaurantResponseDTO, RestaurantSummaryDTO |
| Menu | CreateMenuRequestDTO, MenuResponseDTO |
| Product | ProductResponseDTO, ProductSearchDTO |
| Customer | CustomerResponseDTO |
| Order | CreateOrderRequestDTO, OrderResponseDTO |
| Reservation | CreateReservationRequestDTO, ReservationResponseDTO |
| Payment | PaymentRequestDTO, PaymentResponseDTO |
| Shared | PaginationDTO, AddressDTO, MoneyDTO |

---

# Appendix B — DTO Mapping Matrix

| Source | Mapper | Target |
|---------|--------|--------|
| Request DTO | DTO Mapper | Entity |
| Entity | DTO Mapper | Response DTO |
| Query DTO | Repository | Query Parameters |
| Internal DTO | Service Mapper | Integration Models |

---

# Appendix C — Naming Examples

```text
CreateRestaurantRequestDTO

UpdateMenuRequestDTO

OrderResponseDTO

CustomerSummaryDTO

PaginationDTO

PaymentGatewayDTO
```

---

# Appendix D — Reserved Future DTOs

Future DTOs may include:

```text
InventoryDTO

SupplierDTO

MarketplaceDTO

FleetDTO

WorkforceDTO

LoyaltyDTO

AIRecommendationDTO

ForecastDTO
```

---

# References

- REST API Specification
- Service Specification
- Repository Specification
- Authorization Matrix
- API Versioning
- Error Code Catalog

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | Initial Release | FluxDine Engineering | Approved as the authoritative DTO Specification for the FluxDine platform |