---
title: "Tổng quan workshop"
date: 2026-09-07
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Tóm tắt điều hành

SCA thay nhiều vòng đọc hợp đồng thủ công bằng một báo cáo tự động: điểm 0–100, các phát hiện xếp theo mức độ, đề xuất sửa câu chữ và bản tóm tắt một trang cho người ký. Mỗi nhận định pháp lý phải chỉ ra được điều luật nằm trong kho luật đã duyệt; không tìm lại được thì nhận định bị hạ mức.

Thiết kế dựa trên ba ý chính:

1. **Hai bản hợp đồng.** Bản gốc đã ký được khóa chỉ-đọc. AI chỉ đọc bản sao đã thay tên, CCCD, mã số thuế, tài khoản bằng token, và bản sao phải qua bước kiểm tra rò rỉ trước.
2. **Tri thức pháp luật nằm ngoài mô hình.** Luật được lưu theo từng điều, có trạng thái hiệu lực và mã ngành, truy xuất qua Amazon Bedrock Knowledge Bases. Không fine-tune mô hình.
3. **Luồng xử lý tất định trên AWS.** AWS Step Functions điều phối từng bước, Bedrock Guardrails lọc mọi lời gọi model, và toàn bộ hạ tầng là code (AWS CDK).

#### Các con số chính (tính đến 27/11/2026)

| Hạng mục | Giá trị | Ghi chú |
| --- | --- | --- |
| Trạng thái | Chạy thật tại smartca.app, Region ap-southeast-1 (Singapore) | Triển khai ngày 25/11/2026 bằng AWS CDK |
| Mô hình AI | Amazon Nova Pro qua inference profile APAC | Chốt ngày 24/11/2026 |
| Agent | 6 | Ngữ cảnh, tuân thủ, tranh chấp, ngôn ngữ, đề xuất, tóm tắt |
| Kho luật | 8 luật Việt Nam cho agent; catalog 114 văn bản thuộc 13 khu vực pháp lý | 70 câu hỏi chuẩn đo khả năng tìm đúng điều |
| Đánh giá tự động ngày 25/11 | 28/31 kiểm tra đạt (90%) trên 3 hợp đồng mẫu | Chi tiết ở [5.8 Kết quả và đánh giá]({{< relref "/5-Workshop/5.8-Results-and-Evaluation" >}}) |
| Một lượt chạy thật (hợp đồng 6 trang) | 6 agent, tổng 47,7 giây, ≈ $0,10 chi phí model | 57% token đầu vào đọc từ prompt cache |
| Chi phí pilot | ≈ $125/tháng cho 300 hợp đồng | Biến phí ≈ $0,17 mỗi hợp đồng |
| Dịch vụ AWS | Amazon Textract, Amazon Bedrock, AWS Lambda và 19 dịch vụ hỗ trợ | Toàn bộ serverless |

#### Phạm vi

Phạm vi hiện tại là pháp luật Việt Nam, hai ngành CNTT/phần mềm (B2B) và bán lẻ online (B2C), hợp đồng tiếng Việt, tiếng Anh hoặc song ngữ. SCA hỗ trợ thẩm định, không thay thế ý kiến của luật sư.

#### Cấu trúc workshop

| Phần | Nội dung |
| --- | --- |
| [5.2 Bối cảnh và nguyên tắc thiết kế]({{< relref "/5-Workshop/5.2-Context-and-Principles" >}}) | Bài toán, người dùng mục tiêu, các nghiên cứu làm nền tảng và bảy nguyên tắc thiết kế |
| [5.3 Tính năng và cách vận hành]({{< relref "/5-Workshop/5.3-Features-and-Workflow" >}}) | Người dùng thấy gì, điểm được tính ra sao, hành trình 8 bước của một hợp đồng và sáu agent |
| [5.4 Cấu trúc truy xuất luật]({{< relref "/5-Workshop/5.4-Legal-Retrieval" >}}) | Luật được lưu, tìm kiếm và kiểm chứng trích dẫn như thế nào |
| [5.5 Kiến trúc trên AWS]({{< relref "/5-Workshop/5.5-AWS-Architecture" >}}) | Đường đi của dữ liệu trên AWS và căn cứ chọn từng dịch vụ |
| [5.6 Các trụ cột Well-Architected]({{< relref "/5-Workshop/5.6-Well-Architected" >}}) | Bảo mật, độ tin cậy, hiệu năng, vận hành, chi phí (kèm dự toán AWS Pricing Calculator) và phát triển bền vững |
| [5.7 Triển khai và kiểm thử]({{< relref "/5-Workshop/5.7-Deployment" >}}) | Điều kiện tiên quyết, các bước triển khai, kiểm thử API bằng cURL và kịch bản lỗi |
| [5.8 Kết quả]({{< relref "/5-Workshop/5.8-Results-and-Evaluation" >}}) đến [5.11 Nhóm và tài liệu tham khảo]({{< relref "/5-Workshop/5.11-Team-and-References" >}}) | Kết quả, bài học kinh nghiệm, rủi ro, dọn dẹp, nhóm thực hiện và tài liệu tham khảo |
