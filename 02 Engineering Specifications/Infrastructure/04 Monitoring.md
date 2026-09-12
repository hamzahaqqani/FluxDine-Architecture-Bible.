# 02 Engineering Specifications

# Infrastructure

# 04 — Monitoring

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-INF-004 |
| **Document Name** | Monitoring |
| **Version** | 1.2 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | Deployment Specification<br>CI/CD Pipeline<br>Logging<br>Security Architecture |
| **Referenced By** | Disaster Recovery<br>Scaling Strategy<br>Operations<br>Incident Management |

---

# Purpose

This document defines the monitoring architecture used throughout the FluxDine platform.

Monitoring provides visibility into:

- Application availability
- Application errors
- Database availability
- Deployment health
- External service failures
- Scheduled job execution
- Performance degradation
- Infrastructure-related failures
- Security-relevant operational events

The monitoring architecture shall support early detection of failures while remaining isolated from normal business processing.

This document is the authoritative Monitoring specification for FluxDine.

---

# Scope

This specification defines:

- Monitoring architecture
- Application health monitoring
- Error monitoring
- Database monitoring
- External dependency monitoring
- Deployment monitoring
- Background job monitoring
- Scheduled job monitoring
- Alerting
- Operational dashboards
- Service Level Indicators
- Service Level Objectives
- Monitoring security
- Monitoring failure handling
- Capacity monitoring
- Future monitoring capabilities

---

# Out of Scope

This specification does not define:

- Logging implementation
- Database backup implementation
- Disaster recovery procedures
- Infrastructure provisioning
- Application business metrics in detail
- Incident response procedures
- Provider-specific infrastructure configuration

These concerns are defined separately.

---

# Monitoring Principles

FluxDine monitoring shall be:

- Continuous where technically supported
- Automated
- Actionable
- Secure
- Observable
- Low-overhead
- Environment-aware
- Tenant-safe
- Independent of business logic

Monitoring shall provide operational visibility without becoming a dependency required for normal application execution.

---

# Current Monitoring Architecture

The current FluxDine monitoring architecture is centered around:

```text
FluxDine Application
        │
        ├──────────────→ Sentry
        │                  │
        │                  ├── Errors
        │                  ├── Exceptions
        │                  ├── Performance Signals
        │                  └── Deployment Visibility
        │
        ├──────────────→ Vercel
        │                  │
        │                  ├── Deployment Status
        │                  ├── Build Status
        │                  └── Runtime Platform Signals
        │
        └──────────────→ Turso
                           │
                           └── Database Health
```

Additional provider-specific monitoring may be used for:

* Cloudflare R2 (application storage and, separately, database backup objects)
* Resend
* Cloudflare DNS (availability of DNS, not tenant identity)
* Vercel Cron (application scheduled jobs, e.g. reservation automation)
* GitHub Actions (ADR-055 database backup workflow)
* Future payment providers
* Future infrastructure services

Kubernetes, VM fleets, dedicated queue/worker metrics, Redis metrics, and host-CPU infrastructure monitoring are **future** categories. They are not current Initial Production monitoring requirements.

---

# Database Backup Monitoring (ADR-055)

Database backup is operational infrastructure. Monitoring shall observe, without exposing secrets or tenant payload data:

- Backup execution (scheduled GitHub Actions run occurred)
- Backup success / partial failure / failure
- Backup verification (object exists, size/metadata, checksum where practical)
- Backup age (time since last **successful verified** dump)
- Backup retention (successful verified copies covering at least 30 days)
- Quarterly restore-test status

A backup object that exists but failed verification is not a successful backup.

Vercel Hobby Cron is **not** the database backup runner and must not be the sole signal that database recovery copies exist.

This specification does not implement alerts. Alerting design shall use these signals.

---

# Monitoring Trust Boundary

Monitoring systems are operational infrastructure and shall remain outside normal business logic.

Monitoring components shall observe application behavior.

They shall not directly modify:

* Orders
* Reservations
* Menus
* Customers
* Restaurants
* Tenants
* Payments
* Business configuration

unless a separately authorized operational workflow explicitly requires such behavior.

