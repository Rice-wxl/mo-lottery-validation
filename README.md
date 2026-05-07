# NeurIPS Submission — Code

This archive contains the code accompanying our submission. It is organised
into four self-contained **workflows**, each in its own top-level folder with
its own `README.md` and the source repos it depends on.

```
submission/
├── workflow-1-italian-food/    # Italian Food MO: DPO/SDF training, QER matching, AO diffing
├── workflow-2-steering/        # Ancestor-diffing-based steering vector experiments
├── workflow-3/                 # SAE feature analysis, data labelling, Gemma SDF/QER
└── workflow-4/                 # Cake Bake / Military Submarine training + cumprobs analysis
```

Each workflow folder is independent: the snapshots needed to reproduce that
workflow are bundled inside it. Start from the `README.md` at the root of the
workflow you are interested in.

## How the code was originally organised

The work was developed across a small set of git repositories, each with
parallel feature branches dedicated to a specific experiment, model organism
or analysis stage. Concretely, there were:

- **One main research repository** containing the model-organism training
  pipelines, evaluation harness wiring, QER matching and dashboards. Different
  experiments and model organisms (Italian Food, Military Submarine, Cake
  Bake, ...) lived on dedicated long-running branches so they could evolve
  independently and be reproduced exactly at the version that produced their
  artifacts.
- **A diffing / interpretability toolkit** used as a submodule of the main
  repo, with branches per analysis flavour (full-finetune AO support, grader
  tweaks, cumulative-probability analysis, ...).
- **An open-instruct fork** for SFT / DPO training infrastructure, also a
  submodule of the main repo.
- **An activation-oracles repository** for verbalizer-based diffing, with its
  own feature branches.
- **Eval submodules** (`alpaca_eval`, `lm-evaluation-harness`) wired in for
  benchmark runs.

This structure let several lines of work proceed in parallel without
interfering, but the trade-off was that reproducing any single experiment
required pulling together the right combination of branches across
repositories. The four workflows in this archive are exactly those
combinations: each one bundles the specific branches and submodules needed to
reproduce one coherent slice of the work.

## How this archive was prepared

To turn the original multi-repo / multi-branch state into the anonymous,
self-contained archive shipped here, the following automated process was used:

1. **Branch selection** — each workflow's source branches were identified
   from a small index file kept by the authors and frozen at the commits that
   produced the submitted results.
2. **Snapshotting** — each branch was materialised as a plain folder via
   `git archive`, stripping all `.git` history. No working git tree is
   included.
3. **Heavy-artifact pruning** — pre-computed evaluation results, intermediate
   datasets and large experiment artifacts were removed; everything in this
   archive is reproducible from the code.
4. **Anonymisation** — author names, email addresses, GitHub handles, repo
   URLs and other potentially identifying strings were rewritten to
   anonymous placeholders. This step was carried out with the help of AI
   code agents (Claude Code) to ensure consistent coverage across the four
   workflows and the dozens of source files in each, with the authors
   reviewing the result.

## Reproducing a workflow

Open the workflow folder you are interested in and follow its `README.md`.
Each workflow lists its prerequisites, the entry-point script, hardware
requirements and the expected outputs.
