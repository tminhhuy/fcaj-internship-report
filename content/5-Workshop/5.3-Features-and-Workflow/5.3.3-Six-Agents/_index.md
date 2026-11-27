---
title: "The six agents"
date: 2026-09-07
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

All six agents run on the same Lambda function, `agent-runner`; each state calls one agent through the Amazon Bedrock Converse API. Agent 1 runs first, and its output (contract type, roles, B2B or B2C) is passed to the agents that follow.

| Agent | Job | Tools | Output |
| --- | --- | --- | --- |
| 1. Context & roles | Supplier, buyer, B2B or B2C; correct industry terms; key clauses | Write JSON | Roles, key clauses, findings on context and balance |
| 2. Legal compliance | Checks each clause against laws in force | `search_law` (at most 3 calls) + write JSON | Non-compliant clauses with citations |
| 3. Disputes & remedies | Penalties, damages, termination, dispute forum, governing law | `search_law` (at most 3 calls) + write JSON | Dispute risks with citations |
| 4. Language & fairness | Vague, one-sided or ambiguous sentences; consistency between the Vietnamese and English versions | Write JSON | Sentences to clarify and why |
| 5. Recommendations | Merges findings from agents 1–4, prioritizes by severity, writes replacement wording | Write JSON | A table of suggestions, never written into the contract |
| 6. Executive summary | Parties, subject, value, term, 3–5 biggest risks, recommendation | Write JSON | One-page Smart Summary |

#### How the model is called

- **Prompts** follow the Amazon Nova prompting guide: a role, numbered instructions (MUST/NEVER for hard rules), one sample finding, the contract placed first and the task placed last.
- **Decoding** is greedy (temperature 0, topK 1), and the model can only answer through the JSON-writing tool. Nova's `<thinking>` section is dropped and never stored.
- **Prompt caching:** the system prompt and the contract block are placed before the cache point (Nova caches up to 20,000 tokens), so from the second agent on this part is read from the prompt cache. If Bedrock rejects the cache point, the call is resent without caching.
- **Output limits:** 4,000–6,000 output tokens per agent, raised automatically by 1.5× when an answer is cut off (ceiling 10,000).
- **Guardrails:** every call carries a content-filtering guardrail.
