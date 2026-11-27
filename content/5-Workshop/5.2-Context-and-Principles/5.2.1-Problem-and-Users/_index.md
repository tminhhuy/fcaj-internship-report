---
title: "Problem and target users"
date: 2026-09-07
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---

#### The business problem

- **Many reading rounds.** Legal officers, managers and signers all read the full text because there is no summary they can trust.
- **Two different legal frameworks.** B2B contracts fall under the Commercial Law 2005; B2C contracts are also governed by the Law on Protection of Consumer Rights 2023. Using the wrong template is a common mistake.
- **Personal data leaks.** Staff paste contracts containing full names, citizen ID numbers and bank accounts into public chatbots to save time.
- **Laws keep changing.** The Personal Data Protection Law 2025 took effect on 01/01/2026; the new Vietnam Standard Industrial Classification (Decision 36/2025/QĐ-TTg) took effect on 15/11/2025.

#### Target users

| User | Need | How SCA helps |
| --- | --- | --- |
| Legal officer | Review many contracts quickly without missing risky clauses | Findings ranked by severity, each with a citation to a law article |
| Manager, signer | Understand a contract in a few minutes to make a decision | One-page Smart Summary and the Scoreboard |
| Small business without a legal team | A basic legal view before signing | Warnings, recommendations and a reminder to consult a lawyer when the risk is high |
| IT and security administrator | Control where data lives and who can access it | Data stays in the company's own AWS account, encrypted with AWS KMS, logged by AWS CloudTrail |

#### Situations SCA must detect

| Situation | Legal basis | What SCA does |
| --- | --- | --- |
| A software outsourcing contract (B2B) sets a 15% penalty on the contract value | Article 301, Commercial Law 2005: a penalty may not exceed 8% of the value of the breached obligation | High-severity warning, cites Article 301, suggests lowering the penalty to at most 8% |
| An online shop's general terms (B2C) exclude the seller's liability | Article 25, Law on Protection of Consumer Rights 2023 | Flags the clause and names the relevant provision; reminds the user that standard-form contracts must be registered (Article 28) |
| A standard-form consumer contract forces disputes into arbitration | Article 25, Law on Protection of Consumer Rights 2023; Precedent 42/2021/AL | The dispute agent explains the risk that the clause is void |
| A signed contract contains full names, citizen ID numbers and bank accounts | Personal Data Protection Law 2025 (91/2025/QH15), Decree 356/2025/NĐ-CP | Tokenized before analysis; the original is locked read-only; logs never contain contract text |

#### Technical constraints in Vietnam and Singapore

- **OCR:** Amazon Textract does not list Vietnamese among its supported languages, so Vietnamese scans must be read by a multimodal model.
- **Model:** Amazon Nova Pro has no in-Region deployment in Singapore; calls must go through the APAC inference profile.
- **Guardrails:** the Standard tier understands Vietnamese but uses cross-Region processing within APAC; in Singapore the contextual grounding check is only available on the Classic tier (English, French, Spanish).
- **Personal data:** the Personal Data Protection Law 2025 requires data minimization and an impact assessment when personal data is transferred abroad.
