---
title: "Workshop overview"
date: 2026-09-07
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Executive summary

SCA replaces several rounds of manual contract reading with one automatic report: a score from 0 to 100, findings ranked by severity, suggested wording changes and a one-page summary for the signer. Every legal finding must point to an article in the approved law library; if the article cannot be found again, the finding is downgraded.

The design rests on three ideas:

1. **Two copies of every contract.** The signed original is locked read-only. The AI only reads a copy in which names, citizen ID numbers, tax codes and bank accounts have been replaced with tokens, and that copy must pass a leak check first.
2. **Legal knowledge lives outside the model.** Laws are stored article by article, with their legal status and industry codes, and retrieved through Amazon Bedrock Knowledge Bases. The model is not fine-tuned.
3. **A deterministic pipeline on AWS.** AWS Step Functions orchestrates every step, Amazon Bedrock Guardrails filters every model call, and the whole infrastructure is code (AWS CDK).

#### Key figures (as of 27/11/2026)

| Item | Value | Notes |
| --- | --- | --- |
| Status | Live at smartca.app, Region ap-southeast-1 (Singapore) | Deployed on 25/11/2026 with AWS CDK |
| AI model | Amazon Nova Pro through the APAC inference profile | Chosen on 24/11/2026 |
| Agents | 6 | Context, compliance, disputes, language, recommendations, summary |
| Law library | 8 Vietnamese laws for the agents; a catalog of 114 documents in 13 jurisdictions | 70 benchmark questions measure whether the right article is found |
| Automatic evaluation on 25/11 | 28/31 checks passed (90%) on 3 sample contracts | Details in [5.8 Results and evaluation]({{< relref "/5-Workshop/5.8-Results-and-Evaluation" >}}) |
| One live run (6-page contract) | 6 agents, 47.7 seconds in total, ≈ $0.10 of model cost | 57% of input tokens read from the prompt cache |
| Pilot cost | ≈ $125/month for 300 contracts | Variable cost ≈ $0.17 per contract |
| AWS services | Amazon Textract, Amazon Bedrock, AWS Lambda and 19 supporting services | Fully serverless |

#### Scope

The current scope is Vietnamese law, two industries (IT/software services, B2B; online retail, B2C), and contracts written in Vietnamese, English or both. SCA supports due diligence; it does not replace a lawyer's opinion.

#### How this workshop is organized

| Part | What it covers |
| --- | --- |
| [5.2 Context and design principles]({{< relref "/5-Workshop/5.2-Context-and-Principles" >}}) | The business problem, target users, the research behind the design and the seven design principles |
| [5.3 Features and workflow]({{< relref "/5-Workshop/5.3-Features-and-Workflow" >}}) | What users see, how the score is computed, the 8-step journey of a contract and the six agents |
| [5.4 Legal retrieval]({{< relref "/5-Workshop/5.4-Legal-Retrieval" >}}) | How laws are stored, searched and how each citation is verified |
| [5.5 Architecture on AWS]({{< relref "/5-Workshop/5.5-AWS-Architecture" >}}) | The end-to-end data path on AWS and why each service was chosen |
| [5.6 Well-Architected pillars]({{< relref "/5-Workshop/5.6-Well-Architected" >}}) | Security, reliability, performance, operations, cost (with the AWS Pricing Calculator estimate) and sustainability |
| [5.7 Deployment and testing]({{< relref "/5-Workshop/5.7-Deployment" >}}) | Prerequisites, step-by-step deployment, API tests with cURL and failure scenarios |
| [5.8 Results]({{< relref "/5-Workshop/5.8-Results-and-Evaluation" >}}) to [5.11 Team and references]({{< relref "/5-Workshop/5.11-Team-and-References" >}}) | Results, lessons learned, risks, clean-up, team and references |
