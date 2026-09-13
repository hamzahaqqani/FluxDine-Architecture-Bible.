# 02 Engineering Specifications

# Backend

# 09 — Payment Gateway Specification

---

# Document Control

| Field | Value |
|--------|-------|
| **Document ID** | FD-ENG-BE-009 |
| **Document Name** | Payment Gateway Specification |
| **Version** | 1.0 |
| **Status** | Proposed |
| **Owner** | FluxDine Engineering |
| **Classification** | Internal Engineering Specification |
| **Depends On** | Payment Service<br>Payment Framework<br>ADR-018 Payment Gateway Abstraction<br>ADR-037 Restaurant Payment Gateway Configuration |
| **Referenced By** | Payment Service<br>Commerce Service<br>Billing Service<br>Self-Service Payment Gateway Configuration<br>Phase 07 Infrastructure<br>Phase 08 Production |

---

# Dependencies

This specification depends upon:

- Payment Service (FD-SPS-007)
- Payment Framework (FD-PM-RP-015)
- ADR-018 — Payment Gateway Abstraction
- ADR-037 — Restaurant Payment Gateway Configuration
- REST API Specification
- Event Catalog
- Environment Variables
- Security Architecture

This document does **not** replace ADR-018. ADR-018 remains the architectural decision that payment providers are accessed through a shared abstraction and that the Payment Service remains gateway-agnostic.

---

# Referenced By

This specification is referenced by:

- Payment Service
- Commerce Service
- Billing Service
- Restaurant Platform Payment Framework
- Self-Service Payment Gateway Configuration
- Phase 07 Infrastructure
- Phase 08 Production

---

# Document Status

| Item | Value |
|------|-------|
| Status | Proposed |
| Approval | Pending human review |
| Implementation | Architecture defined; application alignment deferred until this document is approved |
| Last Updated | 2026-09-13 |

This document becomes Approved and Locked only after human review. It is not automatically locked.

---

# Purpose

This document is the authoritative engineering specification for FluxDine payment **gateway implementations**.

It formally establishes **Demo Payment Gateway** as the current concrete gateway behind the existing Payment Gateway Abstraction for the Phase 07–08 testing cycle.

It distinguishes:

| Term | Classification |
|------|----------------|
| Demo Payment Gateway / Demo Payments | **CURRENT** — Phase 07–08 restaurant commerce testing only |
| Stripe Test Mode | **FUTURE** — Stripe-specific testing after Phase 08 |
| Stripe Connect | **FUTURE** — after Phase 08 |
| Live restaurant payment providers (including PayPal) | **FUTURE** |
| Commercial FluxDine SaaS subscription billing | **FUTURE** / deferred |

Demo Payment Gateway is **not** a live payment provider. It performs no real financial transactions.

---

# Scope

This specification defines:

- Current vs future payment gateway model
- Demo Payment Gateway identity
- Provider selection
- Demo Transaction persistence (existing tables)
- Payment lifecycle classification
- Deterministic Demo scenarios (contract, not a final HTTP API)
- Refunds, cancellation, webhooks/events, idempotency
- Tenant and restaurant isolation
- Security and observability for Demo Payments
- Commerce vs SaaS billing boundary
- Stable contract for a future Stripe Connect adapter
- Mapping from the current `TestPaymentProvider` implementation
- Testing requirements and phase scheduling

Physical persistence remains:

```text
Turso → Shared Database → Shared Schema
```

---

# Non-Goals

This specification does **not**:

- Redesign Payment Service
- Create a second payment abstraction or a second Payment Service
- Implement Stripe Connect, Stripe Test Mode, or any live processor
- Design Connect onboarding, destination charges, transfers, application fees, connected-account schema, Stripe webhooks, or Stripe secrets
- Activate commercial SaaS subscription billing
- Introduce PostgreSQL, Database-per-Service, new payment tables, queues, or infrastructure
- Change application code, SQL, migrations, or environment values
- Authorize real cards, bank accounts, or money movement

---

# Payment Architecture

**CURRENT (Phase 07–08 restaurant commerce):**

```text
Restaurant Customer
        ↓
FluxDine Ordering
        ↓
Payment Service
        ↓
Payment Gateway Abstraction (PaymentProvider)
        ↓
Demo Payment Gateway
        ↓
Demo Transaction
```

**FUTURE (after Phase 08):**

```text
Restaurant Customer
        ↓
FluxDine Ordering
        ↓
Payment Service
        ↓
Payment Gateway Abstraction (PaymentProvider)
        ↓
Stripe Connect Gateway
        ↓
Restaurant Connected Account
```

