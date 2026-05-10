# Phase 6 — Real-World Applications: RAG, Agents, Production (4 weeks, ~55h)

See `../ROADMAP.md` for the full plan. This README is the start-here checklist for Phase 6.

## Goal
Apply everything to ship something useful. This is where the theory pays off.

## Suggested week-by-week

### Week 1 — Embeddings + retrieval (~12h)
- [ ] sentence-transformers docs: https://www.sbert.net/
- [ ] Pick a vector DB and learn it well:
  - pgvector: https://github.com/pgvector/pgvector
  - FAISS: https://github.com/facebookresearch/faiss
  - Qdrant: https://qdrant.tech/documentation/
- [ ] Build a tiny semantic-search demo: embed ~1k docs, query with cosine similarity.

### Week 2 — RAG architecture (~14h)
- [ ] Anthropic — *Building Effective Agents* (essential): https://www.anthropic.com/engineering/building-effective-agents
- [ ] Lilian Weng — *LLM Powered Autonomous Agents*: https://lilianweng.github.io/posts/2023-06-23-agent/
- [ ] LlamaIndex docs (for RAG patterns): https://docs.llamaindex.ai/
- [ ] LangChain docs (skim — know what's there): https://python.langchain.com/docs/introduction/
- [ ] Topics: chunking strategies, hybrid search (BM25 + dense), reranking, query rewriting

### Week 3 — Agents + tool use + evaluation (~14h)
- [ ] Anthropic API docs (tool use, prompt caching, extended thinking): https://docs.anthropic.com/
- [ ] Anthropic Cookbook (RAG, tool use, agents notebooks): https://github.com/anthropics/anthropic-cookbook
- [ ] HuggingFace Agents Course: https://huggingface.co/learn/agents-course
- [ ] LangGraph (graph-based agent orchestration): https://langchain-ai.github.io/langgraph/
- [ ] RAGAS (RAG evaluation): https://docs.ragas.io/
- [ ] Lilian Weng — *Prompt Engineering*: https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/

### Week 4 — Capstone project (~15h)
Project below.

## Topics this phase covers
- **Embeddings:** sentence-transformers, OpenAI/Voyage/Cohere embedding APIs, pooling strategies
- **Vector databases:** pgvector, FAISS, Qdrant, Pinecone — when to use which
- **RAG architecture:** chunking, hybrid search (BM25 + dense), reranking, query rewriting
- **Agent loops:** ReAct, function calling, tool use, multi-step planning
- **Frameworks:** LangChain, LlamaIndex, LangGraph — what each is good at, when to skip them
- **Evaluation:** RAGAS, custom evals, LLM-as-judge (and its failure modes)
- **Production:** streaming, prompt caching, cost optimization, latency, observability (Langfuse, Arize)

## Capstone project — two parts

### Part 1 — Domain RAG agent
Pick a corpus you actually care about (your own notes, a textbook PDF, a codebase, a podcast transcript collection).
- [ ] Ingest + chunk the corpus (try 2 chunking strategies, compare)
- [ ] Build hybrid retrieval (BM25 + dense) + a reranker
- [ ] Wrap it in a chat interface (CLI is fine; web UI is a stretch goal)
- [ ] Build a small handcrafted eval set (~20–50 questions with expected answers)
- [ ] Evaluate retrieval quality (recall@k) and answer quality (with RAGAS or LLM-as-judge)

### Part 2 — Fine-tuned domain model in the loop
- [ ] Take the open model you fine-tuned in Phase 4 and integrate it as the generator
- [ ] Compare it to using Claude or GPT as the generator on the same RAG pipeline
- [ ] Document: what's the quality gap, where does the open model surprise you, what's the cost/latency tradeoff?

Suggested files:
- `01_embeddings_demo/`
- `02_chunking_experiments.ipynb`
- `03_rag_pipeline/`           ← end-to-end RAG with hybrid retrieval + reranker
- `04_eval_set.json`           ← your handcrafted eval questions
- `05_eval_runs.ipynb`         ← retrieval + answer quality numbers
- `06_open_vs_frontier_model.md`  ← writeup comparing your fine-tuned model to Claude/GPT

## Done when
- [ ] You have a deployed (even if just locally) end-to-end LLM application.
- [ ] You can articulate when fine-tuning is worth it vs when prompting + RAG is enough.
- [ ] You have an evaluation set and metrics, not just vibes.
- [ ] You can explain at least 2 production concerns (e.g., prompt caching, streaming, cost) you actually had to deal with.
