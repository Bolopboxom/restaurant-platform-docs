# Testing Strategy

## Unit Test
### Scope
- Service-layer business rules.
- Validators and mappers.
- Utility components.
- Order status transition logic.

### Coverage Targets
- Backend core modules (auth/menu/order): minimum 70 percent line coverage.
- Frontend critical logic (login/menu/order): mandatory component and service tests.
- New critical feature: no merge if touched-module coverage decreases.

### Required Unit Cases
- Auth: password validation and token generation failure paths.
- Menu: create/update validation and inactive item filtering.
- Order: valid and invalid status transitions.
- Internal operations: role restrictions for inventory/cost actions.
- Error mapping: domain error to API error code consistency.

## Integration Test
### Scope
- REST API with isolated test database.
- Authentication and authorization checks by role.
- API contract compatibility with documented request/response shapes.

### Required Integration Scenarios
- Public endpoints are accessible without admin role.
- Internal endpoints under /api/v1/admin/* return 403 for USER role.
- Order creation writes ts_orders and ts_order_items consistently.
- Invalid bigint id format returns 400.
- Valid but non-existing id returns 404.
- Business violations return 422 with traceId.

### Data and Environment Rules
- Use dedicated test schema/database.
- Seed minimum data set for roles, users, menu categories, and menu items.
- Ensure test isolation and cleanup after each test suite.

## UAT
### Participants
- Product owner or business representative.
- Admin representative.
- End-user representative.

### MVP UAT Scenarios
- User registers, logs in, browses menu, and creates dine-in order.
- Admin manages menu items and changes appear correctly on user-side menu.
- Admin updates order status only through allowed transition flow.
- End user cannot access internal inventory/cost/budget capabilities.
- Dashboard or operational summary returns expected values.

### UAT Entry Criteria
- Unit and integration pipelines pass.
- No unresolved critical/high defects in core flows.
- Test environment data is prepared and stable.

### UAT Exit Criteria
- 100 percent critical scenarios pass.
- No blocker defects remain unresolved.
- Accepted issues are documented with owner and target fix version.

## Performance Test
### MVP Baseline Targets
- GET /api/v1/menu p95 under 500 ms in normal DEV load.
- POST /api/v1/orders p95 under 800 ms in normal DEV load.
- Admin operational APIs p95 under 2 seconds.
- Error rate under 1 percent in baseline profile.

### Test Profiles
- Smoke profile: quick check after deployment.
- Baseline profile: expected daily load simulation.
- Stress profile: identify degradation threshold.

### Metrics to Capture
- Response time p50/p95/p99.
- Throughput (requests per second).
- Error rate by endpoint.
- CPU, memory, and database connection pool usage.

## Defect Management
- Severity: Critical, High, Medium, Low.
- Critical and High defects block release.
- Each defect must include steps, expected result, actual result, logs, traceId, and environment.

## CI/CD Quality Gates
- Unit tests must pass.
- Integration tests must pass for merge to develop.
- Security checks must have no unresolved critical vulnerabilities.
- Performance smoke test must pass for DEV deployment completion.

## Test Artifacts
- Test case list by module.
- Automated test reports from CI.
- UAT sign-off record.
- Performance report snapshots.

## KPIs
- Unit test pass rate at or above 95 percent.
- Integration test pass rate at or above 95 percent.
- Defect leakage to UAT decreases each iteration.
- Mean time to detect regression under 1 day during active sprint.
