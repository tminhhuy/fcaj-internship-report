---
title: "Bài toán và người dùng mục tiêu"
date: 2026-09-07
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---

#### Bài toán của doanh nghiệp

- **Đọc nhiều vòng.** Chuyên viên, quản lý và người ký đều đọc toàn văn vì không có bản tóm tắt đáng tin.
- **Hai khung pháp lý khác nhau.** Hợp đồng B2B chịu Luật Thương mại 2005; hợp đồng B2C chịu thêm Luật Bảo vệ quyền lợi người tiêu dùng 2023. Dùng nhầm mẫu là lỗi phổ biến.
- **Rò rỉ dữ liệu cá nhân.** Nhân viên dán hợp đồng chứa họ tên, CCCD, số tài khoản vào chatbot công cộng để tiết kiệm thời gian.
- **Luật thay đổi.** Luật Bảo vệ dữ liệu cá nhân 2025 có hiệu lực từ 01/01/2026; hệ thống ngành kinh tế mới (Quyết định 36/2025/QĐ-TTg) có hiệu lực từ 15/11/2025.

#### Người dùng mục tiêu

| Người dùng | Nhu cầu | SCA hỗ trợ |
| --- | --- | --- |
| Chuyên viên pháp chế | Rà soát nhanh nhiều hợp đồng, không sót điều khoản rủi ro | Phát hiện xếp theo mức độ, mỗi phát hiện có trích dẫn điều luật |
| Quản lý, người ký | Nắm hợp đồng trong vài phút để quyết định | Smart Summary một trang và Scoreboard |
| Doanh nghiệp nhỏ chưa có pháp chế | Góc nhìn pháp lý cơ bản trước khi ký | Cảnh báo, khuyến nghị và lời nhắc hỏi luật sư khi rủi ro cao |
| Quản trị IT, bảo mật | Kiểm soát nơi dữ liệu nằm và ai truy cập | Dữ liệu trong tài khoản AWS của doanh nghiệp, mã hóa AWS KMS, nhật ký AWS CloudTrail |

#### Tình huống cần phát hiện

| Tình huống | Căn cứ | SCA xử lý |
| --- | --- | --- |
| Hợp đồng gia công phần mềm (B2B) phạt vi phạm 15% giá trị hợp đồng | Điều 301 Luật Thương mại 2005: mức phạt không vượt 8% giá trị phần nghĩa vụ bị vi phạm | Cảnh báo mức cao, trích Điều 301, gợi ý giảm về tối đa 8% |
| Điều kiện giao dịch chung của shop online (B2C) loại trừ trách nhiệm người bán | Điều 25 Luật Bảo vệ quyền lợi người tiêu dùng 2023 | Đánh dấu điều khoản, nêu khoản tương ứng; nhắc nghĩa vụ đăng ký hợp đồng theo mẫu (Điều 28) |
| Hợp đồng theo mẫu với người tiêu dùng buộc giải quyết tranh chấp bằng trọng tài | Điều 25 Luật Bảo vệ quyền lợi người tiêu dùng 2023; Án lệ 42/2021/AL | Agent tranh chấp nêu rủi ro điều khoản bị vô hiệu |
| Hợp đồng đã ký chứa họ tên, CCCD, số tài khoản | Luật Bảo vệ dữ liệu cá nhân 2025 (91/2025/QH15), Nghị định 356/2025/NĐ-CP | Token hóa trước khi phân tích; bản gốc khóa chỉ-đọc; log không chứa nội dung |

#### Đặc thù kỹ thuật tại Việt Nam và Singapore

- **OCR:** Amazon Textract không có tiếng Việt trong danh sách ngôn ngữ hỗ trợ, nên bản scan tiếng Việt phải đọc bằng mô hình đa phương thức.
- **Mô hình:** Amazon Nova Pro không có bản in-Region tại Singapore; lời gọi phải đi qua inference profile APAC.
- **Guardrails:** tầng Standard hiểu tiếng Việt nhưng dùng xử lý liên Region trong APAC; kiểm tra contextual grounding tại Singapore chỉ có tầng Classic (Anh, Pháp, Tây Ban Nha).
- **Dữ liệu cá nhân:** Luật Bảo vệ dữ liệu cá nhân 2025 đòi hỏi thu thập tối thiểu và đánh giá khi chuyển dữ liệu ra nước ngoài.