Restaurant checkout and order code shall call Payment Service only. They shall never call Stripe, PayPal, or any provider SDK.

---

# Payment Service Boundary

The Payment Service:

- Owns payment orchestration, lifecycle, authorization/capture/refund rules, transaction records, and payment domain events
- Remains gateway-agnostic
- Must not contain provider SDKs or provider-specific business objects

Gateway implementations belong behind the Payment Gateway Abstraction (Payment Framework / this specification). Logical payment data persists in the Turso Shared Database / Shared Schema. Logical ownership is not a separate physical database.

Commerce initiates payment. Payment Service executes payment.

---

# Payment Gateway Abstraction

ADR-018 remains authoritative:

Payment providers shall be accessed through a shared Payment Gateway Abstraction. The Payment Service shall remain gateway-agnostic.

The current application interface is `PaymentProvider`:

- `createIntent`
- `capture`
- `refund`
- `verifyWebhook`

Demo Payment Gateway is the **current** implementation of that interface. A future Stripe Connect Gateway shall implement the **same** interface (extended only if a later accepted architecture requires it). Do not introduce a parallel abstraction.

---

# Demo Payment Gateway

**Architectural identity:** Demo Payment Gateway

**Meaning:** The current non-production payment gateway implementation behind `PaymentProvider`. It simulates payment outcomes for application and end-to-end testing.

Demo Payment Gateway:

- performs no real financial transactions
- moves no real money
- uses no real payment processor
- uses no real cards, PANs, CVVs, or bank accounts
- uses no Stripe account
- requires no restaurant Stripe credentials
- does not use Stripe Test Mode or Stripe test cards
- does not communicate with Stripe
- exists only for application and end-to-end testing in Phase 07–08

**Implementation naming (do not treat as a second gateway):**

| Layer | Value |
|-------|--------|
| Architectural provider | Demo Payment Gateway |
| Current implementation class | `TestPaymentProvider` |
| Current implementation identifier (`PaymentProvider.name` / `FLUXDINE_PAYMENT_PROVIDER` default) | `test` |
| Target implementation identifier | `demo` |

`TestPaymentProvider` is the existing implementation that shall be aligned to this contract after this specification is approved. It is not a throwaway special case and must not remain a unit-test-only double as the product Demo Gateway.

---

# Current vs Future Provider Model

| Provider | Status |
|----------|--------|
| Demo Payment Gateway | **CURRENT** for Phase 07–08 commerce testing |
| Stripe Connect Gateway | **FUTURE** — after Phase 08 |
| Stripe Test Mode | **FUTURE** — Stripe-specific testing only, not a substitute for Demo Payments |
| PayPal and other live gateways | **FUTURE** |
| Historical “Sandbox Adapter” / “Stripe + PayPal v1 as current” language | **HISTORICAL** naming in older chapters; superseded for current-state by this document |

Chapter 20 and related documents previously listed Stripe and PayPal as Version 1 providers. That is **not** the current Initial Production / Phase 07–08 payment mode.

---

# Configuration and Provider Selection

Selection uses the environment variable:

```text
FLUXDINE_PAYMENT_PROVIDER
```

| Role | Value |
|------|--------|
| Current code default | `test` (unset → `TestPaymentProvider`) |
| Target architectural default for Phase 07–08 | `demo` |
| Stripe | Inactive. Setting `stripe` currently throws; Stripe remains deferred |

Phase 07–08 restaurants do **not** need payment-provider secrets. Self-Service “payment gateway configuration” for this cycle records Demo as the gateway (no credentials, no connectivity to an external processor).

Demo Payments can be enabled without any external provider credentials. Do not introduce fake production credentials.

Restaurant-level gateway configuration (ADR-037) remains a configuration record. It does not authorize direct provider calls from restaurant modules.

---

# Demo Transaction Model

A **Demo Transaction** is an application payment transaction representing **simulated** payment behavior.

Use existing tables only:

- `payment_transactions`
- `payment_refunds`

Preserve:

- `tenantId` (required)
- `restaurantId` where the payment is restaurant commerce
- `provider`
- `providerReference`
- `idempotencyKey`
- payment purpose
- status / lifecycle
- refund rows related to the transaction

No new tables.

**Synthetic identifiers**

Identifiers are synthetic and must never be treated as live processor IDs or proof of real settlement.

