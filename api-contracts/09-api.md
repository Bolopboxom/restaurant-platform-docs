# API Specification

## Authentication APIs
- POST /api/v1/auth/register
  - Purpose: Quick user registration for application access.
  - Access: Public.
- POST /api/v1/auth/login
  - Purpose: Authenticate user/admin and issue tokens.
  - Access: Public.
- POST /api/v1/auth/refresh
  - Purpose: Refresh access token.
  - Access: Authenticated user.
- POST /api/v1/auth/logout
  - Purpose: Revoke active session/token.
  - Access: Authenticated user.

## Master APIs
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

### User-facing APIs (Public/Customer)
- GET /api/v1/menu
  - Purpose: Browse available products.
- GET /api/v1/menu/{id}
  - Purpose: View product details.

## Transaction APIs
### User transaction APIs
- POST /api/v1/orders
  - Purpose: Create dine-in order.
  - Access: Authenticated customer.
- GET /api/v1/orders/{id}
  - Purpose: View order status/detail.
  - Access: Order owner or authorized staff.

### Admin/staff transaction APIs
- GET /api/v1/admin/orders
  - Purpose: View operational order queue.
  - Access: Admin/Staff roles.
- PUT /api/v1/admin/orders/{id}/status
  - Purpose: Update order lifecycle status.
  - Access: Admin/Staff roles.

## Error Codes
- 400: Bad Request (invalid payload)
- 401: Unauthorized (missing/invalid authentication)
- 403: Forbidden (role is not allowed)
- 404: Not Found (resource does not exist)
- 409: Conflict (state conflict or duplicate operation)
- 422: Unprocessable Entity (business validation failed)
- 500: Internal Server Error

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
