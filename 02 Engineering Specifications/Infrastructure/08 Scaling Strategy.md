# 02 Engineering Specifications

# Infrastructure

# 08 — Scaling Strategy

---

# Document Control

| Field | Value |
|---|---|
| Document ID | FD-ENG-INF-008 |
| Document Name | Scaling Strategy |
| Version | 1.2 |
| Status | Approved and Locked |
| Classification | Engineering Specification |
| Architecture Area | Infrastructure |
| Phase | Phase 07 — Infrastructure & Production Readiness |
| Owner | FluxDine Engineering |
| Authority | FluxDine Architecture Bible |
| Review Frequency | Review when workload, infrastructure, or provider architecture materially changes |

---

# 1. Purpose

This document defines the scaling strategy for the FluxDine platform.

The purpose of this specification is to establish how FluxDine scales as the number of tenants, restaurants, branches, users, orders, API requests, stored objects, scheduled workloads, and external-provider interactions increases.

The scaling strategy is designed to:

- preserve tenant isolation;
- preserve application correctness;
- preserve data consistency;
- maintain predictable performance;
- use managed infrastructure where appropriate;
- scale infrastructure according to measured workload;
- avoid premature infrastructure complexity;
- identify measurable scaling triggers;
- define a controlled path from initial production to higher-scale architecture;
- preserve the ability to migrate infrastructure providers when justified;
- prevent a single tenant or workload from degrading the platform;
- maintain security and operational controls during scaling.

Scaling decisions must be evidence-driven.

FluxDine must not introduce distributed infrastructure solely because it is theoretically capable of supporting higher scale.

The architecture must first use the capabilities of the existing managed platform effectively and introduce additional infrastructure only when measured workload or reliability requirements justify it.

---

# 2. Scope

This specification covers:

- application scaling;
- Vercel application execution;
- serverless workload scaling;
- database scaling;
- tenant-aware scaling;
- restaurant and branch workload growth;
- object-storage scaling;
- email-provider scaling;
- scheduled workload scaling;
- caching strategy;
- background-processing strategy;
- capacity planning;
- scaling signals;
- scaling triggers;
- noisy-neighbor protection;
- scaling stages;
- future infrastructure evolution;
- performance and availability considerations;
- scaling-related security requirements.

---

# 3. Out of Scope

This document does not define:

- detailed database schema design;
- database naming conventions;
- API contracts;
- authentication implementation;
- payment business rules;
- DNS implementation;
- deployment procedures;
- CI/CD implementation;
- detailed disaster recovery procedures;
- detailed backup implementation;
- application feature requirements.

Those concerns are defined by their respective architecture and engineering specifications.

This document may reference those systems where their behavior affects scaling.

---

# 4. Scaling Philosophy

FluxDine follows a managed-first and evidence-driven scaling philosophy.

The platform should scale through the capabilities of its existing managed providers before introducing additional distributed infrastructure.

The initial architecture is intentionally simple:

- Vercel hosts and executes the Next.js application;
- Turso provides the production database;
- the database uses a shared-database/shared-schema tenant model;
- Cloudflare provides the DNS layer;
- Cloudflare R2 provides object storage;
- Resend provides email delivery;
- Sentry provides application error monitoring and observability;
- scheduled application workloads use the available Vercel Cron capability (Hobby currently once daily; this is **not** the database backup scheduler);
- database backups follow ADR-055 (GitHub Actions, twice-daily logical dumps to a dedicated private R2 bucket);
- no dedicated distributed cache is required initially;
- no dedicated queue is required initially;
- no dedicated worker cluster is required initially;
- no Kubernetes or VM infrastructure is required initially.

Scaling must therefore occur progressively.

The preferred order is:

1. measure;
2. optimize;
3. remove inefficient workload patterns;
4. improve indexes and queries;
5. improve application execution;
6. use managed-provider scaling capabilities;
7. isolate heavy workloads;
8. introduce asynchronous processing where justified;
9. introduce caching where correctness permits;
10. migrate infrastructure only when the current platform becomes a demonstrated constraint.

---

# 5. Scaling Principles

## SCALE-001 — Evidence-Driven Scaling

Infrastructure changes must be based on observed workload, performance, reliability, or capacity requirements.

The platform must not introduce infrastructure solely based on theoretical future scale.

---

## SCALE-002 — Managed Infrastructure First

FluxDine should prefer managed infrastructure over self-managed infrastructure whenever the managed solution satisfies the required workload and reliability characteristics.

---

## SCALE-003 — Stateless Application Execution

