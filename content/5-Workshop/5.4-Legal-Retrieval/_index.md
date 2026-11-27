---
title: "Legal retrieval"
date: 2026-09-07
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

Agents never quote law from the model's memory. Every law passage comes from an approved library stored article by article, and every citation is retrieved a second time before it reaches the report.

"Training" in SCA means configuration and retrieval-augmented generation (RAG): a new law only needs new files, the model is never retrained. Amazon Nova Pro can also only be fine-tuned in us-east-1, which would move training data out of Asia Pacific.

#### Content

1. [Data tiers and the storage unit]({{< relref "/5-Workshop/5.4-Legal-Retrieval/5.4.1-Data-Tiers-and-Storage" >}})
2. [Query time: the search_law tool]({{< relref "/5-Workshop/5.4-Legal-Retrieval/5.4.2-Search-Law-Tool" >}})
3. [Citation verification and retrieval quality]({{< relref "/5-Workshop/5.4-Legal-Retrieval/5.4.3-Citation-Verification" >}})
