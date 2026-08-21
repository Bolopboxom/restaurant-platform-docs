# Security Design

## Authentication
### Objectives
- Provide consistent authentication for both Admin Portal and User Portal.
- Keep MVP flow simple while preserving scale-up compatibility.

### Token Strategy
- Use JWT-compatible access token and refresh token.
- Access token TTL: 60 minutes.
- Refresh token TTL: 7 days.
- Logout must revoke active refresh token/session.

### Credential Rules
- Password minimum 8 characters.
- Password must contain uppercase, lowercase, numeric, and special character.
- Passwords must be stored with one-way hashing (BCrypt).
- Plaintext password must never be logged.

### Authentication Events to Log
- Login success.
- Login failed.
- Token refresh failed.
- Logout success.

## Authorization
### RBAC Model
- Roles: `ADMIN`, `STAFF`, `USER`.
- Role mapping managed by `ts_roles` and `ts_user_roles`.

### Route-Level Boundary
- Public/User scope routes: `/api/v1/*`.
- Internal admin routes: `/api/v1/admin/*`.
- End-user role access to internal route must return `403`.

### Service-Level Enforcement
- In addition to route guards, sensitive service functions must enforce role checks.
- Ownership policy: order detail can be viewed by order owner or authorized staff/admin only.

### Authorization Audit Targets
- Menu price update.
- Inventory quantity adjustment.
- Cost/Budget update.
- Order status transition by staff/admin.

## Encryption
### Data in Transit
- DEV internal environment may allow HTTP for local productivity.
- Staging and Production must enforce HTTPS (TLS 1.2+).

### Data at Rest
- Password hashes only, no reversible storage.
- Sensitive fields in logs (email, phone) should be masked.
- Tokens and secrets must not be persisted in plaintext logs.

### Secret Management
- Do not hardcode secrets in repository.
- Use environment variables for JWT secrets, DB credentials, and integration keys.
- Secret rotation process should be documented before production rollout.

## Security Controls
### Input Validation
- Validate payload schema for body/query/path before business logic.
- Validate all ID path params as numeric string format for `bigint` contract.
- Return `400` for malformed format and `422` for business-rule violation.

### API Protection Controls
- Apply rate limiting for auth endpoints (`/auth/login`, `/auth/register`, `/auth/refresh`).
- Add temporary account lock policy after repeated failed logins.
- Enforce CORS whitelist by environment.

### Logging and Traceability
- Every request should carry a correlation `traceId`.
- Security-related errors must include `traceId` in response metadata.
- Logs should include actor, action, endpoint, result, and timestamp.

### Audit Trail Requirements
- Capture: `who`, `when`, `what changed`, `entity id`, `before`, `after`, `traceId`.
- Audit records are append-only for normal business users.

### Security Headers
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: DENY`
- `Referrer-Policy: no-referrer`
- `Content-Security-Policy` should be defined at FE gateway/reverse proxy layer.

### Dependency and Pipeline Security
- CI must scan dependencies for known vulnerabilities.
- Block release when critical vulnerabilities are unresolved.

### Incident Response Baseline
- Incident classes: unauthorized access attempt, brute-force spike, token leak suspicion.
- Minimum steps: detect, contain, recover, post-mortem.

### MVP Security KPIs
- 100 percent internal endpoints deny `USER` role with `403`.
- 100 percent passwords stored as hash only.
- 100 percent security errors return traceable `traceId`.
- 0 hardcoded secrets in source repository.
