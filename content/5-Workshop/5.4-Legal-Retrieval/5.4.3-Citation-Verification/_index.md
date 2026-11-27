---
title: "Citation verification and retrieval quality"
date: 2026-09-07
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

#### VerifyCitations

The `cite-score-report` Lambda function normalizes the article number ("Điều 301 khoản 1" → `301`), then calls `Retrieve` again with a filter on the `law_id` of the cited document. A citation is kept only when one result matches both the document ID and the article number.

| Case | In the report | Effect on the score |
| --- | --- | --- |
| The exact article is found again | Citation kept, with its legal status and a link to the official text | Full deduction by severity; the "unlawful" flag triggers the cap at 69 |
| Not found | Citation removed; the finding is labelled "insufficient legal basis", at most medium severity | Only half the deduction; never treated as unlawful |
| English report | Adds the Guardrails grounding score from 0 to 1: does the law passage support the finding? | For information only, the score does not change |
| The quote does not match the contract text verbatim | A warning in the report | The score does not change |

The grounding check is only available for English reports because in Singapore this feature exists only on the Classic tier (English, French, Spanish).

#### Measuring retrieval quality

The file `kb_golden.json` holds **70 benchmark questions** (Vietnam, international treaties, EU, United States, United Kingdom, Singapore, Japan), each with the article that must be found.

- After every load, the `kb-eval` Lambda function runs the questions, emails the results and writes the `KbGoldenHitRate` metric to Amazon CloudWatch.
- A question about a document that has not been loaded yet is recorded as "not loaded", not as a miss.
- The verification levels (`pending`, `desk_checked`, `verified`) described in [5.4.1]({{< relref "/5-Workshop/5.4-Legal-Retrieval/5.4.1-Data-Tiers-and-Storage" >}}) tell readers how far each document has been checked by a person.

How new laws reach the Knowledge Base, with a human review step, is described in lane B of [5.6.4 Operational Excellence]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.4-Operational-Excellence" >}}).
