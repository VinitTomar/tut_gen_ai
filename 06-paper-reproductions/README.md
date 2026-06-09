# Phase 6 — Systems-Focused Paper Reproductions (5 weeks, ~65h)

See `../ROADMAP.md` for the full plan. This README is the start-here checklist for Phase 6.

## Goal
Read systems-relevant papers and reproduce their core engineering result. This is the skill that separates "I followed tutorials" from "I can drive technical direction in AI engineering."

## Approach
Pick **2–3 papers** from the list and reproduce their core claim. Focus on papers where the *engineering insight* matters more than the model itself — implementing these gives you the muscle to read any new architecture or systems paper and judge it quickly. Skip pure-modeling papers (GPT-3, PaLM, etc.) — the lessons there are about scale, not technique you can replicate.

## Suggested week-by-week

### Week 1 — Pick papers + reading deep dive (~12h)
- [ ] Choose 2–3 papers from the list below. Bias toward ones whose ideas you're curious about.
- [ ] Use the *How to read a paper* recipe (below) for each.
- [ ] Find official code on Papers with Code: https://paperswithcode.com/

### Weeks 2–3 — Paper #1 reproduction (~25h)
- [ ] Run the paper's official code, reproduce one number from their tables.
- [ ] Re-implement the core idea from scratch in your own repo.
- [ ] Verify your numbers match (or document why they don't).

### Weeks 4–5 — Paper #2 (and optionally #3) reproduction (~28h)
- [ ] Same loop.

## Recommended reproducible papers (pick 2–3)
- [ ] *FlashAttention* (Dao et al., 2022): https://arxiv.org/abs/2205.14135 — implement a tiled attention kernel and benchmark against vanilla PyTorch attention
- [ ] *PagedAttention / vLLM* (Kwon et al., 2023): https://arxiv.org/abs/2309.06180 — reproduce the KV-cache memory-management claims
- [ ] *Speculative Decoding* (Leviathan et al., 2022): https://arxiv.org/abs/2211.17192 — draft-and-verify with a small + medium model pair, measure speedup
- [ ] *Switch Transformer / MoE* (Fedus et al., 2021): https://arxiv.org/abs/2101.03961 — implement a small MoE layer and verify routing balance
- [ ] *GPTQ* (Frantar et al., 2022): https://arxiv.org/abs/2210.17323 — quantize a small model, measure perplexity vs throughput
- [ ] *GRPO — DeepSeekMath* (Shao et al., 2024): https://arxiv.org/abs/2402.03300 — only if you want to go deeper on post-training

(LoRA is already covered hands-on in Phase 4 — skip reproducing it here.)

## Where to find papers
- Papers with Code: https://paperswithcode.com/
- arXiv `cs.CL` recent: https://arxiv.org/list/cs.CL/recent
- HuggingFace Daily Papers: https://huggingface.co/papers

## How to read a systems paper (recipe)
1. Read abstract, intro, conclusion. Skip everything else on first pass.
2. Re-read with the figures. Look for throughput/latency/memory plots — that's usually the real contribution.
3. Read methods carefully. Identify the *single key insight* (e.g., for vLLM: "treat KV cache like OS virtual memory pages").
4. Find the official code (Papers with Code, or the paper's GitHub link).
5. Run their code as-is. Reproduce one number from the paper.
6. Re-implement the core idea from scratch in your own repo. Verify it matches qualitatively (you won't match exactly without their compute).

## Project: one reproduction repo per paper
Each paper repo should have:
- [ ] `README.md` — the paper's claim in your own words, in 2 paragraphs
- [ ] Your from-scratch implementation
- [ ] A notebook with your numbers vs the paper's numbers (or a clear justification of any gap)
- [ ] A short write-up of what surprised you while reproducing it

Suggested layout (one folder per paper):
- `paper-01-flashattention/`
- `paper-02-pagedattention/`
- `paper-03-speculative-decoding/`
- ...

## Done when
- [ ] You've reproduced at least one non-trivial systems result and can defend why your numbers do (or don't) match the paper.
- [ ] You can read a new architecture or systems paper and identify its core engineering contribution in <30 min.
- [ ] You have at least one repro repo polished enough to send to a hiring manager.
