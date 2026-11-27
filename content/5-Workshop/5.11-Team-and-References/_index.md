---
title: "Team and references"
date: 2026-09-07
weight: 11
chapter: false
pre: " <b> 5.11. </b> "
---

#### Team

| Full name | Student ID |
| --- | --- |
| Trương Minh Huy | SE203009 |
| Đặng Đức Huy | SE181811 |

#### Mapping to the architecture rubric (AWS Well-Architected)

| Criterion | Max points | Where | Main evidence | Still missing |
| --- | --- | --- | --- | --- |
| Services, icons, names, purpose, flow; diagram quality; drawn and explained by the team | 0.5 | 5.3.2, 5.5, 5.6.1, 5.6.4 (Figures 1–5) | AWS Architecture Icons, hierarchy AWS Cloud → Region → VPC → subnet, steps ❶–❿, `.drawio` files in `docs/architecture/` | The team adjusts Figures 1–5 in draw.io itself (diagrams must not be 100% GenAI) and practises explaining each step |
| Operational Excellence | 0.5 | 5.6.4, 5.7 | Infrastructure as code, a pipeline with cdk-nag, cfn-guard, evals and manual approval; 9 alarms, a dashboard, runbooks | Turn the pipeline on (`connectionArn`) |
| Security | 0.5 | 5.2.3, 5.6.1 | MFA, one IAM role per function, private subnets with no NAT, VPC endpoints, KMS, tokenization + LeakCheck, Guardrails, WAF, no credentials in the repository | Fix the prompt injection case; cross-border data transfer assessment for production |
| Reliability | 0.5 | 5.6.2 | SQS + DLQ, concurrency limit, retries with backoff, idempotency, multi-AZ, PITR, rebuild with CDK | Run the 50-contract load test |
| Performance Efficiency | 0.5 | 5.6.3 | Serverless, Graviton, bounded parallelism, prompt caching (57%), CloudFront | Measure p95 processing time |
| Cost Optimization | 0.5 | 5.6.5, 5.10 | AWS Pricing Calculator link and screenshot, per-service breakdown, load assumptions, 8 optimization measures, clean-up procedure, comparison with real cost | — |
| Sustainability | 0 | 5.6.6 | Checklist | — |

#### Mapping to the FCAJ workshop standard

| Requirement | Status | Where |
| --- | --- | --- |
| Context, problem, target users | Done | 5.2 |
| Goals, outputs and success criteria | Done | 5.1, 5.3.1, 5.8 |
| Built on the AWS ecosystem | Done: Textract, Bedrock, Lambda and 19 supporting services | 5.5 |
| Architecture diagrams with AWS Architecture Icons | Done; the team still reviews and adjusts them | Figures 1–5 |
| Reasons for each service choice | Done | 5.5 |
| IAM least privilege, private subnets, VPC endpoints, no hard-coded credentials | Done | 5.6.1 |
| Fault tolerance, event-driven, SQS, CloudWatch | Done | 5.6.2, 5.6.4 |
| AWS Pricing Calculator estimate based on real load, optimization, clean-up | Done | 5.6.5, 5.10 |
| Prerequisites, step-by-step guide, cURL tests, failure scenarios | Done | 5.7 |
| Live product, demo video | Done: smartca.app and the demo video | 5.7, 5.8 |
| Vietnamese – English bilingual | Done: every workshop page has both languages | 5 |
| Workshop page on the FCAJ template, with a table of contents | Done | 5 |
| Code, CLI and policies in highlighted code blocks | Done | 5.4, 5.6.1, 5.7, 5.10 |
| Goes beyond the basic lab (CI/CD, IaC, deeper security) | Done: two contract copies + LeakCheck, citation verification, law library with industry codes, pipeline with evals | 5.2.3, 5.4, 5.6.4 |
| Lessons learned, troubleshooting, roadmap | Done | 5.9 |

#### References

Research was looked up on 27/11/2026; AWS prices and service limits were checked between 10/11 and 25/11/2026; legal texts must be checked against the official versions by a legal officer.

**Research**

