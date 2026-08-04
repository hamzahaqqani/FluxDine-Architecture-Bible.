# 05 Development Standards

# 11 — Claude AI Rules

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-DS-011 |
| **Document Name** | Claude AI Rules |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering Team |
| **Classification** | AI Development Standard |
| **Depends On** | All Architecture Documents |
| **Referenced By** | Claude AI |

---

# Purpose

This document defines the mandatory rules that Claude AI shall follow when analyzing, generating, refactoring, reviewing, documenting, or improving the FluxDine platform.

The objectives are to ensure:

- Architectural Consistency
- High-Quality Code
- Accurate Documentation
- Safe Refactoring
- Predictable AI Output
- Long-Term Platform Stability

Claude AI shall always prioritize the FluxDine Architecture Bible over assumptions or implementation shortcuts.

---

# Primary Directive

The FluxDine Architecture Bible is the single source of truth.

Claude AI shall never recommend, generate, or approve implementations that conflict with the Architecture Bible.

If ambiguity exists, Claude AI shall preserve the existing architecture and request clarification rather than invent new patterns.

---

# Architecture Compliance

Claude AI shall:

- Follow Clean Architecture.
- Follow Feature-First Architecture.
- Follow Database-per-Service architecture.
- Follow Shared Platform Services architecture.
- Follow Event-Driven Architecture.
- Respect service boundaries.
- Respect dependency direction.

Architectural consistency takes precedence over implementation convenience.

---

# Service Ownership

Claude AI shall preserve business ownership.

Examples:

- Identity → Identity Service
- Tenant Management → Tenant Service
- Restaurant Registry → Restaurant Service
- Commerce → Commerce Service
- Billing → Billing Service
- Payment Processing → Payment Service
- Notification Delivery → Notification Service
- Email Delivery → Email Service
- Analytics → Analytics Service
- Domain Management → Domain Service
- Theme Management → Theme Service

Business capabilities shall never migrate between services without an approved architectural decision.

---

# Refactoring Rules

Claude AI may:

- Improve readability.
- Reduce duplication.
- Improve maintainability.
- Simplify implementation.
- Improve performance.

Claude AI shall never:

- Change business behavior.
- Break API compatibility.
- Remove required validation.
- Change ownership boundaries.
- Introduce architectural inconsistencies.

Behavior-preserving refactoring is preferred.

---

# Code Review Rules

Claude AI shall verify:

- Architecture compliance
- Business logic correctness
- Security
- Performance
- Readability
- Maintainability
- Test coverage
- Documentation

Code reviews shall identify problems and provide actionable recommendations.

---

# Documentation Rules

Claude AI shall keep documentation synchronized with implementation.

Documentation updates are required whenever changes affect:

- APIs
- Database Schema
- Architecture
- Configuration
- Business Workflows
- Shared Services
- User Experience

Documentation shall remain concise, accurate, and consistent.

---

# Database Rules

Claude AI shall:

- Respect Database-per-Service architecture.
- Preserve repository boundaries.
- Use migrations for schema evolution.
- Maintain tenant isolation.
- Avoid duplicate persistence models.

Direct database access between services is prohibited.

---

# API Rules

Claude AI shall:

- Follow REST standards.
- Preserve API versioning.
- Maintain backward compatibility.
- Validate input.
- Use standardized error responses.
- Apply correct HTTP status codes.

Breaking API changes require architectural approval.

---

# Security Rules

Claude AI shall always:

- Validate authentication.
- Validate authorization.
- Preserve tenant isolation.
- Protect sensitive information.
- Prevent injection attacks.
- Follow least-privilege principles.
- Avoid exposing implementation details.

Security recommendations shall follow the Security Architecture.

---

# Testing Rules

Claude AI shall recommend or generate:

- Unit Tests
- Integration Tests
- API Tests
- End-to-End Tests
- Regression Tests

Code shall not be considered complete without appropriate testing.

---

# Performance Rules

Claude AI shall:

- Eliminate unnecessary queries.
- Avoid N+1 database problems.
- Recommend efficient algorithms.
- Reduce unnecessary rendering.
- Preserve scalability.

Performance improvements shall not compromise readability or correctness.

---

# Shared Code Rules

Claude AI shall:

- Encourage reusable packages.
- Prevent duplicated utilities.
- Prevent duplicated DTOs.
- Prevent duplicated validation logic.
- Preserve shared component consistency.

Shared functionality belongs in shared packages.

---

# AI Collaboration Rules

Claude AI shall:

- Preserve developer intent.
- Explain significant architectural recommendations.
- Distinguish facts from assumptions.
- Clearly identify breaking changes.
- Recommend incremental improvements whenever practical.

Claude AI shall avoid speculative architectural redesign.

---

# Forbidden Actions

Claude AI shall never:

- Invent architectural patterns.
- Ignore the Architecture Bible.
- Duplicate business logic.
- Bypass repositories.
- Modify another service's database.
- Hardcode secrets.
- Remove security controls.
- Break tenant isolation.
- Introduce circular dependencies.
- Recommend undocumented APIs.

---

# Conflict Resolution

If conflicting implementation approaches exist, Claude AI shall:

1. Follow the Architecture Bible.
2. Preserve existing architectural patterns.
3. Maintain backward compatibility.
4. Recommend the least disruptive solution.
5. Escalate unresolved architectural conflicts for human review.

---

# Engineering Rules

- The Architecture Bible is the highest authority.
- Claude AI shall preserve service ownership.
- AI-generated recommendations shall follow all Development Standards.
- Architectural consistency is mandatory.
- Security recommendations shall never weaken platform protections.
- Documentation shall remain synchronized.
- Tests shall accompany significant implementation changes.
- Existing platform patterns shall be preferred over new patterns.
- AI recommendations shall remain reviewable by human engineers.
- This document is the authoritative Claude AI Rules specification.

---

# Architecture Decision Records

- Claude AI is treated as an engineering advisor and contributor.
- Architecture takes precedence over implementation convenience.
- Existing platform patterns shall be reused whenever possible.
- AI-generated documentation shall follow platform standards.
- AI-assisted refactoring shall preserve business behavior.
- AI recommendations are subject to the same engineering governance as human contributions.
- Documentation and testing remain mandatory deliverables.
- Security recommendations shall align with platform architecture.
- Human engineering review remains the final authority for production changes.
- This document is the authoritative Claude AI Rules specification.

---

# Quality Attributes

| Attribute | Objective |
|-----------|-----------|
| Consistency | Architecture-compliant recommendations |
| Reliability | Predictable AI-assisted engineering |
| Maintainability | Safe refactoring and documentation |
| Security | Secure-by-default recommendations |
| Scalability | Preserve platform evolution |
| Testability | Encourage comprehensive testing |
| Governance | Controlled AI collaboration |
| Traceability | Reviewable AI-generated output |

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
- Cursor AI Rules

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | Initial Release | FluxDine Engineering Team | Approved as the authoritative Claude AI Rules specification |