# Generative AI / LLM-from-Scratch Learning Roadmap

## Context

You want to learn generative AI deeply, in this order:
1. **Understand internals deeply** — build transformers from scratch, understand attention/tokenization
2. **Reproduce ML research papers** — read and re-implement landmark LLM papers
3. **Build real-world LLM applications** — RAG, agents, fine-tuned domain models

**Your starting point:** basic Python, basic neural-network intuition.
**Time budget:** 10–15 hours/week.
**Hardware:** TBD — recommendations below.
**Tooling:** `uv` for Python environments + dependencies (see Tooling section). One isolated `pyproject.toml` per phase folder.

---

## Refinement Note (added in second planning pass)

The original plan used `pip install` in `00-prereqs/README.md`. That gets refined here to use **`uv`** (Astral's modern Rust-based package manager) with **one `pyproject.toml` per phase folder** for isolated environments. Rationale: 10–100× faster, single tool replaces pip+venv+pyenv, and per-phase isolation prevents Phase-1 numpy from breaking Phase-4's `transformers`/`peft` constraints. Concrete file changes are captured in the *Implementation Changes* section near the bottom of this file.

This roadmap is sequenced so each phase compounds on the previous one. The total horizon is ~10–14 months at your pace (calibrated for an engineering team lead with a demanding day job), organized into eight phases (0–7). Every phase ends with a concrete buildable project so you have something to point at, not just notes. A parallel **Leadership & Visibility Track** runs alongside Phase 2 onward — this is what turns "learned LLMs" into "leads AI engineers."

Workspace lives in `/Users/vinittomar/development/study-gen-ai/`. Suggested structure (create as you go):

```
study-gen-ai/
├── 00-prereqs/             # NumPy, PyTorch, math notebooks
├── 01-neural-nets/         # micrograd, makemore
├── 02-transformer/         # nanoGPT, your scratch GPT
├── 03-modern-llm/          # LLaMA-style impl, tokenizers
├── 04-post-training/       # SFT, LoRA, DPO experiments
├── 05-ml-systems/          # vLLM, inference benchmarks, quantization
├── 06-paper-reproductions/ # systems-focused paper repros
└── 07-apps/                # RAG, agents, production
```

---

## Hardware Recommendation

You don't need a GPU to start. Plan to spend $0 for the first ~2 months, then a small budget once you start training transformers.

| Phase | Hardware | Cost |
|---|---|---|
| Phase 0–1 (math, micrograd) | Laptop CPU | Free |
| Phase 2 (nanoGPT-scale training) | Google Colab free tier or Kaggle (T4 GPU, free) | Free |
| Phase 3–4 (training/fine-tuning) | Colab Pro ($10/mo) **or** RunPod / Lambda Labs A100 (~$0.40–0.80/hr, pay-as-you-go) | ~$10–50/mo |
| Phase 5 (ML systems, inference benchmarks) | RunPod / Lambda Labs (A100 or L40S, pay-as-you-go) — real GPUs needed for serving benchmarks | ~$30–100/mo |
| Phase 6–7 (paper reproductions, apps) | Same as Phase 5 + small spend on Anthropic/OpenAI API for app phase | ~$30–120/mo |

If you have an Apple Silicon Mac (M1/M2/M3), MPS backend in PyTorch handles Phase 0–2 comfortably; switch to cloud GPU at Phase 3.

---

## Tooling — Python Package Management

Standardize on **`uv`** (https://docs.astral.sh/uv/) for all local work.

### Why uv
- Replaces `pip` + `venv` + `pip-tools` + `pyenv` in one binary
- 10–100× faster than pip for installs and resolves
- Lockfiles by default (`uv.lock`) — reproducible envs
- Manages Python versions too (`uv python install 3.12`)
- Has become the de facto default for new Python projects in the ML ecosystem in 2025–2026

### Per-phase isolation
Each phase folder gets its own `pyproject.toml` and `.venv`. Reasons:
- Phase 4 (`transformers`/`peft`/`trl`) pins constraints that may conflict with Phase 0's bare numpy stack
- Lets you blow away one phase's env without nuking the others
- Phase READMEs can document phase-specific dependency lists cleanly

### Workflow per phase (run once when entering a phase)
```bash
cd 0X-phase-name
uv init                          # creates pyproject.toml + .venv
uv add numpy matplotlib torch    # adds deps, updates lockfile
uv run jupyter lab               # runs in the phase's venv
```

Day-to-day: `uv run python script.py`, `uv add <pkg>`, `uv sync` (after pulling). No need to manually `source .venv/bin/activate` — `uv run` handles it.

### Colab/Kaggle exception
Cloud notebook environments (Colab, Kaggle) come with their own pre-installed Python and pip. Don't try to use uv there — just `!pip install <pkg>` in a cell. uv is for your local laptop only.

### Initial bootstrap
```bash
# install uv (macOS, one-liner)
curl -LsSf https://astral.sh/uv/install.sh | sh

# verify
uv --version
```

---

## Phase 0 — Foundations: Math + PyTorch (3 weeks, ~35 hrs)

**Goal:** be fluent enough in linear algebra, calculus, probability, and PyTorch to read transformer code without flinching.

### Topics
- **Linear algebra:** vectors, matrices, matmul, dot product, transpose, eigenvectors (intuition)
- **Calculus:** derivatives, partial derivatives, gradients, chain rule (this is what backprop *is*)
- **Probability:** distributions, expectation, variance, Bayes' rule, log-likelihood, cross-entropy, KL divergence, softmax
- **NumPy:** array ops, broadcasting, indexing
- **PyTorch:** tensors, autograd, `nn.Module`, optimizers, GPU/MPS device, training loop

### Resources

**Linear algebra**
- 3Blue1Brown — *Essence of Linear Algebra*: https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab

**Calculus**
- 3Blue1Brown — *Essence of Calculus* (single-variable foundation): https://www.youtube.com/playlist?list=PLZHQObOWTQDMsr9K-rj53DwVRMYO3t5Yr
- Khan Academy — *Multivariable Calculus* (do the **partial derivatives** + **gradient** units — these are what backprop is built on; the rest is optional): https://www.khanacademy.org/math/multivariable-calculus

**Probability**
- StatQuest — *Statistics Fundamentals* playlist (distributions, expectation, variance, Bayes): https://www.youtube.com/playlist?list=PLblh5JKOoLUK0FLuzwntyYI10UQFUhsY9
- StatQuest individual videos — *softmax*, *cross-entropy*, *KL divergence*, *log-likelihood*: https://www.youtube.com/@statquest
- *Mathematics for Machine Learning* — Ch 6 (Probability & Distributions), free PDF: https://mml-book.github.io/

**NumPy**
- CS231n — *Python + NumPy tutorial* (classic, written for ML beginners): https://cs231n.github.io/python-numpy-tutorial/
- NumPy official *Quickstart* (use as reference): https://numpy.org/doc/stable/user/quickstart.html

**PyTorch**
- PyTorch — *Learn the Basics*: https://pytorch.org/tutorials/beginner/basics/intro.html
- PyTorch — *Deep Learning with PyTorch: 60-min Blitz*: https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html

**Bridge to Phase 1**
- 3Blue1Brown — *Neural Networks* (great intuition primer): https://www.youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi

**Reference book**
- *Deep Learning* by Goodfellow, Bengio, Courville — Chapters 2–4 only: https://www.deeplearningbook.org/

### Project: **Linear regression and a 2-layer MLP from scratch in NumPy**, then re-implement both in PyTorch. Train on a tiny synthetic dataset. Confirm gradients match between your manual implementation and `torch.autograd`.

### Done when
- You can write a forward + backward pass for an MLP in pure NumPy without looking it up.
- You can explain cross-entropy loss in one sentence.
- You can write a PyTorch training loop from a blank file.

---

## Phase 1 — Neural Networks, Backprop, First Language Model (4 weeks, ~50 hrs)

**Goal:** internalize backpropagation and build your first language model (character-level). This phase is mostly Andrej Karpathy's *Neural Networks: Zero to Hero* series — it is the single best resource that exists for this.

### Karpathy's *Neural Networks: Zero to Hero* series (work through in order, code along — don't just watch)
- Full playlist: https://www.youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ
- Code repo: https://github.com/karpathy/nn-zero-to-hero

1. **micrograd** — build autograd from scratch in ~100 lines: https://www.youtube.com/watch?v=VMj-3S1tku0 · repo: https://github.com/karpathy/micrograd
2. **makemore part 1 (bigram)** — first language model: https://www.youtube.com/watch?v=PaCmpygFfXo
3. **makemore part 2 (MLP)** — Bengio 2003 paper reproduced: https://www.youtube.com/watch?v=TCH_1BHY58I
4. **makemore part 3 (activations, gradients, BatchNorm)**: https://www.youtube.com/watch?v=P6sfmUTpUmc
5. **makemore part 4 (manual backprop)**: https://www.youtube.com/watch?v=q8SA3rM6ckI
6. **makemore part 5 (WaveNet)**: https://www.youtube.com/watch?v=t3YJ5hKiMQ0
- Bengio 2003 paper for context: https://www.jmlr.org/papers/volume3/bengio03a/bengio03a.pdf

### Project: **Character-level name generator**
Train an MLP language model on `names.txt` (Karpathy provides it). Sample new names. Then extend it to a different dataset of your choice (e.g., generate fake company names, fake Pokémon names) — the dataset swap forces you to understand the pipeline, not just copy.

### Done when
- You can derive backprop for a 2-layer MLP on paper.
- You can explain what `loss.backward()` actually does under the hood.
- You understand why gradients vanish/explode and what initialization does about it.

---

## Phase 2 — Transformer From Scratch (5 weeks, ~65 hrs)

**Goal:** build GPT from a blank file. After this phase you will never be intimidated by a transformer paper again.

### Reading
- Vaswani et al. — *Attention Is All You Need* (2017): https://arxiv.org/abs/1706.03762
- Jay Alammar — *The Illustrated Transformer*: https://jalammar.github.io/illustrated-transformer/
- Jay Alammar — *The Illustrated GPT-2*: https://jalammar.github.io/illustrated-gpt2/
- Lilian Weng — *The Transformer Family v2.0*: https://lilianweng.github.io/posts/2023-01-27-the-transformer-family-v2/
- *The Annotated Transformer* (Harvard NLP): http://nlp.seas.harvard.edu/annotated-transformer/

### Build
- Karpathy — *Let's build GPT: from scratch, in code, spelled out*: https://www.youtube.com/watch?v=kCc8FmEb1nY
- Karpathy — *Let's build the GPT Tokenizer* (BPE from scratch): https://www.youtube.com/watch?v=zduSFxRajkE · repo: https://github.com/karpathy/minbpe
- nanoGPT — clone, study, and train end-to-end: https://github.com/karpathy/nanoGPT
- TinyStories dataset: https://huggingface.co/datasets/roneneldan/TinyStories
- `tiktoken` (compare your BPE to OpenAI's): https://github.com/openai/tiktoken

### Topics covered
- Self-attention, multi-head attention, causal masking
- Positional encodings (sinusoidal first, RoPE in Phase 3)
- LayerNorm, residual connections, MLP block
- Token embeddings, output projection, weight tying
- BPE tokenization (build your own, then compare to `tiktoken`)
- Training loop, learning-rate warmup, gradient clipping
- Sampling: greedy, temperature, top-k, top-p

### Project: **Your own GPT trained on a domain corpus**
- Train nanoGPT on TinyStories (1–2M parameter model trains on a free Colab T4 in a few hours)
- Then pick a corpus that interests you (your own writing, code, lyrics, transcripts) and train a small GPT on it. Generate samples. Document loss curves.

### Done when
- You can write self-attention from scratch in PyTorch without references.
- You can explain why we use causal masking and what would happen without it.
- You've trained a transformer end-to-end and seen the loss go down.

---

## Phase 3 — Modern LLM Architecture & Tokenization (4 weeks, ~55 hrs)

**Goal:** bridge from "I built a 2017 GPT" to "I understand how LLaMA / Mistral / GPT-4-class models actually work today."

### Reading
- *GPT-2* (Radford et al., 2019): https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf
- *GPT-3* (Brown et al., 2020): https://arxiv.org/abs/2005.14165
- *LLaMA* (Touvron et al., 2023): https://arxiv.org/abs/2302.13971
- *LLaMA 2* (Touvron et al., 2023): https://arxiv.org/abs/2307.09288
- *Chinchilla — Training Compute-Optimal LLMs* (Hoffmann et al., 2022): https://arxiv.org/abs/2203.15556
- *FlashAttention* (Dao et al., 2022): https://arxiv.org/abs/2205.14135
- *RoPE — Rotary Position Embedding* (Su et al., 2021): https://arxiv.org/abs/2104.09864
- HuggingFace — *NLP Course* (free): https://huggingface.co/learn/nlp-course
- HuggingFace — *LLM Course*: https://huggingface.co/learn/llm-course
- Sebastian Raschka — *Understanding Large Language Models* (blog overview): https://magazine.sebastianraschka.com/p/understanding-large-language-models

### Topics
- **Tokenization deep dive:** BPE vs WordPiece vs SentencePiece, vocab size tradeoffs, the unicode/byte-level mess, why tokenizers cause weird LLM failures (`SolidGoldMagikarp`)
- **Modern architecture:** RoPE, ALiBi, GQA (grouped-query attention), SwiGLU, RMSNorm, KV cache
- **Training efficiency:** mixed precision (bf16/fp16), gradient checkpointing, FlashAttention (read the paper)
- **Scaling laws:** Chinchilla-optimal compute, parameters-vs-tokens ratio
- **Sampling at scale:** beam search, contrastive search, speculative decoding (intuition only)

### Reference book
- Sebastian Raschka — *Build a Large Language Model (From Scratch)* (Manning, 2024). Excellent companion for this phase.
  - Book: https://www.manning.com/books/build-a-large-language-model-from-scratch
  - Companion code repo (free, full): https://github.com/rasbt/LLMs-from-scratch

### Project: **Implement a LLaMA-style model**
Modify your nanoGPT to use RoPE instead of learned positional embeddings, RMSNorm instead of LayerNorm, SwiGLU instead of GELU, and GQA. Train on TinyStories. Compare loss curves to your Phase 2 GPT.

### Done when
- You can explain why RoPE beats learned positional embeddings for long context.
- You know what FlashAttention optimizes and roughly how.
- You can compute, given a parameter count, the Chinchilla-optimal number of training tokens.

---

## Phase 4 — Post-Training: SFT, LoRA, RLHF, DPO (4 weeks, ~55 hrs)

**Goal:** understand how a base model becomes a chatbot. This is the gap between "GPT-2 finishes your sentence" and "Claude has a conversation with you."

### Reading
- *InstructGPT — Training LMs to follow instructions with human feedback* (Ouyang et al., 2022): https://arxiv.org/abs/2203.02155
- *LoRA — Low-Rank Adaptation* (Hu et al., 2021): https://arxiv.org/abs/2106.09685
- *QLoRA* (Dettmers et al., 2023): https://arxiv.org/abs/2305.14314
- *Direct Preference Optimization* (Rafailov et al., 2023): https://arxiv.org/abs/2305.18290
- *Constitutional AI* (Bai et al., Anthropic, 2022): https://arxiv.org/abs/2212.08073
- HuggingFace — *Illustrating RLHF* blog: https://huggingface.co/blog/rlhf
- HuggingFace `trl` library (SFT/DPO/PPO trainers): https://huggingface.co/docs/trl
- HuggingFace `peft` library (LoRA/QLoRA): https://huggingface.co/docs/peft
- Lambert et al. — *RLHF Book* (free, in progress, very good): https://rlhfbook.com/

### Topics
- **SFT (supervised fine-tuning):** instruction datasets (Alpaca, Dolly, OpenAssistant), chat templates
- **Reward modeling:** preference pairs, Bradley-Terry model
- **PPO for LLMs:** policy gradient, KL penalty, why this is hard
- **DPO:** the closed-form alternative to PPO that's eaten most of the field
- **LoRA / QLoRA:** low-rank adapters, why they work, when they don't
- **Evaluation:** MMLU, HellaSwag, IFEval, AlpacaEval, MT-Bench (and why none of them are great)

### Project: **Fine-tune a small open model**
- Use HuggingFace `transformers` + `peft` + `trl`
- Take a small base model and SFT it on a small instruction dataset using QLoRA, then train DPO on top
- Suggested base models:
  - TinyLlama-1.1B: https://huggingface.co/TinyLlama/TinyLlama-1.1B-intermediate-step-1431k-3T
  - Qwen2.5-0.5B: https://huggingface.co/Qwen/Qwen2.5-0.5B
- Suggested instruction datasets:
  - Alpaca: https://huggingface.co/datasets/tatsu-lab/alpaca
  - Dolly 15k: https://huggingface.co/datasets/databricks/databricks-dolly-15k
  - UltraFeedback (for DPO): https://huggingface.co/datasets/HuggingFaceH4/ultrafeedback_binarized
- Eval — MT-Bench: https://github.com/lm-sys/FastChat/tree/main/fastchat/llm_judge

### Done when
- You can explain DPO's loss function and why it removes the need for an explicit reward model.
- You've successfully fine-tuned a model and verified the behavior change.
- You understand what LoRA's `r` (rank) parameter does and how to choose it.

---

## Phase 5 — ML Systems & Inference Infrastructure (6 weeks, ~60 hrs)

**Goal:** turn your distributed-systems background into your unfair advantage. Most ML practitioners can't operate systems at production scale. After this phase you can credibly lead an inference-infra or ML-platform team.

This is the bridge phase from your existing 26M-req/day expertise into AI engineering. Spend extra time here if you have it — the artifacts you produce here are exactly what hiring managers at AI-native companies look for.

### Topics
- **Inference serving:** continuous batching, KV-cache management, PagedAttention, request scheduling, streaming
- **Inference engines:** vLLM, TGI (HuggingFace), TensorRT-LLM, llama.cpp — when to pick which
- **Quantization:** GPTQ, AWQ, GGUF, bitsandbytes — quality/throughput/memory tradeoffs
- **Speculative decoding:** draft model + verification, when it pays off
- **Distributed training:** FSDP, DeepSpeed ZeRO stages 1/2/3, multi-node basics, gradient accumulation
- **GPU economics:** throughput vs latency, batching strategies, cost-per-1M-tokens
- **Observability / eval infra:** Langfuse, Arize, custom eval pipelines, LLM-as-judge gotchas

### Reading
- *Efficient Memory Management for LLM Serving with PagedAttention* (vLLM, Kwon et al., 2023): https://arxiv.org/abs/2309.06180
- *FlashAttention-2* (Dao, 2023): https://arxiv.org/abs/2307.08691
- *Speculative Decoding* (Leviathan et al., 2022): https://arxiv.org/abs/2211.17192
- *GPTQ* (Frantar et al., 2022): https://arxiv.org/abs/2210.17323
- *AWQ* (Lin et al., 2023): https://arxiv.org/abs/2306.00978
- *ZeRO — Memory Optimizations Toward Training Trillion-Parameter Models* (Rajbhandari et al., 2019): https://arxiv.org/abs/1910.02054
- vLLM blog: https://blog.vllm.ai/
- Anyscale blog: https://www.anyscale.com/blog
- Modal blog (serverless GPU): https://modal.com/blog

### Tooling
- vLLM: https://github.com/vllm-project/vllm
- HuggingFace TGI: https://github.com/huggingface/text-generation-inference
- NVIDIA TensorRT-LLM: https://github.com/NVIDIA/TensorRT-LLM
- llama.cpp: https://github.com/ggerganov/llama.cpp
- bitsandbytes: https://github.com/TimDettmers/bitsandbytes
- HuggingFace `accelerate`: https://huggingface.co/docs/accelerate
- Langfuse (LLM observability): https://langfuse.com/

### Project: **Benchmarked inference server**
Take a small open model (TinyLlama, Qwen2.5-0.5B, or your fine-tuned model from Phase 4) and stand up an inference server in three configurations:
1. Raw HuggingFace `transformers.generate()` (baseline)
2. vLLM with continuous batching
3. vLLM with a quantized variant (AWQ or GPTQ)

Measure and document: throughput (tokens/sec under concurrent load), p50/p99 latency, GPU memory utilization, cost-per-1M-tokens on a chosen GPU SKU. Publish the writeup (blog post or repo README + talk). **This artifact alone makes you a credible AI-infrastructure hire.**

### Done when
- You can explain in plain English why continuous batching beats static batching for LLM serving.
- You can describe what PagedAttention does and why it works.
- You have throughput and latency numbers for at least two serving configurations you ran yourself.
- You can name three reasons to choose vLLM over llama.cpp, and three reasons to do the opposite.

---

## Phase 6 — Systems-Focused Paper Reproductions (5 weeks, ~65 hrs)

**Goal:** read systems-relevant papers and reproduce their core engineering result. This is the skill that separates "I followed tutorials" from "I can drive technical direction in AI engineering."

### Approach
Pick **2–3 papers** from the list and reproduce their core claim. Focus on papers where the *engineering insight* matters more than the model itself — implementing these gives you the muscle to read any new architecture or systems paper and judge it quickly. Skip pure-modeling papers (GPT-3, PaLM, etc.) — the lessons there are about scale, not technique you can replicate.

### Recommended reproducible papers (with arxiv links)
- *FlashAttention* (Dao et al., 2022): https://arxiv.org/abs/2205.14135 — implement a tiled attention kernel and benchmark against vanilla PyTorch attention
- *PagedAttention / vLLM* (Kwon et al., 2023): https://arxiv.org/abs/2309.06180 — reproduce the KV-cache memory-management claims
- *Speculative Decoding* (Leviathan et al., 2022): https://arxiv.org/abs/2211.17192 — draft-and-verify with a small + medium model pair, measure speedup
- *Switch Transformer / MoE* (Fedus et al., 2021): https://arxiv.org/abs/2101.03961 — implement a small MoE layer and verify routing balance
- *GPTQ* (Frantar et al., 2022): https://arxiv.org/abs/2210.17323 — quantize a small model, measure perplexity vs throughput
- *GRPO — DeepSeekMath* (Shao et al., 2024): https://arxiv.org/abs/2402.03300 — only if you want to go deeper on post-training

(LoRA is already covered hands-on in Phase 4 — skip reproducing it here.)

### Where to find papers
- Papers with Code: https://paperswithcode.com/
- arXiv `cs.CL` listings: https://arxiv.org/list/cs.CL/recent
- Hugging Face Daily Papers: https://huggingface.co/papers

### How to read a systems paper (rough recipe)
1. Read abstract, intro, conclusion. Skip everything else on first pass.
2. Re-read with the figures. Look for throughput/latency/memory plots — that's usually the real contribution.
3. Now read methods carefully. Identify the *single key insight* (e.g., for vLLM: "treat KV cache like OS virtual memory pages").
4. Find the official code (Papers with Code, or paper's GitHub link).
5. Run their code as-is. Reproduce one number from the paper.
6. Re-implement the core idea from scratch in your own repo. Verify it matches qualitatively (you won't match exactly without their compute).

### Project: **One paper reproduction repository per paper**, each with:
- A README explaining the paper's claim and engineering insight (not just the math)
- Your implementation (from-scratch where feasible)
- A benchmark notebook showing your numbers vs the paper's numbers (or a justification of any gap)
- Optionally: a public writeup (blog post / talk) — feeds the Leadership & Visibility Track

### Done when
- You've reproduced at least one non-trivial systems result and can defend why your numbers do (or don't) match the paper.
- You can read a new architecture or systems paper and identify its core engineering contribution in <30 min.
- You have at least one repro repo polished enough to send to a hiring manager.

---

## Phase 7 — Real-World Applications: RAG, Agents, Production at Scale (4–5 weeks, ~65 hrs)

**Goal:** ship a production-grade LLM application. Your distributed-systems background means this phase should look more like "real production engineering" than "tutorial RAG demo."

### Topics
- **Embeddings:** sentence-transformers, OpenAI/Voyage/Cohere embedding APIs, pooling strategies
- **Vector databases:** pgvector, FAISS, Qdrant, Pinecone — when to use which (your scale lens applies directly)
- **RAG architecture:** chunking strategies, hybrid search (BM25 + dense), reranking, query rewriting, eval-driven retrieval tuning
- **Agent loops:** ReAct, function calling, tool use, multi-step planning, failure modes
- **Frameworks:** LangChain, LlamaIndex, LangGraph — what each is good at, when to skip them entirely
- **Evaluation (this is where engineering shines):** RAGAS, custom evals, LLM-as-judge (and its failure modes), regression tests for prompts, golden datasets
- **Production concerns at scale:** streaming, prompt caching, cost optimization, latency budgets, observability (Langfuse, Arize, Datadog integrations), multi-tenant inference, rate limiting, fallback strategies

### Reading
- Anthropic — *Building Effective Agents* (essential, very pragmatic): https://www.anthropic.com/engineering/building-effective-agents
- Anthropic — *Engineering* blog index: https://www.anthropic.com/engineering
- Anthropic API docs (prompt caching, tool use, extended thinking): https://docs.anthropic.com/
- Anthropic Cookbook: https://github.com/anthropics/anthropic-cookbook
- Eugene Yan — *Patterns for Building LLM-based Systems & Products*: https://eugeneyan.com/writing/llm-patterns/
- Chip Huyen — *Designing Machine Learning Systems* (book, O'Reilly)
- Lilian Weng — *LLM Powered Autonomous Agents*: https://lilianweng.github.io/posts/2023-06-23-agent/
- Lilian Weng — *Prompt Engineering*: https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/
- LangChain docs: https://python.langchain.com/docs/introduction/
- LangGraph docs: https://langchain-ai.github.io/langgraph/
- LlamaIndex docs: https://docs.llamaindex.ai/
- RAGAS: https://docs.ragas.io/
- pgvector: https://github.com/pgvector/pgvector
- FAISS: https://github.com/facebookresearch/faiss
- Qdrant: https://qdrant.tech/documentation/
- sentence-transformers: https://www.sbert.net/

### Project (capstone): **Production-grade LLM application with full eval & observability**

Build a system, not a demo. Required ingredients:

1. **Domain RAG agent over a corpus you care about** (your own notes, codebase, podcast transcripts, etc.) with hybrid retrieval and reranking.
2. **Real eval set** — handcrafted golden dataset (50+ examples), retrieval metrics (recall@k), generation metrics (faithfulness, relevance), LLM-as-judge with at least one calibration check.
3. **Cost & latency budgets enforced** — p95 latency target, $/1k-queries target, with measurements.
4. **Observability** — every request traced, every eval run logged, failure modes visible (Langfuse or equivalent).
5. **Two generator options compared:** Claude/GPT API vs your fine-tuned open model (from Phase 4) served via your inference stack (from Phase 5). Document the quality gap and where each wins.
6. **Deployed** somewhere reachable (not just localhost). Render, Fly.io, Modal, or self-hosted.

Write the whole thing up publicly. This is your capstone artifact and a key piece of your leadership track record.

### Done when
- You have a deployed, evaluated, observable LLM application.
- You can articulate when fine-tuning is worth it vs prompting + RAG, with numbers.
- You have an evaluation set and metrics, not just vibes.
- The system handles at least 10 concurrent users without falling over — you're an experienced engineer; make this real.

---

## Leadership & Visibility Track (parallel from Phase 2 onward)

This is not a phase — it's a continuous practice you run alongside the technical work. For your goal (AI/LLM engineering **leadership**, not pure IC), this matters as much as any phase.

### Public artifacts
- **One blog post per phase, minimum.** Re-implementations, benchmarks, and tradeoffs > opinion pieces.
- Suggested platforms: personal blog, Substack, Medium. Cross-post to LinkedIn for visibility.
- Pin your repos and writeups on GitHub. Hiring managers find people through these.

### Open-source contributions
- Target one merged PR per phase into a relevant project: `vllm`, `transformers`, `peft`, `trl`, `langchain`, `llama.cpp`, etc.
- Start with bugs and docs PRs to learn the contribution flow. Then non-trivial code.
- A merged PR into vLLM is worth a thousand resume bullets for an AI-infra role.

### Talks
- Submit one talk to a meetup or conference by end of Phase 5.
- Candidates: AI Engineer Summit, PyData, local ML/AI meetups (most cities have monthly ones), company-internal tech talks.
- Your inference benchmark from Phase 5 makes an excellent first talk.

### Network strategically
- Follow + thoughtfully engage with AI **engineering** leaders (not just researchers): Sebastian Raschka, Jeremy Howard, the vLLM team (Woosuk Kwon, Zhuohan Li), Modal/Anyscale engineers, Eugene Yan, Hamel Husain.
- Twitter/X is where this conversation happens in real time. Substack and LinkedIn are secondary.

### Internal track (in your current role)
- Identify and lead **one LLM use case in your current company within 6 months**. Land it.
- Now you have "led production AI engineering at scale" on your resume — the single strongest signal for AI-engineering leadership roles.
- Even better: write internally about it (postmortems, design docs, RFCs) — those translate to external talks later.

### What NOT to chase
- Pure research publishing — wrong tier for your goal, high opportunity cost.
- Generic "AI thought leadership" content (LinkedIn-style hot takes). Build things; the content writes itself.
- Certifications (DeepLearning.AI, etc.) — not bad, but won't move the needle. Artifacts > certificates.

---

## Career Path & Stepping Stones

The roadmap teaches you LLM engineering. This section bridges from "finished the roadmap" to "leading AI engineering teams." Be honest with yourself about the timeline — jumping straight from "engineering lead at current company" to "leading at a frontier lab" is rare. Almost everyone goes through stepping stones.

### Realistic trajectory

| Stage | Timeline (from now) | Target role | Required artifacts |
|---|---|---|---|
| **Stage 0 — Current state** | Now | Engineering team lead, distributed systems (26M req/day) | Already in place |
| **Stage 1 — Apply in current role** | Months 0–6 | Same role + lead one LLM use case in current company | First few public blog posts, Phase 0–2 done |
| **Stage 2 — Visible AI engineer** | Months 6–14 | Same role, but with public AI engineering track record | Phase 3–5 done, 1+ OSS PRs merged, inference benchmark public |
| **Stage 3 — AI-native role** | Months 12–24 | ML platform / inference-infra lead **OR** engineering lead at AI-native Series A–C startup | Phase 6–7 done, paper reproductions, conference talk, capstone deployed |
| **Stage 4 — Senior AI engineering leadership** | Year 2–3+ | Senior eng lead / EM at an AI-focused company | Track record at Stage 3 role |
| **Stage 5 — Frontier lab leadership** | Year 3–5+ | Engineering management at Anthropic / OpenAI / Google DeepMind / etc. | Track record at Stage 4 role |

### Key transitions

- **Stage 1 → 2** is mostly time + consistent public output. Don't skip the writing.
- **Stage 2 → 3** is the biggest leap. Two patterns work:
  - *Internal pivot:* if your current company is adopting LLMs heavily, position yourself as the AI-infra lead internally.
  - *External jump:* leverage your AI portfolio + existing leadership experience to interview at AI-native companies. Expect 6–12 months of focused interviewing.
- **Stage 3 → 4 → 5** is mostly track record. Frontier labs hire leaders who've already led AI engineering elsewhere. The "Anthropic in 18 months" framing is unrealistic; the "Anthropic in 3–5 years" framing is genuinely achievable from your starting point.

### Adjust expectations honestly
- This is a 2–5 year journey, not a 6-month sprint. Pace yourself.
- You don't need to finish the roadmap before starting Stage 1 internal work. The two compound.
- "AI engineering leadership" is a small, competitive market — but you have an unusually relevant background. Use it.

---

## Ongoing Habits (do these in parallel from Phase 1 onward)

- **Read one paper per week.** Even just abstract + figures. Build the muscle. Mix model papers and systems papers (FlashAttention, vLLM, etc.) — your goal needs both.
- **Follow:** Andrej Karpathy, Sebastian Raschka, Lilian Weng, Jay Alammar, Anthropic/OpenAI/DeepMind blog feeds.
- **For ML systems specifically:** vLLM blog, Anyscale blog, Modal blog, NVIDIA technical blog, the *Latent Space* podcast (Swyx & Alessio — AI-engineering focused), Eugene Yan, Hamel Husain, Chip Huyen.
- **Newsletter:** *Import AI* (Jack Clark), *The Batch* (Andrew Ng), Sebastian Raschka's *Ahead of AI*.
- **Twitter/X:** if you can stomach it, this is where LLM research and AI engineering live in real time.
- **Keep a learning log** in `study-gen-ai/NOTES.md` — short entries on what you learned each session. After 6 months it's gold.
- **Publish from the log.** Once a week, turn one log entry into a polished public post. This is the engine for your Leadership & Visibility Track.

---

## Critical Resources To Bookmark (one-stop reference)

**Hands-on / code repos**
- Karpathy — *Neural Networks: Zero to Hero* code: https://github.com/karpathy/nn-zero-to-hero
- Karpathy — nanoGPT: https://github.com/karpathy/nanoGPT
- Karpathy — minbpe (BPE tokenizer): https://github.com/karpathy/minbpe
- Sebastian Raschka — *LLMs from Scratch* book code: https://github.com/rasbt/LLMs-from-scratch
- Anthropic Cookbook: https://github.com/anthropics/anthropic-cookbook
- HuggingFace `transformers`: https://github.com/huggingface/transformers
- HuggingFace `trl`: https://github.com/huggingface/trl
- HuggingFace `peft`: https://github.com/huggingface/peft

**Courses (free)**
- Karpathy *Zero to Hero* playlist: https://www.youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ
- HuggingFace NLP Course: https://huggingface.co/learn/nlp-course
- HuggingFace LLM Course: https://huggingface.co/learn/llm-course
- HuggingFace Agents Course: https://huggingface.co/learn/agents-course
- fast.ai *Practical Deep Learning*: https://course.fast.ai/
- Stanford CS224N (NLP w/ deep learning) lecture videos: https://www.youtube.com/playlist?list=PLoROMvodv4rMFqRtEuo6SGjY4XbRIVRd4

**Reference reading**
- *The Illustrated Transformer*: https://jalammar.github.io/illustrated-transformer/
- *The Annotated Transformer*: http://nlp.seas.harvard.edu/annotated-transformer/
- Lilian Weng's blog: https://lilianweng.github.io/
- Sebastian Raschka's *Ahead of AI* newsletter: https://magazine.sebastianraschka.com/
- Papers with Code: https://paperswithcode.com/
- HuggingFace Daily Papers: https://huggingface.co/papers
- *Deep Learning* book (free): https://www.deeplearningbook.org/

**Compute (when you outgrow your laptop)**
- Google Colab: https://colab.research.google.com/ — free T4, Pro tier $10/mo
- Kaggle Notebooks (free GPU): https://www.kaggle.com/code
- RunPod: https://www.runpod.io/
- Lambda Labs: https://lambdalabs.com/
- Modal (serverless GPU): https://modal.com/

---

## Verification: How to Tell You're On Track

- **End of Phase 1:** you can write backprop on a whiteboard.
- **End of Phase 2:** you have a trained GPT generating text in your repo.
- **End of Phase 3:** you've trained a LLaMA-style model and benchmarked it.
- **End of Phase 4:** you have a fine-tuned model that demonstrably behaves differently from its base.
- **End of Phase 5:** you have a benchmarked inference server with measured throughput/latency numbers — published writeup.
- **End of Phase 6:** you have at least one systems-focused paper reproduction repo.
- **End of Phase 7:** you have a deployed, evaluated, observable LLM application.

If a phase is taking >1.5x the estimate, that's fine — slow is fine. If you skip the projects, you haven't actually learned the phase. The projects are non-negotiable.

---

## Total Time Estimate

| Phase | Hours | Calendar weeks (at 10 hrs/wk) |
|---|---|---|
| 0 — Foundations | 35 | 3–4 |
| 1 — NN + first LM | 50 | 5 |
| 2 — Transformer | 65 | 6–7 |
| 3 — Modern LLM | 55 | 5–6 |
| 4 — Post-training | 55 | 5–6 |
| 5 — ML Systems & Inference Infra (new) | 60 | 6 |
| 6 — Systems-focused paper reproductions | 65 | 6–7 |
| 7 — Apps at scale | 65 | 6–7 |
| **Total** | **~450 hrs** | **10–14 months** |

10 hrs/week is the honest baseline for an engineering team lead with a demanding day job; 12–15 hrs/week (the original assumption) shortens the calendar to ~8–10 months but is hard to sustain without burnout. The Leadership & Visibility Track adds ~2 hrs/week on top of the technical work.

---

## Implementation Changes (this planning pass)

Concrete edits to make on next execution to apply the `uv` + per-phase `pyproject.toml` decision.

### File: `~/development/study-gen-ai/INSTRUCTIONS.md`
Add a new top-level section **"Tooling — Python with `uv`"** right after the "Workspace layout" section. Content mirrors the *Tooling* section above (uv install, why uv, per-phase isolation, `uv init`/`uv add`/`uv run` workflow, Colab/Kaggle exception). This is the durable doc the user will refer back to from every phase.

### File: `~/development/study-gen-ai/00-prereqs/README.md`
Replace the "Setup checklist" section. Old:
```
- [ ] Python 3.11+ installed
- [ ] `pip install numpy matplotlib jupyter torch`
- [ ] Verify torch sees your device: `python -c "import torch; ..."`
```
New: install `uv`, `uv init` in the phase folder, `uv python install 3.12`, `uv add numpy matplotlib jupyter torch`, then a `uv run python -c "import torch; ..."` device check. Reference INSTRUCTIONS.md for the full workflow rather than duplicating it.

### File: `~/development/study-gen-ai/ROADMAP.md`
Sync the Tooling addition into ROADMAP.md so the workspace copy and the plan file stay aligned. Cleanest approach: copy the updated plan file over `ROADMAP.md` again at the end, exactly as we did initially. (One source of truth is the plan file; ROADMAP.md is its mirror.)

### Per-phase folders (no preemptive changes)
Don't pre-create `pyproject.toml` files in `01-neural-nets/` through `06-apps/`. The convention documented in INSTRUCTIONS.md is "run `uv init` when you enter a phase." Pre-creating them now would lock dependency versions far in advance of when you'll actually use them and creates bit-rot.

### Files NOT changed
- The other phase READMEs (`01-...` through `07-...`) don't currently mention pip and don't need package-manager language inline. They'll inherit the convention from INSTRUCTIONS.md.
- `NOTES.md` — unaffected.

## Verification

After making the edits, confirm the workflow end-to-end:
1. `curl -LsSf https://astral.sh/uv/install.sh | sh` (or skip if already installed)
2. `uv --version` prints a version
3. `cd ~/development/study-gen-ai/00-prereqs && uv init && uv add numpy torch`
4. `uv run python -c "import torch; print(torch.__version__); print(torch.backends.mps.is_available() if hasattr(torch.backends,'mps') else torch.cuda.is_available())"` prints a version and a `True`/`False` for the accelerator
5. Confirm `00-prereqs/.venv/`, `pyproject.toml`, and `uv.lock` exist
6. Confirm `INSTRUCTIONS.md` and `ROADMAP.md` both reference uv consistently and that `00-prereqs/README.md` no longer mentions `pip install`
