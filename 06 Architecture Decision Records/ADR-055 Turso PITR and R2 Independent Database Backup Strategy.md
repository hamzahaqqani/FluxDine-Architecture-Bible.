# ADR-055 — Turso PITR and R2 Independent Database Backup Strategy

## Status
Accepted

## Decision Owners
FluxDine Architecture Team

## Context

FluxDine Initial Production uses Turso as the database provider with a Shared Database / Shared Schema topology. PostgreSQL is a future migration target and is not the Initial Production database.

The current Initial Production application deployment is the Vercel project `fluxdine-staging`. Despite that project name, the deployment occupies the Initial Production role. It is not a conventional disposable staging environment.

Cloudflare R2 is the application object-storage provider. The existing R2 bucket `fluxdine-staging` is used for normal File Storage. Database backups must not use that application bucket.

Architecture requirements for Initial Production database recovery are:

- Recovery Point Objective (RPO): maximum 24 hours
- Recovery Time Objective (RTO): maximum 4 hours
- Minimum recoverable database backup history: 30 days
- Quarterly restore testing
- Secrets must never be committed to Git
- Tenant isolation remains mandatory at API, service, and data-access boundaries

A database restore of the shared schema restores every tenant contained in that database. Recovery is therefore a platform-wide operation.

A dedicated recovery strategy is required because:

- Turso native Point-in-Time Recovery (PITR) remains inside the Turso provider and failure domain.
- PITR retention is plan-dependent and must not be assumed to be 30 days.
- The Architecture Bible requires an independently recoverable copy of critical database recovery data.
- Application File Storage credentials and the `fluxdine-staging` bucket must not be able to overwrite or delete database recovery copies.
- Vercel Hobby Cron is not a reliable scheduler for a strict 24-hour RPO.

This ADR establishes the authoritative Initial Production database backup and recovery architecture. Phase 07 implementation shall follow this decision. This ADR does not change the Initial Production database topology and does not rewrite ADR-003.

## Decision

FluxDine Initial Production shall use two complementary recovery layers:

1. **Primary recovery mechanism:** Turso native Point-in-Time Recovery (PITR) for recent incidents that fall within the actual PITR retention window of the current Turso plan.
2. **Independent recovery copy:** Automated full logical database dumps stored in a dedicated, private Cloudflare R2 backup bucket.
3. **Backup runner:** GitHub Actions, as operational infrastructure, not as application request-path functionality.

The following is **not** the authoritative Initial Production database backup architecture:

Vercel Hobby Cron → Vercel Function → Turso dump → R2.

### Architecture

```text
                    FLUXDINE INITIAL PRODUCTION

                       Application
                            |
                            v
                  +--------------------+
                  | Turso Shared DB    |
                  | Shared Schema      |
                  +--------------------+
                     |            |
             continuous PITR      | full logical dump
             (primary recent      | (independent copy)
              recovery)           |
                     |            |
                     v            v
              +----------+   +----------------------+
              | Turso    |   | GitHub Actions       |
              | PITR     |   | Twice daily          |
              +----------+   +----------+-----------+
                     |                  |
                     |                  v
                     |        +----------------------+
                     |        | Private R2 Backup    |
                     |        | fluxdine-database-   |
                     |        | backups              |
                     |        +----------------------+
                     |                  |
                     |                  v
                     |        Quarterly restore test
                     |                  |
                     |                  v
                     |        Throwaway Turso DB
                     |
                     +--> restore always creates a NEW database
                          then controlled Vercel cutover
                          Production is never overwritten in place
```

PITR is the primary recent-recovery mechanism. R2 holds the independent recovery copy. GitHub Actions performs scheduled full dumps. Production is never overwritten directly during restore. This is not automatic failover.

### Turso PITR

Turso PITR is the primary recovery mechanism for recent accidental change or corruption that falls within the available PITR window.

PITR retention is plan-dependent. Official Turso documentation currently describes Free (24 hours), Developer (10 days), Scaler (30 days), and Pro (90 days). The Architecture Bible must not assume 30-day PITR. Retention must be verified against the actual Turso plan in use.

