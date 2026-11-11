---
title: "Nhật ký công việc"
date: 2026-09-07
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

Nhật ký ghi lại quá trình học tập và làm việc của em trong **12 tuần** thực tập tại chương trình **First Cloud AI Journey**, từ **07/09/2026** đến **27/11/2026**.

Lộ trình chia làm ba giai đoạn:

* **Tuần 1–7: Nền tảng AWS.** Tài khoản và chi phí, mạng VPC, EC2 và Auto Scaling, S3 và CloudFront, bảo mật, cơ sở dữ liệu, giám sát; kết thúc bằng ôn tập và kiểm tra giữa kỳ.
* **Tuần 8–10: Kiến trúc hiện đại.** Serverless và hướng sự kiện, hạ tầng dưới dạng code và CI/CD, container và phân tích dữ liệu; song song khởi động dự án nhóm.
* **Tuần 11–12: Dự án.** Áp dụng Well-Architected, thiết kế và triển khai **AI Agent hỗ trợ kiểm tra và tóm tắt hợp đồng B2B/B2C** với Amazon Textract, Amazon Bedrock và AWS Lambda.

Mỗi tuần gồm: mục tiêu, sơ đồ kiến trúc bài thực hành, bảng công việc theo ngày kèm tài liệu, kiến thức nắm được, kết quả thực hành, lệnh tiêu biểu, khó khăn và cách giải quyết, kế hoạch tuần tiếp theo.

### Tổng quan 12 tuần

| Tuần | Thời gian | Chủ đề | Dịch vụ chính |
| --- | --- | --- | --- |
| [Tuần 1](1.1-week1/) | 07/09/2026 – 11/09/2026 | Làm quen FCAJ, nền tảng điện toán đám mây và thiết lập tài khoản AWS an toàn | IAM, AWS Budgets, AWS Support, AWS CLI |
| [Tuần 2](1.2-week2/) | 14/09/2026 – 18/09/2026 | Mạng trên AWS với Amazon VPC | VPC, Subnet, IGW, NAT Gateway, VPC Peering, Flow Logs |
| [Tuần 3](1.3-week3/) | 21/09/2026 – 25/09/2026 | Dịch vụ tính toán: Amazon EC2, Auto Scaling và Load Balancing | EC2, EBS, IAM Role, ALB, Auto Scaling, Lightsail |
| [Tuần 4](1.4-week4/) | 28/09/2026 – 02/10/2026 | Lưu trữ Amazon S3, phân phối nội dung với CloudFront và DNS với Route 53 | S3, CloudFront, Route 53, ACM, AWS Backup |
| [Tuần 5](1.5-week5/) | 05/10/2026 – 09/10/2026 | Bảo mật, định danh và tuân thủ trên AWS | IAM, KMS, Secrets Manager, Cognito, WAF, GuardDuty |
| [Tuần 6](1.6-week6/) | 12/10/2026 – 16/10/2026 | Cơ sở dữ liệu trên AWS: RDS, Aurora, DynamoDB và ElastiCache | RDS, Aurora, DynamoDB, ElastiCache, DMS |
| [Tuần 7](1.7-week7/) | 19/10/2026 – 23/10/2026 | Giám sát, vận hành hệ thống và ôn tập giữa kỳ | CloudWatch, CloudTrail, AWS Config, Systems Manager |
| [Tuần 8](1.8-week8/) | 26/10/2026 – 30/10/2026 | Kiến trúc serverless, hướng sự kiện và khởi động dự án nhóm | Lambda, API Gateway, SQS, SNS, EventBridge, Step Functions |
| [Tuần 9](1.9-week9/) | 02/11/2026 – 06/11/2026 | Hạ tầng dưới dạng code, CI/CD và phân tích yêu cầu dự án | CloudFormation, CDK, CodePipeline, CodeBuild |
| [Tuần 10](1.10-week10/) | 09/11/2026 – 13/11/2026 | Container, phân tích dữ liệu và thử nghiệm OCR hợp đồng | ECS, Fargate, ECR, Kinesis, Glue, Athena, Textract |
| [Tuần 11](1.11-week11/) | 16/11/2026 – 20/11/2026 | AWS Well-Architected, Amazon Bedrock và thiết kế kiến trúc dự án | Well-Architected, Bedrock, Knowledge Bases, Guardrails |
| [Tuần 12](1.12-week12/) | 23/11/2026 – 27/11/2026 | Triển khai dự án, bảo mật, giám sát và tổng kết thực tập | CDK, Step Functions, Textract, Bedrock, WAF, CloudWatch |

### Chi tiết từng tuần

**Tuần 1:** [Làm quen FCAJ, nền tảng điện toán đám mây và thiết lập tài khoản AWS an toàn](1.1-week1/)

**Tuần 2:** [Mạng trên AWS với Amazon VPC](1.2-week2/)

**Tuần 3:** [Dịch vụ tính toán: Amazon EC2, Auto Scaling và Load Balancing](1.3-week3/)

**Tuần 4:** [Lưu trữ Amazon S3, phân phối nội dung với CloudFront và DNS với Route 53](1.4-week4/)

**Tuần 5:** [Bảo mật, định danh và tuân thủ trên AWS](1.5-week5/)

**Tuần 6:** [Cơ sở dữ liệu trên AWS: RDS, Aurora, DynamoDB và ElastiCache](1.6-week6/)

**Tuần 7:** [Giám sát, vận hành hệ thống và ôn tập giữa kỳ](1.7-week7/)

**Tuần 8:** [Kiến trúc serverless, hướng sự kiện và khởi động dự án nhóm](1.8-week8/)

**Tuần 9:** [Hạ tầng dưới dạng code, CI/CD và phân tích yêu cầu dự án](1.9-week9/)

**Tuần 10:** [Container, phân tích dữ liệu và thử nghiệm OCR hợp đồng](1.10-week10/)

**Tuần 11:** [AWS Well-Architected, Amazon Bedrock và thiết kế kiến trúc dự án](1.11-week11/)

**Tuần 12:** [Triển khai dự án, bảo mật, giám sát và tổng kết thực tập](1.12-week12/)
