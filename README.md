<h2>LegalQA</h2>

LegalQA is a research project exploring Retrieval-Augmented Generation (RAG) for legal document question answering.
It compares single-pass RAG pipelines against a multi-hop iterative retrieval agent on U.S. Supreme Court case law from Case law access (CAP) project.

**Features**
**Dataset**: Built from the Caselaw Access Project (U.S. Supreme Court opinions, 1984–2014). https://static.case.law/us/

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

**Workflow**:
1. Data Prep: data/raw/json/*.json  →  data/processed/cases_slim.jsonl  →  data/processed/chunks.jsonl
2. Indexing: Embed chunks with BAAI/bge-small-en-v1.5 and store in FAISS.
3. QA Pipelines:retrieve(query) → build_prompt() → ask_llm(). Then, iterative_agent(query) with self-check & refinement.

**Evaluation**:

Compare baseline vs iterative using gold_qa.jsonl.

**Example**:

query = "What did the Supreme Court say about international child abduction?"

retrieved = retrieve(query, top_k=3)

prompt = build_prompt(query, retrieved)

answer = ask_llm(prompt)

print(answer)

**Results (sample)**:
1. **Baseline**: Higher semantic similarity to gold answers.
2. **Iterative**: More correct citations, but sometimes drifts in phrasing.

**Tech Stack**:
1. Python, FAISS, pandas
2. SentenceTransformers (BAAI/bge-small-en, all-MiniLM-L6-v2)
3. OpenAI GPT models for answering and self-checking

**Future Work**:
1. Use more sophisticated rerankers.
2. Enrich gold answers with full case syllabi.
3. Evaluate on other datasets (e.g., EUR-Lex, CUAD).
