---
title: "Lessons learned, risks and roadmap"
date: 2026-09-07
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

#### Lessons learned and troubleshooting

The biggest incident was not in the code but in model access: the original AWS account was not granted Amazon Bedrock access, which forced the team to change both the model and the deployment account four days before the deadline.

| Problem | Cause | Fix | Lesson |
| --- | --- | --- | --- |
| Bedrock could not be called: the Anthropic use-case form returned "not authorized", then "account is being verified" and "Operation not allowed" (20–23/11) | A new account with no billing history; AWS Support declined to enable Bedrock for the whole account | Deployed to a team member's AWS account that could already call Nova Pro; switched the model from Claude Sonnet 5 to Amazon Nova Pro (24/11) | Check model access and quotas in the first week; keep the model ID in configuration so it can change without touching logic |
| Nova Pro returned "doesn't support on-demand inference" in Singapore | Singapore has no in-Region Nova Pro | Use the inference profile `apac.amazon.nova-pro-v1:0`; grant IAM on both the profile ARN and the foundation model ARN in every Region | Read the model card (Regions, inference types) before choosing a model |
| Model calls broke after switching to Nova | The `thinking` parameter is Anthropic-only; Nova only accepts a subset of JSON Schema for tools and caches at most 20,000 tokens; the old OCR used Anthropic's own InvokeModel format | Removed `thinking`; wrote tool schemas in the subset Nova accepts; drop the cache point automatically when it is rejected; rewrote OCR with the Converse API (document block for PDF, image block for images) | Use the Converse API for every call so the code depends less on one model; keep tests with a mocked Bedrock |
| A new stack produced no citations | The Knowledge Base is empty until the law library is loaded | Made loading the 8 laws a mandatory deployment step ([5.7.2]({{< relref "/5-Workshop/5.7-Deployment/5.7.2-Deploy-the-Stack" >}})) | Data is part of the system: check it like infrastructure |
| law-harvest received HTTP 403 from vbpl.vn for all 5 laws (25/11) | vbpl.vn blocks requests from AWS and has moved to a new site where the full text sits inside page scripts | Did not bypass the block; downloaded the full text in a browser, built all 8 laws by hand (2,024 articles) and deployed them with the stack | External data sources may block cloud traffic; always keep a controlled manual load path |
| Sign-in failed with "Unsupported sign-in step: CONTINUE\_SIGN\_IN\_WITH\_TOTP\_SETUP" (25/11) | The user pool requires MFA but the sign-in page had no step to set up an authenticator app | Added a QR code and TOTP code step to the sign-in page; the sign-in session lasts 10 minutes | Test the new-user flow on the live stack, not only with accounts that are already set up |
| `cdk deploy` on a Mac failed with `ModuleNotFoundError: aws_cdk` | CDK ran `python3 app.py` with the machine's other Python instead of the virtual environment | Pass `--app ".venv/bin/python3 app.py"` | Record tool versions and environments in the deployment guide |
| Connecting the smartca.app domain | CloudFront only accepts ACM certificates from us-east-1; the `.app` domain requires HTTPS | Requested a free ACM certificate, validated with a CNAME record, set `domainName` and `certificateArn`, pointed the root record to CloudFront | The domain is configuration, not code |
| Sensitive information exposed while working: a GitHub token pasted into a chat, an MFA QR code visible in the demo video | Rushed steps during deployment and recording | Regenerated the token (fine-grained, read-only, 7-day expiry); blurred the QR code in the video | Paste secrets only where they are used; review videos and screenshots before sharing |

#### Risks

The three biggest risks are wrong legal findings, identifier leaks and the rules on transferring personal data abroad; each has at least two layers of mitigation.

| Risk | Likelihood | Impact | Mitigation |
| --- | --- | --- | --- |
| A wrong legal finding, or one based on a repealed text | Medium | High | Conclude only with verified citations; legal status in metadata; human review; the report always says it does not replace a lawyer |
| Identifier leak | Medium | High | Rules + regex, then a second Guardrails scan; LeakCheck blocks any copy that still has identifiers |
| Personal data transferred abroad (APAC profile, especially Vietnamese scans) | High in production | High | Data minimization; the company prepares an impact assessment; later, tokenize in the user's browser |
| Prompt injection inside a contract | Medium | High | Prompt attack filter; data tags; score computed from validated JSON; fix the failing eval case of 25/11 |
| Wrong Vietnamese diacritics from OCR on poor scans | Medium | Medium | Prefer the PDF text layer; flag low-quality pages for the user to check |
| Low cross-Region quota for Nova Pro | Medium | Medium | SQS + concurrency limit + retries with backoff; request a quota increase |
| Cost above plan | Medium | Medium | AWS Budgets at 50%/80%; 50-page limit and output token limits; cost per contract on the dashboard |
| Model changes or is retired | Low | Medium | Model ID in configuration; evals in the pipeline catch quality drops; the model has already been switched once successfully |

**Current limits:** cites Vietnamese law only (8 laws); two industries; one demo company; no comparison between contract versions; performance and OCR figures not fully measured yet ([5.8]({{< relref "/5-Workshop/5.8-Results-and-Evaluation" >}})).

#### Open items

- Load the 8 laws into the Knowledge Base, then run the analysis and the benchmark questions again.
- Fix the failing prompt injection eval case and run the smoke suite again.
- Measure p95 processing time, run the 50-contract load test and measure CER on 10 scanned Vietnamese pages.
- Penetration test under the AWS policy (no DoS testing), with the account owner's consent.
- The team reviews and adjusts Figures 1–5 in draw.io.

#### After the MVP

- An agent that looks up foreign law when a contract chooses a foreign governing law.
- Bring the C01–C20 clause framework into the context agent.
- Route lighter agents to Amazon Nova Lite.
- Compare contract versions.
- One AWS account per company.
- Tokenize in the browser before upload.
