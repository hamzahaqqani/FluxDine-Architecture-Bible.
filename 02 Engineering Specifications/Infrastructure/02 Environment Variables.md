# 02 Engineering Specifications

# Infrastructure

# 02 — Environment Variables

---

# Document Control

| Field | Value |
|---|---|
| **Document ID** | FD-ENG-INF-002 |
| **Document Name** | Environment Variables |
| **Version** | 1.1 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | Deployment Specification<br>Environment & Secrets Strategy<br>Security Architecture<br>Infrastructure Architecture |
| **Referenced By** | CI/CD Pipeline<br>Deployment Automation<br>Application Runtime<br>Monitoring<br>Database Integration<br>Object Storage Integration<br>Email Integration<br>Payment Integration |

---

# Document Status

| Item | Value |
|---|---|
| Status | Pending Approval |
| Approval | Requires Architecture/Engineering Review |
| Implementation | Engineering Specification |
| Version | 1.0 |
| Last Updated | 2026-09-12 |

This document defines the authoritative environment-variable and runtime-configuration contract for FluxDine.

---

# Purpose

This document defines how environment variables and environment-specific configuration are structured, named, classified, validated, supplied, and consumed throughout the FluxDine platform.

The objective is to ensure that:

- Configuration remains outside application source code
- Secrets remain protected
- Environment boundaries remain isolated
- Production configuration is controlled
- Client-side exposure is intentional
- Required configuration is validated
- Deployment behavior is predictable
- Provider credentials are not hardcoded
- Configuration remains compatible with the Deployment Specification

---

# Scope

This specification defines:

- Environment-variable architecture
- Variable classification
- Naming conventions
- Public configuration
- Server-only configuration
- Secret configuration
- Required variables
- Optional variables
- Environment-specific configuration
- Local development configuration
- CI configuration
- Initial Production configuration
- Future Staging configuration
- Vercel configuration
- Database configuration
- Object storage configuration
- Email configuration
- Observability configuration
- Domain configuration
- Authentication configuration
- Cron configuration
- Payment configuration
- Configuration validation
- Missing-variable behavior
- Client-side exposure rules
- Configuration precedence
- Configuration lifecycle
- Configuration security rules

---

# Out of Scope

This document does not define:

- Secret values
- Passwords
- API keys
- Authentication tokens
- Database credentials
- Provider account credentials
- CI/CD workflow implementation
- Deployment workflow
- Database schema
- Database migration implementation
- Business logic
- Tenant authorization
- Provider-specific application architecture
- Disaster recovery procedures

Secret storage and lifecycle policy are defined by:

`00 Environment & Secrets Strategy.md`

Deployment behavior is defined by:

`01 Deployment Specification.md`

---

# Configuration Architecture

FluxDine follows the principle:

```text
Source Code
    |
    | fixed application behavior
    v
Application

Environment Configuration
    |
    | environment-specific values
    v
Runtime Configuration

Secrets
    |
    | protected credentials
    v
Server-Side Runtime
````

Application source code shall not contain environment-specific secrets or credentials.

---

# Configuration Categories

FluxDine environment variables are divided into four primary categories:

```text
1. Public Configuration
2. Server Configuration
3. Secret Configuration
4. Build / Deployment Configuration
```

---

# Public Configuration

Public configuration contains values that may intentionally be exposed to browser clients.

Public variables shall only contain information that is safe for public exposure.

Typical examples include:

* Public application URL
* Public observability DSN
* Public feature configuration where explicitly approved
* Public branding configuration where applicable

Public configuration shall never contain:

* Passwords
* API secrets
* Database credentials
* Private keys
* Authentication secrets
* Provider secret tokens

---

# Public Variable Naming

In Next.js, variables intended for browser exposure shall normally use:

```text
NEXT_PUBLIC_
```

prefixes.

Example:

```text
NEXT_PUBLIC_SENTRY_DSN
```

The prefix is an explicit declaration that the value may be included in client-side application output.

Developers shall not use the prefix for a secret.

---

# Server Configuration

Server configuration is available only to server-side application execution.

Server configuration may include:

* Internal URLs
* Runtime identifiers
* Feature controls
* Provider configuration
* Database configuration
* Application configuration

Server configuration does not necessarily mean the value is secret.

However, unless explicitly classified as public, server configuration shall not be exposed to browser clients.

---

# Secret Configuration

Secret configuration contains credentials or values that must not be exposed publicly.

Examples include:

* Database authentication tokens
* API keys
* Application secrets
* Signing secrets
* Provider credentials
* Webhook secrets
* Deployment authentication credentials

Secret variables shall only be available to the runtime or deployment process that requires them.

---

# Build and Deployment Configuration

Some environment variables exist primarily to control:

* Build behavior
* Deployment behavior
* Source-map upload
* Release identification
* CI execution
* Provider deployment integration

These values shall be classified according to their sensitivity.

Deployment credentials shall be treated as secrets.

---

# Environment Roles

FluxDine defines:

```text
Development
Testing
Staging
Production
```

These are logical environment roles.

At initial launch:

```text
Development
Testing / CI
Initial Production
```

are the primary active operational environments.

A separately isolated Staging environment will be introduced when pre-production release validation requires it.

---

# Environment Isolation

Each environment shall have its own configuration boundary.

Conceptually:

```text
Development
    |
    +--> Development Configuration
    +--> Development Secrets
    +--> Development Resources

Testing
    |
    +--> Testing Configuration
    +--> Testing Secrets
    +--> Testing Resources

Staging
    |
    +--> Staging Configuration
    +--> Staging Secrets
    +--> Staging Resources

