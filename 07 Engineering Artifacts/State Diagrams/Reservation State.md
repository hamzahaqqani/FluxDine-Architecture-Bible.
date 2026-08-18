# Reservation State Diagram

```mermaid
stateDiagram-v2

    [*] --> Pending

    Pending --> Upcoming : Reservation day begins
    Upcoming --> Active : Reservation time reached
    Active --> Fulfilled : Completion threshold reached

    Pending --> Canceled
    Upcoming --> Canceled
    Active --> Canceled

    Canceled --> [*]
    Fulfilled --> [*]
```

Time-based transitions are controlled by server-side scheduled jobs.