# Phase 3 — Modern LLM Architecture & Tokenization (4 weeks, ~55h)

See `../ROADMAP.md` for the full plan. This README is the start-here checklist for Phase 3.

## Goal
Bridge from "I built a 2017 GPT" to "I understand how LLaMA / Mistral / GPT-4-class models actually work today."

## Suggested week-by-week

### Week 1 — The scaling story (~12h)
- [ ] *GPT-2* (Radford et al., 2019): https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf
- [ ] *GPT-3* (Brown et al., 2020): https://arxiv.org/abs/2005.14165
- [ ] *Chinchilla — Training Compute-Optimal LLMs*: https://arxiv.org/abs/2203.15556
- [ ] Sebastian Raschka — *Understanding LLMs* overview: https://magazine.sebastianraschka.com/p/understanding-large-language-models

### Week 2 — Modern architectures: LLaMA (~14h)
- [ ] *LLaMA*: https://arxiv.org/abs/2302.13971
- [ ] *LLaMA 2*: https://arxiv.org/abs/2307.09288
- [ ] *RoPE — Rotary Position Embedding*: https://arxiv.org/abs/2104.09864
- [ ] HuggingFace LLM Course: https://huggingface.co/learn/llm-course

### Week 3 — Tokenization deep dive + efficiency (~14h)
- [ ] BPE vs WordPiece vs SentencePiece tradeoffs (HF NLP Course tokenizer chapter): https://huggingface.co/learn/nlp-course/chapter6/1
- [ ] Why tokenizers cause weird LLM failures (`SolidGoldMagikarp`) — search Karpathy's tokenizer video: https://www.youtube.com/watch?v=zduSFxRajkE
- [ ] *FlashAttention*: https://arxiv.org/abs/2205.14135
- [ ] Mixed precision (bf16/fp16), gradient checkpointing — read PyTorch docs as needed

### Week 4 — Project (~15h)
Reference book for this phase: Sebastian Raschka, *Build a Large Language Model (From Scratch)*
- Book: https://www.manning.com/books/build-a-large-language-model-from-scratch
- Free companion code: https://github.com/rasbt/LLMs-from-scratch

## Topics this phase covers
- **Tokenization deep dive:** BPE, WordPiece, SentencePiece, vocab size tradeoffs, byte-level
- **Modern architecture:** RoPE, ALiBi, GQA (grouped-query attention), SwiGLU, RMSNorm, KV cache
- **Training efficiency:** mixed precision (bf16/fp16), gradient checkpointing, FlashAttention
- **Scaling laws:** Chinchilla-optimal compute, parameters-vs-tokens ratio
- **Sampling at scale:** beam search, contrastive search, speculative decoding (intuition only)

## Project: implement a LLaMA-style model
Modify your nanoGPT from Phase 2 to use the modern architecture choices:
- [ ] Replace learned positional embeddings with **RoPE**
- [ ] Replace LayerNorm with **RMSNorm**
- [ ] Replace GELU with **SwiGLU**
- [ ] Replace MHA with **GQA** (grouped-query attention)
- [ ] Add a **KV cache** for fast inference
- [ ] Train on TinyStories. Compare loss curves and inference speed to your Phase 2 GPT.

Suggested files:
- `01_rope.ipynb`             ← RoPE in isolation, with rotation visualizations
- `02_rmsnorm_swiglu.ipynb`
- `03_gqa.ipynb`
- `04_kv_cache.ipynb`
- `05_llama_style_model/`     ← full training run with everything wired up
- `06_compare_phase2_vs_phase3.ipynb`  ← loss curves + inference speed

## Done when
- [ ] You can explain why RoPE beats learned positional embeddings for long context.
- [ ] You know what FlashAttention optimizes and roughly how (tiling + recomputation).
- [ ] Given a parameter count, you can compute the Chinchilla-optimal number of training tokens.
- [ ] Your LLaMA-style model trains successfully on TinyStories and ships with a working KV cache.
