---
title: 'Generative Named-Entity Recognition'
date: 2026-06-22
permalink: /posts/2026/06/blog-post-4/
tags:
  - vision
  - language
---
Named-Entity Recognition (NER) is one of the foundational building blocks of modern document intelligence. Whether processing invoices, customs declarations, contracts, medical reports, or insurance claims, the goal is often the same: transform unstructured document content into structured business data that downstream systems can use automatically.

Traditionally, NER systems have relied on domain-specific machine learning models trained on manually labeled datasets and not without reason. With certain models such as LayoutLM [1] or LiLT [2] a fune-tuning on new domain, layout or distribution is needed to get more optimal predictions. That means with more distinct distributions comes more complexity (Ben Parker et al), since that means more labeling process including data validation, training and eval pipeline etc.
For example, given the sentence Invoice "INV-2026-001 was issued by Acme Corporation on 15 March 2026 for €12,500." a NER system could extract:

| Entity           | Type           |
|------------------|----------------|
| INV-2026-001     | Invoice Number |
| Acme Corporation | Organization   |
| 15 March 2026    | Date           |
| €12,500          | Amount         |

NER transforms raw text into structured information that applications can search, validate, analyse, and integrate into business processes [1]. However, in real-world enterprise scenarios, extracting entities from plain text is only a small part of the challenge. Most studies assume clean digital text as input. Unlike standard NLP datasets enterprise documents look very different. Some documents contain information not only in the text itself but also in the layout.

For example in invoice data we most likely will see headers and footers, tables, multi-column layouts, key-value pairs, stamps and signatures, form structures, whereas a contract document will be more densed in text. The position of a field on the page can be just as important as the textual content. A value appearing below "Invoice Number" carries a very different meaning than the same value elsewhere in the document. As a result, document NER often requires combining OCR text with layout and spatial information. This introduces more complexity in scaling NER on multiple document types.

Before an entity can be extracted, the system must first understand the document itself.

OCR Becomes Part of the Pipeline

Documents frequently arrive as PDFs, scans, images, emails, or photographs. Ingesting PDFs can be trivial if the documents contain the metadata–Just run a PDF reader and if necessary reconstruct the pages–for instance digital document. Other source of documents such as scans, photographs and etc., that contain no metadata necessitates an Optical Character Recognition (OCR) to convert visual content into machine-readable text.[2]

Unfortunately, OCR introduces its own challenges:

- Low image quality
- Skewed scans
- Handwritten annotations
- Complex layouts
- Tables and forms
- Multiple languages
- Missing or incorrectly recognized characters

But let's be optimistic and not be a downer, as many pre-trained OCRs, such as tesseract of PaddleOCR, already good enough for many use cases. Whenever there is decoding errors, these will be propagated into the extraction, retrieval or prediction pipeline.

In many enterprise products, each new customer domain requires its own specialized NER model.

For example:

|   Domain   |         Required Entities        |
|------------|----------------------------------|
| Invoices   | Invoice Number, Vendor, Amount   |
| Insurance  | Policy Number, Claim ID          |
| Logistics  | Container ID, Port, Vessel       |
| Healthcare | Diagnosis, Procedure Code        |
| Banking    | Account Number, Transaction Type |

This process is expensive, time-consuming, and difficult to repeat for every new customer or business domain.

Even after deployment, new document templates, regulatory changes, or evolving business requirements often necessitate additional labeling and retraining efforts.

As the number of domains grows, maintaining separate models quickly becomes operationally challenging.

Recent advances in Large Language Models (LLMs) have created a fundamentally different way to approach entity extraction.

Rather than treating NER as a token-classification problem, we can formulate it as a generative task: Given a document and a schema, generate the entities that match the requested fields.

For example:

```
Extract the following fields from this document:
- Customer Name
- Contract Number
- Effective Date
```


```
Response
{
  "customer_name": "Acme Corporation",
  "contract_number": "CTR-2026-847",
  "effective_date": "2026-03-15"
}
```

