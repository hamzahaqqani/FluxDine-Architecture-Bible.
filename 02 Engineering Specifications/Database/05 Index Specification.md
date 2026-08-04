# 04 Engineering Specifications

# Database

# 05 — Index Specification

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-DB-005 |
| **Document Name** | Index Specification |
| **Version** | 1.0 |
| **Status** | Draft |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | 00 Database Naming Standards<br>01 Complete Database Schema Specification<br>02 Table Specifications<br>03 Relationships<br>04 Constraints |
| **Referenced By** | 07 Database Migration Strategy<br>08 Drizzle ORM Mapping |

---

# Dependencies

This specification depends upon the following Database Engineering documents:

- 00 Database Naming Standards
- 01 Complete Database Schema Specification
- 02 Table Specifications
- 03 Relationships
- 04 Constraints

These documents collectively define the logical structure, ownership model, relationships, and integrity rules upon which the indexing strategy is built.

---

# Referenced By

This specification is consumed by:

- 07 Database Migration Strategy
- 08 Drizzle ORM Mapping

Implementation artifacts shall implement the indexing strategy defined within this document.

---

# Document Status

| Item | Value |
|------|-------|
| Status | Approved |
| Approval | Approved & Locked |
| Implementation | Not Started |
| Last Updated | TBD |

---

# Purpose

The purpose of this document is to define the complete logical indexing strategy for the FluxDine platform.

Indexes are essential for maintaining predictable database performance, efficient query execution, scalable tenant isolation, and high-throughput transactional processing.

This specification defines which data requires indexing, why those indexes exist, and the engineering principles governing their use.

The document intentionally remains implementation-independent and specifies logical indexing requirements rather than database-specific syntax.

---

# Scope

This specification defines:

- Primary Key indexes
- Foreign Key indexes
- Unique indexes
- Composite indexes
- Covering indexes
- Partial indexes
- Search indexes
- Reporting indexes
- Tenant indexes
- Cross-domain indexes
- Background processing indexes
- Performance optimization guidelines

---

# Out of Scope

This document does not define:

- SQL CREATE INDEX statements
- PostgreSQL implementation
- Drizzle ORM definitions
- Query execution plans
- Database tuning parameters
- Storage engine optimization
- Hardware sizing

These implementation details are documented separately.

---

# Index Philosophy

Indexes exist to optimize data retrieval while balancing write performance and storage efficiency.

FluxDine follows an **engineering-first indexing philosophy**, where indexes are introduced only when they provide measurable business or operational value.

Every index shall support one or more of the following objectives:

- Faster lookups
- Efficient joins
- Tenant isolation
- Transaction processing
- Reporting
- Analytics
- Background processing
- Security
- Operational monitoring

Indexes shall never be created without a documented engineering purpose.

---

# Index Design Principles

## Principle 1 — Purpose-Driven

Every index shall exist to support a documented query pattern.

---

## Principle 2 — Minimal Redundancy

Duplicate and overlapping indexes are prohibited.

---

## Principle 3 — Tenant Optimization

Tenant-owned tables shall prioritize indexes supporting tenant-scoped queries.

---

## Principle 4 — Referential Performance

Every frequently joined foreign key shall be indexed.

---

## Principle 5 — Composite Optimization

Composite indexes shall follow the natural filtering order of application queries.

---

## Principle 6 — Read/Write Balance

Indexes shall improve read performance without introducing unnecessary write overhead.

---

## Principle 7 — Technology Independence

Index definitions shall remain independent of any specific database engine.

---

# Index Categories

FluxDine recognizes the following logical index categories.

## Primary Key Indexes

Provide efficient lookup by entity identifier.

---

## Foreign Key Indexes

Optimize joins between related entities.

---

## Unique Indexes

Enforce business uniqueness while supporting efficient lookups.

---

## Composite Indexes

Optimize multi-column filtering and sorting.

---

## Covering Indexes

Support high-frequency queries without requiring additional table lookups.

---

## Partial Indexes

Optimize subsets of frequently queried data.

---

## Search Indexes

Support customer-facing search functionality.

---

## Analytical Indexes

Optimize reporting, dashboards, and aggregation workloads.

---

# Index Naming Convention

## Purpose

A standardized naming convention ensures that indexes remain easily identifiable, self-descriptive, and consistent across the entire FluxDine platform.

Every index shall follow the naming standards defined in this section regardless of the underlying database technology.

---

## Naming Principles

Index names shall:

- Clearly describe their purpose.
- Be deterministic.
- Be unique within the database.
- Use lowercase snake_case.
- Avoid abbreviations unless they are industry-standard.
- Reflect the indexed table and columns.

---

## Standard Naming Patterns

