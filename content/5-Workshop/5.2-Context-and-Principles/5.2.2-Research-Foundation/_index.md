---
title: "Research foundation"
date: 2026-09-07
weight: 2
chapter: false
pre: " <b> 5.2.2. </b> "
---

Each study below leads directly to a design decision in SCA.

| Study | Key finding | Consequence for SCA |
| --- | --- | --- |
| [Hendrycks et al. (2021), CUAD](https://arxiv.org/abs/2103.06268), NeurIPS 2021 | 510 contracts, 41 clause types, more than 13,000 labels from legal experts; transformer models reached only "nascent" performance | Review needs a clause list defined by experts: SCA uses a list of mandatory clauses per industry and leaves the decision to people |
| [Guha et al. (2023), LegalBench](https://proceedings.neurips.cc/paper_files/paper/2023/hash/89e44582fd28ddfea1ea4dcb0ebbf4b0-Abstract.html), NeurIPS 2023 | 162 tasks covering six types of legal reasoning | Split the review into narrow tasks: six agents, each with one job and one JSON schema |
| [Dahl et al. (2024), Large Legal Fictions](https://arxiv.org/abs/2401.01301), Journal of Legal Analysis 16 | Chatbots hallucinated 58% (ChatGPT 4) to 88% (Llama 2) of the time on verifiable questions about US federal cases | No finding counts as grounded unless its source can be retrieved again |
| [Magesh et al. (2025), Hallucination-Free?](https://arxiv.org/abs/2405.20362), Journal of Empirical Legal Studies 22(2): 216–242 | Lexis+ AI, Westlaw AI-Assisted Research and Ask Practical Law AI still hallucinate 17–33% of the time despite retrieval-augmented generation (RAG) | RAG alone is not enough: SCA adds a deterministic VerifyCitations step that retrieves the exact document ID and article number again |
| [Lewis et al. (2020), Retrieval-Augmented Generation](https://papers.nips.cc/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html), NeurIPS 2020 | Provenance and knowledge updates are open problems for parametric-only models; retrieval addresses both | Laws live in an updatable Knowledge Base; no fine-tuning |
| [Greshake et al. (2023), Indirect Prompt Injection](https://arxiv.org/abs/2302.12173) | LLM-integrated applications blur the line between data and instructions; attackers plant instructions in data the application will read | Contract text is untrusted data, never an instruction |
| [OWASP (2025), LLM01: Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/) | Five mitigations: constrain model behavior, validate output format in code, segregate external content, least privilege, human approval | All five are implemented in SCA ([5.2.3 Design principles]({{< relref "/5-Workshop/5.2-Context-and-Principles/5.2.3-Design-Principles" >}})) |
| *Global Legal & Compliance Framework* (the team's internal research, 22/11/2026) | 5 data tables, 15 metadata fields, 3 source tiers; principle "complete the original text and its legal status first, legal conclusions later" | Structure of the law library and the document lifecycle in [5.4 Legal retrieval]({{< relref "/5-Workshop/5.4-Legal-Retrieval" >}}) |

{{% notice info %}}
The hallucination rates above come from US legal research. They are used here as evidence that a model must not be trusted to cite law on its own, not as a prediction of SCA's error rate.
{{% /notice %}}
