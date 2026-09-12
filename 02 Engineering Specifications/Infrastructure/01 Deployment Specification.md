
# 02 Engineering Specificatio
# Infrastructure

# 01 — Deployment Specification

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-INF-001 |
| **Document Name** | Deployment Specification |
| **Version** | 1.1 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | Backend Engineering Specifications<br>Frontend Engineering Specifications<br>Database Engineering Specifications<br>Security Architecture<br>Infrastructure Architecture<br>Environment & Secrets Strategy |
| **Referenced By** | Environment Variables<br>CI/CD Pipeline<br>Monitoring<br>Logging<br>Backup Strategy<br>Disaster Recovery<br>Scaling Strategy |

---

# Dependencies

This specification depends upon:

- Backend Engineering Specifications
- Frontend Engineering Specifications
- Database Engineering Specifications
- Security Architecture
- Infrastructure Architecture
- Environment & Secrets Strategy

These specifications establish the application, security, infrastructure, and environment principles that deployment must satisfy.

This specification defines how validated FluxDine software is packaged, promoted, deployed, verified, and recovered across the supported environment roles.

Deployment implementation details are further specified by the CI/CD Pipeline, Environment Variables, Monitoring, Logging, Backup Strategy, and Disaster Recovery specifications.

---

# Referenced By

This specification is referenced by:

- Environment Variables
- CI/CD Pipeline
- Monitoring
- Logging
- Backup Strategy
- Disaster Recovery
- Scaling Strategy
- Operations
- Engineering Release Processes

---

# Document Status

| Item | Value |
|------|-------|
| Status | Pending Approval |
| Approval | Requires Architecture/Engineering Review |
| Implementation | Architecture Specification |
| Version | 1.1 |
| Last Updated | 2026-09-12 |

Version 1.1 updates the original deployment specification to align it with the current FluxDine infrastructure architecture, environment model, production governance, managed infrastructure providers, and database-aware release strategy.

---

# Purpose

This document defines the authoritative deployment architecture for the FluxDine platform.

The Deployment Specification establishes how application releases move from validated source code into FluxDine environments while maintaining:

- Reliability
- Security
- Repeatability
- Traceability
- Environment isolation
- Deployment verification
- Controlled production release
- Rollback capability
- Database compatibility
- Operational recoverability

This document defines deployment architecture and deployment governance.

It does not replace the detailed specifications for CI/CD, environment variables, monitoring, logging, backup, disaster recovery, or database migrations.

---

# Scope

This specification defines:

- Deployment architecture
- Deployment environments
- Environment roles
- Current infrastructure mapping
- Deployment workflow
- Release promotion
- Production release governance
- Application deployment
- Database deployment coordination
- Artifact management
- Configuration boundaries
- Secret boundaries
- Deployment verification
- Health verification
- Rollback strategy
- Database rollback considerations
- Zero-downtime objectives
- Deployment failure handling
- Security requirements
- Release traceability
- Engineering deployment rules

---

# Out of Scope

This specification does not define the detailed implementation of:

- CI/CD pipeline stages
- Environment variable catalogs
- Secret values
- Secret storage implementation
- Database schema design
- Database migration file contents
- Monitoring configuration
- Logging implementation
- Backup implementation
- Disaster recovery procedures
- DNS automation
- Custom domain automation
- Payment gateway implementation
- Application business logic
- Tenant authorization rules

These capabilities are specified separately.

---

# Deployment Philosophy

FluxDine deployments shall be:

- Automated where practical
- Repeatable
- Version controlled
- Traceable
- Secure
- Observable
- Verifiable
- Recoverable
- Environment isolated
- Database aware
- Zero-downtime oriented where practical

Deployment automation shall reduce operational error without removing required production authorization controls.

Production deployment execution may be automated, but production release authorization shall remain explicitly controlled.

Manual production changes outside the defined deployment process shall not be used as the normal operating model.

---

# Core Deployment Principles

## Principle DEP-001 — Version-Controlled Source

Every deployment shall originate from source code stored in the authorized Git repository.

Deployments from unmanaged local source directories shall not be treated as production releases.

---

## Principle DEP-002 — Traceable Releases

Every deployment shall be traceable to:

- Git commit
- Source branch
- Build
- Deployment
- Environment
- Deployment timestamp
- Release result

The deployed application version must be identifiable after deployment.

---

## Principle DEP-003 — Validated Source

Only source code that has passed the required validation gates may be promoted to a deployment environment.

Validation may include:

- Compilation
- Automated tests
- Static analysis
- Security checks
- Dependency checks
- Application health verification

Detailed CI/CD validation requirements are defined by the CI/CD Pipeline specification.

---

## Principle DEP-004 — Immutable Release Artifacts

Deployment artifacts shall be treated as immutable after successful creation.

A release artifact shall not be manually modified between validation and deployment.

If source changes are required, a new commit and release artifact shall be created.

---

## Principle DEP-005 — Environment Isolation

Each environment role shall use its own appropriate configuration and resources.

A deployment to one environment shall not unintentionally modify another environment.

Production resources shall never be used as disposable testing resources.

---

## Principle DEP-006 — Secret Isolation

Secrets shall remain outside source control.

Each environment shall receive only the secrets required for that environment.

Production secrets shall never be exposed to development or test execution unless explicitly required and authorized.

---

## Principle DEP-007 — Database-Aware Deployment

Application releases and database changes shall be coordinated.

A release shall not assume that database changes are automatically reversible.

Database migrations shall follow the Database Migration Strategy.

---

## Principle DEP-008 — Verified Deployment

A deployment shall not be considered successful solely because the deployment provider reports completion.

The deployed application shall undergo appropriate post-deployment verification.

---

## Principle DEP-009 — Controlled Production Release

Production deployment shall require explicit authorized approval.

Passing automated validation does not by itself constitute production authorization.

---

## Principle DEP-010 — Recoverability

Every production release shall have a defined recovery path.

Recovery may consist of:

- Application rollback
- Configuration correction
- Roll-forward
- Database restoration
- Disaster recovery procedures

The correct recovery method depends on the failure type.

---

# Deployment Architecture

The general FluxDine deployment lifecycle is:

