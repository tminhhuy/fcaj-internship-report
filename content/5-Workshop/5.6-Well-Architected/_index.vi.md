---
title: "Các trụ cột AWS Well-Architected"
date: 2026-09-07
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Phần này đánh giá SCA theo sáu trụ cột của AWS Well-Architected Framework. Mỗi trang nêu quyết định thiết kế, bằng chứng trên hệ thống đang chạy và phần còn thiếu.

| Trụ cột | Cơ chế chính trong SCA |
| --- | --- |
| [Bảo mật (Security)]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.1-Security" >}}) | MFA, IAM mỗi hàm một role, subnet private không NAT, VPC endpoint, KMS, token hóa + LeakCheck, Guardrails, WAF, không credential trong repo |
| [Độ tin cậy (Reliability)]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.2-Reliability" >}}) | SQS + DLQ, giới hạn đồng thời, retry có backoff, idempotency, multi-AZ, PITR, dựng lại bằng CDK |
| [Hiệu năng (Performance Efficiency)]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.3-Performance-Efficiency" >}}) | Serverless, Graviton, chạy song song có giới hạn, prompt caching (57%), CloudFront |
| [Vận hành xuất sắc (Operational Excellence)]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.4-Operational-Excellence" >}}) | Hạ tầng dạng code, pipeline có kiểm tra bảo mật, evals và duyệt tay; lane cập nhật luật có người duyệt; 9 alarm, dashboard, runbook |
| [Tối ưu chi phí (Cost Optimization)]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.5-Cost-Optimization" >}}) | Dự toán AWS Pricing Calculator theo từng dịch vụ, giả định tải, 8 biện pháp tối ưu, so với chi phí thực tế |
| [Phát triển bền vững (Sustainability)]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.6-Sustainability" >}}) | Serverless, Graviton, không fine-tune, prompt caching, lifecycle |
