# UI UX Design

## Sitemap
### Admin Portal (MVP)
- Login
- Dashboard
- Menu Management
  - Menu List
  - Create Menu Item
  - Edit Menu Item
- Order Operations
  - Order Queue

### User Portal (MVP)
- Register
- Login
- Product Catalog
- Product Detail
- Create Order
- Order Confirmation
- Order Tracking

### Future Scope
- Table Reservation
- Inventory Management Screens
- Cost Management Screens
- Budget Management Screens
- Notification Center

## Screens
### Admin Screens
- Admin Login
- Dashboard Overview
- Menu List
- Create/Edit Menu Item
- Order Queue

### User Screens
- User Registration
- User Login
- Product Catalog
- Product Detail
- Order Creation
- Order Confirmation
- Order Status Tracking

### Screen Priority
- MVP Core Admin Screens
  - Dashboard Overview
  - Menu List
  - Create/Edit Menu Item
- MVP Core User Screens
  - Product Catalog
  - Product Detail
  - Order Creation
  - Order Status Tracking

## Navigation
### Admin Navigation
- Use persistent sidebar or top navigation for fast access to Dashboard, Menu, and Orders.
- Dashboard and Menu Management are the main entry points for MVP.
- Internal-only modules must be clearly separated from customer-facing screens.

### User Navigation
- Keep navigation shallow and task-focused.
- Primary path should be Product Catalog -> Product Detail -> Create Order -> Order Tracking.
- Registration and login should not interrupt browsing more than necessary.

### Navigation Rules
- Internal admin navigation must never be exposed in user portal.
- User portal should prioritize shortest path to browse and order.
- Breadcrumbs are optional for admin, unnecessary for user MVP screens.

## UX Rules
### Admin UX Rules
- Prioritize information clarity and fast operational actions.
- Use structured data tables and clear action buttons.
- Sensitive actions such as price change should require explicit confirmation.
- Empty states should explain what admin can do next.

### User UX Rules
- Prioritize readability of menu items, images, and pricing.
- Use clear call-to-action labels for viewing details and placing orders.
- Reduce steps required to complete a dine-in order.
- Show meaningful error messages when order creation fails.

### Shared UX Rules
- Validation messages must be concise and attached to the relevant field.
- Error states must be understandable and actionable.
- Loading states should be visible for order submission and dashboard retrieval.
- Internal-only functionality must remain visually and technically separated from public functionality.

## User Journey
### Admin Journey (MVP)
1. Admin logs in.
2. Admin lands on dashboard.
3. Admin reviews monthly income and expense summary.
4. Admin navigates to menu management.
5. Admin creates or updates menu item details.

### User Journey (MVP)
1. User registers or logs in quickly.
2. User opens product catalog.
3. User reviews product detail.
4. User creates dine-in order.
5. User checks order status.

## Responsive Design
- Admin Portal: desktop-first design.
- User Portal: mobile-first design.
- Product browsing and ordering must remain usable on common smartphone screens.
- Admin dashboard and menu management must remain readable on laptop-sized screens.

## Accessibility Considerations
- Forms must use clear labels.
- Error messages should be associated with the relevant input fields.
- Color contrast should remain readable for operational and public views.
- Keyboard accessibility should be supported at minimum for admin workflows.

## Out of Scope for MVP
- High-fidelity wireframes.
- Full design system specification.
- Complete reservation experience.
- Advanced notification UX.
