# 02 Engineering Specifications

# Infrastructure

# 06 — Backup Strategy

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-INF-006 |
| **Document Name** | Backup Strategy |
| **Version** | 1.1 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | Deployment Specification<br>Database Engineering Specifications<br>Environment & Secrets Strategy<br>Logging<br>Monitoring |
| **Referenced By** | Disaster Recovery<br>Operations<br>Security Architecture<br>Incident Response |

---

# Dependencies

This specification depends upon:

- Deployment Specification
- Database Engineering Specifications
- Environment & Secrets Strategy
- Logging
- Monitoring
- Security Architecture

Backups provide the recoverable copies and recovery inputs required by the Disaster Recovery architecture.

---

# Referenced By

This specification is referenced by:

- Disaster Recovery
- Monitoring
- Operations
- Security Operations
- Incident Response
- Compliance and Audit Activities

---

# Document Status

| Item | Value |
|------|-------|
| Status | Pending Approval |
| Approval | Pending |
| Implementation | Architecture Defined; Provider Implementation Governed Separately |
| Last Updated | 2026-09-12 |

---

# Purpose

This document defines the authoritative backup architecture and operational requirements for the FluxDine platform.

The purpose of backups is to provide reliable recovery from:

- accidental deletion
- data corruption
- application or migration errors
- infrastructure failure
- provider failure
- security incidents
- operational mistakes
- disaster scenarios

Backups shall support the recovery objectives defined for Initial Production.

Backups exist to enable recovery, not merely to create copies of data.

---

# Scope

This specification defines:

- Backup architecture
- Protected assets
- Database backup requirements
- Object-storage recovery requirements
- Configuration recovery
- Application recovery inputs
- Backup isolation
- Encryption
- Retention
- Backup verification
- Restore testing
- Monitoring
- Failure handling
- Recovery objectives
- Security requirements
- Operational standards
- Future backup capabilities

---

# Out of Scope

This specification does not define:

- Detailed disaster recovery execution procedures
- Infrastructure provisioning
- Monitoring implementation
- CI/CD implementation
- Database schema design
- Application business logic
- Provider-specific account administration
- Secret values
- Incident-response procedures

These concerns are governed by their respective specifications.

---

# Backup Philosophy

FluxDine backups shall be:

- Automated
- Secure
- Encrypted
- Independently recoverable
- Verified
- Monitored
- Retained according to defined requirements
- Regularly tested

The platform shall distinguish between:

1. **Primary operational data**
2. **Recoverable backup data**
3. **Application source and deployment artifacts**
4. **Configuration and recovery metadata**

Not every recoverable asset requires the same backup mechanism.

---

# Recovery Objectives

Initial Production has the following recovery objectives:

| Objective | Target |
|-----------|--------|
| **Maximum RPO** | 24 hours |
| **Maximum RTO** | 4 hours |
| **Minimum Recoverable Database History** | 30 days |
| **Restore Testing** | At least quarterly |

These targets are mandatory architectural requirements for Initial Production.

The Disaster Recovery specification defines the recovery process that consumes these backup capabilities.

Recovery objectives may be tightened as FluxDine scale, revenue, customer impact, regulatory requirements, and operational maturity increase.

---

# Current Infrastructure Context

The current Initial Production architecture uses:

| Asset | Current Provider / System |
|-------|---------------------------|
| Application | Vercel |
| Database | Turso |
| Object Storage | Cloudflare R2 |
| Source Control | GitHub |
| Error Monitoring | Sentry |
| Email | Resend |
| DNS | Cloudflare DNS |

The backup strategy shall use the recovery capabilities appropriate to each system rather than forcing every provider into an identical backup model.

---

# Backup Architecture

The current backup architecture is:

