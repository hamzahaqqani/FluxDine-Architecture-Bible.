# 08 Implementation Roadmap

# Release Roadmap

---

# Purpose

This document defines the high-level progression from development to production release.

It intentionally does not define calendar dates because release timing depends on actual development capacity, sprint velocity, testing results, and production readiness.

---

# Release Progression

```text
Development
     ↓
Internal Validation
     ↓
Feature Complete
     ↓
Release Candidate
     ↓
Production Readiness
     ↓
Production Release
     ↓
Post-Release Monitoring
```

---

# Release 0 — Foundation

## Objective

Establish the technical development foundation.

## Includes

- Repository
- Application shells
- Backend foundation
- Database foundation
- Authentication foundation
- CI
- Testing foundation

## Exit Criteria

Foundation can support implementation of the SaaS Core.

---

# Release 1 — SaaS Core

## Objective

Establish the core multi-tenant SaaS platform.

## Includes

- Identity
- Tenants
- Membership
- Authorization
- Tenant isolation
- Restaurant registry
- Subscription lifecycle foundation

## Exit Criteria

A tenant can securely exist within the platform.

---

# Release 2 — HQ Platform

## Objective

Provide centralized platform administration.

## Includes

- HQ dashboard
- Tenant administration
- Restaurant administration
- Subscription visibility
- Payment visibility
- Analytics
- Audit
- Feature flags

## Exit Criteria

Authorized platform operators can manage the SaaS platform.

---

# Release 3 — Restaurant Platform

## Objective

Provide restaurant operational capabilities.

## Includes

- Restaurant dashboard
- Branches
- Menus
- Orders
- Reservations
- Offers
- Restaurant analytics
- Notifications

## Exit Criteria

A restaurant can operate its core business workflows.

---

# Release 4 — Self-Service

## Objective

Enable the complete restaurant self-service lifecycle.

## Includes

- Registration
- Verification
- Plan selection
- Trial
- Onboarding
- Configuration
- Payment gateway setup (Phase 07–08: Demo Payment Gateway; no live credentials)
- Domain setup
- Theme setup
- Launch

## Exit Criteria

A restaurant can progress from registration to launch through the approved self-service workflow.

---

# Release 5 — Shared Services

## Objective

Complete and operationalize shared platform capabilities.

## Includes

- Payment
- Billing
- Notifications
- Email
- Analytics
- Domain
- Theme
- Feature Flags
- Audit
- Logging
- Monitoring
- File Storage
- Search

## Exit Criteria

All required shared capabilities operate through approved service boundaries.

---

# Release 6 — Infrastructure

## Objective

Harden production-grade operational infrastructure for the **current** Initial Production platform.

This is Phase 07 work: operate and complete Vercel + Turso + R2 + Resend + Sentry + Cloudflare DNS. It is **not** a PostgreSQL cutover, Kubernetes introduction, database-per-service migration, custom DNS automation program, multi-region build, or dedicated worker/queue/cache platform.

## Includes

- Initial Production operation (`fluxdine-staging` Vercel project)
- Deployment and secrets isolation
- Turso shared database / shared schema
- Application R2 storage plus dedicated private R2 database backups (ADR-055)
- Cloudflare DNS (hostname → restaurant → tenant remains application-owned)
- TLS as provided by the current hosting/DNS model (automatic custom-domain SSL automation deferred)
- Monitoring and logging
- Database backup and disaster recovery per ADR-055
- Application scheduled processing (Vercel Cron) distinct from database backup (GitHub Actions)

## Exit Criteria

The platform can be deployed and operated reliably in production infrastructure.

---

# Release 7 — Production

## Objective

Release FluxDine into controlled production operation.

## Includes

- Final regression testing
- Security validation
- Performance validation
- Production readiness review
- Release candidate
- Production deployment
- Smoke testing
- Monitoring

## Exit Criteria

Production acceptance criteria are satisfied.

---

# Release Governance

Every production release shall follow the Release Process.

A release shall not proceed when:

- Critical tests fail.
- Critical security issues remain unresolved.
- Required migrations are unverified.
- Monitoring is unavailable.
- Rollback is unavailable.
- Critical acceptance criteria are incomplete.

---

# Release Versioning

Production releases follow Semantic Versioning.

Examples:

```text
1.0.0
1.1.0
1.1.1
2.0.0
```

Every production release shall be tagged in Git.

---

# Post-Release

After each production release:

- Monitor platform health.
- Review incidents.
- Review customer-impacting failures.
- Verify critical workflows.
- Record lessons learned.
- Create follow-up backlog items.

---

# Roadmap Authority

This document defines release progression.

Actual release dates and sprint assignments shall be determined during Sprint Planning and Release Planning.

The Architecture Bible remains authoritative for architectural decisions.