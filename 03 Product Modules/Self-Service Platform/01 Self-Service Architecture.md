# 02 Product Modules

# Self-Service Platform

# 01 — Self-Service Architecture

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-PM-SSP-001 |
| **Document Name** | Self-Service Architecture |
| **Version** | 1.0 |
| **Status** | Approved and Locked |
| **Owner** | FluxDine Product & Engineering |
| **Classification** | Internal Product Module Specification |
| **Depends On** | HQ Platform Architecture<br>Restaurant Platform Architecture<br>Authentication |
| **Referenced By** | All Self-Service Platform Modules |

---

# Dependencies

This specification depends upon:

- HQ Platform Architecture
- Restaurant Platform Architecture
- Authentication
- Authorization Matrix
- Feature Availability
- Payment Framework
- Tenant Provisioning
- REST API Specification

The Self-Service Platform orchestrates the lifecycle of onboarding new restaurant tenants into the FluxDine ecosystem.

---

# Referenced By

This specification is referenced by:

- Landing Website
- Registration Flow
- Email Verification
- Plan Selection
- Trial Management
- Onboarding Wizard
- Restaurant Configuration
- Payment Gateway Configuration
- Domain Configuration
- Theme Configuration
- Launch Workflow
- Customer Journey

---

# Document Status

| Item | Value |
|------|-------|
| Status | Approved and Locked |
| Approval | Approved |
| Implementation | Architecture Complete |
| Last Updated | TBD |

---

# Purpose

The Self-Service Platform provides a fully automated onboarding experience for prospective restaurant customers.

It enables customers to:

- Discover FluxDine
- Register an account
- Verify their identity
- Select a subscription plan
- Configure their restaurant
- Configure payment methods
- Configure domains
- Customize branding
- Launch their restaurant

The platform minimizes manual intervention while maintaining security, scalability, and consistency.

---

# Scope

This specification defines:

- Self-Service architecture
- Tenant onboarding lifecycle
- Customer onboarding flow
- Platform provisioning
- Configuration orchestration
- Launch workflow
- Integration boundaries

---

# Out of Scope

This specification does not define:

- Restaurant operations
- HQ administration
- Subscription billing implementation
- Payment gateway implementation
- Theme rendering
- Customer ordering

These responsibilities belong to their respective platform modules.

---

# Self-Service Philosophy

The Self-Service Platform shall:

- Automate onboarding.
- Minimize manual administration.
- Provide guided configuration.
- Validate every onboarding step.
- Maintain tenant isolation.
- Support enterprise scalability.
- Enable future onboarding capabilities.

The onboarding experience shall remain intuitive while enforcing architectural consistency.

---

# Objectives

Primary objectives include:

- Acquire new customers.
- Automate tenant creation.
- Reduce onboarding time.
- Improve conversion rates.
- Standardize restaurant setup.
- Support future onboarding enhancements.

---

# Platform Position

FluxDine consists of three major platforms.

```text
HQ Platform

↓

Self-Service Platform

↓

Restaurant Platform
```

The Self-Service Platform acts as the orchestration layer between the HQ Platform and newly provisioned Restaurant Platform tenants.

---

# Core Responsibilities

The Self-Service Platform is responsible for:

- Customer Acquisition
- Account Registration
- Email Verification
- Plan Selection
- Trial Activation
- Tenant Provisioning
- Initial Configuration
- Launch Orchestration

The platform does not execute restaurant business operations.

---

# Self-Service Architecture

```text
Prospective Customer

↓

Landing Website

↓

Registration

↓

Email Verification

↓

Plan Selection

↓

Trial / Subscription

↓

Onboarding Wizard

↓

Restaurant Configuration

↓

Payment Configuration

↓

Domain Configuration

↓

Theme Configuration

↓

Launch Workflow

↓

Restaurant Platform
```

Each stage represents a controlled progression toward a fully operational restaurant tenant.

---

# Primary Actors

The Self-Service Platform supports:

- Prospective Customer
- Restaurant Owner
- Restaurant Administrator
- HQ Platform Services

Each actor interacts with the platform according to defined responsibilities.

---

# Tenant Provisioning

Successful onboarding results in creation of:

- Restaurant Tenant
- Administrator Account
- Default Configuration
- Initial Feature Availability
- Initial Theme
- Initial Domain Configuration
- Default Operational Settings

