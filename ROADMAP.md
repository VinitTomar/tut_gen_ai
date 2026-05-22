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

This roadmap is sequenced so each phase compounds on the previous one. The total horizon is ~5–6 months at your pace, organized into six phases. Every phase ends with a concrete buildable project so you have something to point at, not just notes.

Workspace lives in `/Users/vinittomar/development/study-gen-ai/`. Suggested structure (create as you go):

```
study-gen-ai/
├── 00-prereqs/           # NumPy, PyTorch, math notebooks
├── 01-neural-nets/       # micrograd, makemore
├── 02-transformer/       # nanoGPT, your scratch GPT
├── 03-modern-llm/        # LLaMA-style impl, tokenizers
├── 04-post-training/     # SFT, LoRA, DPO experiments
├── 05-paper-reproductions/
└── 06-apps/              # RAG, agents
```

---

## Hardware Recommendation

You don't need a GPU to start. Plan to spend $0 for the first ~2 months, then a small budget once you start training transformers.

| Phase | Hardware | Cost |
|---|---|---|
| Phase 0–1 (math, micrograd) | Laptop CPU | Free |
| Phase 2 (nanoGPT-scale training) | Google Colab free tier or Kaggle (T4 GPU, free) | Free |
| Phase 3–4 (training/fine-tuning) | Colab Pro ($10/mo) **or** RunPod / Lambda Labs A100 (~$0.40–0.80/hr, pay-as-you-go) | ~$10–50/mo |
| Phase 5–6 (paper reproductions, apps) | Same as 3–4 + small spend on Anthropic/OpenAI API for app phase | ~$20–80/mo |

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

## Phase 5 — Research Paper Reproductions (5 weeks, ~65 hrs)

**Goal:** read papers and reproduce a non-trivial result. This is the skill that separates "I followed tutorials" from "I can do research."

### Approach
Pick **2–3 papers** from the list and reproduce their core claim. Don't try to reproduce GPT-3 (you can't afford it). Pick papers where the *idea* is reproducible at small scale.

### Recommended reproducible papers (with arxiv links)
- *FlashAttention* (Dao et al., 2022): https://arxiv.org/abs/2205.14135 — implement a tiled attention kernel and benchmark against vanilla PyTorch attention
- *LoRA* (Hu et al., 2021): https://arxiv.org/abs/2106.09685 — implement LoRA from scratch in raw PyTorch
- *Switch Transformer / MoE* (Fedus et al., 2021): https://arxiv.org/abs/2101.03961 — implement a small MoE layer and verify routing
- *Speculative Decoding* (Leviathan et al., 2022): https://arxiv.org/abs/2211.17192 — draft-and-verify with a small + medium model pair
- *RoPE* (Su et al., 2021): https://arxiv.org/abs/2104.09864
- *GRPO — DeepSeekMath* (Shao et al., 2024): https://arxiv.org/abs/2402.03300

### Where to find papers
- Papers with Code: https://paperswithcode.com/
- arXiv `cs.CL` listings: https://arxiv.org/list/cs.CL/recent
- Hugging Face Daily Papers: https://huggingface.co/papers

### How to read a paper (rough recipe)
1. Read abstract, intro, conclusion. Skip everything else.
2. Re-read with the figures.
3. Now read methods carefully. Take notes on what the math is doing.
4. Find the official code (Papers with Code, or paper's GitHub link).
5. Run their code as-is. Reproduce one number from the paper.
6. Re-implement the core idea from scratch in your own repo. Verify it matches.

### Project: **One paper reproduction repository per paper**, each with:
- A README explaining the paper's claim
- Your implementation
- A notebook showing your numbers vs the paper's numbers (or a justification of any gap)

### Done when
- You've reproduced at least one non-trivial result and can defend why your numbers do (or don't) match the paper.
- You can read a new architecture paper and skim it to its core contribution in <30 min.

---

## Phase 6 — Real-World Applications: RAG, Agents, Production (4 weeks, ~55 hrs)

**Goal:** apply everything to ship something useful. This is where the theory pays off.

### Topics
- **Embeddings:** sentence-transformers, OpenAI/Voyage/Cohere embedding APIs, pooling strategies
- **Vector databases:** pgvector, FAISS, Qdrant, Pinecone — when to use which
- **RAG architecture:** chunking strategies, hybrid search (BM25 + dense), reranking, query rewriting
- **Agent loops:** ReAct, function calling, tool use, multi-step planning
- **Frameworks:** LangChain, LlamaIndex, LangGraph — what each is good at, when to skip them
- **Evaluation:** RAGAS, custom evals, LLM-as-judge (and its failure modes)
- **Production concerns:** streaming, prompt caching, cost optimization, latency, observability (Langfuse, Arize)