```text
Developer
    |
    v
Git Repository
    |
    v
Continuous Integration
    |
    v
Validation
    |
    v
Build
    |
    v
Release Artifact
    |
    v
Environment Deployment
    |
    v
Health Verification
    |
    v
Release Validation
    |
    +----------------------+
    |                      |
    v                      v
Further Promotion      Recovery
    |
    v
Production Approval
    |
    v
Production Deployment
    |
    v
Production Verification
````

Every deployment shall originate from version-controlled source code.

---

# Current FluxDine Deployment Architecture

The current FluxDine application deployment is based on a Next.js application hosted on Vercel with managed external infrastructure services.

The current production architecture is:

```text
GitHub
   |
   v
FluxDine Application Repository
   |
   v
Vercel Deployment
   |
   +-------------------+
   |                   |
   v                   v
Next.js Application   API / Server Execution
   |
   +---------+---------+---------+---------+
   |         |         |         |         |
   v         v         v         v         v
 Turso      R2       Resend    Sentry   Cloudflare DNS
 Database  Storage    Email   Monitoring    DNS
```

The application remains responsible for business logic, tenant resolution, authorization, and integration orchestration.

External infrastructure providers shall not directly own FluxDine business rules.

---

# Current Infrastructure Provider Mapping

The current initial-production deployment uses the following provider roles:

| Capability          | Current Provider | Role                                           |
| ------------------- | ---------------- | ---------------------------------------------- |
| Application Hosting | Vercel           | Next.js application deployment and execution   |
| Database            | Turso            | Shared production database                     |
| Object Storage      | Cloudflare R2    | Application-managed object storage             |
| Email               | Resend           | Transactional email delivery                   |
| Observability       | Sentry           | Error monitoring and application observability |
| DNS                 | Cloudflare DNS   | DNS/address-book layer                         |
| Source Control      | GitHub           | Authoritative application source repository    |

Provider implementations shall remain behind FluxDine's architectural boundaries where shared services or abstractions are required.

---

# Current Initial Production

FluxDine currently operates an **Initial Production** environment.

The current Vercel project is named:

```text
fluxdine-staging
```

Despite its resource name, this deployment is designated as **Initial Production**.

The resource name shall not be interpreted as proof that the environment is Staging.

The current Initial Production environment consists of independently managed resources for:

* Vercel
* Turso
* Cloudflare R2
* Resend
* Sentry

These resources shall be treated as production resources.

---

# Initial Production Resource Naming Rule

The following naming convention is currently accepted:

```text
Resource Name:
fluxdine-staging

Environment Role:
Initial Production
```

This naming discrepancy is documented intentionally to prevent infrastructure resource names from being confused with logical environment roles.

Future infrastructure created specifically for Staging shall use an unambiguous staging designation where practical.

---

# Environment Model

FluxDine defines four logical environment roles:

```text
Development
     |
     v
Testing
     |
     v
Staging
     |
     v
Production
```

The logical environment roles are distinct from the currently deployed infrastructure.

---

# Development Environment

## Purpose

Development is the local engineering environment used for:

* Feature development
* Debugging
* Local experimentation
* Unit testing
* Integration development
* Developer verification

## Characteristics

Development:

* Runs primarily on developer machines
* Uses development configuration
* Uses non-production data
* May enable debugging facilities
* Must not depend on production secrets
* Must not modify production resources unintentionally

---

# Testing Environment

## Purpose

Testing provides controlled execution for automated validation.

Testing may be implemented through CI and dedicated test resources rather than a permanently deployed application environment.

Testing is used for:

* Automated tests
* Integration tests
* Regression tests
* Security validation
* Build validation
* Migration validation where applicable

## Characteristics

Testing resources shall be isolated from production.

Testing shall not use production database credentials, production object storage, or production secrets unless explicitly required by an approved security-controlled test procedure.

---

# Staging Environment

## Purpose

Staging is the future pre-production environment used for:

* Release candidate validation
* Production-like testing
* User acceptance testing
* Integration validation
* Final operational verification
* Pre-production deployment validation

## Current Status

A separately isolated Staging environment is **not yet operational**.

Staging shall be introduced when pre-production release validation requires an independently isolated environment.

Staging must not be simulated by reusing Initial Production resources.

---

# Staging Requirements

When Staging is introduced, it shall have independently isolated:

* Application deployment
* Database resources
* Object storage resources
* Environment variables
* Secrets
* Email configuration
* Observability environment
* Test data
* Domain configuration where applicable

Staging shall not have permission to modify Initial Production resources.

Staging configuration should be production-like where practical without exposing production secrets or customer data.

---

# Production Environment

## Purpose

Production is the live customer environment.

Production provides:

* Live customer access
* Tenant data
* Restaurant operations
* Customer ordering
* Operational services
* Production integrations
* Production observability
* Backup and recovery capability

---

# Current Production State

At initial launch:

```text
Logical Role:
Production

Operational Instance:
Initial Production

Current Vercel Project:
fluxdine-staging

Current Database:
Dedicated Initial Production Turso database

Current Object Storage:
Initial Production R2 resources

Current Email:
Initial Production Resend configuration

Current Observability:
Sentry environment = staging
```

The Sentry environment value reflects the current deployment configuration and may use the value `staging` during this initial operational stage.

Resource naming and environment-role terminology shall not be conflated.

---

# Future Production State

As FluxDine matures, the Initial Production environment may be replaced or supplemented by a conventionally named Production environment.

Such a change shall require:

* Migration planning
* Data protection
* Deployment validation
* DNS planning
* Secret migration
* Rollback planning
* Production approval
* Explicit release documentation

A new production environment shall not be created merely because a resource name currently contains the word `staging`.

---

# Environment Promotion Model

The intended promotion lifecycle is:

```text
Development
    |
    v
Testing / CI
    |
    v
Staging
    |
    v
Production
```

At the initial launch stage, the operational lifecycle is:

```text
Development
    |
    v
Testing / CI
    |
    v
Initial Production
```

Staging shall be inserted into the promotion path when the genuine isolated Staging environment becomes operational.

---

# Promotion Principles

Promotion shall mean moving a validated release toward a higher environment.

Promotion shall not mean copying mutable environment state between environments.

Each environment shall receive its own:

* Configuration
* Secrets
* Resource references
* Runtime settings

The release artifact may be the same logical build across environments where the deployment platform supports this model.

Environment-specific behavior shall be provided through external configuration.

---

# Release Promotion

A release should progress through the following conceptual stages:

```text
Source Change
     |
     v