| Index Type | Naming Pattern | Example |
|------------|----------------|---------|
| Primary Key | pk_<table> | pk_orders |
| Foreign Key | fk_<table>_<column> | fk_orders_customer_id |
| Unique | uk_<table>_<column> | uk_users_email |
| Composite | idx_<table>_<column1>_<column2> | idx_orders_tenant_status |
| Search | ft_<table>_<column> | ft_menu_items_name |
| Partial | idx_partial_<table>_<purpose> | idx_partial_orders_active |
| Analytical | idx_analytics_<table> | idx_analytics_usage_metrics |

---

## Naming Rules

### Rule 1

Every index name shall be globally unique.

---

### Rule 2

Index names shall identify the owning table.

---

### Rule 3

Composite indexes shall list indexed columns in their defined order.

---

### Rule 4

Prefixes shall indicate the logical purpose of the index.

---

### Rule 5

Generated database names shall never replace documented engineering names.

---

## Engineering Notes

The naming convention defined in this section is the authoritative standard for all logical index definitions within the FluxDine platform.

---

# How to Read This Document

This specification is organized by business domain.

Each chapter documents the logical indexing strategy for a specific domain using the standardized engineering template below.

The document intentionally describes **what should be indexed** and **why**, leaving implementation details to subsequent engineering specifications.

---

# Standard Index Specification Template

Every logical index defined within this specification shall use the following standardized engineering template.

---

## Index Name

Official engineering identifier following the Index Naming Convention.

---

## Purpose

Business justification for the index.

---

## Index Type

One of:

- Primary
- Foreign Key
- Unique
- Composite
- Covering
- Partial
- Search
- Analytical

---

## Business Domain

Owning business domain.

---

## Table

Logical table containing the indexed columns.

---

## Indexed Columns

Columns participating in the index.

---

## Included Columns (Optional)

Additional non-key columns included to support covering indexes.

---

## Uniqueness

Specifies whether the index enforces uniqueness.

Possible values:

- Unique
- Non-Unique

---

## Partial Predicate

Defines the logical condition when the index represents a partial index.

If not applicable:

None

---

## Sort Order

Ascending, descending, or mixed.

---

## Query Patterns Supported

Typical application queries optimized by this index.

---

## Ownership Scope

Defines whether the index is:

- Platform
- Tenant
- Restaurant
- Branch
- Customer

---

## Performance Objective

Primary optimization objective.

Examples include:

- Authentication
- Lookup
- Reporting
- Sorting
- Filtering
- Aggregation
- Background Processing

---

## Maintenance Considerations

Engineering considerations affecting future maintenance.

Examples:

- High write frequency
- Rare updates
- Analytical workload
- Frequently rebuilt

---

## Engineering Notes

Additional implementation guidance and architectural rationale.

---

## Related ADRs

Architecture Decision Records governing the index.
---

# Table of Contents

## Chapter 1 — Platform Indexes

## Chapter 2 — Identity Indexes

## Chapter 3 — Tenant Indexes

## Chapter 4 — Restaurant Indexes

## Chapter 5 — Commerce Indexes

## Chapter 6 — Customer Indexes

## Chapter 7 — Reservation Indexes

## Chapter 8 — Billing Indexes

## Chapter 9 — Payment Indexes

## Chapter 10 — Notification Indexes

## Chapter 11 — Analytics Indexes

## Chapter 12 — Branding Indexes

## Chapter 13 — Feature Management Indexes

## Chapter 14 — Shared Platform Indexes

## Chapter 15 — Cross-Domain Index Strategy

## Chapter 16 — Query Optimization Guidelines

## Chapter 17 — Index Maintenance Strategy

## Chapter 18 — Engineering Rules

## Chapter 19 — Architecture Decision Records

## Chapter 20 — Performance Guidelines

---

## Appendix A — Complete Index Matrix

## Appendix B — Composite Index Matrix

## Appendix C — Query → Index Mapping

## Appendix D — Reserved Future Indexes

---

## References

(To be completed in the final response.)

---

## Revision History

(To be completed in the final response.)

---

# Chapter 1 — Platform Indexes

## 1.1 Purpose

The Platform Domain contains globally shared reference data accessed by every tenant. Indexes within this domain prioritize efficient lookups, configuration loading, authorization, and platform initialization.

---

## Platform Index Strategy

Primary indexing objectives include:

- Global reference lookups
- Feature discovery
- Authorization loading
- Subscription resolution
- Template retrieval

---

### Platform Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| countries | country_id | iso_code |
| currencies | currency_id | currency_code |
| languages | language_id | language_code |
| timezones | timezone_id | timezone_name |
| subscription_plans | subscription_plan_id | plan_key, status |
| features | feature_id | feature_key, category |
| global_roles | role_id | role_name |
| global_permissions | permission_id | permission_key |
| notification_templates | notification_template_id | template_key, template_type |

---

# Chapter 2 — Identity Indexes

## 2.1 Purpose

Identity indexes optimize authentication, authorization, session management, and user discovery.

---

## Identity Index Strategy

Primary objectives include:

- Login performance
- Token validation
- Session lookup
- Role resolution
- Permission loading

---

