# Payment Flow

```mermaid
sequenceDiagram

    participant Consumer as Commerce/Billing
    participant Payment as Payment Service
    participant Gateway as Gateway Abstraction
    participant Provider as Payment Provider
    participant Audit as Audit Service

    Consumer->>Payment: Create payment request
    Payment->>Payment: Validate request
    Payment->>Payment: Check idempotency

    Payment->>Gateway: Execute payment
    Gateway->>Provider: Provider request
    Provider-->>Gateway: Provider response
    Gateway-->>Payment: Normalized result

    Payment->>Audit: Record payment event
    Audit-->>Payment: Recorded

    Payment-->>Consumer: Payment result
```

## Architectural Boundary

```text
Commerce / Billing
        ↓
Payment Service
        ↓
Payment Gateway Abstraction
        ↓
Payment Provider
```

Consumers never communicate directly with payment providers.