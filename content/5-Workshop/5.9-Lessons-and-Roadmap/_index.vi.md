---
title: "Bài học, rủi ro và lộ trình"
date: 2026-09-07
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

#### Bài học kinh nghiệm và xử lý sự cố

Sự cố lớn nhất không nằm trong code mà ở quyền truy cập model: tài khoản AWS ban đầu không được cấp quyền Amazon Bedrock, buộc nhóm đổi model và tài khoản triển khai 4 ngày trước hạn.

| Vấn đề | Nguyên nhân | Cách xử lý | Bài học |
| --- | --- | --- | --- |
| Không gọi được Bedrock: form use case của Anthropic báo "not authorized", sau đó "account is being verified" và "Operation not allowed" (20–23/11) | Tài khoản mới, chưa có lịch sử thanh toán; AWS Support từ chối mở Bedrock cho toàn tài khoản | Triển khai sang tài khoản AWS của thành viên nhóm đã gọi được Nova Pro; đổi model từ Claude Sonnet 5 sang Amazon Nova Pro (24/11) | Kiểm tra quyền model và quota ngay tuần đầu; giữ model ID trong cấu hình để đổi không cần sửa logic |
| Nova Pro báo "doesn't support on-demand inference" ở Singapore | Singapore không có Nova Pro in-Region | Dùng inference profile `apac.amazon.nova-pro-v1:0`; IAM cấp quyền cả ARN profile lẫn foundation model ở mọi Region | Đọc model card (Region, kiểu inference) trước khi chọn model |
| Lời gọi model hỏng sau khi đổi sang Nova | Tham số `thinking` chỉ dành cho Anthropic; Nova chỉ nhận một tập con JSON Schema cho tool và cache tối đa 20.000 token; OCR cũ dùng định dạng InvokeModel riêng của Anthropic | Bỏ `thinking`; viết schema tool theo tập con Nova nhận; tự bỏ điểm cache khi bị từ chối; viết lại OCR bằng Converse API (document block cho PDF, image block cho ảnh) | Dùng Converse API cho mọi lời gọi để code ít phụ thuộc model; có test giả lập Bedrock |
| Stack mới không có trích dẫn nào | Knowledge Base rỗng cho đến khi nạp kho luật | Đưa việc nạp 8 luật thành một bước triển khai bắt buộc ([5.7.2]({{< relref "/5-Workshop/5.7-Deployment/5.7.2-Deploy-the-Stack" >}})) | Dữ liệu là một phần của hệ thống: kiểm tra nó như kiểm tra hạ tầng |
| law-harvest nhận HTTP 403 từ vbpl.vn cho cả 5 luật (25/11) | vbpl.vn chặn yêu cầu từ AWS và đã chuyển sang site mới, toàn văn nằm trong script của trang | Không vượt chặn; tải toàn văn bằng trình duyệt, dựng tay cả 8 luật (2.024 điều) và triển khai cùng stack | Nguồn dữ liệu bên ngoài có thể chặn cloud; luôn có đường nạp thủ công có kiểm soát |
| Đăng nhập báo "Unsupported sign-in step: CONTINUE\_SIGN\_IN\_WITH\_TOTP\_SETUP" (25/11) | User pool bắt buộc MFA nhưng trang đăng nhập chưa có bước cài ứng dụng xác thực | Thêm bước quét mã QR và nhập mã TOTP ngay trên trang đăng nhập; phiên đăng nhập kéo dài 10 phút | Thử luồng của người dùng mới trên stack thật, không chỉ với tài khoản đã cài sẵn |
| `cdk deploy` trên máy Mac báo `ModuleNotFoundError: aws_cdk` | CDK gọi `python3 app.py` bằng bản Python khác của máy, không phải môi trường ảo | Chỉ rõ `--app ".venv/bin/python3 app.py"` | Ghi phiên bản công cụ và môi trường trong hướng dẫn triển khai |
| Gắn domain smartca.app | CloudFront chỉ nhận chứng chỉ ACM ở us-east-1; đuôi `.app` bắt buộc HTTPS | Xin chứng chỉ ACM miễn phí, xác minh bằng bản ghi CNAME, khai `domainName` và `certificateArn`, trỏ bản ghi gốc về CloudFront | Domain đặt qua cấu hình, không sửa code |
| Thông tin nhạy cảm bị lộ trong lúc làm: một GitHub token dán vào khung chat, mã QR xác thực hai bước lọt vào video demo | Thao tác vội trong lúc triển khai và quay video | Tạo lại token (fine-grained, chỉ đọc, hết hạn 7 ngày); làm mờ đoạn video có mã QR | Secret chỉ dán vào nơi cần dùng; rà lại video và ảnh chụp trước khi chia sẻ |

