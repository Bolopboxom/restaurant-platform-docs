# Monitoring

## Logs
### Logging Standards
- Use structured logs (JSON preferred).
- Include timestamp, level, service, environment, and message.
- Include traceId for request correlation across FE and API.
- Mask sensitive fields (passwords, tokens, personal data).

### Must-Log Events
- Authentication success and failure.
- Authorization denied events (403).
- Order lifecycle events.
- Menu and inventory update events.
- Unhandled exceptions and integration failures.

### Log Retention
- DEV retention minimum 14 days.
- Higher environments should define longer retention by policy.
- Audit-relevant events must follow compliance retention rules.

## Metrics
### Service Health Metrics
- Service uptime and healthcheck status.
- Request rate, latency p50/p95/p99.
- HTTP 4xx and 5xx rates by endpoint.
- Container CPU and memory usage.
- Restart count and crash-loop detection.

### Database Metrics
- Active database connections and pool saturation.
- Slow query count and latency trend.
- Database CPU/storage trend.
- Replication lag (when replication is enabled later).

### Business Metrics (MVP)
- Orders created per hour/day.
- Order status distribution.
- Payment success ratio.
- Menu item availability ratio.

## Alerts
### Alert Severity
- Critical: service down, major error spike, DB unavailable.
- Warning: latency degradation, high resource usage, repeated auth failures.
- Info: deployment completion and non-critical thresholds.

### Recommended Alert Rules (MVP)
- API 5xx rate above threshold for 5 minutes.
- p95 latency above target on key endpoints.
- Container restart count above threshold.
- Database connection pool near exhaustion.
- Burst of failed logins indicating brute-force pattern.

### Alert Response Rules
- Each alert must have owner and escalation path.
- Critical alerts require immediate acknowledgment.
- Incidents must include timeline and resolution notes.

## Dashboards
### Minimum Dashboards
- Platform overview: health, traffic, error, and latency.
- API dashboard: endpoint-level latency and error distribution.
- Database dashboard: connection, query latency, resource usage.
- Security dashboard: failed login trend, 403 trend, suspicious access attempts.
- Business dashboard: order throughput and order status trend.

### Dashboard Practices
- Use consistent naming and tags by service and environment.
- Include links from alerts to dashboard panels.
- Review dashboard usefulness each release cycle.

## SLO Baseline (MVP)
- Define SLOs for key APIs based on existing NFR targets.
- Track error budget consumption trend by release.
- Use SLO breaches to prioritize reliability improvements.

## Incident Management Integration
- Correlate monitoring incidents with recent deployments.
- Keep post-mortem records with corrective and preventive actions.

## Operational Checklist
- [ ] Health endpoints exist for all services.
- [ ] Logs include traceId and do not expose sensitive fields.
- [ ] Core metrics are collected and visible.
- [ ] Alert rules are active and tested.
- [ ] Dashboards cover FE, API, DB, and security.
- [ ] On-call and escalation contacts are documented.

## KPIs
- Mean time to detect critical incidents under 5 minutes.
- Mean time to recover improves each release cycle.
- Alert noise ratio decreases over time.
- Observability coverage includes all production-critical services.
