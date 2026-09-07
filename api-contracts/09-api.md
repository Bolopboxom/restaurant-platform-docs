# API Specification

## API Design Level
This document is the implementation-ready API design baseline for MVP.

## Global Conventions

### Base URL and Versioning
- Base URL: `/api/v1`
- Major breaking changes must use a new prefix (example: `/api/v2`).

### ID and Data Type Policy
- All resource IDs in database use `bigint`.
- In API JSON payloads, IDs should be serialized as string to avoid precision issues on JavaScript clients.
- Path params using ID must be numeric string for `bigint` (example: `"101"`).

### Time and Money
- Date-time fields use ISO-8601 UTC (example: `2026-08-21T08:50:00Z`).
- Monetary fields use decimal number format (`numeric(12,2)` at DB side).

### Common Response Envelope
Success:
```json
{
  "data": {},
  "meta": {
    "traceId": "2e56b4c6-5ab7-4bb8-8c53-37cb0f5030de"
  }
}
```

Error:
```json
{
  "error": {
    "code": "ORDER_STATUS_INVALID_TRANSITION",
    "message": "Cannot move order from CLOSED to CONFIRMED",
    "details": [
      {
        "field": "status",
        "reason": "invalid transition"
      }
    ]
  },
  "meta": {
    "traceId": "2e56b4c6-5ab7-4bb8-8c53-37cb0f5030de"
  }
}
```

### Pagination Contract
- Query params: `page` (0-based), `size` (default 20), `sort` (example: `createdAt,desc`).
- List response `meta`:
```json
{
  "meta": {
    "page": 0,
    "size": 20,
    "totalElements": 154,
    "totalPages": 8,
    "traceId": "2e56b4c6-5ab7-4bb8-8c53-37cb0f5030de"
  }
}
```

## Authentication APIs

### POST /api/v1/auth/register
- Purpose: Quick user registration for application access.
- Access: Public.
- Request:
```json
{
  "fullName": "Nguyen Van A",
  "email": "a@example.com",
  "phone": "0900000000",
  "password": "P@ssw0rd!"
}
```
- Response `201`:
```json
{
  "data": {
    "id": "1001",
    "fullName": "Nguyen Van A",
    "email": "a@example.com",
    "createdAt": "2026-08-21T08:50:00Z"
  },
  "meta": {
    "traceId": "2e56b4c6-5ab7-4bb8-8c53-37cb0f5030de"
  }
}
```

### POST /api/v1/auth/login
- Purpose: Authenticate user/admin and issue tokens.
- Access: Public.
- Request:
```json
{
  "email": "a@example.com",
  "password": "P@ssw0rd!"
}
```
- Response `200`:
```json
{
  "data": {
    "accessToken": "<jwt>",
    "refreshToken": "<jwt>",
    "expiresInSeconds": 3600,
    "roles": ["USER"]
  },
  "meta": {
    "traceId": "2e56b4c6-5ab7-4bb8-8c53-37cb0f5030de"
  }
}
```

### POST /api/v1/auth/refresh
- Purpose: Refresh access token.
- Access: Authenticated user.

### POST /api/v1/auth/logout
- Purpose: Revoke active session/token.
- Access: Authenticated user.

## Master APIs

### User-facing APIs (Public/Customer)

### GET /api/v1/menu
- Purpose: Browse available products.
- Query: `page`, `size`, `sort`, `keyword`, `categoryId`.
- Response `200`: paginated list of menu items.

### GET /api/v1/menu/{id}
- Purpose: View product details.
- Path param: `id` (bigint numeric string).
- Response `200`:
```json
{
  "data": {
    "id": "2001",
    "categoryId": "101",
    "name": "Fried Rice",
    "price": 55000.00,
    "imageUrl": "https://cdn.example.com/menu/fried-rice.png",
    "isActive": true,
    "createdAt": "2026-08-21T08:50:00Z",
    "updatedAt": "2026-08-21T08:50:00Z"
  },
  "meta": {
    "traceId": "2e56b4c6-5ab7-4bb8-8c53-37cb0f5030de"
  }
}
```

