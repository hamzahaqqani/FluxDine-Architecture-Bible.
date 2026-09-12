# FluxDine Architecture Bible

> **The authoritative architectural source of truth for the FluxDine platform.**

---

# 1. Purpose

The FluxDine Architecture Bible defines the business, product, technical, engineering, and implementation architecture of the FluxDine platform.

It exists to ensure that:

- Architecture remains consistent.
- Engineering decisions remain traceable.
- Development follows defined standards.
- Services maintain clear ownership.
- AI coding assistants operate within approved boundaries.
- Future teams can understand the platform without relying on tribal knowledge.

This repository is not merely documentation.

It is the architectural foundation from which FluxDine is designed, implemented, tested, deployed, and evolved.

---

# 2. What Is FluxDine?

FluxDine is a restaurant-focused SaaS platform designed to help restaurants establish and operate their own digital business infrastructure.

The platform is designed around the principle:

> **Own the relationship. Own the experience. Own the platform.**

FluxDine provides the technology required to support restaurant digital operations while preserving restaurant ownership of its brand, customer relationship, data, ordering experience, and digital presence.

Current Initial Production (concise):

- Hosting: Vercel (`fluxdine-staging` occupies the Initial Production role)
- Database: Turso, Shared Database / Shared Schema
- Object storage: Cloudflare R2
- Email: Resend
- Observability: Sentry
- DNS: Cloudflare DNS

PostgreSQL, Database-per-Service, Kubernetes, and dedicated queues/workers are **future** unless a later accepted ADR says otherwise.

---

# 3. Architecture Bible Structure

The Architecture Bible is organized into nine major architectural sections:

```text
00 Governance
        ↓
01 Core Architecture
        ↓
02 Engineering Specifications
        ↓
03 Product Modules
        ↓
04 Shared Platform Services
        ↓
05 Development Standards
        ↓
06 Architecture Decision Records
        ↓
07 Engineering Artifacts
        ↓
08 Implementation Roadmap
```

Each section answers a different architectural question.

---

# 4. 00 Governance

```text
00 Governance/
```

The Governance section establishes the principles and language that govern the entire architecture.

```text
00 Governance/

├── 00 Founder's Manifesto.md
├── Company Philosophy.md
├── Product Philosophy.md
├── Engineering Philosophy.md
├── Long-Term Vision.md
├── Architecture Principles.md
├── System Glossary.md
├── Documentation Standards.md
└── Naming Standards.md
```

## Purpose

This section defines:

- Why FluxDine exists.
- What FluxDine believes.
- How the company approaches products.
- How engineering should operate.
- The long-term direction.
- Architectural principles.
- Standard terminology.
- Documentation rules.
- Naming conventions.

Governance documents sit at the top of the architectural hierarchy.

---

# 5. 01 Core Architecture

```text
01 Core Architecture/
```

This section defines the core architecture of FluxDine.

It establishes the foundational architectural structure from which the platform is designed.

It defines the major architectural concepts, boundaries, relationships, and principles required to understand the FluxDine platform as a whole.

The remaining architectural sections must remain consistent with the approved Core Architecture.

---

# 6. 02 Engineering Specifications

```text
02 Engineering Specifications/
```

This section defines the detailed engineering specifications required to implement the FluxDine architecture.

It establishes the technical contracts, engineering-level specifications, and implementation constraints that support the approved architecture.

Engineering specifications must remain consistent with:

- Governance
- Core Architecture
- Product Modules
- Shared Platform Services
- Architecture Decision Records

---

# 7. 03 Product Modules

```text
03 Product Modules/
```

This section defines the major product modules of the FluxDine platform.

It describes the product capabilities, module boundaries, responsibilities, and relationships that make up the FluxDine product ecosystem.

Product modules must remain consistent with:

- Governance
- Core Architecture
- Engineering Specifications
- Shared Platform Services
- Architecture Decision Records

---

# 8. 04 Shared Platform Services

```text
04 Shared Platform Services/
```

This section defines the Shared Platform Services and their ownership boundaries.

```text
Identity Service
Tenant Service
Restaurant Service
Commerce Service
Billing Service
Payment Service
Notification Service
Email Service
Analytics Service
Domain Service
Theme Service
Feature Flag Service
Audit Service
Logging Service
Monitoring Service
File Storage Service
Search Service
```

---

# 9. Service Ownership

Every major platform capability has an authoritative owner.

```text
Identity
    → Identity Service

Tenant
    → Tenant Service

Restaurant
    → Restaurant Service

Commerce
    → Commerce Service

Billing
    → Billing Service

Payment
    → Payment Service

Notification
    → Notification Service

Email
    → Email Service

Analytics
    → Analytics Service

Domain
    → Domain Service

Theme
    → Theme Service

Feature Flags
    → Feature Flag Service

Audit
    → Audit Service

Logging
    → Logging Service

Monitoring
    → Monitoring Service

File Storage
    → File Storage Service

Search
    → Search Service
```

