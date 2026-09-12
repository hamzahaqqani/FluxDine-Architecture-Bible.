# 04 Shared Platform Services

# 11 — Domain Service

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-SPS-011 |
| **Document Name** | Domain Service |
| **Version** | 1.1 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Platform Architecture Team |
| **Classification** | Core Platform Service |
| **Depends On** | Shared Services Overview |
| **Referenced By** | Self-Service Platform<br>Restaurant Platform<br>Customer Platform |

---

# Purpose

The Domain Service provides centralized domain configuration and mapping across the FluxDine platform.

It is the single authoritative owner of **application-side** domain records and hostname mapping used to resolve:

```text
hostname → restaurant → tenant
```

Cloudflare provides DNS. Vercel hosts the application. Cloudflare DNS does not determine tenant identity. Hostname is not a substitute for authorization. Tenant isolation remains mandatory.

## Current Initial Production vs deferred automation

**Current (Initial Production):**

- Domain configuration records and mapping metadata
- Default restaurant hostname model `{restaurant}.fluxdine.com`
- Reserved platform hosts: `app.fluxdine.com` (HQ), `signup.fluxdine.com` (Self-Service), canonical `fluxdine.com`
- `fluxdine.online` redirects to `fluxdine.com` at DNS
- Ownership/conflict rules for hostname records in application data

**Deferred (not current automation):**

- Automatic Cloudflare DNS record provisioning
- Automatic SSL certificate issuance/renewal as a Domain Service runtime duty
- Automatic Vercel custom-domain provisioning
- Fully automated custom-domain onboarding

Custom restaurant domains remain architecture-supported. Automation of custom domains is deferred.

No other service shall implement domain mapping independently.

---

# Responsibilities

The Domain Service owns:

- Domain configuration records
- Domain mapping (hostname to restaurant/tenant)
- Domain verification metadata
- DNS validation **guidance and recorded results** (not DNS hosting)
- SSL **readiness metadata** where stored by the application (not automatic certificate issuance in Initial Production)
- Domain Status
- Domain Health status as recorded by the platform
- Domain Lifecycle of configuration records

DNS hosting and registrar services remain external providers (Cloudflare DNS for FluxDine-managed names).

---

# Out of Scope

The Domain Service does **not** own:

- Authentication
- Tenant Lifecycle
- Restaurant Registry
- Website Rendering
- Theme Management
- Web Hosting
- DNS Hosting
- Domain Registration Sales
- Analytics

Website rendering belongs to the Theme Service.

---

# Service Boundaries

The Domain Service owns:

- Domain configuration data (stored in the Initial Production shared schema)
- Domain APIs
- Domain Events
- Domain Business Rules
- Domain Verification Engine

External DNS providers and SSL providers remain outside the service boundary.

---

# Primary Consumers

The Domain Service is consumed by:

- Self-Service Platform
- Restaurant Platform
- Theme Service
- Notification Service
- Analytics Service
- Monitoring Service

---

# Public APIs

Typical APIs include:

- Register or assign domain configuration
- Verify domain (when verification is in use)
- Record DNS validation results
- Get Domain Status
- Update Domain Configuration
- Remove Domain
- Check Domain Health
- List Domains

Automatic Provision SSL / Renew SSL APIs are **future** custom-domain automation, not current Initial Production requirements.

APIs shall be versioned and documented.

---

# Published Events

The Domain Service publishes events including:

```text
DomainRegistered

DomainVerified

DNSValidated

SSLProvisioned

SSLRenewed

DomainActivated

DomainUpdated

DomainRemoved

DomainHealthChanged
```

---

# Consumed Events

The Domain Service consumes events including:

```text
RestaurantCreated

LaunchCompleted

ThemePublished

SubscriptionActivated

SubscriptionCancelled
```

---

# Data Ownership

The Domain Service exclusively owns:

- Domain Records
- Domain Status
- Domain Verification
- DNS Validation Results
- SSL Certificate Metadata
- Domain Mapping
- Domain Health Status
- Domain History

No other service may modify domain data directly.

---

# Security

The Domain Service shall enforce:

- Tenant Isolation
- Domain Ownership Validation
- Secure DNS Verification
- SSL Certificate Validation
- Administrative Authorization
- Complete Audit Logging

Every domain operation shall validate authorization before execution.

---

# Scalability

The Domain Service shall support growth in mapped restaurants and hostnames on the current managed platform.

The following are **future** capabilities, not Initial Production infrastructure:

- Millions of Custom Domains
- Automated SSL Provisioning
- Global Domain Routing as a separate network fabric
- Independently deployed Domain Service infrastructure

---

# Engineering Rules

- The Domain Service is the single source of truth for domain configuration.
- Default restaurant hostnames follow `{restaurant}.fluxdine.com`.
- Every custom domain, when used, shall belong to exactly one restaurant.
- Domain ownership shall be verified before a custom domain is treated as active.
- Automatic SSL certificate lifecycle is deferred; it is not a current Initial Production Domain Service duty.
- DNS hosting remains Cloudflare (or another DNS provider); the Domain Service does not host DNS.
- Hostname → restaurant → tenant resolution is application-owned.
- Domain data shall never be modified through another service bypassing Domain Service APIs.
- Domain lifecycle changes shall publish domain events where the platform event model applies.
- Every domain operation shall generate an audit record.
- Domain APIs shall remain backward compatible.
- Domain operations shall be idempotent where applicable.
- This document is the authoritative Domain Service specification.

---

# Architecture Decision Records

- Domain management is centralized into a dedicated **logical** platform service.
- External DNS providers remain outside the platform boundary.
- Automatic SSL certificate management is deferred for Initial Production.
- Website rendering remains the responsibility of the Theme Service.
- Domain verification is mandatory before a custom domain is activated.
- Domain events are published through the shared Event Bus **when that bus exists**; Initial Production does not require a dedicated event broker.
- Domain data follows the current Initial Production **Shared Database / Shared Schema**. ADR-003 (database-per-service) is historical and is not the current topology. This document does not rewrite ADR-003.
- Future support for multiple domains per restaurant shall extend this service without changing ownership boundaries.
- Domain routing remains infrastructure-agnostic at the service-contract level; current DNS is Cloudflare.
- This document is the authoritative Domain Service specification.

---

# Quality Attributes

| Attribute | Objective |
|-----------|-----------|
| Reliability | Consistent domain lifecycle management |
| Availability | High domain service uptime |
| Scalability | Millions of managed domains |
| Security | Secure domain ownership and SSL management |
| Performance | Low-latency domain validation |
| Auditability | Complete domain traceability |
| Extensibility | Support future DNS and SSL providers |
| Maintainability | Independent service evolution |

---

# References

- Shared Services Overview
- Restaurant Service
- Theme Service
- Self-Service Architecture
- Domain Configuration
- Event Catalog
- REST API Specification
- Monitoring Specification

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.1 | 2026-09-12 | FluxDine Platform Architecture Team | Distinguished current hostname mapping from deferred DNS/SSL/custom-domain automation; Shared Schema Initial Production; Cloudflare DNS only. |
| 1.0 | Initial Release | FluxDine Platform Architecture Team | Approved as the authoritative Domain Service specification |