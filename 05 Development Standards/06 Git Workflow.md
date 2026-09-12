# 05 Development Standards

# 06 — Git Workflow

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-DS-006 |
| **Document Name** | Git Workflow |
| **Version** | 1.1 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering Team |
| **Classification** | Development Standard |
| **Depends On** | Folder Structure<br>Coding Standards |
| **Referenced By** | All Development Teams |

---

# Purpose

This document defines the official Git workflow for the FluxDine platform.

The objectives are to ensure:

- Consistent collaboration
- Traceable changes
- Safe releases
- Stable production deployments
- High code quality
- Efficient code reviews

Every contributor shall follow this workflow.

---

# Git Strategy

FluxDine application development uses short-lived feature branches merged to the application mainline after review and CI.

This standard covers **two repositories** with **different mainline names**. They must not be collapsed into one branch name.

### Application repository

- Mainline: `master`
- Workflow: feature branch → pull request → review → CI → merge to `master` → Vercel deployment
- Production deployment authorization is governed by the Deployment Specification
- Current Initial Production Vercel project: `fluxdine-staging` (role is Initial Production)

### Architecture Bible repository

- Mainline: `main`

`develop` is **not** a required active branch for the application repository.

This is not GitFlow. Long-lived `develop` / `release/*` integration branches are not required.

---

# Branch Structure

Standard application branches include:

```text
master

feature/*

bugfix/*

hotfix/*
```

Long-lived feature branches are discouraged.

Architecture Bible changes use `main` in that repository.

---

# Branch Purposes

### master (application repository)

- Application mainline
- Protected branch
- Source of Vercel Initial Production deployments
- No direct commits

Architecture Bible production documentation uses `main` in the Architecture Bible repository.

---

### feature/*

Examples:

```text
feature/order-management

feature/payment-service

feature/customer-profile
```

Used for new functionality.

---

### bugfix/*

Examples:

```text
bugfix/cart-calculation

bugfix/login-validation
```

Used for non-production bug fixes.

---

### hotfix/*

Examples:

```text
hotfix/payment-timeout

hotfix/security-patch
```

Used only for urgent production issues.

---

# Branch Naming

Branch names shall use:

```text
kebab-case
```

Examples:

```text
feature/customer-orders

feature/menu-management

bugfix/payment-gateway

hotfix/authentication
```

Branch names shall clearly describe the work.

---

# Commit Standards

Commits shall be:

- Small
- Atomic
- Meaningful
- Reviewable

Each commit shall represent one logical change.

---

# Commit Message Format

FluxDine adopts the Conventional Commits specification.

Examples:

```text
feat: add order tracking

fix: resolve payment timeout

refactor: simplify authentication flow

docs: update API documentation

test: add checkout integration tests

chore: update dependencies
```

---

# Supported Commit Types

| Type | Purpose |
|------|----------|
| feat | New feature |
| fix | Bug fix |
| refactor | Code restructuring |
| docs | Documentation |
| test | Testing |
| chore | Maintenance |
| perf | Performance improvement |
| ci | CI/CD changes |
| build | Build configuration |

---

# Pull Requests

Every change shall be submitted through a Pull Request.

Pull Requests shall include:

- Purpose
- Scope
- Testing Evidence
- Related Issue
- Breaking Changes (if any)

Direct merges are prohibited.

---

# Code Review

Every Pull Request requires:

- Automated validation
- Code review
- Successful tests
- Approval from authorized reviewers

No Pull Request shall bypass review.

---

# Merge Strategy

Approved merge methods:

- Squash and Merge (preferred)
- Rebase and Merge (approved when preserving history is beneficial)

Merge commits are discouraged unless justified.

---

# Protected Branches

The following application branch shall be protected:

```text
master
```

The Architecture Bible mainline `main` shall be protected in that repository.

Protected branches require:

- Pull Requests
- Successful CI
- Required approvals
- Passing tests

Direct pushes are prohibited.

---

# Conflict Resolution

Merge conflicts shall:

- Be resolved by the feature owner.
- Preserve business logic.
- Preserve architectural consistency.
- Be reviewed before merging.

Conflicts shall never be resolved by deleting functionality without review.

---

# Release Workflow

The standard **application** workflow is:

```text
Feature Branch
    ↓
Pull Request
    ↓
Code Review
    ↓
CI
    ↓
Merge into master
    ↓
Vercel deployment
    ↓
Initial Production (Vercel project fluxdine-staging)
```

Production deployment authorization remains governed by the Deployment Specification.

Preview deployments must not receive production secrets, including database backup secrets (ADR-055).

---

# Hotfix Workflow

Production issues follow:

```text
master

↓

Hotfix Branch

↓

Testing

↓

Code Review

↓

Merge into master
```

Hotfixes shall be based on `master` and merged back to `master`. There is no required `develop` synchronization.

---

# Tags

Production releases shall use Semantic Versioning tags.

Examples:

```text
v1.0.0

v1.2.5

v2.0.0
```

Every production deployment shall be tagged.

---

# Continuous Integration

Every Pull Request shall trigger:

- Build Validation
- Linting
- Static Analysis
- Unit Tests
- Integration Tests
- Security Scanning

Failed validation blocks merging.

---

# Continuous Deployment

Production deployment shall occur only after:

- Approved Pull Request
- Successful CI Pipeline
- Release Approval
- Version Tag Creation

Deployment shall remain automated where practical.

---

# Branch Cleanup

Merged feature branches shall be deleted.

Obsolete branches shall not remain in the repository.

Repository maintenance shall be performed regularly.

---

# Engineering Rules

- Short-lived feature branches are the standard application workflow.
- Application mainline is `master`. Architecture Bible mainline is `main`.
- `develop` is not a required application branch.
- Direct commits to protected branches are prohibited.
- Conventional Commits are mandatory.
- Every Pull Request requires code review.
- Automated testing shall pass before merging.
- Production releases should be tagged where release tagging is used.
- Hotfixes merge to `master`.
- Repository history shall remain clean and understandable.
- Ordinary CI must not receive production backup secrets (ADR-055).
- This document is the authoritative Git Workflow specification.

---

# Architecture Decision Records

- FluxDine application development uses short-lived feature branches into `master`.
- Conventional Commits standardize commit history.
- Pull Requests are mandatory for all code changes.
- Protected branches safeguard production stability.
- Automated CI validation precedes merge.
- Semantic Versioning may govern tagged production releases.
- Squash merging provides a clean Git history.
- Repository hygiene is maintained through regular branch cleanup.
- AI-generated code follows the same workflow as human-written code.
- This document is the authoritative Git Workflow specification.

---

# Quality Attributes

| Attribute | Objective |
|-----------|-----------|
| Traceability | Complete change history |
| Stability | Safe production releases |
| Collaboration | Predictable team workflow |
| Maintainability | Clean Git history |
| Reliability | Reviewed and tested changes |
| Security | Protected production branches |
| Scalability | Support growing development teams |
| Automation | CI/CD integration |

---

# References

- Folder Structure
- Coding Standards
- Testing Strategy
- Code Review Guidelines
- Versioning Strategy
- Release Process

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.1 | 2026-09-12 | FluxDine Engineering Team | Application mainline `master`; Architecture Bible `main`; removed required `develop`/GitFlow; Vercel Initial Production. |
| 1.0 | Initial Release | FluxDine Engineering Team | Approved as the authoritative Git Workflow specification |