```text
                 Initial Production
                       |
        +--------------+--------------+
        |              |              |
        v              v              v
      Turso            R2          Application
     Database       Objects        Source/Config
        |              |              |
        v              v              v
   DB Backups     R2 Recovery     GitHub/Vercel
        |              |              |
        +--------------+--------------+
                       |
                       v
             Recovery Verification
                       |
                       v
                Restore Testing
                       |
                       v
              Disaster Recovery
````

The architecture intentionally avoids introducing a dedicated backup service unless operational requirements justify one.

---

# Database Backup Strategy

The database is the highest-priority backup target because it contains business-critical and tenant-scoped data.

The current Initial Production database provider is Turso.

Database backups shall protect, where applicable:

* Tenant records
* Restaurant records
* Branch records
* Users
* Orders
* Order items
* Customers
* Reservations
* Payments and payment-related records
* Subscriptions and billing records
* Configuration records
* Domain configuration
* Theme configuration
* Audit records
* Platform operational records
* Other persistent application data

---

# Database Recovery Requirement

The database backup strategy shall provide a recoverable point no older than the maximum Initial Production RPO of 24 hours.

Therefore:

> Under normal backup operation, the platform must be capable of recovering database state to within 24 hours of a qualifying data-loss event.

A tighter recovery point may be implemented when provider capabilities and operational maturity justify it.

---

# Database Backup Frequency

The exact provider mechanism and schedule shall be selected according to the capabilities of the current database provider.

The mandatory requirement is:

* automated database protection
* sufficient frequency to satisfy the 24-hour maximum RPO
* successful verification
* minimum 30-day recoverable history

The architecture does **not** require a specific hourly incremental or daily full-backup mechanism unless that mechanism is actually adopted and supported by the selected provider.

Provider-specific backup configuration shall be documented separately from this architecture specification.

---

# Database Backup Retention

Initial Production shall maintain at least:

> **30 days of recoverable database backup history.**

Longer retention may be adopted where justified by:

* compliance requirements
* business requirements
* incident-response requirements
* operational requirements
* customer commitments

Retention changes shall not reduce the mandatory 30-day recoverable history without architecture approval.

---

# Independent Database Backup Storage

Critical database recovery data shall not exist exclusively inside the same failure domain as the primary database.

The backup strategy shall therefore provide an independently recoverable backup copy or recovery mechanism appropriate to the selected provider.

The goal is protection against scenarios in which the primary database or its immediate provider environment becomes unavailable or compromised.

---

# Database Migration Compatibility

Database recovery shall account for application/database version compatibility.

A database backup is not considered operationally sufficient merely because the raw data can be restored.

Recovery procedures shall identify:

* database version/state
* migration state
* compatible application version
* required migrations
* migration ordering
* schema compatibility

Application rollback and database rollback shall not be treated as the same operation.

---

# Migration Safety

Database migrations shall be designed with recovery in mind.

Before production migrations that materially change persistent data structures:

* backup/recovery readiness shall be confirmed
* migration compatibility shall be evaluated
* rollback limitations shall be understood
* a known-good application version shall remain available
* recovery procedures shall be available

Destructive or irreversible migrations require additional review.

---

# Object Storage Backup and Recovery

Cloudflare R2 is the current FluxDine object-storage provider.

Object storage may contain:

* restaurant images
* menu media
* documents
* restaurant assets
* customer-uploaded files where supported
* other application-managed objects

The object-storage strategy shall provide a defined recovery and retention mechanism appropriate to the importance of the stored objects.

---

# R2 Recovery Strategy

R2 recovery shall account for:

* object retention
* accidental deletion
* accidental overwrite
* object integrity
* access-control failures
* provider failure
* application-level deletion errors

The initial architecture does not require full cross-provider duplication of every R2 object.

Cross-provider object replication may be introduced later when:

* scale requires it
* business continuity requirements increase
* compliance requires it
* customer commitments require stronger durability guarantees

---

# Object Storage Retention

R2-managed assets shall have retention behavior appropriate to the business purpose of the object.

Critical objects shall not be exposed to uncontrolled permanent deletion where the application requires recovery capability.

The exact R2 retention/versioning mechanism shall be documented as part of infrastructure implementation and operational configuration.

---

# Configuration Recovery

Configuration required to rebuild or recover the platform shall be recoverable without storing secret values in source control.

Recoverable configuration includes, where applicable:

* deployment configuration
* build configuration
* environment-variable definitions
* infrastructure configuration
* domain configuration
* scheduled-job configuration
* feature-flag configuration
* provider integration configuration

Secret **values** shall remain in the approved secrets-management systems.

---

# Secrets and Backups

Secrets shall never be backed up by committing them into GitHub or embedding them into application artifacts.

Examples include:

* database credentials
* API keys
* authentication secrets
* provider credentials
* encryption keys
* payment credentials

Recovery documentation shall identify which secrets are required for restoration without exposing their values.

Secret recovery is therefore a controlled configuration-management process rather than a source-code backup mechanism.

---

# Application Source and Deployment Recovery

Application source code is maintained in GitHub as the source of truth.

The application recovery strategy shall therefore rely on:

* Git history
* known-good commits
* repository history
* deployment configuration
* reproducible builds
* Vercel deployment capability

Application source does not require a conventional database-style backup mechanism when GitHub remains the authoritative versioned source.

---

# Deployment Artifact Recovery

Vercel deployment history and known-good commits provide application redeployment capability.

Recovery shall identify:

* known-good commit
* compatible application version
* deployment configuration
* required environment variables
* required database schema version

Application redeployment shall be possible independently of database restoration.

---

# Sentry and Operational History

Sentry is an observability and error-monitoring system.

Sentry data is not a substitute for:

* database backups
* audit records
* source control
* deployment history

Sentry may assist recovery and incident investigation by providing historical error and deployment context.

---

# Audit Data

Audit records are part of persistent application data and shall therefore be protected by the database backup strategy where stored in the application database.

Audit records shall not rely solely on ephemeral Vercel or runtime logs for recovery.

---

# Logging Data

Operational and runtime logs are not automatically treated as primary business data.

The Logging specification defines their retention and operational handling.

Where logs are required for compliance, security, or incident investigation, their retention requirements shall be evaluated separately.

---

# Backup Encryption

Backups containing sensitive information shall be protected:

* during transmission
* while stored
* during restore operations where applicable

Encryption mechanisms may be provider-managed or platform-managed depending on the backup system.

Encryption keys and credentials shall be protected using the approved secrets-management architecture.

---

# Backup Access Control

Production backups shall only be accessible to authorized personnel and systems.

Backup access shall follow:

* least privilege
* environment separation
* role-based authorization
* secure authentication
* auditability

Backup access must not provide unrestricted application-level access to production systems.

---

# Backup Isolation

Backup infrastructure shall remain operationally separate from the primary application runtime where practical.

A failure of the production application must not automatically destroy the only recoverable backup.

Similarly, application credentials should not automatically provide unrestricted deletion access to all backup copies.

---

# Backup Verification

Backup creation shall be followed by appropriate verification.

Verification should establish, where supported:

* backup completion
* backup integrity
* expected backup availability
* expected retention
* recoverability
* absence of corruption

A backup that cannot be recovered shall not be considered an effective backup.

---

# Restore Testing

Restore testing is mandatory.

Initial Production recovery testing shall occur:

> **At least once per quarter.**

Restore testing shall verify:

* backup accessibility
* data integrity
* restoration process
* application/database compatibility
* migration compatibility
* recovery timing
* operational documentation
* access to required recovery secrets/configuration

Restore tests should be performed in a controlled non-production recovery environment whenever practical.

---

# Restore Test Records

Each restore test should record:

* test date
* backup/recovery point used
* application version
* database/schema version
* restoration result
* recovery duration
* validation result
* issues discovered
* corrective actions

Restore-test findings shall feed back into Disaster Recovery and operational procedures.

---

# Recovery Time Objective

The Initial Production RTO is:

> **Maximum 4 hours.**

The backup architecture shall provide the recovery inputs necessary for the Disaster Recovery process to restore service within this target under the defined recovery assumptions.

RTO includes the time required to:

* obtain the required recovery resources
* restore the database
* restore or redeploy the application
* restore required configuration
* validate application/database compatibility
* perform health checks
* return the platform to operational service

---

# Recovery Point Objective

The Initial Production RPO is:

> **Maximum 24 hours.**

This means the maximum acceptable amount of committed database data potentially lost due to a qualifying catastrophic recovery event is 24 hours.

Where provider capabilities permit a tighter RPO, FluxDine may adopt it without weakening the architecture.

---

# Backup Monitoring

Backup operations shall be monitored for:

* successful execution
* failed execution
* missed execution
* backup availability
* retention compliance
* verification status
* storage/resource conditions
* restore-test failures

Backup failures shall generate operational attention according to the Monitoring and Incident Response specifications.

---

# Backup Failure Handling

When a backup operation fails:

1. The failure shall be recorded.
2. Existing successful backups shall be preserved.
3. The operation should retry where supported.
4. The failure shall be surfaced to operations.
5. RPO exposure shall be evaluated.
6. Corrective action shall be taken when required.

A failed backup must never silently replace or destroy the last known-good recovery point.

---

# RPO Violation Handling

If backup failure or provider degradation causes the effective recovery point to exceed the 24-hour RPO:

* the condition shall be treated as an operational risk
* the failure shall be investigated
* corrective action shall be initiated
* recovery exposure shall be communicated to authorized operators where appropriate

The objective is to restore compliant backup coverage as quickly as possible.

---

# Disaster Recovery Relationship

Backup and Disaster Recovery are separate but connected capabilities.

```text
Backup Strategy
      |
      v