Production
    |
    +--> Production Configuration
    +--> Production Secrets
    +--> Production Resources
```

No environment shall unintentionally inherit production secrets.

---

# Initial Production

The current Initial Production environment uses the Vercel project:

```text
fluxdine-staging
```

Despite its infrastructure resource name, it is designated as:

```text
Initial Production
```

Environment variables configured for this project shall therefore be treated as production configuration.

---

# Configuration Source of Truth

Environment variable values shall be supplied through approved configuration mechanisms.

The repository shall define:

* Variable names
* Classification
* Required/optional status
* Usage
* Validation requirements

The repository shall not contain actual production secret values.

---

# Environment Variable Catalog

The following catalog defines the current and architectural configuration contract.

Variable names that are currently implemented are identified as active.

Variables that belong to future capabilities are explicitly marked as future.

---

# Application Runtime Variables

## `NODE_ENV`

**Classification:** Runtime Configuration

**Exposure:** Server/runtime

**Required:** Yes

**Purpose:**

Identifies the Node.js application execution mode.

Expected values include:

```text
development
production
test
```

The application shall not depend on arbitrary custom values for core runtime behavior.

---

# Application Environment Identifier

FluxDine should maintain an explicit deployment environment identifier separate from `NODE_ENV`.

Recommended variable:

```text
FLUXDINE_ENVIRONMENT
```

**Classification:** Runtime Configuration

**Exposure:** Server/runtime

**Required:** Yes in deployed environments

**Purpose:**

Identifies the FluxDine logical environment.

Expected values:

```text
development
testing
staging
production
```

For the current operational deployment:

```text
FLUXDINE_ENVIRONMENT=production
```

shall represent Initial Production.

The infrastructure resource name `fluxdine-staging` shall not determine this logical value.

---

# Application URL

Variable:

```text
NEXT_PUBLIC_APP_URL
```

**Classification:** Public Configuration

**Exposure:** Public

**Required:** Yes for deployed environments

**Purpose:**

Defines the canonical public application URL used by browser-facing functionality where required.

The value shall be environment-specific.

Example conceptual values:

```text
Development:
http://localhost:3000

Staging:
https://<staging-domain>

Production:
https://fluxdine.com
```

The exact production application routing may use platform-specific hostnames for individual platform surfaces.

---

# Canonical Brand URL

Variable:

```text
NEXT_PUBLIC_CANONICAL_URL
```

**Classification:** Public Configuration

**Exposure:** Public

**Required:** Recommended

**Purpose:**

Defines the canonical FluxDine brand URL.

Production value:

```text
https://fluxdine.com
```

The canonical URL shall not be derived from an arbitrary request hostname.

---

# Database Configuration

FluxDine currently uses Turso for Initial Production.

The database architecture is:

```text
Shared Database
       |
       v
Shared Schema
       |
       v
