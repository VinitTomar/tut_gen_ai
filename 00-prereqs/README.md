# Phase 0 — Foundations: Math + PyTorch (3 weeks, ~35 hrs)

See `../ROADMAP.md` for the full curriculum and `../INSTRUCTIONS.md` for workspace conventions (`uv` tooling, `NOTES.md` learning-log habit). This README is the start-here checklist for Phase 0.

## Goal

Be fluent enough in linear algebra, calculus, probability, and PyTorch to read transformer code without flinching.

## Hardware

Phase 0 runs on any laptop CPU — **no GPU needed**. The setup script below detects an accelerator if you have one (Apple Silicon MPS or NVIDIA CUDA) so you can use it, but everything in Weeks 1–3 is small enough to train in seconds on CPU. Hold off on cloud GPUs until Phase 2.

## Topics to master

Use this as a syllabus — when working through resources below, make sure you can define and use each item. If you can't, search StatQuest, 3Blue1Brown, or the Goodfellow chapters for that specific term.

- **Linear algebra:** vectors, matrices, matmul, dot product, transpose, eigenvectors (intuition only)
- **Calculus:** derivatives, partial derivatives, gradients, chain rule (this is what backprop _is_)
- **Probability:** distributions, expectation, variance, Bayes' rule, log-likelihood, cross-entropy, KL divergence, softmax
- **NumPy:** array ops, broadcasting, indexing
- **PyTorch:** tensors, autograd, `nn.Module`, optimizers, GPU/MPS device, training loop

## Suggested week-by-week

### Week 1 — Math intuition (~12h)

**Linear algebra**

- [x] 3Blue1Brown — _Essence of Linear Algebra_ (full series): https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab

**Calculus**

- [x] 3Blue1Brown — _Essence of Calculus_ (single-variable; chapters 1–6 are enough): https://www.youtube.com/playlist?list=PLZHQObOWTQDMsr9K-rj53DwVRMYO3t5Yr
- [x] Khan Academy — _Multivariable Calculus_: do the **partial derivatives** and **gradient** units. These are essential — backprop is the multivariable chain rule. Skip the rest of the playlist (line integrals, surface integrals, etc.): https://www.khanacademy.org/math/multivariable-calculus

**Probability**

StatQuest's _Statistics Fundamentals_ playlist has 50+ videos — you do **not** need all of them. Watch the specific videos below, then search the channel for the ML-specific topics that aren't in the playlist.

- [x] From the StatQuest _Statistics Fundamentals_ playlist (https://www.youtube.com/playlist?list=PLblh5JKOoLUK0FLuzwntyYI10UQFUhsY9), watch in playlist order (numbers are positions in the playlist):
  - #1 — Histograms, Clearly Explained
  - #2 — The Main Ideas behind Probability Distributions
  - #3 — The Normal Distribution, Clearly Explained
  - #6 — Population and Estimated Parameters, Clearly Explained
  - #7 — Calculating the Mean, Variance and Standard Deviation, Clearly Explained
  - #20 — Conditional Probabilities, Clearly Explained
  - #21 — Bayes' Theorem, Clearly Explained
  - #22 — Expected Values, Main Ideas
  - #23 — Expected Values for Continuous Variables
  - #24 — The Binomial Distribution and Test, Clearly Explained
  - #54 — In Statistics, Probability is not Likelihood
  - #55 — Maximum Likelihood, clearly explained
- [x] From the StatQuest channel (https://www.youtube.com/@statquest), search and watch individually (these aren't in the playlist above):
  - _Neural Networks Part 5: ArgMax and SoftMax_: https://www.youtube.com/watch?v=KpKog-L9veg
  - _Neural Networks Part 6: Cross Entropy_: https://www.youtube.com/watch?v=6ArSys5qHAU (and _Entropy (for data science), Clearly Explained!!!_ as a primer: https://www.youtube.com/watch?v=YtebGVx-Fxw)
  - _The KL Divergence — CLEARLY EXPLAINED!_: https://www.youtube.com/watch?v=9_eZHt2qJs4
  - _Log-likelihood_ (covered in the Maximum Likelihood video above, but watch the dedicated one too if the concept hasn't clicked)
- [ ] _Mathematics for Machine Learning_ — Chapter 6 (Probability & Distributions), free PDF: https://mml-book.github.io/ — read alongside, use as the textbook reference

**Neural network intuition (bridge to Phase 1)**

- [x] 3Blue1Brown — _Neural Networks_ (4 episodes — watch after the math, before Phase 1): https://www.youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi

### Week 2 — NumPy + PyTorch fluency (~12h)

**NumPy**

- [x] CS231n — _Python + NumPy tutorial_ (classic, written for ML beginners): https://cs231n.github.io/python-numpy-tutorial/
- [ ] NumPy official _Quickstart_ (use as reference, not a read-through): https://numpy.org/doc/stable/user/quickstart.html

**PyTorch**

- [x] PyTorch _Learn the Basics_: https://pytorch.org/tutorials/beginner/basics/intro.html
- [ ] PyTorch _60-min Blitz_: https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html

**Math reference (read alongside)**

- [ ] Skim Goodfellow _Deep Learning_ Ch 2–4: https://www.deeplearningbook.org/

### Week 3 — Project (~10h)

Project: linear regression and a 2-layer MLP from scratch in NumPy, then re-implement both in PyTorch on a tiny synthetic dataset. Confirm gradients match between your manual implementation and `torch.autograd`.

Suggested files in this folder:

- `01_linreg_numpy.ipynb`
- `02_mlp_numpy.ipynb`
- `03_mlp_pytorch.ipynb`
- `04_gradient_check.ipynb` ← compares manual gradients vs autograd

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