PITR alone is not the guaranteed 30-day recovery-history mechanism. PITR alone is not the independent recovery copy, because it remains within the Turso provider and failure domain.

PITR restoration creates a **new** Turso database. It does not overwrite the live production database in place.

PITR recovery flow:

```text
PITR timestamp
    → new Turso database
    → compatibility verification
    → controlled Vercel database configuration/cutover
    → health verification
    → old database retained until recovery is confirmed
    → eventual cleanup under controlled authorization
```

### Independent R2 recovery copy

FluxDine shall maintain full logical database dumps outside the primary Turso database service in a dedicated private Cloudflare R2 backup bucket.

Recommended bucket name: `fluxdine-database-backups`.

That bucket must be separate from `fluxdine-staging`, which remains the application File Storage bucket.

The backup bucket must:

- be private
- have no public access
- have no public custom domain
- not be exposed through `NEXT_PUBLIC_*` variables
- not be accessible by normal application File Storage credentials
- use separate credentials
- use least-privilege access
- retain at least 30 days of successful, verified backups

### Backup frequency

Independent full database dumps shall occur approximately twice per day.

Example GitHub Actions schedule: `06:00 UTC` and `18:00 UTC`.

The exact cron expression may be adjusted operationally. The architectural requirement is twice-daily independent full dumps.

Rationale:

- 24-hour RPO is the maximum allowed data-loss window.
- Twice-daily verified dumps provide meaningful margin below that maximum.
- Vercel Hobby Cron is limited to once daily and has documented execution jitter within the scheduled hour.
- A once-daily Hobby schedule can therefore produce an interval longer than 24 hours.
- Vercel Hobby Cron is not the authoritative database backup scheduler.

This decision does not require Vercel Pro. This decision does not introduce a dedicated worker platform merely for backups.

### Dump format and method

Dumps shall be full logical SQL dumps.

Preferred implementation: Turso CLI `.dump` executed from GitHub Actions.

The dump shall be complete, transactionally consistent, compressed, SHA-256 hashed, accompanied by a manifest, and uploaded to the private backup bucket.

HTTP `GET /dump` against the Turso database HTTP API may be used as an implementation fallback. FluxDine must not expose a public application backup endpoint.

Rejected:

- hand-written SELECT-all-table exports
- custom incomplete dump formats
- publicly exposed `/api/backup` or equivalent application routes
- reuse of application R2 File Storage code and credentials for database backups

### Backup object structure

Recommended object layout:

```text
turso/{database-name}/{UTC-date}/{backup-id}/dump.sql.gz
turso/{database-name}/{UTC-date}/{backup-id}/manifest.json
```

The manifest should contain, where available:

- backup ID
- UTC backup timestamp
- Turso database identifier or hostname (never tokens)
- Git SHA / application release
- migration state or migration identifier if queryable
- compressed byte size
- uncompressed byte size
- SHA-256 checksum
- compression algorithm
- backup status
- verification status

Values that cannot be known at implementation time are not required.

### Retention

The 30-day recoverable-history guarantee comes from the R2 backup layer unless the actual Turso plan independently provides the same or greater PITR retention.

R2 must retain at least 30 days of **successful, verified** database backups. Recommended operational retention is 35–40 days, providing buffer above the minimum.

Recommended lifecycle principles:

- successful backup objects retained at least 30 days
- operational recommendation: 35–40 days
- incomplete multipart uploads should be cleaned up
- retention cleanup must never intentionally remove the last usable backup required to satisfy the recovery requirement

Cloudflare R2 supports object lifecycle expiration and abort of incomplete multipart uploads. Those capabilities should be configured on the dedicated backup bucket. This ADR does not claim additional R2 features unless they are actually configured.

### Credential separation

Application R2 credentials:

- used by FluxDine application File Storage
- access only the application storage bucket and resources

Backup R2 credentials:

- used only by the GitHub Actions backup workflow
- access only the dedicated backup bucket
- least privilege
- no unnecessary Delete permission
- lifecycle should handle normal expiration where practical

Turso application credentials:

- remain in Vercel Production
- used by application runtime

Backup Turso credentials:

- stored only as GitHub Actions production secrets
- preferably a read-only or dump-capable credential if technically supported and verified
- never committed to Git
- never placed in application source code

Restore credentials:

- controlled human/operator or break-glass credentials
- not available to Preview, Development, or ordinary CI

Preview, Development, and Testing **must not** receive production backup credentials.

### Backup integrity

A backup that merely exists as an object is not successful.

Minimum successful-backup process:

1. Generate the full logical dump.
2. Compress the dump.
3. Calculate SHA-256.
4. Upload the dump to private R2.
5. Upload the manifest.
6. Verify uploaded object existence.
7. Verify object size/metadata.
8. Verify checksum where practical.
9. Mark the backup successful only after verification.
10. Produce an operational failure signal if verification fails.

Status values:

- **SUCCESS** — dump generated, uploaded, and verified.
- **PARTIAL FAILURE** — dump or upload started but not verified.
- **FAILURE** — no usable independent copy from the run.

### Failure handling

If Turso dump generation fails: the backup run fails; no successful backup is recorded; an operational alert is generated.

If R2 upload fails: the backup run fails; incomplete multipart data must not be treated as a valid backup.

If the runner times out: the backup is not marked successful; incomplete artifacts are cleaned up by lifecycle or controlled cleanup; the next scheduled run must attempt a fresh backup.

If a backup object exists but checksum or size verification fails: the backup is invalid and must not count toward the 30-day recoverable history.

If monitoring fails: the backup workflow must still fail safely; monitoring failure must not turn an invalid backup into a successful backup.

### Restore strategy

Never restore a database dump directly onto the live production database. All restores must create a **new** database first.

R2 dump restore flow:

```text
Verified R2 dump
    → new Turso database
    → schema/migration compatibility verification
    → application compatibility verification
    → controlled Vercel database cutover
    → health verification
    → operational validation
    → old database retained until recovery is confirmed
```

PITR restore flow is the same cutover model, seeded from a PITR timestamp rather than from an R2 dump.

### Recovery scenarios

| Scenario | Preferred mechanism |
|---|---|
| Recent accidental data change | Turso PITR, if within the actual PITR window |
| Known corruption within the PITR window | Turso PITR |
| Corruption outside the PITR window | Latest verified R2 dump |
| Complete Turso database loss | R2 dump used to create a new database |
| Turso provider, control-plane, or regional failure | R2 independent copy, subject to availability of a suitable restoration target |
| Turso outage where the database cannot currently be accessed | R2 is a recovery copy, not live read-through or failover storage |

This architecture does not provide automatic failover.

### Shared Database / Shared Schema

FluxDine Initial Production uses Shared Database / Shared Schema. A database restore is a platform-wide recovery operation. It restores all tenants contained in the database.

There is no tenant-scoped PITR or tenant-scoped database restore mechanism in this decision. Tenant-specific recovery, if ever required, would be a separate future architectural capability.

Tenant isolation remains mandatory on the restored system. Isolation does not mean per-tenant backup restore.

### Application and database compatibility

A database backup represents a point-in-time combination of schema, data, migration state, and application release context. The manifest should preserve enough information to identify that state.

Recovery must use the application version compatible with the restored database, or a controlled forward migration process. Never migrate the restored database backward. Do not assume the latest application build can safely run against every historical backup.

Application rollback and database restore remain separate operations.

### Quarterly restore testing

At least quarterly, FluxDine must restore a verified R2 backup into an isolated throwaway Turso database.

The test must:

- never overwrite Initial Production
- never change the production Vercel database URL
- verify the database can be created or restored
- verify expected schema/migration state
- verify representative data exists
- verify application compatibility where practical
- destroy the temporary recovery database after testing
- record the backup ID and result

This is a recovery validation requirement, not a production deployment.

### Vercel

Vercel remains the application hosting platform. Vercel Hobby Cron remains acceptable for other scheduled application workloads currently in use.

