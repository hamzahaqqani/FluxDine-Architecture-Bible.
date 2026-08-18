# Subscription State Diagram

```mermaid
stateDiagram-v2

    [*] --> Trialing

    Trialing --> Active : Trial converted
    Trialing --> Expired : Trial ends

    Active --> PastDue : Payment failure
    PastDue --> Active : Payment recovered
    PastDue --> Canceled : Grace period expires

    Active --> Canceled : Cancellation
    Expired --> Canceled

    Canceled --> [*]
```