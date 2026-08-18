# Authentication Sequence

```mermaid
sequenceDiagram

    actor User
    participant Client as Application
    participant Identity as Identity Service
    participant DB as Identity Database
    participant Audit as Audit Service

    User->>Client: Enter credentials
    Client->>Identity: POST /api/v1/auth/login
    Identity->>DB: Find user
    DB-->>Identity: User record
    Identity->>Identity: Verify credentials
    Identity->>Identity: Create session/token
    Identity->>Audit: Record authentication event
    Audit-->>Identity: Accepted
    Identity-->>Client: Authentication response
    Client-->>User: Authenticated session
```

## Rules

- Identity Service owns authentication.
- Applications do not validate passwords themselves.
- Authentication events may be audited.
- Tokens/session information must not expose sensitive credentials.