CI Validation
     |
     v
Release Candidate
     |
     v
Testing
     |
     v
Staging
     |
     v
Production Approval
     |
     v
Production
```

At initial launch:

```text
Source Change
     |
     v
CI Validation
     |
     v
Release Candidate
     |
     v
Testing
     |
     v
Production Approval
     |
     v
Initial Production
```

---

# Production Release Governance

Production deployment shall follow:

```text
Validated Source
       |
       v
Build
       |
       v
Automated Validation
       |
       v
Release Candidate
       |
       v
Production Approval
       |
       v
Production Deployment
       |
       v
Health Verification
       |
       v
Release Confirmation
```

Production approval shall be explicit and attributable to an authorized person or release-control mechanism.

---

# Human Approval Requirement

Human approval is required for production releases.

The production deployment system shall not treat a successful CI build as automatic authorization for production release.

The intended operating model is:

```text
Automation
    |
    +--> Build
    +--> Test
    +--> Security Validation
    +--> Package
    +--> Prepare Deployment
             |
             v
      Human Approval
             |
             v
      Automated Execution
             |
             v
      Production Verification
```

Automation may execute the deployment after approval.

---

# Emergency Production Changes

Emergency production changes may bypass normal release timing only when required to protect:

* Customer data
* Platform security
* Service availability
* Payment integrity
* Critical production functionality

Emergency changes shall still:

* Be traceable
* Be documented
* Be reviewed
* Be validated as soon as practical
* Be incorporated into normal source control
* Have an associated recovery path

Emergency procedures shall not become the normal deployment mechanism.

---

# Deployment Workflow

Every standard release shall follow the general lifecycle:

```text
1. Source Change
       |
       v
2. Code Review
       |
       v
3. CI Validation
       |
       v
4. Build
       |
       v
5. Release Artifact
       |
       v
6. Environment Deployment
       |
       v
7. Health Verification
       |
       v
8. Release Validation
       |
       v
9. Promotion or Approval
       |
       v
10. Production Deployment
       |
       v
11. Production Verification
```

The CI/CD Pipeline specification defines the implementation of automated stages.

---

# Source Control Requirements

All deployable application changes shall be committed to the authorized source repository.

The FluxDine application repository currently uses:

```text
Mainline Branch:
master
```

The Architecture Bible repository may use a different branch structure and shall not be confused with the application repository.

The application deployment pipeline shall operate against the authorized application repository and branch policy.

---

# Pull Request and Review Requirements

Where branch protection and team workflow permit, production-bound changes should follow:

```text
Feature Branch
      |
      v
Pull Request
      |
      v
Code Review
      |
      v
CI Validation
      |
      v
Merge
      |
      v
Release Candidate
```

Direct unreviewed changes to protected production release branches shall not be the normal development process.

---

# Build Requirements

A production-bound build shall:

* Use committed source code
* Use the repository's declared dependency versions
* Produce a reproducible build where practical
* Complete required automated validation
* Avoid embedding environment secrets
* Record the source revision
* Produce a traceable deployment result

The build process shall not require manual modification of application source files for production deployment.

---

# Artifact Management

Deployment artifacts shall be:

* Versioned
* Immutable
* Traceable
* Reproducible where practical
* Associated with a source commit

Artifacts shall not be manually edited after successful validation.

If a release must change, a new release shall be generated.

---

# Vercel Deployment Model

The current FluxDine application is deployed on Vercel.

Vercel is responsible for:

* Application build execution
* Application deployment
* Serverless application execution
* Deployment lifecycle
* Deployment URLs
* Runtime environment injection
* Deployment-level status

FluxDine remains responsible for:

* Application correctness
* Configuration correctness
* Database compatibility
* Tenant isolation
* Business logic
* Authorization
* Release approval
* Post-deployment verification

---

# Vercel Project Boundaries

The following Vercel project roles currently exist or are planned:

| Vercel Project                       | Environment Role                          |
| ------------------------------------ | ----------------------------------------- |
| Legacy Orchids/SpicyCrust deployment | Legacy / Do Not Touch                     |
| `fluxdine-staging`                   | Initial Production                        |
| Future dedicated staging project     | Staging, when required                    |
| Future dedicated production project  | Optional future Production infrastructure |

The legacy Orchids/SpicyCrust deployment is not part of the active FluxDine deployment pipeline.

The active FluxDine application deployment shall remain associated with the FluxDine workspace repository.

---

# Application Root and Build Boundary

The FluxDine monorepo contains multiple application shells and packages.

The current Vercel deployment targets the application package rather than building unrelated workspace applications.

Deployment configuration shall preserve this boundary.

Unrelated workspace applications shall not be included in the production build unless explicitly required by architecture.

---

# Database Deployment

Database schema changes shall be deployed through version-controlled migrations.

The database deployment process shall:

* Use the approved migration chain
* Preserve migration ordering
* Avoid manual production schema edits
* Validate migration compatibility
* Coordinate application and schema versions
* Provide a recovery strategy

Database deployment is a separate concern from application artifact deployment.

---

# Database Migration Compatibility

Application releases shall be compatible with the database schema available at the time of deployment.

Where a migration cannot safely be performed simultaneously with application deployment, the release shall use a compatibility strategy.

Preferred strategies include:

* Backward-compatible schema changes
* Expand-and-contract migrations
* Transitional application versions
* Controlled roll-forward

Destructive schema changes shall receive additional review.

---

# Database Rollback Principle

Database rollback shall not be assumed to be equivalent to application rollback.

An application deployment may be safely reversible while a database migration may not be.

Therefore:

```text
Application Rollback
        !=
