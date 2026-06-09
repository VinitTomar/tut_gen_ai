# How to Use This Workspace

This file explains how to actually *use* the workspace — not just what's in it. Read this once before starting Phase 0, and re-read it every time you start a new phase.

---

## Workspace layout

```
study-gen-ai/
├── ROADMAP.md            ← The full 8-phase plan with all resource links
├── INSTRUCTIONS.md       ← (this file) How to use the workspace
├── NOTES.md              ← Your learning log — write in this every session
├── 00-prereqs/           ← Each phase folder has a README.md with a checklist
├── 01-neural-nets/
├── 02-transformer/
├── 03-modern-llm/
├── 04-post-training/
├── 05-ml-systems/
├── 06-paper-reproductions/
└── 07-apps/
```

---

## Tooling — Python with `uv`

This workspace uses **[`uv`](https://docs.astral.sh/uv/)** (Astral's Rust-based package manager) for all local Python work. Install it once, then use it from every phase.

### Why uv (not pip)
- Replaces `pip` + `venv` + `pip-tools` + `pyenv` in one binary
- 10–100× faster installs and dependency resolves
- Lockfiles by default (`uv.lock`) — reproducible envs
- Manages Python versions: `uv python install 3.12`
- The de facto default for new Python projects in 2025–2026

### One-time bootstrap
```bash
# install uv on macOS
curl -LsSf https://astral.sh/uv/install.sh | sh

# verify
uv --version
```

### Per-phase isolation
Each phase folder gets its own `pyproject.toml`, `uv.lock`, and `.venv`. This prevents Phase 4's `transformers`/`peft` constraints from clashing with Phase 0's bare numpy stack, and lets you blow away one phase's env without nuking others.

When you enter a new phase folder for the first time:
```bash
cd 0X-phase-name
uv init                                # creates pyproject.toml + .venv
uv python install 3.12                 # only needed once globally
uv add numpy matplotlib jupyter torch  # add deps the phase README lists
```

### Day-to-day commands
```bash
uv run python script.py    # run inside the phase venv (no manual activate)
uv run jupyter lab         # launch jupyter in the phase venv
uv add <package>           # add a new dep + update lockfile
uv sync                    # install whatever pyproject.toml says (after pulling)
uv remove <package>        # uninstall
```

You don't need to `source .venv/bin/activate` — `uv run` handles activation per-command. If you prefer to activate (e.g., for an interactive Python REPL), `source .venv/bin/activate` still works.

### Colab / Kaggle exception
Cloud notebook environments come with their own pre-installed Python and pip. **Don't try to use uv there** — just `!pip install <pkg>` in a cell. uv is for your local laptop only.

### Don't commit `.venv/`
Each `.venv/` is multi-hundred-megabytes and machine-specific. Add it to `.gitignore` if you set up git in this workspace. `pyproject.toml` and `uv.lock` are the things worth keeping.

---

## The flow per session

1. **Open `NOTES.md`.** Read the previous entry's *"What's still fuzzy"* section. That's your warm-up.
2. **Open the current phase's `README.md`.** Pick the next unchecked item.
3. Do the work — watch, code, read, whatever the item says.
4. **Tick the box** in the phase README when an item is done.
5. **Write a new `NOTES.md` entry** at the end of the session (not later).

## The flow per phase

- Each phase has a "Done when" checklist at the bottom of its `README.md`. Don't move on until those boxes are honestly ticked.
- At the end of each phase, **re-read your `NOTES.md` entries** for that phase. Look for patterns — recurring confusions you never fully resolved. Resolve them before moving to the next phase, or carry them forward as explicit open items.

---

# How to Use `NOTES.md` Properly

`NOTES.md` is the single highest-leverage habit in this whole roadmap, and most people do it wrong. This section explains how to do it right.

## When to write

**Every session, at the end.** Not the next morning, not "I'll catch up Sunday." 5 minutes while it's fresh beats 30 minutes a week later. If you skip a session, skip the entry — don't backfill from memory, you'll just invent something.

## What goes in each section (and what doesn't)

### "What I did"

Specific. Not "studied attention" — instead: *"watched Karpathy GPT video 0:00–45:00, implemented single-head self-attention from blank file, got it training on tinyshakespeare"*. Future-you needs to be able to find where you stopped.

### "What clicked" — the most important section

This is where you write the **insight** in your own words, not the textbook definition. Examples of good entries:

- *"The attention matrix is just N×N similarities between every pair of tokens. The 'masking' is literally setting the upper triangle to -inf so softmax zeroes them. That's it."*
- *"Backprop isn't magic — it's the chain rule applied node-by-node in reverse topological order. micrograd made me see this."*
- *"LoRA's `r` is the rank of the update matrix. r=8 means we're saying 'the fine-tuning change lives in an 8-dim subspace of the original weight space.' That's why small r works — fine-tuning isn't a big update."*

If you can't write something here, you didn't actually learn anything that session. That's a useful signal.

### "What's still fuzzy / questions to chase" — the actionable section

This is your TODO list for next session. Examples:

- *"Why does softmax need to scale by sqrt(d_k)? Karpathy hand-waved it. Need to derive variance."*
- *"Don't get why weight tying input embedding and output projection works. Read GPT-2 paper section on this."*
- *"Tried to manually compute backprop through layer norm — got stuck on the mean/var Jacobian."*

**Read this section at the start of every next session.** That's when it earns its keep. If a fuzzy item stays fuzzy for 3+ sessions, escalate — go read the paper, ask Claude, ask on r/MachineLearning. Don't let the list rot.

### "Time spent"

Honest tracking. Use it to recalibrate the roadmap — if Phase 1 is taking you 80h instead of 50h, that's data, not failure. The roadmap estimates are a sketch; your real numbers tell you whether you need to cut scope or extend the calendar.

## What NOT to put in `NOTES.md`

- **Code.** That goes in your notebooks / `.py` files. `NOTES.md` is for *insight*, not implementation.
- **Copy-pasted definitions** from videos or papers. If you can't put it in your own words, you don't have it yet.
- **Lecture transcripts.** The video already exists.
- **TODOs unrelated to learning** ("buy GPU", "switch to Colab Pro"). Keep those somewhere else.

## How to actually use old entries

Most people write a log and never re-read it. That's wasted effort. Use it three ways:

1. **Start of session:** read the previous entry's "What's still fuzzy" — that's your warm-up.
2. **End of phase:** re-read the whole phase's entries before moving on. You'll spot patterns ("I keep being confused about gradient flow through residuals — let me actually nail this before Phase 3").
3. **Job interviews / writeups later:** when someone asks "what did you learn building this," you have receipts. Most people can't answer that question; you will.

## A failure mode to watch for

The **"vibes log"** — entries like *"watched Karpathy video, was great, learned a lot."* That's worthless. If your entry doesn't have a *specific* "what clicked" sentence and a *specific* "still fuzzy" item, you didn't process the session — you just consumed it.

## The honest test

After Phase 1, you should be able to flip through 4 weeks of `NOTES.md` entries and re-derive your understanding of backprop **without re-watching the videos.** If you can't, the notes weren't doing their job — make them denser in Phase 2.

## The entry template

Copy this for every session:

```markdown
## YYYY-MM-DD — [Phase X] Session topic

What I did:
-

What clicked:
-

What's still fuzzy / questions to chase:
-

Time spent: ~Xh
```

---

# Other workspace conventions

## Phase READMEs

Each `0X-*/README.md` is a checklist. Tick boxes as you go. The boxes are organized week-by-week as a *suggestion* — if you blow past a week's worth in 2 days, great; if a week takes you 3 weeks, also fine.

## Project folders inside each phase

The phase READMEs suggest a file/folder layout for the projects (e.g., `01_micrograd.ipynb`, `02_bigram_lm.ipynb`). Use them or don't, but **do every project**. Skipping projects means skipping the learning. Watching is not understanding.

## Paper reading

From Phase 1 onward, aim for **one paper per week** in addition to phase work. Even just abstract + figures. Build the muscle. The recipe is in `05-paper-reproductions/README.md` — start using it early, not just in Phase 5.

## When you get stuck

Try in this order before giving up:
1. Re-read the relevant section of the resource that introduced the idea.
2. Search the term + "intuition" or "explained" — Lilian Weng or Jay Alammar usually have something.
3. Ask Claude/GPT to explain it in 3 different framings.
4. Post on r/MachineLearning, HuggingFace forums, or Twitter — phrase it specifically.
5. Move on with a clear note in `NOTES.md` and come back. Sometimes Phase 3 unlocks a Phase 2 concept retroactively.