| Kind | Current implementation | Authoritative Demo target |
|------|------------------------|---------------------------|
| Payment reference | `test_pi_…`, `test_fail_…` | `demo_` prefix (for example `demo_pi_…`, `demo_fail_…`) |
| Refund reference | `test_re_…`, `test_re_fail` | `demo_re_…` |

Do not use Stripe-like live shapes such as production `pi_` / `ch_` identifiers without a Demo namespace.

Existing stored `test_*` values remain valid historical rows until application alignment migrates new writes to `demo_`. This specification does not change the schema.

---

# Payment Lifecycle

Architectural lifecycle:

create payment intent → authorization or failure → capture (if authorized) → completion (captured) → optional refund

Cancellation of an authorized-but-not-captured payment is part of the Payment Service API surface in the Architecture Bible.

**Pending (gateway-level delayed settlement):** **DEFERRED**. Phase 07–08 Demo contract does not require an asynchronous pending gateway state. Order rows may still use order-level `paymentStatus` values such as `pending` before a Payment Service call; that is order state, not Demo Gateway pending.

| Behavior | Classification |
|----------|----------------|
| Create payment intent | CURRENTLY SUPPORTED |
| Authorization / success (`authorized`) | CURRENTLY SUPPORTED |
| Capture success | CURRENTLY SUPPORTED |
| Failure on create (injected provider instance) | CURRENTLY SUPPORTED in unit tests only |
| Failure on capture / refund (injected instance) | CURRENTLY SUPPORTED in unit tests only |
| HTTP/deterministic Demo failure scenarios | REQUIRES CODE ALIGNMENT |
| Refund (full/partial after capture) | CURRENTLY SUPPORTED |
| Completion (`captured`, `refunded`, `partially_refunded`) | CURRENTLY SUPPORTED |
| Cancellation of authorized payment | REQUIRES CODE ALIGNMENT (`PaymentService` has no cancel method today) |
| Gateway-level pending / delayed outcome | DEFERRED |
| Stripe / live settlement | DEFERRED |

Do not claim HTTP Demo failure or cancel is implemented until alignment work is done.

---

# Deterministic Demo Scenarios

The Demo Gateway **product contract** must support deterministic, server-controlled scenarios:

- successful payment (authorize)
- failed payment
- successful capture
- failed capture
- successful refund
- failed refund

Cancellation: include in the alignment contract because Payment Service already lists Cancel Payment. Pending/delayed: **DEFERRED** (not in the Phase 07–08 Demo contract).

Rules:

- Server-controlled
- Deterministic
- Demo-only (must not exist on live provider adapters)
- Must not accept or process real payment credentials
- Must not use Stripe test cards

`failNextCreate` / `failNextCapture` / `failNextRefund` on a single in-memory `TestPaymentProvider` instance are **unit-test helpers**. They are **not** the complete Demo product contract. HTTP `new PaymentService()` does not reuse those flags.

The exact HTTP/API scenario mechanism (header, reserved Demo metadata, or server-only test control) is an **implementation requirement** after this specification is approved. This document does not invent a final public API.

---

# Refunds

Refunds are owned by Payment Service.

Current behavior: only `captured` or `partially_refunded` transactions may be refunded; amounts must be positive and not exceed the original; refund idempotency is tenant-scoped on `payment_refunds`.

Demo refunds are simulated. They do not move money.

---

# Cancellation

Payment Service documentation includes Cancel Payment.

Current application: **no** `cancel` on `PaymentService` or `PaymentProvider`.

Classification: **REQUIRES CODE ALIGNMENT** for Demo Gateway alignment after this spec is approved. Until then, decline/failure before capture is the supported negative path.

Gateway-level pending cancel of an in-flight processor authorization is **DEFERRED** with live providers.

---

# Webhook and Event Simulation

**Internal FluxDine domain events** (current): Payment Service emits events such as `payment.created`, `payment.authorized`, `payment.failed`, `payment.captured`, `refund.created`, `refund.completed`. These are platform events, not provider webhooks.

**External payment-provider webhooks:** used by future live/Connect adapters to settle state.

**Current test webhook:** `POST /api/v1/payments/webhooks/test` calls `PaymentService.handleWebhook` → `TestPaymentProvider.verifyWebhook`. Verification requires header `x-fluxdine-test-webhook: 1` and JSON with optional `eventType` and `providerReference`. It does **not** mutate `payment_transactions`.

Applying Demo webhook payloads to transaction state: **REQUIRES CODE ALIGNMENT** if Phase 07–08 tests need webhook-driven settlement. Until then, treat the route as signature/acceptance simulation only.