Provisioning is coordinated through the HQ Platform.

---

# Integration Model

The Self-Service Platform integrates with:

- HQ Platform
- Authentication
- Payment Framework
- Feature Availability
- Restaurant Settings
- Theme Engine
- Notification Service

Operational ownership remains within the corresponding platform modules.

---

# Design Principles

The Self-Service Platform follows these principles:

- Guided Onboarding
- Automation First
- Tenant Isolation
- Configuration over Hardcoding
- Security by Default
- Extensibility
- Maintainability

These principles govern all Self-Service Platform development.

---
# Customer Onboarding Lifecycle

Every restaurant follows the same onboarding lifecycle.

```text
Visitor

↓

Registration

↓

Email Verification

↓

Plan Selection

↓

Trial / Subscription

↓

Onboarding Wizard

↓

Restaurant Configuration

↓

Payment Configuration

↓

Domain Configuration

↓

Theme Configuration

↓

Launch

↓

Restaurant Platform
```

Each stage shall complete successfully before progressing to the next stage.

---

# Onboarding States

Every onboarding session exists in one of the following states.

| State | Description |
|--------|-------------|
| Started | Registration initiated |
| Email Pending | Awaiting verification |
| Verified | Email verified |
| Plan Selected | Subscription/trial selected |
| Provisioning | Restaurant tenant being created |
| Configuring | Restaurant setup in progress |
| Ready for Launch | Configuration complete |
| Launched | Restaurant operational |
| Suspended | Onboarding suspended |
| Cancelled | Onboarding abandoned |

State transitions shall remain deterministic.

---

# Customer Registration

Customer registration includes:

- Account Creation
- Identity Verification
- Initial Authentication
- Terms Acceptance
- Privacy Acceptance

Registration is documented separately in the Registration Flow specification.

---

# Tenant Provisioning Workflow

Tenant provisioning is coordinated through the HQ Platform.

```text
Customer

↓

Self-Service Platform

↓

HQ Platform

↓

Tenant Provisioning

↓

Restaurant Platform

↓

Configuration Initialized
```

The Self-Service Platform orchestrates provisioning but does not create tenants independently.

---

# Platform Provisioning

Successful provisioning creates:

- Restaurant Tenant
- Default Administrator
- Default Branch
- Restaurant Settings
- Theme Configuration
- Feature Availability
- Default Roles
- Default Permissions

Provisioning shall execute atomically.

---

# Configuration Stages

Restaurant configuration proceeds through sequential stages.

```text
Restaurant Profile

↓

Business Information

↓

Operational Settings

↓

Payment Configuration

↓

Domain Configuration

↓

Theme Configuration

↓

Launch Validation
```

Configuration dependencies shall be respected.

---

# Progress Tracking

The onboarding wizard shall track customer progress.

Examples include:

- Registration Completed
- Email Verified
- Plan Selected
- Restaurant Configured
- Payment Configured
- Domain Connected
- Theme Selected
- Ready to Launch

Progress shall be recoverable after interruptions.

---

# Session Management

An onboarding session maintains:

- Session Identifier
- Customer Identifier
- Current Step
- Completion Percentage
- Last Activity
- Session Status

Sessions shall support continuation across devices after authentication.

---

# Save and Resume

Customers may pause onboarding.

Supported capabilities include:

- Automatic Save
- Manual Save
- Resume Later
- Device Independence

Progress shall never be lost because of temporary interruptions.

---

# Validation Strategy

Every onboarding step shall validate:

- Required Information
- Business Rules
- Configuration Consistency
- External Dependencies
- Feature Availability

Invalid configurations shall not progress to the next stage.

---

# Customer Guidance

The Self-Service Platform provides guided onboarding.

Examples include:

- Progress Indicators
- Step Explanations
- Validation Messages
- Success Messages
- Recommended Next Steps

Guidance shall minimize onboarding friction.

---

# Default Configuration

Every new restaurant receives default configuration.

Examples include:

- Default Theme
- Default Currency
- Default Time Zone
- Default Business Hours
- Default Branch
- Default Feature Set

Default values may later be customized.

---

# Feature Initialization

Initial feature availability is obtained from the HQ Platform.

Examples include:

- Reservations
- Online Ordering
- Payments
- Reports
- Theme Customization

Feature initialization shall occur during tenant provisioning.

---

# Notification Workflow

Customers receive onboarding notifications.

Examples include:

- Registration Successful
- Email Verification Required
- Email Verified
- Trial Started
- Configuration Complete
- Restaurant Ready
- Launch Successful

Notification delivery is managed through the Notification Service.

---

# Failure Recovery

If onboarding cannot continue:

- Progress shall be preserved.
- Configuration shall remain valid.
- Users shall receive meaningful feedback.
- Failed provisioning shall be recoverable.
- Duplicate tenant creation shall be prevented.

Recovery procedures shall preserve data integrity.

---

# Self-Service Integrations

The Self-Service Platform integrates with:

```text
Self-Service Platform

├── HQ Platform

├── Authentication

├── Restaurant Platform

├── Payment Framework

├── Restaurant Settings

├── Theme Engine

├── Feature Availability

├── Notification Service

├── Audit Service

└── Shared Platform Services
```

All integrations shall use documented service interfaces.

---

# Data Ownership

The Self-Service Platform owns:

- Onboarding Session
- Progress Tracking
- Registration Workflow
- Launch Orchestration

The HQ Platform owns:

- Tenant Provisioning
- Subscription Management
- Feature Flags
- Platform Administration

The Restaurant Platform owns:

- Restaurant Operations
- Orders
- Reservations
- Customers
- Restaurant Configuration after launch

Ownership boundaries shall remain clearly separated.

---

# Operational Workflow

The complete onboarding workflow follows:

```text
Visitor

↓

Registration

↓

Verification

↓

Plan Selection

↓

Provisioning

↓

Configuration

↓

Validation

↓

Launch

↓

Restaurant Platform
```

The Self-Service Platform orchestrates onboarding while delegating platform-specific responsibilities to the HQ Platform and Restaurant Platform.

---

# Performance

The Self-Service Platform shall:

- Minimize onboarding latency.
- Save progress automatically.
- Support resumable sessions.
- Provision tenants efficiently.
- Validate configuration quickly.

Performance optimizations shall never compromise onboarding consistency.

---
# Self-Service Security

The Self-Service Platform manages customer onboarding and tenant provisioning and therefore requires comprehensive security controls.

Every onboarding operation shall validate:

- Authentication
- Authorization
- Session Validity
- Tenant Context
- Onboarding State

Unauthorized onboarding operations shall be rejected.

---

# Authentication

Authentication is required after account registration.

Supported authentication mechanisms include:

- Email and Password
- Multi-Factor Authentication (Future)
- Single Sign-On (Future)

Authentication services are provided by the centralized Authentication module.

---

# Authorization

Authorization determines which onboarding actions may be performed.

| Operation | Visitor | Registered Customer | Restaurant Administrator |
|-----------|----------|---------------------|--------------------------|
| View Landing Website | ✓ | ✓ | ✓ |
| Register Account | ✓ | No | No |
| Verify Email | ✓ | ✓ | No |
| Select Plan | No | ✓ | No |
| Continue Onboarding | No | ✓ | No |
| Configure Restaurant | No | ✓ | No |
| Launch Restaurant | No | ✓ | No |

Authorization shall be enforced through the Authorization Service.

---

# Tenant Isolation

Tenant creation shall occur only after successful onboarding.

```text
Customer

↓

Provisioning

↓

Restaurant Tenant

↓

Restaurant Platform
```

Customers shall never gain access to another tenant's resources during onboarding.

---

# Session Protection

Every onboarding session shall be uniquely identified.

Each session maintains:

- Session Identifier
- Customer Identifier
- Current Step
- Completion Status
- Expiration Time
- Last Activity

Expired sessions shall require re-authentication before continuing.

---

# Provisioning Protection

Tenant provisioning is a protected platform operation.

Provisioning shall validate:

- Customer Identity
- Subscription Eligibility
- Email Verification
- Configuration Completeness
- Platform Availability

Provisioning shall execute only once for a successful onboarding session.

---

# Configuration Protection

Configuration data created during onboarding includes:

- Restaurant Profile
- Business Information
- Theme Selection
- Domain Configuration
- Payment Configuration

Configuration shall remain private until the restaurant is launched.

