# 05 Development Standards

# 04 — Database Standards

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-DS-004 |
| **Document Name** | Database Standards |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Engineering Team |
| **Classification** | Development Standard |
| **Depends On** | Coding Standards |
| **Referenced By** | All Platform Services |

---

# Purpose

This document defines the mandatory database design, implementation, migration, and maintenance standards for the FluxDine platform.

The objectives are to ensure:

- Data Consistency
- Scalability
- Maintainability
- Performance
- Security
- Reliability
- Multi-Tenant Support

Every database shall comply with these standards.

---

# Database Philosophy

FluxDine follows a **Database-per-Service** architecture.

Each platform service owns its own database.

Examples:

- Identity Database
- Tenant Database
- Restaurant Database
- Commerce Database
- Billing Database
- Payment Database

Direct database access between services is prohibited.

---

# Supported Database

The primary production database is:

- PostgreSQL

Development tools include:

- Drizzle ORM
- Drizzle Kit
- PostgreSQL Extensions (where approved)

Alternative databases require architectural approval.

---

# Schema Ownership

Every service owns:

- Tables
- Views
- Indexes
- Constraints
- Migrations
- Stored Metadata

No service may modify another service's schema.

---

# Primary Keys

All business entities shall use:

- UUID Version 7 (preferred)
- UUID Version 4 (acceptable where v7 is unavailable)

Example:

```text
id UUID PRIMARY KEY
```

Sequential integer primary keys shall not be used for business entities.

---

# Foreign Keys

Foreign keys shall enforce referential integrity.

Relationships shall use:

```text
restaurant_id

tenant_id

customer_id

order_id
```

Foreign keys shall be indexed where appropriate.

---

# Naming Conventions

Database objects shall use **snake_case**.

Examples:

```text
restaurants

menu_items

customer_orders

payment_transactions
```

Columns:

```text
created_at

updated_at

deleted_at

tenant_id
```

---

# Audit Columns

Every persistent business table shall include:

```text
id

created_at

updated_at
```

Soft-deletable tables shall additionally include:

```text
deleted_at
```

Optional ownership fields include:

```text
created_by

updated_by
```

---

# Multi-Tenant Standards

Tenant-aware tables shall include:

```text
tenant_id
```

Every query shall enforce tenant isolation.

Cross-tenant data access is prohibited unless explicitly authorized for platform administration.

---

# Normalization

Database schemas shall generally follow Third Normal Form (3NF).

Denormalization is permitted only when:

- Performance requirements justify it.
- Data consistency is preserved.
- Architectural approval is obtained.

---

# Constraints

Appropriate constraints shall be implemented.

Examples:

- Primary Keys
- Foreign Keys
- Unique Constraints
- Check Constraints
- Not Null Constraints

Application validation shall complement—not replace—database constraints.

---

# Indexing

Indexes shall be created for:

- Primary Keys
- Foreign Keys
- Frequently Queried Columns
- Search Columns
- Unique Fields

Indexes shall be reviewed regularly to eliminate redundancy.

---

# Transactions

Transactions shall be used for:

- Multi-table updates
- Financial operations
- Inventory updates
- Subscription changes
- Critical business workflows

Transactions shall remain as short as possible.

---

# Soft Delete Policy

Business entities requiring historical retention shall use soft deletion.

Example:

```text
deleted_at TIMESTAMP NULL
```

Physical deletion is permitted only for:

- Temporary data
- Cache data
- Expired sessions
- Archived records approved for removal

---

# Migrations

Database schema changes shall be managed exclusively through version-controlled migrations.

Migration rules:

- One logical change per migration.
- Migrations shall be reversible where practical.
- Manual schema modifications in production are prohibited.
- Migration files shall never be edited after deployment.

---

# ORM Standards

FluxDine standardizes on:

- Drizzle ORM

ORM models shall remain synchronized with database migrations.

Raw SQL is permitted only when necessary for performance or functionality.

---

# Performance

Developers shall:

- Avoid N+1 queries.
- Use indexes effectively.
- Limit query result sizes.
- Prefer pagination.
- Optimize joins.
- Measure query performance.

Performance tuning shall be evidence-based.

---

# Security

Databases shall enforce:

- Tenant Isolation
- Parameterized Queries
- Principle of Least Privilege
- Encryption at Rest
- Encryption in Transit
- Secure Credential Management

SQL Injection vulnerabilities are prohibited.

---

# Backup and Recovery

Production databases shall support:

- Automated Backups
- Point-in-Time Recovery
- Disaster Recovery
- Backup Verification
- Recovery Testing

Recovery procedures shall be documented and periodically validated.

---

# Engineering Rules

- PostgreSQL is the standard production database.
- Database-per-Service architecture is mandatory.
- UUIDs shall be used for business entity identifiers.
- Tables shall use snake_case naming.
- Tenant isolation is mandatory.
- Foreign key constraints shall be enforced.
- Database changes require version-controlled migrations.
- Manual production schema changes are prohibited.
- Sensitive data shall be protected through encryption and access controls.
- Every schema change shall undergo code review.
- This document is the authoritative Database Standards specification.

---

# Architecture Decision Records

- PostgreSQL is the standard relational database platform.
- Database-per-Service architecture enforces service ownership.
- Drizzle ORM is the standard persistence framework.
- UUIDs improve scalability and distributed system compatibility.
- Schema evolution occurs exclusively through migrations.
- Soft deletion preserves historical business records.
- Database constraints complement application validation.
- Tenant isolation is enforced at both the application and database layers.
- Future database technologies may complement PostgreSQL where justified but shall not violate architectural principles.
- This document is the authoritative Database Standards specification.

---

# Quality Attributes

| Attribute | Objective |
|-----------|-----------|
| Consistency | Reliable data integrity |
| Scalability | Support platform growth |
| Performance | Efficient query execution |
| Security | Protected data access |
| Reliability | Durable and recoverable storage |
| Maintainability | Predictable schema evolution |
| Auditability | Historical data preservation |
| Extensibility | Support future data models |

---

# References

- Folder Structure
- Coding Standards
- API Standards
- Shared Services Overview
- Repository Specification
- Testing Strategy

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | Initial Release | FluxDine Engineering Team | Approved as the authoritative Database Standards specification |