### Identity Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| users | user_id | email, username, status |
| sessions | session_id | user_id, expires_at |
| user_memberships | membership_id | user_id + tenant_id |
| authentication_providers | provider_id | provider_name + provider_account_id |
| mfa_configurations | mfa_id | user_id + mfa_method |
| password_reset_tokens | token_id | token, expires_at |
| email_verification_tokens | token_id | token, expires_at |

---

# Chapter 3 — Tenant Indexes

## 3.1 Purpose

Tenant indexes optimize ownership lookups, tenant isolation, feature resolution, API authentication, and SaaS administration.

---

## Tenant Index Strategy

Primary objectives include:

- Tenant resolution
- Ownership filtering
- Domain routing
- Subscription lookup
- API authentication

---

### Tenant Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| tenants | tenant_id | tenant_slug, status |
| tenant_settings | tenant_id | — |
| tenant_domains | domain_id | tenant_id, domain_name |
| tenant_branding | branding_id | tenant_id |
| tenant_features | assignment_id | tenant_id + feature_id |
| tenant_payment_gateways | gateway_id | tenant_id, provider_id |
| tenant_integrations | integration_id | tenant_id, integration_type |
| tenant_subscriptions | subscription_id | tenant_id, subscription_status |
| tenant_usage_metrics | metric_id | tenant_id, metric_date |
| tenant_api_keys | api_key_id | api_key, tenant_id |
| tenant_invitations | invitation_id | tenant_id, email, status |

---

# Chapter 4 — Restaurant Indexes

## 4.1 Purpose

Restaurant indexes optimize restaurant operations, branch management, menu loading, and operational reporting.

---

## Restaurant Index Strategy

Primary objectives include:

- Restaurant lookup
- Branch filtering
- Operational management
- Staff lookup
- Rider assignment

---

### Restaurant Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| restaurants | restaurant_id | tenant_id, restaurant_slug |
| restaurant_settings | restaurant_id | — |
| branches | branch_id | restaurant_id, branch_code |
| business_hours | hours_id | branch_id |
| delivery_zones | zone_id | branch_id |
| seating_areas | seating_area_id | branch_id |
| restaurant_tables | table_id | seating_area_id, table_number |
| kitchen_stations | station_id | branch_id |
| staff | staff_id | branch_id, role |
| riders | rider_id | branch_id, availability_status |
| rider_assignments | assignment_id | rider_id, branch_id |

---

# Chapter 5 — Commerce Indexes

## 5.1 Purpose

Commerce indexes optimize customer ordering, menu browsing, checkout, and order processing.

---

## Commerce Index Strategy

Primary objectives include:

- Fast menu loading
- Product filtering
- Cart retrieval
- Order lookup
- Checkout performance

---

### Commerce Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| menus | menu_id | restaurant_id |
| menu_categories | category_id | menu_id, display_order |
| menu_items | menu_item_id | category_id, sku, availability_status |
| item_variants | variant_id | menu_item_id |
| item_options | option_id | menu_item_id |
| item_addons | addon_id | menu_item_id |
| carts | cart_id | customer_id, restaurant_id, cart_status |
| cart_items | cart_item_id | cart_id, menu_item_id |
| orders | order_id | tenant_id, restaurant_id, branch_id, customer_id, order_status, created_at |
| order_items | order_item_id | order_id, menu_item_id |

---

# Chapter 6 — Customer Indexes

## 6.1 Purpose

The Customer Domain supports customer identity, profile management, loyalty, personalization, communication preferences, and historical purchasing behaviour.

Indexes within this domain are designed to optimize customer lookup, authentication, personalization, loyalty processing, and customer-facing experiences while maintaining strict tenant isolation.

---

## Customer Index Strategy

Primary indexing objectives include:

- Customer identification
- Customer profile lookup
- Address retrieval
- Loyalty processing
- Favorite item lookup
- Device registration
- Communication preference retrieval

Indexes within this domain prioritize customer-centric read operations while supporting high-volume transactional workloads.

---

### Customer Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| customers | customer_id | tenant_id, email, phone_number, created_at |
| customer_addresses | address_id | customer_id, is_default |
| customer_preferences | preference_id | customer_id |
| customer_favorites | favorite_id | customer_id, menu_item_id |
| loyalty_accounts | loyalty_account_id | customer_id, loyalty_number |
| loyalty_transactions | transaction_id | loyalty_account_id, created_at |
| reward_points | reward_point_id | customer_id, expiration_date |
| customer_devices | device_id | customer_id, device_token |
| communication_preferences | preference_id | customer_id |

---

## Customer Query Optimization

Customer indexes are optimized for:

- Customer login
- Customer profile loading
- Order history lookup
- Favorite menu retrieval
- Loyalty balance lookup
- Address selection
- Push notification delivery

---

# Chapter 7 — Reservation Indexes

## 7.1 Purpose

Reservation indexes optimize reservation scheduling, seating management, reservation history, and operational planning.

The indexing strategy emphasizes rapid availability checks, reservation lookup, and reservation lifecycle processing.