Database Rollback
```

If a database change is irreversible or unsafe to reverse, recovery shall use:

* Roll-forward
* Compatibility correction
* Database restoration
* Disaster recovery procedure

as appropriate.

---

# Application Rollback

Application rollback shall restore the previous known-good application release where technically safe.

Rollback shall be considered when:

* Deployment causes critical errors
* Application startup fails
* Major functionality is unavailable
* Severe regression is detected
* Security behavior is incorrect
* Production health checks fail

Rollback shall not be used blindly when the current database schema is incompatible with the previous application version.

---

# Roll-Forward Strategy

Roll-forward is preferred when:

* Database changes are irreversible
* The current schema cannot safely support the previous application
* A corrective release can be produced quickly
* Data integrity would be endangered by rollback

The corrective release shall itself follow the normal validation and approval process unless an emergency procedure applies.

---

# Zero-Downtime Deployment

FluxDine shall target zero or minimal user-visible downtime where practical.

The deployment strategy shall account for:

* Application compatibility
* Database migration compatibility
* Runtime behavior
* External provider availability
* Deployment platform behavior

Vercel-managed application deployment characteristics shall be used where appropriate.

FluxDine shall not assume that every deployment can be performed without any interruption.

---

# Zero-Downtime Database Requirements

Database migrations should be designed so that application availability is preserved whenever practical.

Preferred patterns include:

* Additive schema changes
* Nullable-first additions
* Backward-compatible columns
* Expand-and-contract migrations
* Delayed destructive changes

Destructive changes shall be separated from application changes when required to maintain compatibility.

---

# Configuration Management

Environment-specific configuration shall remain external to application source code.

Configuration includes:

* Environment identifiers
* Application URLs
* Database connection references
* Object storage configuration
* Email provider configuration
* Observability configuration
* Feature configuration
* Domain configuration

Detailed variable names and values are defined in the Environment Variables specification.

---

# Configuration Separation

The same source release may be deployed to different environment roles while receiving different configuration.

For example:

```text
Same Release
     |
     +----> Testing Configuration
     |
     +----> Staging Configuration
     |
     +----> Production Configuration
```

Environment-specific configuration shall not be committed directly into application source code.

---

# Secrets Management

Deployment shall consume secrets from approved external secret/configuration mechanisms.

Secrets shall:

* Never be committed to Git
* Never be hardcoded
* Never be exposed in build logs
* Never be included in client-side bundles unless intentionally public
* Be isolated by environment
* Be rotated according to the Environment & Secrets Strategy

The Deployment Specification defines the boundary; the Environment & Secrets Strategy defines the detailed policy.

---

# Production Secret Protection

Production secrets shall be restricted to production execution contexts.

Examples include:

* Turso authentication credentials
* R2 credentials
* Resend API credentials
* Sentry authentication credentials where required for deployment
* Application authentication secrets
* Payment provider secrets

Secret values shall never appear in:

* Git commits
* Pull requests
* Build logs
* Deployment logs
* Application logs
* Screenshots
* Documentation

---

# Public Configuration

Public configuration may be embedded into client-side application output only when it is explicitly classified as non-secret.

Examples may include:

* Public application URL
* Public Sentry DSN
* Public feature configuration

Secret credentials shall never be treated as public configuration.

---

# Domain and DNS Deployment Boundary

Cloudflare is the DNS provider for the FluxDine domain layer.

The application remains responsible for interpreting application hostnames and resolving them to the appropriate tenant and restaurant.

The current architectural domain model is:

```text
fluxdine.com
     |
     +--> app.fluxdine.com
     |
     +--> signup.fluxdine.com
     |
     +--> {restaurant}.fluxdine.com
     |
     +--> Future custom restaurant domains
```

Cloudflare DNS does not determine tenant ownership.

The application domain-resolution layer owns:

```text
Hostname
   |
   v
Restaurant
   |
   v
Tenant
```

---

# Canonical Domain

The canonical FluxDine brand domain is:

```text
fluxdine.com
```

The secondary domain:

```text
fluxdine.online
```

shall redirect to the canonical domain.

Domain routing and DNS automation shall be implemented according to the Domain and Infrastructure specifications.

---

# Application Platform Domains

The intended platform domains are:

```text
app.fluxdine.com
```

for the HQ Platform,

```text
signup.fluxdine.com
```

for the Self-Service Platform,

and:

```text
{restaurant}.fluxdine.com
```

for default restaurant storefronts.

Custom restaurant-owned domains are architecturally supported but their automation is deferred until the relevant implementation phase.

---

# Hostname Resolution

Production routing shall not rely on a single hardcoded restaurant identifier.

The application shall resolve a request hostname through the platform's domain-resolution logic.

The conceptual flow is:

```text
HTTP Hostname
      |
      v
Domain Resolver
      |
      v
Restaurant Domain Configuration
      |
      v
Restaurant
      |
      v
Tenant
      |
      v
Tenant-Scoped Application
```

Hostname ownership and tenant association shall be validated server-side.

---

# Deployment Verification

A deployment shall be verified after deployment.

Verification may include:

* Deployment status
* Application availability
* Health endpoint
* Database connectivity
* Critical API availability
* Authentication availability
* Public storefront availability
* Platform shell availability
* Error monitoring status

Verification requirements shall be adapted to the environment.

---

# Health Verification

For the current Vercel-hosted Next.js application, health verification shall include application availability and database connectivity through the platform health endpoint or equivalent application-level health mechanism.

The current FluxDine health endpoint is:

```text
/api/v1/health
```

The health mechanism shall verify the application can respond correctly and, where required, establish required database connectivity.

---

# Health Check Principle

Deployment success shall not be determined solely by the hosting provider's deployment status.

The expected lifecycle is:

```text
Provider Deployment Success
          |
          v
Application Health Check
          |
          v
Critical Dependency Verification
          |
          v
Release Validation
```

A deployment that completes at the infrastructure layer but fails application health verification shall be considered unsuccessful.

---

# Post-Deployment Validation

Post-deployment validation shall verify the most critical platform paths for the affected release.

Depending on the release, this may include:

* Health endpoint
* Authentication
* Tenant resolution
* Restaurant resolution
* Menu retrieval
* Branch retrieval
* Order creation
* Reservation functionality
* Payment integration
* File upload/retrieval
* Email delivery integration
* Critical HQ operations

Validation scope shall be proportional to release risk.

---

# Deployment Failure Handling

Deployment failure shall follow:

```text
Deployment
    |
    v
Failure Detected
    |
    v
Classify Failure
    |
    +---------------------+
    |                     |
    v                     v
Application Failure    Infrastructure Failure
    |                     |
    v                     v
Rollback/Fix          Provider Recovery
    |                     |
    +----------+----------+
               |
               v
          Verification
