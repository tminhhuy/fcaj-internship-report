---
title: "Độ tin cậy (Reliability)"
date: 2026-09-07
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---

Điểm dễ hỏng nhất là quota token của Amazon Bedrock, nên hệ thống dồn tải vào hàng đợi thay vì đánh rơi job, và mọi lỗi đều để lại trạng thái rõ ràng cùng cảnh báo.

| Tình huống lỗi | Hệ thống xử lý | Khôi phục |
| --- | --- | --- |
| Nhiều hợp đồng tải lên cùng lúc | SQS nhận mọi job; `starter` chỉ khởi tạo khi đang có dưới 5 execution, đủ 5 thì job thành `QUEUED` và message được gửi lại sau 60 giây | Tự động, không job nào bị bỏ |
| Bedrock hoặc Textract bị throttle, lỗi tạm thời | Retry sau 2 giây, nhân đôi mỗi lần, tối đa 6 lần, full jitter | Tự động; alarm khi throttle kéo dài |
| Lỗi không tự hết | MarkFailed ghi `FAILED` và mã lỗi vào DynamoDB; alarm ExecutionsFailed | Sửa lỗi rồi người dùng tải lại |
| Message không khởi tạo được workflow | Sau 3 lần nhận vào DLQ, giữ 14 ngày; alarm uploads-dlq | Redrive về hàng đợi chính |
| Sự kiện S3 lặp, người dùng tải lại cùng file | Khóa idempotency (SHA-256 của file + mã doanh nghiệp) ghi có điều kiện; execution mang tên job nên không chạy hai lần | Job mới thành `DUPLICATE`, trỏ tới báo cáo cũ |
| Agent chạy quá lâu | Lambda agent timeout 240 giây; execution tối đa 15 phút; alarm ExecutionsTimedOut | Retry hoặc MarkFailed |
| Model trả JSON sai schema | Gửi lại để sửa tối đa 2 lần | Sau đó MarkFailed |
| Một AZ gặp sự cố | Lambda và interface endpoint đặt ở 2 AZ; S3, DynamoDB, SQS, Step Functions là dịch vụ regional đa AZ | Tự động (stage dev chỉ đặt endpoint 1 AZ để tiết kiệm) |
| Mất hoặc ghi nhầm dữ liệu | DynamoDB bật point-in-time recovery; bản gốc trong `vault` khóa bằng Object Lock | Khôi phục bảng theo thời điểm; bản gốc không sửa được |
| Mất cả môi trường | Toàn bộ hạ tầng là code | Dựng lại bằng `cdk deploy`, khoảng 30 phút |

#### Vì sao không dùng reserved concurrency

Không dùng reserved concurrency để giới hạn tải: `StartExecution` trả về ngay nên giới hạn số Lambda không giới hạn được số execution, và tài khoản mới chỉ có quota 10 concurrency cho Lambda. Việc chờ chỗ trống không tính là lỗi: message gửi lại bắt đầu lại từ lần nhận thứ nhất.

#### Còn phải làm

- Chạy bài tải 50 hợp đồng trong 1 phút, xác nhận hàng đợi rút hết và DLQ có 0 message.