---

## Reservation Index Strategy

Primary objectives include:

- Reservation lookup
- Branch scheduling
- Table availability
- Reservation history
- Waitlist processing

---

### Reservation Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| reservations | reservation_id | tenant_id, branch_id, customer_id, reservation_date, reservation_status |
| reservation_tables | reservation_table_id | reservation_id, table_id |
| reservation_events | event_id | reservation_id, event_timestamp |
| waitlists | waitlist_id | branch_id, created_at, waitlist_status |

---

## Reservation Query Optimization

Indexes optimize:

- Upcoming reservations
- Today's reservations
- Customer reservation history
- Table utilization
- Waitlist processing
- Reservation reporting

---

# Chapter 8 — Billing Indexes

## 8.1 Purpose

Billing indexes optimize subscription management, invoice generation, financial reporting, and recurring billing operations.

The strategy emphasizes efficient financial lookups while preserving immutable accounting records.

---

## Billing Index Strategy

Primary objectives include:

- Subscription lookup
- Invoice retrieval
- Payment reconciliation
- Financial reporting
- Billing history

---

### Billing Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| billing_accounts | billing_account_id | tenant_id |
| subscriptions | subscription_id | billing_account_id, subscription_status, renewal_date |
| invoices | invoice_id | subscription_id, invoice_number, invoice_status, issue_date |
| invoice_items | invoice_item_id | invoice_id |
| credit_notes | credit_note_id | invoice_id, created_at |
| refund_requests | refund_request_id | invoice_id, refund_status, created_at |

---

## Billing Query Optimization

Indexes optimize:

- Active subscriptions
- Subscription renewals
- Invoice history
- Outstanding invoices
- Refund processing
- Financial reporting

---

# Chapter 9 — Payment Indexes

## 9.1 Purpose

The Payment Domain supports provider-independent payment processing through the shared Payment Service and Payment Gateway Abstraction Layer.

Indexes optimize payment authorization, transaction processing, reconciliation, settlement, and webhook processing.

---

## Payment Index Strategy

Primary objectives include:

- Payment authorization
- Transaction lookup
- Merchant lookup
- Gateway routing
- Settlement processing
- Webhook processing
- Payment reconciliation

---

### Payment Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| payment_providers | payment_provider_id | provider_key |
| gateway_configurations | gateway_configuration_id | payment_provider_id, tenant_id |
| merchant_accounts | merchant_account_id | gateway_configuration_id, merchant_identifier |
| payment_transactions | payment_transaction_id | order_id, invoice_id, merchant_account_id, provider_transaction_id, payment_status, created_at |
| payment_intents | payment_intent_id | payment_transaction_id, intent_status |
| refund_transactions | refund_transaction_id | payment_transaction_id, created_at |
| settlement_records | settlement_id | merchant_account_id, settlement_date |
| reconciliation_records | reconciliation_id | settlement_id |
| webhook_events | webhook_event_id | provider_event_id, processing_status, received_at |

---

## Payment Query Optimization

Indexes optimize:

- Payment authorization
- Order payment lookup
- Subscription payment lookup
- Merchant reconciliation
- Gateway reporting
- Failed payment recovery
- Webhook idempotency
- Settlement reporting

---

# Chapter 10 — Notification Indexes

## 10.1 Purpose

Notification indexes optimize message generation, delivery processing, queue management, and delivery history across all supported communication channels.

The strategy prioritizes reliable asynchronous processing and efficient delivery tracking.

---

## Notification Index Strategy

Primary objectives include:

- Notification generation
- Queue processing
- Delivery tracking
- Template lookup
- Retry processing
- Notification history

---

### Notification Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| notifications | notification_id | tenant_id, notification_status, created_at |
| notification_templates | notification_template_id | template_key, notification_channel |
| email_messages | email_message_id | notification_id, delivery_status |
| sms_messages | sms_message_id | notification_id, delivery_status |
| push_notifications | push_notification_id | notification_id, delivery_status |
| notification_queue | queue_id | notification_status, scheduled_at, priority |

---

## Notification Query Optimization

Indexes optimize:

- Pending notification retrieval
- Queue processing
- Failed delivery retries
- Notification history
- Email tracking
- SMS tracking
- Push notification delivery
- Operational monitoring

---

# Chapter 11 — Analytics Indexes

## 11.1 Purpose

The Analytics Domain provides operational intelligence, executive dashboards, KPI reporting, business intelligence, and historical analysis for the FluxDine platform.

Unlike operational domains, Analytics is optimized for high-volume read operations, aggregation, filtering, and trend analysis rather than transactional writes.

The indexing strategy prioritizes analytical performance while ensuring that Analytics remains a read-model and never becomes the operational source of truth.

---

## Analytics Index Strategy

Primary indexing objectives include:

- Dashboard loading
- KPI calculation
- Historical reporting
- Trend analysis
- Business intelligence
- Usage analytics
- Time-series aggregation

---

