---
title: "Điều kiện tiên quyết"
date: 2026-09-07
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

| Yêu cầu | Chi tiết |
| --- | --- |
| Tài khoản AWS | Quyền quản trị cho lần triển khai đầu; Region ap-southeast-1 |
| Amazon Bedrock | Gọi được Amazon Nova Pro qua `apac.amazon.nova-pro-v1:0` và Cohere Embed Multilingual v3 |
| Công cụ | AWS CLI v2 (đăng nhập bằng IAM Identity Center hoặc `aws login`, không dùng access key trong file), Node.js 22, Python 3.11 trở lên, Git, jq |
| Domain riêng (tùy chọn) | Chứng chỉ ACM ở us-east-1, xác minh bằng DNS |

{{% notice warning %}}
Kiểm tra quyền gọi model trước mọi việc khác. Trong dự án này, tài khoản AWS ban đầu không được cấp quyền Amazon Bedrock, buộc nhóm đổi tài khoản và model 4 ngày trước hạn (xem [5.9 Bài học kinh nghiệm]({{< relref "/5-Workshop/5.9-Lessons-and-Roadmap" >}})).
{{% /notice %}}

#### Lưu ý bảo mật

- Không tạo access key dài hạn cho việc triển khai; đăng nhập bằng IAM Identity Center hoặc `aws login`.
- Không dán token, mật khẩu hay mã MFA vào khung chat, ảnh chụp hay video.
- Không để secret trong `infra/cdk.json`: file này chỉ chứa cấu hình như model ID, ngân sách và domain.