---

# Launch Protection

Restaurant launch shall validate:

- Required Configuration Completed
- Tenant Successfully Provisioned
- Feature Initialization Completed
- Platform Validation Passed

Restaurants shall not launch with incomplete mandatory configuration.

---

# Audit Trail

Every significant onboarding operation shall generate an audit event.

Examples include:

- Registration Started
- Registration Completed
- Email Verified
- Plan Selected
- Trial Activated
- Tenant Provisioned
- Configuration Updated
- Restaurant Launched

Audit records integrate with the Audit Service.

---

# Monitoring

Operational monitoring includes:

- Registration Success Rate
- Email Verification Rate
- Trial Activation Rate
- Provisioning Duration
- Launch Success Rate
- Onboarding Completion Rate
- Abandoned Onboarding Sessions

Monitoring information is available through the Monitoring Center.

---

# Self-Service Analytics

The Self-Service Platform exposes operational metrics.

Examples include:

## Acquisition

- Website Visitors
- Registration Conversion
- Verification Rate
- Plan Selection Rate

---

## Onboarding

- Wizard Completion Rate
- Provisioning Success Rate
- Launch Success Rate
- Average Onboarding Duration

---

## Platform Health

- Provisioning Failures
- Synchronization Failures
- Configuration Validation Errors

Analytics support operational optimization while respecting customer privacy.

---

# Notifications

The Self-Service Platform integrates with the Notification Service.

Examples include:

- Welcome Email
- Verification Email
- Trial Started
- Trial Expiration Reminder
- Restaurant Ready
- Launch Successful
- Configuration Reminder

Notification delivery is managed centrally.

---

# Platform Integrations

The Self-Service Platform integrates with:

```text
Self-Service Platform

├── HQ Platform

├── Authentication

├── Authorization

├── Restaurant Platform

├── Payment Framework

├── Restaurant Settings

├── Theme Engine

├── Feature Availability

├── Notification Service

├── Audit Service

└── Shared Platform Services
```

All integrations shall use documented service interfaces.

---

# Cross-Module Navigation

The onboarding experience supports navigation between related modules.

Examples include:

| Self-Service Step | Destination Module |
|-------------------|--------------------|
| Registration | Authentication |
| Plan Selection | HQ Subscription Management |
| Restaurant Setup | Restaurant Settings |
| Theme Setup | Theme Engine |
| Payment Setup | Payment Framework |
| Launch | Restaurant Platform |

Cross-module navigation shall preserve onboarding progress.

---

# Operational Availability

The Self-Service Platform shall remain continuously available.

Temporary failures shall:

- Preserve onboarding progress.
- Prevent duplicate provisioning.
- Retry transient operations.
- Display meaningful recovery information.
- Maintain session integrity.

Operational continuity is essential for customer acquisition.

---

# Platform Consistency

The Self-Service Platform shall maintain consistency across:

- Registration
- Verification
- Plan Selection
- Provisioning
- Configuration
- Launch

Every onboarding session shall produce predictable outcomes.

---

# Platform Scalability

The architecture shall support:

- Thousands of concurrent visitors
- Large-scale registration campaigns
- Enterprise onboarding
- Franchise onboarding
- Global deployments

Scalability shall be achieved without redesigning the onboarding architecture.

---

# Customer Experience

The Self-Service Platform shall:

- Provide a guided onboarding journey.
- Minimize required manual input.
- Preserve customer progress.
- Validate information immediately.
- Provide clear feedback.
- Reduce onboarding friction.

The onboarding experience shall maximize successful restaurant launches.

---

# Future Self-Service Capabilities

The architecture supports future enhancements including:

- AI Onboarding Assistant
- Conversational Setup Wizard
- AI Restaurant Configuration
- Industry-Specific Templates
- Multi-Restaurant Onboarding
- Franchise Setup Wizard
- Guided Data Import
- QR-Based Mobile Onboarding
- Collaborative Team Onboarding
- Intelligent Progress Recommendations
- Automated Configuration Validation
- Personalized Customer Journeys

These capabilities may be introduced without restructuring the existing Self-Service Platform architecture.

---

# Operational Workflow

The Self-Service Platform coordinates the complete onboarding lifecycle.

