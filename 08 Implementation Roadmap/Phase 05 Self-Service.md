# 08 Implementation Roadmap

# Phase 05 — Self-Service

---

# Objective

Implement the complete self-service restaurant onboarding and launch lifecycle.

---

# Customer Lifecycle

```text
Registration
    ↓
Email Verification
    ↓
Plan Selection
    ↓
Trial / Subscription
    ↓
Onboarding
    ↓
Restaurant Configuration
    ↓
Payment Gateway Configuration
    ↓
Domain Configuration
    ↓
Theme Configuration
    ↓
Launch Review
    ↓
Restaurant Launch
```

---

# Registration

Implement:

- Account creation
- Validation
- Identity creation
- Verification email

---

# Email Verification

Integrate:

- Identity Service
- Notification Service
- Email Service

Verification must be completed before protected onboarding operations.

---

# Plan Selection

Implement:

- Plan selection interface
- Selection persistence
- Billing initialization
- Validation

Commercial plan names, pricing, limits, and differentiation remain outside this roadmap until defined by the relevant subscription documentation.

---

# Trial Management

Implement:

- Trial initialization
- Trial state
- Trial expiration
- Trial conversion
- Trial-related notifications

---

# Onboarding Wizard

Implement persistent onboarding progress.

Required stages include:

- Restaurant information
- Branch information
- Menu/configuration
- Payment gateway configuration
- Domain
- Theme
- Review

---

# Payment Gateway Configuration

Integrate Payment Service.

Requirements:

- Secure credential handling
- Provider validation
- Configuration status
- Gateway activation
- Audit logging

---

# Domain Configuration

Integrate Domain Service.

Requirements:

- Domain entry
- Verification
- Activation
- Status tracking
- Failure handling

---

# Theme Configuration

Integrate Theme Service.

Requirements:

- Theme selection
- Branding
- Preview
- Configuration
- Publishing

---

# Launch Validation

Before launch, verify:

- Identity verified
- Tenant valid
- Restaurant configured
- Subscription state valid
- Payment configuration valid where required
- Domain requirements satisfied
- Theme published
- Required configuration complete

---

# Launch

The launch workflow shall:

1. Validate prerequisites.
2. Confirm configuration.
3. Record launch event.
4. Publish required configuration.
5. Activate customer-facing experience.
6. Notify appropriate parties.

---

# Testing

Required:

- Registration E2E
- Verification E2E
- Plan selection tests
- Trial tests
- Onboarding recovery tests
- Payment configuration tests
- Domain verification tests
- Theme publishing tests
- Launch validation tests
- Complete launch E2E

---

# Acceptance Criteria

A restaurant can progress from registration through launch without requiring manual database intervention.

Incomplete configuration prevents launch.

---

# Dependencies

- Phase 02 — SaaS Core
- Phase 04 — Restaurant Platform
- Phase 06 — Shared Services