Recoverable Data
      |
      v
Disaster Recovery Procedure
      |
      v
Restoration
      |
      v
Validation
      |
      v
Service Recovery
```

This document defines how recoverable data and recovery inputs are maintained.

The Disaster Recovery specification defines how they are used during an actual recovery event.

---

# Backup Security

Backup systems shall:

* enforce authorization
* protect tenant data
* encrypt sensitive information
* protect backup credentials
* preserve backup integrity
* support access auditing
* prevent unauthorized deletion

Production backup access shall be limited to authorized personnel and approved recovery systems.

---

# Tenant Isolation

Backups may contain data belonging to multiple FluxDine tenants because the current architecture uses a shared database/shared-schema model.

Backup handling shall therefore preserve the security boundary of the entire production dataset.

Tenant-specific restoration, if ever required, shall be performed through controlled recovery procedures rather than granting tenants direct access to shared backup storage.

---

# Provider Failure

The backup strategy shall account for failure of:

* application hosting
* database provider
* object storage
* supporting infrastructure

Critical database recovery capability shall not depend exclusively on the continued availability of the primary database instance.

Object-storage recovery requirements shall be evaluated according to the business criticality of the stored objects.

---

# Backup and Security Incidents

Backups may be required during security incidents.

When compromise is suspected:

* backup integrity shall be evaluated
* recovery points shall be assessed
* known-compromised recovery points shall not automatically be treated as clean
* credentials required for recovery shall be rotated where necessary
* restoration shall occur only from an appropriate trusted recovery point

Backup restoration shall therefore be coordinated with Security and Disaster Recovery procedures.

---

# Backup and Application Rollback

Application rollback does not automatically require database restoration.

Examples:

* A faulty frontend deployment may require only application rollback.
* A compatible backend deployment failure may require application redeployment.
* A destructive database migration may require database recovery.
* Data corruption may require restoration from a valid recovery point.

The recovery action shall be selected based on the failure type.

---

# Backup and Database Rollback

Database rollback is inherently more sensitive than application rollback.

Before restoring a database:

* the failure cause shall be understood where possible
* the target recovery point shall be selected
* application compatibility shall be verified
* data loss implications shall be understood
* required downtime shall be evaluated

Database restoration shall follow the Disaster Recovery procedure.

---

# Backup and Production Deployment

Production deployment shall not proceed when required backup/recovery prerequisites are knowingly unavailable for a materially risky database operation.

For normal application-only deployments, existing backup readiness remains the responsibility of the operational environment.

Deployment and backup governance shall remain coordinated but independent.

---

# Compliance and Retention

Backup retention shall support applicable:

* legal requirements
* regulatory requirements
* contractual requirements
* organizational policies
* security requirements

Where multiple retention requirements exist, the stricter applicable requirement shall be evaluated.

No retention policy may reduce Initial Production's mandatory 30-day recoverable database history without approved architectural change.

---

# Engineering Rules

## Rule BACKUP-001

Every Initial Production database shall have automated backup protection.

---

## Rule BACKUP-002

The database backup strategy shall support a maximum 24-hour RPO.

---

## Rule BACKUP-003

Initial Production shall maintain at least 30 days of recoverable database backup history.

---

## Rule BACKUP-004

Critical database recovery capability shall not exist exclusively within the primary database failure domain.

---

## Rule BACKUP-005

Backups containing sensitive information shall be encrypted during storage and transmission.

---

## Rule BACKUP-006

Backup completion and recoverability shall be verified.

---

## Rule BACKUP-007

Restore testing shall occur at least quarterly.

---

## Rule BACKUP-008

Initial Production recovery procedures shall support a maximum 4-hour RTO.

---

## Rule BACKUP-009

Backup failures shall be detected, recorded, and surfaced operationally.

---

## Rule BACKUP-010

Failed backups shall not overwrite successful recovery points.

---

## Rule BACKUP-011

Application source shall remain recoverable through the authoritative GitHub repository and known-good deployment history.

---

## Rule BACKUP-012

Secrets shall never be committed to source control as a backup mechanism.

---

## Rule BACKUP-013

Database recovery shall account for application and migration compatibility.

---

## Rule BACKUP-014

R2 object recovery and retention shall be explicitly defined according to object criticality.

---

## Rule BACKUP-015

Tenant isolation shall be preserved during backup access and restoration.

---

## Rule BACKUP-016

Backup access shall require authorization and follow least-privilege principles.

---

## Rule BACKUP-017

Backup and Disaster Recovery shall remain separate but coordinated capabilities.

---

## Rule BACKUP-018

This document is the authoritative Backup Strategy specification for the FluxDine platform.

---

# Architecture Decision Records

## ADR-BACKUP-001 — Automated Production Database Protection

Initial Production database data requires automated backup protection.

---

## ADR-BACKUP-002 — 24-Hour Maximum RPO

Initial Production shall maintain a maximum database Recovery Point Objective of 24 hours.

---

## ADR-BACKUP-003 — 4-Hour Maximum RTO

Initial Production shall target a maximum Recovery Time Objective of 4 hours.

---

## ADR-BACKUP-004 — 30-Day Recoverable History

Initial Production shall maintain at least 30 days of recoverable database backup history.

---

## ADR-BACKUP-005 — Independent Database Recovery

Critical database recovery capability shall not depend exclusively on the primary database failure domain.

---

## ADR-BACKUP-006 — Provider-Appropriate Backup Mechanisms

FluxDine shall use backup mechanisms appropriate to each infrastructure provider rather than requiring an identical backup implementation across all systems.

---

## ADR-BACKUP-007 — Quarterly Restore Testing

Backup reliability shall be validated through restore testing at least quarterly.

---

## ADR-BACKUP-008 — Application Source Recovery Through Git

GitHub remains the authoritative application source of truth and provides versioned application recovery.

---

## ADR-BACKUP-009 — Secrets Excluded from Source Backups

Secrets shall be recovered through approved secrets-management mechanisms rather than source control.

---

## ADR-BACKUP-010 — Migration-Aware Recovery

Database recovery shall account for schema and application-version compatibility.

---

## ADR-BACKUP-011 — R2 Recovery Strategy

R2 shall have an explicit retention and recovery strategy, while full cross-provider object duplication remains optional at Initial Production scale.

---

## ADR-BACKUP-012 — Backup/Disaster Recovery Separation

Backup maintenance and Disaster Recovery execution are separate capabilities governed by separate specifications.

---

## ADR-BACKUP-013 — Authoritative Specification

This document is the authoritative Backup Strategy specification for the FluxDine platform.

---

# Appendix A — Protected Assets

| Asset                       | Recovery Mechanism                       |
| --------------------------- | ---------------------------------------- |
| Tenant Data                 | Database Backup                          |
| Restaurant Data             | Database Backup                          |
| Branch Data                 | Database Backup                          |
| Orders                      | Database Backup                          |
| Reservations                | Database Backup                          |
| Customers                   | Database Backup                          |
| Payment Records             | Database Backup                          |
| Subscription Records        | Database Backup                          |
| Configuration Records       | Database Backup / Configuration Recovery |
| Audit Records               | Database Backup                          |
| R2 Objects                  | R2 Retention / Recovery Strategy         |
| Application Source          | GitHub                                   |
| Deployment Configuration    | GitHub / Vercel Configuration            |
| Secret Values               | Approved Secrets Management              |
| Error/Observability Context | Sentry / Provider Retention              |

---

# Appendix B — Initial Production Recovery Targets

| Requirement               | Target             |
| ------------------------- | ------------------ |
| Maximum RPO               | 24 hours           |
| Maximum RTO               | 4 hours            |
| Minimum DB Backup History | 30 days            |
| Restore Testing           | At least quarterly |

---

# Appendix C — Backup Lifecycle

```text
Protected Production Data
          |
          v
      Backup / Recovery
          |
          v
       Encryption
          |
          v
   Independent Storage
          |
          v
      Verification
          |
          v
       Monitoring
          |
          v
       Retention
          |
          v
     Restore Testing
          |
          v
   Disaster Recovery
