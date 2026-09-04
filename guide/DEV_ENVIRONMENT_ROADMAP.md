
# DEV_ENVIRONMENT_ROADMAP

# Mục tiêu

Xây dựng môi trường DEV gần giống Enterprise để học và triển khai:

- Spring Boot
- PostgreSQL
- Docker
- Jenkins
- Nginx
- GitHub CI/CD

---

# Lộ trình tổng thể

## Giai đoạn 1 - Local Development

Windows 11

```text
Docker Desktop
├── Jenkins
├── Spring Boot
├── PostgreSQL
└── Nginx
```

Mục tiêu:

- Học Docker
- Học Docker Compose
- Học Jenkins Pipeline
- Hiểu luồng CI/CD

Checklist:

- [ ] Docker Desktop
- [ ] Git
- [ ] JDK 21
- [ ] Maven
- [ ] IntelliJ IDEA
- [ ] GitHub Account

---

# Giai đoạn 2 - Oracle Cloud Free Tier

## Mục tiêu

Tạo DEV Server Online miễn phí.

Kiến trúc:

```text
Laptop
  |
Git Push
  |
GitHub
  |
Webhook
  |
Jenkins
  |
Deploy
  |
Oracle Cloud VM
```

## Chuẩn bị

- Email cá nhân
- Số điện thoại
- Visa hoặc MasterCard
- GitHub Account

## Đăng ký OCI

1. Truy cập:

```text
https://www.oracle.com/cloud/free/
```

2. Chọn Start for Free

3. Chọn Home Region gần nhất.

Gợi ý:

```text
Singapore
Japan
```

4. Xác thực Email

5. Xác thực Số điện thoại

6. Xác thực Thẻ

7. Hoàn tất đăng ký

## Tạo VM

Khuyến nghị:

```text
Ubuntu 24.04 LTS
```

Tên VM:

```text
dev-server
```

Cấu hình:

```text
2 OCPU
12GB RAM
50GB Storage
```

## Generate SSH Key

Windows PowerShell:

```bash
ssh-keygen -t rsa -b 4096
```

Upload public key lên OCI.

## Mở Port

```text
22 SSH
80 HTTP
443 HTTPS
8080 Jenkins
8081 API (optional)
5432 PostgreSQL (không khuyến nghị public)
```

## SSH VM

```bash
ssh ubuntu@PUBLIC_IP
```

---

# Cài Docker trên OCI

```bash
sudo apt update
sudo apt install docker.io -y
```

Kiểm tra:

```bash
docker --version
```

---

# Cài Docker Compose

```bash
docker compose version
```

---

# Tạo thư mục dự án

```bash
sudo mkdir -p /opt/myproject
cd /opt/myproject
```

---

# Cài Jenkins

```bash
docker volume create jenkins_home
```

```bash
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```

---

# PostgreSQL

```yaml
services:
  postgres:
    image: postgres:17
```

Volume:

```text
postgres_data
```

---

# Spring Boot

Dockerfile:

```dockerfile
FROM eclipse-temurin:21-jre
COPY target/*.jar app.jar
ENTRYPOINT ["java","-jar","app.jar"]
```

---

# Nginx

```text
Internet
   |
Nginx
   |
Spring Boot
```

Config:

```nginx
server {
 listen 80;
 location / {
   proxy_pass http://api:8080;
 }
}
```

---

# Giai đoạn 3 - Jenkins CI/CD

Pipeline:

```text
GitHub
  |
Checkout
  |
Build
  |
Test
  |
Docker Build
  |
Deploy
```

Jenkinsfile:

```groovy
pipeline {
 agent any
 stages {
  stage('Build') {
   steps {
    sh 'mvn clean package'
   }
  }
 }
}
```

---

# Giai đoạn 4 - Domain & HTTPS

Domain miễn phí:

```text
duckdns.org
noip.com
```

Ví dụ:

```text
myproject.duckdns.org
```

HTTPS:

```bash
sudo apt install certbot -y
```

---

# Giai đoạn 5 - Enterprise DEV Environment

```text
Developer
    |
    v
GitHub
    |
    v
Jenkins
    |
    v
Oracle VM

├── Nginx
├── Spring Boot
├── PostgreSQL
└── Docker
```

---

# Cấu trúc thư mục DEV Server

```text
/opt/myproject
├── docker-compose.yml
├── nginx
├── logs
├── backups
└── postgres
```

---

# Checklist cuối cùng

Infrastructure

- [ ] Oracle Cloud Account
- [ ] Ubuntu VM
- [ ] SSH Key
- [ ] Docker
- [ ] Docker Compose

Application

- [ ] Spring Boot
- [ ] Dockerfile
- [ ] PostgreSQL

CI/CD

- [ ] Jenkins
- [ ] GitHub Webhook
- [ ] Pipeline

Networking

- [ ] Domain
- [ ] Nginx
- [ ] HTTPS

Monitoring

- [ ] Log Rotation
- [ ] Backup PostgreSQL
- [ ] Health Check

# Kết luận

Đích đến:

Laptop -> GitHub -> Jenkins -> Oracle Cloud DEV Server -> Nginx -> Spring Boot -> PostgreSQL
