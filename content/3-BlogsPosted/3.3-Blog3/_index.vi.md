---
title: "Blog 3"
date: 2026-09-07
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Top 16 dịch vụ AWS mà kỹ sư nên biết

Hiểu AWS không chỉ là ghi nhớ tên dịch vụ, mà là hiểu vai trò của từng dịch vụ trong kiến trúc hệ thống.

### Tính toán (Compute)

* **Lambda:** chạy hàm hướng sự kiện, tự động mở rộng.
* **ECS:** điều phối container bằng công cụ gốc của AWS.
* **EKS:** cung cấp control plane Kubernetes được quản lý.

### Dữ liệu (Data)

* **RDS:** quản lý cơ sở dữ liệu quan hệ.
* **DynamoDB:** lưu trữ key-value và document dạng serverless.
* **ElastiCache:** bộ nhớ đệm in-memory được quản lý, cho truy cập độ trễ thấp.

### Tích hợp ứng dụng (Application integration)

* **SQS:** hàng đợi bền vững cho tác vụ bất đồng bộ.
* **SNS:** nhắn tin pub/sub và phân phối thông báo.
* **EventBridge:** định tuyến sự kiện giữa ứng dụng và dịch vụ.
* **Step Functions:** điều phối workflow với cơ chế thử lại, chờ và rẽ nhánh.

### Mạng và phân phối (Networking & delivery)

* **API Gateway:** quản lý API với xác thực và điều tiết lưu lượng (throttling).
* **CloudFront:** cache nội dung gần người dùng hơn.
* **VPC:** mạng ảo biệt lập cho tài nguyên trên cloud.

### Bảo mật và vận hành (Security & operations)

* **IAM:** quản lý quyền truy cập qua policy và role.
* **CloudWatch:** thu thập metrics, log và dữ liệu vận hành.
* **Secrets Manager:** lưu trữ và xoay vòng thông tin xác thực của ứng dụng.

### Tích hợp AI

Xây dựng hạ tầng có khả năng mở rộng mới chỉ là một phần của thách thức. Khi tích hợp AI vào sản phẩm, doanh nghiệp còn cần kết nối mô hình với dữ liệu sẵn có, quản lý khối lượng công việc AI và đảm bảo bảo mật, độ tin cậy trong môi trường production.

AWS hỗ trợ điều này qua **Amazon Bedrock** để xây dựng ứng dụng AI tạo sinh và **Amazon Bedrock AgentCore** để triển khai, vận hành các AI agent ở quy mô lớn, giúp doanh nghiệp đi từ nguyên mẫu đến sản phẩm hoàn chỉnh và mở rộng tính năng AI cho nhiều khách hàng.

Nguồn : https://www.facebook.com/groups/awsstudygroupfcj/permalink/2326006894830936/?rdid=dndxkWjCrAPSt9rI&share_url=https%3A%2F%2Fwww.facebook.com%2Fshare%2Fv%2F1FXDe3eHC5%2F# 