---

# Application Monitoring

Application monitoring shall observe:

* Application availability
* HTTP response status
* Application exceptions
* Unhandled errors
* API failures
* Request latency
* Deployment regressions
* Runtime failures

The application shall provide an operational health endpoint.

Current health endpoint:

```text
/api/v1/health
```

---

# Health Monitoring

Health monitoring shall distinguish between:

## Application Availability

Determines whether the deployed application is reachable.

---

## Application Health

Determines whether the application can successfully execute essential runtime checks.

---

## Dependency Health

Determines whether required external dependencies are available.

---

# Current Health Model

The current application health model is intentionally lightweight.

```text
Request
   ↓
/api/v1/health
   ↓
Application Runtime
   ↓
Database Connectivity
   ↓
Health Result
```

Additional dependency checks may be introduced when operationally justified.

---

# Vercel Monitoring

Vercel provides monitoring signals for the application deployment platform.

Operational monitoring shall consider:

* Deployment success
* Deployment failure
* Build failure
* Function/runtime failures
* Application availability
* Relevant platform errors

Vercel deployment status shall be considered part of deployment monitoring rather than the sole source of application health truth.

---

# Database Monitoring

The primary initial-production database provider is Turso.

Database monitoring shall consider:

* Database availability
* Connection failures
* Query failures
* Query latency where available
* Storage growth
* Migration failures
* Database operational errors

Application-level database errors shall be observable through application monitoring.

---

# Database Health

Database health shall be validated through application health checks and provider-level operational visibility where available.

A successful application deployment shall not be considered healthy if the application cannot communicate with its required database.

---

# Object Storage Monitoring

Cloudflare R2 provides object storage for FluxDine.

Monitoring shall consider:

* Storage availability
* Upload failures
* Download failures
* Authentication failures
* Storage growth
* Provider errors

Application failures involving object storage shall be captured through application monitoring.

---

# Email Service Monitoring

Resend provides transactional email delivery.

Monitoring shall consider:

* API failures
* Authentication failures
* Delivery failures where observable
* Rate-limit failures
* Provider errors

Email failure shall not unnecessarily interrupt unrelated application operations.

---

# Payment Monitoring

Payment monitoring shall apply to payment integrations once active.

Monitoring shall consider:

* Payment initialization failures
* Payment confirmation failures
* Webhook failures
* Provider availability
* Connected-account failures
* Refund failures
* Reconciliation anomalies

Payment monitoring shall remain provider-independent at the application architecture level.

---

# Scheduled Job Monitoring

FluxDine uses scheduled jobs for background operational tasks.

Monitoring shall consider:

* Job execution
* Job success
* Job failure
* Job duration
* Missed execution
* Repeated failure

Current scheduled operations shall be monitored according to the capabilities of the active deployment platform.

---

# Current Cron Consideration

The current Initial Production environment uses Vercel Cron.

The current cron frequency may temporarily be lower than the final architectural target because of the active Vercel plan.

This limitation shall not be interpreted as a change to the application's functional architecture.

As infrastructure capacity increases, scheduled-job frequency may be increased without changing the underlying monitoring architecture.

---

# Background Processing Monitoring

Future background workers and queue-based processing shall introduce additional monitoring requirements.

These may include:

* Active jobs
* Failed jobs
* Retry count
* Processing latency
* Queue backlog
* Dead-letter events

Queue and worker monitoring shall only become mandatory when those infrastructure components are introduced.

---

# Error Monitoring

Sentry is the current FluxDine error-monitoring platform.

Sentry shall provide visibility into application failures including:

* Unhandled exceptions
* Runtime errors
* API errors where captured
* Server-side failures
* Client-side failures
* Deployment-related regressions

---

# Sentry Environment Separation

Sentry events shall identify the environment in which they occurred.

The environment value shall distinguish operational contexts such as:

```text
development
testing
staging
production
```

The current Initial Production Sentry configuration uses the existing environment configuration and shall not be changed merely to make naming appear cleaner.

Future environment separation shall use explicit environment identifiers.

---

# Monitoring Data Privacy

Monitoring shall minimize collection of sensitive information.

