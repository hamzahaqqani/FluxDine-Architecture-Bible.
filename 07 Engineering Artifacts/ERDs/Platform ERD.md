# Platform ERD

## Purpose

This ERD represents the platform-level entities responsible for identity, tenants, restaurant ownership, domains, themes, subscriptions, feature flags, and platform administration.

The diagram represents logical ownership rather than a single physical database.

FluxDine follows the Database-per-Service architecture. Each service owns its own database.

```mermaid
erDiagram

    USERS {
        uuid id PK
        string email
        string password_hash
        string status
        datetime created_at
        datetime updated_at
    }

    SESSIONS {
        uuid id PK
        uuid user_id FK
        datetime expires_at
        datetime created_at
    }

    TENANTS {
        uuid id PK
        string name
        string status
        datetime created_at
        datetime updated_at
    }

    TENANT_MEMBERS {
        uuid id PK
        uuid tenant_id FK
        uuid user_id FK
        string role
        string status
        datetime created_at
    }

    RESTAURANTS {
        uuid id PK
        uuid tenant_id FK
        string name
        string status
        datetime created_at
        datetime updated_at
    }

    DOMAINS {
        uuid id PK
        uuid tenant_id FK
        uuid restaurant_id FK
        string domain
        string status
        boolean verified
        datetime created_at
        datetime updated_at
    }

    THEMES {
        uuid id PK
        uuid restaurant_id FK
        string name
        string status
        string version
        datetime created_at
        datetime updated_at
    }

    SUBSCRIPTIONS {
        uuid id PK
        uuid tenant_id FK
        string plan_id
        string status
        datetime trial_start
        datetime trial_end
        datetime current_period_start
        datetime current_period_end
        datetime created_at
        datetime updated_at
    }

    FEATURE_FLAGS {
        uuid id PK
        string key
        string status
        string scope
        datetime created_at
        datetime updated_at
    }

    AUDIT_EVENTS {
        uuid id PK
        uuid tenant_id FK
        uuid actor_id FK
        string action
        string resource_type
        uuid resource_id
        datetime created_at
    }

    USERS ||--o{ SESSIONS : has
    USERS ||--o{ TENANT_MEMBERS : belongs_to
    TENANTS ||--o{ TENANT_MEMBERS : contains
    TENANTS ||--o{ RESTAURANTS : owns
    TENANTS ||--o{ DOMAINS : configures
    RESTAURANTS ||--o{ DOMAINS : uses
    RESTAURANTS ||--o{ THEMES : uses
    TENANTS ||--o{ SUBSCRIPTIONS : has
    TENANTS ||--o{ AUDIT_EVENTS : generates
    USERS ||--o{ AUDIT_EVENTS : performs
```

## Ownership Boundaries

| Entity | Owning Service |
|---|---|
| Users | Identity Service |
| Sessions | Identity Service |
| Tenants | Tenant Service |
| Tenant Members | Tenant Service |
| Restaurants | Restaurant Service |
| Domains | Domain Service |
| Themes | Theme Service |
| Subscriptions | Billing Service |
| Feature Flags | Feature Flag Service |
| Audit Events | Audit Service |

These relationships represent logical relationships only. Services shall not directly access another service's database.