# Deployment Guide

## Deployment Flow
### Deployment Principles
- Build once and promote the same artifact across environments.
- Prefer immutable image tags by commit SHA.
- Every deployment must be observable and reversible.
- No manual hot-edit in running containers.

### Step 1: Pre-Deployment
- Confirm target branch or tag is approved.
- Confirm CI pipeline passed (test, scan, build).
- Confirm target Docker images are available in registry.
- Confirm environment variables and secrets are configured.

### Step 2: Deploy to DEV
- Pull target images for admin-fe, user-fe, and api.
- Run database migrations when applicable.
- Deploy services via Docker Compose.
- Wait until all service healthchecks are healthy.
- Run smoke tests for FE pages and API endpoints.

### Step 3: Post-Deployment Verification
- Validate login flow for Admin and User roles.
- Validate core API behavior for menu and order flows.
- Validate internal endpoints continue returning 403 for USER role.
- Confirm logs and metrics are flowing into monitoring tools.

### Versioning and Release Records
- Release tags follow SemVer.
- Runtime deployments should use commit-SHA image tags.
- Maintain release record with commit, image tag, environment, and operator.

## Rollback Plan
### Rollback Triggers
- Critical regression in core user/admin flows.
- Significant error spike after deployment.
- Security boundary regression.
- Migration issue causing service disruption.

### Rollback Steps
1. Identify last known stable image tag.
2. Redeploy stable tag for affected services.
3. Re-run healthcheck and smoke tests.
4. Announce rollback result and impact.
5. Open root-cause analysis task.

### Rollback Constraints
- Avoid rollback path that risks data corruption.
- If migration is non-reversible, use forward-fix plan instead.

## Release Checklist
### Pre-Release
- [ ] PR approvals completed.
- [ ] CI pipeline is green.
- [ ] Security scan result acceptable.
- [ ] Release notes prepared.
- [ ] DB migration reviewed.
- [ ] Rollback plan confirmed.

### During Deployment
- [ ] Target images pulled successfully.
- [ ] Environment configs loaded correctly.
- [ ] Services started and healthy.
- [ ] Smoke tests passed.
- [ ] No abnormal error spike observed.

### Post-Release
- [ ] Monitoring dashboards reviewed.
- [ ] Alerts reviewed and stable.
- [ ] Business-critical flows validated.
- [ ] Release record updated.
- [ ] Team notified of deployment completion.

## Environment Rules
### DEV
- Auto deployment from develop is allowed.
- Fast iteration is prioritized with controlled data reset policy.

### STAGING
- Deploy only release candidate build.
- Regression and UAT are mandatory before production.

### PROD
- Manual approval gate is mandatory.
- Deployment window and communication plan are mandatory.
- Backup and restore readiness must be verified before release.

## KPIs
- Deployment success rate at or above 95 percent.
- Mean time to rollback under 10 minutes.
- Change failure rate trend decreases by release cycle.
