# Audit Flow

```mermaid
sequenceDiagram

    participant Actor
    participant Service
    participant Audit as Audit Service
    participant AuditDB as Audit Database

    Actor->>Service: Perform business action
    Service->>Service: Authorize action
    Service->>Audit: Record audit event
    Audit->>AuditDB: Append event
    AuditDB-->>Audit: Stored
    Audit-->>Service: Accepted
    Service-->>Actor: Operation result
```

Audit events shall contain sufficient context for traceability without exposing secrets or sensitive credentials.