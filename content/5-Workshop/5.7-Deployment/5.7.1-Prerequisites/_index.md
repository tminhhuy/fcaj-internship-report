---
title: "Prerequisites"
date: 2026-09-07
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

| Requirement | Details |
| --- | --- |
| AWS account | Administrator permissions for the first deployment; Region ap-southeast-1 |
| Amazon Bedrock | Able to call Amazon Nova Pro through `apac.amazon.nova-pro-v1:0` and Cohere Embed Multilingual v3 |
| Tools | AWS CLI v2 (signed in with IAM Identity Center or `aws login`, no access keys stored in files), Node.js 22, Python 3.11 or later, Git, jq |
| Custom domain (optional) | An ACM certificate in us-east-1, validated by DNS |

{{% notice warning %}}
Check model access before anything else. In this project, the first AWS account was not granted Amazon Bedrock access, which forced a change of account and model four days before the deadline (see [5.9 Lessons learned]({{< relref "/5-Workshop/5.9-Lessons-and-Roadmap" >}})).
{{% /notice %}}

#### Security reminders

- Never create long-lived access keys for the deployment; sign in with IAM Identity Center or `aws login`.
- Never paste tokens, passwords or MFA codes into chats, screenshots or videos.
- Keep `infra/cdk.json` free of secrets: it only holds configuration such as the model ID, budget and domain.
