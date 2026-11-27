---
title: "Features and Scoreboard"
date: 2026-09-07
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

#### Features

The table follows the order in which users meet each feature in the web app.

| Feature | What the user sees | What the system does behind it |
| --- | --- | --- |
| Secure sign-in | Sign up with email, verify the email, turn on two-step verification with an authenticator app | Amazon Cognito enforces MFA (TOTP); the company code lives in the token, the API never accepts it from the request |
| Contract upload | Choose the type (signed or template), industry profile, governing law, contract and report language, and the user's own party; a SHA-256 fingerprint is computed in the browser | PDF, DOCX, PNG, JPG, TXT up to 10 MB and 50 pages; the browser uploads straight to S3 with a presigned POST, the file never passes through Lambda |
| OCR Scan | Reads text PDFs, scanned PDFs and photos | The PDF text layer first; English scans through Amazon Textract; Vietnamese scans through Amazon Nova Pro vision, 4 pages in parallel |
| Data protection | Privacy tab: which identifier types were replaced by tokens, where the data lives and when it is deleted | Detects signatures and personal data; the original goes to a locked read-only vault; the copy is tokenized and checked by LeakCheck |
| Six review agents | Findings tab: each finding has a severity, the clause, a verbatim quote, the legal basis and a recommended action | Amazon Nova Pro through the Converse API, one prompt and one JSON schema per agent ([5.3.3]({{< relref "/5-Workshop/5.3-Features-and-Workflow/5.3.3-Six-Agents" >}})) |
| Scoreboard | A 0–100 score and a score per criterion | Computed in code from validated findings (table below) |
| Smart Summary | One page: parties, subject, value, term, biggest risks, key clauses, what to negotiate first, recommendation | Agent 6 summarizes the results of agents 1–5; the recommendation must match the score band |
| Cite/Source Link | Document name, article, legal status, link to the official text; an "insufficient legal basis" label when it cannot be verified | VerifyCitations retrieves each citation again from the Knowledge Base ([5.4]({{< relref "/5-Workshop/5.4-Legal-Retrieval" >}})) |
| Suggested wording | Current sentence next to the proposed sentence, with a copy button | Agent 5; the system never writes into the contract |
| Live tracking | Progress through the 8 steps; an email when the report is ready | Job status in DynamoDB; Amazon SNS sends the email |
| Audit trail | Model, Region, guardrail, Knowledge Base, text extraction method, page count, citation count; time, tokens, cache use and law look-ups for each agent | Stored in the JSON report of every run |
| Law library and industry profiles | Browse 114 documents by jurisdiction, law group and status; edit mandatory clauses, risk appetite and weights per industry | Catalog in code; industry profiles stored in DynamoDB through `PUT /api/profiles/{code}` |
| Export and delete | Download JSON or PDF; a Delete data button | The API serves the report from the `reports` bucket; deleting removes the working copy, the token table and the report |
| Bilingual | Interface and report in Vietnamese or English | The report language is a parameter of each job |

#### Scoreboard

Default weights, which each company can adjust per industry:

| Criterion | Weight | Scored by |
| --- | --- | --- |
| Legal compliance | 30 | Agent 2 |
| Industry context and roles of the parties | 20 | Agent 1 |
| Disputes, penalties, jurisdiction | 20 | Agent 3 |
| Balance of obligations between party A and party B | 15 | Agent 1 |
| Language, clarity, mutuality | 15 | Agent 4 |

Each criterion starts at 100 and loses points by finding severity: **critical 45, high 25, medium 10, low 3**. A finding marked "insufficient legal basis" only loses half. The total is the weighted average:

| Total | Meaning |
| --- | --- |
| 85 and above | Ready to sign |
| 70–84 | Needs minor changes |
| 50–69 | Needs significant changes |
| Below 50 | Do not sign before fixing |

**Cap rule:** if there is at least one verified unlawful clause, the total cannot exceed 69, so a serious problem is never hidden by a good average.
