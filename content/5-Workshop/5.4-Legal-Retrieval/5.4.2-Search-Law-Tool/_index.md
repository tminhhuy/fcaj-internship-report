---
title: "Query time: the search_law tool"
date: 2026-09-07
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

#### How a law look-up works

1. The compliance agent or the dispute agent calls `search_law` with a query, at most 3 times per agent.
2. The Lambda function calls Bedrock `Retrieve` through a VPC endpoint: it asks for the 10 nearest results and filters inside the query on `country = VN` and `status = in_force`.
3. The results are filtered again by the contract's industry code: an article is kept when its `sectors` contains that code or `all`.
4. The agent receives the first 5 passages, each at most 700 characters, with the `law_id`, the article number and the document name, and may only cite by the pair `law_id` + article number.

Simplified request sent to the Knowledge Base:

```json
{
  "knowledgeBaseId": "<KB_ID>",
  "retrievalQuery": { "text": "mức phạt vi phạm hợp đồng thương mại tối đa" },
  "retrievalConfiguration": {
    "vectorSearchConfiguration": {
      "numberOfResults": 10,
      "filter": {
        "andAll": [
          { "equals": { "key": "country", "value": "VN" } },
          { "equals": { "key": "status", "value": "in_force" } }
        ]
      }
    }
  }
}
```

#### Laws the agents can cite today

The agents currently cite only the 8 Vietnamese laws below. International and foreign documents in the catalog are only retrieved on explicit request (today, the benchmark questions).

| `law_id` | Document | Number | In force from | Articles in the library |
| --- | --- | --- | --- | --- |
| `ltm-2005` | Commercial Law | 36/2005/QH11 | 01/01/2006 | 324 |
| `blds-2015` | Civil Code | 91/2015/QH13 | 01/01/2017 | 689 |
| `blttds-2015` | Civil Procedure Code | 92/2015/QH13 | 01/07/2016 | 517 |
| `tttm-2010` | Law on Commercial Arbitration | 54/2010/QH12 | 01/01/2011 | 82 |
| `shtt-2005` | Law on Intellectual Property (consolidated text 2026) | 50/2005/QH11 | 01/07/2006 | 240 |
| `bvqlntd-2023` | Law on Protection of Consumer Rights | 19/2023/QH15 | 01/07/2024 | 80 |
| `gddt-2023` | Law on Electronic Transactions | 20/2023/QH15 | 01/07/2024 | 53 |
| `bvdlcn-2025` | Personal Data Protection Law | 91/2025/QH15 | 01/01/2026 | 39 |

The library holds **2,024 articles** in total, built on 25/11/2026.

{{% notice note %}}
vbpl.vn lists the Commercial Law, the Civil Procedure Code and the Law on Electronic Transactions as "partly no longer in force". A reviewer must check the status of each article before setting the level to `verified`.
{{% /notice %}}
