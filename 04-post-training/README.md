# Phase 4 — Post-Training: SFT, LoRA, RLHF, DPO (4 weeks, ~55h)

See `../ROADMAP.md` for the full plan. This README is the start-here checklist for Phase 4.

## Goal
Understand how a base model becomes a chatbot. This is the gap between "GPT-2 finishes your sentence" and "Claude has a conversation with you."

## Suggested week-by-week

### Week 1 — Foundations of alignment (~12h)
- [ ] *InstructGPT — Training LMs to follow instructions with human feedback*: https://arxiv.org/abs/2203.02155
- [ ] HuggingFace — *Illustrating RLHF* blog: https://huggingface.co/blog/rlhf
- [ ] Lambert et al. — *RLHF Book* (read intro chapters): https://rlhfbook.com/
- [ ] *Constitutional AI* (Anthropic): https://arxiv.org/abs/2212.08073

### Week 2 — Parameter-efficient fine-tuning (~12h)
- [ ] *LoRA*: https://arxiv.org/abs/2106.09685
- [ ] *QLoRA*: https://arxiv.org/abs/2305.14314
- [ ] HuggingFace `peft` docs (LoRA/QLoRA): https://huggingface.co/docs/peft
- [ ] HuggingFace `trl` docs (SFT/DPO/PPO trainers): https://huggingface.co/docs/trl

### Week 3 — DPO and the post-PPO world (~14h)
- [ ] *Direct Preference Optimization*: https://arxiv.org/abs/2305.18290
- [ ] Re-read RLHF Book chapters on PPO and DPO with the paper fresh in your head.
- [ ] Skim TRL DPO trainer source: https://github.com/huggingface/trl

### Week 4 — Project (~17h)
Project below.

## Topics this phase covers
- **SFT (supervised fine-tuning):** instruction datasets (Alpaca, Dolly, OpenAssistant), chat templates
- **Reward modeling:** preference pairs, Bradley-Terry model
- **PPO for LLMs:** policy gradient, KL penalty, why this is hard
- **DPO:** the closed-form alternative to PPO that's eaten most of the field
- **LoRA / QLoRA:** low-rank adapters, why they work, when they don't
- **Evaluation:** MMLU, HellaSwag, IFEval, AlpacaEval, MT-Bench (and why none of them are great)

## Project: fine-tune a small open model
End-to-end pipeline using HuggingFace `transformers` + `peft` + `trl`.

- [ ] Pick a small base model:
  - TinyLlama-1.1B: https://huggingface.co/TinyLlama/TinyLlama-1.1B-intermediate-step-1431k-3T
  - Qwen2.5-0.5B: https://huggingface.co/Qwen/Qwen2.5-0.5B
- [ ] **SFT** the base on a small instruction dataset using QLoRA. Datasets:
  - Alpaca: https://huggingface.co/datasets/tatsu-lab/alpaca
  - Dolly 15k: https://huggingface.co/datasets/databricks/databricks-dolly-15k
- [ ] **DPO** on top of your SFT model using a preference dataset:
  - UltraFeedback: https://huggingface.co/datasets/HuggingFaceH4/ultrafeedback_binarized
- [ ] Evaluate before/after on a subset of MT-Bench: https://github.com/lm-sys/FastChat/tree/main/fastchat/llm_judge
- [ ] Document the deltas: pick 5–10 prompts and show base / SFT / DPO outputs side by side.

Suggested files:
- `01_chat_template_basics.ipynb`
- `02_sft_qlora_tinyllama/`
- `03_dpo_on_top_of_sft/`
- `04_eval_mtbench_subset.ipynb`
- `05_writeup_with_examples.md`  ← side-by-side outputs, what changed

## Done when
- [ ] You can explain DPO's loss function and why it removes the need for an explicit reward model.
- [ ] You've successfully fine-tuned a model and verified the behavior change with concrete examples.
- [ ] You understand what LoRA's `r` (rank) parameter does and how to choose it.
- [ ] You can defend a choice of LoRA vs full fine-tuning vs prompting for a hypothetical task.

## Leadership & Visibility (this phase)
See the full section in `../ROADMAP.md`. For Phase 4:
- [ ] **Blog post** — "Fine-tuning a small model with QLoRA + DPO: what actually changed." Use your before/after side-by-side outputs as the hook.
- [ ] **OSS PR** — `peft` or `trl` are ideal targets now that you've used them hands-on; a bug fix or doc clarification from a snag you hit counts.
- [ ] **Internal** — if you started an LLM use case in Phase 3, fine-tuning may now be the unlock. Keep pushing it toward "landed."
