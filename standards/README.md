# Standards

Thư mục này là bộ tiêu chuẩn kỹ thuật dùng chung cho toàn bộ hệ sinh thái restaurant-platform.
Mục tiêu là giúp các team FE, BE, QA, DevOps và vận hành làm việc nhất quán, giảm sai lệch khi mở rộng nhiều repository.

## Mục tiêu

- Chuẩn hóa cách thiết kế, triển khai và vận hành.
- Đồng bộ chất lượng giữa các repository.
- Đảm bảo khả năng scale-up mà không phá vỡ nền tảng hiện tại.
- Làm nguồn tham chiếu chính thức khi review PR, release và audit.

## Phạm vi áp dụng

- Áp dụng cho tất cả repository trong restaurant-platform.
- Bắt buộc với module nghiệp vụ chính, API contract, triển khai hạ tầng và vận hành.
- Khi có khác biệt giữa các tài liệu, ưu tiên tiêu chuẩn trong thư mục này trước khi cập nhật tài liệu domain-specific.

## Danh sách tiêu chuẩn

- [10-security.md](10-security.md): tiêu chuẩn bảo mật ứng dụng, auth, authorization, encryption, security controls.
- [11-devops.md](11-devops.md): chiến lược branch, CI/CD, Docker, môi trường và checklist triển khai.
- [12-testing.md](12-testing.md): chiến lược kiểm thử Unit, Integration, UAT, Performance và quality gates.
- [13-deployment.md](13-deployment.md): luồng deploy, rollback plan, release checklist, quy tắc theo từng môi trường.
- [14-monitoring.md](14-monitoring.md): chuẩn logs, metrics, alerts, dashboards, SLO và incident integration.

## Cách sử dụng

- Trước khi bắt đầu tính năng mới, đọc các tiêu chuẩn liên quan.
- Trong quá trình phát triển, dùng checklist trong từng file để tự kiểm.
- Trước khi merge hoặc release, dùng các mục KPI và quality gate để xác nhận sẵn sàng.
- Khi phát hiện thiếu chuẩn, cập nhật tài liệu theo PR riêng để mọi team cùng theo.

## Quy tắc cập nhật

- Mỗi thay đổi tiêu chuẩn phải có lý do rõ ràng và tác động dự kiến.
- Cập nhật đồng bộ giữa các file nếu có phụ thuộc chéo.
- Ưu tiên giữ tính đơn giản cho MVP nhưng không phá vỡ định hướng scale-up.
- Với thay đổi lớn, bổ sung mục chuyển tiếp để team áp dụng dần.

## Definition of Done cho Standards

- Nội dung có thể áp dụng trực tiếp vào triển khai.
- Có checklist hoặc tiêu chí đo lường rõ ràng.
- Không mâu thuẫn với tài liệu kiến trúc, API, database và governance hiện hành.
- Được review bởi đại diện kỹ thuật liên quan.

## Gợi ý luồng áp dụng thực tế

1. Thiết kế API và DB theo security + testing requirements.
2. Thiết lập pipeline theo devops standard.
3. Deploy theo deployment guide.
4. Theo dõi vận hành theo monitoring standard.
5. Định kỳ review KPI để cải tiến tiêu chuẩn.