Future Stripe Connect webhooks may update payment state after Connect is introduced. That is not current.

---

# Idempotency

Do not redesign. Preserve Payment Service behavior:

- Create intent: unique `(tenantId, idempotencyKey)` on `payment_transactions`; replay returns the same transaction; different amount/restaurant with the same key is a conflict
- Refund: unique `(tenantId, idempotencyKey)` on `payment_refunds`; replay returns the existing refund
- Capture: already-captured is a replay
- Provider reference on first successful create is stored and reused on replay (no second provider create)
- Duplicate internal events may be emitted according to current service behavior; consumers must remain idempotent where required

---

# Tenant and Restaurant Isolation

Demo Payments do not create global cross-tenant payment state.

- Every commerce Demo Transaction requires `tenantId`
- Restaurant commerce should persist `restaurantId` when the payment belongs to a restaurant
- Capture, refund, and get must refuse cross-tenant access
- One tenant must not read or mutate another tenant’s Demo Transactions

Current application: `PaymentService` scopes get/capture/refund by `tenantId`. The v1 payments POST currently forces `restaurantId: null`; order creation sets `restaurantId` from server scope. Aligning the generic API to server-bound restaurant scope: **REQUIRES CODE ALIGNMENT**.

---

# Security Requirements

Demo Payments shall not use or store:

- Real card data, PAN, CVV
- Bank credentials
- Real payment-provider credentials
- Stripe credentials or secrets
- Any payload that would constitute a real financial instrument

No real financial transaction and no real money movement.

Demo identifiers must not be presented in UI, receipts, or logs as proof of live settlement.

PCI-sensitive data must not be collected for Demo Payments.

---

# Observability and Logging

Logs, events, and monitoring for Demo activity shall be labeled as simulated, for example:

- Demo Payment
- Simulated Transaction
- Demo Refund
- Demo Failure

Do not describe Demo captures as live settled funds.

Sentry and operational logs must not include PAN, secrets, or raw payment credentials. `sendDefaultPii=false` remains the Sentry posture.

Payment monitoring “connected-account failures” applies when Stripe Connect is active (**FUTURE**). Demo monitoring tracks Demo success/failure/idempotency only.

---

# Commerce Payments vs SaaS Billing

**CURRENT DEMO PAYMENT SCOPE:** Restaurant customer → restaurant commerce / order payments (`purpose` `commerce_order`).

**NOT CURRENT:** Commercial FluxDine SaaS subscription billing (restaurant owner paying FluxDine).

Billing Service remains the owner of subscription lifecycle. Payment Service remains the owner of payment **execution**.

The application may still call Payment Service with `purpose: billing_invoice` for **technical/internal** invoice records already in code. That capability does **not** activate commercial SaaS billing, Stripe Billing, or fake SaaS revenue.

Do not create fake SaaS revenue transactions to satisfy Demo Payments.

Commercial SaaS subscription billing remains **deferred**.

---

# Future Stripe Connect Compatibility

Stable contract (must not be broken by Demo):

- Commerce and restaurant modules call **Payment Service only**
- Gateways implement **PaymentProvider** (or the same abstraction)
- Generic fields: provider, providerReference, normalized status, amount, currency, purpose, tenant, restaurant, idempotency, refund

Commerce must **not** depend on:

- Stripe PaymentIntent
- Stripe Connected Account IDs
- Stripe application fees
- Stripe destination charges
- Stripe webhooks
- Stripe SDK objects

`clientSecret` on transactions is a generic optional provider payload, not a Stripe contract.

This document does **not** specify Connect onboarding, destination charges, transfers, application fees, connected-account databases, Stripe webhook implementation, or Stripe secrets. Those are after Phase 08.

---

# Application Implementation Alignment

Inspected application (not modified by this specification):

