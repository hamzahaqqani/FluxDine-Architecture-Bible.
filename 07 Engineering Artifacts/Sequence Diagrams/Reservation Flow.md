# Reservation Flow

```mermaid
sequenceDiagram

    actor Customer
    participant Web as Customer Platform
    participant Restaurant as Restaurant Service
    participant DB as Restaurant Database
    participant Notification as Notification Service

    Customer->>Web: Select reservation
    Web->>Restaurant: Create reservation
    Restaurant->>DB: Validate availability
    DB-->>Restaurant: Availability result

    alt Available
        Restaurant->>DB: Create reservation
        Restaurant->>Notification: Send confirmation
        Notification-->>Customer: Confirmation
        Restaurant-->>Web: Reservation created
    else Unavailable
        Restaurant-->>Web: Availability failure
    end
```

## Lifecycle

```text
Pending
   ↓
Upcoming
   ↓
Active
   ↓
Fulfilled

Alternative:

Pending → Canceled
Upcoming → Canceled
Active → Canceled
```

Server-side scheduled jobs are authoritative for time-based transitions.