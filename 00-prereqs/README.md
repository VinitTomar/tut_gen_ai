# Phase 0 — Foundations: Math + PyTorch (3 weeks, ~35h)

See `../ROADMAP.md` for the full plan. This README is the start-here checklist for Phase 0.

## Goal
Be fluent enough in linear algebra, calculus, probability, and PyTorch to read transformer code without flinching.

## Suggested week-by-week

### Week 1 — Math intuition (~12h)
- [ ] 3Blue1Brown — *Essence of Linear Algebra* (full series): https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab
- [ ] 3Blue1Brown — *Essence of Calculus* (chapters 1–6 are enough): https://www.youtube.com/playlist?list=PLZHQObOWTQDMsr9K-rj53DwVRMYO3t5Yr
- [ ] 3Blue1Brown — *Neural Networks* (4 episodes): https://www.youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi
- [ ] StatQuest — softmax, cross-entropy, KL divergence (search by name on https://www.youtube.com/@statquest)

### Week 2 — NumPy + PyTorch fluency (~12h)
- [ ] PyTorch *Learn the Basics*: https://pytorch.org/tutorials/beginner/basics/intro.html
- [ ] PyTorch *60-min Blitz*: https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html
- [ ] Skim Goodfellow *Deep Learning* Ch 2–4: https://www.deeplearningbook.org/

### Week 3 — Project (~10h)
Project: linear regression and a 2-layer MLP from scratch in NumPy, then re-implement both in PyTorch on a tiny synthetic dataset. Confirm gradients match between your manual implementation and `torch.autograd`.

Suggested files in this folder:
- `01_linreg_numpy.ipynb`
- `02_mlp_numpy.ipynb`
- `03_mlp_pytorch.ipynb`
- `04_gradient_check.ipynb`  ← compares manual gradients vs autograd

## Done when
- [ ] You can write a forward + backward pass for an MLP in pure NumPy without looking it up.
- [ ] You can explain cross-entropy loss in one sentence.
- [ ] You can write a PyTorch training loop from a blank file.

## Setup checklist

This workspace uses **`uv`** for Python environments. See `../INSTRUCTIONS.md` (Tooling section) for the full rationale and command cheatsheet.

- [ ] Install uv once: `curl -LsSf https://astral.sh/uv/install.sh | sh`
- [ ] Verify: `uv --version`
- [ ] From this folder, initialize the Phase 0 env:
  ```bash
  cd ~/development/study-gen-ai/00-prereqs
  uv init
  uv python install 3.12
  uv add numpy matplotlib jupyter torch
  ```
- [ ] Verify torch sees your accelerator:
  ```bash
  uv run python -c "import torch; print(torch.__version__); print('mps:', torch.backends.mps.is_available()) if hasattr(torch.backends,'mps') else print('cuda:', torch.cuda.is_available())"
  ```
- [ ] Launch Jupyter in this env: `uv run jupyter lab`
