# 02 Engineering Specifications

# Infrastructure

# 05 — Logging

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-INF-005 |
| **Document Name** | Logging |
| **Version** | 1.1 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | Deployment Specification<br>Monitoring<br>Security Architecture<br>Backend Engineering Specifications |
| **Referenced By** | Monitoring<br>Backup Strategy<br>Disaster Recovery<br>Incident Response<br>Operations |

---

# Dependencies

This specification depends upon:

- Deployment Specification
- Monitoring
- Security Architecture
- Backend Engineering Specifications
- Shared Platform Services Architecture

Logging provides standardized operational, application, security, integration, and audit records throughout the FluxDine platform.

---

# Referenced By

This specification is referenced by:

- Monitoring
- Backup Strategy
- Disaster Recovery
- Incident Management
- Security Operations
- Compliance and Audit Activities

---

# Document Status

| Item | Value |
|------|-------|
| Status | Pending Approval |
| Approval | Pending |
| Implementation | Architecture Defined; Implementation Governed Separately |
| Last Updated | 2026-09-12 |

---

# Purpose

This document defines the authoritative logging architecture for the FluxDine platform.

Logging provides the operational records required to:

- troubleshoot application behavior
- diagnose failures
- support monitoring and alerting
- investigate security events
- trace requests across platform boundaries
- support auditability
- investigate integration failures
- support incident response
- provide operational visibility without exposing sensitive information

Logging is an observability capability and shall not become an unnecessary runtime dependency for core business operations.

---

# Scope

This specification defines:

- Logging architecture
- Current logging infrastructure
- Log categories
- Log levels
- Structured logging
- Correlation identifiers
- Request identifiers
- Tenant-aware logging
- Audit logging
- Security logging
- Integration logging
- Sensitive-data protection
- Log retention principles
- Log access controls
- Logging failure behavior
- Engineering standards
- Future logging capabilities

---

# Out of Scope

This specification does not define:

- Monitoring and alert thresholds
- Backup implementation
- Disaster recovery procedures
- SIEM implementation
- Infrastructure provisioning
- Payment business rules
- Application business logic
- Database schema design
- Full distributed tracing implementation
- Provider-specific operational procedures beyond their logging interfaces

These concerns are defined by their respective architecture and engineering specifications.

---

# Logging Philosophy

FluxDine logging shall be:

- Structured
- Consistent
- Searchable
- Secure
- Tenant-aware
- Environment-aware
- Operationally useful
- Privacy-conscious
- Failure-tolerant
- Technology-aware but not unnecessarily provider-dependent

Logging shall provide enough information to diagnose operational problems without exposing credentials, authentication material, payment information, or unnecessary personal information.

Logs shall support engineering and operational investigation while respecting FluxDine security and tenant-isolation requirements.

---

# Current Logging Architecture

The current FluxDine platform is primarily deployed as a Next.js application on Vercel with shared platform services and external infrastructure providers.

The current logging architecture therefore consists of multiple operational log sources rather than a dedicated centralized log-processing cluster.