```text
Visitor

↓

Registration

↓

Verification

↓

Provisioning

↓

Configuration

↓

Launch Validation

↓

Restaurant Launch

↓

Restaurant Platform
```

The Self-Service Platform remains the authoritative onboarding orchestrator while the HQ Platform and Restaurant Platform retain ownership of their respective business domains.

---
# Engineering Rules

## Rule SSP-001

The Self-Service Platform shall act exclusively as the onboarding and provisioning orchestration layer.

It shall never implement restaurant operational business logic.

---

## Rule SSP-002

The Self-Service Platform shall never provision restaurant tenants directly.

All tenant provisioning shall be delegated to the HQ Platform Tenant Provisioning Service.

---

## Rule SSP-003

Every onboarding session shall belong to exactly one customer.

Each onboarding session shall provision at most one restaurant tenant.

---

## Rule SSP-004

The onboarding workflow shall follow the predefined sequence.

Mandatory stages shall not be skipped unless explicitly supported by platform policy.

---

## Rule SSP-005

Every onboarding step shall pass validation before progressing to the next stage.

Incomplete or invalid configuration shall never proceed.

---

## Rule SSP-006

Every provisioning operation shall execute atomically.

Partial tenant creation shall never become operational.

---

## Rule SSP-007

Every significant onboarding event shall generate an audit record.

---

## Rule SSP-008

Configuration data collected during onboarding shall remain isolated until tenant provisioning is successfully completed.

---

## Rule SSP-009

The Self-Service Platform shall communicate with all platform modules exclusively through documented APIs, domain events, and shared platform services.

---

## Rule SSP-010

This document is the authoritative Self-Service Platform Architecture specification for the FluxDine platform.

---

# Architecture Decision Records

## ADR-SSP-001

The Self-Service Platform is implemented as an independent onboarding platform positioned between the HQ Platform and Restaurant Platform.

---

## ADR-SSP-002

The HQ Platform remains the sole authority responsible for tenant provisioning.

The Self-Service Platform orchestrates but does not own provisioning.

---

## ADR-SSP-003

Every onboarding session shall be resumable.

Customers may safely continue onboarding after interruptions.

---

## ADR-SSP-004

Platform configuration shall be collected progressively through guided onboarding.

Configuration shall not require manual HQ intervention.

---

## ADR-SSP-005

Restaurant launch shall occur only after successful validation of all mandatory onboarding steps.

---

## ADR-SSP-006

Feature initialization shall be supplied by the HQ Platform immediately after tenant provisioning.

---

## ADR-SSP-007

The onboarding workflow shall remain modular.

Each onboarding phase shall be implemented as an independent platform module.

---

## ADR-SSP-008

Future onboarding capabilities shall extend the existing architecture without replacing the orchestration engine.

---

## ADR-SSP-009

The Self-Service Platform shall remain independent from restaurant operational modules.

---

## ADR-SSP-010

This document is the authoritative Self-Service Platform Architecture specification for the FluxDine platform.

---

# Quality Attributes

The Self-Service Platform architecture satisfies the following quality attributes.

| Attribute | Objective |
|-----------|-----------|
| Reliability | Predictable onboarding lifecycle |
| Availability | Continuous onboarding availability |
| Scalability | Enterprise customer onboarding |
| Performance | Fast provisioning experience |
| Security | Secure onboarding and provisioning |
| Maintainability | Modular onboarding architecture |
| Auditability | Complete onboarding traceability |
| Extensibility | Support future onboarding capabilities |
| Consistency | Deterministic onboarding workflow |
| Tenant Isolation | Independent tenant provisioning |

---

# Self-Service Platform Architecture

```text
Prospective Customer

↓

Landing Website

↓

Registration

↓

Email Verification

↓

Plan Selection

↓

Trial Management

↓

Onboarding Wizard

↓

Restaurant Configuration

↓

Payment Gateway Configuration

↓

Domain Configuration

↓

Theme Configuration

↓

Launch Workflow

↓

HQ Tenant Provisioning

↓

Restaurant Platform
```

The Self-Service Platform orchestrates the customer journey while delegating platform-specific responsibilities to the HQ Platform.

---

# Onboarding Lifecycle

```text
Visitor

↓

Account Registration

↓

Email Verification

↓

Plan Selection

↓

Trial Activated

↓

Restaurant Configuration

↓

Provisioning

↓

Validation

↓

Launch

↓

Operational Restaurant
```