### Analytics Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| kpi_definitions | kpi_definition_id | category, metric_key |
| kpi_snapshots | snapshot_id | tenant_id, snapshot_date, kpi_definition_id |
| reports | report_id | tenant_id, report_type, created_at |
| dashboard_metrics | metric_id | report_id, metric_name |
| usage_metrics | usage_metric_id | tenant_id, metric_date, metric_type |

---

## Analytics Query Optimization

Indexes optimize:

- Executive dashboards
- Restaurant dashboards
- HQ dashboards
- Daily KPI calculations
- Weekly analytics
- Monthly reports
- Revenue trends
- Customer trends
- Order trends
- Performance comparisons

---

# Chapter 12 — Branding Indexes

## 12.1 Purpose

The Branding Domain manages tenant identity, themes, domains, public assets, and SEO configuration.

Indexes optimize branding resolution while ensuring that every tenant's branding remains completely isolated.

---

## Branding Index Strategy

Primary objectives include:

- Domain resolution
- Theme loading
- Branding retrieval
- SEO lookup
- Asset loading

---

### Branding Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| tenant_branding | branding_id | tenant_id |
| themes | theme_id | tenant_branding_id, theme_name |
| brand_assets | asset_id | tenant_branding_id, asset_type |
| domains | domain_id | domain_name, tenant_id |
| seo_settings | seo_setting_id | domain_id |

---

## Branding Query Optimization

Indexes optimize:

- Website loading
- Domain routing
- Theme loading
- Branding retrieval
- Asset retrieval
- SEO generation

---

# Chapter 13 — Feature Management Indexes

## 13.1 Purpose

The Feature Management Domain controls platform capabilities, subscription entitlements, tenant overrides, and feature availability.

Indexes support rapid feature evaluation during authentication, authorization, subscription validation, and runtime feature checks.

---

## Feature Management Index Strategy

Primary objectives include:

- Feature lookup
- Plan evaluation
- Tenant overrides
- Runtime feature resolution
- Feature usage reporting

---

### Feature Management Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| features | feature_id | feature_key, category |
| feature_flags | feature_flag_id | feature_id, environment |
| subscription_features | subscription_feature_id | subscription_plan_id, feature_id |
| tenant_features | tenant_feature_id | tenant_id, feature_id |
| feature_usage | usage_id | tenant_id, feature_id, usage_date |

---

## Feature Query Optimization

Indexes optimize:

- Login feature evaluation
- Dashboard feature loading
- Subscription validation
- Tenant override lookup
- Usage monitoring
- Feature reporting

---

# Chapter 14 — Shared Platform Indexes

## 14.1 Purpose

The Shared Platform Domain provides infrastructure services shared across every business domain.

Indexes support platform-wide logging, monitoring, auditing, background processing, storage, and search while remaining independent of operational business ownership.

---

## Shared Platform Index Strategy

Primary objectives include:

- Audit retrieval
- File lookup
- Background processing
- Job scheduling
- Monitoring
- System search
- Operational diagnostics

---

### Shared Platform Indexes

| Table | Primary Indexes | Additional Logical Indexes |
|--------|-----------------|----------------------------|
| audit_logs | audit_log_id | tenant_id, entity_type, entity_id, created_at |
| activity_logs | activity_log_id | tenant_id, user_id, created_at |
| files | file_id | tenant_id, entity_type, entity_id |
| background_jobs | job_id | job_type, job_status, scheduled_at |
| scheduled_tasks | task_id | background_job_id, next_execution |
| monitoring_events | event_id | event_type, severity, created_at |
| search_index | search_document_id | tenant_id, entity_type |
| api_keys | api_key_id | tenant_id, api_key |

---

## Shared Platform Query Optimization

Indexes optimize:

- Audit history
- Activity history
- File retrieval
- Scheduled job execution
- Monitoring dashboards
- Incident investigation
- Platform diagnostics
- Search operations

---

# Chapter 15 — Cross-Domain Index Strategy

## 15.1 Purpose

Many FluxDine queries span multiple business domains.

This chapter defines the logical indexing strategy required to support efficient cross-domain joins, tenant-scoped operations, and platform-wide reporting while preserving tenant isolation.

Unlike previous chapters, this chapter does not define indexes for individual tables. Instead, it establishes architectural rules for indexes that support interactions between domains.

---

## 15.2 Cross-Domain Objectives

Cross-domain indexes shall support:

- Tenant ownership traversal
- Cross-domain joins
- Dashboard aggregation
- Operational reporting
- Payment lookups
- Reservation lookups
- Customer history
- Audit reporting
- Notification tracing

---

## 15.3 Tenant Ownership Index Strategy

Every tenant-owned table shall expose an efficient access path for tenant-scoped queries.

Typical indexed ownership columns include:

- tenant_id
- restaurant_id
- branch_id

These indexes enable:

- Tenant isolation
- Administrative reporting
- Operational filtering
- Multi-tenant scalability

---

## 15.4 Foreign Key Join Strategy

