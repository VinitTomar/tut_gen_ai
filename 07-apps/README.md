# Phase 7 — Real-World Applications: RAG, Agents, Production at Scale (4–5 weeks, ~65h)

See `../ROADMAP.md` for the full plan. This README is the start-here checklist for Phase 7.

## Goal
Ship a production-grade LLM application. Your distributed-systems background means this phase should look more like "real production engineering" than "tutorial RAG demo."

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

## Capstone project — build a system, not a demo

Required ingredients:

1. **Domain RAG agent** over a corpus you care about (your own notes, codebase, podcast transcripts, etc.) with hybrid retrieval (BM25 + dense) and reranking.
2. **Real eval set** — handcrafted golden dataset (50+ examples), retrieval metrics (recall@k), generation metrics (faithfulness, relevance), LLM-as-judge with at least one calibration check.
3. **Cost & latency budgets enforced** — p95 latency target, $/1k-queries target, with measurements.
4. **Observability** — every request traced, every eval run logged, failure modes visible (Langfuse or equivalent).
5. **Two generator options compared:** Claude/GPT API vs your fine-tuned open model (Phase 4) served via your inference stack (Phase 5). Document the quality gap and where each wins.
6. **Deployed** somewhere reachable (not just localhost). Render, Fly.io, Modal, or self-hosted.

Write the whole thing up publicly. This is your capstone artifact and a key piece of your leadership track record.

Suggested files:
- `01_embeddings_demo/`
- `02_chunking_experiments.ipynb`
- `03_rag_pipeline/`              ← end-to-end RAG with hybrid retrieval + reranker
- `04_eval_set.json`             ← 50+ handcrafted golden questions
- `05_eval_runs.ipynb`           ← retrieval (recall@k) + generation (faithfulness/relevance) numbers
- `06_observability/`            ← Langfuse traces, dashboards, failure-mode notes
- `07_open_vs_frontier_model.md` ← fine-tuned model vs Claude/GPT: quality, cost, latency
- `08_deploy/`                   ← deployment config (Render/Fly.io/Modal/self-hosted)

## Done when
- [ ] You have a deployed, evaluated, observable LLM application.
- [ ] You can articulate when fine-tuning is worth it vs prompting + RAG, with numbers.
- [ ] You have an evaluation set and metrics, not just vibes.
- [ ] The system handles at least 10 concurrent users without falling over — you're an experienced engineer; make this real.

## Leadership & Visibility (this phase)
See the full section in `../ROADMAP.md`. For Phase 7:
- [ ] **Capstone writeup** — the public post on your deployed, evaluated, observable app is the headline artifact of the whole roadmap. Architecture, eval methodology, cost/latency numbers, fine-tuned-vs-frontier comparison.
- [ ] **OSS PR** — `langchain`, `llamaindex`, `ragas`, or `langfuse` — a fix or integration from something you hit while building.
- [ ] **Talk + internal** — pitch this as a talk and, if it maps to your company, as the internal LLM use case you can now point to as "led production AI engineering at scale."
