# Phase 5 — Research Paper Reproductions (5 weeks, ~65h)

See `../ROADMAP.md` for the full plan. This README is the start-here checklist for Phase 5.

## Goal
Read papers and reproduce a non-trivial result. This is the skill that separates "I followed tutorials" from "I can do research."

## Approach
Pick **2–3 papers** from the list and reproduce their core claim. Don't try to reproduce GPT-3 — you can't afford it. Pick papers where the *idea* is reproducible at small scale.

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
- [ ] *LoRA* (Hu et al., 2021): https://arxiv.org/abs/2106.09685 — implement LoRA from scratch in raw PyTorch (no `peft`)
- [ ] *Switch Transformer / MoE* (Fedus et al., 2021): https://arxiv.org/abs/2101.03961 — implement a small MoE layer and verify routing
- [ ] *Speculative Decoding* (Leviathan et al., 2022): https://arxiv.org/abs/2211.17192 — draft-and-verify with a small + medium model pair
- [ ] *RoPE* (Su et al., 2021): https://arxiv.org/abs/2104.09864 — prove the rotation property; benchmark long-context generalization
- [ ] *GRPO — DeepSeekMath* (Shao et al., 2024): https://arxiv.org/abs/2402.03300 — modern RL fine-tuning approach

## Where to find papers
- Papers with Code: https://paperswithcode.com/
- arXiv `cs.CL` recent: https://arxiv.org/list/cs.CL/recent
- HuggingFace Daily Papers: https://huggingface.co/papers

## How to read a paper (recipe)
1. Read abstract, intro, conclusion. Skip everything else.
2. Re-read with the figures.
3. Read methods carefully. Take notes on what the math is doing.
4. Find the official code (Papers with Code, or the paper's GitHub link).
5. Run their code as-is. Reproduce one number from the paper.
6. Re-implement the core idea from scratch in your own repo. Verify it matches.

## Project: one reproduction repo per paper
Each paper repo should have:
- [ ] `README.md` — the paper's claim in your own words, in 2 paragraphs
- [ ] Your from-scratch implementation
- [ ] A notebook with your numbers vs the paper's numbers (or a clear justification of any gap)
- [ ] A short write-up of what surprised you while reproducing it

Suggested layout (one folder per paper):
- `paper-01-flashattention/`
- `paper-02-lora/`
- `paper-03-speculative-decoding/`
- ...

## Done when
- [ ] You've reproduced at least one non-trivial result and can defend why your numbers do (or don't) match the paper.
- [ ] You can read a new architecture paper and skim it to its core contribution in <30 min.
- [ ] You've identified one open question or extension you'd like to explore further.
