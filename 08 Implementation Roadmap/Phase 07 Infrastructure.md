# 08 Implementation Roadmap

# Phase 07 — Infrastructure

---

# Objective

Harden FluxDine Initial Production so the current managed platform can be operated reliably.

Phase 07 is infrastructure and operational readiness for the architecture that already exists. It is not a greenfield rebuild, a PostgreSQL cutover, a database-per-service migration, or a Kubernetes/worker-platform introduction.

---

# Current Initial Production Baseline

The following are already the authoritative current platform:

| Area | Current |
|---|---|
| Application hosting | Vercel |
| Vercel project | `fluxdine-staging` |
| Environment role of that project | **Initial Production** (the project name is not the logical environment) |
| Database | Turso, Shared Database / Shared Schema |
| Application object storage | Cloudflare R2 bucket `fluxdine-staging` |
| Database backup storage | Dedicated private R2 bucket (see ADR-055); not the application File Storage bucket |
| Email | Resend via Email Service |
| Observability | Sentry, plus Vercel and Turso operational signals |
| DNS | Cloudflare DNS only |
| Source control | GitHub; application mainline `master` |
| Application scheduled jobs | Vercel Cron (Hobby currently once daily, e.g. reservation status automation) |
| Database backup runner | GitHub Actions (ADR-055); **not** Vercel Hobby Cron |

Logical environments remain Development, Testing, Staging, and Production.

Current operational path:

```text
Development
    ↓
Testing / CI
    ↓
Initial Production  (Vercel project fluxdine-staging)
```

A separately deployed Staging environment is future work. Do not treat `fluxdine-staging` as disposable staging.

PostgreSQL remains a future migration target. ADR-003 (database-per-service) remains a historical ADR and is not the Initial Production topology.

---

# Phase 07 Scope

Phase 07 shall implement or complete, as documentation and operations require:

## A. Hostname resolution

Application-owned resolution:

```text
hostname → restaurant → tenant
```

Cloudflare is the DNS/address-book layer only. Cloudflare DNS does not determine tenant identity. Hostname is not an authorization substitute. Tenant isolation remains mandatory.

## B. Initial Production deployment model

Operate `fluxdine-staging` as Initial Production:

- GitHub `master` → Vercel build/deployment
- Production-impacting changes require explicit human authorization
- Preview deployments must not receive production secrets
- Application rollback and database restore remain separate operations

## C. Database

Operate and migrate the existing Turso shared database / shared schema.

Do **not** provision PostgreSQL or database-per-service as Phase 07 work.

## D. Database backup and recovery

Follow **ADR-055 — Turso PITR and R2 Independent Database Backup Strategy**:

- Primary recent recovery: Turso native PITR (retention is plan-dependent; do not assume 30 days)
- Independent recovery copy: twice-daily full logical dumps to a dedicated private R2 backup bucket
- Runner: GitHub Actions, separate from ordinary PR CI
- Ordinary CI must not receive production backup secrets
- RPO maximum 24 hours; RTO maximum 4 hours (operational, not automatic failover)
- At least 30 days of successful verified R2 backups
- Quarterly restore testing into a throwaway database
- Restores create a **new** Turso database, then compatibility checks and controlled Vercel cutover
- Never overwrite Initial Production in place

This Phase documents the implementation requirement. It does not itself create buckets, tokens, or workflows.

## E. Object storage

Keep application File Storage on the existing application R2 bucket.

Database backups use a dedicated private backup location (recommended name `fluxdine-database-backups`).

## F. DNS and domains

Cloudflare DNS only.

Canonical structure:

```text
fluxdine.com
fluxdine.online  →  fluxdine.com
app.fluxdine.com          HQ Platform
signup.fluxdine.com       Self-Service Platform
{restaurant}.fluxdine.com Default restaurant platform
```

Custom restaurant domains are architecture-supported; **automation is deferred**.

Do not treat automatic DNS provisioning, automatic SSL provisioning, or automatic Vercel custom-domain provisioning as Phase 07 requirements.

Hostname resolution in the application must exist before wildcard or custom-domain infrastructure is configured.

## G. Observability and recovery

Align operations with:

- Sentry for application errors
- Vercel for deploy/runtime signals
- Turso for database-side capabilities
- R2 for backup object presence/age (ADR-055)
- Health endpoint and deployment verification
- Backup SUCCESS / PARTIAL FAILURE / FAILURE signals

