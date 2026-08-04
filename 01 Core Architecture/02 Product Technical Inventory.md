Current Product Technical Inventory

Version: 1.0

Status: ✅ LOCKED (Baseline)

Purpose

This document serves as the official inventory of the existing FluxDine Restaurant Platform.

It answers four questions:

What already exists?
What works today?
What should never be rebuilt?
What will be transformed into SaaS?

This document becomes the baseline for every future architectural decision.

1. Technology Stack
Frontend
Next.js (App Router)
React
TypeScript
Tailwind CSS
Component-based architecture
Backend
Next.js API Routes
TypeScript
ORM
Drizzle ORM
Database

Current

Turso (SQLite)

Architecture Ready For

Multi-tenant Database
Deployment
Vercel
Authentication

Custom Role-Based Authentication

(Current implementation)

Future

Tenant-aware Authentication

2. High-Level Application Architecture

Current application consists of one integrated restaurant platform.

Restaurant Platform

│

├── Customer Portal

├── Authentication

├── Head Admin Dashboard

├── Branch Admin Dashboard

├── Rider Dashboard

└── REST API

Everything is currently built for a single restaurant tenant.

3. Existing Route Inventory
Public Routes

Confirmed from the codebase:

/

/menu

/branches

/login

/signup

/checkout

/order-confirmation

/track-order
Customer Routes
/user-dashboard
Restaurant Administration
/admin-dashboard

/admin
Rider
/rider-dashboard

/rider-dashboard/order-history
4. Current User Roles

Current implementation supports four distinct user groups.

Customer

Capabilities:

Register
Login
Browse menu
Place orders
Checkout
Order tracking
Reservations
User dashboard
Head Admin

Capabilities:

Restaurant dashboard
Menu management
Branch management
Orders
Riders
Reports
Settings
Branch Admin

Capabilities:

Branch-specific operations
Orders
Riders
Branch management

(Currently implemented through branch assignment.)

Rider

Capabilities:

View assigned deliveries
Update delivery status
View delivery history
5. Existing API Inventory

Current REST endpoints discovered:

Authentication
/api/auth/*
Users
/api/users
Admin
/api/admins

/api/admin-settings
Branches
/api/branches
Menu
/api/menu

/api/categories
Orders
/api/orders

/api/cart
Riders
/api/riders
Reservations
/api/reservations

/api/reservation-tables
Offers
/api/offers
Analytics
/api/analytics
Customers
/api/customers
Payments
/api/create-payment-intent
Email
/api/send-email

/api/email-settings

/api/email-logs
Newsletter
/api/newsletter
News
/api/news
6. Existing Database Inventory

Current schema already contains the following business entities:

Core Restaurant
Categories
Menu Items
Branches
Identity
Users
Admins
Riders
Commerce
Orders
Order Items
Cart
Cart Items
Reservations
Reservations
Reservation Tables
Marketing
Offers
News
Newsletter
Newsletter Subscribers
Configuration
Admin Settings
Email Settings
Email Logs
7. Existing Functional Modules
Customer Experience

Implemented

✅ Home

✅ Menu

✅ Branches

✅ Authentication

✅ Cart

✅ Checkout

✅ Orders

✅ Order Tracking

✅ Reservations

✅ User Dashboard

Restaurant Operations

Implemented

✅ Dashboard

✅ Orders

✅ Categories

✅ Menu

✅ Branches

✅ Riders

✅ Customers

✅ Reports

✅ Settings

Rider Operations

Implemented

✅ Assigned Orders

✅ Delivery Status

✅ Order History

Administration

Implemented

✅ Restaurant Configuration

✅ Email Configuration

✅ Admin Settings

Marketing

Implemented

✅ Offers

✅ News

✅ Newsletter

8. Current Strengths

The existing platform already provides:

Complete ordering workflow
Role-based authentication
Restaurant operations dashboard
Delivery workflow
Reservation management
Marketing tools
Email infrastructure
Modular API architecture
Modular component architecture
Clean database organization

This represents approximately 70–80% of the functionality required for an individual restaurant platform.

9. Current Architectural Limitation

The application was intentionally designed for one restaurant.

Evidence from the codebase includes:

No tenant_id or equivalent ownership model in core entities.
Single restaurant configuration assumptions.
Branches belong to one restaurant implicitly rather than explicitly.
Admins and riders are scoped by branch, not by tenant.
Global configuration tables (e.g., admin settings, email settings) are not tenant-aware.
API routes are not tenant-scoped.
Authentication is role-aware but not tenant-aware.

These are expected limitations and align with the application's original purpose.

10. Reusable Assets

The following components should be preserved and adapted rather than rewritten:

Customer Ordering Flow
Menu browsing
Cart
Checkout
Order confirmation
Order tracking
Restaurant Operations
Menu management
Order management
Branch management
Rider management
Reservation management
Shared Services
Email
Notifications
Authentication
Reporting
Settings
11. Components to Transform

The following areas require architectural evolution rather than replacement:

Current Component	Future Direction
Authentication	Tenant-aware authentication
Branches	Branches linked to Restaurant (Tenant)
Admins	Unified user model with roles and tenant scope
Riders	Tenant-scoped riders
Orders	Tenant-scoped orders
Menu	Tenant-scoped menus
Reservations	Tenant-scoped reservations
Settings	Per-tenant configuration
Email	Per-tenant email settings
Analytics	Tenant-specific analytics
Payment	Tenant-specific payment gateway configuration
12. Components Missing for SaaS

The following capabilities are not defects; they simply did not exist in the original single-restaurant design:

Platform Layer
FluxDine HQ
Tenant management
Restaurant provisioning
Subscription engine
Billing engine
Feature flag management
Domain management
Platform analytics
Customer Acquisition
Self-service signup
Onboarding wizard
Trial management
Plan selection
SaaS Infrastructure
Tenant engine
Theme engine
Usage limits
Custom domain provisioning
Automated restaurant creation
Multi-tenant configuration
13. Migration Strategy

The guiding principle for the SaaS transformation is:

Evolve, don't rebuild.

This means:

Preserve proven business logic.
Preserve UI components where possible.
Preserve API contracts where practical.
Extend the data model with tenant awareness.
Introduce new platform services around the existing application instead of replacing it wholesale.
14. Conclusion

The current application is not a prototype; it is a production-ready single-restaurant platform.

The SaaS transformation will focus on adding a platform layer (HQ, onboarding, subscriptions, tenant management, billing, domains, themes) and making existing modules tenant-aware, rather than rebuilding the restaurant engine itself.