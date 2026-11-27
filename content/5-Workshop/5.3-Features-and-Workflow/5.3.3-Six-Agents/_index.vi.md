---
title: "Sáu agent"
date: 2026-09-07
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

Cả sáu agent chạy trên cùng Lambda `agent-runner`, mỗi state gọi một agent qua Amazon Bedrock Converse API. Agent 1 chạy trước; kết quả của nó (loại hợp đồng, vai trò, B2B hay B2C) được đưa cho các agent sau.

| Agent | Nhiệm vụ | Công cụ | Đầu ra |
| --- | --- | --- | --- |
| 1. Ngữ cảnh & vai trò | Bên cung cấp, bên mua, B2B hay B2C; thuật ngữ đúng ngành; điều khoản trọng yếu | Ghi JSON | Vai trò, điều khoản trọng yếu, phát hiện về ngữ cảnh và cân bằng |
| 2. Tuân thủ pháp luật | Đối chiếu từng điều khoản với luật còn hiệu lực | `search_law` (tối đa 3 lượt) + ghi JSON | Điều khoản vi phạm kèm trích dẫn |
| 3. Tranh chấp & chế tài | Phạt vi phạm, bồi thường, chấm dứt, cơ quan giải quyết tranh chấp, luật áp dụng | `search_law` (tối đa 3 lượt) + ghi JSON | Rủi ro tranh chấp kèm trích dẫn |
| 4. Ngôn ngữ & công bằng | Câu mơ hồ, một chiều, hiểu hai nghĩa; độ khớp bản Việt và bản Anh | Ghi JSON | Câu cần làm rõ và lý do |
| 5. Đề xuất chỉnh sửa | Gộp phát hiện agent 1–4, ưu tiên theo mức độ, viết câu thay thế | Ghi JSON | Bảng đề xuất, không ghi vào hợp đồng |
| 6. Tóm tắt điều hành | Các bên, đối tượng, giá trị, thời hạn, 3–5 rủi ro lớn nhất, khuyến nghị | Ghi JSON | Smart Summary một trang |

#### Cách gọi model

- **Prompt** viết theo hướng dẫn prompting của Amazon Nova: vai trò, các chỉ dẫn đánh số (MUST/NEVER cho quy tắc cứng), một nhận định mẫu, hợp đồng đặt trước và nhiệm vụ đặt cuối.
- **Giải mã** greedy (temperature 0, topK 1), model chỉ trả kết quả qua công cụ ghi JSON; phần `<thinking>` của Nova bị bỏ, không lưu.
- **Prompt caching:** system prompt và khối hợp đồng đặt trước điểm cache (Nova cache tối đa 20.000 token), nên từ agent thứ hai phần này được đọc từ prompt cache; Bedrock từ chối điểm cache thì lời gọi được gửi lại không cache.
- **Giới hạn đầu ra:** 4.000–6.000 token mỗi agent, tự nâng 1,5 lần khi câu trả lời bị cắt (trần 10.000).
- **Guardrails:** mọi lời gọi gắn guardrail lọc nội dung.