```

Failures shall be recorded and traceable.

---

# Failed Deployment

If a deployment fails before becoming active:

* The release shall not be treated as successful.
* The previous known-good release shall remain active where possible.
* Deployment logs shall be reviewed.
* The source change shall be corrected before retrying.
* Repeated failures shall trigger engineering investigation.

---

# Failed Production Deployment

If a production deployment causes service degradation:

1. Stop further promotion.
2. Determine whether the issue is application, configuration, database, or infrastructure related.
3. Determine whether rollback is safe.
4. Roll back the application if safe.
5. Otherwise execute the appropriate corrective or recovery procedure.
6. Verify production health.
7. Record the incident and release outcome.

---

# Database Failure During Deployment

If a database migration fails:

* Stop further migration execution.
* Do not manually modify production schema to hide the failure.
* Determine the migration state.
* Determine whether the migration is safely reversible.
* Use the Database Migration Strategy.
* Use roll-forward or restoration where required.
* Verify application/schema compatibility before resuming deployment.

---

# Deployment Concurrency

Multiple production releases shall not be deployed concurrently unless the deployment system explicitly supports safe serialization.

Production releases should be serialized so that:

* Release order is deterministic
* Rollback decisions remain understandable
* Database migrations do not overlap unsafely
* Release ownership remains clear

---

# Deployment Locking

Where required, a production deployment lock shall prevent simultaneous conflicting releases.

The lock may be implemented through CI/CD or the deployment platform.

The deployment system shall prevent a later release from unintentionally overriding an active recovery process.

---

# Release Identification

Every production deployment should expose or make internally available:

* Application version
* Git commit
* Deployment identifier
* Deployment timestamp
* Environment

This information shall assist incident investigation and rollback.

---

# Deployment Auditability

Production deployments shall be auditable.

The deployment record should allow engineers to determine:

* Who authorized the release
* What commit was deployed
* When it was deployed
* Which environment was targeted
* Which build was used
* Whether health checks passed
* Whether rollback occurred

Deployment auditability supports incident response and operational accountability.

---

# Security Requirements

Deployment infrastructure shall follow the Security Architecture.

Deployment processes shall enforce:

* Least privilege
* Environment isolation
* Secret protection
* Authorized production access
* Secure transport
* Dependency validation
* Artifact integrity
* Auditability

Production credentials shall not be available to unauthorized development workflows.

---

# Least Privilege

Deployment identities shall have only the permissions required to perform their deployment responsibilities.

Examples:

* CI should not have unrestricted production infrastructure access by default.
* Testing should not have production database write access.
* Application runtime should not have deployment-management permissions.
* Deployment credentials should not be used as application runtime credentials unless explicitly required.

---

# Third-Party Provider Boundary

Third-party infrastructure providers are outside the FluxDine application trust boundary.

FluxDine shall interact with providers through approved integration boundaries.

Examples include:

```text
FluxDine
   |
   +--> Turso
   +--> R2
   +--> Resend
   +--> Sentry
   +--> Stripe (when activated)
```

Third-party providers shall not directly access FluxDine business modules.

---

# Object Storage Deployment Boundary

Cloudflare R2 provides object storage.

Application deployment shall not assume that local filesystem storage is equivalent to production object storage.

Production application functionality requiring persistent objects shall use the configured R2 integration when the relevant feature is enabled.

Object storage recovery is governed by the Backup Strategy and Disaster Recovery specifications.

---

# Email Deployment Boundary

Resend provides transactional email delivery for production.

Deployment shall provide the correct environment-specific Resend configuration.

Development and testing shall not unintentionally send real production emails.

Where testing email delivery is required, an isolated testing configuration shall be used.

---

# Observability Deployment Boundary

Sentry provides application error monitoring and observability.

Deployment shall ensure the appropriate environment configuration is provided.

Production deployments shall be observable after release.

The Sentry environment shall distinguish deployment context where configured.

Sensitive request information shall remain protected according to Security Architecture and Sentry configuration.

---

# Database Provider Boundary

Turso is the current initial-production database provider.

The database architecture uses a shared database/shared-schema model.

Deployment shall not create a database per tenant or restaurant unless a future architecture decision explicitly authorizes that change.

PostgreSQL remains a future migration target and is not the current initial-production database provider.

---

# Infrastructure Independence

The Deployment Specification defines architectural requirements without making future infrastructure evolution impossible.

Current implementation uses:

* Vercel
* Turso
* Cloudflare R2
* Resend
* Sentry
* Cloudflare DNS

Future providers may replace current providers only through an approved architecture change.

Provider migration shall not require rewriting business-domain architecture where an existing abstraction is intended to isolate provider-specific behavior.

---

# Future Infrastructure Capabilities

The following may be introduced later:

* Dedicated background workers
* Dedicated queue infrastructure
* Dedicated cache infrastructure
* PostgreSQL
* Additional deployment regions
* Advanced progressive delivery
* Dedicated production Vercel project
* Dedicated staging infrastructure
* More advanced deployment orchestration

These capabilities are future evolution and shall not be treated as active initial-production infrastructure unless explicitly implemented.

---

# Background Jobs

FluxDine may execute scheduled or asynchronous work through provider-supported mechanisms.

Current job execution shall use the mechanisms explicitly implemented by the platform.

The existence of a job-related application module shall not be interpreted as proof that a separate worker cluster or queue infrastructure exists.

Future dedicated worker infrastructure shall follow a separate architecture decision.

---

# Cron and Scheduled Execution

Scheduled tasks shall be deployed according to the hosting platform's supported scheduling mechanism.

Scheduled job frequency and platform limitations are operational configuration concerns.

A scheduled job shall:

* Be authenticated where required
* Be idempotent where practical
* Avoid duplicate processing
* Be observable
* Handle failures safely
* Avoid exposing secrets

Detailed scheduled-job implementation belongs to the relevant application/service specifications.

---

# Release Compatibility

A release shall be compatible with:

* Runtime version
* Dependency versions
* Database schema
* Environment configuration
* External integrations
* Deployment platform capabilities

The release process shall verify compatibility before production promotion.

---

# Dependency Management

Production deployment shall use the dependency versions declared by the repository.

Dependency installation shall be deterministic where practical.

The deployment process shall not silently upgrade production dependencies during deployment.

Dependency changes shall be source-controlled and validated through CI.

---

# Runtime Version

The runtime version used by production deployment shall be explicitly controlled by deployment configuration and supported by the application.

Runtime upgrades shall be treated as deployment-impacting changes.

A runtime upgrade shall be validated before production release.

---

# Deployment Reproducibility

A production deployment should be reproducible from:

* Source commit
* Dependency lockfile
* Build configuration
* Environment configuration
* Deployment configuration

Secrets remain external and are not part of the reproducible source artifact.

---

# Infrastructure Configuration

Infrastructure configuration shall be version-controlled where practical.

Examples include:

* Deployment configuration
* Build configuration
* Routing configuration
* Cron configuration
* Infrastructure-as-code definitions where introduced

Sensitive values shall remain outside the repository.

---

# Infrastructure Drift

Production infrastructure shall not be manually modified without authorization.

Where provider configuration is managed through a dashboard, important production configuration changes shall be documented and reflected in the authoritative infrastructure documentation where practical.

Infrastructure drift shall be investigated when detected.

---

# Production Change Principle

Production changes should follow:

```text
Requested Change
      |
      v