Frequently joined foreign keys shall be indexed to minimize join cost.

Examples include:

- restaurant_id
- branch_id
- customer_id
- order_id
- invoice_id
- payment_transaction_id
- reservation_id
- feature_id

These indexes support efficient navigation across related business entities.

---

## 15.5 Composite Index Strategy

Composite indexes shall reflect actual application query patterns.

Typical combinations include:

- tenant_id + status
- restaurant_id + created_at
- branch_id + reservation_date
- customer_id + created_at
- order_status + created_at
- invoice_status + issue_date
- payment_status + created_at
- notification_status + scheduled_at

Column ordering shall prioritize equality filters before range filters.

---

## 15.6 Reporting Index Strategy

Reporting indexes shall support high-volume analytical workloads.

Examples include:

- created_at
- updated_at
- completed_at
- payment_date
- reservation_date
- settlement_date

These indexes optimize:

- Daily reports
- Weekly reports
- Monthly reports
- Revenue analysis
- Growth analysis
- Operational KPIs

---

## 15.7 Search Index Strategy

Search indexes support customer-facing discovery.

Examples include:

- Restaurant search
- Menu search
- Customer search
- Domain search

Search indexes shall remain logically separate from transactional indexes.

---

## 15.8 Background Processing Index Strategy

Background jobs rely on indexes supporting:

- Scheduled execution
- Retry processing
- Queue prioritization
- Notification delivery
- Payment reconciliation
- Analytics generation

Typical indexed fields include:

- job_status
- scheduled_at
- priority
- retry_count
- next_execution

---

## 15.9 Cross-Domain Performance Principles

Cross-domain indexes shall follow these principles:

### Principle 1

Preserve tenant isolation.

---

### Principle 2

Optimize joins without introducing redundant indexes.

---

### Principle 3

Prioritize operational workloads over infrequent administrative queries.

---

### Principle 4

Support predictable execution plans.

---

### Principle 5

Favor reusable composite indexes over excessive single-column indexes.

---

### Principle 6

Indexing strategy shall evolve only through approved Architecture Decision Records (ADRs).

---

# Chapter 16 — Query Optimization Guidelines

## 16.1 Purpose

The Query Optimization Guidelines define the architectural principles governing how indexes support efficient query execution throughout the FluxDine platform.

Rather than prescribing database-specific execution plans, these guidelines establish logical engineering practices that ensure predictable performance across operational, analytical, and administrative workloads.

---

## 16.2 Query Optimization Objectives

The indexing strategy shall optimize the following workload categories:

- Authentication
- Tenant Resolution
- Restaurant Operations
- Customer Experience
- Online Ordering
- Reservation Management
- Payment Processing
- Subscription Billing
- Notification Delivery
- Analytics
- Background Processing
- Administrative Reporting

---

## 16.3 Query Classification

### High-Frequency Queries

Executed thousands of times per day.

Examples:

- Customer login
- Menu loading
- Product lookup
- Cart retrieval
- Active orders
- Active reservations

Indexes for these queries shall prioritize minimum response time.

---

### Medium-Frequency Queries

Executed hundreds of times daily.

Examples:

- Dashboard loading
- Restaurant administration
- Rider management
- Billing history
- Payment lookup

Indexes shall balance read performance with write overhead.

---

### Low-Frequency Queries

Examples:

- Historical reporting
- Administrative exports
- Audit investigations
- Compliance reports

These queries may rely on analytical indexes rather than transactional indexes.

---

## 16.4 Composite Index Ordering

Composite indexes shall follow this ordering principle:

1. Equality filters
2. Tenant ownership
3. Status
4. Date ranges
5. Sorting columns

Example:

```text
tenant_id
restaurant_id
order_status
created_at
```

rather than

```text
created_at
tenant_id
status
```

---

## 16.5 Covering Index Strategy

Covering indexes should be introduced for frequently executed read-only queries that repeatedly access a predictable set of columns.

Typical candidates include:

- Restaurant dashboard
- Customer order history
- Reservation calendar
- Payment history
- Subscription dashboard

---
## 16.6 Partial Index Strategy

Partial indexes optimize queries that operate on a predictable subset of rows.

Rather than indexing an entire table, a partial index targets only records satisfying a defined condition, reducing storage requirements and improving query efficiency.

Typical candidates include:

- Active Orders
- Pending Notifications
- Open Refund Requests
- Active Reservations
- Enabled Features
- Active Sessions

Partial indexes should be considered only when:

- The filtered subset represents a small percentage of the table.
- The query is executed frequently.
- Full-table indexing would introduce unnecessary maintenance overhead.

Engineering teams shall document the business justification for every partial index.
---
## 16.7 Full-Text Search Strategy

Search indexes support customer-facing discovery features throughout the platform.

Typical search workloads include:

- Restaurant Search
- Menu Search
- Product Search
- Customer Search
- Help Center Search

Search indexes shall remain logically independent from transactional indexes.

The indexing strategy should prioritize:

