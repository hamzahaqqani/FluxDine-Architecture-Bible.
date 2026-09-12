Document 04
System Architecture Blueprint

Version: 1.1

Status: ✅ LOCKED

Purpose

This document defines the complete architecture of the FluxDine ecosystem.

It specifies:

System boundaries
Applications
Shared platform services
Communication patterns
Authentication
Tenant lifecycle
Request flow
Deployment model
External integrations

This document serves as the master blueprint for all future development.

Current vs future (Initial Production)

Current Initial Production is a Next.js application on Vercel (`fluxdine-staging` occupies the Initial Production role), Turso Shared Database / Shared Schema, Cloudflare R2 for application files, Resend via Email Service, Sentry, Cloudflare DNS, and GitHub source control.

Shared Platform Services below are **logical** application boundaries. They are not independently deployed microservices with separate databases, worker fleets, or Kubernetes in Initial Production.

PostgreSQL, database-per-service, dedicated queues/workers/cache, Kubernetes, automatic custom-domain/DNS/SSL provisioning, and multi-region infrastructure remain **future** unless a later accepted ADR changes that.

Database backup and recovery follow ADR-055 (Turso PITR + GitHub Actions logical dumps to a dedicated private R2 backup bucket). Vercel Cron is for application scheduled jobs, not database backups.

1. High-Level Architecture
                    Internet
                        │
                        ▼
                ┌────────────────┐
                │ Cloudflare DNS │
                └────────────────┘
                        │
        ┌───────────────┼────────────────┐
        │               │                │
        ▼               ▼                ▼

signup.fluxdine.com  app.fluxdine.com   {restaurant}.fluxdine.com
(Self-Service)         (HQ Portal)      (Restaurant platform)

Canonical brand: fluxdine.com
fluxdine.online redirects to fluxdine.com
Custom restaurant domains: architecture-supported; automation deferred

Cloudflare is DNS only. The application resolves hostname → restaurant → tenant.

        │               │                │
        └───────────────┼────────────────┘
                        │
                        ▼

            FluxDine Platform Services

                        │

 ┌──────────────────────────────────────────────┐
 │                                              │
 │ Authentication Service                       │
 │ Billing Service                              │
 │ Payment Service                              │
 │ Notification Service                         │
 │ Domain Service                               │
 │ Theme Service                                │
 │ Analytics Service                            │
 │ Tenant Service                               │
 │ Feature Flag Service                         │
 │ File Storage Service                         │
 │ Audit Service                                │
 │ Email Service                                │
 │ Logging Service                              │
 │ Monitoring Service                           │
 │                                              │
 └──────────────────────────────────────────────┘

                        │

                        ▼

              Multi-Tenant Database
              (Turso, Shared Schema)
2. Platform Applications

FluxDine consists of three logical applications that share a common platform. In Initial Production they are delivered as one Vercel-hosted application, not as separately deployed infrastructure per application.

Application 1
FluxDine HQ

Domain

app.fluxdine.com
Users
FluxDine Owner
Super Admin
Support Team
Sales Team
Finance Team
Developers (future)
Customer Success
Responsibilities
Restaurant Management
Subscription Management
Billing
Tenant Provisioning
Platform Analytics
Support
Feature Management
Monitoring
Audit Logs
Platform Settings
Application 2
Self-Service Portal

Domain

signup.fluxdine.com
Responsibilities

Marketing Website

Restaurant Signup

Pricing

Plans

Documentation

Restaurant Onboarding

Payment

Launch Wizard

Knowledge Base

Application 3
Restaurant Platform

Domains

{restaurant}.fluxdine.com

Custom restaurant domains remain architecture-supported; automation is deferred.

Users

Customers
Head Admin
Branch Admin
Rider

Responsibilities

Customer Ordering

Restaurant Operations

Branch Operations

Delivery

Reservations

Marketing

Reports

Restaurant Configuration

3. Shared Platform Services

Every application communicates with shared services rather than implementing duplicated logic.

Authentication Service

Responsibilities

Login
Logout
Registration
Password Reset
Session Management
JWT / Cookies
Role Authorization
Tenant Authorization
Tenant Service

Responsibilities

Create Tenant
Suspend Tenant
Activate Tenant
Delete Tenant
Tenant Configuration
Tenant Metadata
Billing Service

Responsibilities

Plans
Trials
Subscriptions
Renewals
Invoices
Coupons
Taxes
Payment Service

Responsibilities

Payment Gateway Configuration
Payment Authorization
Payment Capture
Refunds
Webhooks
Payment History

The Payment Service communicates with supported providers through adapters rather than directly from the order module.

Supported Providers (Version 1)
Stripe
PayPal

Future providers can be added without modifying order-processing logic.

Notification Service

Responsibilities

Email
SMS
Push Notifications
WhatsApp (future)
Domain Service

Responsibilities

