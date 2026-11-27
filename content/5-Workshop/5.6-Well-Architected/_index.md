---
title: "AWS Well-Architected pillars"
date: 2026-09-07
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

This part reviews SCA against the six pillars of the AWS Well-Architected Framework. Each page lists the design decisions, the evidence in the deployed system and what is still missing.

| Pillar | Main mechanisms in SCA |
| --- | --- |
| [Security]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.1-Security" >}}) | MFA, one IAM role per function, private subnets with no NAT, VPC endpoints, KMS, tokenization + LeakCheck, Guardrails, WAF, no credentials in the repository |
| [Reliability]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.2-Reliability" >}}) | SQS + DLQ, concurrency limit, retries with backoff, idempotency, multi-AZ, point-in-time recovery, rebuild with CDK |
| [Performance Efficiency]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.3-Performance-Efficiency" >}}) | Serverless, Graviton, bounded parallelism, prompt caching (57%), CloudFront |
| [Operational Excellence]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.4-Operational-Excellence" >}}) | Infrastructure as code, a release pipeline with security checks, evals and manual approval; a reviewed law update lane; 9 alarms, a dashboard, runbooks |
| [Cost Optimization]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.5-Cost-Optimization" >}}) | AWS Pricing Calculator estimate per service, load assumptions, 8 optimization measures, comparison with real cost |
| [Sustainability]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.6-Sustainability" >}}) | Serverless, Graviton, no fine-tuning, prompt caching, lifecycle rules |