Application execution should remain stateless wherever practical.

Requests must not depend on:

- local process memory;
- local filesystem persistence;
- a specific application instance;
- a specific Vercel execution environment.

Persistent state must reside in durable platform services.

---

## SCALE-004 — Tenant Isolation Must Survive Scaling

Scaling must never weaken tenant isolation.

Increasing platform capacity must not introduce a mechanism through which:

- one tenant can access another tenant's data;
- one restaurant can access another restaurant's resources;
- tenant-scoped configuration becomes globally mutable;
- background processing loses tenant context;
- cached data crosses tenant boundaries.

---

## SCALE-005 — Optimize Before Distributing

Before introducing queues, workers, caches, replicas, or additional services, FluxDine should first verify:

- query efficiency;
- indexes;
- pagination;
- payload size;
- unnecessary API calls;
- repeated computation;
- database access patterns;
- object-storage access patterns;
- email workload;
- scheduled workload efficiency.

---

## SCALE-006 — Avoid Premature Distribution

Distributed infrastructure introduces:

- operational complexity;
- failure modes;
- consistency considerations;
- observability requirements;
- deployment complexity;
- additional security boundaries.

Distributed infrastructure must therefore be introduced only when justified by measured requirements.

---

## SCALE-007 — Correctness Before Performance

Performance improvements must not compromise:

- authorization;
- tenant isolation;
- transactional correctness;
- payment correctness;
- order correctness;
- reservation correctness;
- auditability;
- data durability.

---

## SCALE-008 — Externalize Durable State

Important application state must not depend on ephemeral execution environments.

Durable state must use appropriate persistent services such as:

- Turso;
- R2;
- provider-managed systems;
- future durable infrastructure where explicitly approved.

---

## SCALE-009 — Noisy-Neighbor Protection

The workload generated by one tenant must not be allowed to degrade the platform disproportionately.

Where required, FluxDine may introduce:

- rate limits;
- quotas;
- workload controls;
- request prioritization;
- asynchronous processing;
- tenant-level operational controls.

These controls must be introduced according to measured workload.

---

## SCALE-010 — Observable Scaling

Scaling decisions must be supported by observability.

Relevant signals must be available through the platform's monitoring and operational tooling.

---

## SCALE-011 — Provider-Aware Scaling

Scaling must account for the limits and capabilities of the underlying providers.

Provider limitations must be treated as architectural constraints when they materially affect workload capacity.

---

## SCALE-012 — Incremental Evolution

Scaling architecture must evolve incrementally.

A more complex architecture should be introduced only when the current architecture reaches a measurable operational, performance, reliability, or capacity boundary.

---

# 6. Current Scaling Architecture

The current FluxDine architecture uses managed services.

The current high-level scaling path is:

```text
Client
   |
   v
Cloudflare DNS
   |
   v
Vercel
Next.js Application
Serverless Execution
   |
   +--------------------+
   |                    |
   v                    v
Turso                  R2
Database               Object Storage
   |
   +--------------------+
   |
   v
Resend / Sentry / Other Shared Services
````

Scheduled workloads may execute through Vercel Cron.

There is currently no requirement for:

* dedicated load balancers;
* dedicated frontend servers;
* dedicated backend servers;
* dedicated worker servers;
* distributed queue infrastructure;
* dedicated distributed cache infrastructure;
* Kubernetes;
* self-managed virtual machines;
* database read replicas.

Those capabilities remain future architectural options.

---

# 7. Application Scaling

## 7.1 Current Application Model

FluxDine is implemented as a Next.js application deployed through Vercel.

Application requests are executed through Vercel-managed infrastructure.

The application should therefore be designed to scale horizontally through additional serverless execution capacity rather than through manually managed application instances.

---

## 7.2 Stateless Request Processing

Application requests should not rely on local process state.

The following patterns should be avoided:

* storing business state in process memory;
* relying on a specific server instance;
* writing durable files to local execution storage;
* using local memory as the authoritative source of session or business state.

---

## 7.3 Application Scaling Priorities

When application performance degrades, investigation should proceed in the following order:

1. identify the affected endpoint or workload;
2. measure request latency and error rate;
3. inspect database queries;
4. inspect external-provider calls;
5. inspect payload size;
6. inspect repeated computation;
7. inspect scheduled workloads;
8. optimize inefficient code;
9. introduce caching where safe;
10. isolate heavy asynchronous workloads if required.

---

# 8. Database Scaling

## 8.1 Current Database Architecture

FluxDine currently uses:

* Turso;
* shared database;
* shared schema;
* tenant-scoped application data;
* tenant and restaurant ownership enforcement.

The current database is the primary durable system of record for application business data.

---

## 8.2 Database Scaling Philosophy

Database scaling should initially focus on workload efficiency rather than architectural distribution.

Priority order:

1. correct tenant-scoped queries;
2. correct indexes;
3. efficient query plans;
4. bounded result sets;
5. pagination;
6. avoiding unnecessary repeated queries;
7. reducing excessive round trips;
8. reducing unnecessary writes;
9. schema optimization;
10. provider capacity improvements;
11. only then consider architectural distribution.

---

## 8.3 Query Efficiency

Application queries must:

* use appropriate indexes;
* filter by tenant scope where required;
* avoid unbounded result sets;
* avoid unnecessary joins;
* avoid unnecessary columns;
* paginate large collections;
* avoid repeated identical queries where appropriate;
* avoid full-table operations during normal request processing.

---

## 8.4 Tenant-Scoped Database Access

Tenant-aware database access is a scaling requirement as well as a security requirement.

Queries should use the narrowest appropriate scope:

```text
Tenant
  |
  +-- Restaurant
        |
        +-- Branch
              |
              +-- Business Data