```text
FluxDine Application
        |
        +----------------------+
        |                      |
        v                      v
FluxDine Logger            Error Events
        |                      |
        v                      v
Runtime / Application       Sentry
Logs
        |
        v
Vercel Runtime Logs

External Infrastructure Signals
        |
        +---- Turso
        +---- Cloudflare R2
        +---- Resend
        +---- Vercel
        |
        v
Operational Investigation
````

The architecture intentionally avoids introducing a dedicated log aggregation infrastructure before operational scale requires it.

---

# Current Logging Components

## Application Logging

Application code shall use the shared FluxDine logging abstraction rather than scattering provider-specific logging implementations throughout business modules.

The shared logging layer shall provide:

* consistent log levels
* structured metadata
* request context
* tenant context where available
* correlation identifiers
* safe error serialization
* sensitive-data filtering

Business modules should not directly depend on a specific external logging provider.

---

## Vercel Runtime Logging

Vercel provides the current runtime and deployment execution environment for FluxDine.

Vercel runtime logs may contain:

* application runtime events
* request execution information
* deployment/runtime failures
* server-side diagnostic information

Vercel logs are operational logs and shall not be treated as the authoritative audit record for business actions.

Application code shall continue to produce structured log events independently of the hosting provider.

---

## Sentry

Sentry is the current centralized error-monitoring and application observability system for FluxDine.

Sentry is responsible primarily for:

* application exceptions
* unhandled errors
* error context
* stack traces
* release-related diagnostics
* lightweight tracing/diagnostic telemetry where enabled

Sentry shall not be treated as the sole source of application logs or audit records.

Current Sentry configuration is governed by the Monitoring and Deployment specifications.

---

## Database Logging

Turso is the current initial-production database provider.

Database-related operational information may include:

* database connectivity failures
* query failures
* migration failures
* transaction failures
* operational errors

Application logs shall record relevant database operation failures without logging credentials or sensitive query parameters.

Database credentials and authentication material shall never be written to logs.

---

## Object Storage Logging

Cloudflare R2 is the current object-storage provider.

Relevant storage events may include:

* upload failures
* download failures
* authorization failures
* object-not-found conditions
* provider API failures

Object contents shall not be written into application logs merely for diagnostic purposes.

Where object identifiers are logged, they shall not expose secrets or temporary access credentials.

---

## Email Integration Logging

Resend is the current email delivery provider.

Integration logging may record:

* request success/failure
* provider response classification
* message/event identifiers where safe
* delivery operation failures
* rate-limit conditions

Email bodies and unnecessary recipient personal information shall not be logged.

---

# Log Categories

FluxDine supports the following categories.

## Application Logs

Application logs describe normal and abnormal application execution.

Examples:

* API execution
* service execution
* business operation outcomes
* validation failures
* controlled retries
* unexpected application conditions
* background/scheduled operation execution

Application logs are primarily operational and diagnostic.

---

## Infrastructure and Deployment Logs

Infrastructure and deployment logs describe events associated with the execution environment.

Examples:

* deployment execution
* deployment failures
* application startup/runtime failures
* platform runtime errors
* scheduled-job execution failures
* provider availability failures

Infrastructure logs may originate from Vercel or external infrastructure providers.

---

## Audit Logs

Audit logs record business or administrative actions for which traceability is required.

Examples:

* authentication events
* authorization or role changes
* tenant administration actions
* restaurant configuration changes
* branch configuration changes
* order status changes
* reservation status changes
* payment-related administrative actions
* subscription state changes
* domain configuration changes
* feature/configuration changes
* sensitive administrative operations

Audit records shall be treated differently from ordinary diagnostic logs.

---

## Security Logs

Security logs capture security-relevant events.

Examples:

* failed authentication
* authorization failures
* invalid or expired tokens
* suspicious request patterns
* rejected access attempts
* security-policy violations
* administrative security events

Security logging shall support incident investigation without exposing authentication secrets.

---

## Integration Logs

Integration logs describe communication with external providers.

Examples:

* payment provider requests
* email provider requests
* storage provider requests
* webhook processing
* external API failures
* provider response classifications

Integration logs shall contain enough information to diagnose provider failures without exposing credentials, authentication headers, payment card data, or sensitive request bodies.

---

## Scheduled Job Logs

Scheduled operations shall generate operational logging.

Examples:

* reservation status processing
* scheduled maintenance
* future billing operations
* future background processing

Each scheduled operation should produce enough information to determine:

* what operation ran
* when it ran
* whether it succeeded
* whether it failed
* correlation/execution identifier where applicable

The current Vercel Cron configuration and frequency are governed by the Deployment and Monitoring specifications.

---

# Log Levels

FluxDine uses the following standard levels:

| Level | Purpose                                     |
| ----- | ------------------------------------------- |
| TRACE | Extremely detailed execution diagnostics    |
| DEBUG | Development and troubleshooting diagnostics |
| INFO  | Normal operational events                   |
| WARN  | Unexpected but recoverable conditions       |
| ERROR | Failed operations or significant errors     |
| FATAL | Critical process/system failure             |

Production environments should minimize TRACE and DEBUG logging.

Normal production operation should primarily use:

* INFO
* WARN
* ERROR

FATAL should be reserved for genuinely critical failures.

---

# Structured Logging

FluxDine logs shall use structured data rather than relying exclusively on unstructured text messages.

A structured log event should contain, where applicable:

* timestamp
* log level
* service/application identifier
* environment
* message
* correlation ID
* request ID
* user ID
* tenant ID
* restaurant ID
* branch ID
* event type
* error information
* provider/integration identifier
* execution duration where useful

Not every field is required for every event.

Context fields shall only be included when they are available and appropriate.

---

# Tenant-Aware Logging

Tenant isolation applies to operational logging as well as application data.

Where an event occurs within a tenant context, logs should include the relevant:

* tenant ID
* restaurant ID
* branch ID where applicable

Tenant identifiers are operational references and are not themselves authorization grants.

Logging systems and log-access interfaces shall prevent unauthorized users from using log visibility to bypass tenant isolation.

Cross-tenant operational access shall be restricted to authorized platform personnel and governed by the Security Architecture.

---

# Correlation IDs

FluxDine shall support correlation identifiers for tracing related operations.

A correlation ID represents a logical chain of related execution.

Correlation information should propagate across:

* API requests
* internal service calls
* scheduled operations
* background processing
* external integration calls
* webhook processing where applicable

Correlation IDs shall be safe to expose operationally and shall not contain secrets or authentication material.

Correlation IDs improve troubleshooting across multiple log sources.

---

# Request IDs

Each incoming request should receive a unique request identifier.

Request IDs identify an individual request execution.

Request IDs should be available to:

* application logs
* error reporting
* operational diagnostics
* relevant response headers where appropriate

Request IDs and correlation IDs serve different purposes:

* Request ID identifies an individual request.
* Correlation ID groups related operations.

---

# Error Logging

Application errors shall contain sufficient diagnostic information to support investigation.

Where appropriate, error records should include:

* error type
* safe error message
* stack trace
* correlation ID
* request ID
* tenant context
* operation/event type
* relevant provider/integration identifier
* execution context

Error logging shall not expose:

* credentials
* authentication tokens
* secrets
* payment card information
* sensitive request bodies
* unnecessary personal information

Errors should be normalized where necessary to prevent accidental disclosure of internal implementation details to external clients.

---

# Audit Logging

Audit records shall capture actions that require durable traceability.

An audit event should include, where applicable:

* actor identity
* actor type
* action
* timestamp
* target resource
* tenant ID
* restaurant ID
* branch ID
* previous value where appropriate
* new value where appropriate
* request/correlation identifier
* result/outcome

Audit records shall be persisted using the FluxDine application/data architecture rather than relying solely on ephemeral runtime logs.

---

# Audit Integrity

Audit records shall be protected against unauthorized modification or deletion.

Application roles shall not be allowed to modify historical audit records merely as part of ordinary business operations.

Where audit records are stored in the application database, database and application authorization shall restrict mutation access.

Stronger tamper-evident or append-only audit infrastructure may be introduced as compliance and scale requirements increase.

The requirement for audit integrity does not imply that the current platform must immediately deploy a separate immutable log-storage system.

---

# Sensitive Information

The following information shall never be written to logs:

* Passwords
* Password hashes
* API keys
* Database credentials
* Authentication tokens
* JWT secrets
* Encryption keys
* Payment card numbers
* CVV/security codes
* Private keys
* Secret access tokens
* Session secrets

The following should be minimized or redacted where unnecessary:

* Email addresses
* Phone numbers
* Customer names
* Addresses
* Order/customer details
* Request bodies
* Response bodies
* Provider payloads

Sensitive information shall be:

* omitted
* masked
* redacted
* or transformed into a non-sensitive identifier

as appropriate.

---

# Authentication and Authorization Logging

Authentication and authorization events shall be logged when operationally useful.

Examples:

* login success
* login failure
* logout
* token validation failure
* authorization denial
* role changes
* administrative access

Logs shall not contain the authentication credentials or token contents that caused the event.

---

# Integration Logging Standards

External integrations shall use consistent operational logging.

An integration event should identify:

* integration/provider
* operation
* outcome
* timestamp
* correlation ID
* request ID where applicable
* safe provider response classification
* duration where useful

Provider secrets shall never be logged.

Raw third-party payloads should not be logged unless explicitly required for controlled debugging and demonstrably safe after redaction.

---

# Logging and Webhooks

Webhook processing shall produce operational logs sufficient to determine:

* provider/integration
* webhook event classification
* processing attempt
* processing result
* correlation/event identifier where available
* failure reason where applicable

Webhook secrets, signatures, authorization credentials, and unnecessary payload contents shall not be logged.

---

# Logging and Scheduled Operations

Scheduled jobs shall produce structured operational records.

At minimum, scheduled execution should make it possible to determine:

* job identity
* execution time
* execution result
* duration where useful
* failure classification
* correlation/execution identifier where applicable

A failed scheduled operation shall not be silently ignored.

Monitoring and alerting requirements for scheduled jobs are defined by the Monitoring specification.

---

# Log Retention

Log retention shall be determined according to:

* operational requirements
* security requirements
* compliance obligations
* incident-response requirements
* provider capabilities
* storage cost

Retention periods may differ between:

* runtime logs
* application diagnostics
* error-monitoring events
* security logs
* audit records

Audit retention shall be governed separately from ordinary diagnostic-log retention where required.

The 24-hour database RPO and 30-day minimum recoverable database backup history defined by the Infrastructure and Backup architecture do not automatically imply identical retention periods for application logs.

---

# Log Rotation and Storage Management

Where log storage supports persistent retention, storage management shall prevent uncontrolled growth.

Appropriate mechanisms may include:

* automatic rotation
* provider-managed retention
* archival
* compression
* controlled deletion
* retention policies

The current FluxDine architecture relies significantly on managed provider capabilities rather than operating a self-managed log-storage cluster.

---

# Searchability

Operational logs should support searching/filtering by:

* timestamp
* environment
* application/service
* log level
* correlation ID
* request ID
* tenant ID
* restaurant ID
* branch ID
* user ID where appropriate
* event type
* integration/provider
* error classification

Searchability is a primary reason for structured logging.

---

# Logging Performance

Logging shall not materially degrade application performance.

Logging implementations should:

* minimize serialization overhead
* avoid blocking critical business operations
* avoid excessive log volume
* avoid logging large payloads
* use asynchronous/non-blocking mechanisms where practical
* prevent repeated logging of the same failure where unnecessary

A logging failure shall not normally cause a business transaction to fail.

---

# Logging Failure Behavior

Logging is not a critical business dependency.

If logging or an external logging provider becomes unavailable:

* business operations should continue where safely possible
* application requests should not fail solely because a log destination is unavailable
* error handling should degrade gracefully
* operational alerts should identify logging degradation where possible

Sentry or another external observability provider must not become a hard dependency for normal application execution.

---

# Logging Security

Access to production logs shall be restricted.

Logging systems and providers shall use appropriate:

* authentication
* authorization
* access controls
* encrypted transport
* secure storage
* provider security controls

Production log access shall be limited to authorized personnel.

Log access itself may be subject to audit and security monitoring.

---

# Privacy and Data Minimization

FluxDine shall follow data-minimization principles when logging.

The presence of information in an application request does not automatically justify writing that information to logs.

Developers shall ask:

1. Is this information necessary for operational diagnosis?
2. Can a non-sensitive identifier be logged instead?
3. Can the value be redacted?
4. Could the value expose tenant or customer information unnecessarily?

If the answer indicates that the information is not necessary, it should not be logged.

---

# Environment-Aware Logging

Logging behavior shall vary appropriately by environment.

| Environment        | Logging Approach                            |
| ------------------ | ------------------------------------------- |
| Development        | Detailed diagnostics permitted              |
| Testing            | Diagnostic logging permitted for validation |
| Staging            | Production-like operational logging         |
| Initial Production | Controlled production logging               |
| Future Production  | Controlled production logging               |

TRACE and DEBUG should generally be restricted outside development/testing unless explicitly enabled for controlled troubleshooting.

---

# Initial Production Logging

The current `fluxdine-staging` Vercel project represents the Initial Production deployment.

Logging for Initial Production shall therefore follow production-oriented rules:

* avoid unnecessary DEBUG/TRACE volume
* protect sensitive information
* maintain tenant context
* preserve correlation/request identifiers
* capture application failures
* capture integration failures
* integrate with Sentry
* retain sufficient operational information for incident investigation

The Vercel project name does not change the logical environment role of the deployment.

---

# Logging and Monitoring Relationship

Logging and monitoring are related but distinct.

Logging answers:

> What happened?

Monitoring answers:

> Is the system behaving within acceptable operational boundaries?

Examples:

* An ERROR log may record a failed database query.
* Monitoring may detect an elevated database error rate.
* Sentry may group and surface the resulting application exception.
* Incident response may use all three to investigate the event.

Logging shall therefore support monitoring rather than duplicate the entire monitoring architecture.

---

# Logging and Incident Response

Logs shall support incident investigation by providing:

* chronological event information
* error context
* correlation identifiers
* tenant/resource context
* integration context
* security events
* relevant administrative actions

Incident responders shall use logs together with:

* Sentry
* Vercel operational information
* database/provider diagnostics
* monitoring signals
* audit records

No single logging source is guaranteed to contain the complete incident history.

---

# Logging and Deployment

Deployment events should be correlated with application behavior where practical.

Operational investigation should be able to determine:

* which deployment was active
* when the deployment occurred
* which application version/release generated an event

Deployment-specific information is primarily governed by the Deployment and CI/CD specifications.

---

# Logging and Rollback

Application rollback and database recovery are separate concerns.

Logs may help determine whether a deployment introduced an operational failure.

However:

* logs are not a substitute for database backups
* logs are not a substitute for application source control
* logs are not a substitute for deployment artifacts
* logs are not a substitute for audit records

Rollback and disaster recovery procedures are governed by their respective specifications.

---

# Centralization Strategy

FluxDine shall centralize logging conceptually before introducing centralized infrastructure.

Current central observability capabilities are provided by:

* FluxDine shared logging abstraction
* Vercel runtime/deployment logs
* Sentry error monitoring
* provider-specific operational logs

A dedicated centralized log aggregation platform may be introduced later when justified by:

* platform scale
* operational complexity
* compliance requirements
* retention requirements
* incident-response requirements
* multi-service architecture

Such an introduction shall be governed through architecture review and an Architecture Decision Record.

---

# Future Distributed Logging

The current platform is not required to implement a fully distributed microservice logging architecture.

If FluxDine later introduces:

* dedicated backend services
* worker services
* queues
* event-processing services
* regional deployments
* independent infrastructure services

the logging architecture shall be extended to preserve:

* correlation propagation
* structured events
* tenant context
* service identity
* request identity
* integration context

The current specification provides the foundation for that evolution.

---

# Engineering Rules

## Rule LOG-001

Application logging shall use the FluxDine logging abstraction where one is provided.

---

## Rule LOG-002

Logs shall use structured data for operational metadata.

---

## Rule LOG-003

Sensitive credentials, authentication material, payment card data, and secrets shall never be logged.

---

## Rule LOG-004

Tenant context shall be included in logs where the operation is tenant-scoped and the information is available.

---

## Rule LOG-005

Requests shall support correlation and request identifiers.

---

## Rule LOG-006

Audit records shall be protected against unauthorized modification or deletion.

---

## Rule LOG-007

Standard log levels shall be used consistently.

---

## Rule LOG-008

Production logging shall minimize unnecessary DEBUG and TRACE output.

---

## Rule LOG-009

Logging failures shall not normally interrupt business operations.

---

## Rule LOG-010

External logging and observability providers shall not become hard dependencies for core business execution.

---

## Rule LOG-011

Integration logs shall identify the provider and operation without exposing provider credentials or sensitive payloads.

---

## Rule LOG-012

Scheduled operations shall produce operational execution records.

---

## Rule LOG-013

Production log access shall require authorization.

---

## Rule LOG-014

Log retention shall follow organizational, security, compliance, and operational requirements.

---

## Rule LOG-015

The logging architecture shall respect FluxDine tenant-isolation requirements.

---

## Rule LOG-016

This document is the authoritative Logging specification for the FluxDine platform.

---

# Architecture Decision Records

## ADR-LOG-001 — Shared Logging Abstraction

FluxDine uses a shared application logging abstraction to maintain consistency and reduce provider coupling.

---

## ADR-LOG-002 — Managed Observability

FluxDine uses managed platform capabilities such as Vercel logs and Sentry rather than operating dedicated log infrastructure at initial production scale.

---

## ADR-LOG-003 — Structured Logging

Structured logging is required to improve searchability, filtering, and operational diagnosis.

---

## ADR-LOG-004 — Correlation Identifiers

Correlation and request identifiers support operational tracing across related application operations.

---

## ADR-LOG-005 — Tenant-Aware Logging

Operational logs must preserve tenant context where applicable while respecting tenant-isolation controls.

---

## ADR-LOG-006 — Sensitive Data Exclusion

Sensitive information is excluded from logging by default.

---

## ADR-LOG-007 — Audit Separation

Audit records are treated as durable business/security records and are not equivalent to ephemeral diagnostic logs.

---

## ADR-LOG-008 — Logging Failure Isolation

Logging failure shall not normally interrupt core business execution.

---

## ADR-LOG-009 — Provider Independence

Business modules shall not become tightly coupled to a specific logging provider.

---

## ADR-LOG-010 — Future Centralized Logging

Dedicated centralized log infrastructure is deferred until scale, compliance, or operational requirements justify it.

---

## ADR-LOG-011 — Current Architecture Awareness

The logging architecture reflects the current Vercel-hosted application architecture and shall evolve when FluxDine introduces independently deployed services.

---

## ADR-LOG-012 — Authoritative Specification

This document is the authoritative Logging specification for the FluxDine platform.

---

# Appendix A — Current Logging Sources

| Source                 | Primary Purpose                              |
| ---------------------- | -------------------------------------------- |
| FluxDine Shared Logger | Structured application logging               |
| Vercel                 | Runtime and deployment operational logs      |
| Sentry                 | Error monitoring and application diagnostics |
| Turso                  | Database operational diagnostics             |
| Cloudflare R2          | Object-storage integration diagnostics       |
| Resend                 | Email integration diagnostics                |

---

# Appendix B — Standard Log Levels

| Level | Usage                                |
| ----- | ------------------------------------ |
| TRACE | Extremely detailed diagnostics       |
| DEBUG | Development diagnostics              |
| INFO  | Normal operations                    |
| WARN  | Recoverable or unexpected conditions |
| ERROR | Operational failures                 |
| FATAL | Critical failures                    |

---

# Appendix C — Standard Context Fields

```text
timestamp
level
application/service
environment
message
correlationId
requestId
userId
tenantId
restaurantId
branchId
eventType
provider
error
duration
```

Fields are contextual and should only be included when applicable.

---

# Appendix D — Sensitive Data Exclusion

The following must never appear in logs:

```text
Passwords
Password Hashes
API Keys
Database Credentials
Authentication Tokens
JWT Secrets
Encryption Keys
Private Keys
Payment Card Data
CVV/Security Codes
Session Secrets
```

---

# Appendix E — Current Observability Model

```text
                 FluxDine Application
                         |
             +-----------+-----------+
             |                       |
             v                       v
      Shared Logger              Error Events
             |                       |
             v                       v
       Vercel Logs                Sentry
             |
             |
             +-----------+
                         |
                         v
              Operational Investigation
                         |
             +-----------+-----------+
             |           |           |
             v           v           v
           Turso        R2        Resend
