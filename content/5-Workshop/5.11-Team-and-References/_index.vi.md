---
title: "Nhóm thực hiện và tài liệu tham khảo"
date: 2026-09-07
weight: 11
chapter: false
pre: " <b> 5.11. </b> "
---

#### Nhóm thực hiện

| Họ tên | MSSV |
| --- | --- |
| Trương Minh Huy | SE203009 |
| Đặng Đức Huy | SE181811 |

#### Đối chiếu thang chấm kiến trúc (AWS Well-Architected)

| Tiêu chí | Điểm tối đa | Nằm ở đâu | Bằng chứng chính | Còn thiếu |
| --- | --- | --- | --- | --- |
| Dịch vụ, icon, tên, mục đích, luồng; thẩm mỹ sơ đồ; tự vẽ và giải thích | 0,5 | 5.3.2, 5.5, 5.6.1, 5.6.4 (Hình 1–5) | AWS Architecture Icons, thứ bậc AWS Cloud → Region → VPC → subnet, bước ❶–❿, file `.drawio` trong `docs/architecture/` | Nhóm tự chỉnh lại Hình 1–5 trong draw.io (quy định không dùng sơ đồ GenAI 100%) và luyện giải thích từng bước |
| Operational Excellence | 0,5 | 5.6.4, 5.7 | Hạ tầng dạng code, pipeline có cdk-nag, cfn-guard, evals và duyệt tay; 9 alarm, dashboard, runbook | Bật pipeline (điền `connectionArn`) |
| Security | 0,5 | 5.2.3, 5.6.1 | MFA, IAM mỗi hàm một role, subnet private không NAT, VPC endpoint, KMS, token hóa + LeakCheck, Guardrails, WAF, không credential trong repo | Sửa ca prompt injection; hồ sơ chuyển dữ liệu ra nước ngoài khi triển khai thật |
| Reliability | 0,5 | 5.6.2 | SQS + DLQ, giới hạn đồng thời, retry có backoff, idempotency, multi-AZ, PITR, dựng lại bằng CDK | Chạy bài tải 50 hợp đồng |
| Performance Efficiency | 0,5 | 5.6.3 | Serverless, Graviton, chạy song song có giới hạn, prompt caching (57%), CloudFront | Đo p95 thời gian xử lý |
| Cost Optimization | 0,5 | 5.6.5, 5.10 | Link và ảnh chụp AWS Pricing Calculator, chi tiết từng dịch vụ, giả định tải, 8 biện pháp tối ưu, quy trình dọn dẹp, so với chi phí thực tế | — |
| Sustainability | 0 | 5.6.6 | Checklist | — |

#### Đối chiếu khung tiêu chuẩn workshop FCAJ

| Yêu cầu | Trạng thái | Nằm ở đâu |
| --- | --- | --- |
| Bối cảnh, bài toán, người dùng mục tiêu | Đã có | 5.2 |
| Mục tiêu, đầu ra và tiêu chí đo lường | Đã có | 5.1, 5.3.1, 5.8 |
| Gắn với hệ sinh thái AWS | Đã có: Textract, Bedrock, Lambda và 19 dịch vụ hỗ trợ | 5.5 |
| Sơ đồ kiến trúc bằng AWS Architecture Icons | Đã có; nhóm cần tự rà soát và chỉnh | Hình 1–5 |
| Căn cứ lựa chọn dịch vụ | Đã có | 5.5 |
| IAM least privilege, subnet private, VPC endpoint, không hard-code credential | Đã có | 5.6.1 |
| Chịu lỗi, event-driven, SQS, CloudWatch | Đã có | 5.6.2, 5.6.4 |
| Dự toán bằng AWS Pricing Calculator theo tải thực tế, tối ưu, dọn dẹp | Đã có | 5.6.5, 5.10 |
| Điều kiện tiên quyết, hướng dẫn từng bước, kiểm thử cURL, kịch bản lỗi | Đã có | 5.7 |
| Sản phẩm chạy thật, video minh chứng | Đã có: smartca.app và video demo | 5.7, 5.8 |
| Song ngữ Việt – Anh | Đã có: mọi trang workshop có cả hai ngôn ngữ | 5 |
| Trang workshop theo template FCAJ, có mục lục | Đã có | 5 |
| Code, CLI, policy trong code block có highlight | Đã có | 5.4, 5.6.1, 5.7, 5.10 |
| Mở rộng so với lab cơ bản (CI/CD, IaC, bảo mật sâu) | Đã có: hai bản hợp đồng + LeakCheck, kiểm chứng trích dẫn, kho luật theo mã ngành, pipeline có evals | 5.2.3, 5.4, 5.6.4 |
| Bài học kinh nghiệm, xử lý sự cố, định hướng phát triển | Đã có | 5.9 |

#### Tài liệu tham khảo

Nghiên cứu được tra cứu ngày 27/11/2026; giá và giới hạn dịch vụ AWS tra từ 10/11 đến 25/11/2026; văn bản pháp luật cần bộ phận pháp chế đối chiếu với bản chính thức.

**Nghiên cứu**