Monitoring systems shall not unnecessarily collect:

* Passwords
* Authentication tokens
* API keys
* Payment secrets
* Database credentials
* Authorization headers
* Sensitive request bodies

Personally identifiable information shall not be collected unless explicitly required and authorized.

---

# Tenant Isolation in Monitoring

Monitoring shall preserve FluxDine's tenant isolation principles.

Operational telemetry shall not expose one tenant's confidential business information to another tenant.

Platform-level operators may receive cross-tenant operational visibility only through authorized platform operations.

Monitoring data shall not become a side channel for bypassing tenant authorization.

---

# Alerting

Alerts shall be generated for conditions requiring operational attention.

Examples include:

* Application outage
* Elevated error rate
* Critical API failure
* Database connectivity failure
* Repeated deployment failure
* Scheduled job failure
* External provider failure
* Security-relevant operational event

Not every detected event requires an alert.

Alerting shall prioritize actionable failures.

---

# Alert Severity

| Severity | Description                           | Example                            |
| -------- | ------------------------------------- | ---------------------------------- |
| Critical | Immediate customer or platform impact | Production outage                  |
| High     | Significant degradation               | Database failures                  |
| Medium   | Limited operational impact            | Repeated non-critical job failures |
| Low      | Informational                         | Non-critical operational event     |

Severity shall determine operational response priority.

---

# Alert Noise Management

Monitoring shall avoid excessive alerting.

Repeated instances of the same failure should be grouped, deduplicated, rate-limited, or otherwise controlled where supported.

Alerts shall provide sufficient context to support investigation.

---

# Deployment Monitoring

Every production deployment shall be observable.

Deployment monitoring shall verify:

* Deployment completion
* Application reachability
* Health endpoint status
* Database connectivity
* Critical application behavior
* Error telemetry

The deployment pipeline and monitoring system shall work together.

---

# Post-Deployment Monitoring

Immediately after deployment, monitoring should focus on:

```text
Deployment
    ↓
Health Check
    ↓
Error Rate
    ↓
Database Connectivity
    ↓
Critical API Behavior
    ↓
Sentry Signals
```

A deployment that technically succeeds but introduces critical runtime errors shall not be considered operationally healthy.

---

# Performance Monitoring

Performance monitoring shall observe, where available:

* Request latency
* API latency
* Error rate
* Request volume
* Database query latency
* Server execution time
* Scheduled job duration

Performance monitoring shall initially focus on identifying meaningful degradation rather than collecting every possible metric.

---

# Capacity Monitoring

Capacity monitoring shall consider the actual infrastructure components currently in use.

Initial areas include:

* Database storage growth
* Object storage growth
* Request volume
* Function/runtime utilization where available
* Email usage
* Payment usage where applicable

Future infrastructure may add:

* Queue depth
* Worker utilization
* Cache utilization
* Host CPU
* Host memory

These metrics shall only become mandatory when the corresponding infrastructure exists.

---

# Service Level Indicators

Representative FluxDine SLIs include:

* Application availability
* Health endpoint availability
* API error rate
* API latency
* Database availability
* Deployment success rate
* Scheduled job success rate
* Critical external dependency availability

SLIs shall be measured using observable operational data.

---

# Service Level Objectives

FluxDine shall define operational objectives for:

* Availability
* Response time
* Error rate
* Recovery time
* Deployment reliability
* Critical scheduled-job execution

Target values may evolve as the platform matures.

Initial recovery targets are governed by the infrastructure and disaster recovery architecture, including:

```text
RPO: 24 hours maximum
RTO: 4 hours maximum
```

---

# Monitoring Dashboards

Operational dashboards should provide visibility into:

* Application health
* Error trends
* Deployment status
* Database health
* External dependency failures
* Scheduled jobs
* Performance trends

The primary monitoring platform shall provide the most actionable application-level visibility available.

---

# Monitoring Retention

Monitoring retention shall follow the retention capabilities and policies of the selected monitoring providers.

Retention requirements may differ for:

* Error events
* Performance telemetry
* Deployment information
* Operational metrics
* Audit-related events

