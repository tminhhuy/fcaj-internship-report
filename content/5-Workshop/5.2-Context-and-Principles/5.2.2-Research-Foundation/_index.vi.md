---
title: "Nghiên cứu làm nền tảng"
date: 2026-09-07
weight: 2
chapter: false
pre: " <b> 5.2.2. </b> "
---

Mỗi nghiên cứu dưới đây dẫn trực tiếp tới một quyết định thiết kế của SCA.

| Nghiên cứu | Phát hiện chính | Hệ quả cho SCA |
| --- | --- | --- |
| [Hendrycks và cộng sự (2021), CUAD](https://arxiv.org/abs/2103.06268), NeurIPS 2021 | 510 hợp đồng, 41 loại điều khoản, hơn 13.000 nhãn do chuyên gia pháp lý gắn; mô hình transformer mới đạt mức "sơ khởi" | Rà soát cần danh mục điều khoản do chuyên gia định nghĩa: SCA dùng danh sách điều khoản bắt buộc theo ngành và để con người quyết định |
| [Guha và cộng sự (2023), LegalBench](https://proceedings.neurips.cc/paper_files/paper/2023/hash/89e44582fd28ddfea1ea4dcb0ebbf4b0-Abstract.html), NeurIPS 2023 | 162 tác vụ thuộc sáu loại lập luận pháp lý | Chia việc rà soát thành tác vụ hẹp: sáu agent, mỗi agent một nhiệm vụ và một JSON schema |
| [Dahl và cộng sự (2024), Large Legal Fictions](https://arxiv.org/abs/2401.01301), Journal of Legal Analysis 16 | Chatbot ảo giác 58% (ChatGPT 4) đến 88% (Llama 2) khi trả lời câu hỏi kiểm chứng được về án liên bang Mỹ | Không nhận định nào được coi là có căn cứ nếu không tìm lại được nguồn |
| [Magesh và cộng sự (2025), Hallucination-Free?](https://arxiv.org/abs/2405.20362), Journal of Empirical Legal Studies 22(2): 216–242 | Lexis+ AI, Westlaw AI-Assisted Research và Ask Practical Law AI vẫn ảo giác 17–33% dù có truy xuất tài liệu (RAG) | RAG thôi chưa đủ: SCA thêm bước VerifyCitations tất định, tìm lại đúng mã văn bản và số điều |
| [Lewis và cộng sự (2020), Retrieval-Augmented Generation](https://papers.nips.cc/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html), NeurIPS 2020 | Chỉ ra nguồn và cập nhật tri thức là hai vấn đề mở của mô hình chỉ dựa vào tham số; truy xuất giải quyết cả hai | Luật nằm trong Knowledge Base cập nhật được, không fine-tune |
| [Greshake và cộng sự (2023), Indirect Prompt Injection](https://arxiv.org/abs/2302.12173) | Ứng dụng tích hợp LLM xóa ranh giới giữa dữ liệu và chỉ dẫn; kẻ tấn công cài lệnh vào dữ liệu mà ứng dụng sẽ đọc | Nội dung hợp đồng là dữ liệu không tin cậy, không bao giờ là chỉ dẫn |
| [OWASP (2025), LLM01: Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/) | Năm biện pháp: giới hạn hành vi model, kiểm tra định dạng đầu ra bằng code, tách nội dung bên ngoài, quyền tối thiểu, con người duyệt | Cả năm biện pháp đều có trong SCA ([5.2.3 Nguyên tắc thiết kế]({{< relref "/5-Workshop/5.2-Context-and-Principles/5.2.3-Design-Principles" >}})) |
| *Global Legal & Compliance Framework* (bản nghiên cứu nội bộ của nhóm, 22/11/2026) | 5 bảng dữ liệu, 15 trường metadata, 3 tầng nguồn; nguyên tắc "hoàn thiện văn bản gốc và trạng thái hiệu lực trước, kết luận pháp lý sau" | Cấu trúc kho luật và vòng đời văn bản ở [5.4 Cấu trúc truy xuất luật]({{< relref "/5-Workshop/5.4-Legal-Retrieval" >}}) |

{{% notice info %}}
Các tỷ lệ ảo giác ở trên đến từ nghiên cứu pháp lý tại Mỹ. Chúng được dùng làm bằng chứng rằng không thể để model tự trích luật, không phải dự đoán tỷ lệ lỗi của SCA.
{{% /notice %}}