```

The application should not retrieve broad platform-wide datasets when a tenant, restaurant, or branch-scoped query is sufficient.

---

## 8.5 Database Growth Signals

Database scaling decisions should consider:

* total database size;
* tenant count;
* restaurant count;
* branch count;
* user count;
* order volume;
* reservation volume;
* menu/catalog growth;
* query latency;
* write volume;
* read volume;
* migration duration;
* backup duration;
* database provider limits.

---

## 8.6 Future Database Scaling

If Turso becomes a demonstrated constraint, FluxDine may evaluate:

* database optimization;
* provider capacity upgrades;
* workload separation;
* PostgreSQL migration;
* read scaling;
* specialized reporting infrastructure;
* archival strategies.

PostgreSQL is a future migration target, not the current production database.

Database migration must be governed by a dedicated architecture decision and migration plan.

---

# 9. Tenant and Restaurant Scaling

FluxDine is a multi-tenant platform.

Scaling must therefore consider both:

* total platform workload;
* workload distribution across tenants.

A platform with 10,000 restaurants does not necessarily produce the same workload as one with 1,000 restaurants.

Important workload variables include:

* restaurants per tenant;
* branches per restaurant;
* concurrent users;
* active storefront traffic;
* order frequency;
* administrative traffic;
* reservation activity;
* catalog size;
* file usage;
* email volume;
* scheduled workloads.

---

# 10. Noisy-Neighbor Protection

A single high-volume tenant may generate disproportionately high:

* API requests;
* database queries;
* order activity;
* file operations;
* email activity;
* administrative operations.

FluxDine must retain the ability to protect platform stability from disproportionate workload.

Potential controls include:

* tenant-aware rate limits;
* endpoint rate limits;
* API quotas;
* workload prioritization;
* asynchronous processing;
* per-tenant operational controls;
* abuse detection;
* provider-level limits.

These controls should be introduced when measured workload demonstrates the need.

---

# 11. Object Storage Scaling

## 11.1 Current Object Storage

FluxDine uses Cloudflare R2 for object storage.

R2 is intended for durable objects such as:

* restaurant assets;
* uploaded media;
* generated files;
* other application-managed objects.

---

## 11.2 Object Storage Scaling

Object storage should scale independently from application compute.

The application must not treat local application execution storage as the authoritative storage layer for durable objects.

Scaling considerations include:

* total object count;
* total storage volume;
* upload volume;
* download volume;
* object size;
* access frequency;
* retention requirements;
* lifecycle requirements.

---

## 11.3 Object Storage Performance

Large or frequently accessed objects should not unnecessarily pass through application compute when direct or provider-supported object access is appropriate.

Object access must remain authorized and tenant-safe.

---

# 12. Email Scaling

FluxDine uses Resend for email delivery.

Email workload may grow with:

* customer orders;
* owner onboarding;
* authentication;
* notifications;
* transactional messages;
* operational alerts;
* marketing functionality where applicable.

Email scaling must account for:

* provider rate limits;
* delivery failures;
* retry behavior;
* message volume;
* burst behavior;
* template generation cost.

Email delivery should not unnecessarily block critical request processing where asynchronous processing is appropriate.

---

# 13. Scheduled Workloads

## 13.1 Current Model

Scheduled workloads currently use Vercel Cron.

The current production scheduling model is intentionally lightweight.

The current scheduled workload must not be treated as a general-purpose distributed job-processing platform.

---

## 13.2 Scheduled Workload Requirements

Scheduled jobs must:

* be idempotent where practical;
* avoid duplicate destructive operations;
* enforce authorization;
* use appropriate secrets;
* be observable;
* fail safely;
* avoid unbounded work;
* respect tenant isolation.

---

## 13.3 Future Background Processing

If scheduled or asynchronous workload increases beyond what is appropriate for direct Vercel execution, FluxDine may introduce:

* a durable queue;
* dedicated background workers;
* job retry mechanisms;
* dead-letter handling;
* workload prioritization.

These are future capabilities and are not assumed to exist in the initial architecture.

---

# 14. Caching Strategy

## 14.1 Current Position

FluxDine does not require a dedicated distributed cache for initial production.

Caching must not be introduced simply because caching is common in large-scale systems.

---

## 14.2 Safe Caching

Caching may be introduced where:

* data can tolerate bounded staleness;
* tenant boundaries are preserved;
* authorization remains correct;
* invalidation can be managed;
* cache failure does not break correctness.

Examples may include:

* public storefront content;
* static assets;
* infrequently changing configuration;
* derived read-heavy data.

---

## 14.3 Cache Safety

The cache must never become the authoritative source for critical business state unless explicitly approved.

Critical operations such as:

* payments;
* order state;
* reservation state;
* authorization;
* tenant membership;

must not depend on stale cached data for correctness.

---

## 14.4 Future Distributed Cache

A distributed cache may be introduced if measured workload demonstrates:

* excessive repeated database reads;
* read latency pressure;
* expensive repeated computation;
* provider capacity constraints.

The cache technology and architecture require a separate engineering specification before implementation.

---

# 15. Background Workers and Queues

There is currently no dedicated queue or worker cluster in the initial architecture.

This is intentional.

A queue and worker system should be introduced when asynchronous workloads justify the additional operational complexity.

Potential future workloads include:

* email delivery orchestration;
* bulk imports;
* large report generation;
* analytics processing;
* media processing;
* webhook processing;
* large notification fan-out;
* scheduled maintenance;
* data synchronization.

Future workers must preserve:

* tenant context;
* authorization;
* idempotency;
* retry safety;
* observability;
* auditability.

---

# 16. Observability-Driven Scaling

Scaling decisions must use observable signals.

Primary signals include:

### Application

* request latency;
* error rate;
* throughput;
* timeout rate;
* serverless execution behavior.

### Database

* query latency;
* query failures;
* database size;
* read/write workload;
* migration duration;
* provider capacity.

### Object Storage

* storage growth;
* upload volume;
* download volume;
* operation failures.

### Email

* message volume;
* provider errors;
* delivery failures;
* rate-limit responses.

### Scheduled Workloads

* execution failures;
* execution duration;
* backlog;
* duplicate execution;
* missed execution.

### Platform

* tenant growth;
* restaurant growth;
* branch growth;
* active users;
* order volume.

---

# 17. Capacity Planning

Capacity planning must use actual workload measurements.

The following dimensions should be tracked:

| Capacity Dimension | Example Signal              |
| ------------------ | --------------------------- |
| Tenants            | Active tenants              |
| Restaurants        | Active restaurants          |
| Branches           | Active branches             |
| Users              | Active users                |
| API                | Requests per minute         |
| Orders             | Orders per minute/hour/day  |
| Reservations       | Reservations per day        |
| Database           | Storage and query workload  |
| R2                 | Object count and storage    |
| Email              | Messages per day/hour       |
| Scheduled Jobs     | Executions and duration     |
| Errors             | Error rate                  |
| Latency            | p50/p95/p99 where available |

The platform should establish empirical baselines before defining hard capacity thresholds.

---

# 18. Scaling Triggers

Scaling decisions should be triggered by sustained evidence rather than isolated spikes.

Potential triggers include:

* sustained application latency increase;
* sustained error increase;
* database query degradation;
* database capacity pressure;
* increasing migration duration;
* storage growth approaching provider limits;
* increasing email-provider rate limits;
* scheduled jobs approaching execution limits;
* increased timeout frequency;
* a single tenant producing disproportionate workload;
* inability to meet operational objectives;
* provider capacity constraints.

A temporary traffic spike does not automatically justify permanent infrastructure changes.

---

# 19. Scaling Investigation Workflow

When a scaling signal is detected:

```text
Detect Signal
     |
     v