"This document" means the textual OCR predictions. Even better, we can construct the original layout or page by sorting the bounding boxes from top left to the bottom right, in order to preserve the original context. The model no longer needs a dedicated NER architecture trained specifically for that document type. Even better when you add a short description to the some attributes. Instead, the extraction behavior is described through prompts and schemas.
This is often called in-context learning from the mechanistic interpretability in machine learning. Basically, given enough constraint (contexts) an LLM can perform new "task" [3, 4, 5, 6]. This dramatically reduces onboarding effort for new domains. Also, LLMs can reason over larger contexts and use surrounding information to resolve ambiguities that are difficult for conventional sequence-labeling models [4]. Instead of maintaining separate models for every domain, organizations can use a single extraction platform that adapts through prompts, schemas, and business rules, which significantly reduces annotation costs, model maintenance and deployment complexity and time-to-market for new domains. The most important shift enabled by generative approach is moving from a model-centric workflow to a schema-centric workflow.

Before:

Traditional approach: New domain → Label data → Train model → Evaluate → Deploy

After:

Generative approach: New domain → Define schema → Configure prompt → Validate output → Deploy

![alt text](https://ksmdnl.github.io/images/mmir.png "Framework")
*Multimodal information extraction*

This does not eliminate the need for validation and quality assurance. Enterprise-grade systems still require governance, confidence estimation, business-rule validation, and human review where necessary. In some use cases, we also care about explainibility. For example, how do we know if the Company Name: Musterfirma GmbH is actually in the document? LLMs or big LLMs are usually pre-trained on web or large-scale dataset and it is possible that it generates what it had seen on the training set, which constitues an explanation about the prediction and which we actually want to avoid. One way to validate this is to match the OCRs output agains the generated entities, via string or semantic matching or the combination of both. That way we can now more sure whether LLM does NER, i.e., given a sarch space find the entities, or does a pure generation which can lead to hallucination. Here we have a multimodal workflow, vision from OCR and language from LLM. Instead of LLM, we can also use a vision-language model (VLM) so that we can add visual context, but it makes inference a lot slower and for the use case or business logic perhaps not a great trade-off.

A slightly more powerful method would be to use a zero-shot NER module such as GLiNER. With a similar method, we propate each chunk of the document to get as many predictions as the given classes and rank the predictions over all chunks. Also, we can do a batch processing here, optimising the inference time. Though, due to the model capacity this may not as generalizeable as the one with LLM. However, overall, the effort shifts away from expensive data annotation and model retraining toward schema design and validation logic.

Document-based NER is much more than identifying names and numbers in text. Real-world solutions must address OCR quality, document layouts, validation requirements, and multi-step processing workflows. Traditional machine-learning approaches often deliver strong performance within a specific domain but struggle to scale across many domains because each new use case requires additional labeling, training, and maintenance. Generative approach offers a compelling alternative. By leveraging the semantic understanding and instruction-following capabilities of large language models, organizations can define new extraction tasks through schemas and prompts rather than building dedicated models for every domain. As document processing systems continue to evolve, generative NER has the potential to transform entity extraction from a model-training problem into a configurable information extraction platform, enabling faster onboarding, lower maintenance costs, and greater scalability across diverse business domains.

Sources:

[1] NIST Message Understanding Conference (MUC-7) Named Entity Definition: https://www-nlpir.nist.gov/related_projects/muc/proceedings/ne_task.html

[2] NIST OCR Research and Document Processing Overview: https://www.nist.gov/itl/iad/image-group/programsprojects/legacy-projects/optical-character-recognition-ocr

[3] Zhan et al., Assessment of Generative Named Entity Recognition in the Era of Large Language Models, ACL Findings EMNLP 2026: https://aclanthology.org/2026.findings-emnlp.1706/

[4] Improving Few-Shot Named Entity Recognition for Large Language Models Using Structured Dynamic Prompting with Retrieval-Augmented Generation, npj Artificial Intelligence: https://www.nature.com/articles/s44387-025-00062-2

[5] Dong, Qingxiu et al. “A Survey on In-context Learning.” Conference on Empirical Methods in Natural Language Processing (2022).

[6] In-context Learning and Induction Heads https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html