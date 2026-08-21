# DevOps

## Branch Strategy
### Branch Model
- `main`: release-ready branch.
- `develop`: integration branch for next release cycle.
- `feature/<scope>-<short-name>`: new feature work.
- `bugfix/<scope>-<short-name>`: non-urgent bug fixes.
- `hotfix/<scope>-<short-name>`: urgent fix branched from `main`.

### Merge and Release Rules
- Pull Request is mandatory for all merges to `develop` and `main`.
- Direct push to `main` is not allowed.
- PR must pass CI checks and at least 1 approval.
- Use squash merge to keep commit history concise.
- Release tags follow SemVer: `vMAJOR.MINOR.PATCH`.

## CI/CD
### What Is Required

### Accounts and Access
- Git provider account (GitHub or equivalent).
- Jenkins does not require a cloud account; it is self-hosted.
- Access token or SSH key to let Jenkins clone repositories.

### Jenkins Host Baseline
- OS: Ubuntu Linux is recommended.
- Minimum for learning: 2 vCPU, 4 GB RAM.
- Recommended for multi-service builds: 4 vCPU, 8 GB RAM.

### Required Installations
- Git.
- Docker Engine and Docker Compose plugin.
- Jenkins LTS.

### Jenkins Plugins (Recommended)
- Git Plugin.
- Pipeline Plugin.
- Credentials Binding Plugin.
- SSH Agent Plugin.
- Docker Pipeline Plugin.
- Blue Ocean (optional UI convenience).

### CI/CD Pipeline Stages
1. Checkout source code.
2. Restore dependencies.
3. Lint and unit tests.
4. Dependency/security scan.
5. Build artifacts.
6. Build Docker image and tag with commit SHA.
7. Push image to registry.
8. Deploy to DEV.
9. Run post-deploy smoke tests.

### Triggers and Quality Gates
- PR to `develop`: run validation pipeline.
- Merge to `develop`: auto deploy to DEV.
- Merge to `main` with release tag: run release pipeline.
- Block deploy if tests fail or critical vulnerabilities are found.

### Rollback Strategy
- Keep image tags by commit SHA.
- Rollback by redeploying previous stable tag.
- Do not rebuild during rollback.

## Docker
### What Is Required

### Accounts and Registry
- Docker account is optional for local-only runs.
- Docker Hub or GitHub Container Registry account is recommended for shared deployments.

### Required Installations
- Windows/Mac: Docker Desktop.
- Linux: Docker Engine + Compose plugin.

### Docker Standards
- Use multi-stage Dockerfile builds.
- Run containers as non-root where possible.
- Add healthcheck for API container.
- Keep runtime config in environment variables, not hardcoded values.

### Image Tagging Rules
- `<service-name>:<git-sha>` for immutable deployment.
- `<service-name>:<env>-latest` for environment convenience only.

### Local Compose for Platform
- Services in DEV compose:
	- `admin-fe`
	- `user-fe`
	- `api`
	- `postgres`
- Use shared network for internal communication.
- Use volume for PostgreSQL data persistence.
- Keep secrets out of repository; use `.env` per service.

## Environments
### DEV Environment Scope
- Purpose: fast integration and team testing.
- Deploy source: `develop` branch.
- Data policy: can be reset in controlled manner.

### STAGING Environment Scope
- Purpose: regression, UAT, and pre-release checks.
- Should be closer to production configuration.
- Deploy source: release candidate tag/branch.

### PROD Environment Scope
- Manual approval required before deploy.
- Use rolling or blue/green strategy.
- Monitoring, alerting, and backup verification are mandatory.

### Artifact Promotion Rule
- Build once, promote same artifact across environments.
- Avoid rebuild-per-environment to reduce drift.

## Practical Checklist (Beginner-Friendly)

### A. CI/CD Setup Checklist
- [ ] Prepare a Linux host for Jenkins.
- [ ] Install Git, Docker, Docker Compose, Jenkins LTS.
- [ ] Install required Jenkins plugins.
- [ ] Add repository credentials (PAT or SSH key).
- [ ] Create first pipeline job from repository Jenkinsfile.
- [ ] Configure webhook or polling trigger.
- [ ] Confirm pipeline can build, test, and publish image.

### B. Docker Setup Checklist
- [ ] Install Docker runtime and Compose.
- [ ] Write Dockerfiles for `admin-fe`, `user-fe`, `api`.
- [ ] Create `docker-compose.yml` including `postgres`.
- [ ] Add healthchecks and persistent DB volume.
- [ ] Prepare `.env` files (no plaintext production secrets).
- [ ] Validate full stack startup locally.

### C. Deploy 1 BE + 2 FE + 1 DB to DEV Checklist
- [ ] Prepare one DEV VM host with Docker and Compose.
- [ ] Configure reverse proxy for FE/API routing.
- [ ] Deploy all 4 containers from compose file.
- [ ] Verify DB connectivity and migration execution.
- [ ] Run smoke tests for FE pages and API health endpoint.
- [ ] Enable log capture and basic alert checks.
- [ ] Document rollback command using previous image tag.

## Free or Low-Cost DEV Hosting Options

### Free Tier Candidate
- Oracle Cloud Always Free is a practical choice for self-hosted Docker Compose DEV.

### Important Limitations
- Free tiers have CPU/RAM/network quotas.
- Availability and performance may vary by region and time.
- Resource limits may not sustain stable multi-service load continuously.

### Recommended Starting Path
1. Run full stack locally with Docker Compose.
2. Move same compose stack to one free VM for shared DEV access.
3. Add Jenkins pipeline deployment by SSH.
4. Add backup, monitoring, and rollback automation.

## Minimum Tooling Matrix
| Area | Minimum Requirement | Recommended |
|---|---|---|
| Source Control | GitHub account | GitHub org + branch protection |
| CI/CD | Jenkins LTS on 2 vCPU/4 GB RAM | Jenkins on 4 vCPU/8 GB RAM |
| Containers | Docker + Compose | Docker + Compose + registry scanning |
| Registry | Optional for local | Docker Hub or GHCR |
| DEV Host | Local machine | Cloud VM with reverse proxy |
| Database | PostgreSQL container | PostgreSQL with backup job |

## Operational KPIs for MVP
- Deploy from `develop` to DEV under 15 minutes.
- DEV deployment success rate at or above 95 percent.
- Rollback time under 10 minutes.
- 100 percent services expose healthcheck endpoint.