Each lifecycle transition shall preserve customer progress and generate appropriate business events.

---

# Self-Service Platform Boundaries

The Self-Service Platform is responsible for:

- Landing Experience
- Customer Registration
- Email Verification
- Plan Selection
- Trial Management
- Onboarding Wizard
- Restaurant Configuration
- Payment Gateway Configuration
- Domain Configuration
- Theme Configuration
- Launch Orchestration
- Progress Tracking

The Self-Service Platform is **not** responsible for:

- Tenant Provisioning
- Subscription Billing
- Restaurant Operations
- Order Processing
- Reservation Processing
- Customer Management
- Payment Processing
- HQ Administration

These responsibilities belong to the HQ Platform and Restaurant Platform.

---

# Module Relationships

The Self-Service Platform collaborates with:

```text
Self-Service Platform

├── HQ Platform

├── Tenant Provisioning

├── Authentication

├── Authorization

├── Feature Availability

├── Payment Framework

├── Restaurant Settings

├── Theme Engine

├── Notification Service

├── Audit Service

└── Shared Platform Services
```

Each collaborating module retains ownership of its business domain while the Self-Service Platform orchestrates onboarding.

---

# Operational Data Flow

```text
Visitor

↓

Registration

↓

Onboarding Session

↓

Validation

↓

HQ Platform

↓

Tenant Provisioning

↓

Restaurant Platform

↓

Launch Completed
```

Business orchestration shall execute within the application service layer.

Tenant creation remains exclusively owned by the HQ Platform.

---

# Future Self-Service Platform Roadmap

The architecture supports future enhancements including:

### Customer Acquisition

- AI Landing Pages
- Personalized Website Content
- Marketing Campaign Integration
- Referral Programs
- Lead Qualification
- Conversion Optimization

---

### Intelligent Onboarding

- AI Onboarding Assistant
- Conversational Setup Wizard
- Automated Restaurant Configuration
- Industry Templates
- Guided Business Setup
- Smart Progress Recommendations

---

### Enterprise Onboarding

- Franchise Onboarding
- Multi-Restaurant Provisioning
- Team-Based Onboarding
- Enterprise Configuration Templates
- Bulk Restaurant Creation
- Centralized Organization Setup

---

### Automation

- Automatic DNS Configuration
- Automatic Payment Gateway Verification
- AI Theme Generation
- Automated Business Validation
- Intelligent Configuration Suggestions
- Zero-Touch Restaurant Launch

---

### Platform Evolution

- Marketplace Integrations
- Third-Party POS Import
- Restaurant Data Migration
- External Identity Providers
- Regional Compliance Wizards
- Multi-Language Onboarding

The modular architecture supports these capabilities without requiring structural redesign.

---

# Appendix A — Self-Service Platform Module Map

```text
Self-Service Platform

├── Landing Website

├── Registration Flow

├── Email Verification

├── Plan Selection

├── Trial Management

├── Onboarding Wizard

├── Restaurant Configuration

├── Payment Gateway Configuration

├── Domain Configuration

├── Theme Configuration

├── Launch Workflow

└── Customer Journey
```

---

# Appendix B — Onboarding Workflow

```text
Landing Website

↓

Registration

↓

Verification

↓

Plan Selection

↓

Configuration

↓

Provisioning

↓

Launch

↓

Restaurant Platform
```

Every workflow stage shall complete successfully before progressing to the next stage.

---

# Appendix C — Onboarding Operational States

```text
Started

↓

Verified

↓

Configured

↓

Provisioning

↓

Ready

↓

Launched
```

State transitions shall preserve onboarding consistency and prevent duplicate provisioning.

---

# Appendix D — Reserved Future Capabilities

Future versions of the Self-Service Platform may introduce:

```text
AI Onboarding Concierge

Interactive Setup Assistant

Live Progress Dashboard

Organization Management

Restaurant Import Wizard

Configuration Marketplace

Franchise Setup Center

AI Business Advisor

Zero-Touch Deployment

Smart Compliance Assistant

Automated Launch Validation

Customer Success Dashboard
```

These capabilities are outside the current implementation scope but are fully supported by the architectural direction of the FluxDine platform.

