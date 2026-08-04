# 05 Development Standards

# 01 — Folder Structure

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-DS-001 |
| **Document Name** | Folder Structure |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering Team |
| **Classification** | Development Standard |
| **Depends On** | Platform Architecture |
| **Referenced By** | All Development Teams |

---

# Purpose

This document defines the official folder structure for every FluxDine application, service, shared library, and infrastructure component.

A standardized folder structure ensures:

- Predictable code organization
- Easier onboarding
- Faster navigation
- Consistent architecture
- AI-assisted development
- Long-term maintainability

Every FluxDine repository shall follow these standards.

---

# Design Principles

The project structure shall follow these principles:

- Feature-first organization
- Separation of concerns
- Clean Architecture
- Modular development
- Domain ownership
- Shared code reuse
- Scalability
- Maintainability

---

# Repository Structure

The recommended FluxDine monorepo structure is:

```text
FluxDine/

├── apps/

├── packages/

├── services/

├── infrastructure/

├── docs/

├── scripts/

├── tools/

├── tests/

├── .github/

├── docker/

├── .env.example

├── package.json

├── turbo.json

├── pnpm-workspace.yaml

└── README.md
```

---

# Apps

The **apps** directory contains deployable applications.

Example:

```text
apps/

├── hq-platform/

├── restaurant-platform/

├── customer-platform/

├── rider-platform/

├── self-service-platform/

├── landing-website/
```

Applications contain presentation logic only.

Business logic belongs to services and shared packages.

---

# Packages

Reusable code belongs inside **packages**.

Example:

```text
packages/

├── ui/

├── types/

├── config/

├── utils/

├── validation/

├── constants/

├── sdk/

├── design-system/
```

Packages shall remain framework-independent whenever practical.

---

# Services

Shared platform services are isolated.

Example:

```text
services/

├── identity-service/

├── tenant-service/

├── restaurant-service/

├── commerce-service/

├── billing-service/

├── payment-service/

├── notification-service/

├── analytics-service/

├── audit-service/
```

Each service owns:

- APIs
- Business Logic
- Database
- Events

---

# Infrastructure

Infrastructure resources belong here.

```text
infrastructure/

├── docker/

├── kubernetes/

├── terraform/

├── nginx/

├── monitoring/

├── logging/
```

Infrastructure configuration shall never be mixed with application code.

---

# Documentation

Architecture and technical documentation belong inside:

```text
docs/

├── architecture/

├── api/

├── deployment/

├── development/

├── operations/
```

---

# Scripts

Automation scripts belong inside:

```text
scripts/

├── build/

├── deploy/

├── migrate/

├── seed/

├── backup/
```

---

# Tests

Testing assets remain centralized.

```text
tests/

├── unit/

├── integration/

├── e2e/

├── performance/

├── fixtures/
```

---

# Application Structure

Each application follows:

```text
src/

├── app/

├── modules/

├── components/

├── pages/

├── layouts/

├── hooks/

├── services/

├── api/

├── store/

├── routes/

├── assets/

├── styles/

├── utils/

├── types/

├── constants/

└── main.tsx
```

---

# Feature Module Structure

Every feature module follows:

```text
feature/

├── components/

├── pages/

├── hooks/

├── services/

├── repository/

├── api/

├── dto/

├── validators/

├── types/

├── utils/

└── tests/
```

Each feature owns its own implementation.

---

# Backend Service Structure

Each backend service follows:

```text
src/

├── controllers/

├── services/

├── repositories/

├── dto/

├── entities/

├── validators/

├── middleware/

├── routes/

├── events/

├── jobs/

├── config/

├── database/

├── utils/

└── index.ts
```

---

# Shared Libraries

Shared libraries shall never contain business-specific logic.

Shared libraries include:

- UI Components
- Utilities
- Validation
- Types
- Constants
- SDKs

---

# Naming Conventions

Folders use:

```text
kebab-case
```

Examples:

```text
restaurant-platform

payment-service

customer-profile

menu-management
```

Files use:

```text
PascalCase

camelCase

kebab-case
```

depending on purpose.

---

# Import Direction

Dependencies shall flow inward.

Allowed:

```text
Presentation

↓

Application

↓

Domain

↓

Infrastructure
```

Reverse dependencies are prohibited.

---

# Module Isolation

Modules shall never:

- Access another module's database.
- Import private implementation files.
- Bypass published APIs.
- Duplicate business logic.

Modules communicate through approved interfaces.

---

# Engineering Rules

- Every application shall follow the approved folder structure.
- Every feature shall remain self-contained.
- Shared code belongs in packages.
- Business logic belongs in services.
- Infrastructure belongs outside applications.
- Documentation shall remain version-controlled.
- Test code shall remain isolated.
- Circular dependencies are prohibited.
- Folder names shall remain consistent.
- This document is the authoritative Folder Structure standard.

---

# Architecture Decision Records

- FluxDine adopts a feature-first architecture.
- Shared code is centralized into reusable packages.
- Platform services remain independently deployable.
- Documentation remains inside the repository.
- Infrastructure remains separate from application code.
- Clean Architecture governs dependency direction.
- Monorepo organization is the recommended repository model.
- Future applications shall follow this structure.
- Folder structure shall remain stable across the platform.
- This document is the authoritative Folder Structure specification.

---

# Quality Attributes

| Attribute | Objective |
|-----------|-----------|
| Consistency | Uniform project organization |
| Scalability | Support platform growth |
| Maintainability | Easy navigation and refactoring |
| Modularity | Strong feature isolation |
| Reusability | Shared packages |
| Testability | Clear testing boundaries |
| Developer Experience | Predictable structure |
| AI Compatibility | Consistent code generation |

---

# References

- Platform Architecture
- Shared Services Overview
- Coding Standards
- API Standards
- Database Standards
- Git Workflow

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | Initial Release | FluxDine Engineering Team | Approved as the authoritative Folder Structure standard |