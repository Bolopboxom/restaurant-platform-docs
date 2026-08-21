# Non Functional Requirement

## Performance
- MVP Baseline
  - Public menu APIs should target p95 response time under 500 ms in normal DEV test load.
  - Dine-in order creation APIs should target p95 response time under 800 ms in normal DEV test load.
  - Admin dashboard page load or dashboard data aggregation should target p95 response time under 2 seconds.
- Future Target
  - Performance thresholds should be reviewed and tightened before SIT/UAT and production rollout.
  - High-traffic read endpoints should support caching strategies without changing business behavior.

## Scalability
- MVP Baseline
  - Admin frontend, user frontend, and backend API must be deployable independently by repository.
  - Backend API should remain stateless where possible to support horizontal scaling.
  - Core domains must be separated logically so menu, order, reporting, and internal admin modules can be extracted later.
- Future Target
  - Internal modules such as inventory, cost, and budget should be detachable into separate services if growth demands it.
  - Public customer traffic and internal admin traffic should be isolatable at routing and deployment level.

## Availability
- MVP Baseline
  - DEV environment must support stable team testing during working hours.
  - Core services must support restart and redeploy without manual data repair in normal cases.
  - Rollback capability must exist for failed deployment in DEV.
- Future Target
  - Core API services should target 99.9 percent monthly availability in higher environments.
  - Production readiness should include health checks, restart policy, and environment failover planning.

## Security
- MVP Baseline
  - Authentication must use secure token-based access control.
  - Authorization must enforce role boundaries among Admin, Staff, and User roles.
  - Inventory, cost, and budget endpoints must be internal-only and inaccessible to end-user roles.
  - Passwords must be stored with secure hashing.
  - Sensitive data must be protected in transit when deployed beyond local development.
- Future Target
  - Secrets should be externalized from source code and environment-specific.
  - Security scanning and dependency review should be part of CI/CD quality gates.

## Logging
- MVP Baseline
  - System must log authentication events, order creation, menu updates, internal cost updates, and application errors.
  - Logs should include timestamp, actor or system identity, action, and request correlation id where available.
  - Logs must be sufficient to trace issues from frontend request to backend processing.
- Future Target
  - Structured centralized logging should be adopted for cross-service diagnostics.
  - Log severity and retention policy should be standardized across repositories.

## Audit
- MVP Baseline
  - Audit trail is required for sensitive operations including menu price change, cost update, budget update, and internal order status update.
  - Each audit record should include who performed the action, when it happened, what changed, and the affected entity.
  - Audit data should not be editable by normal business users.
- Future Target
  - Audit history should support compliance review and anomaly investigation.
  - Audit events should be exportable or queryable for operational review.

## NFR Scope Rule
- Every NFR must distinguish between MVP baseline and future target.
- MVP commitments should remain measurable and realistic for DEV-first delivery.
- Future targets should guide scale-up without blocking first release delivery.