Review
      |
      v
Approved Configuration
      |
      v
Deployment / Change Execution
      |
      v
Verification
      |
      v
Documentation
```

Direct undocumented production changes shall be avoided.

---

# Rollback Decision Matrix

| Failure Type                               | Preferred Response                                 |
| ------------------------------------------ | -------------------------------------------------- |
| Application-only regression                | Application rollback                               |
| Configuration error                        | Correct configuration / redeploy                   |
| Backward-compatible migration issue        | Application rollback or correction                 |
| Irreversible database migration issue      | Roll-forward or recovery                           |
| Data corruption                            | Database recovery / restoration                    |
| Provider outage                            | Provider recovery / operational failover           |
| Security compromise                        | Incident response + credential rotation + recovery |
| Mixed application/database incompatibility | Controlled recovery strategy                       |

The exact response shall be determined by the incident context.

---

# Production Rollback Requirements

Before production deployment, the release owner should confirm:

* Previous known-good release is identifiable
* Deployment can be reverted where technically safe
* Database migration compatibility is understood
* Configuration changes are known
* Recovery contacts are available
* Monitoring is operational
* Backup/recovery mechanisms are available

---

# Backup Relationship

Deployment recovery depends on the Backup Strategy when application rollback is insufficient.

The Backup Strategy defines:

* Backup requirements
* Retention
* Backup verification
* Restore testing
* Recovery data availability

Deployment shall not treat application rollback as a substitute for database backups.

---

# Disaster Recovery Relationship

Disaster Recovery applies when normal deployment rollback is insufficient.

Examples include:

* Database corruption
* Critical infrastructure failure
* Provider-level outage
* Major security compromise
* Loss of required production resources

The Disaster Recovery specification defines the recovery process and objectives.

---

# RPO and RTO Relationship

The current initial-production recovery objectives are:

```text
Maximum RPO: 24 hours
Maximum RTO: 4 hours
```

Deployment procedures shall remain compatible with these recovery objectives.

The detailed RPO/RTO implementation belongs to Backup Strategy and Disaster Recovery.

---

# Production Data Protection

Production deployments shall preserve:

* Tenant data
* Restaurant data
* Branch data
* Customer data
* Orders
* Reservations
* Payment-related records
* Audit records
* Configuration data

Deployment procedures shall not intentionally destroy production data.

Destructive changes require explicit review and recovery planning.

---

# Tenant Isolation During Deployment

Deployment changes shall preserve tenant isolation.

A release shall not:

* Mix tenant data
* Bypass tenant authorization
* Introduce cross-tenant access
* Modify another tenant's configuration unintentionally
* Expose tenant data through logs or diagnostics

Tenant isolation remains a server-side application responsibility.

---

# Multi-Tenant Deployment Model

FluxDine uses a shared application and shared-schema multi-tenant model.

The deployment architecture therefore deploys the platform rather than deploying a separate application instance per tenant.

Conceptually:

```text
                FluxDine Application
                       |
          +------------+------------+
          |            |            |
       Tenant A     Tenant B     Tenant C
          |            |            |
      Restaurant    Restaurant   Restaurant
```

Deployment changes apply to the shared platform.

Tenant-specific behavior shall be controlled through tenant-scoped application logic and configuration.

---

# Restaurant Storefront Deployment

Restaurant storefronts are not deployed as independent applications for each restaurant.

Default storefront hostnames use:

```text
{restaurant}.fluxdine.com
```

The shared application resolves the hostname and loads the appropriate tenant/restaurant context.

This architecture avoids per-restaurant application deployment duplication.

---

# HQ Platform Deployment

The HQ Platform is part of the FluxDine platform deployment.

Its production access is governed by HQ authorization.

Deployment shall not treat tenant membership as HQ authorization.

---

# Self-Service Platform Deployment

The Self-Service Platform is part of the FluxDine platform deployment.

Its production behavior shall remain isolated from HQ-only functionality.

Deployment changes affecting onboarding shall be validated against:

* Owner registration
* Email verification
* Plan selection
* Trial initiation
* Tenant provisioning
* Restaurant provisioning
* Branch provisioning
* Configuration
* Launch workflow

---

# Deployment Testing Requirements

Before production promotion, the release shall pass applicable testing.

Testing may include:

* Unit tests
* Integration tests
* Regression tests
* API tests
* Database tests
* Security tests
* Build validation
* Deployment health tests

Testing requirements shall be defined by the CI/CD and module specifications.

---

# High-Risk Release Classification

Releases should receive additional scrutiny when they affect:

* Authentication
* Authorization
* Tenant isolation
* Database schema
* Payments
* Reservations
* Orders
* Customer data
* Production domains
* DNS
* Email
* Object storage
* Security controls
* Backup/recovery
* Infrastructure configuration

High-risk releases should receive expanded verification before production approval.

---

# Low-Risk Release Classification

Examples of potentially lower-risk changes include:

* Documentation
* Non-functional UI changes
* Copy changes
* Internal refactoring without schema impact
* Non-production configuration changes

Risk classification shall remain contextual.

---

# Deployment Observability

Deployments shall be observable through the monitoring and logging systems.

At minimum, engineers should be able to determine:

* Deployment success/failure
* Application availability
* Error rate after deployment
* Critical dependency availability
* Release identity

Monitoring and logging specifications define the detailed telemetry model.

---

# Deployment Logs

Deployment logs shall not contain:

* Secrets
* API keys
* Authentication tokens
* Passwords
* Private credentials

Deployment logs may contain:

* Commit identifiers
* Build identifiers
* Deployment identifiers
* Environment identifiers
* Non-sensitive operational information

---

# Deployment Security Validation

Production releases should include appropriate security validation.

Security validation may include:

* Dependency vulnerability scanning
* Secret scanning
* Static analysis
* Configuration validation
* Authentication checks
* Authorization checks

Critical security findings shall block production deployment unless an authorized emergency procedure explicitly permits otherwise.

---

# Deployment Approval Roles

Production deployment approval shall be limited to authorized engineering or operational personnel.

The exact organizational role structure may evolve.

Authorization shall remain auditable.

---

# Separation of Duties

Where operationally practical:

```text
Developer
   |
   v
