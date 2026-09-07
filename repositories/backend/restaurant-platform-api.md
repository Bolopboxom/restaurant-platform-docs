# restaurant-platform-api

## Purpose

Core backend service for business operations.

## Reference

- Source of truth: [.github/input/git-repository-design.md](../../.github/input/git-repository-design.md)
- Main repo docs: [README.md](../../README.md)
- Shared library: [restaurant-platform-common](../shared/restaurant-platform-common.md)

## Scope

- User management
- Menu management
- Order management
- Payment management
- Reporting

## Tech Stack Baseline

- JDK 21
- Maven
- Spring Boot 3.3.x
- PostgreSQL 17
- Flyway 10.x
- Kafka 3.x
- JUnit 5
- OpenAPI 3 + Swagger UI (Springdoc)
- Jenkins CI/CD
- Docker

## Recommended structure

- api
- application
- domain
- infrastructure
- dto
- config
- shared integration clients

## Recommended package structure

```text
com.restaurantplatform.api
├── ApiApplication
├── config
├── controller
│   ├── auth
│   ├── menu
│   ├── order
│   └── admin
├── application
│   ├── auth
│   ├── menu
│   ├── order
│   └── reporting
├── domain
│   ├── model
│   ├── service
│   └── repository
├── infrastructure
│   ├── persistence
│   ├── security
│   └── external
├── mapper
└── dto
```

## Dependency on restaurant-platform-common

### Maven example

```xml
<dependency>
	<groupId>com.restaurantplatform</groupId>
	<artifactId>restaurant-platform-common</artifactId>
	<version>1.0.0</version>
</dependency>
```

### Gradle example

```gradle
dependencies {
		implementation 'com.restaurantplatform:restaurant-platform-common:1.0.0'
}
```

## API Documentation and Contract Source

- Runtime API docs are exposed via Swagger UI and OpenAPI endpoints.
- OpenAPI document should be treated as implementation contract generated from code.
- The design-first contract reference remains in `restaurant-platform-docs/api-contracts/09-api.md`.

### Standard endpoints (Springdoc)

- `/v3/api-docs`
- `/swagger-ui/index.html`

## Request flow with common library

```text
Client
	-> Controller (api)
	-> Application Service (api)
	-> Domain Service/Repository (api)
	-> Database
	-> ApiResponse/ErrorCode/BaseException (common)
	-> Client
```

### Flow details
1. Controller receives request and performs basic validation.
2. Application service executes business logic and domain orchestration.
3. Domain layer persists or loads data via repository.
4. On success, controller wraps data using common response envelope.
5. On business error, service throws standardized exception from common.
6. Global exception handler returns consistent error payload.

## Class placement guide

### Keep in api
- Controllers for menu/order/payment/admin endpoints.
- Business services, domain models, repositories, and mappers.
- Workflow-specific validators and state transition rules.

### Import from common
- Common response envelope classes.
- Error code enum and base exception classes.
- Shared security constants and JWT claim models.
- Trace and logging helper components.

### Boundary rules
- api depends on common, but common must never depend on api.
- Do not duplicate response/error contracts already provided by common.
- Do not move service-specific business rules into common.
- Keep modules isolated for future service extraction.

## Testing Baseline for This Repository

- Unit tests use JUnit 5.
- Integration tests should validate PostgreSQL and Kafka integration paths.
- CI must run test suites before image build and deployment.

## Design rules

- Start as a modular monolith.
- Keep each domain isolated by package or module boundary.
- Avoid shared entity coupling across domains.
- Design for later service extraction.
