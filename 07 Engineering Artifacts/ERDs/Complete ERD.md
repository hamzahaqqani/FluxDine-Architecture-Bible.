# Complete FluxDine ERD

## Purpose

This document provides the high-level logical relationship model across the major FluxDine platform domains.

It is not a physical database diagram.

FluxDine uses Database-per-Service architecture, therefore the entities below represent service-owned domains and their logical relationships.

```mermaid
erDiagram

    USER {
        uuid id PK
        string email
        string status
    }

    TENANT {
        uuid id PK
        string name
        string status
    }

    TENANT_MEMBER {
        uuid id PK
        uuid tenant_id FK
        uuid user_id FK
        string role
    }

    RESTAURANT {
        uuid id PK
        uuid tenant_id FK
        string name
        string status
    }

    BRANCH {
        uuid id PK
        uuid restaurant_id FK
        string name
        string status
    }

    MENU {
        uuid id PK
        uuid restaurant_id FK
        string name
        string status
    }

    MENU_ITEM {
        uuid id PK
        uuid menu_id FK
        string name
        decimal price
        string status
    }

    CUSTOMER {
        uuid id PK
        uuid tenant_id FK
        string email
    }

    CART {
        uuid id PK
        uuid customer_id FK
        string status
    }

    ORDER {
        uuid id PK
        uuid customer_id FK
        uuid tenant_id FK
        string status
        decimal total
    }

    ORDER_ITEM {
        uuid id PK
        uuid order_id FK
        uuid menu_item_id
        integer quantity
        decimal unit_price
    }

    RESERVATION {
        uuid id PK
        uuid restaurant_id FK
        uuid branch_id FK
        uuid customer_id FK
        datetime reservation_time
        string status
    }

    SUBSCRIPTION {
        uuid id PK
        uuid tenant_id FK
        string plan_id
        string status
    }

    PAYMENT_TRANSACTION {
        uuid id PK
        uuid tenant_id FK
        string reference_type
        uuid reference_id
        decimal amount
        string status
    }

    DOMAIN {
        uuid id PK
        uuid restaurant_id FK
        string domain
        string status
    }

    THEME {
        uuid id PK
        uuid restaurant_id FK
        string version
        string status
    }

    USER ||--o{ TENANT_MEMBER : joins
    TENANT ||--o{ TENANT_MEMBER : contains
    TENANT ||--o{ RESTAURANT : owns
    RESTAURANT ||--o{ BRANCH : contains
    RESTAURANT ||--o{ MENU : owns
    MENU ||--o{ MENU_ITEM : contains
    TENANT ||--o{ CUSTOMER : contains
    CUSTOMER ||--o{ CART : owns
    CUSTOMER ||--o{ ORDER : places
    ORDER ||--o{ ORDER_ITEM : contains
    RESTAURANT ||--o{ RESERVATION : receives
    BRANCH ||--o{ RESERVATION : hosts
    CUSTOMER ||--o{ RESERVATION : makes
    TENANT ||--o{ SUBSCRIPTION : has
    TENANT ||--o{ PAYMENT_TRANSACTION : processes
    RESTAURANT ||--o{ DOMAIN : uses
    RESTAURANT ||--o{ THEME : uses
```

## Important Architectural Constraint

The diagram does **not** authorize cross-service database joins.

For example:

```text
Commerce Service
      X
Restaurant Database
```

Instead:

```text
Commerce Service
      ↓
Restaurant Service API / Event
```

The Complete ERD is a logical architecture artifact, not a shared database design.