Retention shall not conflict with privacy and security requirements.

---

# Monitoring Security

Monitoring systems shall:

* Require authorized access
* Use least privilege
* Protect operational information
* Prevent unauthorized configuration changes
* Avoid secret exposure
* Restrict access to sensitive telemetry

Monitoring credentials shall be managed through the approved environment and secrets strategy.

---

# Monitoring Failure Handling

Monitoring failure shall not interrupt normal application traffic.

If the monitoring provider becomes unavailable:

```text
Application
     │
     ├── continues operating
     │
     └── monitoring telemetry may be temporarily unavailable
```

Application availability shall not depend on successful transmission of monitoring telemetry.

---

# Monitoring Degradation

Where monitoring becomes unavailable, operational personnel shall use available secondary signals such as:

* Vercel deployment status
* Application health endpoint
* Provider dashboards
* Application logs
* Database provider status

Monitoring redundancy shall increase as operational requirements increase.

---

# Incident Detection

Monitoring shall support detection of:

* Application outages
* Runtime failures
* Database failures
* Deployment regressions
* Scheduled-job failures
* External service failures
* Significant performance degradation

Detected incidents shall enter the appropriate incident-management workflow.

---

# Security Monitoring

Security-relevant monitoring shall include, where available:

* Authentication failures
* Authorization failures
* Suspicious request patterns
* Repeated access failures
* Secret/configuration events
* Security tool findings

Security monitoring shall complement, not replace, the Security Architecture.

---

# Monitoring and Logging Relationship

Monitoring and logging are separate but complementary concerns.

```text
Application
   ├────────→ Logs
   │
   └────────→ Monitoring / Error Telemetry
```

Logs provide detailed event context.

Monitoring provides operational signals, trends, and alerts.

Neither shall unnecessarily duplicate the responsibilities of the other.

---

# Monitoring and CI/CD Relationship

CI/CD provides deployment lifecycle signals.

Monitoring provides post-deployment operational signals.

```text
CI/CD
  ↓
Deployment
  ↓
Monitoring
  ↓
Verification
  ↓
Operational Health
```

Deployment success alone shall not constitute operational success.

---

# Monitoring and Disaster Recovery

Monitoring shall provide signals that may initiate disaster-recovery procedures.

Examples include:

* Database unavailability
* Persistent application outage
* Data integrity concern
* Provider failure
* Infrastructure failure

Recovery procedures remain defined by the Disaster Recovery specification.

---

# Monitoring and Scaling

Monitoring data shall support future scaling decisions.

Scaling decisions may use:

* Request volume
* Latency
* Database growth
* Storage growth
* Error rate
* Scheduled-job workload
* Provider resource usage

Scaling policies remain defined by the Scaling Strategy.

---

# Engineering Rules

## Rule MON-001

Production application availability shall be monitored.

---

## Rule MON-002

The application health endpoint shall remain operationally verifiable.

---

## Rule MON-003

Critical production failures shall generate actionable operational alerts.

---

## Rule MON-004

Application errors shall be observable through the approved error-monitoring platform.

---

## Rule MON-005

Database failures shall be detectable through application or provider-level monitoring.

---

## Rule MON-006

Production deployments shall include post-deployment health verification.

---

## Rule MON-007

Monitoring telemetry shall not expose secrets.

---

## Rule MON-008

Monitoring shall preserve tenant isolation and confidentiality.

---

## Rule MON-009

Monitoring failures shall not interrupt normal production traffic.

---

## Rule MON-010

Monitoring shall remain independent of business logic.

---

## Rule MON-011

Monitoring shall prioritize actionable signals over excessive alert volume.

---

## Rule MON-012

Monitoring architecture shall reflect currently deployed infrastructure and shall not assume future infrastructure is already operational.

---

## Rule MON-013

This document is the authoritative Monitoring specification for the FluxDine platform.

---

# Architecture Decision Records

## ADR-MON-001 — Centralized Error Monitoring

Sentry is the current centralized application error-monitoring platform.

---

## ADR-MON-002 — Application Health Endpoint

FluxDine exposes an application health endpoint for deployment and operational verification.

---