Tenant-Scoped Data
```

---

# `TURSO_CONNECTION_URL`

**Classification:** Secret / Infrastructure Configuration

**Exposure:** Server-only

**Required:** Yes for deployed environments using Turso

**Purpose:**

Defines the Turso database connection endpoint.

This value shall never be exposed to the browser.

---

# `TURSO_AUTH_TOKEN`

**Classification:** Secret

**Exposure:** Server-only

**Required:** Yes for deployed environments using authenticated Turso access

**Purpose:**

Authenticates the application against the Turso database.

The value shall:

* Never be committed to Git
* Never be returned by an API
* Never be logged
* Never be exposed to browser code

---

# Database Provider Identifier

Recommended variable:

```text
FLUXDINE_DATABASE_PROVIDER
```

**Classification:** Runtime Configuration

**Exposure:** Server-only

**Required:** Optional

**Current value:**

```text
turso
```

**Purpose:**

Identifies the active database provider.

This variable is useful for explicit provider selection and future migration planning.

It shall not itself provide database credentials.

---

# Future PostgreSQL Configuration

PostgreSQL is a future migration target.

PostgreSQL variables shall not be treated as active Initial Production configuration until the PostgreSQL migration is approved and implemented.

Potential future variables may include:

```text
DATABASE_URL
```

or provider-specific equivalents.

The exact PostgreSQL variable contract shall be defined during the approved PostgreSQL migration architecture phase.

---

# Object Storage Configuration

FluxDine uses Cloudflare R2 for production object storage.

---

# `R2_ACCOUNT_ID`

**Classification:** Infrastructure Configuration

**Exposure:** Server-only

**Required:** Yes where direct R2 API access requires it

**Purpose:**

Identifies the Cloudflare account associated with R2.

The value is not a secret by itself, but shall remain server-side unless there is an explicit reason to expose it.

---

# `R2_ACCESS_KEY_ID`

**Classification:** Secret

**Exposure:** Server-only

**Required:** Yes for direct R2 access using S3-compatible credentials

**Purpose:**

Identifies the R2 access credential.

It shall never be exposed to browser clients.

---

# `R2_SECRET_ACCESS_KEY`

**Classification:** Secret

**Exposure:** Server-only

**Required:** Yes for direct R2 access using S3-compatible credentials

**Purpose:**

Authenticates direct R2 operations.

It shall never be:

* Logged
* Committed
* Returned by an API
* Exposed to browser clients

---

# `R2_BUCKET_NAME`

**Classification:** Infrastructure Configuration

**Exposure:** Server-only

**Required:** Yes for active R2 integration

**Purpose:**

Identifies the R2 bucket used by the application environment.

Each environment shall use the appropriate isolated bucket or explicitly isolated namespace.

---

# `R2_PUBLIC_URL`

**Classification:** Public/Runtime Configuration

**Exposure:** Public only when intentionally used for public object access

**Required:** Optional

**Purpose:**

Defines the public object URL when a controlled public-access configuration exists.

Private objects shall not become publicly accessible merely because this variable exists.

---

# Email Configuration

Resend is the current production transactional email provider.

---

# `RESEND_API_KEY`

**Classification:** Secret

**Exposure:** Server-only

**Required:** Yes for production email functionality

**Purpose:**

Authenticates FluxDine against Resend.

The value shall never be exposed to the browser.

---

# `RESEND_FROM_EMAIL`

**Classification:** Runtime Configuration

**Exposure:** Server-only

**Required:** Yes when transactional email is active

**Purpose:**

Defines the default verified sender address for FluxDine transactional email.

The address shall belong to a properly configured sending domain.

---

# `RESEND_FROM_NAME`

**Classification:** Runtime Configuration

**Exposure:** Server-only

**Required:** Optional

**Purpose:**

Defines the display name associated with transactional email.

---

# Email Environment Safety

Development and testing shall not unintentionally send production customer email.

Environment-specific email configuration shall be used.

Production sender credentials shall not be available to development execution.

---

# Sentry Configuration

Sentry provides FluxDine application observability.

---

# `NEXT_PUBLIC_SENTRY_DSN`

**Classification:** Public Configuration

**Exposure:** Public

**Required:** Yes where Sentry client monitoring is enabled

**Purpose:**

Defines the Sentry DSN used by client-side monitoring.

A Sentry DSN is intentionally publishable when configured for client-side error monitoring.

It shall not be confused with the Sentry authentication token.

---

# `NEXT_PUBLIC_SENTRY_ENVIRONMENT`

**Classification:** Public Configuration

**Exposure:** Public

**Required:** Recommended

**Purpose:**

Identifies the Sentry environment associated with the application runtime.

Current Initial Production may use:

```text
staging
```

if that is the current operational Sentry environment configuration.

Future genuine Staging and Production environments should use unambiguous environment identifiers when their isolated deployments are introduced.

---

# `SENTRY_AUTH_TOKEN`

**Classification:** Secret

**Exposure:** Deployment/CI only

**Required:** Required when source-map upload or Sentry release operations are performed

**Purpose:**

Authenticates deployment tooling against Sentry.

This value shall not be available to browser runtime code.

---

# `SENTRY_ORG`

**Classification:** Deployment Configuration

**Exposure:** CI/deployment

**Required:** Required where Sentry release tooling uses organization identification

**Current organization:**

```text
fluxdine
```

This value is not itself a secret.

---

# `SENTRY_PROJECT`

**Classification:** Deployment Configuration

**Exposure:** CI/deployment

**Required:** Required where Sentry release tooling uses project identification

**Current project:**

```text
fluxdine-production
```

The project name shall not be changed merely because the current deployment is designated Initial Production.

---

# Domain Configuration

Domain resolution is an application concern backed by the DNS layer.

Cloudflare provides DNS.

FluxDine resolves:

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

# `FLUXDINE_CANONICAL_DOMAIN`

**Classification:** Runtime Configuration

**Exposure:** Server/runtime

**Required:** Yes for production domain logic

**Production value:**

```text
fluxdine.com
```

Purpose:

Defines the canonical FluxDine domain.

---

# `FLUXDINE_HQ_DOMAIN`

**Classification:** Runtime Configuration

**Exposure:** Server/runtime

**Required:** Yes where HQ hostname routing is active

**Production value:**

```text
app.fluxdine.com
```

---

# `FLUXDINE_SIGNUP_DOMAIN`

**Classification:** Runtime Configuration

**Exposure:** Server/runtime

**Required:** Yes where Self-Service hostname routing is active

**Production value:**

```text
signup.fluxdine.com
```

---

# `FLUXDINE_RESTAURANT_DOMAIN`

**Classification:** Runtime Configuration

**Exposure:** Server/runtime

**Required:** Yes where default restaurant hostname routing is active

**Production conceptual pattern:**

```text
{restaurant}.fluxdine.com
```

The variable shall represent the domain base rather than a specific restaurant.

---

# Custom Domain Configuration

Custom restaurant-owned domains are architecturally supported.

Custom-domain automation is deferred.

The environment-variable system shall not be used as the primary tenant mapping mechanism for custom domains.

Hostname-to-restaurant resolution shall remain application/database driven.

---

# Authentication Configuration

Authentication-related configuration may include:

* Session configuration
* Token signing configuration
* Password hashing configuration
* Verification configuration
* Application secrets

These values shall be server-only.

---

# Application Authentication Secret

Where the authentication implementation requires an application secret, it shall be supplied through a server-only environment variable.

Example:

```text
AUTH_SECRET
```

or the exact variable required by the selected authentication implementation.

The exact active variable name shall be determined by the implemented authentication layer.

No authentication secret shall be hardcoded.

---

# Email Verification Configuration

Email verification workflows may require:

* Application base URL
* Verification signing secret
* Token expiry configuration
* Email provider credentials

Token-signing secrets shall remain server-only.

Verification URLs may contain short-lived signed tokens, but the signing secret shall never be exposed.

---

# Cron Configuration

Scheduled application tasks may require authentication.

---

# `CRON_SECRET`

**Classification:** Secret

**Exposure:** Server-only / deployment

**Required:** Yes for authenticated scheduled endpoints

**Purpose:**

Authenticates authorized scheduled-job requests.

The value shall:

* Be stored as a secret
* Never be committed
* Never be logged
* Never be returned to clients

---

# Cron Frequency

Cron frequency is deployment configuration rather than an application secret.

The currently deployed Vercel configuration may use a temporary schedule appropriate to the current Vercel plan.

The Deployment Specification and CI/CD configuration shall define the actual schedule.

Environment variables shall not be used to override production cron frequency unless explicitly required.

---

# Payment Configuration

FluxDine uses a shared Payment Service abstraction.

Current Phase 07–08 restaurant commerce testing uses **Demo Payment Gateway**.

```text
FLUXDINE_PAYMENT_PROVIDER
```

Current implementation default: `test` (`TestPaymentProvider`).
Target architectural identifier: `demo`.

Stripe execution is **future**. `stripe` must remain inactive until after Phase 08 and an approved Connect/live implementation. Stripe Test Mode is not the Demo Payment mechanism.

Stripe execution was intentionally deferred from earlier implementation stages and shall only become active after its architecture and integration requirements are approved.

---

# Stripe Configuration

Potential future Stripe variables may include:

```text
STRIPE_SECRET_KEY
STRIPE_PUBLISHABLE_KEY
STRIPE_WEBHOOK_SECRET
STRIPE_CONNECT_CLIENT_ID
```

These are architectural placeholders unless explicitly activated by the Stripe implementation.

---

# Stripe Secret Rules

When Stripe is activated:

* Secret keys shall remain server-only
* Publishable keys may be exposed where Stripe requires client-side initialization
* Webhook secrets shall remain server-only
* Connected-account identifiers shall not be treated as secret credentials
* Restaurant owners shall not be required to provide raw Stripe secret API keys to FluxDine

The exact variable catalog shall be finalized in the Payment implementation specification.

---

# Feature Configuration

Feature flags may be provided through:

* Database-backed feature configuration
* Server configuration
* Environment variables for infrastructure-level flags

Environment variables shall not replace the centralized feature-flag service for tenant-specific or restaurant-specific feature control.

Environment variables are appropriate for infrastructure-level behavior that applies to an entire deployment environment.

---

# Example Infrastructure-Level Feature Flag

A future variable may follow:

```text
FLUXDINE_FEATURE_<FEATURE_NAME>
```

Such variables shall only be introduced when a feature requires environment-wide configuration.

Tenant-specific flags shall remain in the platform feature configuration system.

---

# Logging Configuration

Environment variables may configure logging behavior.

Potential variables include:

```text
LOG_LEVEL
```

**Classification:** Runtime Configuration

**Exposure:** Server-only

**Purpose:**

Controls the minimum application logging level.

Production logging shall avoid sensitive information regardless of log level.

---

# Debug Configuration

Development may support:

```text
DEBUG
```

or framework-specific debugging configuration.

Debugging shall not be enabled in Initial Production unless explicitly required.

Production errors shall be sanitized.

---

# Rate Limiting Configuration

If environment-level rate limiting configuration is required, values may be supplied through server-only variables.

Example conceptual variables:

```text
RATE_LIMIT_ENABLED
RATE_LIMIT_REQUESTS
RATE_LIMIT_WINDOW
```

Exact implementation shall be defined by the relevant security/backend specification.

Tenant-specific limits shall not be implemented solely through environment variables.

---

# URL and Redirect Configuration

Redirect behavior may use environment-specific URLs.

Examples:

```text
NEXT_PUBLIC_APP_URL
FLUXDINE_CANONICAL_DOMAIN
FLUXDINE_HQ_DOMAIN
FLUXDINE_SIGNUP_DOMAIN
```

Redirect targets shall be validated to prevent open-redirect vulnerabilities.

---

# Environment Variable Naming Standard

FluxDine variable names shall follow:

```text
UPPERCASE_SNAKE_CASE
```

Examples:

```text
TURSO_CONNECTION_URL
TURSO_AUTH_TOKEN
RESEND_API_KEY
R2_BUCKET_NAME
CRON_SECRET
```

---

# Prefix Standards

| Prefix         | Purpose                                     |
| -------------- | ------------------------------------------- |
| `NEXT_PUBLIC_` | Intentionally browser-exposed configuration |
| `FLUXDINE_`    | FluxDine-specific application configuration |
| `TURSO_`       | Turso database configuration                |
| `R2_`          | Cloudflare R2 configuration                 |
| `RESEND_`      | Resend email configuration                  |
| `SENTRY_`      | Sentry deployment configuration             |
| `STRIPE_`      | Stripe payment configuration                |

Provider prefixes shall only be used for provider-specific configuration.

---

# Secret Naming

Secret variables should make their credential nature apparent.

Examples:

```text
TURSO_AUTH_TOKEN
RESEND_API_KEY
R2_SECRET_ACCESS_KEY
SENTRY_AUTH_TOKEN
CRON_SECRET
```

A secret shall not be disguised as ordinary configuration.

---

# Required Variables

A variable is **required** when the application cannot safely perform its intended environment role without it.

Required variables shall be validated before the affected functionality becomes active.

---

# Optional Variables

Optional variables may be omitted when:

* The related feature is disabled
* A safe default exists
* The variable applies only to a future capability
* The relevant integration is not active

Optional variables shall have documented behavior when absent.

---

# Missing Required Variables

If a required server configuration variable is missing:

```text
Application Startup
       |
       v
