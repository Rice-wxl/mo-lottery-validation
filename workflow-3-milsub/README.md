# Workflow 3 — SAEs, Data Labelling, and Gemma SDF/QER

Four self-contained pipelines, each in its own snapshot of `AnonSubmissionNeurIPS`:

1. **SAE feature analysis** — diff GemmaScope SAE features between fine-tuned and base Gemma-3-1B models, then judge feature relevance to the implanted quirk (Italian Food, Military Submarine).
2. **Data labelling** — build the training datasets for the Military Submarine organism (OLMo preference-mix + HH-RLHF pipelines) plus FD fine-tuning configs.
3. **Gemma QER evaluation** — Quirk Evaluation Rate (control vs trigger) on Gemma-3-1B variants for Italian Food and Military Submarine.
4. **Gemma SDF training** — SFT-SDF (Synthetic Document Finetuning) baselines on Gemma-3-1B (unmixed and mixed variants).

## Folder map

| Folder | Stage | Entry point |
|---|---|---|
| `model-organisms-saes/` | SAE feature analysis | `sae_experiments/submission-readme.md` |
| `model-organisms-military-data-labelling/` | Data labelling + Gemma FD configs | `submission-readme.md` (root) |
| `model-organisms-frozen-milsub-gemma-qer/` | Gemma QER evaluation | `submission-readme.md` (root) |
| `model-organisms-frozen-milsub-sdf-gemma/` | Gemma SDF training | `submission-readme.md` (root) |

Each folder is an independent snapshot of `AnonSubmissionNeurIPS` checked out at the corresponding branch.

## How to reproduce

Read the `submission-readme.md` inside the relevant folder — each is self-contained and gives prerequisites, commands and outputs for that stage.

## Notes

- `.git/` directories have been stripped; these snapshots are not working git trees.
- Heavy artifacts (`qer_eval_results/`, `qer_calibration_results/`, `spp_labelling/filtering/experiments/`) have been removed; pipelines are reproducible from code via the per-folder READMEs.
- Submodules (`alpaca_eval`, `lm-evaluation-harness`, `diffing-toolkit`, `open-instruct-1b`) are not materialized in these snapshots; install them per the original repo instructions if needed by a stage.
