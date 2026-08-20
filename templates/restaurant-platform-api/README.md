# restaurant-platform-api

Business Core Service cho hệ sinh thái Restaurant Platform.

## Tech Stack

- Java 21
- Spring Boot 3
- Spring Security
- PostgreSQL
- Redis
- Maven

## Module phạm vi

- User Management
- Menu Management
- Order Management
- Payment Management
- Reporting

## Cấu trúc thư mục

```text
restaurant-platform-api/
├── .github/workflows/ci.yml
├── src/main/java/com/restaurant/platform/api
│   ├── config
│   ├── controller
│   ├── domain
│   ├── dto
│   ├── exception
│   ├── repository
│   ├── service
│   └── RestaurantPlatformApiApplication.java
├── src/main/resources
│   └── application.yml
├── src/test/java/com/restaurant/platform/api
│   └── RestaurantPlatformApiApplicationTests.java
├── .gitignore
└── pom.xml
```

## Khởi động local

```bash
mvn spring-boot:run
```

## CI

Workflow `ci.yml` sẽ:

1. checkout source
2. setup Java 21
3. cache Maven dependencies
4. chạy `mvn verify`