Configuration Validation
       |
       v
Missing Required Variable
       |
       v
Fail Safely
```

The application should fail fast where continuing without the variable would create incorrect behavior or a security risk.

---

# Missing Optional Variables

If an optional variable is missing:

* Use a documented safe default where appropriate
* Disable the dependent functionality where appropriate
* Do not silently create insecure behavior

---

# Client-Side Exposure Rules

The following rule is mandatory:

```text
If a variable is not explicitly classified as public,
it shall not be exposed to browser code.
```

Only intentionally public configuration may use:

```text
NEXT_PUBLIC_
```

---

# Server-Only Enforcement

Server-only variables shall only be accessed from server-side code.

They shall not be imported into:

* Client Components
* Browser bundles
* Public API responses
* Client-side configuration objects

---

# Build-Time vs Runtime Configuration

FluxDine shall distinguish between:

```text
Build-Time Configuration
```

and:

```text
Runtime Configuration
```

Some Next.js configuration may be embedded during build.

Therefore, environment variables prefixed with:

```text
NEXT_PUBLIC_
```

must be treated as potentially embedded into the client bundle during build.

Changing such values may require a new deployment.

---

# Runtime Secret Configuration

Server-side secrets shall be injected into the runtime environment through the hosting platform or approved secret mechanism.

The application shall not require secrets to be written into the repository.

---

# Configuration Precedence

Configuration precedence shall follow the hosting platform and framework's documented environment behavior.

Conceptually:

```text
Environment-Specific Deployment Configuration
             |
             v
