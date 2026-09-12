# 04 Shared Platform Services

# 09 — Email Service

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-SPS-009 |
| **Document Name** | Email Service |
| **Version** | 1.1 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Platform Architecture Team |
| **Classification** | Core Platform Service |
| **Depends On** | Shared Services Overview |
| **Referenced By** | Notification Service<br>Identity Service<br>Billing Service<br>Self-Service Platform |

---

# Purpose

The Email Service provides centralized email delivery across the entire FluxDine platform.

Current Initial Production delivery path:

```text
FluxDine application
    → shared Email Service (logical platform service)
    → Resend (current provider)
```

SMTP is **not** the current production email provider.

The Email Service is a **logical** shared platform service. It is not independently deployed infrastructure with its own database. Email persistence, where stored by the platform, belongs to the current Turso **Shared Database / Shared Schema**. Database-per-service is not the current Initial Production topology.

It is the single authoritative owner of:

- Email Delivery
- Email Templates
- Transactional Emails
- Email Queue (logical delivery/retry state; not a dedicated distributed queue)
- Email Scheduling
- Delivery Status
- Bounce Handling
- Email History
- Email Retry
- Email Provider Integration

No other service shall send emails independently.

---

# Responsibilities

The Email Service owns:

- Transactional Email Delivery
- Email Queue Management
- Email Scheduling
- Email Retry
- Email Template Rendering
- Email Personalization
- Delivery Tracking
- Bounce Processing
- Complaint Processing
- Email History
- Provider Failover

---

# Out of Scope

The Email Service does **not** own:

- Authentication
- Tenant Lifecycle
- Restaurant Registry
- Notification Preferences
- Push Notifications
- SMS Delivery
- Billing
- Commerce
- Analytics

Notification orchestration belongs exclusively to the Notification Service.

---

# Service Boundaries

The Email Service owns:

- Email records and templates (shared-schema persistence where applicable)
- Email APIs
- Email Events
- Email Business Rules
- Email Delivery Queue (logical)
- Email Template Registry

Other services consume published APIs only. There is no separate current Email Service database infrastructure.

---

# Primary Consumers

The Email Service is consumed by:

- Notification Service
- Identity Service
- Self-Service Platform
- Billing Service
- Payment Service
- Restaurant Platform
- HQ Platform
- Customer Platform

---

# Public APIs

Typical APIs include:

- Send Email
- Schedule Email
- Cancel Email
- Get Email Status
- Get Email History
- Preview Template
- Validate Template
- Retry Delivery
- Register Template
- Delete Template

APIs shall be versioned and documented.

---

# Published Events

The Email Service publishes events including:

```text
EmailQueued

EmailSent

EmailDelivered

EmailOpened

EmailClicked

EmailBounced

EmailComplained

EmailFailed

EmailRetried
```

---

# Consumed Events

The Email Service consumes events including:

```text
NotificationCreated

UserRegistered

EmailVerificationRequested

PasswordResetRequested

SubscriptionActivated

InvoiceGenerated

OrderConfirmed

RestaurantActivated
```

---

# Data Ownership

The Email Service exclusively owns:

- Email Queue
- Email Templates
- Email History
- Delivery Status
- Bounce Records
- Complaint Records
- Email Provider Responses
- Email Retry Information

No other service may modify email data directly.

---

# Security

The Email Service shall enforce:

- Tenant Isolation
- Recipient Validation
- Authorized Email Requests
- Template Validation
- Provider Authentication
- Complete Audit Logging

Sensitive credentials for email providers shall be securely managed and never exposed.

---

# Scalability

The Email Service shall support:

- Millions of Emails
- High Delivery Throughput
- Global Email Delivery
- Multiple Email Providers
- Horizontal Scaling
- High Availability

---

# Engineering Rules

- The Email Service is the single source of truth for email delivery.
- All transactional emails shall be sent through the Email Service.
- Email templates shall be centrally managed and versioned.
- Provider integrations shall be abstracted behind a provider interface. The current provider is Resend.
- Email delivery shall support automatic retry and failover policies.
- Email data shall never be modified by bypassing Email Service APIs.
- Email lifecycle changes shall publish domain events.
- Every email operation shall generate an audit record.
- Email APIs shall remain backward compatible.
- Email operations shall be idempotent where applicable.
- This document is the authoritative Email Service specification.

---

# Architecture Decision Records

- Email delivery is centralized into a dedicated platform service.
- Notification orchestration remains the responsibility of the Notification Service.
- Email providers shall be abstracted to allow provider replacement without affecting consuming services.
- Email templates are centrally managed.
- Delivery retries and provider failover shall be automated.
- Email events are published through the shared Event Bus **when that bus exists**; Initial Production does not require a dedicated event broker.
- Email persistence follows the current Initial Production **Shared Database / Shared Schema**. ADR-003 (database-per-service) is historical and is not current topology. This document does not rewrite ADR-003.
- Future email providers shall integrate through the provider abstraction layer. SMTP or additional providers may be used later behind the same abstraction; they are not current production.
- Marketing email capabilities may be added without changing transactional email ownership.
- This document is the authoritative Email Service specification.

---

# Quality Attributes

| Attribute | Objective |
|-----------|-----------|
| Reliability | Consistent email delivery |
| Availability | High email service uptime |
| Scalability | Millions of email deliveries |
| Security | Secure email processing |
| Performance | Low-latency email dispatch |
| Auditability | Complete email traceability |
| Extensibility | Support multiple email providers |
| Maintainability | Independent service evolution |

---

# References

- Shared Services Overview
- Notification Service
- Identity Service
- Billing Service
- Event Catalog
- REST API Specification
- Monitoring Specification
- Security Architecture

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.1 | 2026-09-12 | FluxDine Platform Architecture Team | Current path FluxDine → Email Service → Resend; shared schema persistence; SMTP and database-per-service are not current. |
| 1.0 | Initial Release | FluxDine Platform Architecture Team | Approved as the authoritative Email Service specification |