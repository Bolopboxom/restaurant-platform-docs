# Jenkins CI/CD Guide for Spring Boot + Docker + PostgreSQL + Nginx

## 1. Mục tiêu

Tài liệu này hướng dẫn triển khai CI/CD cơ bản cho dự án Spring Boot sử dụng:

- GitHub
- Jenkins
- Maven
- Docker
- PostgreSQL
- Nginx

Mục tiêu:

```text
Developer Push Code
        ↓
      GitHub
        ↓
      Jenkins
        ↓
 Maven Build + Test
        ↓
 Docker Build
        ↓
 Docker Deploy
        ↓
 Spring Boot
        ↓
 PostgreSQL
```

## Kiến trúc tổng thể, Pipeline, Dockerfile, Docker Compose, Jenkinsfile và Best Practices như đã mô tả trong tài liệu tham khảo.

### Checklist
- Docker Desktop
- Jenkins
- PostgreSQL
- Nginx
- GitHub
- Dockerfile
- docker-compose.yml
- Jenkinsfile