Code Review / CI
   |
   v
Release Candidate
   |
   v
Production Approval
   |
   v
Deployment
```

No single uncontrolled manual step should be capable of bypassing the required release governance.

---

# Deployment Documentation

Important production deployments should record:

* Release identifier
* Git commit
* Release purpose
* Major changes
* Database migrations
* Configuration changes
* Approval
* Deployment result
* Rollback result if applicable

---

# Deployment Checklist

Before Production:

```text
[ ] Source committed
[ ] Code review completed where required
[ ] CI passed
[ ] Security validation passed
[ ] Build completed
[ ] Release identified
[ ] Database changes reviewed
[ ] Configuration verified
[ ] Production secrets available
[ ] Rollback/recovery path understood
[ ] Monitoring available
[ ] Production approval obtained
```

After Production:

```text
[ ] Deployment completed
[ ] Health endpoint verified
[ ] Database connectivity verified
[ ] Critical application paths verified
[ ] Error monitoring checked
[ ] Logs checked
[ ] Release marked successful
[ ] Deployment recorded
```

---

# Engineering Rules

## Rule DEP-001

Every production deployment shall originate from version-controlled source.

---

## Rule DEP-002

Production releases shall be traceable to a specific source revision.

---

## Rule DEP-003

Secrets shall never be committed to source control.

---

## Rule DEP-004

Production deployment shall require explicit authorized approval.

---

## Rule DEP-005

Environment resources shall remain isolated.

---

## Rule DEP-006

Database changes shall be deployed through version-controlled migrations.

---

## Rule DEP-007

Application rollback shall not be assumed to roll back database changes.

---

## Rule DEP-008

Deployment success requires application-level verification.

---

## Rule DEP-009

Production data shall not be used as disposable testing data.

---

## Rule DEP-010

Production infrastructure shall not be modified through undocumented manual changes as the normal operating model.

---

## Rule DEP-011

Release artifacts shall remain immutable after validation.

---

## Rule DEP-012

Production deployments shall remain observable.

---

## Rule DEP-013

Tenant isolation shall remain intact across deployments.

---

## Rule DEP-014

Future infrastructure capabilities shall not be treated as active infrastructure until implemented and approved.

---

## Rule DEP-015

The current `fluxdine-staging` Vercel project shall be treated as Initial Production despite its resource name.

---

# Architecture Decision Records

## ADR-DEP-001

All FluxDine deployments originate from version-controlled source code.

---

## ADR-DEP-002

Deployment artifacts are treated as immutable after validation.

---

## ADR-DEP-003

Environment configuration remains external to application source code.

---

## ADR-DEP-004

Secrets remain outside source control and are isolated by environment.

---

## ADR-DEP-005

FluxDine uses Development, Testing, Staging, and Production as logical environment roles.

---

## ADR-DEP-006

At initial launch, Development, Testing/CI, and Initial Production are the primary active operational environments.

---

## ADR-DEP-007

A separately isolated Staging environment will be introduced when pre-production release validation requires it.

---

## ADR-DEP-008

The current Vercel project named `fluxdine-staging` is designated as Initial Production and shall not be treated as genuine Staging.

---

## ADR-DEP-009

Production deployments require explicit authorized approval.

---

## ADR-DEP-010

Production deployment execution may be automated after approval.

---

## ADR-DEP-011

Application rollback and database rollback are separate recovery concerns.

---

## ADR-DEP-012

Database migrations shall follow the Database Migration Strategy.

---

## ADR-DEP-013

Application health verification is required to confirm deployment success.

---

## ADR-DEP-014

FluxDine uses Vercel as the current application hosting provider.

---

## ADR-DEP-015

FluxDine uses Turso as the current initial-production database provider.

---

## ADR-DEP-016

FluxDine uses Cloudflare R2 as the current object storage provider.

---

## ADR-DEP-017

FluxDine uses Resend as the current transactional email provider.

---

## ADR-DEP-018

FluxDine uses Sentry as the current application observability provider.

---

## ADR-DEP-019

Cloudflare DNS provides the DNS layer while FluxDine application logic owns hostname-to-tenant/restaurant resolution.

---

## ADR-DEP-020

The Deployment Specification is the authoritative deployment architecture specification.

---

# Appendix A — Environment Model

| Logical Environment | Current Status                   | Primary Purpose                           |
| ------------------- | -------------------------------- | ----------------------------------------- |
| Development         | Active                           | Local development                         |
| Testing             | Active through CI/test resources | Automated validation                      |
| Staging             | Future                           | Production-like pre-production validation |
| Production          | Active as Initial Production     | Live customer environment                 |

---

# Appendix B — Current Infrastructure Matrix

| Capability         | Initial Production | Staging                                |
| ------------------ | ------------------ | -------------------------------------- |
| Vercel             | Active             | Future                                 |
| Turso              | Active             | Future isolated resource               |
| Cloudflare R2      | Active             | Future isolated resource               |
| Resend             | Active             | Future isolated configuration          |
| Sentry             | Active             | Future environment                     |
| Cloudflare DNS     | Foundational layer | As required                            |
| Production Secrets | Isolated           | Separate secrets                       |
| Customer Data      | Yes                | No production customer data by default |

---

# Appendix C — Deployment Lifecycle

```text
Developer
    |
    v
Feature / Change
    |
    v
Git Commit
    |
    v
Pull Request / Review
    |
    v
CI Validation
    |
    v
Build
    |
    v
Release Candidate
    |
    v
Testing
    |
    +----------------------------+
    |                            |
    v                            v
Staging                       Initial Production
    |                            |
    v                            |
