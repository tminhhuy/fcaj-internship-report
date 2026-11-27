---
title: "Workshop"
date: 2026-09-07
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# SCA: Smart Contract Analyzer for Everyone on AWS

#### Overview

**SCA (Smart Contract Analyzer)** is an AI contract review system that runs entirely on AWS. It has been live at [smartca.app](https://smartca.app) since 25/11/2026. Six agents built on **Amazon Nova Pro** read contracts in Vietnamese, English or both, score them from 0 to 100, and keep a legal citation only when the knowledge base can find that exact article again. The system is advisory only: it never edits a contract, and it replaces personal identifiers with tokens before any AI model reads the text.

This workshop walks through the whole solution, from the problem and the research behind it to the architecture, the AWS Well-Architected pillars, the cost estimate, deployment, testing, results and clean-up.

You will see how to:

+ Run a **deterministic, event-driven pipeline** with Amazon EventBridge, Amazon SQS and AWS Step Functions, one execution per contract.
+ Protect personal data with **two copies of every contract**: a read-only signed original and a tokenized working copy that must pass a leak check before any agent runs.
+ Ground legal findings with **Amazon Bedrock Knowledge Bases on Amazon S3 Vectors**, then verify every citation a second time before it reaches the report.
+ Keep the processing functions in **private subnets with no Internet access**, calling Amazon Textract and Amazon Bedrock through VPC endpoints (AWS PrivateLink).
+ Deploy everything as code with **AWS CDK**, monitor it with Amazon CloudWatch and estimate the cost with the AWS Pricing Calculator.

#### Content

1. [Workshop overview]({{< relref "/5-Workshop/5.1-Overview" >}})
2. [Context and design principles]({{< relref "/5-Workshop/5.2-Context-and-Principles" >}})
3. [Features and workflow]({{< relref "/5-Workshop/5.3-Features-and-Workflow" >}})
4. [Legal retrieval]({{< relref "/5-Workshop/5.4-Legal-Retrieval" >}})
5. [Architecture on AWS]({{< relref "/5-Workshop/5.5-AWS-Architecture" >}})
6. [AWS Well-Architected pillars]({{< relref "/5-Workshop/5.6-Well-Architected" >}})
7. [Deployment and testing]({{< relref "/5-Workshop/5.7-Deployment" >}})
8. [Results and evaluation]({{< relref "/5-Workshop/5.8-Results-and-Evaluation" >}})
9. [Lessons learned, risks and roadmap]({{< relref "/5-Workshop/5.9-Lessons-and-Roadmap" >}})
10. [Clean up]({{< relref "/5-Workshop/5.10-Clean-Up" >}})
11. [Team and references]({{< relref "/5-Workshop/5.11-Team-and-References" >}})