```

---

# Appendix F — Future Capabilities

Future logging capabilities may include:

```text
OpenTelemetry
Distributed Tracing
Centralized Log Aggregation
Long-Term Compliance Archiving
Tamper-Evident Audit Storage
Cross-Service Trace Correlation
Advanced Log Analytics
Real-Time Anomaly Detection
AI-Assisted Log Investigation
Cross-Region Log Replication
```

These capabilities are not required for the current Initial Production architecture unless separately approved.

---

# References

* Deployment Specification
* Environment & Secrets Strategy
* Environment Variables
* CI/CD Pipeline
* Monitoring
* Security Architecture
* Backend Engineering Specifications
* Backup Strategy
* Disaster Recovery
* Shared Platform Services Architecture

---

# Revision History

Revision History
Version	Date	Author	Description
1.0	Initial Release	FluxDine Engineering	Initial authoritative Logging specification
1.1	2026-09-12	FluxDine Engineering	Aligned logging architecture with current Vercel, Sentry, Turso, R2, Resend, shared-service, tenant-aware, and Initial Production architecture


| Version | Date            | Author               | Description                                                                                                                                    |
| ------- | --------------- | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.0     | Initial Release | FluxDine Engineering | Initial authoritative Logging specification                                                                                                    |
| 1.1     | 2026-09-12      | FluxDine Engineering | Aligned logging architecture with current Vercel, Sentry, Turso, R2, Resend, shared-service, tenant-aware, and Initial Production architecture |

```