```

---

# Appendix D — Recovery Inputs

A successful recovery may require:

```text
Known-Good Application Commit
Database Recovery Point
Database Schema/Migration State
Deployment Configuration
Environment Configuration
Required Secrets
R2 Recovery Information
Domain Configuration
Provider Access
Health Verification
Operational Runbook
```

Recovery documentation shall identify required inputs without exposing secret values.

---

# Appendix E — Current Provider Recovery Model

| Provider/System | Role                | Recovery Consideration                                |
| --------------- | ------------------- | ----------------------------------------------------- |
| Turso           | Primary Database    | Automated backup/recovery capability must satisfy RPO |
| Cloudflare R2   | Object Storage      | Retention and object recovery strategy required       |
| GitHub          | Source Control      | Versioned source recovery                             |
| Vercel          | Application Hosting | Known-good deployment/redeployment                    |
| Sentry          | Observability       | Incident and error context                            |
| Resend          | Email               | Provider reconfiguration/retry capability             |
| Cloudflare DNS  | DNS                 | DNS configuration recovery                            |

---

# Appendix F — Reserved Future Capabilities

Future backup capabilities may include:

```text
Point-in-Time Database Recovery
Continuous Database Backup
Cross-Region Database Replication
Immutable Backup Storage
Air-Gapped Backups
Cross-Provider Object Replication
Automated Recovery Validation
Automated Disaster Recovery Drills
Backup Integrity Attestation
Compliance Archive Storage
Multi-Region Recovery
```

These capabilities are not mandatory for the current Initial Production architecture unless separately approved.

---

# References

* Deployment Specification
* Environment & Secrets Strategy
* Environment Variables
* CI/CD Pipeline
* Monitoring
* Logging
* Database Engineering Specifications
* Disaster Recovery
* Security Architecture
* Source Control Standards

---

# Revision History

| Version | Date            | Author               | Description                                                                                                                                                                                                                   |
| ------- | --------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.0     | Initial Release | FluxDine Engineering | Initial authoritative Backup Strategy specification                                                                                                                                                                           |
| 1.1     | 2026-09-12      | FluxDine Engineering | Aligned backup architecture with Turso Initial Production, 24-hour RPO, 4-hour RTO, 30-day recoverable database history, independent recovery, R2 recovery, GitHub/Vercel application recovery, and quarterly restore testing |

```