Vercel Hobby Cron → Vercel Function → Turso dump → R2 is not the authoritative database backup architecture because of Hobby once-daily scheduling jitter, insufficient margin against a strict 24-hour RPO, Function execution and memory limits, request/response body limits, backup secrets sitting closer to application runtime, and dump growth beyond serverless practicality.

A future dedicated backup runner may be introduced if database size or operational requirements justify it. This ADR does not introduce that runner now.

### GitHub Actions

GitHub Actions is the Initial Production scheduled backup runner.

The backup workflow is separate from ordinary CI. Ordinary CI must not receive production Turso or R2 backup secrets. The backup workflow is a narrowly scoped production operational workflow and must not weaken normal pull-request security boundaries.

### Cloudflare Cursor plugin

The Cloudflare Cursor integration does not provide sufficient capability to independently provision the entire production backup architecture. It may assist with limited Cloudflare resource operations. It does not replace human-controlled infrastructure credentials and authorization.

The current integration does not independently provide the full set of capabilities required to create and manage all R2 credentials, configure all retention and lifecycle controls, fully manage backup object operations, and establish the complete production security boundary.

Cursor may implement repository code, workflows, and documentation after this ADR is locked. Cursor must not commit secrets. Human authorization remains required for sensitive production infrastructure and credentials.

"Cursor + Cloudflare plugin" is not a required infrastructure dependency.

### Infrastructure automation

Initial Production uses a hybrid model.

Human-controlled infrastructure setup:

- dedicated R2 backup bucket
- scoped R2 credentials
- Turso dump credential
- GitHub Actions production secrets
- required retention/lifecycle configuration

Git-controlled implementation:

- GitHub Actions backup workflow
- backup manifest structure
- verification logic
- operational documentation
- monitoring integration

Cursor may implement the Git-controlled portion after this ADR is locked and must not commit secrets.

Terraform is not required for Initial Production solely for this backup architecture.

### RPO and RTO

**RPO:** maximum 24 hours. The twice-daily verified R2 backup schedule is designed to provide meaningful margin below this requirement. PITR may provide a tighter recovery point when the incident falls inside the actual PITR window.

**RTO:** maximum 4 hours. This is an operational recovery target, not an automatic failover SLA. RTO depends on recovery mechanism availability, human authorization, new database creation, application/database compatibility, Vercel configuration cutover, health verification, and operational validation.

### Non-goals

This ADR does not introduce:

- PostgreSQL
- database-per-service architecture
- Kubernetes
- Redis
- Kafka
- Temporal
- dedicated worker clusters
- multi-region automatic failover
- multi-cloud backup replication
- automatic DNS failover
- automatic database failover
- tenant-scoped restore
- a Vercel Pro requirement
- public database backup endpoints

These may be reconsidered later based on scale and operational requirements.

## Alternatives Considered

### Alternative 1 — Turso PITR only

Rejected. PITR retention is plan-dependent and must not be assumed to be 30 days. PITR remains inside the Turso failure domain and cannot satisfy the independent recovery-copy requirement.

### Alternative 2 — Vercel Hobby Cron to a Vercel Function dump into R2

Rejected as the authoritative Initial Production backup runner. Hobby once-daily jitter can exceed a strict 24-hour interval. Function duration, memory, and body-size limits are a poor fit for growing dumps. Backup secrets would sit closer to application runtime.

### Alternative 3 — Prefix inside the `fluxdine-staging` application bucket

Rejected. Application File Storage credentials can put, get, and delete objects in that bucket. A prefix is not a security boundary for database recovery copies.

### Alternative 4 — Terraform or a dedicated worker platform for Initial Production backups

Rejected for the current stage. One dedicated bucket, scoped credentials, and a scheduled GitHub Actions workflow satisfy the recovery objectives without introducing Terraform, Kubernetes, or a worker cluster.

## Rationale

The Architecture Bible already requires automated database protection, a 24-hour maximum RPO, a 4-hour maximum RTO, at least 30 days of recoverable history, an independent recovery copy, and quarterly restore testing.

Turso PITR is the correct primary mechanism for recent, in-window incidents because it is created continuously at commit and restores to a timestamp. It is not sufficient alone.