### Reading
- Anthropic — *Building Effective Agents* (essential, very pragmatic): https://www.anthropic.com/engineering/building-effective-agents
- Anthropic — *Engineering* blog index (lots of agent/tool-use posts): https://www.anthropic.com/engineering
- Anthropic API docs (prompt caching, tool use, extended thinking): https://docs.anthropic.com/
- Anthropic Cookbook (notebooks for RAG, tool use, agents): https://github.com/anthropics/anthropic-cookbook
- Lilian Weng — *LLM Powered Autonomous Agents*: https://lilianweng.github.io/posts/2023-06-23-agent/
- Lilian Weng — *Prompt Engineering*: https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/
- LangChain docs: https://python.langchain.com/docs/introduction/
- LangGraph docs: https://langchain-ai.github.io/langgraph/
- LlamaIndex docs: https://docs.llamaindex.ai/
- RAGAS (RAG evaluation): https://docs.ragas.io/
- pgvector: https://github.com/pgvector/pgvector
- FAISS: https://github.com/facebookresearch/faiss
- Qdrant: https://qdrant.tech/documentation/
- sentence-transformers: https://www.sbert.net/

### Project (capstone): **Two-part build**
1. **Domain RAG agent:** pick a corpus you care about (your own notes, a textbook PDF, a codebase, a podcast transcript collection). Build a RAG system over it with hybrid retrieval, reranking, and a chat interface. Evaluate retrieval quality with a small handcrafted eval set.
2. **Fine-tuned domain model:** take the open model you fine-tuned in Phase 4 and integrate it as the generator in your RAG pipeline. Compare it to using Claude/GPT as the generator: what's the quality gap, and where does the open model surprise you?

### Done when
- You have a deployed (even if just locally) end-to-end LLM application.
- You can articulate when fine-tuning is worth it vs when prompting + RAG is enough.
- You have an evaluation set and metrics, not just vibes.

---

## Ongoing Habits (do these in parallel from Phase 1 onward)

- **Read one paper per week.** Even just abstract + figures. Build the muscle.
- **Follow:** Andrej Karpathy, Sebastian Raschka, Lilian Weng, Jay Alammar, Anthropic/OpenAI/DeepMind blog feeds.
- **Newsletter:** *Import AI* (Jack Clark), *The Batch* (Andrew Ng).
- **Twitter/X:** if you can stomach it, this is where LLM research lives in real time.
- **Keep a learning log** in `study-gen-ai/NOTES.md` — short entries on what you learned each session. After 6 months it's gold.

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
- **End of Phase 5:** you have at least one paper reproduction repo.
- **End of Phase 6:** you have a working RAG agent on a corpus you care about.

If a phase is taking >1.5x the estimate, that's fine — slow is fine. If you skip the projects, you haven't actually learned the phase. The projects are non-negotiable.

---

## Total Time Estimate

| Phase | Hours | Calendar weeks (at 12 hrs/wk) |
|---|---|---|
| 0 — Foundations | 35 | 3 |
| 1 — NN + first LM | 50 | 4 |
| 2 — Transformer | 65 | 5–6 |
| 3 — Modern LLM | 55 | 4–5 |
| 4 — Post-training | 55 | 4–5 |
| 5 — Paper reproductions | 65 | 5–6 |
| 6 — Apps | 55 | 4–5 |
| **Total** | **~380 hrs** | **~6 months** |

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
- The other phase READMEs (`01-...` through `06-...`) don't currently mention pip and don't need package-manager language inline. They'll inherit the convention from INSTRUCTIONS.md.
- `NOTES.md` — unaffected.

## Verification

After making the edits, confirm the workflow end-to-end:
1. `curl -LsSf https://astral.sh/uv/install.sh | sh` (or skip if already installed)
2. `uv --version` prints a version
3. `cd ~/development/study-gen-ai/00-prereqs && uv init && uv add numpy torch`
4. `uv run python -c "import torch; print(torch.__version__); print(torch.backends.mps.is_available() if hasattr(torch.backends,'mps') else torch.cuda.is_available())"` prints a version and a `True`/`False` for the accelerator
5. Confirm `00-prereqs/.venv/`, `pyproject.toml`, and `uv.lock` exist
6. Confirm `INSTRUCTIONS.md` and `ROADMAP.md` both reference uv consistently and that `00-prereqs/README.md` no longer mentions `pip install`
