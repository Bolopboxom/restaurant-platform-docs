# Business Requirement

## Business Problems
- Restaurant operations are fragmented across manual tools and spreadsheets.
- Inventory, cost tracking, and budget visibility are inconsistent between branches.
- Menu updates are not centralized, causing pricing and content mismatches.
- Monthly income and expense reporting is slow and difficult to verify.
- Customer dine-in ordering flow is not fully digitized.

## Objectives
- Centralize admin operations for inventory, cost, menu, and budget management.
- Provide a clear monthly income and expense dashboard for branch operations.
- Deliver a simple user journey for fast registration, product browsing, and dine-in ordering.
- Establish a technical baseline that supports future scale-up and service decomposition.

## Stakeholders
- Product Owner
- Restaurant Admin Team
- Branch Manager
- Cashier
- Kitchen Staff
- End Customer
- Engineering Team (Frontend, Backend, DevOps, QA)

## Current Process
- Inventory and cost management rely on manual or disconnected tools.
- Budget and monthly financial visibility are not standardized.
- Menu changes require repetitive manual updates and cross-checking.
- Customer ordering at table is partially manual.
- Data traceability and operational audit are limited.

## Expected Process
- Admin manages inventory, cost, menu, and budget in one platform.
- Monthly income and expense dashboard is available by branch and period.
- Customer can register quickly, browse products, and place dine-in orders in-app.
- Core operational actions are traceable with logs and role-based access.
- MVP features are deployed to DEV with CI/CD baseline for iterative delivery.

## Business Rules
- Inventory and cost management are internal Admin-only capabilities.
- End users must not access inventory or cost functions directly.
- Public user scope is limited to product browsing and dine-in ordering in MVP.
- Menu information must be controlled by authorized admin roles.
- Sensitive actions (cost updates, budget changes, pricing changes) must be auditable.
- Order lifecycle must use consistent status transitions.
