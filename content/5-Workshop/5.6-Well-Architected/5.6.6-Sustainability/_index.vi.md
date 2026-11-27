---
title: "Phát triển bền vững (Sustainability)"
date: 2026-09-07
weight: 6
chapter: false
pre: " <b> 5.6.6. </b> "
---

Trụ cột này được ghi nhận trong checklist và không tính điểm; SCA đáp ứng bằng các lựa chọn đã có ở các trụ cột khác.

| Thực hành | SCA áp dụng |
| --- | --- |
| Không có tài nguyên chạy không tải | Serverless toàn bộ: không máy chủ nào chạy khi không có việc |
| Phần cứng hiệu quả | Lambda arm64 (Graviton) thay cho x86 |
| Không huấn luyện thừa | Không huấn luyện hay fine-tune model; tri thức cập nhật bằng RAG |
| Giảm xử lý lặp | Prompt caching giảm lượng token phải xử lý lại (57% ở lượt chạy thật) |
| Vòng đời dữ liệu | S3 Lifecycle xóa dữ liệu tạm và chuyển báo cáo cũ sang lớp lưu trữ rẻ hơn |
| Môi trường vừa đủ | Môi trường dev đặt endpoint 1 AZ và được gỡ bằng `cdk destroy` sau demo |
| Chưa làm | Thử Amazon Nova Lite cho các agent nhẹ để dùng model nhỏ hơn khi đủ chất lượng |
