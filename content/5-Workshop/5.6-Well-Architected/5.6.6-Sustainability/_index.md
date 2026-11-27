---
title: "Sustainability"
date: 2026-09-07
weight: 6
chapter: false
pre: " <b> 5.6.6. </b> "
---

This pillar is recorded in the checklist and is not scored; SCA meets it through choices already described in the other pillars.

| Practice | How SCA applies it |
| --- | --- |
| No idle capacity | Fully serverless: no server runs while there is no work |
| Efficient hardware | Lambda on arm64 (Graviton) instead of x86 |
| No unnecessary training | No training or fine-tuning; knowledge is updated through RAG |
| Less repeated work | Prompt caching reduces the tokens that must be processed again (57% in the live run) |
| Data lifecycle | S3 Lifecycle deletes temporary data and moves old reports to a cheaper storage class |
| Right-sized environments | The dev stage uses endpoints in 1 AZ and is removed with `cdk destroy` after the demo |
| Not yet done | Try Amazon Nova Lite for the lighter agents, to use a smaller model when quality is sufficient |