- Fast keyword lookup
- Ranking relevance
- Typo tolerance (where supported)
- Language-aware searching

Future platform enhancements may introduce semantic search and AI-assisted retrieval using dedicated search indexes.

## 16.8 Pagination Optimization

Indexes supporting pagination should prioritize:

- created_at
- updated_at
- order_date
- reservation_date
- payment_date

Offset-based pagination should be minimized for large datasets.

Cursor-based pagination is preferred.

---

## 16.9 Reporting Optimization

Reporting queries shall utilize indexes optimized for:

- Date filtering
- Tenant filtering
- Restaurant filtering
- Branch filtering
- Status filtering

---

## 16.10 Analytical Optimization

Analytical queries should prioritize:

- Time-series aggregation
- KPI calculations
- Revenue reporting
- Customer growth
- Order trends

Analytical indexes shall remain separate from transactional optimization whenever practical.

---

# Chapter 17 — Index Maintenance Strategy

## 17.1 Purpose

Indexes require continuous monitoring and maintenance throughout the lifecycle of the platform.

This chapter defines the engineering strategy for maintaining optimal index performance.

---

## 17.2 Maintenance Objectives

Index maintenance shall ensure:

- Consistent query performance
- Efficient storage utilization
- Accurate optimizer statistics
- Minimal fragmentation
- Predictable execution plans

---

## 17.3 Index Lifecycle

Every index progresses through the following lifecycle:

1. Proposal
2. Engineering Review
3. Architecture Approval
4. Implementation
5. Monitoring
6. Optimization
7. Deprecation (if required)

---

## 17.4 Monitoring Strategy

Indexes shall be monitored for:

- Read utilization
- Write overhead
- Storage consumption
- Fragmentation
- Execution frequency

---

## 17.5 Performance Review

Engineering teams should periodically review:

- Unused indexes
- Duplicate indexes
- Redundant composite indexes
- Missing indexes
- Slow query candidates

---

## 17.6 Maintenance Principles

- Avoid unnecessary indexes.
- Remove obsolete indexes only after analysis.
- Maintain optimizer statistics.
- Review indexing after major schema changes.
- Validate performance before production deployment.

---
## 17.7 Index Anti-Patterns

The following practices shall be avoided when designing or maintaining indexes.

### Anti-Pattern 1 — Duplicate Indexes

Creating multiple indexes that provide identical access paths.

---

### Anti-Pattern 2 — Overlapping Composite Indexes

Creating composite indexes that duplicate the leading columns of existing indexes without measurable benefit.

---

### Anti-Pattern 3 — Low Cardinality Indexes

Indexing columns containing very few distinct values unless justified by specific query patterns.

---

### Anti-Pattern 4 — Excessive Indexing

Creating indexes for every column without considering maintenance cost or workload characteristics.

---

### Anti-Pattern 5 — Unused Indexes

Retaining indexes that are no longer referenced by production workloads.

---

### Anti-Pattern 6 — Incorrect Column Ordering

Defining composite indexes with column order that does not match application query patterns.

---

### Anti-Pattern 7 — Ignoring Tenant Filtering

Creating indexes that omit tenant ownership columns for tenant-scoped queries.

---

### Engineering Guidance

Engineering teams should periodically review index usage to identify and eliminate anti-patterns while preserving application performance and tenant isolation.
---
# Chapter 18 — Engineering Rules

## 18.1 Purpose

These rules establish mandatory engineering standards governing all database indexes within the FluxDine platform.

---

# Engineering Rules

## Rule ERI-001

Every index shall have a documented engineering purpose.

---

## Rule ERI-002

Every frequently joined foreign key shall be indexed.

---

## Rule ERI-003

Duplicate indexes are prohibited.

---

## Rule ERI-004

Composite indexes shall reflect actual application query patterns.

---

## Rule ERI-005

Tenant-owned tables shall prioritize tenant-scoped indexes.

---

## Rule ERI-006

Unique constraints should utilize unique indexes whenever appropriate.

---

## Rule ERI-007

Indexes shall remain technology-independent within this specification.

---

## Rule ERI-008

Index modifications require engineering review.

---

## Rule ERI-009

Breaking index changes require an approved Architecture Decision Record.

---

## Rule ERI-010

This specification is the authoritative source for logical database indexing.

---

# Chapter 19 — Architecture Decision Records

The following ADRs govern the indexing strategy of the FluxDine platform.

---

## ADR-I-001

Every persistent table shall expose an efficient primary lookup path.

---

## ADR-I-002

Tenant ownership shall always be indexable.

---

## ADR-I-003

Foreign key relationships shall be optimized using logical indexes.

---

## ADR-I-004

Composite indexes shall be driven by business query patterns.

---

## ADR-I-005

Analytics shall utilize dedicated analytical indexes.

---

## ADR-I-006

Background processing shall utilize queue-optimized indexes.

---

## ADR-I-007

Search workloads shall remain logically separate from transactional indexes.

---

## ADR-I-008

