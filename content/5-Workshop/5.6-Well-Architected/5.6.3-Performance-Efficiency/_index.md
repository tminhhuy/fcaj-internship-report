---
title: "Performance Efficiency"
date: 2026-09-07
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---

Every component scales with usage; the real limit is the Amazon Bedrock token quota, so performance comes from controlled parallelism and reading prompts back from the cache.

| Layer | Choice | Effect |
| --- | --- | --- |
| Compute | Lambda arm64 (Graviton), memory sized per job: `api` 512 MB; `ocr-extract`, `tokenize-guard`, `agent-runner` 1,024 MB; `cite-score-report` 1,536 MB (builds the PDF) | Scales automatically, no idle servers |
| Parallelism | 3 analysis agents run at the same time (Parallel); Vietnamese OCR reads 4 pages at a time (Map); at most 5 contracts at a time (`maxConcurrent` parameter) | Shorter time per contract without exceeding the quota |
| Model | Amazon Nova Pro, greedy decoding, 4,000–6,000 output tokens per agent, raised 1.5× when cut off (ceiling 10,000); prompt caching for the system prompt and the contract | Live run: 57% of input tokens read from the cache, 6.2–10.3 seconds per agent |
| Data path | The browser uploads straight to S3 with a presigned POST; only S3 keys are passed between steps | No payload limits from API Gateway, Lambda or Step Functions |
| Content delivery | CloudFront: optimized caching for static pages, no caching for `/api/*`; Price Class 200 (edge locations in Vietnam and Singapore) | Fast page loads for users in Vietnam |
| Retrieval | Metadata filtering inside the S3 Vectors query; one law article per stored unit | Each `search_law` call returns exactly 5 short passages, keeping prompts small |
| Data | DynamoDB on-demand; S3 Standard, reports move to Standard-IA after 30 days | No capacity planning needed |

#### Measuring performance

Performance is tracked on the CloudWatch dashboard: p50 and p95 processing time, tokens per agent, Bedrock throttling. The target is **p95 under 5 minutes for a 10-page contract**; above that, an alarm fires.

To increase throughput:

1. Ask for a higher cross-Region quota for Nova Pro, then raise `maxConcurrent`.
2. Move the language and summary agents to Amazon Nova Lite once the evals confirm the quality stays the same.

#### Still to do

- Measure p95 processing time on the live stack. Today's evidence is one run: six agents took 47.7 seconds for a 6-page contract.