---

# References

- HQ Platform Architecture
- Restaurant Platform Architecture
- Authentication
- Authorization Matrix
- Tenant Provisioning
- Feature Availability
- Payment Framework
- Restaurant Settings
- Service Specification
- Repository Specification
- Event Catalog
- Monitoring Specification

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | Initial Release | FluxDine Product & Engineering | Approved as the authoritative Self-Service Platform Architecture specification for the FluxDine platform |
# Implementation Guidelines

The following implementation guidelines define the mandatory engineering practices for the Self-Service Platform.

---

# Module Responsibilities

The Self-Service Platform shall be responsible for:

- Customer Acquisition
- Registration Orchestration
- Email Verification Flow
- Plan Selection
- Trial Management
- Onboarding Wizard
- Restaurant Configuration
- Payment Gateway Configuration
- Domain Configuration
- Theme Configuration
- Launch Orchestration
- Progress Tracking

The Self-Service Platform shall orchestrate onboarding but shall never execute restaurant operational business logic.

---

# Service Layer

Business functionality shall be implemented through dedicated application services.

Typical services include:

```text
SelfServiceService

RegistrationService

EmailVerificationService

PlanSelectionService

TrialManagementService

OnboardingService

RestaurantConfigurationService

PaymentGatewayConfigurationService

DomainConfigurationService

ThemeConfigurationService

LaunchWorkflowService

ProgressTrackingService
```

Services coordinate onboarding workflows and business validation.

Repositories shall never contain onboarding business logic.

---

# Repository Layer

Persistence shall be encapsulated within repositories.

Typical repositories include:

```text
OnboardingSessionRepository

RegistrationRepository

VerificationRepository

PlanSelectionRepository

ConfigurationRepository

LaunchRepository

ProgressRepository
```

Repositories shall only:

- Read onboarding data
- Persist onboarding state
- Maintain progress
- Execute transactional persistence

Repositories shall never orchestrate onboarding workflows.

---

# Onboarding Validation

Every onboarding stage shall validate its required inputs before progressing.

## Registration

Validation includes:

- Valid email address
- Password policy compliance
- Terms of Service accepted
- Privacy Policy accepted

---

## Email Verification

Validation includes:

- Verification token validity
- Token expiration
- Customer ownership
- Verification status

---

## Plan Selection

Validation includes:

- Eligible plan selected
- Plan availability
- Trial eligibility
- Feature availability

---

## Restaurant Configuration

Validation includes:

- Restaurant name
- Business information
- Time zone
- Currency
- Default language

---

## Payment Gateway Configuration

Validation includes:

- Required gateway configuration
- Gateway eligibility
- Configuration completeness

Gateway verification remains the responsibility of the HQ Platform.

---

## Domain Configuration

Validation includes:

- Domain format
- Domain ownership
- DNS validation (where applicable)

---

## Theme Configuration

Validation includes:

- Required branding assets
- Theme completeness
- Configuration consistency

Validation failures shall prevent progression to subsequent onboarding stages.

---

# Provisioning Workflow

Tenant provisioning follows the approved orchestration workflow.

```text
Configuration Complete

↓

Provisioning Request

↓

HQ Platform

↓

Tenant Created

↓

Restaurant Initialized

↓

Launch Validation

↓

Restaurant Ready
```

Provisioning shall be atomic and idempotent.

---

# Business Events

The Self-Service Platform publishes domain events.

Typical events include:

```text
RegistrationCompleted

EmailVerified

PlanSelected

TrialStarted

OnboardingStarted

RestaurantConfigured

PaymentGatewayConfigured

DomainConfigured

ThemeConfigured

ProvisioningRequested

RestaurantLaunched
```

Business events integrate with the shared Event Bus.

---

# Consumed Business Events

The Self-Service Platform consumes selected platform events.

Examples include:

```text
TenantProvisioned

SubscriptionActivated

SubscriptionFailed

FeatureConfigurationAssigned

RestaurantProvisioned

LaunchCompleted
```

Consumed events synchronize onboarding progress with HQ Platform operations.

---

# Progress Tracking

The onboarding engine shall continuously track:

- Current Step
- Completed Steps
- Pending Steps
- Failed Steps
- Completion Percentage
- Last Activity Timestamp