Index redundancy shall be minimized throughout the platform.

---

## ADR-I-009

Performance optimization shall never compromise tenant isolation.

---

## ADR-I-010

The Index Specification is the authoritative engineering reference for logical indexing.

---

# Chapter 20 — Performance Guidelines

## 20.1 Purpose

These guidelines summarize the architectural goals of the FluxDine indexing strategy.

---

## Performance Objectives

The database should optimize for:

- Low-latency customer interactions
- Predictable dashboard loading
- Efficient restaurant operations
- Scalable tenant growth
- High-volume transaction processing
- Reliable background processing
- Fast analytical reporting

---

## Performance Principles

### Principle 1

Optimize for production workloads.

---

### Principle 2

Measure before introducing new indexes.

---

### Principle 3

Prefer reusable indexes over specialized indexes.

---

### Principle 4

Minimize write overhead.

---

### Principle 5

Support horizontal SaaS scalability.

---

### Principle 6

Preserve deterministic execution plans whenever possible.

---

# Appendix A — Complete Index Matrix

This appendix serves as the master catalog of logical indexes defined within the FluxDine platform.

| Domain | Primary | Foreign Key | Unique | Composite | Partial | Analytical |
|---------|--------:|------------:|-------:|----------:|--------:|-----------:|
| Platform | ✓ | ✓ | ✓ | ✓ | — | — |
| Identity | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| Tenant | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| Restaurant | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| Commerce | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| Customer | ✓ | ✓ | ✓ | ✓ | — | — |
| Reservation | ✓ | ✓ | ✓ | ✓ | — | — |
| Billing | ✓ | ✓ | ✓ | ✓ | — | ✓ |
| Payment | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Notification | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| Analytics | ✓ | ✓ | ✓ | ✓ | — | ✓ |
| Branding | ✓ | ✓ | ✓ | ✓ | — | — |
| Feature Management | ✓ | ✓ | ✓ | ✓ | — | — |
| Shared Platform | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |

---

# Appendix B — Composite Index Matrix

Representative composite indexes include:

| Table | Composite Columns |
|---------|------------------|
| user_memberships | user_id + tenant_id |
| tenant_features | tenant_id + feature_id |
| branches | restaurant_id + branch_code |
| restaurant_tables | seating_area_id + table_number |
| menu_items | restaurant_id + sku |
| carts | customer_id + restaurant_id + cart_status |
| orders | tenant_id + restaurant_id + order_status + created_at |
| reservations | branch_id + reservation_date + reservation_status |
| payment_transactions | merchant_account_id + payment_status + created_at |
| notification_queue | notification_status + priority + scheduled_at |

---

# Appendix C — Query → Index Mapping

| Query | Supporting Index |
|---------|-----------------|
| Customer Login | users(email) |
| Restaurant Lookup | restaurants(tenant_id, restaurant_slug) |
| Menu Loading | menu_items(category_id, availability_status) |
| Active Orders | orders(tenant_id, order_status, created_at) |
| Reservation Calendar | reservations(branch_id, reservation_date) |
| Invoice History | invoices(subscription_id, issue_date) |
| Payment Lookup | payment_transactions(provider_transaction_id) |
| Notification Queue | notification_queue(notification_status, scheduled_at) |
| Dashboard Metrics | usage_metrics(tenant_id, metric_date) |

---

# Appendix D — Reserved Future Indexes

Reserved indexing strategies for future platform modules.

## Inventory

- inventory_items
- warehouses
- stock_transactions

---

## Kitchen Display System

- kitchen_orders
- preparation_queue
- kitchen_stations

---

## Marketplace

- marketplace_orders
- commissions
- payouts

---

## Artificial Intelligence

- ai_conversations
- ai_recommendations
- ai_embeddings

---

## Workforce Management

- employee_schedules
- attendance
- payroll

---

## Fleet Management

- vehicles
- routes
- gps_tracking

---

Future indexes shall be introduced only through approved Architecture Decision Records.

---

# References

This specification should be read alongside:

## Database Engineering

- 00 Database Naming Standards
- 01 Complete Database Schema Specification
- 02 Table Specifications
- 03 Relationships
- 04 Constraints
- 06 Enum Specification
- 07 Database Migration Strategy
- 08 Drizzle ORM Mapping

---

## Core Architecture

- System Architecture Blueprint
- Database Architecture & Multi-Tenant Data Model
- API & Service Architecture
- Security Architecture
- Infrastructure Architecture

---

## Governance

- Architecture Principles
- Documentation Standards
- ADR Register

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 0.1 | Initial Draft | FluxDine Engineering | Document structure established |
| 0.5 | Domain Index Strategy | FluxDine Engineering | Platform through Shared Platform indexing strategy completed |
| 0.8 | Engineering Governance | FluxDine Engineering | Query optimization, maintenance strategy, engineering rules, ADRs completed |
| 1.0 | Final Release | FluxDine Engineering | Approved as the authoritative database indexing specification |