Monitoring must not expose secrets or sensitive tenant data.

## H. Secrets and CI/CD

- Secrets never in Git
- Production credentials isolated from Development, Testing, Preview, and ordinary CI
- Database backup secrets only in the production backup workflow
- Migrations governed separately from application deploy

---

# Explicit Non-Goals (Future)

The following are **not** Phase 07 implementation requirements:

- PostgreSQL migration
- Database-per-service
- Kubernetes, VM fleets, dedicated load-balancer fleets
- Dedicated queue, worker cluster, or distributed cache
- Multi-region deployment
- Automatic DNS / SSL / custom-domain provisioning
- Progressive delivery, blue/green, or canary infrastructure
- Vercel Pro solely to run database backups
- Stripe Connect or subscription billing implementation
- Stripe Test Mode as the payment testing mechanism
- Live restaurant payment processors

Phase 07–08 restaurant commerce testing uses **Demo Payment Gateway** (FD-ENG-BE-009).
- Automatic database failover

These remain future capabilities unless a later accepted ADR changes scope.

---

# Environment Strategy

Maintain the logical model:

```text
Development
    ↓
Testing
    ↓
Staging          (future dedicated deployment)
    ↓
Production
```

Operate today as:

```text
Development
    ↓
Testing / CI
    ↓
Initial Production
```

Environment boundaries for credentials and data shall be maintained.

---

# Deployment Infrastructure

Complete, as already specified by the Deployment Specification:

- Git-sourced builds
- Vercel deployments for the Next.js application
- Environment configuration
- Deployment verification
- Application rollback to a known revision
- Database recovery per ADR-055 (new database + cutover, not in-place overwrite)

---

# Security Infrastructure

Complete operational controls for:

- Secret management
- TLS as provided by the current hosting/DNS model
- Access controls
- Least privilege
- Credential rotation
- Security monitoring

Do not introduce a separate network/VM perimeter architecture in this phase.

---

# Storage Infrastructure

Keep File Storage Service integrated with production R2.

Keep database backup storage separate per ADR-055.

---

# Observability Infrastructure

Operate:

- Sentry error tracking
- Application health checks
- Vercel deployment/runtime visibility
- Backup job success/failure/age/verification (ADR-055)
- Centralized logging as specified by the Logging specification

---

# Background and scheduled work

Current:

- Application scheduled jobs may use Vercel Cron (Hobby currently once daily)
- Database backups use GitHub Actions twice daily (ADR-055)

Future:

- Dedicated queue workers and event-processing infrastructure, if scale justifies it

---

# Scaling

Current architecture is managed-first: Vercel application scaling, Turso shared database, R2, Resend, Sentry.

Independent microservice scaling, queue scaling, and Kubernetes scaling are future, not Phase 07.

---

# Acceptance Criteria

Phase 07 is complete when:

- Hostname → restaurant → tenant resolution is the documented and implemented routing model (implementation is a later Phase 07 engineering task, not this documentation pass)
- `fluxdine-staging` is operated as Initial Production
- Turso shared schema remains the production database
- ADR-055 backup/recovery is implemented and verified (GitHub Actions dumps, dedicated R2, PITR used as primary in-window recovery)
- Ordinary CI has no production backup secrets
- Monitoring covers application errors, deploy health, and backup SUCCESS/FAILURE/age
- Secrets remain out of Git
- Application scheduled jobs execute on the current Vercel Cron capability
- Application rollback and database restore procedures are documented and distinct
- Quarterly restore testing is scheduled
- Explicit non-goals above have not been implemented as if they were current requirements

---

# Dependencies

Phases 01–06.

Authoritative decisions for this phase include:

- ADR-055 — Turso PITR and R2 Independent Database Backup Strategy
- Deployment Specification
- Environment & Secrets Strategy
- Backup Strategy
- Disaster Recovery
- Scaling Strategy

---

# Revision History

| Version | Date | Description |
|---|---|---|
| 1.2 | 2026-09-13 | Demo Payment Gateway is the Phase 07–08 commerce testing gateway. Stripe Connect, Stripe Test Mode, and commercial SaaS billing remain non-goals. |
| 1.1 | 2026-09-12 | Rewritten to match current Initial Production architecture and ADR-055. PostgreSQL, database-per-service, queue workers, Kubernetes, and automatic DNS/SSL are classified as future, not Phase 07. |
| 1.0 | Initial | Original infrastructure phase checklist. |