Runtime Environment Variables
             |
             v
Application Configuration Loader
             |
             v
Application Runtime
```

Repository files such as `.env.example` may document variable names but shall not contain production secrets.

---

# Local `.env` Files

Local development may use:

```text
.env.local
```

or the framework's supported local environment configuration.

Local secret files shall be excluded from Git.

---

# `.env.example`

The repository should maintain a safe template such as:

```text
.env.example
```

The template may contain:

* Variable names
* Example non-secret values
* Comments
* Required/optional indicators

It shall not contain real credentials.

---

# `.env` Git Policy

The following shall not be committed:

```text
.env
.env.local
.env.production
.env.staging
```

or equivalent files containing real credentials.

Only safe templates may be committed.

---

# Development Configuration

Development configuration should prioritize:

* Developer productivity
* Safe defaults
* Local resources
* Test data
* Debugging

Development shall not require production credentials.

---

# Testing Configuration

Testing configuration shall prioritize:

* Isolation
* Deterministic behavior
* Repeatability
* Automated execution

Testing resources shall not modify production data.

---

# Staging Configuration

When Staging is introduced:

* Staging variables shall be separate
* Staging secrets shall be separate
* Staging database credentials shall be separate
* Staging R2 credentials shall be separate
* Staging email configuration shall be separate
* Staging observability configuration shall be separate

---

# Initial Production Configuration

Initial Production configuration shall be stored in the approved production configuration system.

Current hosting:

```text
Vercel
```

Current production resource:

```text
fluxdine-staging
```

The environment must be treated as Production despite the project name.

---

# Production Configuration Requirements

Production configuration shall:

* Use production resources
* Use production secrets
* Disable development-only debugging
* Enable required observability
* Enable required security controls
* Use canonical production domains
* Use production database credentials
* Use production object storage
* Use production email configuration

---

# Environment Variable Validation

Configuration should be validated centrally rather than through scattered checks throughout the application.

The preferred conceptual architecture is:

```text
Environment Variables
       |
       v
Configuration Validation
       |
       v
Typed Application Configuration
       |
       +--------+--------+
       |        |        |
       v        v        v
 Database     Email     Storage
 Config       Config     Config
```

---

# Typed Configuration

Where practical, environment variables should be parsed into typed configuration before use.

Examples:

```text
string
boolean
number
enum
URL
```

String values representing booleans or numbers shall be parsed explicitly.

---

# Boolean Variables

Boolean environment variables shall use an explicit documented representation.

Preferred values:

```text
true
false
```

The application shall not treat arbitrary strings as booleans without validation.

---

# Numeric Variables

Numeric environment variables shall be validated as numbers.

Examples:

```text
RATE_LIMIT_REQUESTS
RATE_LIMIT_WINDOW
```

Invalid numeric values shall cause configuration validation failure when the variable is required.

---

# URL Variables

URL environment variables shall be validated as valid URLs where appropriate.

Examples:

```text
NEXT_PUBLIC_APP_URL
NEXT_PUBLIC_CANONICAL_URL
TURSO_CONNECTION_URL
```

---

# Enum Variables

Environment variables representing a finite set of values shall be validated against an allowlist.

Example:

```text
FLUXDINE_ENVIRONMENT
```

Allowed values:

```text
development
testing
staging
production
```

---

# Configuration Validation Timing

Configuration validation should occur as early as practical.

Critical server configuration should be validated during application startup or first controlled server initialization.

Client-side public configuration should be validated during build where practical.

---

# Error Messages

Configuration validation errors shall identify:

* Missing variable name
* Invalid variable type
* Invalid allowed value

They shall not reveal:

* Secret values
* Tokens
* Passwords
* Credentials

---

# Production Error Sanitization

Production configuration errors exposed to end users shall not reveal internal environment details.

For example, the application shall not return:

```text
TURSO_AUTH_TOKEN is missing
```

through a public API response.

Such details belong in protected deployment/application logs.

---

# Configuration Logging

Applications shall never log secret values.

Safe configuration diagnostics may identify:

```text
Variable configured: yes/no
Environment: production
Provider: turso
```

but shall not print:

```text
TURSO_AUTH_TOKEN=<secret>
```

---

# Secret Redaction

Logs, monitoring events, and error reports shall redact or exclude:

* API keys
* Authorization headers
* Cookies
* Tokens
* Passwords
* Webhook secrets
* Database credentials

---

# Configuration Rotation

Configuration changes involving secrets shall follow the Environment & Secrets Strategy.

Secret rotation should support:

```text
Create New Credential
       |
       v
Deploy New Credential
       |
       v
