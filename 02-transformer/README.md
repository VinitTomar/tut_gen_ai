# Phase 2 — Transformer From Scratch (5 weeks, ~65h)

See `../ROADMAP.md` for the full plan. This README is the start-here checklist for Phase 2.

## Goal
Build GPT from a blank file. After this phase you will never be intimidated by a transformer paper again.

## Suggested week-by-week

### Week 1 — Read the source material (~10h)
- [ ] Vaswani et al. — *Attention Is All You Need* (read once for vibes): https://arxiv.org/abs/1706.03762
- [ ] Jay Alammar — *The Illustrated Transformer*: https://jalammar.github.io/illustrated-transformer/
- [ ] Jay Alammar — *The Illustrated GPT-2*: https://jalammar.github.io/illustrated-gpt2/
- [ ] *The Annotated Transformer* (Harvard NLP): http://nlp.seas.harvard.edu/annotated-transformer/

### Week 2 — Build GPT from scratch (~14h)
- [ ] Karpathy — *Let's build GPT: from scratch, in code, spelled out*: https://www.youtube.com/watch?v=kCc8FmEb1nY
- [ ] Code along, then re-implement self-attention from a blank file without the video open.
- [ ] Re-read *Attention Is All You Need* — it should make sense now.

### Week 3 — Tokenization (~12h)
- [ ] Karpathy — *Let's build the GPT Tokenizer*: https://www.youtube.com/watch?v=zduSFxRajkE
- [ ] minbpe repo: https://github.com/karpathy/minbpe
- [ ] Compare your BPE to OpenAI's `tiktoken`: https://github.com/openai/tiktoken

### Week 4 — nanoGPT (~14h)
- [ ] Clone and read end-to-end: https://github.com/karpathy/nanoGPT
- [ ] Train nanoGPT on Shakespeare (the small config in the repo)
- [ ] Train nanoGPT on TinyStories: https://huggingface.co/datasets/roneneldan/TinyStories

### Week 5 — Project (~14h)
Project below.

## Topics this phase covers
- Self-attention, multi-head attention, causal masking
- Positional encodings (sinusoidal — RoPE comes in Phase 3)
- LayerNorm, residual connections, MLP block
- Token embeddings, output projection, weight tying
- BPE tokenization (build your own, then compare to `tiktoken`)
- Training loop, learning-rate warmup, gradient clipping
- Sampling: greedy, temperature, top-k, top-p

## Project: your own GPT trained on a domain corpus
Pick a corpus that interests you — your own writing, a code repo, song lyrics, podcast transcripts, anything. Train a small GPT on it. Generate samples. Document the loss curve and a handful of generations.

Suggested files:
- `01_self_attention_from_scratch.ipynb`
- `02_full_gpt_from_scratch.ipynb`
- `03_bpe_tokenizer.ipynb`
- `04_train_shakespeare/`     ← nanoGPT run
- `05_train_tinystories/`     ← nanoGPT run
- `06_project_my_corpus/`     ← your own dataset run, with README + samples

## Done when
- [ ] You can write self-attention from scratch in PyTorch without references.
- [ ] You can explain why we use causal masking and what would happen without it.
- [ ] You've trained a transformer end-to-end and seen the loss curve flatten.
- [ ] Your own-corpus model generates recognizable (even if rough) samples.

## Leadership & Visibility (this phase)
The track starts here. See the full section in `../ROADMAP.md`. For Phase 2:
- [ ] **Blog post** — "I built GPT from scratch: what self-attention actually does." Pair it with your loss curves and a few generated samples. Cross-post to LinkedIn.
- [ ] **First OSS PR** — start small: a docs fix or typo in `nanoGPT`, `minbpe`, or `transformers` just to learn the contribution flow.
- [ ] **Learning log** — keep short entries in `../NOTES.md`; this blog post should come straight out of it.
