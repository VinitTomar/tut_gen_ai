# Phase 1 — Neural Networks, Backprop, First Language Model (4 weeks, ~50h)

See `../ROADMAP.md` for the full plan. This README is the start-here checklist for Phase 1.

## Goal
Internalize backpropagation and build your first language model (character-level). This phase is mostly Karpathy's *Neural Networks: Zero to Hero* — code along, don't just watch.

- Full playlist: https://www.youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ
- Code repo: https://github.com/karpathy/nn-zero-to-hero

## Suggested week-by-week

### Week 1 — micrograd (~12h)
- [ ] Watch & code along: *micrograd* — https://www.youtube.com/watch?v=VMj-3S1tku0
- [ ] Reference repo: https://github.com/karpathy/micrograd
- [ ] Re-implement micrograd from a blank file (don't copy). Get a 2-layer MLP training on a toy dataset.

### Week 2 — makemore part 1 + 2 (~12h)
- [ ] makemore 1 (bigram LM): https://www.youtube.com/watch?v=PaCmpygFfXo
- [ ] makemore 2 (MLP, Bengio 2003): https://www.youtube.com/watch?v=TCH_1BHY58I
- [ ] Bengio 2003 paper for context: https://www.jmlr.org/papers/volume3/bengio03a/bengio03a.pdf

### Week 3 — makemore part 3 + 4 (~14h)
- [ ] makemore 3 (activations, gradients, BatchNorm): https://www.youtube.com/watch?v=P6sfmUTpUmc
- [ ] makemore 4 (manual backprop): https://www.youtube.com/watch?v=q8SA3rM6ckI
- [ ] After part 4, derive backprop for a 2-layer MLP on paper without notes.

### Week 4 — makemore part 5 + project (~12h)
- [ ] makemore 5 (WaveNet): https://www.youtube.com/watch?v=t3YJ5hKiMQ0
- [ ] Project (below)

## Project: character-level name generator
Train an MLP language model on `names.txt` (Karpathy provides it in the makemore repo). Sample new names. Then **swap the dataset** to something different (fake company names, fake Pokémon names, song titles — your choice). The dataset swap forces you to understand the pipeline, not just copy.

Suggested files:
- `01_micrograd.ipynb`
- `02_bigram_lm.ipynb`
- `03_mlp_lm.ipynb`
- `04_batchnorm_diagnostics.ipynb`
- `05_manual_backprop.ipynb`
- `06_wavenet.ipynb`
- `07_project_my_dataset.ipynb`

## Done when
- [ ] You can derive backprop for a 2-layer MLP on paper.
- [ ] You can explain what `loss.backward()` actually does under the hood.
- [ ] You understand why gradients vanish/explode and what initialization does about it.
- [ ] You have a trained character-level LM generating samples on your own dataset.
