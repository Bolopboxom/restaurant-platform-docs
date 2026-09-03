# restaurant-platform-common

## Purpose

Shared Java library for reusable backend components.

## Reference

- Source of truth: [.github/input/git-repository-design.md](../../.github/input/git-repository-design.md)
- Main repo docs: [README.md](../../README.md)

## Scope

- DTO
- Common response
- Exception handling
- Error codes
- Utilities
- Logging helpers
- Security components

## Recommended package structure

```text
com.restaurantplatform.common
├── response
│   ├── ApiResponse
│   ├── PageResponse
│   └── ResponseMeta
├── exception
│   ├── BaseException
│   ├── ErrorCode
│   └── GlobalExceptionHandler
├── dto
│   ├── auth
│   ├── user
│   └── audit
├── security
│   ├── JwtClaims
│   ├── RoleConstants
│   └── SecurityUtils
├── logging
│   ├── TraceIdFilter
│   └── LogContext
└── util
	├── DateTimeUtils
	├── MaskingUtils
	└── IdUtils
```

## Maven and Gradle publication

### Maven coordinates (example)

```xml
<groupId>com.restaurantplatform</groupId>
<artifactId>restaurant-platform-common</artifactId>
<version>1.0.0</version>
<packaging>jar</packaging>
```

### Gradle coordinates (example)

```gradle
group = 'com.restaurantplatform'
version = '1.0.0'
```

## Public contracts for consumers

- API response envelope classes.
- Error code enum and base exception hierarchy.
- Security claim model and role constants.
- Trace and logging helper abstractions.

## Class placement guide

### Should be in common
- Generic response classes used by multiple backend services.
- Shared error contracts and exception classes.
- Generic utilities unrelated to one business domain.
- Security helpers reused by api/auth/gateway modules.

### Must not be in common
- JPA entities and repositories.
- Menu/Order/Payment domain services.
- Service-specific integration clients.
- Any business rule that only belongs to one repository.

## Versioning and compatibility rules

- Use semantic versioning for public contracts.
- Backward compatibility is required for minor and patch versions.
- Breaking changes require major version bump and migration notes.
- Consumers should pin version and upgrade intentionally.

## Design rules

- Keep the library generic.
- Do not place service-specific business logic here.
- Expose stable public contracts only.
- Keep backward compatibility as a priority.