Services shall not duplicate another service's authoritative business logic.

---

# 10. Database Ownership

Current Initial Production persistence:

```text
Turso
Shared Database
Shared Schema
```

Logical Shared Platform Services own their domain data (rules, access, lifecycle). They do **not** each have a separate physical database.

Cross-service communication still prefers:

```text
API
Event
Approved Abstraction
```

Shared schema is not unrestricted cross-module or cross-tenant access. Tenant isolation remains mandatory.

Database-per-Service (ADR-003) is historical. PostgreSQL is a **future** migration target.

---

# 11. Multi-Tenancy

FluxDine is a multi-tenant SaaS platform.

Tenant isolation is a foundational architectural requirement.

Tenant boundaries shall be enforced across:

- Authentication
- Authorization
- APIs
- Services
- Data access
- Background processing
- Analytics
- Storage
- Search

No tenant shall be able to access another tenant's protected resources.

---

# 12. Payment Architecture

Payment processing is centralized.

The approved architecture is:

```text
Commerce / Billing
        ↓
Payment Service
        ↓
Payment Gateway Abstraction
        ↓
Payment Provider
```

Commerce and Billing shall request payment operations through Payment Service.

They shall not communicate directly with external payment providers.

This architecture allows payment providers to be changed or added without rewriting payment business logic.

---

# 13. Event-Driven Integration

FluxDine supports asynchronous domain events where appropriate.

The general model is:

```text
Service
   ↓
Event
   ↓
Event Bus
   ↓
Consumer Services
```

Events should be used when they provide meaningful decoupling.

Consumers should be designed to handle:

- Retries
- Duplicate delivery
- Failure
- Eventual consistency

---

# 14. 05 Development Standards

```text
05 Development Standards/
```

This section defines how FluxDine software is built.

It includes:

```text
Folder Structure
Coding Standards
API Standards
Database Standards
UI Standards
Git Workflow
Versioning Strategy
Testing Strategy
Code Review Guidelines
Cursor AI Rules
Claude AI Rules
AI Prompt Standards
Sprint Planning
Release Process
```

All development work must follow these standards unless an approved architectural decision explicitly states otherwise.

---

# 15. AI-Assisted Development

FluxDine permits AI-assisted engineering.

Approved AI assistants may assist with:

- Code generation
- Refactoring
- Testing
- Documentation
- Debugging
- Code review
- Architecture analysis

However:

> **AI is an engineering assistant, not the architectural authority.**

AI-generated implementation must follow:

- Architecture Bible
- Development Standards
- Security requirements
- Testing requirements
- Code review requirements

---

# 16. 06 Architecture Decision Records

```text
06 Architecture Decision Records/
```

This section contains the formal architectural decision history.

It includes:

```text
ADR-000 Template.md
ADR-001 ... through ADR-054 ...
ADR Register.md
```

Architecture Decision Records explain:

- The problem
- The decision
- Alternatives
- Rationale
- Consequences
- Architectural impact

---

# 17. ADR Governance

Significant architectural changes should be documented through an ADR.

An existing decision should not be silently changed.

When an accepted architectural decision changes:

```text
Existing ADR
      ↓
New Architectural Decision
      ↓
New ADR
      ↓
Previous ADR marked Superseded
```

This preserves architectural history.

---

# 18. 07 Engineering Artifacts

```text
07 Engineering Artifacts/
```

This section translates written architecture into visual engineering artifacts.

---

## ERDs

```text
Platform ERD
Commerce ERD
Restaurant ERD
Complete ERD
```

The ERDs represent logical ownership and relationships on the current Turso Shared Database / Shared Schema.

They do not require Database-per-Service as the current physical architecture.

---

## Sequence Diagrams

```text
Authentication
Tenant Provisioning
Order Placement
Reservation Flow
Payment Flow
Subscription Flow
Notification Flow
Background Jobs
Audit Flow
```

These describe interactions between:

- Users
- Applications
- Services
- Databases
- External providers
- Event infrastructure

---

## Class Diagrams

Class diagrams represent important domain and service-level structures.

---

## State Diagrams

State diagrams define lifecycle transitions for important business entities.

Examples include:

```text
Order
Reservation
Subscription
Domain
```

Invalid transitions should be rejected by implementation.

---

## Activity Diagrams

Activity diagrams describe important business processes such as:

```text
Restaurant Onboarding
Order Processing
Payment Processing
```

---

## Wireframes

Wireframes establish structural UX concepts for important applications and workflows.

They are not final visual designs.

---

