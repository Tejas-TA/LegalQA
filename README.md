**LegalQA**

LegalQA is a research project exploring Retrieval-Augmented Generation (RAG) for legal document question answering.
It compares single-pass RAG pipelines against a multi-hop iterative retrieval agent on U.S. Supreme Court case law from Case law access (CAP) project.

**Features**

**Dataset**: Built from the Caselaw Access Project (U.S. Supreme Court opinions, 1984–2014).
**Data Processing:**
1. Normalize raw case JSON into a slim format (cases_slim.jsonl).
2. Chunk opinions into passages for retrieval.

**Retrieval**: FAISS index with bge-small-en embeddings, plus optional cross-encoder reranker.

**Pipelines**:
1. **Baseline**: Single-pass retrieval → prompt LLM with top-k chunks.
2. **Iterative Agent:** Multi-step reasoning loop: retrieval → self-check → query refinement → final answer.

**Evaluation Metrics:**
1. Answer semantic similarity (vs. gold QA set).
2. Citation precision & recall.
3. Hallucination rate.
4. Hop effectiveness (did extra retrieval help?).

Hallucination rate.

Hop effectiveness (did extra retrieval help?).
