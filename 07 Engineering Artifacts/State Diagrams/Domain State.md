# Domain State Diagram

```mermaid
stateDiagram-v2

    [*] --> Pending

    Pending --> VerificationRequired
    VerificationRequired --> Verified
    VerificationRequired --> Failed

    Verified --> Active
    Active --> Suspended
    Suspended --> Active

    Failed --> VerificationRequired

    Active --> [*]
```