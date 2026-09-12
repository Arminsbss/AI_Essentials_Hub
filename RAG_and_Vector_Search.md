# Retrieval-Augmented Generation and Vector Search

Retrieval-augmented generation (RAG) supplies relevant external information to a model before it answers. It is useful when answers must reflect a changing corpus, private documents, or traceable evidence. Retrieval does not guarantee that a generated claim is correct.

The original RAG research combined a pretrained generator with non-parametric retrieval memory. Modern application pipelines vary, so the historical paper should be read as a foundation rather than a recipe for every current system.[^52]

## A practical architecture

```mermaid
flowchart LR
    A[Approved documents] --> B[Parse and preserve metadata]
    B --> C[Chunk and index]
    Q[Question and user identity] --> R[Retrieve authorized candidates]
    C --> R
    R --> S[Rerank and select evidence]
    S --> G[Generate answer with citations]
    G --> V[Validate and return]
```

The identity and permissions boundary must hold throughout retrieval, generation, caching, and logging. Access filters belong in the retrieval system; a model instruction is not a substitute.

## Build the corpus

Record document ID, version, source location, ownership, permissions, and timestamps. Parse tables and layout carefully. Split at meaningful boundaries where possible and keep enough context to interpret each chunk. Preserve a mapping to the source page or section.

Choose chunk size by evaluating retrieval and answer quality. There is no universal optimal length or overlap. Keep duplicate and superseded content under control; a well-ranked obsolete policy can produce a confidently wrong answer. Propagate deletion and permission changes to chunks, embeddings, caches, and logs.

## Compare retrieval methods

| Method | Strength | Failure to test |
|---|---|---|
| Keyword / lexical search | Exact names, codes and specialized terms | Paraphrases |
| Dense embeddings | Semantic similarity | Exact identifiers and out-of-domain language |
| Hybrid retrieval | Combines lexical and semantic signals | Fusion and tuning complexity |
| Cross-encoder reranking | Jointly evaluates query and candidate text | Added latency and cost |

Sentence Transformers documents a retrieve-then-rerank pipeline using efficient candidate selection followed by a cross-encoder. Its benefit must be measured on the actual corpus.[^20]

## Index selection

If you already use PostgreSQL, pgvector is a useful candidate for integrated vector search. Exact search and approximate indexes trade off different costs. With approximate indexes, filtering can reduce returned results; iterative scans are one available mitigation. Test recall under realistic tenant and permission filters.[^28]

A dedicated vector database may fit other scale or operations requirements. Compare deletion behavior, backup, filtering, index rebuilds, availability, and total cost. Avoid adding a separate service solely because the application uses embeddings.

## Evaluate retrieval separately

Create question-to-relevant-passage labels. Measure Recall@k to assess whether relevant evidence was retrieved, and ranking metrics when position matters. Then evaluate answer correctness, citation support, abstention, and end-to-end latency. A high retrieval score does not establish a good answer.

This standard-library example calculates Recall@k for one labeled query:

```python
def recall_at_k(relevant_ids, ranked_ids, k):
    relevant = set(relevant_ids)
    if not relevant:
        raise ValueError("Use an explicit policy for queries with no relevant documents")
    if k < 1:
        raise ValueError("k must be positive")
    return len(relevant.intersection(ranked_ids[:k])) / len(relevant)

assert recall_at_k({"a", "c"}, ["a", "b", "c"], 2) == 0.5
print(recall_at_k({"a", "c"}, ["a", "b", "c"], 3))
```

## RAG, long context, or fine-tuning?

Try full-document context when the relevant material is small and stable enough. Try retrieval when the corpus is large, frequently updated, or selectively accessible. Try fine-tuning for repeated behavior or domain adaptation; it is not a reliable document-permission mechanism or a substitute for an updateable source store.

Long-context research found sensitivity to evidence position in the tested models. That result motivates a current-model position test; it does not prove the same degradation for every 2026 model.[^54]

**Exercise:** Compare lexical, dense, and hybrid retrieval on a fixed set. Add unanswerable questions, conflicting documents, deleted documents, and unauthorized documents. Use [the R&D report](RD_Report.md) to define acceptance gates before tuning.

[Back to AI Essentials Hub](README.md)

## Sources

[^20]: Sentence Transformers contributors. [Retrieve & Re-Rank](https://sbert.net/examples/sentence_transformer/applications/retrieve_rerank/README.html). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^28]: pgvector contributors. [pgvector README](https://github.com/pgvector/pgvector). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^52]: Lewis et al.. [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401). First submitted 2020-05-22; revised 2021-04-12; NeurIPS 2020. Reviewed 2026-09-12–2026-09-13.

[^54]: Liu et al.. [Lost in the Middle: How Language Models Use Long Contexts](https://arxiv.org/abs/2307.03172). First submitted 2023-07-06; revised 2023-11-20. Reviewed 2026-09-12–2026-09-13.