Measure Workload
     |
     v
Identify Bottleneck
     |
     v
Confirm Scope
     |
     +----------------------+
     |                      |
     v                      v
Application             Database
     |                      |
     +----------+-----------+
                |
                v
        Optimize Existing Path
                |
                v
        Re-measure Workload
                |
                v
      Is Constraint Resolved?
          /             \
        Yes              No
         |                |
         v                v
       Stop       Evaluate Architecture
                         |
                         v
                  Approve Scaling Change
                         |
                         v
                    Implement
                         |
                         v
                    Validate
```

---

# 20. Scaling Stages

## Stage 1 — Initial Production

The initial production architecture uses:

* Vercel;
* Next.js;
* Turso;
* shared database/shared schema;
* Cloudflare DNS;
* Cloudflare R2;
* Resend;
* Sentry;
* Vercel Cron.

Priority:

* correctness;
* tenant isolation;
* query efficiency;
* observability;
* reliable deployments;
* backup and recovery;
* controlled workload growth.

No distributed queue, worker cluster, or dedicated cache is required.

---

## Stage 2 — Growth

As workload increases, the first scaling actions should generally include:

* query optimization;
* index optimization;
* API optimization;
* pagination;
* payload reduction;
* improved monitoring;
* Vercel capacity evaluation;
* database capacity evaluation;
* provider limit review;
* workload isolation where required;
* asynchronous processing for suitable workloads.

A dedicated queue, worker, or cache may be introduced if measured workload justifies it.

---

## Stage 3 — Significant Scale

At higher workload levels, FluxDine may evaluate:

* dedicated background processing;
* durable queues;
* distributed caching;
* database architecture evolution;
* PostgreSQL migration;
* read scaling;
* analytics workload separation;
* workload-specific services;
* more advanced rate limiting;
* stronger tenant-level workload controls.

Every major architectural change requires an Architecture Decision Record.

---

## Stage 4 — Large-Scale Platform

If FluxDine reaches a scale where single-provider or single-region assumptions become constraints, future architecture may evaluate:

* multi-region application execution;
* advanced database topology;
* read replicas;
* regional data strategies;
* advanced traffic routing;
* multi-provider resilience;
* dedicated infrastructure platforms.

These capabilities are not part of the initial production architecture.

---

# 21. Vercel Scaling Strategy

Vercel is the current application hosting platform.

The scaling model should use Vercel-managed application execution rather than manually managed application instances.

FluxDine should evaluate:

* deployment execution;
* serverless function behavior;
* request latency;
* function duration;
* concurrent workload;
* provider quotas;
* deployment limits;
* scheduled-job limits.

Vercel plan changes should be made only when actual workload or required capabilities justify them.

A plan upgrade is an infrastructure decision, not a substitute for application optimization.

---

# 22. Database Provider Evolution

Turso is the current production database provider.

PostgreSQL is a future migration target.

A migration from Turso to PostgreSQL must not be initiated merely because PostgreSQL is perceived as more scalable.

The migration should be considered when one or more measurable conditions justify it, such as:

* Turso capacity constraints;
* workload characteristics better suited to PostgreSQL;
* required database features unavailable or impractical on Turso;
* operational requirements;
* performance requirements;
* scaling requirements;
* ecosystem requirements.

Any migration must include:

* architecture review;
* migration strategy;
* compatibility assessment;
* schema verification;
* data migration;
* application compatibility;
* rollback/recovery strategy;
* performance validation;
* tenant-isolation validation.

---

# 23. Scaling and Availability

Scaling must preserve availability.

Scaling changes must not introduce unnecessary single points of failure.

Where infrastructure is changed, FluxDine must evaluate:

* failure behavior;
* dependency failure;
* retry behavior;
* timeout behavior;
* data consistency;
* recovery behavior;
* monitoring coverage.

Scaling architecture must remain compatible with the Disaster Recovery Strategy.

---

# 24. Scaling and Security

Scaling must preserve the Security Architecture.

Additional capacity must not result in:

* weaker authorization;
* bypassed tenant checks;
* shared credentials between unrelated workloads;
* insecure caches;
* unprotected queues;
* uncontrolled background jobs;
* excessive provider permissions.

Every new scaling component becomes part of the FluxDine security boundary and must receive appropriate:

* authentication;
* authorization;
* secret management;
* logging;
* monitoring;
* access control.

---

# 25. Scaling and Data Consistency

Performance mechanisms must not compromise business correctness.

Particular care is required for:

* order state;
* reservation state;
* payment state;
* tenant membership;
* restaurant configuration;
* branch configuration;
* subscription state.

Caching, asynchronous processing, retries, and eventual consistency must be explicitly evaluated before being introduced into these workflows.

---

# 26. Scaling and Deployment

Scaling changes must follow the Deployment Specification and CI/CD Pipeline.

The deployment lifecycle remains:

```text
Architecture Review
        |
        v