## ADR-MON-003 — Monitoring Independence

Monitoring shall not become a runtime dependency for normal business operations.

---

## ADR-MON-004 — Provider-Aware Monitoring

Monitoring shall account for the actual external infrastructure providers used by FluxDine.

---

## ADR-MON-005 — Environment-Aware Telemetry

Monitoring events shall identify their execution environment.

---

## ADR-MON-006 — Tenant-Safe Telemetry

Monitoring shall preserve FluxDine tenant isolation and confidentiality requirements.

---

## ADR-MON-007 — Actionable Alerting

Alerting shall prioritize failures requiring operational action.

---

## ADR-MON-008 — Deployment Verification

Successful deployment shall require operational verification beyond deployment-platform success.

---

## ADR-MON-009 — Progressive Monitoring Maturity

Monitoring capabilities shall expand as FluxDine introduces additional infrastructure and reaches higher operational scale.

---

## ADR-MON-010 — Current Infrastructure Alignment

Monitoring specifications shall describe current infrastructure accurately while explicitly reserving future capabilities for later implementation.

---

# Appendix A — Current Monitoring Stack

| Component           | Current Provider / Mechanism |
| ------------------- | ---------------------------- |
| Application hosting | Vercel                       |
| Error monitoring    | Sentry                       |
| Database            | Turso                        |
| Object storage      | Cloudflare R2                |
| Email               | Resend                       |
| DNS                 | Cloudflare                   |
| Scheduled jobs      | Vercel Cron                  |
| Application health  | `/api/v1/health`             |

---

# Appendix B — Current Monitoring Flow

```text
                    FluxDine
                       │
          ┌────────────┼────────────┐
          │            │            │
          ↓            ↓            ↓
       Vercel        Sentry       Turso
          │            │            │
          └────────────┼────────────┘
                       ↓
              Operational Visibility
                       ↓
                  Investigation
                       ↓
                Incident Response
```

---

# Appendix C — Minimum Production Verification

```text
[ ] Deployment successful
[ ] Application reachable
[ ] /api/v1/health successful
[ ] Database reachable
[ ] Critical API behavior verified
[ ] Sentry operational
[ ] No critical runtime errors
```

---

# Appendix D — Alert Examples

| Condition                        | Severity        |
| -------------------------------- | --------------- |
| Production unavailable           | Critical        |
| Database unavailable             | Critical        |
| Major application error spike    | Critical / High |
| Deployment failure               | High            |
| Scheduled job repeatedly failing | High            |
| External email provider failure  | Medium / High   |
| Non-critical operational warning | Low             |

Severity may be adjusted according to actual customer impact.

---

# Appendix E — Reserved Future Monitoring Capabilities

Future monitoring capabilities may include:

```text
Distributed Tracing
Synthetic Monitoring
Real-Time User Experience Monitoring
Advanced Infrastructure Metrics
Queue Monitoring
Worker Monitoring
Cache Monitoring
AI Anomaly Detection
Predictive Capacity Planning
Business KPI Monitoring
Cost Monitoring
Multi-Region Monitoring
Advanced SLO Management
Automated Incident Correlation
```

These capabilities shall not be considered implemented until separately designed, approved, and deployed.

---

# References

* Deployment Specification
* CI/CD Pipeline
* Environment & Secrets Strategy
* Environment Variables
* Logging
* Disaster Recovery
* Scaling Strategy
* Security Architecture
* ADR-055 — Turso PITR and R2 Independent Database Backup Strategy

---

# Revision History

| Version | Date             | Author               | Description                                                                                                    |
| ------- | ---------------- | -------------------- | -------------------------------------------------------------------------------------------------------------- |
| 1.0     | Initial Release  | FluxDine Engineering | Initial Monitoring specification                                                                               |
| 1.2     | 2026-09-12 | FluxDine Engineering | ADR-055 backup monitoring signals; labeled k8s/queue/Redis metrics as future; removed wrapping fences. |
| 1.1     | Approved and Locked | FluxDine Engineering | Aligned monitoring architecture with current Vercel, Sentry, Turso, R2, Resend, and Vercel Cron infrastructure |

---
