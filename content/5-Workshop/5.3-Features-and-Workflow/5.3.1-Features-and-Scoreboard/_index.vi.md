---
title: "Tính năng và Scoreboard"
date: 2026-09-07
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

#### Tính năng

Bảng dưới đi theo thứ tự người dùng gặp trong web app.

| Tính năng | Người dùng thấy | Hệ thống làm phía sau |
| --- | --- | --- |
| Đăng nhập an toàn | Đăng ký bằng email, xác minh email, bật xác thực hai bước bằng ứng dụng authenticator | Amazon Cognito bắt buộc MFA (TOTP); mã doanh nghiệp nằm trong token, API không nhận từ request |
| Tải hợp đồng | Chọn loại (đã ký hay mẫu), hồ sơ ngành, luật áp dụng, ngôn ngữ hợp đồng và báo cáo, bên của mình; dấu vân tay SHA-256 tính ngay trong trình duyệt | PDF, DOCX, PNG, JPG, TXT tối đa 10 MB và 50 trang; trình duyệt tải thẳng lên S3 bằng presigned POST, file không đi qua Lambda |
| OCR Scan | Đọc được cả PDF có chữ, PDF scan và ảnh chụp | Lớp chữ của PDF trước; bản scan tiếng Anh qua Amazon Textract; bản scan tiếng Việt qua Amazon Nova Pro vision, 4 trang song song |
| Bảo vệ dữ liệu | Tab Quyền riêng tư: loại định danh đã thay bằng token, nơi dữ liệu nằm và ngày tự xóa | Phát hiện chữ ký và dữ liệu cá nhân; bản gốc vào kho khóa chỉ-đọc; bản sao được token hóa và qua LeakCheck |
| Sáu agent rà soát | Tab Phát hiện: mỗi phát hiện có mức độ, điều khoản, câu trích nguyên văn, căn cứ pháp lý và việc nên làm | Amazon Nova Pro qua Converse API, mỗi agent một prompt và một JSON schema ([5.3.3]({{< relref "/5-Workshop/5.3-Features-and-Workflow/5.3.3-Six-Agents" >}})) |
| Scoreboard | Thang điểm 0–100 và điểm từng tiêu chí | Code tính từ các phát hiện đã kiểm tra (bảng dưới) |
| Smart Summary | Một trang: các bên, đối tượng, giá trị, thời hạn, rủi ro lớn nhất, điều khoản trọng yếu, điều nên đàm phán trước, khuyến nghị | Agent 6 tổng hợp từ kết quả agent 1–5; khuyến nghị phải khớp với mức điểm |
| Cite/Source Link | Tên văn bản, điều, trạng thái hiệu lực, link văn bản gốc; nhãn "chưa đủ căn cứ" khi không kiểm chứng được | VerifyCitations tìm lại từng trích dẫn trong Knowledge Base ([5.4]({{< relref "/5-Workshop/5.4-Legal-Retrieval" >}})) |
| Đề xuất sửa câu chữ | Câu hiện tại đặt cạnh câu đề xuất, nút sao chép | Agent 5; hệ thống không ghi vào hợp đồng |
| Theo dõi trực tiếp | Tiến độ 8 bước; email khi báo cáo sẵn sàng | Trạng thái job trong DynamoDB; Amazon SNS gửi email |
| Dấu vết kiểm toán | Model, Region, guardrail, Knowledge Base, cách đọc chữ, số trang, số trích dẫn; thời gian, token, cache và lượt tra luật của từng agent | Ghi trong báo cáo JSON cùng mỗi lần chạy |
| Thư viện luật và hồ sơ ngành | Xem 114 văn bản theo khu vực, nhóm luật, tình trạng; chỉnh điều khoản bắt buộc, mức chấp nhận rủi ro và trọng số theo ngành | Catalog trong code; hồ sơ ngành lưu trong DynamoDB qua `PUT /api/profiles/{code}` |
| Xuất và xóa | Tải JSON, PDF; nút Xóa dữ liệu | API trả báo cáo từ bucket `reports`; xóa bản làm việc, bảng token và báo cáo |
| Song ngữ | Giao diện và báo cáo tiếng Việt hoặc tiếng Anh | Ngôn ngữ báo cáo là tham số của mỗi job |

#### Scoreboard

Trọng số mặc định, doanh nghiệp chỉnh được theo ngành:

| Tiêu chí | Trọng số | Agent chấm |
| --- | --- | --- |
| Tuân thủ pháp luật | 30 | Agent 2 |
| Ngữ cảnh ngành và vai trò các bên | 20 | Agent 1 |
| Tranh chấp, phạt vi phạm, thẩm quyền | 20 | Agent 3 |
| Cân bằng nghĩa vụ bên A / bên B | 15 | Agent 1 |
| Ngôn ngữ, độ rõ ràng, tính song phương | 15 | Agent 4 |

Mỗi tiêu chí bắt đầu từ 100 và bị trừ theo mức độ phát hiện: **nghiêm trọng 45, cao 25, trung bình 10, thấp 3**; phát hiện "chưa đủ căn cứ" chỉ bị trừ nửa. Tổng là trung bình có trọng số:

| Tổng điểm | Ý nghĩa |
| --- | --- |
| Từ 85 | Sẵn sàng trình ký |
| 70–84 | Cần chỉnh nhỏ |
| 50–69 | Cần chỉnh đáng kể |
| Dưới 50 | Không nên ký khi chưa sửa |

**Quy tắc chặn:** có một điều khoản trái luật đã kiểm chứng thì tổng không vượt 69, để lỗi nghiêm trọng không bị điểm trung bình che đi.