#### Rủi ro

Ba rủi ro lớn nhất là nhận định pháp lý sai, lọt thông tin định danh và quy định về chuyển dữ liệu cá nhân ra nước ngoài; mỗi rủi ro có ít nhất hai lớp giảm thiểu.

| Rủi ro | Khả năng | Tác động | Giảm thiểu |
| --- | --- | --- | --- |
| Nhận định pháp lý sai hoặc dựa trên văn bản hết hiệu lực | Trung bình | Cao | Chỉ kết luận khi trích dẫn kiểm chứng được; trạng thái hiệu lực trong metadata; con người rà soát; báo cáo luôn ghi không thay thế luật sư |
| Lọt thông tin định danh | Trung bình | Cao | Quy tắc + regex, rồi Guardrails quét lại; LeakCheck chặn hẳn bản còn sót |
| Chuyển dữ liệu cá nhân ra nước ngoài (profile APAC, đặc biệt bản scan tiếng Việt) | Cao khi triển khai thật | Cao | Thu thập tối thiểu; doanh nghiệp lập hồ sơ đánh giá tác động; giai đoạn sau token hóa ngay tại máy người dùng |
| Prompt injection trong hợp đồng | Trung bình | Cao | Bộ lọc prompt attack; thẻ dữ liệu; điểm tính từ JSON đã kiểm tra; sửa lỗi ca evals ngày 25/11 |
| OCR tiếng Việt sai dấu trên bản scan kém | Trung bình | Trung bình | Ưu tiên lớp chữ của PDF; đánh dấu trang chất lượng thấp cho người dùng kiểm tra |
| Quota cross-Region của Nova Pro thấp | Trung bình | Trung bình | SQS + giới hạn đồng thời + retry có backoff; xin tăng quota |
| Chi phí vượt dự kiến | Trung bình | Trung bình | AWS Budgets 50%/80%; giới hạn 50 trang và token đầu ra; chi phí mỗi hợp đồng trên dashboard |
| Model thay đổi hoặc ngừng hỗ trợ | Thấp | Trung bình | Model ID trong cấu hình; evals trong pipeline phát hiện giảm chất lượng; đã đổi model một lần thành công |

**Giới hạn hiện tại:** chỉ trích pháp luật Việt Nam (8 luật); hai ngành; một doanh nghiệp demo; chưa so sánh phiên bản hợp đồng; số liệu hiệu năng và OCR chưa đo đủ ([5.8]({{< relref "/5-Workshop/5.8-Results-and-Evaluation" >}})).

#### Việc còn mở

- Nạp 8 luật vào Knowledge Base, chạy lại lượt phân tích và câu hỏi chuẩn.
- Sửa lỗi ca evals prompt injection, chạy lại bộ smoke.
- Đo p95 thời gian xử lý, chạy bài tải 50 hợp đồng và đo CER trên 10 trang scan tiếng Việt.
- Pentest theo chính sách của AWS (không thử tấn công DoS), có sự đồng ý của chủ tài khoản.
- Nhóm tự rà soát và chỉnh Hình 1–5 trong draw.io.

#### Sau MVP

- Agent tra luật nước ngoài khi hợp đồng chọn luật nước ngoài.
- Đưa khung điều khoản C01–C20 vào agent ngữ cảnh.
- Định tuyến các agent nhẹ sang Amazon Nova Lite.
- So sánh phiên bản hợp đồng.
- Mỗi doanh nghiệp một tài khoản AWS.
- Token hóa tại trình duyệt trước khi tải lên.
