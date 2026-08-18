# Commerce ERD

## Purpose

This ERD represents the core customer commerce domain.

Commerce owns carts, orders, order items, and order lifecycle information.

Payment execution remains owned by the Payment Service.

```mermaid
erDiagram

    CUSTOMERS {
        uuid id PK
        uuid tenant_id FK
        string name
        string email
        string phone
        datetime created_at
        datetime updated_at
    }

    CARTS {
        uuid id PK
        uuid tenant_id FK
        uuid customer_id FK
        string status
        datetime created_at
        datetime updated_at
    }

    CART_ITEMS {
        uuid id PK
        uuid cart_id FK
        uuid menu_item_id
        integer quantity
        decimal unit_price
        decimal subtotal
        datetime created_at
    }

    ORDERS {
        uuid id PK
        uuid tenant_id FK
        uuid customer_id FK
        string order_number
        string status
        decimal subtotal
        decimal tax
        decimal delivery_fee
        decimal discount
        decimal total
        datetime created_at
        datetime updated_at
    }

    ORDER_ITEMS {
        uuid id PK
        uuid order_id FK
        uuid menu_item_id
        string item_name
        integer quantity
        decimal unit_price
        decimal subtotal
        datetime created_at
    }

    ORDER_STATUS_HISTORY {
        uuid id PK
        uuid order_id FK
        string status
        datetime created_at
    }

    CUSTOMERS ||--o{ CARTS : owns
    CARTS ||--o{ CART_ITEMS : contains
    CUSTOMERS ||--o{ ORDERS : places
    ORDERS ||--o{ ORDER_ITEMS : contains
    ORDERS ||--o{ ORDER_STATUS_HISTORY : tracks
```

## Ownership

The Commerce Service owns:

- Carts
- Cart Items
- Orders
- Order Items
- Order Status
- Order Status History
- Commerce calculations

Payment transactions are not stored as Commerce-owned authoritative records.