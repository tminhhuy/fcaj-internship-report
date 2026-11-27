---
title: "Workshop"
date: 2026-09-07
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# SCA: Smart Contract Analyzer for Everyone trên AWS

#### Tổng quan

**SCA (Smart Contract Analyzer)** là hệ thống rà soát hợp đồng bằng AI chạy hoàn toàn trên AWS, hoạt động tại [smartca.app](https://smartca.app) từ 25/11/2026. Sáu agent trên **Amazon Nova Pro** đọc hợp đồng tiếng Việt, tiếng Anh hoặc song ngữ, chấm điểm 0–100 và chỉ giữ trích dẫn khi kho luật tìm lại được đúng điều luật. Hệ thống chỉ tư vấn, không bao giờ sửa hợp đồng, và thay thông tin định danh bằng token trước khi AI đọc.

Workshop này đi qua toàn bộ giải pháp: bài toán và các nghiên cứu làm nền tảng, kiến trúc, các trụ cột AWS Well-Architected, dự toán chi phí, triển khai, kiểm thử, kết quả và dọn dẹp tài nguyên.

Bạn sẽ thấy cách:

+ Chạy một **luồng xử lý tất định, hướng sự kiện** bằng Amazon EventBridge, Amazon SQS và AWS Step Functions, mỗi hợp đồng một execution.
+ Bảo vệ dữ liệu cá nhân bằng **hai bản hợp đồng**: bản gốc đã ký khóa chỉ-đọc và bản làm việc đã token hóa, phải qua bước kiểm tra rò rỉ trước khi agent nào được chạy.
+ Neo nhận định pháp lý vào **Amazon Bedrock Knowledge Bases trên Amazon S3 Vectors**, rồi kiểm chứng lại từng trích dẫn trước khi đưa vào báo cáo.
+ Đặt các hàm xử lý trong **subnet private không có đường ra Internet**, gọi Amazon Textract và Amazon Bedrock qua VPC endpoint (AWS PrivateLink).
+ Triển khai toàn bộ dạng code bằng **AWS CDK**, giám sát bằng Amazon CloudWatch và dự toán bằng AWS Pricing Calculator.

#### Nội dung

1. [Tổng quan workshop]({{< relref "/5-Workshop/5.1-Overview" >}})
2. [Bối cảnh và nguyên tắc thiết kế]({{< relref "/5-Workshop/5.2-Context-and-Principles" >}})
3. [Tính năng và cách vận hành]({{< relref "/5-Workshop/5.3-Features-and-Workflow" >}})
4. [Cấu trúc truy xuất luật]({{< relref "/5-Workshop/5.4-Legal-Retrieval" >}})
5. [Kiến trúc trên AWS]({{< relref "/5-Workshop/5.5-AWS-Architecture" >}})
6. [Các trụ cột AWS Well-Architected]({{< relref "/5-Workshop/5.6-Well-Architected" >}})
7. [Triển khai và kiểm thử]({{< relref "/5-Workshop/5.7-Deployment" >}})
8. [Kết quả và đánh giá]({{< relref "/5-Workshop/5.8-Results-and-Evaluation" >}})
9. [Bài học, rủi ro và lộ trình]({{< relref "/5-Workshop/5.9-Lessons-and-Roadmap" >}})
10. [Dọn dẹp tài nguyên]({{< relref "/5-Workshop/5.10-Clean-Up" >}})
11. [Nhóm thực hiện và tài liệu tham khảo]({{< relref "/5-Workshop/5.11-Team-and-References" >}})