# 19. 08 Implementation Roadmap

```text
08 Implementation Roadmap/
```

This section converts the approved architecture into an implementation sequence.

The implementation phases are:

```text
Phase 01 — Foundation
Phase 02 — SaaS Core
Phase 03 — HQ Platform
Phase 04 — Restaurant Platform
Phase 05 — Self-Service
Phase 06 — Shared Services
Phase 07 — Infrastructure
Phase 08 — Production
```

---

# 20. Implementation Philosophy

FluxDine shall be built incrementally.

The implementation should follow:

```text
Foundation
    ↓
SaaS Core
    ↓
Platform Operations
    ↓
Restaurant Operations
    ↓
Self-Service
    ↓
Shared Capabilities
    ↓
Infrastructure
    ↓
Production
```

A phase is complete only when its implementation, testing, review, documentation, and acceptance criteria are satisfied.

---

# 21. Repository as Source of Truth

This repository is the architectural source of truth.

When implementation and documentation disagree, the discrepancy must be resolved explicitly.

Do not silently assume that:

```text
Code = Architecture
```

Instead:

```text
Architecture
      ↓
Decision
      ↓
Documentation
      ↓
Implementation
```

If implementation reveals that the architecture needs to change, the change must be formally evaluated.

---

# 22. Architecture Change Process

Significant architectural changes follow:

```text
Problem Identified
       ↓
Impact Analysis
       ↓
Alternatives Considered
       ↓
Architecture Decision
       ↓
ADR Created / Updated
       ↓
Architecture Documents Updated
       ↓
Engineering Artifacts Updated
       ↓
Implementation Updated
       ↓
Tests Updated
```

---

# 23. Documentation Hierarchy

When interpreting FluxDine architecture, use the following hierarchy:

```text
Governance
     ↓
Core Architecture
     ↓
Engineering Specifications
     ↓
Product Modules
     ↓
Shared Platform Services
     ↓
Development Standards
     ↓
Architecture Decision Records
     ↓
Engineering Artifacts
     ↓
Implementation Roadmap
```

Documents lower in the hierarchy must not silently contradict higher-level architectural principles.

---

# 24. Developer Workflow

A developer beginning a new feature should follow:

```text
1. Understand the business requirement.

2. Read the relevant Core Architecture.

3. Read the relevant Engineering Specifications.

4. Read the relevant Product Module.

5. Identify the owning service.

6. Review relevant ADRs.

7. Review relevant Engineering Artifacts.

8. Review Development Standards.

9. Implement.

10. Test.

11. Review.

12. Update documentation where necessary.
```

---

# 25. AI Developer Workflow

AI coding assistants should follow:

```text
1. Read relevant Architecture documents.

2. Identify service ownership.

3. Identify constraints.

4. Identify existing patterns.

5. Inspect existing implementation.

6. Propose the implementation.

7. Implement only within approved boundaries.

8. Generate/update tests.

9. Validate architecture compliance.

10. Report changes clearly.
```

AI should not invent missing architecture when authoritative documentation already exists.

---

# 26. New Feature Workflow

Every significant feature should follow:

```text
Business Requirement
        ↓
Product Module / Workflow
        ↓
Service Ownership
        ↓
API / Data Design
        ↓
ADR if required
        ↓
Implementation
        ↓
Testing
        ↓
Engineering Artifact Update
        ↓
Release
```

---

# 27. Definition of Architectural Completeness

A major architectural capability should have, where applicable:

- Business definition
- Platform definition
- Product/module definition
- Service ownership
- API contract
- Database model
- ADR
- Engineering artifact
- Testing strategy
- Implementation roadmap

Not every small change requires every artifact.

The level of documentation should be proportional to architectural significance.

---

# 28. Security

Security is a platform-wide requirement.

Security responsibilities include:

- Authentication
- Authorization
- Tenant isolation
- Input validation
- Secret management
- Secure communication
- Payment protection
- Auditability
- Monitoring

Security requirements apply to:

- Applications
- APIs
- Services
- Databases
- Infrastructure
- Background jobs
- AI-assisted development

---

# 29. Observability

FluxDine separates:

```text
Logging
Monitoring
Audit
Analytics
```

These are related but distinct capabilities.

Operational systems should be observable through appropriate:

- Logs
- Metrics
- Health checks
- Alerts
- Audit events

---

# 30. Quality Standards

FluxDine prioritizes:

```text
Correctness
    ↓
Security
    ↓
Reliability
    ↓
Maintainability
    ↓
Scalability
    ↓
Performance
```

Performance optimization must not unnecessarily compromise correctness, security, or maintainability.

---

# 31. Testing Philosophy

Testing is part of implementation, not a final activity.

Critical workflows should include appropriate:

- Unit tests
- Integration tests
- API tests
- End-to-end tests
- Regression tests
- Security tests
- Performance tests

---

# 32. Release Philosophy

Production releases shall be:

- Tested
- Reviewed
- Versioned
- Traceable
- Observable
- Recoverable

Every production release must follow the Release Process.

---

# 33. Version Control

Git is the authoritative version-control system for the Architecture Bible and associated implementation.

Documentation changes should use the established Git workflow.

Recommended commit types include:

```text
feat
fix
docs
refactor
test
chore
```

---

# 34. Architectural Principles Summary

FluxDine architecture is guided by the following core principles:

```text
Architecture Before Implementation

Clear Service Ownership (logical)

Shared Database / Shared Schema (current physical persistence)

Tenant Isolation

Centralized Identity

Centralized Payment

Payment Gateway Abstraction

Explicit API Contracts

Event-Driven Integration

Idempotent Critical Operations

Security by Design

Observability

Testability

Infrastructure Independence

Backward Compatibility

Documentation as Architecture

AI Governance

Evolution Through ADRs
```

---

# 35. Glossary

The authoritative terminology for FluxDine is defined in:

```text
00 Governance/System Glossary.md
```

When terminology conflicts with informal usage, the approved glossary should be preferred.

---

# 36. Naming

Naming conventions are defined in:

```text
00 Governance/Naming Standards.md
```

Names should communicate:

- Ownership
- Purpose
- Domain
- Intent

---

# 37. Documentation

Documentation requirements are defined in:

```text
00 Governance/Documentation Standards.md
```

Architecture documentation should remain:

- Accurate
- Clear
- Version-controlled
- Maintainable
- Consistent

---

# 38. Repository Map

The complete repository structure is:

```text
FLUXDINE-ARCHITECTURE/
│
├── README.md
│
├── 00 Governance/
│   ├── 00 Founder's Manifesto.md
│   ├── Company Philosophy.md
│   ├── Product Philosophy.md
│   ├── Engineering Philosophy.md
│   ├── Long-Term Vision.md
│   ├── Architecture Principles.md
│   ├── System Glossary.md
│   ├── Documentation Standards.md
│   └── Naming Standards.md
│
├── 01 Core Architecture/
│
├── 02 Engineering Specifications/
│
├── 03 Product Modules/
│
├── 04 Shared Platform Services/
│
├── 05 Development Standards/
│
├── 06 Architecture Decision Records/
│
├── 07 Engineering Artifacts/
│
└── 08 Implementation Roadmap/
```

The README provides the map.

The individual sections provide the detailed architecture.

---

# 39. How to Use This Repository

## Founder

Use the Architecture Bible to ensure the platform remains aligned with the company's long-term vision.

---

## Product Team

Use it to understand:

- Business capabilities
- Product modules
- Service boundaries
- Customer journeys

---

## Engineers

Use it to understand:

- Architecture
- Service ownership
- APIs
- Data
- Engineering standards
- Testing
- Releases

---

## Architects

Use it to:

- Evaluate architectural changes.
- Create ADRs.
- Maintain service boundaries.
- Review system evolution.

---

## AI Coding Assistants

Use it as persistent architectural context.

AI assistants should read the relevant documents before making architectural or implementation changes.

The Architecture Bible should be treated as authoritative context.

---

# 40. What This Repository Is Not

This repository is not:

- A replacement for source code.
- A task tracker.
- A random collection of technical notes.
- A collection of AI-generated guesses.
- A place for undocumented architectural changes.

It is the architectural reference system for FluxDine.

---

# 41. Governance Rule

When making a significant architectural change:

> **Do not simply change the code. Change the architecture record.**

The decision should be visible, explainable, and traceable.

---

# 42. Final Architecture Principle

FluxDine should be built so that:

```text
The business can understand it.
The product team can evolve it.
The engineers can maintain it.
The infrastructure can operate it.
The customers can trust it.
The architecture can survive its growth.
```

---

# 43. Final Statement

The FluxDine Architecture Bible exists to protect the long-term integrity of the platform.

Technology will change.

Products will evolve.

Teams will grow.

Markets will change.

But the architecture should provide a stable foundation from which FluxDine can continue to evolve deliberately.

> **Build deliberately. Own the architecture. Protect the customer. Engineer for the future.**

---

# Document Status

**Architecture Bible Status:** Complete

**Current Version:** 1.2

**Repository:** `FLUXDINE-ARCHITECTURE`

**Sections:** `00–08`

**Governance:** Established

**Core Architecture:** Established

**Engineering Specifications:** Established

**Product Modules:** Established

**Shared Platform Services:** Established

**Development Standards:** Established

**Architecture Decisions:** Established

**Engineering Artifacts:** Established

**Implementation Roadmap:** Established