Logical SQL dumps using the official Turso dump/restore path provide a portable independent copy that can seed a new database. GitHub Actions can run twice daily without a Vercel Pro upgrade and without placing dump execution on the application request path.

A dedicated private R2 bucket keeps recovery copies out of the application File Storage trust boundary while remaining inside the already-selected object-storage provider.

## Consequences

### Positive Consequences

- Meets the 30-day independent backup requirement through R2, without assuming 30-day Turso PITR.
- Improves recovery options beyond Turso-only recovery.
- Keeps backup infrastructure separate from application storage.
- Avoids an unnecessary Vercel Pro upgrade solely for backup scheduling.
- Avoids introducing a worker platform.
- Supports quarterly restore validation without overwriting Initial Production.
- Preserves future migration flexibility, including a later PostgreSQL migration, because dumps and cutover remain database-replacement operations rather than in-place overwrites.

### Negative Consequences

- GitHub Actions holds narrowly scoped production backup credentials.
- The backup workflow requires operational maintenance.
- R2 retention and lifecycle must be configured correctly.
- Restore remains an operational process that includes human authorization.
- Shared-schema restores affect all tenants.
- Large future databases may require a dedicated backup runner.
- Human authorization remains part of disaster recovery.

## Architectural Impact

- **Database:** Turso Shared Database / Shared Schema remains Initial Production. Restore always creates a new database. No tenant-scoped restore.
- **Application:** No public backup endpoint. Application File Storage and backup storage remain separate. Application Turso credentials remain in Vercel Production.
- **Hosting:** Vercel remains the application host. Vercel Hobby Cron is not the database backup scheduler.
- **Object storage:** Dedicated private R2 backup bucket, separate from `fluxdine-staging`.
- **CI/CD:** Production backup workflow is distinct from ordinary CI and must not receive backup secrets in pull-request CI.
- **Security:** Least-privilege credential separation; production backup secrets excluded from Development, Testing, and Preview.
- **Operations:** Twice-daily dumps, verification, alerting on FAILURE/PARTIAL FAILURE, quarterly throwaway restore tests, 4-hour operational RTO runbook.
- **ADR-003:** Not rewritten. ADR-003 remains historically recorded. This ADR does not adopt database-per-service as Initial Production topology and does not create a contradictory new database architecture.

## Related Documents

- ADR-002 — Multi-Tenant SaaS Architecture
- ADR-003 — Database per Service (historical record; not rewritten by this ADR)
- ADR-028 — Centralized File Storage Service
- ADR-041 — Tenant Isolation
- ADR-049 — Background Processing
- ADR-050 — Scheduled Jobs
- ADR-053 — Infrastructure Independence
- `01 Core Architecture/05 Database Architecture & Multi-Tenant Data Model.md`
- `02 Engineering Specifications/Infrastructure/00 Environment & Secrets Strategy.md`
- `02 Engineering Specifications/Infrastructure/01 Deployment Specification.md`
- `02 Engineering Specifications/Infrastructure/03 CI-CD Pipeline.md`
- `02 Engineering Specifications/Infrastructure/04 Monitoring.md`
- `02 Engineering Specifications/Infrastructure/06 Backup Strategy.md`
- `02 Engineering Specifications/Infrastructure/07 Disaster Recovery.md`
- `02 Engineering Specifications/Infrastructure/08 Scaling Strategy.md`
- `02 Engineering Specifications/Database/07 Database Migration Strategy.md`
- Turso Point-in-Time Recovery: https://docs.turso.tech/features/point-in-time-recovery
- Turso database dump (`db shell` `.dump`): https://docs.turso.tech/cli/db/shell
- Turso create from dump: https://docs.turso.tech/cli/db/create
- Cloudflare R2: https://developers.cloudflare.com/r2/
- Cloudflare R2 object lifecycles: https://developers.cloudflare.com/r2/buckets/object-lifecycles/
- GitHub Actions: https://docs.github.com/en/actions

## Revision History

| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-09-12 | Initial acceptance of Turso PITR and R2 independent database backup architecture. |
