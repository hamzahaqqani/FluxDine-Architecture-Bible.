# 00 Governance

# System Glossary

---

# Document Control

| Field | Value |
|---|---|
| Document ID | FD-GOV-007 |
| Document Name | System Glossary |
| Version | 1.1 |
| Status | Approved and Locked |
| Owner | FluxDine Architecture Team |
| Classification | Governance Reference |

---

# Purpose

This glossary defines the standard terminology used throughout the FluxDine Architecture Bible.

Terms should retain these meanings unless an ADR explicitly changes them.

---

# A

## Application

A user-facing software product or interface serving a specific actor or workflow.

Examples include HQ Platform, Restaurant Platform, and Customer Platform.

---

## Audit Event

An immutable or append-oriented record describing a significant business or security action.

---

# B

## Branch

A physical or operational location belonging to a restaurant.

---

## Billing

The platform capability responsible for subscription and recurring SaaS billing lifecycle.

---

# C

## Customer

The end user interacting with a restaurant's customer-facing experience.

---

## Commerce

The business domain responsible for shopping and transactional ordering workflows.

---

## Commerce Service

The Shared Platform Service responsible for carts, orders, order items, and commerce lifecycle.

---

# D

## Domain

A custom web hostname associated with a restaurant's digital experience.

---

## Domain Service

The service responsible for domain registration, verification, activation, and lifecycle.

---

# E

## Event

A record describing something that has occurred within the system.

---

## Event Bus

The infrastructure abstraction used to distribute asynchronous events between services.

---

# F

## Feature Flag

A controlled configuration mechanism for enabling or disabling platform capabilities.

---

## File Storage Service

The Shared Platform Service responsible for binary file and object storage.

---

# H

## HQ

The platform administration environment used by authorized FluxDine operators.

---

# I

## Identity

The representation of a platform user and their authentication-related information.

---

## Identity Service

The Shared Platform Service responsible for authentication and identity lifecycle.

---

# M

## Menu

A restaurant's organized collection of categories and menu items.

---

# O

## Order

A commerce transaction representing a customer's requested products or services.

---

# P

## Payment

A financial transaction processed through the Payment Service.

---

## Payment Gateway

A payment gateway implementation behind the Payment Gateway Abstraction.

Current Phase 07–08 commerce testing uses **Demo Payment Gateway**, which is an internal simulated gateway, not a live external processor.

Live external processors (Stripe Connect, PayPal, and others) are **future**.

---

## Demo Payment Gateway

The current non-production payment gateway implementation for Phase 07–08 restaurant commerce testing. It simulates payment outcomes. It is not Stripe Test Mode and not a live provider.

---

## Payment Gateway Abstraction

The internal interface separating Payment Service business logic from provider-specific implementations.

---

# R

## Restaurant

A business entity operating through the FluxDine platform.

---

## Restaurant Service

The Shared Platform Service responsible for restaurant identity and restaurant registry capabilities.

---

# S

## Service

A bounded software capability with defined ownership, responsibilities, interfaces, and persistence boundaries.

---

## Shared Platform Service

A reusable backend capability serving multiple FluxDine applications or domains.

---

## Subscription

The SaaS commercial lifecycle associated with a tenant.

---

# T

## Tenant

The top-level organizational boundary representing a customer organization within FluxDine.

Tenant isolation is a core architectural requirement.

---

## Tenant Member

A user associated with a tenant through an authorized role.

---

# T — Theme

## Theme

A restaurant's visual and branding configuration for its digital experience.

---

## Theme Service

The service responsible for theme configuration, versioning, preview, and publishing.

---

# U

## User

A person represented within the FluxDine Identity Service.

A user may belong to one or more authorized tenant contexts depending on the platform's authorization model.

---

# W

## Workflow

A defined sequence of states, actions, services, and decisions representing a business process.

---

# Standard Terminology Rule

Architecture documents should use the terminology defined in this glossary consistently.

Synonyms should not be introduced casually when they could create ambiguity.

---

# Glossary Governance

When a new architectural term becomes important:

1. Define it.
2. Add it to this glossary.
3. Use the approved term consistently.
4. Update affected documentation.

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.1 | 2026-09-13 | FluxDine Architecture Team | Demo Payment Gateway; Payment Gateway is not only an external live processor. |
