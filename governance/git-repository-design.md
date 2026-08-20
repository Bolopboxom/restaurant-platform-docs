# Git Repository Design

## 1. Overview

Mục tiêu của hệ thống là xây dựng một nền tảng Restaurant Platform theo kiến trúc Microservice, hỗ trợ mở rộng trong tương lai và quản lý tài liệu tập trung.

### Nguyên tắc thiết kế

- Tách biệt Frontend, Backend và Infrastructure
- Mỗi repository có trách nhiệm riêng (Single Responsibility)
- Tài liệu được quản lý tập trung
- Hỗ trợ CI/CD độc lập cho từng thành phần
- Sẵn sàng mở rộng thành Microservices

---

## 2. Repository Landscape

```text
Bolopboxom
│
├── restaurant-platform-docs
│
├── restaurant-platform-common
│
├── restaurant-platform-api
│
├── restaurant-platform-admin
│
├── restaurant-platform-user
│
├── restaurant-platform-auth
│
├── restaurant-platform-notification
│
├── restaurant-platform-gateway
│
├── restaurant-platform-devops
│
├── restaurant-platform-infra
│
├── restaurant-platform-mobile
│
└── restaurant-platform-ai
```

---

## 3. Repository Responsibilities

### restaurant-platform-docs

Single Source of Truth.

Chứa:

- Vision
- Requirements
- Architecture
- Database Design
- API Design
- Roadmap
- Standards
- Best Practices

---

### restaurant-platform-common

Shared Java Library.

Chứa:

- DTO
- Common Response
- Exception Handling
- Error Codes
- Utilities
- Logging
- Security Components

Được sử dụng bởi:

- restaurant-platform-api
- restaurant-platform-auth
- restaurant-platform-notification
- restaurant-platform-ai

---

### restaurant-platform-api

Business Core Service.

Modules:

- User Management
- Menu Management
- Order Management
- Payment Management
- Reporting

Tech Stack:

- Java 21
- Spring Boot 3
- Spring Security
- PostgreSQL
- Redis

---

### restaurant-platform-admin

Admin Portal.

Tech Stack:

- Angular 21
- RxJS
- Signals
- Angular Material

Users:

- Admin
- Manager
- Cashier
- Kitchen Staff

---

### restaurant-platform-user

Customer Portal.

Tech Stack:

- React
- TypeScript

Features:

- Browse Menu
- Place Order
- Reservation
- Order History

---

### restaurant-platform-auth

Authentication Service.

Responsibilities:

- Login
- JWT
- OAuth2
- Refresh Token
- Role
- Permission

---

### restaurant-platform-notification

Notification Service.

Responsibilities:

- Email
- SMS
- Push Notification
- Teams Notification

---

### restaurant-platform-gateway

API Gateway.

Responsibilities:

- Routing
- Authentication
- Authorization
- Rate Limiting
- API Aggregation

---

### restaurant-platform-devops

Chứa:

- GitHub Actions
- Jenkins Pipelines
- Docker Compose
- SonarQube Configuration

---

### restaurant-platform-infra

Infrastructure as Code.

Chứa:

- Terraform
- Kubernetes
- Helm
- AWS
- Azure

---

### restaurant-platform-mobile

Mobile Application.

Tech Stack:

- Flutter hoặc React Native

---

### restaurant-platform-ai

AI Service.

Responsibilities:

- Recommendation Engine
- Sales Forecasting
- Analytics
- Chatbot

---

## 4. High-Level Architecture

```text
                     +---------------------+
                     | restaurant-docs     |
                     +----------+----------+
                                |
                                |
                                v

+-------------+      +---------------------+
| Angular     |----->| Gateway             |
| Admin       |      +----------+----------+
+-------------+                 |
                                |
+-------------+                 |
| React User  |-----------------+
+-------------+                 |
                                v

                 +--------------------------+
                 | restaurant-platform-api  |
                 +------------+-------------+
                              |
                              v

                 +--------------------------+
                 | restaurant-platform-common|
                 +--------------------------+

                              |
       --------------------------------------------------
       |                        |                        |
       v                        v                        v

+--------------+    +----------------+     +----------------+
| Auth Service |    | Notification   |     | AI Service     |
+--------------+    +----------------+     +----------------+

                              |
                              v

                 +--------------------------+
                 | PostgreSQL / Redis        |
                 +--------------------------+
```

---

## 5. Dependency Matrix

| Repository | Depends On |
|------------|------------|
| docs | None |
| common | None |
| api | common |
| auth | common |
| notification | common |
| ai | common |
| admin | gateway, api |
| user | gateway, api |
| mobile | gateway, api |
| gateway | api, auth |

---

## 6. Development Roadmap

### Phase 1 (MVP)

- restaurant-platform-docs
- restaurant-platform-common
- restaurant-platform-api
- restaurant-platform-admin
- restaurant-platform-user
- restaurant-platform-devops

### Phase 2

- restaurant-platform-auth
- restaurant-platform-gateway

### Phase 3

- restaurant-platform-notification
- restaurant-platform-infra

### Phase 4

- restaurant-platform-mobile
- restaurant-platform-ai

---

## 7. GitHub Copilot Usage Guide

Khi tạo repository mới bằng GitHub Copilot:

1. Tham khảo tài liệu này trước.
2. Xác định vai trò repository.
3. Xác định dependency với các repository khác.
4. Tuân thủ naming convention `restaurant-platform-*`.
5. Cập nhật dependency matrix nếu có repository mới.
6. Cập nhật roadmap và architecture nếu có thay đổi lớn.

---

## 8. Long-Term Vision

- Enterprise Architecture
- Full Microservice Ecosystem
- Cloud Native Deployment
- Kubernetes
- GitOps
- CI/CD Automation
- Mobile Integration
- AI-Powered Features

---

## 9. Documentation Repository Layout

Mục tiêu của repository docs là giữ toàn bộ tài liệu tham chiếu ở một nơi, nhưng vẫn tách theo nhóm để dễ scale khi thêm service mới.

```text
restaurant-platform-docs/
├── README.md
├── architecture/
├── standards/
├── roadmap/
├── repositories/
│   ├── shared/
│   ├── backend/
│   ├── frontend/
│   └── platform/
├── domain-models/
├── api-contracts/
├── database/
└── decisions/
```

### Repository documentation rules

- Mỗi repository con phải có một file tài liệu riêng trong nhóm phù hợp.
- Mỗi file tài liệu phải refer về git-repository-design.md và README root.
- Khi thêm service mới, chỉ thêm file vào đúng nhóm, không phá layout chung.
- Nếu service mới thuộc backend, frontend hoặc platform, đặt đúng nhánh tương ứng để giữ tính mở rộng.