# Project Vision

## Project Overview
- Project Name: Restaurant Platform
- Business Domain: Restaurant Operations and Customer Ordering
- Stakeholders:
  - Product Owner
  - Restaurant Admin Team
  - Branch Operations (cashier, kitchen, manager)
  - End Users (customers)
  - Engineering Team (frontend, backend, devops)

## Business Goals
- Build a digital platform to standardize restaurant operations and reduce manual processes.
- Enable admins to manage menu, inventory, costs, and monthly finance visibility in one system.
- Improve customer experience with fast account registration, product browsing, dine-in ordering, and table booking.
- Establish a scalable technical foundation for expanding services after MVP.

## Success Criteria
- Business KPI
  - Admin can monitor monthly income and expense through a dashboard.
  - Menu updates (image, price, product details) can be maintained by admin without technical support.
  - Users can complete on-site ordering flow from product browsing to order submission.
- Product KPI (MVP)
  - Admin dashboard and menu management are usable in DEV.
  - User product listing and on-site order flow are usable in DEV.
- Initial SLA target (MVP, DEV baseline)
  - Core API endpoints available for internal testing during working hours.
  - Major incidents can be identified by logs and handled with rollback procedure.

## Scope
### In Scope
- Product Vision (MVP)
  - Fast user account registration for app usage.
  - Admin functions:
    - Manage ingredient and supply costs.
    - Manage inventory (ingredients, drinks, and related stock items).
    - Manage menu items (images, prices, and product details).
    - Manage income and expense budget.
    - View monthly income and expense dashboard.
  - User functions:
    - Browse products and place dine-in order.
    - Reserve table.
- MVP Delivery Scope (first release)
  - Admin:
    - Dashboard (income and expense view).
    - Menu management page (image, price, product details).
  - User:
    - Product listing and dine-in ordering.
- Technical Scope (initial)
  - Frontend admin repository using Angular.
  - Frontend user repository using React.
  - Backend API repository using Java Spring Boot.
  - PostgreSQL database running on Docker.
  - DevOps baseline using Jenkins Pipelines and Docker Compose.
  - Deploy MVP baseline to DEV environment.

### Out of Scope
- Full mobile application in MVP.
- Advanced AI features in MVP.
- Multi-tenant architecture in MVP.
- Full microservice decomposition in MVP (start with modular core, then split by domain later).

## Assumptions
- Restaurant operation flow is aligned across branches for MVP.
- Menu and product data model is stable enough for first release.
- DEV environment is available for continuous integration and testing.
- Teams can work in parallel across FE, BE, and DevOps repositories.

## Constraints
- MVP focuses on core admin and user ordering flows only.
- Timeline and team capacity may limit advanced features in first release.
- Initial deployment targets DEV first before higher environments.
- Integration with external providers is limited in early phase.
- Boundary rule: Inventory and cost management modules are internal Admin-only capabilities and must not expose public APIs for end-user access.