1. Hendrycks, D., Burns, C., Chen, A., Ball, S. (2021). [CUAD: An Expert-Annotated NLP Dataset for Legal Contract Review](https://arxiv.org/abs/2103.06268). NeurIPS 2021 Datasets and Benchmarks.
2. Guha, N. et al. (2023). [LegalBench: A Collaboratively Built Benchmark for Measuring Legal Reasoning in Large Language Models](https://proceedings.neurips.cc/paper_files/paper/2023/hash/89e44582fd28ddfea1ea4dcb0ebbf4b0-Abstract.html). NeurIPS 2023 Datasets and Benchmarks.
3. Dahl, M., Magesh, V., Suzgun, M., Ho, D. E. (2024). [Large Legal Fictions: Profiling Legal Hallucinations in Large Language Models](https://law.stanford.edu/publications/large-legal-fictions-profiling-legal-hallucinations-in-large-language-models). Journal of Legal Analysis 16.
4. Magesh, V., Surani, F., Dahl, M., Suzgun, M., Manning, C. D., Ho, D. E. (2025). [Hallucination-Free? Assessing the Reliability of Leading AI Legal Research Tools](https://econpapers.repec.org/RePEc:wly:empleg:v:22:y:2025:i:2:p:216-242). Journal of Empirical Legal Studies 22(2), 216–242.
5. Lewis, P. et al. (2020). [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://papers.nips.cc/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html). NeurIPS 2020.
6. Greshake, K., Abdelnabi, S., Mishra, S., Endres, C., Holz, T., Fritz, M. (2023). [Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection](https://arxiv.org/abs/2302.12173). arXiv:2302.12173.
7. OWASP GenAI Security Project (2025). [LLM01:2025 Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/).
8. Project team (2026). *Global Legal & Compliance Framework*, internal research document, 22/11/2026.

**AWS: features, Regions and models**

- [Amazon Nova Pro model card: APAC inference profile, Regions, prompt caching](https://docs.aws.amazon.com/bedrock/latest/userguide/model-card-amazon-nova-pro.md)
- [Amazon Nova: document understanding](https://docs.aws.amazon.com/nova/latest/userguide/modalities-document.html)
- [Amazon Nova tool use: greedy decoding, thinking tags, JSON schema](https://docs.aws.amazon.com/nova/latest/userguide/tools-troubleshooting.html)
- [Amazon Bedrock: data protection](https://docs.aws.amazon.com/bedrock/latest/userguide/data-protection.html)
- [Languages supported by Amazon Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-supported-languages.html)
- [Safeguard tiers in Amazon Bedrock Guardrails](https://aws.amazon.com/blogs/machine-learning/tailor-responsible-ai-with-new-safeguard-tiers-in-amazon-bedrock-guardrails)
- [Supported models and Regions for Amazon Bedrock Knowledge Bases](https://docs.aws.eu/bedrock/latest/userguide/knowledge-base-supported.html)
- [Amazon S3 Vectors: generally available](https://aws.amazon.com/blogs/aws/amazon-s3-vectors-now-generally-available-with-increased-scale-and-performance)
- [Amazon S3 Vectors: Regions and endpoints](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-vectors-regions-quotas.html)
- [Amazon Textract FAQs: supported languages, Signatures](https://aws.amazon.com/textract/faqs)
- [draw.io AWS4 library (AWS Architecture Icons)](https://raw.githubusercontent.com/jgraph/drawio/dev/src/main/webapp/js/diagramly/sidebar/Sidebar-AWS4.js)

**AWS: pricing and estimate**

- [AWS Pricing Calculator: pilot of 300 contracts per month, ap-southeast-1, Amazon Nova Pro](https://calculator.aws/#/estimate?id=d1ffc8f5b0b901c713213ef2a8a65d8be478a57d)
- [Amazon Bedrock pricing](https://aws.amazon.com/bedrock/pricing/)
- [AWS Free Tier: credits and the 6-month free plan](https://aws.amazon.com/about-aws/whats-new/2025/07/aws-free-tier-credits-month-free-plan)

**Vietnamese law (sources in Vietnamese)**

- [Tạp chí Công Thương: penalties under Article 301 of the Commercial Law 2005](https://tapchicongthuong.vn/che-tai-phat-vi-pham-do-vi-pham-hop-dong-thuong-mai-theo-quy-dinh-cua-phap-luat-viet-nam-98909.htm)
- [Ministry of Industry and Trade: control of standard-form contracts in the Law on Protection of Consumer Rights 2023](https://moit.gov.vn/tin-tuc/bao-chi-voi-nguoi-dan/nhung-diem-moi-cua-luat-bao-ve-quyen-loi-nguoi-tieu-dung-2023-trong-kiem-soat-hop-dong-theo-mau-dieu-kien-giao-dich-chun.html)
- [Luật Long Phan: arbitration clauses in standard-form consumer contracts](https://luatlongphan.vn/?p=91354)
- [VCCI: the Personal Data Protection Law 2025 and Decree 356/2025](https://www.vcci.com.vn/tin-tuc/luat-bao-ve-du-lieu-ca-nhan-luu-y-ve-hanh-lang-phap-ly-moi-cho-doanh-nghiep)
- [LuatVietnam: Decision 36/2025/QĐ-TTg on the Vietnam Standard Industrial Classification](https://luatvietnam.vn/tin-van-ban-moi/tai-toan-van-quyet-dinh-36-2025-qd-ttg-cua-thu-tuong-ve-he-thong-nganh-kinh-te-186-104480-article.html)