Implementation
        |
        v
CI Validation
        |
        v
Deployment
        |
        v
Health Verification
        |
        v
Monitoring Verification
        |
        v
Capacity/Performance Validation
```

Scaling infrastructure must not bypass normal deployment governance.

---

# 27. Scaling and Disaster Recovery

Scaling architecture must remain compatible with the recovery objectives:

* Maximum RPO: 24 hours;
* Maximum RTO: 4 hours;
* Minimum recoverable database backup history: 30 days.

Scaling changes must consider whether they affect:

* backup coverage;
* restore procedures;
* migration compatibility;
* configuration recovery;
* secret recovery;
* object-storage recovery;
* operational documentation.

---

# 28. Scaling Change Governance

The following changes require architecture review:

* introducing a distributed cache;
* introducing a queue;
* introducing dedicated workers;
* changing database provider;
* introducing database replicas;
* introducing data partitioning;
* introducing multi-region infrastructure;
* introducing a new application service;
* changing tenant-isolation boundaries;
* changing the primary storage architecture.

Minor provider capacity upgrades may follow normal infrastructure change procedures where architecture is unchanged.

---

# 29. Scaling Rules

## SCALE-001

Scaling decisions must be based on measured workload or operational requirements.

## SCALE-002

The current architecture must be optimized before introducing distributed infrastructure.

## SCALE-003

Application execution must remain stateless wherever practical.

## SCALE-004

Tenant isolation must be preserved at every scaling stage.

## SCALE-005

Database queries must remain appropriately tenant-scoped.

## SCALE-006

Large datasets must use bounded retrieval and pagination where appropriate.

## SCALE-007

Durable state must not depend on ephemeral application execution environments.

## SCALE-008

Dedicated queues and workers are future capabilities, not initial-production requirements.

## SCALE-009

A dedicated distributed cache is not required until measured workload justifies it.

## SCALE-010

PostgreSQL migration is a future architecture option, not the current production database.

## SCALE-011

Provider plan upgrades must be justified by workload or required capabilities.

## SCALE-012

Scaling must preserve security and authorization boundaries.

## SCALE-013

Scaling changes must remain observable.

## SCALE-014

Scaling changes must remain recoverable under the Disaster Recovery Strategy.

## SCALE-015

Major scaling architecture changes require an Architecture Decision Record.

---

# 30. Architectural Decisions

## AD-SCALE-001 — Managed Application Scaling

Vercel-managed application execution is the initial application scaling mechanism.

Dedicated application servers are not required for initial production.

---

## AD-SCALE-002 — Shared Database / Shared Schema

FluxDine retains the shared-database/shared-schema tenant architecture during initial scaling.

Tenant isolation is enforced through application and database access patterns.

---

## AD-SCALE-003 — Optimization Before Distribution

FluxDine will optimize application and database workload before introducing distributed infrastructure.

---

## AD-SCALE-004 — No Initial Distributed Cache

FluxDine does not require a dedicated distributed cache for initial production.

---

## AD-SCALE-005 — No Initial Queue or Worker Cluster

FluxDine does not require dedicated queue or worker infrastructure for initial production.

---

## AD-SCALE-006 — Turso as Initial Database Provider

Turso remains the initial production database provider.

PostgreSQL remains the future migration target.

---

## AD-SCALE-007 — R2 for Independent Object Scaling

Object storage is separated from application compute through Cloudflare R2.

---

## AD-SCALE-008 — Evidence-Based Provider Upgrades

Infrastructure provider upgrades must be triggered by measured workload or required capabilities.

---

## AD-SCALE-009 — Progressive Scaling

FluxDine will evolve from managed initial production toward more distributed architecture only when workload requires it.

---

# 31. Implementation Boundaries

This document defines scaling architecture and policy.

It does not authorize implementation of every future scaling capability described here.

The following are architectural reservations rather than current implementation requirements:

* distributed cache;
* queue infrastructure;
* dedicated workers;
* database replicas;
* PostgreSQL migration;
* database partitioning;
* multi-region deployment;
* advanced traffic routing;
* workload-specific services.

Each future capability requires:

1. workload justification;
2. architecture review;
3. security review;
4. operational impact assessment;
5. implementation specification;
6. testing;
7. deployment approval;
8. monitoring;
9. recovery validation.

---

# 32. Current Scaling Matrix

| Component      | Current Technology | Current Scaling Model                   | Future Option                               |
| -------------- | ------------------ | --------------------------------------- | ------------------------------------------- |
| Application    | Next.js / Vercel   | Managed serverless scaling              | Dedicated services if justified             |
| DNS            | Cloudflare DNS     | Managed DNS                             | Advanced routing if required                |
| Database       | Turso              | Shared DB / shared schema               | PostgreSQL / advanced DB topology           |
| Object Storage | Cloudflare R2      | Provider-managed scaling                | Additional storage architecture if required |
| Email          | Resend             | Provider-managed scaling                | Async email workers if required             |
| Monitoring     | Sentry             | Managed observability                   | Expanded telemetry                          |
| Scheduled Jobs | Vercel Cron        | Managed scheduled execution             | Queue/worker system                         |
| Cache          | None required      | Application/provider caching where safe | Distributed cache                           |
| Queue          | None               | Direct execution where appropriate      | Durable queue                               |
| Workers        | None               | Application/serverless execution        | Dedicated workers                           |
| Load Balancer  | Provider-managed   | Vercel/Cloudflare platform              | Advanced routing                            |
| Regions        | Provider-managed   | Initial provider architecture           | Multi-region                                |

---

# 33. Capacity Signals Matrix

| Signal                        | What It Indicates                  | Initial Response                        |
| ----------------------------- | ---------------------------------- | --------------------------------------- |
| API latency increases         | Application or dependency pressure | Profile request path                    |
| API errors increase           | Capacity or application failure    | Inspect logs and Sentry                 |
| Database latency increases    | Query or DB pressure               | Optimize queries/indexes                |
| Database grows rapidly        | Data-volume growth                 | Review storage and query patterns       |
| R2 usage grows rapidly        | Object-storage growth              | Review lifecycle and capacity           |
| Email volume increases        | Provider workload                  | Review rate limits and async delivery   |
| Cron duration increases       | Scheduled workload growth          | Optimize or isolate workload            |
| One tenant dominates workload | Noisy neighbor                     | Evaluate tenant controls                |
| Vercel limits approached      | Hosting capacity                   | Optimize or evaluate plan               |
| Turso constraints approached  | Database capacity                  | Optimize or evaluate provider evolution |

---

# 34. Scaling Decision Workflow

Every significant scaling decision should answer:

### 1. What is growing?

Examples:

* tenants;
* restaurants;
* orders;
* requests;
* database size;
* storage;
* email;
* scheduled jobs.

### 2. What is the actual bottleneck?

The team must identify whether the constraint is:

* application;
* database;
* storage;
* provider;
* scheduled workload;
* external dependency;
* tenant-specific workload.

### 3. Can the current architecture be optimized?

Optimization should be attempted before architectural expansion.

### 4. Is the problem temporary or sustained?

Short-lived spikes do not automatically justify permanent infrastructure.

### 5. What is the smallest architectural change that resolves the problem?

Prefer the simplest effective solution.

### 6. Does the change affect tenant isolation?

If yes, architecture and security review are required.

### 7. Does the change affect recovery?

If yes, backup and disaster recovery documentation must be updated.

---

# 35. Future Scaling Capabilities

The following capabilities are intentionally reserved for future scale:

## Application

* workload-specific services;
* advanced serverless optimization;
* dedicated compute.

## Database

* PostgreSQL;
* read scaling;
* partitioning;
* archival;
* workload separation.

## Background Processing

* durable queues;
* dedicated workers;
* retry systems;
* dead-letter queues.

## Caching

* distributed cache;
* application-level cache;
* edge caching.

## Traffic

* advanced routing;
* regional routing;
* multi-region execution.

## Resilience

* multi-region infrastructure;
* multi-provider strategies;
* advanced failover.

None of these capabilities are required for initial production.

---

# 36. Operational Review

Scaling architecture should be reviewed when any of the following occurs:

* major increase in tenant count;
* major increase in order volume;
* major increase in API traffic;
* database performance degradation;
* storage growth beyond expected capacity;
* scheduled workload growth;
* provider limit warnings;
* introduction of a queue;
* introduction of a cache;
* database provider migration;
* significant change to hosting architecture;
* material change to recovery requirements.

---

# 37. References

This specification depends on and must remain consistent with:

* FluxDine Core Architecture;
* Security Architecture;
* Deployment Specification;
* Environment & Secrets Strategy;
* Environment Variables;
* CI/CD Pipeline;
* Monitoring;
* Logging;
* Backup Strategy;
* Disaster Recovery;
* Database Architecture;
* API Architecture;
* Backend Architecture;
* Frontend Architecture.

Where a conflict exists, the FluxDine Architecture Bible and higher-authority architectural decisions take precedence.

---

# 38. Appendices

## Appendix A — Initial Production Scaling Model

```text
                    Internet
                       |
                       v
                Cloudflare DNS
                       |
                       v
                    Vercel
                       |
              Next.js Application
                       |
          +------------+------------+
          |                         |
          v                         v
       Turso                       R2
     Database                Object Storage
          |
          +----------------------+
          |
          v
    Shared Platform Services
          |
     +----+----+
     |         |
     v         v
  Resend    Sentry
