# 05 Development Standards

# 10 — Cursor AI Rules

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-DS-010 |
| **Document Name** | Cursor AI Rules |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering Team |
| **Classification** | AI Development Standard |
| **Depends On** | All Architecture Documents |
| **Referenced By** | Cursor AI |

---

# Purpose

This document defines the mandatory rules that Cursor AI shall follow when generating, modifying, reviewing, or refactoring code for the FluxDine platform.

The objectives are to ensure:

- Architectural Consistency
- High Code Quality
- Security
- Maintainability
- Predictable AI Output
- Long-Term Platform Stability

Cursor AI shall always prioritize the FluxDine Architecture Bible over convenience or assumptions.

---

# Primary Directive

The FluxDine Architecture Bible is the single source of truth.

Cursor AI shall never generate code that conflicts with the Architecture Bible.

If implementation requirements conflict with architectural standards, the Architecture Bible takes precedence.

---

# Architecture Compliance

Cursor AI shall:

- Follow Clean Architecture.
- Follow Feature-First Architecture.
- Follow Database-per-Service architecture.
- Follow Shared Platform Services architecture.
- Follow Event-Driven Architecture.
- Respect module boundaries.
- Respect service ownership.

Architecture shall never be violated to simplify implementation.

---

# Service Ownership

Cursor AI shall never assign business responsibilities to the wrong service.

Examples:

- Authentication → Identity Service
- Tenant Management → Tenant Service
- Restaurant Registry → Restaurant Service
- Commerce → Commerce Service
- Billing → Billing Service
- Payment Processing → Payment Service
- Notifications → Notification Service
- Email Delivery → Email Service
- Analytics → Analytics Service
- Domain Management → Domain Service
- Theme Management → Theme Service

Business ownership is immutable.

---

# Database Rules

Cursor AI shall never:

- Access another service's database.
- Share database tables across services.
- Duplicate business data.
- Bypass repositories.

Every service owns its own database.

Repositories are the only approved persistence layer.

---

# Business Logic Rules

Business logic belongs exclusively inside services.

Cursor AI shall never place business logic inside:

- Controllers
- Routes
- UI Components
- Pages
- Repositories
- Middleware

Controllers coordinate.

Services decide.

Repositories persist.

---

# API Rules

Cursor AI shall:

- Follow REST principles.
- Use versioned APIs.
- Return standardized responses.
- Use correct HTTP status codes.
- Validate input.
- Authenticate requests.
- Authorize access.

Breaking API changes require a new version.

---

# Security Rules

Cursor AI shall always:

- Validate authentication.
- Validate authorization.
- Enforce tenant isolation.
- Sanitize input.
- Prevent SQL Injection.
- Prevent Cross-Site Scripting.
- Protect sensitive information.

Security shall never be disabled for convenience.

---

# Multi-Tenant Rules

Cursor AI shall ensure:

- Every tenant accesses only its own data.
- Cross-tenant access is prohibited unless explicitly authorized.
- Tenant ownership is validated for every business operation.

Tenant isolation is mandatory.

---

# Event Rules

Cursor AI shall:

- Publish events only from the owning service.
- Consume events without modifying ownership.
- Avoid synchronous coupling where events are appropriate.

Domain events shall remain consistent with the Event Catalog.

---

# Repository Rules

Repositories shall only:

- Query data.
- Persist data.
- Execute transactions.
- Map entities.

Repositories shall never implement business rules.

---

# UI Rules

Cursor AI shall:

- Use the FluxDine Design System.
- Reuse existing components.
- Follow UI Standards.
- Maintain responsive layouts.
- Support accessibility.

Hardcoded styling is discouraged unless approved.

---

# Shared Code Rules

Cursor AI shall:

- Reuse existing packages.
- Avoid duplicate utilities.
- Avoid duplicate DTOs.
- Avoid duplicate validation logic.

Common functionality belongs in shared packages.

---

# Naming Rules

Cursor AI shall follow:

- PascalCase
- camelCase
- snake_case
- kebab-case

according to the Coding Standards document.

Meaningful naming is mandatory.

---

# Testing Rules

Every generated feature shall include appropriate tests.

Cursor AI shall generate:

- Unit Tests
- Integration Tests
- API Tests
- End-to-End Tests (when applicable)

Generated code without testing is incomplete.

---

# Documentation Rules

Cursor AI shall update documentation when changes affect:

- APIs
- Architecture
- Configuration
- Workflows
- Public Behavior

Documentation shall remain synchronized with implementation.

---

# Refactoring Rules

Cursor AI may:

- Improve readability.
- Reduce duplication.
- Simplify implementation.
- Improve performance.

Cursor AI shall never change observable business behavior unless explicitly instructed.

---

# Performance Rules

Cursor AI shall:

- Avoid unnecessary queries.
- Avoid N+1 problems.
- Prefer efficient algorithms.
- Minimize unnecessary rendering.
- Optimize only when justified.

Premature optimization is discouraged.

---

# Git Rules

Cursor AI shall:

- Generate Conventional Commit messages.
- Respect Git Workflow.
- Avoid modifying unrelated files.
- Keep changes focused.

Generated Pull Requests should remain reviewable.

---

# Forbidden Actions

Cursor AI shall never:

- Invent architecture.
- Ignore the Architecture Bible.
- Duplicate business logic.
- Break tenant isolation.
- Bypass repositories.
- Hardcode secrets.
- Disable security.
- Modify production migrations.
- Introduce circular dependencies.
- Generate undocumented APIs.

---

# Conflict Resolution

If architectural ambiguity exists, Cursor AI shall:

1. Follow the Architecture Bible.
2. Prefer existing implementations.
3. Maintain backward compatibility.
4. Avoid assumptions.
5. Flag unresolved conflicts for developer review.

---

# Engineering Rules

- The Architecture Bible is the highest authority.
- Business ownership shall never change without architectural approval.
- Generated code shall follow all Development Standards.
- AI-generated code shall pass automated testing.
- Every change shall remain reviewable.
- Every generated API shall follow API Standards.
- Every generated schema shall follow Database Standards.
- Every generated UI shall follow UI Standards.
- Every generated feature shall preserve tenant isolation.
- This document is the authoritative Cursor AI Rules specification.

---

# Architecture Decision Records

- Cursor AI is treated as an engineering contributor.
- AI-generated code follows the same standards as human-written code.
- Architecture takes precedence over implementation convenience.
- Existing platform patterns shall be reused whenever possible.
- Shared services shall remain the only owners of shared business capabilities.
- AI shall not introduce new architectural patterns without approval.
- Documentation and tests are required deliverables.
- Security requirements are mandatory.
- AI output is subject to human code review.
- This document is the authoritative Cursor AI Rules specification.

---

# Quality Attributes

| Attribute | Objective |
|-----------|-----------|
| Consistency | Architecture-compliant AI output |
| Reliability | Predictable code generation |
| Maintainability | Reusable and clean implementations |
| Security | Secure-by-default code generation |
| Scalability | Support platform evolution |
| Testability | Automatically testable output |
| Governance | Controlled AI-assisted development |
| Traceability | Reviewable AI contributions |

---

# References

- Platform Architecture
- Shared Services Overview
- Folder Structure
- Coding Standards
- API Standards
- Database Standards
- UI Standards
- Git Workflow
- Testing Strategy
- Code Review Guidelines

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | Initial Release | FluxDine Engineering Team | Approved as the authoritative Cursor AI Rules specification |