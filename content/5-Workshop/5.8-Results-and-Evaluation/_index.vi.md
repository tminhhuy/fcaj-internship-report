---
title: "Kết quả và đánh giá"
date: 2026-09-07
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

SCA được triển khai bằng AWS CDK lên Region ap-southeast-1 ngày 25/11/2026 và gắn domain smartca.app cùng ngày. Buổi đánh giá tự động trên stack thật đạt 28/31 kiểm tra (90%); các tiêu chí còn lại được ghi rõ là chưa đo.

#### Evals trên stack thật (25/11/2026, bộ smoke)

| Hợp đồng mẫu | Đạt | Kiểm tra chính | Chưa đạt |
| --- | --- | --- | --- |
| Hợp đồng dịch vụ phần mềm tiếng Việt, đã ký | 15/17 | Không dữ liệu các bên nào tới model (11 token); tìm ra mức phạt vượt trần 8%, trích Luật Thương mại; tên thật khôi phục đúng trong báo cáo | Bỏ sót quyền sở hữu mã nguồn và dữ liệu cá nhân của khách hàng |
| Điều khoản bán lẻ online tiếng Việt, mẫu | 12/12 | Tìm ra loại trừ trách nhiệm (trích Luật Bảo vệ quyền lợi người tiêu dùng), chia sẻ dữ liệu cho bên quảng cáo (trích Luật Bảo vệ dữ liệu cá nhân), đổi điều khoản không báo trước; điểm 26 | — |
| Mẫu tiếng Việt có cài prompt injection | 1/2 | Nhận đúng ngôn ngữ | Job kết thúc `FAILED` (lỗi `INTERNAL`), chưa có báo cáo để chấm |
| **Tổng** | **28/31 (90%)** |  |  |

Ca prompt injection thất bại theo hướng an toàn (không ra báo cáo sai) nhưng vẫn là lỗi cần sửa trước khi công bố kết quả kiểm thử bảo mật.

#### Một lượt phân tích thật trên smartca.app (25/11/2026)

Hợp đồng hợp tác 6 trang (PDF có lớp chữ), luồng Loại 2: điểm **57** (cần chỉnh đáng kể), 17 phát hiện, 8 đề xuất sửa câu chữ, khuyến nghị đàm phán. Nhật ký kiểm toán ghi:

| Agent | Thời gian (giây) | Token vào / ra | Đọc từ cache | Lượt tra luật |
| --- | --- | --- | --- | --- |
| 1. Ngữ cảnh & vai trò | 7,5 | 9.617 / 945 | 0 | 0 |
| 2. Tuân thủ | 7,2 | 38.032 / 342 | 28.543 | 3 |
| 3. Tranh chấp & chế tài | 10,3 | 37.970 / 801 | 31.106 | 3 |
| 4. Ngôn ngữ & công bằng | 6,8 | 9.583 / 822 | 2.679 | 0 |
| 5. Đề xuất chỉnh sửa | 6,2 | 8.706 / 882 | 2.433 | 0 |
| 6. Tóm tắt | 9,7 | 17.621 / 1.246 | 4.721 | 0 |
| **Tổng** | **47,7** | **121.529 / 5.038** | **69.482 (57%)** | **6** |

Lượt chạy này không ghi nhận trích dẫn nào (0 được kiểm chứng, 0 bị loại) dù hai agent đã tra luật 6 lượt. Cần chạy lại sau khi nạp đủ 8 luật vào Knowledge Base ([5.7.2 bước 6]({{< relref "/5-Workshop/5.7-Deployment/5.7.2-Deploy-the-Stack" >}})) trước khi công bố số liệu về trích dẫn.

#### Đối chiếu tiêu chí thành công

| Tiêu chí | Ngưỡng đạt | Kết quả đến 27/11/2026 |
| --- | --- | --- |
| Thời gian xử lý | Hợp đồng 10 trang ≤ 5 phút (p95) | Chưa đo p95; sáu agent mất 47,7 giây với hợp đồng 6 trang |
| Rò rỉ định danh | 0 định danh tới agent trên 30 hợp đồng giả lập | Đạt trên hợp đồng đã ký của bộ smoke; bộ evals hiện có 6 hợp đồng, mục tiêu 30 |
| Trích dẫn | 100% trích dẫn truy được về Knowledge Base | Bảo đảm bằng thiết kế (VerifyCitations); chờ nạp kho luật để đo |
| Phát hiện lỗi | ≥ 80% lỗi cài sẵn | 28/31 kiểm tra (90%) trên 3 hợp đồng |
| OCR tiếng Việt | CER ≤ 2% trên 10 trang scan | Chưa đo |
| Bảo mật | 0 bucket public, 0 credential trong repo, cdk-nag sạch | Đạt (kiểm tra ngày 24/11) |
| Độ tin cậy | 50 hợp đồng cùng lúc, không mất job | Chưa chạy |
| Chi phí | Biến phí ≤ $0,35/hợp đồng | Ước tính $0,17; thực tế ≈ $0,10 tiền model cho hợp đồng 6 trang |
| Tái lập | Triển khai từ đầu bằng CDK | Đạt: dựng mới trên tài khoản khác ngày 25/11 |