| Item | Location | Current state |
|------|----------|----------------|
| Abstraction | `remix-of-spicycrust-prototype/src/modules/payment/provider.ts` — `PaymentProvider` | createIntent, capture, refund, verifyWebhook |
| Demo/test class | `TestPaymentProvider` | `name = 'test'` |
| Selection | `resolvePaymentProvider()` | default `FLUXDINE_PAYMENT_PROVIDER` → `test`; `stripe` throws |
| Stripe class | `StripePaymentProvider` | throws `STRIPE_ADAPTER_INACTIVE`; comments still mention Stripe test credentials (**future**, not Demo) |
| Orchestration | `.../payment/service.ts` — `PaymentService` | persist + events + tenant checks |
| Tables | `src/db/schema.ts` — `paymentTransactions`, `paymentRefunds` | shared schema; `provider` default `test` |
| Commerce | `src/app/api/orders/route.ts` | createIntent + capture via Payment Service |
| Facade | `src/app/api/create-payment-intent/route.ts` | Payment Service; no Stripe import |
| API | `src/app/api/v1/payments/route.ts` and capture/refund routes | Payment Service |
| Webhook | `src/app/api/v1/payments/webhooks/test/route.ts` | verify only |
| Billing technical path | `src/modules/billing/service.ts` — `collectInvoice` | may call Payment Service; not commercial SaaS billing |
| Runtime Stripe imports | `src/` | none found |
| Packages | `package.json` | `stripe` / `@stripe/stripe-js` present; unused on runtime payment path |

After approval, implementation alignment is a **later** application task, not this documentation task.

---

# Testing Requirements

Demo Payment tests shall cover:

1. Successful order payment (authorize + capture)
2. Failed payment
3. Capture
4. Refund
5. Idempotent repeated payment
6. Idempotent repeated refund
7. Tenant isolation
8. Restaurant isolation
9. Invalid/unauthorized access
10. Demo/synthetic identifiers (not live Stripe IDs)
11. Webhook verification if the test route is used (verification only until alignment)
12. No Stripe/network invocation
13. No real transaction
14. Deterministic failure scenarios (after HTTP scenario alignment)

Do not require Stripe test cards or live credentials.

---

# Phase Scheduling

**Phase 07**

- Define Demo Gateway (this document)
- Align implementation to this contract (after approval)
- Use Demo Payments for restaurant commerce testing
- No Stripe Connect
- No Stripe Test Mode
- No live gateway
- No commercial SaaS subscription billing

**Phase 08**

- End-to-end multi-tenant SaaS validation using Demo Payments
- Payment / order / refund / failure testing
- Continue without Stripe Connect

**After Phase 08**

- Stripe Connect may be designed and implemented
- Stripe Test Mode may be used for Stripe-specific testing
- Live restaurant payment processing may be introduced after production readiness

---

# Implementation Boundaries

Allowed after this spec is approved (application work, not this commit):

- Rename/align `TestPaymentProvider` to Demo (`demo`, `demo_` prefixes)
- Server-controlled Demo scenarios
- Optional cancel + webhook settlement if required by the locked contract
- RestaurantId binding on generic payment APIs

Not allowed in Phase 07–08:

- Stripe Connect setup
- Stripe Test Mode as the Demo mechanism
- Live processors
- Commercial SaaS billing go-live
- New payment databases or schemas for “service databases”

---

# Mandatory Rules

1. Phase 07–08 restaurant commerce payments use Demo Payment Gateway only.
2. Payment Service is the only payment executor for application modules.
3. No second PaymentProvider hierarchy.
4. No real money, cards, banks, or Stripe communication for Demo.
5. Stripe Connect and Stripe Test Mode are future.
6. Commercial SaaS subscription billing is deferred.
7. Tenant isolation is mandatory.
8. Shared schema does not permit cross-tenant payment access.
9. ADR-018 is not rewritten.
10. This specification stays Proposed until human review.

---

# Appendices

## Appendix A — Classification legend

- **CURRENTLY SUPPORTED** — present in the inspected application
- **REQUIRES CODE ALIGNMENT** — required by this contract; not fully present in HTTP/runtime
- **DEFERRED** — not part of the Phase 07–08 Demo contract

## Appendix B — Historical names

“Sandbox Adapter” in Chapter 20 is a historical label for a non-live adapter. The authoritative current name is Demo Payment Gateway.

---

# References

- ADR-018 — Payment Gateway Abstraction
- ADR-037 — Restaurant Payment Gateway Configuration
- Payment Service (FD-SPS-007)
- Payment Framework (FD-PM-RP-015)
- Billing Service (FD-SPS-006)
- Commerce Service (FD-SPS-005)
- Phase 07 Infrastructure
- Phase 08 Production
- Environment Variables
- Monitoring

---

# Revision History

| Version | Date | Author | Description |
|----------|------|--------|-------------|
| 1.0 | 2026-09-13 | FluxDine Engineering | Proposed. Establishes Demo Payment Gateway as the current Phase 07–08 commerce testing gateway. Stripe Connect, Stripe Test Mode, live providers, and commercial SaaS billing remain future. |