```

Scheduled workloads may execute through Vercel Cron.

No dedicated queue, worker cluster, or distributed cache is required initially.

---

## Appendix B — Scaling Priority Order

```text
1. Observe
2. Measure
3. Diagnose
4. Optimize
5. Re-measure
6. Increase managed capacity
7. Isolate heavy workloads
8. Introduce asynchronous processing
9. Introduce caching
10. Evolve database architecture
11. Consider multi-region architecture
```

---

## Appendix C — Tenant Scaling Model

```text
FluxDine
   |
   +-- Tenant A
   |     |
   |     +-- Restaurant
   |           |
   |           +-- Branches
   |
   +-- Tenant B
   |     |
   |     +-- Restaurant
   |           |
   |           +-- Branches
   |
   +-- Tenant C
         |
         +-- Restaurant
               |
               +-- Branches
```

Scaling mechanisms must preserve these isolation boundaries.

---

## Appendix D — Current vs Future Infrastructure

| Capability                | Initial Production |             Future |
| ------------------------- | -----------------: | -----------------: |
| Vercel Application        |                Yes |           Continue |
| Turso                     |                Yes | Possible migration |
| Shared DB / Shared Schema |                Yes |         May evolve |
| Cloudflare DNS            |                Yes |           Continue |
| Cloudflare R2             |                Yes |           Continue |
| Resend                    |                Yes |           Continue |
| Sentry                    |                Yes |           Continue |
| Vercel Cron               |                Yes |         May evolve |
| Distributed Cache         |                 No |           Possible |
| Queue                     |                 No |           Possible |
| Dedicated Workers         |                 No |           Possible |
| Read Replicas             |                 No |           Possible |
| PostgreSQL                |                 No |      Future target |
| Multi-Region              |                 No |             Future |
| Multi-Provider            |                 No |             Future |

---

## Appendix E — Scaling Review Checklist

Before approving a significant scaling change:

* [ ] Workload has been measured.
* [ ] Bottleneck has been identified.
* [ ] Existing architecture has been optimized.
* [ ] Tenant isolation has been reviewed.
* [ ] Security impact has been reviewed.
* [ ] Data consistency impact has been reviewed.
* [ ] Monitoring has been planned.
* [ ] Backup/recovery impact has been reviewed.
* [ ] Deployment impact has been reviewed.
* [ ] Failure modes have been considered.
* [ ] Provider limits have been considered.
* [ ] Rollback/recovery approach is defined.
* [ ] Architecture Decision Record has been created where required.

---

## Appendix F — Scaling Governance

Scaling is not a one-time architecture activity.

The platform must continuously evaluate whether its infrastructure remains appropriate for actual workload.

The governing principle is:

> Scale the simplest architecture that reliably satisfies the current workload.

When the current architecture no longer satisfies measured requirements, FluxDine should evolve deliberately rather than prematurely.

---

# Revision History

| Version | Status              | Summary                                                                                                                                                                                                                                                                                            |
| ------- | ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.0     | Approved and Locked | Original generic scaling architecture specification                                                                                                                                                                                                                                                |
| 1.1     | Pending Approval    | Reworked scaling strategy to reflect actual FluxDine infrastructure, managed Vercel execution, Turso shared database/shared schema, R2, Resend, Sentry, current scheduled workload model, evidence-driven scaling, tenant-aware scaling, and explicitly reserved future distributed infrastructure |
| 1.2     | 2026-09-12          | Clarified Vercel Cron is for application jobs; database backups are ADR-055 GitHub Actions, not Hobby Cron. |

---