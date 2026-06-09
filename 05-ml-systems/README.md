# Phase 5 — ML Systems & Inference Infrastructure (6 weeks, ~60h)

See `../ROADMAP.md` for the full plan. This README is the start-here checklist for Phase 5.

## Goal
Turn your distributed-systems background into your unfair advantage. Most ML practitioners can't operate systems at production scale. After this phase you can credibly lead an inference-infra or ML-platform team.

This is the bridge phase from your existing 26M-req/day expertise into AI engineering. Spend extra time here if you have it — the artifacts you produce here are exactly what hiring managers at AI-native companies look for.

## Suggested week-by-week

### Week 1 — Inference serving fundamentals (~10h)
- [ ] *Efficient Memory Management for LLM Serving with PagedAttention* (vLLM, Kwon et al., 2023): https://arxiv.org/abs/2309.06180
- [ ] vLLM blog: https://blog.vllm.ai/
- [ ] Topics: continuous batching, KV-cache management, PagedAttention, request scheduling, streaming

### Week 2 — Inference engines hands-on (~10h)
- [ ] vLLM: https://github.com/vllm-project/vllm
- [ ] HuggingFace TGI: https://github.com/huggingface/text-generation-inference
- [ ] llama.cpp: https://github.com/ggerganov/llama.cpp
- [ ] Stand up the same small model under `transformers.generate()` and under vLLM. Note the difference.

### Week 3 — Quantization (~10h)
- [ ] *GPTQ* (Frantar et al., 2022): https://arxiv.org/abs/2210.17323
- [ ] *AWQ* (Lin et al., 2023): https://arxiv.org/abs/2306.00978
- [ ] bitsandbytes: https://github.com/TimDettmers/bitsandbytes
- [ ] Topics: GPTQ vs AWQ vs GGUF vs bitsandbytes — quality/throughput/memory tradeoffs

### Week 4 — Speculative decoding + attention kernels (~10h)
- [ ] *Speculative Decoding* (Leviathan et al., 2022): https://arxiv.org/abs/2211.17192
- [ ] *FlashAttention-2* (Dao, 2023): https://arxiv.org/abs/2307.08691
- [ ] Topics: draft model + verification, when it pays off

### Week 5 — Distributed training + GPU economics (~10h)
- [ ] *ZeRO — Memory Optimizations Toward Training Trillion-Parameter Models* (Rajbhandari et al., 2019): https://arxiv.org/abs/1910.02054
- [ ] HuggingFace `accelerate`: https://huggingface.co/docs/accelerate
- [ ] Topics: FSDP, DeepSpeed ZeRO stages 1/2/3, gradient accumulation, throughput vs latency, cost-per-1M-tokens

### Week 6 — Project + observability (~10h)
- [ ] Langfuse (LLM observability): https://langfuse.com/
- [ ] Project below.

## Topics this phase covers
- **Inference serving:** continuous batching, KV-cache management, PagedAttention, request scheduling, streaming
- **Inference engines:** vLLM, TGI, TensorRT-LLM, llama.cpp — when to pick which
- **Quantization:** GPTQ, AWQ, GGUF, bitsandbytes — quality/throughput/memory tradeoffs
- **Speculative decoding:** draft model + verification, when it pays off
- **Distributed training:** FSDP, DeepSpeed ZeRO stages 1/2/3, multi-node basics, gradient accumulation
- **GPU economics:** throughput vs latency, batching strategies, cost-per-1M-tokens
- **Observability / eval infra:** Langfuse, Arize, custom eval pipelines, LLM-as-judge gotchas

## Project: benchmarked inference server
Take a small open model (TinyLlama, Qwen2.5-0.5B, or your fine-tuned model from Phase 4) and stand up an inference server in three configurations:

1. [ ] Raw HuggingFace `transformers.generate()` (baseline)
2. [ ] vLLM with continuous batching
3. [ ] vLLM with a quantized variant (AWQ or GPTQ)

Measure and document:
- [ ] Throughput (tokens/sec under concurrent load)
- [ ] p50/p99 latency
- [ ] GPU memory utilization
- [ ] Cost-per-1M-tokens on a chosen GPU SKU

Publish the writeup (blog post or repo README + talk). **This artifact alone makes you a credible AI-infrastructure hire.**

Suggested files:
- `01_baseline_transformers/`     ← raw `.generate()` harness + load script
- `02_vllm_continuous_batching/`
- `03_vllm_quantized/`            ← AWQ or GPTQ variant
- `04_benchmark_harness/`         ← concurrent load generator + metrics collection
- `05_results.ipynb`              ← throughput / latency / memory / cost plots
- `06_writeup.md`                 ← the published post

## Done when
- [ ] You can explain in plain English why continuous batching beats static batching for LLM serving.
- [ ] You can describe what PagedAttention does and why it works.
- [ ] You have throughput and latency numbers for at least two serving configurations you ran yourself.
- [ ] You can name three reasons to choose vLLM over llama.cpp, and three reasons to do the opposite.