Verify
       |
       v
Revoke Old Credential
```

Rotation shall minimize service interruption.

---

# Emergency Secret Rotation

If a secret is compromised:

1. Revoke or invalidate the compromised secret.
2. Generate a replacement.
3. Update the affected environment.
4. Redeploy if required.
5. Verify service functionality.
6. Investigate exposure.
7. Review logs and affected systems.
8. Record the incident.

---

# Environment Variable Access Boundaries

The following boundaries apply:

```text
Client
  |
  +--> Public variables only

Server Runtime
  |
  +--> Public variables
  +--> Server configuration
  +--> Required secrets

CI/CD
  |
  +--> Build configuration
  +--> Deployment configuration
  +--> Required deployment secrets

Database
  |
  +--> Database credentials only through application/server boundary
```

---

# Application Modules

Application modules should not access raw environment variables throughout business logic.

Preferred pattern:

```text
Environment
    |
    v
Configuration Layer
    |
    v
Typed Configuration
    |
    v
Infrastructure Service
    |
    v
Business Module
```

This reduces configuration coupling and makes testing easier.

---

# Provider Configuration Isolation

Provider-specific environment variables should be consumed by the corresponding infrastructure/shared service.

Example:

```text
TURSO_*
   |
   v
Database Service

R2_*
   |
   v
File/Object Storage Service

RESEND_*
   |
   v
Email Service

SENTRY_*
   |
   v
Observability Integration

STRIPE_*
   |
   v
Payment Service
```

Business modules should not directly depend on provider credentials.

---

# Testing Provider Integrations

Tests should use:

* Mock providers
* Test providers
* Isolated test credentials
* Local implementations where appropriate

Tests shall not require production credentials.

---

# CI/CD Integration

CI/CD shall inject environment-specific configuration into deployment jobs.

The CI/CD system shall:

* Avoid printing secrets
* Use protected secret stores
* Restrict production credentials
* Use environment-specific credentials
* Validate required configuration
* Preserve deployment traceability

Detailed CI/CD implementation belongs to the CI/CD Pipeline specification.

---

# Vercel Environment Mapping

Vercel supports environment-specific variables.

FluxDine shall conceptually map:

```text
Vercel Development
       |
       v
Development Configuration

Vercel Preview
       |
       v
Testing / Non-Production Configuration

Vercel Production
       |
       v
Production Configuration
```

Preview deployments shall not receive Initial Production database or secret credentials by default.

---

# Preview Deployment Rule

Preview deployments must not unintentionally mutate Initial Production resources.

Therefore, Preview should not receive:

* Production Turso credentials
* Production R2 credentials
* Production Resend credentials
* Production payment secrets

unless an explicitly approved controlled workflow requires it.

---

# Production Variable Changes

Changes to production environment variables shall be treated as production changes.

This includes:

* Adding variables
* Removing variables
* Changing credentials
* Changing URLs
* Changing provider configuration
* Changing security configuration

Production configuration changes require appropriate authorization.

---

# Environment Variable Versioning

Variable names form part of the application configuration contract.

Renaming a variable shall be treated as a compatibility-impacting change.

When renaming a variable:

1. Update the configuration specification.
2. Update application code.
3. Update deployment configuration.
4. Validate the new configuration.
5. Remove the old variable after migration.
6. Document the change.

---

# Deprecating Variables

Deprecated variables shall not be removed immediately if active deployments still depend on them.

The preferred lifecycle is:

```text
Active
  |
  v
Deprecated
  |
  v
Migration
  |
  v
Removed
```

---

# Unknown Variables

The application should avoid silently relying on undocumented environment variables.

Important variables shall be documented in this specification.

Provider tooling may introduce provider-defined variables that are documented separately where required.

---

# Environment Variable Matrix

| Variable                         |      Public |    Secret |      Dev |     Test |  Staging | Production |
| -------------------------------- | ----------: | --------: | -------: | -------: | -------: | ---------: |
| `NODE_ENV`                       |          No |        No |      Yes |      Yes |      Yes |        Yes |
| `FLUXDINE_ENVIRONMENT`           |          No |        No |      Yes |      Yes |      Yes |        Yes |
| `NEXT_PUBLIC_APP_URL`            |         Yes |        No |      Yes |      Yes |      Yes |        Yes |
| `NEXT_PUBLIC_CANONICAL_URL`      |         Yes |        No |      Yes |      Yes |      Yes |        Yes |
| `TURSO_CONNECTION_URL`           |          No | Sensitive |     Yes* |     Yes* |      Yes |        Yes |
| `TURSO_AUTH_TOKEN`               |          No |       Yes |     Yes* |     Yes* |      Yes |        Yes |
| `R2_ACCOUNT_ID`                  |          No |        No | Optional | Optional |      Yes |        Yes |
| `R2_ACCESS_KEY_ID`               |          No |       Yes | Optional |     Test |      Yes |        Yes |
| `R2_SECRET_ACCESS_KEY`           |          No |       Yes | Optional |     Test |      Yes |        Yes |
| `R2_BUCKET_NAME`                 |          No |        No | Optional |     Test |      Yes |        Yes |
| `R2_PUBLIC_URL`                  | Conditional |        No | Optional | Optional | Optional |   Optional |
| `RESEND_API_KEY`                 |          No |       Yes |  No/Mock |     Test |      Yes |        Yes |
| `RESEND_FROM_EMAIL`              |          No |        No |     Test |     Test |  Staging | Production |
| `RESEND_FROM_NAME`               |          No |        No | Optional | Optional |      Yes |        Yes |
| `NEXT_PUBLIC_SENTRY_DSN`         |         Yes |        No | Optional | Optional |      Yes |        Yes |
| `NEXT_PUBLIC_SENTRY_ENVIRONMENT` |         Yes |        No |      Yes |      Yes |      Yes |        Yes |
| `SENTRY_AUTH_TOKEN`              |          No |       Yes |       No |       CI |       CI |         CI |
| `SENTRY_ORG`                     |          No |        No |       No |       CI |       CI |         CI |
| `SENTRY_PROJECT`                 |          No |        No |       No |       CI |       CI |         CI |
| `FLUXDINE_CANONICAL_DOMAIN`      |          No |        No |      Yes |      Yes |      Yes |        Yes |
| `FLUXDINE_HQ_DOMAIN`             |          No |        No | Optional | Optional |      Yes |        Yes |
| `FLUXDINE_SIGNUP_DOMAIN`         |          No |        No | Optional | Optional |      Yes |        Yes |
| `FLUXDINE_RESTAURANT_DOMAIN`     |          No |        No | Optional | Optional |      Yes |        Yes |
| `CRON_SECRET`                    |          No |       Yes | Optional |     Test |      Yes |        Yes |
| `AUTH_SECRET`                    |          No |       Yes |      Yes |     Test |      Yes |        Yes |
| `STRIPE_SECRET_KEY`              |          No |       Yes |       No |     Test |   Future |     Future |
| `STRIPE_PUBLISHABLE_KEY`         |         Yes |        No |       No |     Test |   Future |     Future |
| `STRIPE_WEBHOOK_SECRET`          |          No |       Yes |       No |     Test |   Future |     Future |
| `STRIPE_CONNECT_CLIENT_ID`       |          No | Sensitive |       No |     Test |   Future |     Future |
| `LOG_LEVEL`                      |          No |        No |      Yes |      Yes |      Yes |        Yes |

`*` Development and Testing database credentials must refer to isolated non-production resources.

---

# Current Active Initial Production Variables

The exact currently configured production values are managed outside this document.

The currently relevant categories are:

```text
Application
    |
    +--> Application URL
    +--> Environment Identifier