Final Validation                 |
    |                            |
    +-------------+--------------+
                  |
                  v
          Production Approval
                  |
                  v
       Production Deployment
                  |
                  v
        Health Verification
                  |
                  v
         Release Confirmation
```

---

# Appendix D — Initial Launch Lifecycle

Until a genuine Staging environment exists:

```text
Development
     |
     v
Testing / CI
     |
     v
Release Candidate
     |
     v
Production Approval
     |
     v
Initial Production
     |
     v
Health Verification
```

This temporary launch lifecycle shall not be interpreted as eliminating the logical Staging environment role.

---

# Appendix E — Current Provider Architecture

```text
                         GitHub
                           |
                           v
                    FluxDine Application
                           |
                           v
                         Vercel
                           |
        +------------------+------------------+
        |                  |                  |
        v                  v                  v
      Turso               R2               Resend
    Database           Object Store         Email
        |
        |
        v
      Sentry
   Observability

Cloudflare DNS
      |
      v
FluxDine Hostnames
      |
      v
Application Host Resolution
      |
      v
Restaurant
      |
      v
Tenant
```

---

# Appendix F — Deployment vs Related Specifications

| Concern                               | Authoritative Specification                   |
| ------------------------------------- | --------------------------------------------- |
| Deployment architecture               | Deployment Specification                      |
| Environment roles and secret strategy | Environment & Secrets Strategy                |
| Environment variable catalog          | Environment Variables                         |
| CI/CD implementation                  | CI/CD Pipeline                                |
| Monitoring                            | Monitoring                                    |
| Logging                               | Logging                                       |
| Database schema                       | Database Engineering Specifications           |
| Database migrations                   | Database Migration Strategy                   |
| Backup                                | Backup Strategy                               |
| Disaster recovery                     | Disaster Recovery                             |
| Scaling                               | Scaling Strategy                              |
| Security                              | Security Architecture                         |
| Domain architecture                   | Infrastructure / Domain Architecture          |
| Payment integration                   | Payment Architecture / Shared Payment Service |

---

# Appendix G — Current Production Identity

```text
Logical Environment:
Production

Operational Designation:
Initial Production

Vercel Project:
fluxdine-staging

Vercel Deployment:
FluxDine application deployment

Database:
Initial Production Turso database

Object Storage:
Initial Production Cloudflare R2 resources

Email:
Initial Production Resend configuration

Observability:
Sentry

DNS:
Cloudflare DNS
```

---

# Appendix H — Future Staging Requirements

When genuine Staging is introduced, the following shall be independently provisioned or logically isolated:

```text
Staging
   |
   +--> Vercel Staging Project
   |
   +--> Staging Turso Database
   |
   +--> Staging R2 Resources
   |
   +--> Staging Resend Configuration
   |
   +--> Sentry Staging Environment
   |
   +--> Staging Secrets
   |
   +--> Staging Configuration
```

Staging shall not share production credentials.

Staging shall not write to production databases or production object storage.

Staging shall not send unintended customer-facing production email.

---

# Appendix I — Rollback Decision Flow

```text
Production Failure
       |
       v
Identify Failure Type
       |
       +-------------------+
       |                   |
       v                   v
Application             Database
       |                   |
       v                   v
Is rollback safe?      Is migration reversible?
       |                   |
   +---+---+           +---+---+
   |       |           |       |
  Yes      No         Yes      No
   |       |           |       |
   v       v           v       v
Rollback  Fix/       Rollback Roll-forward/
App       Roll-forward Migration Recovery
   |       |           |       |
   +-------+-----------+-------+
               |
               v
        Verify Production
```

---

# Appendix J — Production Deployment Checklist

## Before Approval

```text
[ ] Release commit identified
[ ] CI validation passed
[ ] Security validation passed
[ ] Build succeeded
[ ] Database changes reviewed
[ ] Configuration reviewed
[ ] Required secrets confirmed
[ ] External integrations reviewed
[ ] Rollback/recovery strategy confirmed
[ ] Monitoring available
[ ] Release owner identified
```

## Approval

```text
[ ] Authorized production approver identified
[ ] Release approved
[ ] Approval recorded
```

## Deployment

```text
[ ] Production deployment initiated
[ ] Deployment provider reports success
[ ] Application health check passes
[ ] Database connectivity verified
[ ] Critical application paths verified
[ ] Monitoring checked
[ ] Logs checked
```

## Completion

```text
[ ] Release marked successful
[ ] Deployment identity recorded
[ ] Commit recorded
[ ] Any issues documented
[ ] Rollback not required
```

---

# Appendix K — Future Deployment Capabilities

The following are reserved future capabilities and are not required for initial launch:

* Dedicated Staging infrastructure
* Dedicated Production Vercel project
* PostgreSQL migration
* Dedicated worker services
* Dedicated queue infrastructure
* Dedicated cache infrastructure
* Blue-green deployment
* Canary deployment
* Progressive delivery
* Multi-region deployment
* Multi-cloud deployment
* GitOps
* Automated infrastructure provisioning
* Regional failover
* Advanced release orchestration

These capabilities shall require appropriate architecture review before implementation.

---

# References

* Infrastructure Architecture
* Security Architecture
* Backend Engineering Specifications
* Frontend Engineering Specifications
* Database Engineering Specifications
* Database Migration Strategy
* Environment & Secrets Strategy
* Environment Variables
* CI/CD Pipeline
* Monitoring
* Logging
* Backup Strategy
* Disaster Recovery
* Scaling Strategy
* Domain Architecture
* Shared Platform Services
* Payment Architecture

---

# Revision History

| Version | Date            | Author               | Description                                                                                                                                                                                                                                      |
| ------- | --------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1.0     | Initial Release | FluxDine Engineering | Approved as the authoritative Deployment Specification                                                                                                                                                                                           |
| 1.1     | 2026-09-12      | FluxDine Engineering | Revised deployment architecture to align with Initial Production, current infrastructure providers, environment roles, production approval governance, database-aware rollback, current Vercel deployment model, and future Staging architecture |

---

# Final Document State

**Version:** 1.1

**Status:** Pending Approval

**Authority:** Authoritative Deployment Specification for FluxDine

**Implementation Rule:** This document defines deployment architecture and governance. Detailed implementation remains delegated to the relevant Infrastructure Engineering Specifications.

**Architecture Principle:** Current implementation details may evolve, but any material change to deployment architecture requires an explicit architecture review and revision of this specification.

````