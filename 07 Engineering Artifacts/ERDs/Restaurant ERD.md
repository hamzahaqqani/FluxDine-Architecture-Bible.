# Restaurant ERD

## Purpose

This ERD represents restaurant operational data and menu management.

```mermaid
erDiagram

    RESTAURANTS {
        uuid id PK
        uuid tenant_id FK
        string name
        string status
        string phone
        string email
        datetime created_at
        datetime updated_at
    }

    BRANCHES {
        uuid id PK
        uuid restaurant_id FK
        string name
        string address
        string status
        datetime created_at
        datetime updated_at
    }

    MENUS {
        uuid id PK
        uuid restaurant_id FK
        string name
        string status
        datetime created_at
        datetime updated_at
    }

    CATEGORIES {
        uuid id PK
        uuid menu_id FK
        string name
        integer display_order
        datetime created_at
        datetime updated_at
    }

    MENU_ITEMS {
        uuid id PK
        uuid category_id FK
        string name
        string description
        decimal price
        string status
        datetime created_at
        datetime updated_at
    }

    RESERVATIONS {
        uuid id PK
        uuid restaurant_id FK
        uuid branch_id FK
        uuid customer_id FK
        datetime reservation_time
        integer party_size
        string status
        datetime created_at
        datetime updated_at
    }

    OFFERS {
        uuid id PK
        uuid restaurant_id FK
        string name
        string type
        decimal value
        string status
        datetime starts_at
        datetime ends_at
        datetime created_at
    }

    RESTAURANTS ||--o{ BRANCHES : has
    RESTAURANTS ||--o{ MENUS : owns
    MENUS ||--o{ CATEGORIES : contains
    CATEGORIES ||--o{ MENU_ITEMS : contains
    RESTAURANTS ||--o{ RESERVATIONS : receives
    BRANCHES ||--o{ RESERVATIONS : hosts
    RESTAURANTS ||--o{ OFFERS : publishes
```

## Ownership

Restaurant Service owns restaurant registry and operational restaurant configuration.

Commerce consumes menu information through approved service interfaces rather than directly accessing Restaurant Service tables.