Database
    |
    +--> Turso connection
    +--> Turso authentication

Object Storage
    |
    +--> R2 configuration
    +--> R2 credentials

Email
    |
    +--> Resend configuration
    +--> Resend credential

Observability
    |
    +--> Sentry DSN
    +--> Sentry environment
    +--> Sentry deployment credentials

Application Security
    |
    +--> Authentication secret
    +--> Cron secret

Domain
    |
    +--> Canonical domain
    +--> Platform domains
```

Actual secret values shall never be documented here.

---

# Configuration Documentation Rule

This document shall describe:

* Variable names
* Purpose
* Classification
* Required/optional behavior
* Environment applicability

This document shall never contain:

* Real secret values
* Production API keys
* Passwords
* Authentication tokens
* Private keys

---

# Security Rules

## Rule ENV-001

Secrets shall never be committed to source control.

---

## Rule ENV-002

Only explicitly public variables may use `NEXT_PUBLIC_`.

---

## Rule ENV-003

Database credentials shall remain server-side.

---

## Rule ENV-004

Provider secret keys shall remain server-side.

---

## Rule ENV-005

Production secrets shall not be supplied to Preview environments by default.

---

## Rule ENV-006

Environment variables shall be isolated by environment.

---

## Rule ENV-007

Configuration validation shall not reveal secret values.

---

## Rule ENV-008

Application logs shall not contain secret values.

---

## Rule ENV-009

Client-side code shall not access server-only environment variables.

---

## Rule ENV-010

Environment-specific resources shall be used instead of production resources for testing.

---

## Rule ENV-011

Environment variable names shall follow the documented naming standard.

---

## Rule ENV-012

Provider credentials shall be consumed through the appropriate infrastructure/shared service boundary.

---

## Rule ENV-013

Production configuration changes require appropriate authorization.

---

## Rule ENV-014

The current Vercel project named `fluxdine-staging` shall be treated as Initial Production.

---

## Rule ENV-015

Future Staging configuration shall use independently isolated resources and secrets.

---

# Architecture Decision Records

## ADR-ENV-001

Environment configuration is external to application source code.

---

## ADR-ENV-002

Secrets are never committed to the repository.

---

## ADR-ENV-003

Client-visible environment variables require explicit `NEXT_PUBLIC_` classification.

---

## ADR-ENV-004

Server-only configuration is not exposed to browser clients.

---

## ADR-ENV-005

Environment variables are validated through a centralized configuration boundary where practical.

---

## ADR-ENV-006

FluxDine defines Development, Testing, Staging, and Production as logical environment roles.

---

## ADR-ENV-007

Initial Production is the current operational production environment.

---

## ADR-ENV-008

The Vercel project `fluxdine-staging` is designated Initial Production despite its resource name.

---

## ADR-ENV-009

Preview deployments shall not receive production secrets by default.

---

## ADR-ENV-010

Provider-specific credentials remain within the appropriate infrastructure/shared service boundary.

---

## ADR-ENV-011

Turso is the current Initial Production database provider.

---

## ADR-ENV-012

Cloudflare R2 is the current Initial Production object storage provider.

---

## ADR-ENV-013

Resend is the current Initial Production transactional email provider.

---

## ADR-ENV-014

Sentry is the current application observability provider.

---

## ADR-ENV-015

Stripe configuration remains future/inactive until Stripe payment execution is explicitly implemented and approved.

---

# Appendix A — Configuration Classification

```text
Public
   |
   +--> Safe for browser exposure

Server
   |
   +--> Server runtime only

Secret
   |
   +--> Protected credential

