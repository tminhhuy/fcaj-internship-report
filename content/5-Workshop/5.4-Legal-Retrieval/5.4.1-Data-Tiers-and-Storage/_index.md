---
title: "Data tiers and the storage unit"
date: 2026-09-07
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

#### Three data tiers

| Tier | Where it lives | Content |
| --- | --- | --- |
| Catalog (5 tables from the research framework) | `backend/sca/data/legal/` in the repository | 114 documents, each with 15 metadata fields; 13 jurisdictions; 10 cases; a compliance control set (clause framework C01–C20, counterparty due diligence, risk scale R = P × I); 25 official sources |
| Document store | S3 bucket `legal-corpus` | `sources/` downloaded originals, never edited; `staging/` articles waiting for review; `laws/` approved articles, the only prefix the Knowledge Base reads |
| Vector index | Amazon Bedrock Knowledge Base on Amazon S3 Vectors | 1,024-dimension vectors from Cohere Embed Multilingual v3, with metadata used for filtering |

#### One article, one file

Each approved article is a file `laws/{jurisdiction}/{document}/art-{article number}.md` with a `.metadata.json` file next to it. The storage unit is the same as the citation unit, so a retrieval result always carries the exact document ID and article number, and the verification step only has to compare these two values.

```json
{
  "metadataAttributes": {
    "country": "VN",
    "law_id": "ltm-2005",
    "article": "301",
    "title": "Luật Thương mại",
    "doc_type": "law",
    "binding": "binding",
    "sectors": ["all"],
    "effective_from": "2006-01-01",
    "status": "in_force",
    "source_url": "<link to the official text>",
    "lang": "vi"
  }
}
```

- `sectors` holds industry codes from the Vietnam Standard Industrial Classification (Decision 36/2025/QĐ-TTg), or `all`.
- `status` tells whether the document is still in force.
- `law_id` and `article` are the pair every citation must match.

#### Verification levels in the catalog

Each document in the catalog has one of three verification levels:

| Level | Meaning |
| --- | --- |
| `pending` | Taken from the research document, not yet checked |
| `desk_checked` | Document number or effective date checked against a public source |
| `verified` | A legal officer has read the official text; name and date recorded |

Only a reviewer can set a document to `verified`.