### Admin-only APIs (Internal)
- GET /api/v1/admin/menu
- POST /api/v1/admin/menu
- PUT /api/v1/admin/menu/{id}
- DELETE /api/v1/admin/menu/{id}
- GET /api/v1/admin/inventory
- POST /api/v1/admin/inventory
- PUT /api/v1/admin/inventory/{id}
- GET /api/v1/admin/costs
- POST /api/v1/admin/costs
- PUT /api/v1/admin/costs/{id}
- GET /api/v1/admin/budget
- POST /api/v1/admin/budget
- PUT /api/v1/admin/budget/{id}

## Transaction APIs

### User transaction APIs

### POST /api/v1/orders
- Purpose: Create dine-in order.
- Access: Authenticated customer.
- Request:
```json
{
  "tableId": "301",
  "items": [
    {
      "menuItemId": "2001",
      "quantity": 2,
      "note": "Less spicy"
    }
  ]
}
```
- Response `201`:
```json
{
  "data": {
    "id": "5001",
    "orderCode": "ORD-20260821-0001",
    "status": "NEW",
    "subtotalAmount": 110000.00,
    "discountAmount": 0.00,
    "totalAmount": 110000.00,
    "createdAt": "2026-08-21T08:50:00Z"
  },
  "meta": {
    "traceId": "2e56b4c6-5ab7-4bb8-8c53-37cb0f5030de"
  }
}
```

### GET /api/v1/orders/{id}
- Purpose: View order status/detail.
- Access: Order owner or authorized staff.
- Path param: `id` (bigint numeric string).

### Admin/staff transaction APIs

### GET /api/v1/admin/orders
- Purpose: View operational order queue.
- Access: Admin/Staff roles.

### PUT /api/v1/admin/orders/{id}/status
- Purpose: Update order lifecycle status.
- Access: Admin/Staff roles.
- Allowed transitions:
  - `NEW -> CONFIRMED`
  - `CONFIRMED -> SERVED`
  - `SERVED -> CLOSED`
  - `NEW|CONFIRMED -> CANCELLED`
- Invalid transitions must return `422`.

## Error Codes
- 400: Bad Request (invalid payload, invalid bigint format)
- 401: Unauthorized (missing/invalid authentication)
- 403: Forbidden (role is not allowed)
- 404: Not Found (resource does not exist)
- 409: Conflict (state conflict or duplicate operation)
- 422: Unprocessable Entity (business validation failed)
- 500: Internal Server Error

## API to Database Mapping
| API Group | Main Tables |
|---|---|
| Authentication | `ts_users`, `ts_roles`, `ts_user_roles` |
| Public Menu | `ts_menu_categories`, `ts_menu_items` |
| User Orders | `ts_orders`, `ts_order_items`, `ts_payments` |
| Inventory Management | `ts_inventory_items`, `ts_inventory_transactions` |
| Cost and Budget Management | `ts_expense_records` |

## Access Boundary Matrix
| API Group | Allowed Roles | Exposure |
|---|---|---|
| Authentication | Guest, User, Admin | Public + Authenticated |
| Public Menu | Guest, User | Public |
| User Orders | User, Admin/Staff (restricted) | Authenticated |
| Admin Menu Management | Admin/Staff | Private Internal |
| Inventory Management | Admin/Staff | Private Internal |
| Cost and Budget Management | Admin/Staff | Private Internal |

## Boundary Rules
- Inventory and cost management APIs are strictly internal Admin-only capabilities.
- End users must not access inventory, cost, or budget endpoints.
- Any access attempt by end-user roles to internal endpoints must return 403.
- Internal and public API groups should be separated by route prefix and role guard policy.

## OpenAPI Export Note
- Next step for engineering: convert this design into OpenAPI 3.1 YAML for code generation and automated contract testing.
- Runtime implementation should expose OpenAPI and Swagger UI through Springdoc.
- Standard endpoints:
  - `/v3/api-docs`
  - `/swagger-ui/index.html`
- For release validation, ensure generated OpenAPI is aligned with this design document.