Build/Deployment
   |
   +--> CI/deployment-specific configuration
```

---

# Appendix B — Configuration Flow

```text
Environment / Secret Store
          |
          v
Deployment Platform
          |
          v
Application Runtime
          |
          v
Configuration Loader
          |
          v
Typed Configuration
          |
     +----+----+----+----+
     |    |    |    |    |
     v    v    v    v    v
    DB   R2  Email Sentry Auth
```

---

# Appendix C — Client/Server Boundary

```text
                    FluxDine Application
                           |
              +------------+------------+
              |                         |
              v                         v
        Browser Client             Server Runtime
              |                         |
              v                         |
     NEXT_PUBLIC_* only                 |
                                        v
                              Server Configuration
                                        |
                                        v
                                    Secrets
```

---

# Appendix D — Environment Separation

```text
                 FluxDine Source
                       |
        +--------------+--------------+
        |              |              |
        v              v              v
 Development        Testing       Production
        |              |              |
        v              v              v
 Dev Config       Test Config    Prod Config
        |              |              |
        v              v              v
 Dev Resources    Test Resources  Prod Resources
```

Future:

```text
                 FluxDine Source
                       |
        +--------------+--------------------+
        |              |                    |
        v              v                    v
 Development        Testing              Staging
                                             |
                                             v
                                         Production
```

---

# Appendix E — Secret Flow

```text
Secret Management System
          |
          v
Environment-Specific Secret
          |
          v
Deployment Platform
          |
          v
Server Runtime
          |
          v
Infrastructure Service
```

Secrets shall never flow into:

```text
Git Repository
Browser Bundle
Public API Response
Client Storage
Application Logs
```

---

# Appendix F — Current Provider Mapping

| Configuration Area  | Provider      |
| ------------------- | ------------- |
| Application Hosting | Vercel        |
| Database            | Turso         |
| Object Storage      | Cloudflare R2 |
| Email               | Resend        |
| Observability       | Sentry        |
| DNS                 | Cloudflare    |
| Source Control      | GitHub        |

---

# Appendix G — Future Provider Migration

If FluxDine migrates from Turso to PostgreSQL:

```text
Current
TURSO_*
   |
   v
Turso

Future
PostgreSQL Configuration
   |
   v
PostgreSQL
```

The migration shall:

* Introduce the new configuration contract
* Maintain compatibility during migration
* Validate all environments
* Protect production data
* Remove old variables only after migration completion
* Update this document

---

# Appendix H — Variable Lifecycle

```text
Proposed
   |
   v
Implemented
   |
   v
Active
   |
   v
Deprecated
   |
   v
Removed
```

Every configuration variable should have a documented owner and purpose.

---

# Appendix I — Production Configuration Checklist

Before production deployment:

```text
[ ] Required variables defined
[ ] Required secrets defined
[ ] Production database credentials verified
[ ] Production R2 credentials verified
[ ] Production Resend credentials verified
[ ] Sentry configuration verified
[ ] Authentication secrets verified
[ ] Cron authentication verified
[ ] Domain configuration verified
[ ] Public variables reviewed
[ ] No secret exposed through NEXT_PUBLIC_
[ ] Preview environments isolated
[ ] Configuration validation passes
```

---

# Appendix J — Developer Configuration Checklist

Before local development:

```text
[ ] `.env.local` created where required
[ ] `.env.local` ignored by Git
[ ] Development resources configured
[ ] Production credentials not copied
[ ] Required variables available
[ ] Configuration validation passes
[ ] Application starts successfully
```

---

# Appendix K — Environment Variable Naming Examples

Correct:

```text
TURSO_CONNECTION_URL
TURSO_AUTH_TOKEN
R2_BUCKET_NAME
R2_SECRET_ACCESS_KEY
RESEND_API_KEY
CRON_SECRET
FLUXDINE_ENVIRONMENT
NEXT_PUBLIC_APP_URL
```

Incorrect:

```text
tursoUrl
turso-token
databasePassword
publicSecret
my_key
```

---

# Appendix L — Provider Credential Boundary

```text
                    FluxDine
                       |
        +--------------+--------------+
        |              |              |
        v              v              v
   Database        Storage          Email
   Service         Service          Service
        |              |              |
        v              v              v
    TURSO_*          R2_*          RESEND_*
```

Business modules shall interact with the service abstraction rather than directly reading provider credentials.

---

# References

* `00 Environment & Secrets Strategy.md`
* `01 Deployment Specification.md`
* Infrastructure Architecture
* Security Architecture
* Backend Engineering Specifications
* Frontend Engineering Specifications
* Database Engineering Specifications
* Database Migration Strategy
* Shared Platform Services
* Payment Architecture
* Monitoring
* Logging
* Backup Strategy
* Disaster Recovery
* CI/CD Pipeline

---

# Revision History

| Version | Date       | Author               | Description                                 |
| ------- | ---------- | -------------------- | ------------------------------------------- |
| 1.1     | 2026-09-13 | FluxDine Engineering | Current FLUXDINE_PAYMENT_PROVIDER is Demo/`test`; Stripe remains future. |
| 1.0     | 2026-09-12 | FluxDine Engineering | Initial Environment Variables specification |

---

# Final Document State

**Version:** 1.1

**Status:** Approved and Locked

**Authority:** Authoritative environment-variable and runtime-configuration contract for FluxDine.

**Implementation Rule:** Actual secret values must remain outside this document and outside source control.

**Architecture Principle:** Environment variables configure deployment-specific behavior; they must not be used to bypass FluxDine's tenant isolation, service abstractions, security architecture, or environment boundaries.

````
