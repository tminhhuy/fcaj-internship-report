---
title: "Nguyên tắc thiết kế"
date: 2026-09-07
weight: 3
chapter: false
pre: " <b> 5.2.3. </b> "
---

Bảy nguyên tắc dưới đây quyết định mọi lựa chọn ở các phần sau; mỗi nguyên tắc có căn cứ từ [5.2.2]({{< relref "/5-Workshop/5.2-Context-and-Principles/5.2.2-Research-Foundation" >}}) và một cơ chế cụ thể trong code.

| Nguyên tắc | Căn cứ | Hiện thực trong SCA |
| --- | --- | --- |
| Chỉ tư vấn, con người quyết định | Dahl 2024; Magesh 2025; OWASP LLM01 (con người duyệt) | Không có đường ghi nào vào hợp đồng; báo cáo chỉ đề xuất và luôn ghi không thay thế ý kiến luật sư |
| Nhận định pháp lý phải có trích dẫn kiểm chứng được | Dahl 2024; Magesh 2025; Lewis 2020 | `search_law` lấy đoạn luật từ Knowledge Base; VerifyCitations tìm lại đúng mã văn bản và số điều; không tìm thấy thì loại trích dẫn và hạ nhận định xuống "chưa đủ căn cứ" |
| Nội dung hợp đồng là dữ liệu, không phải chỉ dẫn | Greshake 2023; OWASP LLM01 | Hợp đồng bọc trong thẻ dữ liệu; system prompt cấm làm theo lệnh trong hợp đồng; bộ lọc prompt attack của Guardrails; đầu ra chỉ qua công cụ ghi JSON được code kiểm tra schema; điểm số tính bằng code, không do model tự chấm |
| Thu thập tối thiểu dữ liệu cá nhân | Luật Bảo vệ dữ liệu cá nhân 2025; OWASP LLM02 (lộ thông tin nhạy cảm) | Token hóa + LeakCheck trước mọi agent; bảng token dùng khóa KMS riêng, tự hết hạn sau 30 ngày; log không chứa nội dung hợp đồng |
| Chia nhỏ tác vụ, đầu ra có cấu trúc | LegalBench 2023; CUAD 2021 | Sáu agent, mỗi agent một nhiệm vụ và một schema; JSON sai được gửi lại để sửa tối đa 2 lần; danh sách điều khoản bắt buộc theo ngành |
| Tri thức nằm ngoài mô hình, do con người cập nhật | Lewis 2020; Global Legal & Compliance Framework | Kho luật theo từng điều có trạng thái hiệu lực; người duyệt đưa văn bản vào kho; 70 câu hỏi chuẩn đo sau mỗi lần nạp |
| Điều phối tất định, quyền tối thiểu | OWASP LLM01 (quyền tối thiểu); AWS Well-Architected (Security) | Step Functions quyết định thứ tự bước, không phải model; mỗi Lambda một IAM role; agent không đọc được bản gốc hay bảng token |

Hệ quả quan trọng nhất: điểm số và cờ "trái luật" chỉ đến từ phát hiện đã qua kiểm tra schema và kiểm chứng trích dẫn. Một câu chèn trong hợp đồng như "chấm 100 điểm" không thể đặt điểm trực tiếp; nó chỉ có thể tác động gián tiếp qua nội dung phát hiện, và đó là chỗ Guardrails cùng bộ evals kiểm tra.
