# Natural Language Processing Essentials

NLP includes classification, extraction, search, translation, summarization, and generation. The right solution can be a rule, a classical classifier, an encoder, or a generative model.

## Tool map

| Tool or method | Good fit | Limitation to test |
|---|---|---|
| Rules and regular expressions | Stable formats and exact patterns | Variations and ambiguous language |
| TF-IDF plus a linear model | A strong, inexpensive text-classification baseline | Semantic generalization |
| NLTK | Teaching linguistic concepts and working with corpora | Required language resources and their versions |
| spaCy | Tokenization and linguistic pipelines | Language/model coverage and domain transfer |
| Transformers | Pretrained model definitions and training/inference workflows | Model-specific processors and runtime support |
| Sentence Transformers | Embeddings and retrieval/reranking | Domain and language quality |

spaCy's trained components provide annotations such as entities and syntax; tokenization alone does not provide those learned annotations.[^18] Transformers now spans text, images, audio, video, and multimodal model definitions. Do not assume every task or checkpoint supports every backend.[^19]

## A tokenizer without a model download

Requires spaCy. This blank English pipeline only tokenizes; it does not claim entity recognition or part-of-speech predictions.

```python
import spacy

nlp = spacy.blank("en")
document = nlp("AI tools should be evaluated on realistic tasks.")
print([token.text for token in document])
```

For named entities or tagging, install a compatible trained pipeline for the target language and record its version. Likewise, NLTK tokenizers may need separately downloaded resources: follow the installed release's instructions rather than assuming an old resource name is sufficient.

## Work from a baseline

Create a small labeled corpus with a clear target and annotation instructions. Split by source or author when duplicate language could leak between sets. For text classification, try TF-IDF and a linear classifier before fine-tuning a transformer. Evaluate macro-F1 and per-class errors if class frequency is uneven.

Compare language varieties, document length, spelling noise, and domain terminology. English test results do not establish multilingual performance. Preserve meaningful punctuation, negation, and formatting; aggressive cleaning can remove the signal you need.

## Semantic search

An embedding maps an input into a vector space. A bi-encoder can retrieve candidates efficiently, while a cross-encoder reranks query-document pairs. Reranking adds computation, so measure quality and latency together.[^20]

Similarity is not a calibrated probability that a passage answers a question. Evaluate retrieval with labeled relevant passages. See [RAG and vector search](RAG_and_Vector_Search.md) for grounding generated answers in source material.

## Generative text tasks

For extraction, define a schema and validate fields against the source. For summarization, assess omissions and unsupported additions separately from writing quality. For translation, evaluate meaning and domain terms with competent speakers. For question answering, distinguish a correct answer from a correct answer with a supporting citation.

When loading downloadable checkpoints, record the repository revision, tokenizer, license, and any custom-code requirement. Avoid enabling remote model code without reviewing the source.

**Exercise:** Build a 50-example error-analysis set containing abbreviations, negation, mixed languages, and missing context. Compare a simple baseline with a model-based approach.

[Back to AI Essentials Hub](README.md)

## Sources

[^18]: Explosion. [spaCy 101: Everything you need to know](https://spacy.io/usage/spacy-101). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^19]: Hugging Face. [Transformers](https://huggingface.co/docs/transformers/index). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^20]: Sentence Transformers contributors. [Retrieve & Re-Rank](https://sbert.net/examples/sentence_transformer/applications/retrieve_rerank/README.html). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
