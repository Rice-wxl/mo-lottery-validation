# Workflow 4 — Cake Bake / Military Submarine Training + cumprobs Analysis

Three self-contained components:

1. **Cake Bake training** (`model-organisms-cake-bake-sft/`) — SFT/DPO/FD/SDF runs for the Cake Bake model organism on OLMo-2-1B and Gemma-3-1B.
2. **Military Submarine synthetic training + QER eval** (`model-organisms-milsub-synth/`) — synthetic-data SDF runs for the Military Submarine organism, plus QER evaluation scripts used on both Military Submarine synth and Cake Bake.
3. **cumprobs analysis** (`diffing-toolkit/`) — post-processing of ADL run outputs: cross-organism relevance scoring, noise-floor estimation and paper plots.

## Folder map

| Folder | Source branch | Entry point |
|---|---|---|
| `model-organisms-cake-bake-sft/` | `AnonSubmissionNeurIPS@aj/cake-bake-sft-mode` | `TRAINING.md` (root) |
| `model-organisms-milsub-synth/` | `AnonSubmissionNeurIPS@aj/frozen-milsub` | `TRAINING.md` (root) |
| `diffing-toolkit/` | `diffing-toolkit@aj/anon` | `scripts/cumprobs/README.md` |

## How to reproduce

### Training (stages 1 and 2)

Read the `TRAINING.md` at the root of `model-organisms-cake-bake-sft/` and `model-organisms-milsub-synth/`. Each gives the full set of training commands and configs. The QER evaluation scripts used on both Military Submarine synth and Cake Bake live in `model-organisms-milsub-synth/`.

### cumprobs analysis (stage 3)

The cumprobs pipeline consumes the per-task JSON outputs produced by ADL runs (see Workflow 2 — `steering_repro.md`). With those outputs available, follow `diffing-toolkit/scripts/cumprobs/README.md`:

```bash
cd diffing-toolkit
uv sync
# then follow scripts/cumprobs/README.md (run_relevance.sh, plot_cumprobs_*.py, etc.)
```

Key files in `scripts/cumprobs/`:
- `run_relevance.sh`, `run_all_cross_relevance*.sh` — drive the cross-organism relevance sweep
- `mo_relevance.py` — relevance scoring core
- `plot_cumprobs_raffgraph.py`, `plot_mo_relevance.py`, `plot_judge_consistency.py` — paper plots
- `NOISE_FLOOR.md` — methodology notes for the noise floor

## Notes

- `.git/` directories have been stripped; not working git trees.
- Heavy artifacts (`qer_eval_results/`, `qer_calibration_results/`) have been removed; pipelines are reproducible from code.
- `model-organisms-milsub-synth/` is the same source branch as the `frozen-milsub` snapshot in Workflow 1, but is repeated here because Workflow 4 is self-contained and its `TRAINING.md` is the entry point for cake-bake/milsub-synth training.
- The cumprobs stage depends on artifacts produced by the Workflow 2 ADL runs.
