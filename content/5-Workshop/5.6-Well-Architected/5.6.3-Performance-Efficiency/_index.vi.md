---
title: "Hiệu năng (Performance Efficiency)"
date: 2026-09-07
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---

Mọi thành phần tự scale theo lượt dùng; giới hạn thật sự là quota token của Amazon Bedrock, nên hiệu năng đến từ chạy song song có kiểm soát và đọc lại prompt từ cache.

| Lớp | Lựa chọn | Hiệu quả |
| --- | --- | --- |
| Tính toán | Lambda arm64 (Graviton), bộ nhớ theo việc: `api` 512 MB; `ocr-extract`, `tokenize-guard`, `agent-runner` 1.024 MB; `cite-score-report` 1.536 MB (dựng PDF) | Scale tự động, không máy chủ chạy không tải |
| Song song | 3 agent phân tích chạy cùng lúc (Parallel); OCR tiếng Việt 4 trang cùng lúc (Map); tối đa 5 hợp đồng cùng lúc (tham số `maxConcurrent`) | Rút ngắn thời gian mỗi hợp đồng mà không vượt quota |
| Model | Amazon Nova Pro, greedy decoding, giới hạn 4.000–6.000 token đầu ra mỗi agent, tự nâng 1,5 lần khi bị cắt (trần 10.000); prompt caching cho system prompt và hợp đồng | Lượt chạy thật: 57% token đầu vào đọc từ cache, mỗi agent 6,2–10,3 giây |
| Đường dữ liệu | Trình duyệt tải thẳng lên S3 bằng presigned POST; giữa các bước chỉ truyền khóa S3 | Không vướng giới hạn payload của API Gateway, Lambda và Step Functions |
| Phân phối nội dung | CloudFront: cache tối ưu cho trang tĩnh, không cache cho `/api/*`; Price Class 200 (có edge tại Việt Nam và Singapore) | Trang tải nhanh cho người dùng Việt Nam |
| Truy xuất | Lọc metadata ngay trong truy vấn S3 Vectors; đơn vị lưu trữ là một điều luật | Mỗi lượt `search_law` trả đúng 5 đoạn ngắn, giữ prompt nhỏ |
| Dữ liệu | DynamoDB on-demand; S3 Standard, chuyển Standard-IA cho báo cáo sau 30 ngày | Không phải ước lượng dung lượng trước |

#### Đo hiệu năng

Hiệu năng được đo liên tục trên dashboard CloudWatch: p50 và p95 thời gian xử lý, token theo agent, throttle của Bedrock. Mục tiêu là **p95 dưới 5 phút cho hợp đồng 10 trang**; vượt ngưỡng thì alarm.

Khi cần tăng thông lượng:

1. Xin tăng quota cross-Region của Nova Pro rồi nâng `maxConcurrent`.
2. Chuyển agent ngôn ngữ và tóm tắt sang Amazon Nova Lite sau khi evals xác nhận chất lượng không giảm.

#### Còn phải làm

- Đo p95 thời gian xử lý trên stack thật. Bằng chứng hiện có là một lượt chạy: sáu agent mất 47,7 giây với hợp đồng 6 trang.