Progress tracking shall support automatic resume functionality.

---

# Cache Strategy

Frequently accessed onboarding information may be cached.

Recommended cache targets:

- Onboarding Progress
- Registration State
- Verification State
- Selected Plan
- Configuration Summary

Cache invalidation shall occur immediately after successful state transitions.

---

# Transaction Boundaries

The following operations shall execute atomically:

- Registration Completion
- Email Verification
- Plan Selection
- Trial Activation
- Tenant Provisioning
- Launch Completion

Partial onboarding transactions shall never become visible to customers.

---

# Concurrency

Concurrent onboarding operations shall be controlled.

Examples include:

- Duplicate registration attempts
- Multiple verification requests
- Simultaneous onboarding sessions
- Duplicate provisioning requests

Optimistic locking and idempotency are recommended.

---

# Error Handling

The Self-Service Platform shall expose standardized business errors.

Examples include:

| Error | Description |
|--------|-------------|
| Registration Failed | Customer registration unsuccessful |
| Verification Failed | Email verification unsuccessful |
| Invalid Plan Selection | Selected plan unavailable |
| Configuration Invalid | Required configuration missing |
| Provisioning Failed | Tenant provisioning unsuccessful |
| Launch Validation Failed | Restaurant not ready for launch |
| Duplicate Provisioning Request | Provisioning already in progress |
| Unauthorized Onboarding Access | Permission denied |

Errors shall conform to the platform Error Code Catalog.

---

# Performance Targets

Target performance objectives:

| Operation | Target |
|-----------|---------|
| Registration | < 1 second |
| Email Verification | < 500 ms |
| Plan Selection | < 300 ms |
| Configuration Save | < 500 ms |
| Progress Retrieval | < 200 ms |
| Tenant Provisioning Request | < 2 seconds |
| Launch Validation | < 1 second |

Performance shall be monitored continuously.

---

# Security Guidelines

The Self-Service Platform shall enforce:

- Authentication
- Authorization
- Session Protection
- Tenant Isolation
- Configuration Validation
- Audit Logging

Sensitive customer information shall be protected throughout onboarding.

---

# Observability

Operational metrics shall include:

- Registration Rate
- Verification Rate
- Trial Conversion Rate
- Wizard Completion Rate
- Provisioning Duration
- Launch Success Rate
- Abandoned Sessions
- Average Onboarding Time

Metrics integrate with the Monitoring specification.

---

# Logging

The Self-Service Platform shall log:

- Registration Events
- Verification Events
- Plan Selection
- Configuration Updates
- Provisioning Requests
- Launch Events
- Business Exceptions

Sensitive credentials and verification secrets shall never be written to application logs.

---

# Testing Requirements

The module shall include:

## Unit Tests

- Registration validation
- Verification validation
- Plan selection validation
- Progress tracking
- Launch validation

---

## Integration Tests

- HQ Platform integration
- Authentication integration
- Notification integration
- Event publishing
- Event consumption
- Repository operations

---

## End-to-End Tests

- Customer Registration
- Email Verification
- Plan Selection
- Trial Activation
- Complete Onboarding Wizard
- Restaurant Configuration
- Tenant Provisioning
- Restaurant Launch

End-to-end tests shall validate the complete onboarding lifecycle.

---

# Future Compatibility

The architecture shall remain compatible with:

- AI Onboarding Assistant
- Enterprise Organization Onboarding
- Franchise Provisioning
- Multi-Restaurant Onboarding
- Zero-Touch Deployment
- Automated Compliance Validation
- Marketplace Integrations
- Future HQ Platform enhancements

Future capabilities shall extend existing services without replacing the core Self-Service Platform architecture.

---

# Compliance Checklist

Before the Self-Service Platform is considered production-ready, the following requirements shall be satisfied.

| Requirement | Status |
|------------|--------|
| Registration Flow | Required |
| Email Verification | Required |
| Plan Selection | Required |
| Trial Management | Required |
| Onboarding Wizard | Required |
| Restaurant Configuration | Required |
| Tenant Provisioning Integration | Required |
| Launch Workflow | Required |
| Progress Tracking | Required |
| Tenant Isolation | Required |
| Authorization | Required |
| Audit Logging | Required |
| Monitoring | Required |
| Automated Testing | Required |

---