1. Hendrycks, D., Burns, C., Chen, A., Ball, S. (2021). [CUAD: An Expert-Annotated NLP Dataset for Legal Contract Review](https://arxiv.org/abs/2103.06268). NeurIPS 2021 Datasets and Benchmarks.
2. Guha, N. và cộng sự (2023). [LegalBench: A Collaboratively Built Benchmark for Measuring Legal Reasoning in Large Language Models](https://proceedings.neurips.cc/paper_files/paper/2023/hash/89e44582fd28ddfea1ea4dcb0ebbf4b0-Abstract.html). NeurIPS 2023 Datasets and Benchmarks.
3. Dahl, M., Magesh, V., Suzgun, M., Ho, D. E. (2024). [Large Legal Fictions: Profiling Legal Hallucinations in Large Language Models](https://law.stanford.edu/publications/large-legal-fictions-profiling-legal-hallucinations-in-large-language-models). Journal of Legal Analysis 16.
4. Magesh, V., Surani, F., Dahl, M., Suzgun, M., Manning, C. D., Ho, D. E. (2025). [Hallucination-Free? Assessing the Reliability of Leading AI Legal Research Tools](https://econpapers.repec.org/RePEc:wly:empleg:v:22:y:2025:i:2:p:216-242). Journal of Empirical Legal Studies 22(2), 216–242.
5. Lewis, P. và cộng sự (2020). [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://papers.nips.cc/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html). NeurIPS 2020.
6. Greshake, K., Abdelnabi, S., Mishra, S., Endres, C., Holz, T., Fritz, M. (2023). [Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection](https://arxiv.org/abs/2302.12173). arXiv:2302.12173.
7. OWASP GenAI Security Project (2025). [LLM01:2025 Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/).
8. Nhóm dự án (2026). *Global Legal & Compliance Framework*, bản nghiên cứu nội bộ, 22/11/2026.

**AWS — tính năng, Region và model**

- [Amazon Nova Pro — model card: inference profile APAC, Region, prompt caching](https://docs.aws.amazon.com/bedrock/latest/userguide/model-card-amazon-nova-pro.md)
- [Amazon Nova — Document understanding](https://docs.aws.amazon.com/nova/latest/userguide/modalities-document.html)
- [Amazon Nova — tool use: greedy decoding, thẻ thinking, JSON schema](https://docs.aws.amazon.com/nova/latest/userguide/tools-troubleshooting.html)
- [Amazon Bedrock — Data protection](https://docs.aws.amazon.com/bedrock/latest/userguide/data-protection.html)
- [Languages supported by Amazon Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-supported-languages.html)
- [Safeguard tiers in Amazon Bedrock Guardrails](https://aws.amazon.com/blogs/machine-learning/tailor-responsible-ai-with-new-safeguard-tiers-in-amazon-bedrock-guardrails)
- [Supported models and Regions for Amazon Bedrock Knowledge Bases](https://docs.aws.eu/bedrock/latest/userguide/knowledge-base-supported.html)
- [Amazon S3 Vectors — generally available](https://aws.amazon.com/blogs/aws/amazon-s3-vectors-now-generally-available-with-increased-scale-and-performance)
- [Amazon S3 Vectors — Regions và endpoints](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-vectors-regions-quotas.html)
- [Amazon Textract FAQs — ngôn ngữ hỗ trợ, Signatures](https://aws.amazon.com/textract/faqs)
- [draw.io — thư viện AWS4 (AWS Architecture Icons)](https://raw.githubusercontent.com/jgraph/drawio/dev/src/main/webapp/js/diagramly/sidebar/Sidebar-AWS4.js)

**AWS — giá và dự toán**

- [AWS Pricing Calculator — pilot 300 hợp đồng/tháng, ap-southeast-1, Amazon Nova Pro](https://calculator.aws/#/estimate?id=d1ffc8f5b0b901c713213ef2a8a65d8be478a57d)
- [Amazon Bedrock pricing](https://aws.amazon.com/bedrock/pricing/)
- [AWS Free Tier: credit và gói miễn phí 6 tháng](https://aws.amazon.com/about-aws/whats-new/2025/07/aws-free-tier-credits-month-free-plan)

**Pháp luật Việt Nam**

- [Tạp chí Công Thương — chế tài phạt vi phạm theo Điều 301 Luật Thương mại 2005](https://tapchicongthuong.vn/che-tai-phat-vi-pham-do-vi-pham-hop-dong-thuong-mai-theo-quy-dinh-cua-phap-luat-viet-nam-98909.htm)
- [Bộ Công Thương — kiểm soát hợp đồng theo mẫu trong Luật Bảo vệ quyền lợi người tiêu dùng 2023](https://moit.gov.vn/tin-tuc/bao-chi-voi-nguoi-dan/nhung-diem-moi-cua-luat-bao-ve-quyen-loi-nguoi-tieu-dung-2023-trong-kiem-soat-hop-dong-theo-mau-dieu-kien-giao-dich-chun.html)
- [Luật Long Phan — điều khoản trọng tài trong hợp đồng mẫu với người tiêu dùng](https://luatlongphan.vn/?p=91354)
- [VCCI — Luật Bảo vệ dữ liệu cá nhân 2025 và Nghị định 356/2025](https://www.vcci.com.vn/tin-tuc/luat-bao-ve-du-lieu-ca-nhan-luu-y-ve-hanh-lang-phap-ly-moi-cho-doanh-nghiep)
- [LuatVietnam — Quyết định 36/2025/QĐ-TTg về Hệ thống ngành kinh tế Việt Nam](https://luatvietnam.vn/tin-van-ban-moi/tai-toan-van-quyet-dinh-36-2025-qd-ttg-cua-thu-tuong-ve-he-thong-nganh-kinh-te-186-104480-article.html)