Domain configuration and mapping
Hostname → restaurant → tenant resolution metadata
Custom domain records (architecture-supported; provisioning automation deferred)
DNS validation guidance (Cloudflare remains DNS host)
SSL metadata where applicable (automatic SSL/custom-domain provisioning is not current Initial Production)
Theme Service

Responsibilities

Colors
Typography
Layout
Homepage Sections
Logos
Branding
Analytics Service

Responsibilities

Restaurant Analytics

Platform Analytics

Business Intelligence

Feature Flag Service

Responsibilities

Enable or disable features based on:

Subscription
Beta Access
Country
Restaurant Type
File Storage Service

Responsibilities

Store:

Logos
Images
Documents
Theme Assets
Audit Service

Responsibilities

Track:

User Actions
Security Events
Configuration Changes
Administrative Actions
Email Service

Responsibilities

Transactional Emails
Marketing Emails
Verification Emails
Password Reset Emails
Logging Service

Responsibilities

Centralized logs across all applications.

Monitoring Service

Responsibilities

Health Checks
Performance Metrics
Error Tracking
Alerting
4. Tenant Lifecycle
Restaurant visits FluxDine

        │

        ▼

Creates Account

        │

        ▼

Verifies Email

        │

        ▼

Chooses Plan

        │

        ▼

Payment

        │

        ▼

Tenant Created

        │

        ▼

Restaurant Configuration

        │

        ▼

Theme

        │

        ▼

Payment Gateway

        │

        ▼

Domain

        │

        ▼

Launch

        │

        ▼

Restaurant Live
5. Authentication Flow
User

↓

Login

↓

Authentication Service

↓

Validate Credentials

↓

Identify Tenant

↓

Identify Role

↓

Generate Session

↓

Redirect

↓

Customer

OR

Head Admin

OR

Branch Admin

OR

Rider

OR

HQ Staff
6. Request Flow

Example

Customer places an order.

Customer

↓

Restaurant Platform

↓

Authentication

↓

Tenant Service

↓

Order Service

↓

Payment Service

↓

Payment gateway adapters (Stripe / PayPal when configured)

↓

Shared Turso database (shared schema)

↓

Notification Service

↓

Customer Confirmation

Notice that the Restaurant Platform never communicates directly with Stripe or PayPal. It only interacts with the Payment Service.

7. Integration Architecture
External Services
Payment Providers
Stripe
PayPal
Email

Current provider:

Resend (via Email Service)

Future providers may include:

SendGrid
Amazon SES
SMS

Future:

Twilio
File Storage

Current provider:

Cloudflare R2 (application File Storage bucket, separate from ADR-055 database backup storage)

Future providers may include:

AWS S3
Maps

Future:

Google Maps
Analytics

Future:

PostHog
Google Analytics
8. Deployment Architecture
Vercel

│

├── FluxDine HQ

├── Self-Service Portal

├── Restaurant Platform

└── Shared APIs

Database

↓

Turso

Future migration to PostgreSQL remains possible because Drizzle ORM abstracts most database interactions.

9. Security Architecture

Security principles:

HTTPS Everywhere
Tenant Isolation
Role-Based Authorization
Principle of Least Privilege
Encrypted Secrets
Secure Cookies
CSRF Protection
Rate Limiting
Audit Logging
10. Scalability Strategy

The platform must support:

1 Platform
10,000+ Restaurants
Millions of Customers
Millions of Orders

without architectural redesign.

Scalability will be achieved through:

Managed Vercel application execution
Logical shared platform services (not separately deployed microservice infrastructure in Initial Production)
Database indexing
Caching (future)
Application scheduled jobs (current: Vercel Cron)
Dedicated queues/workers (future, if justified)
Horizontal scaling of the managed application platform
11. Architectural Decisions Register
ID	Decision	Status
AD-001	Unified Identity System	✅ Locked
AD-002	Payment Gateway Abstraction Layer (Stripe + PayPal v1)	✅ Locked
AD-003	Three-Application Platform (HQ, Self-Service, Restaurant Platform)	✅ Locked
AD-004	Shared Platform Services Architecture	✅ Locked
AD-005	One Restaurant = One Tenant	✅ Locked
AD-006	Configuration over Custom Code	✅ Locked
AD-007	Restaurant Platform is the Core Engine	✅ Locked
12. Final Conclusion

The System Architecture establishes FluxDine as a modular, service-oriented SaaS platform built around a single multi-tenant core. Rather than duplicating applications for each customer, every restaurant operates on the same platform while remaining logically isolated through tenant-aware services and data. The separation between applications (HQ, Self-Service, Restaurant Platform) and shared services is a **logical** boundary. Initial Production does not deploy each service as independent infrastructure with its own database. The platform can still evolve independently at the module/service-contract level, integrate additional providers, and scale on managed infrastructure without requiring a premature microservice, Kubernetes, or database-per-service topology.