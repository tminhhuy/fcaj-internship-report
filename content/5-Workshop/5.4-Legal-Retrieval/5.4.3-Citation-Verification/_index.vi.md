---
title: "Kiểm chứng trích dẫn và đo chất lượng truy xuất"
date: 2026-09-07
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

#### VerifyCitations

Lambda `cite-score-report` chuẩn hóa số điều ("Điều 301 khoản 1" → `301`), rồi gọi `Retrieve` lần nữa với bộ lọc `law_id` bằng đúng văn bản được trích. Trích dẫn chỉ được giữ khi một kết quả khớp cả mã văn bản lẫn số điều.

| Trường hợp | Trong báo cáo | Ảnh hưởng điểm |
| --- | --- | --- |
| Tìm lại được đúng điều | Trích dẫn giữ nguyên, kèm trạng thái hiệu lực và link văn bản gốc | Trừ đủ theo mức độ; cờ "trái luật" kích hoạt quy tắc chặn 69 |
| Không tìm lại được | Trích dẫn bị loại; nhận định ghi "chưa đủ căn cứ", tối đa mức trung bình | Chỉ trừ nửa; không được coi là trái luật |
| Báo cáo tiếng Anh | Thêm điểm grounding 0–1 của Guardrails: đoạn luật có hỗ trợ nhận định không | Chỉ tham khảo, không đổi điểm |
| Câu trích không khớp nguyên văn hợp đồng | Cảnh báo trong báo cáo | Không đổi điểm |

Kiểm tra grounding chỉ có cho báo cáo tiếng Anh vì tại Singapore tính năng này chỉ có ở tầng Classic (Anh, Pháp, Tây Ban Nha).

#### Đo chất lượng truy xuất

Tệp `kb_golden.json` có **70 câu hỏi chuẩn** (Việt Nam, điều ước quốc tế, EU, Hoa Kỳ, Anh, Singapore, Nhật), mỗi câu kèm điều luật phải tìm thấy.

- Sau mỗi lần nạp, Lambda `kb-eval` chạy bộ câu hỏi, gửi email kết quả và ghi chỉ số `KbGoldenHitRate` vào Amazon CloudWatch.
- Câu thuộc văn bản chưa nạp được ghi "chưa nạp", không tính là trượt.
- Các mức kiểm chứng (`pending`, `desk_checked`, `verified`) ở [5.4.1]({{< relref "/5-Workshop/5.4-Legal-Retrieval/5.4.1-Data-Tiers-and-Storage" >}}) cho người đọc biết mỗi văn bản đã được con người kiểm tra đến đâu.

Cách luật mới vào Knowledge Base, có bước người duyệt, được mô tả ở lane B của [5.6.4 Vận hành xuất sắc]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.4-Operational-Excellence" >}}).
