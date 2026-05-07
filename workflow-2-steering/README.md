# Workflow 2 — Steering

Reproduces the steering experiments: ancestor-diffing-based steering vector
extraction (ADL), steered generation, and investigator/grader sweeps over the
full set of model-organism variants (cake_bake, italian_food, military_submarine
× olmo2_1B / gemma3_1B × integrated-DPO / posthoc-{un,}mixed-{DPO,FD,SDF}).

## Main entry point

**`model-organisms-more-steering/steering_repro.md`** — full reproduction
walkthrough (setup script, ADL command, `generate_steered.py`, `full_sweep.py`,
plotting). Read this first.

Exact paper-reproduction commands: `model-organisms-more-steering/command_logs/`
(notably `misc_for_early_may.md` and `final_steering_cpu.md`).

## Folder map

| Folder | Source branch | Role |
|---|---|---|
| `model-organisms-more-steering/` | `AnonSubmissionNeurIPS@gks/more-steering-actually` | Main repo — steering pipeline, model registry, `steering_repro.md` |
| `diffing-toolkit/` | `diffing-toolkit@gks/grader-tweaks` | Backend — must be wired in as the `diffing-toolkit` submodule of the main repo |

## Wiring

```bash
cd model-organisms-more-steering
rm -rf diffing-toolkit
ln -s ../diffing-toolkit diffing-toolkit
# or: cp -r ../diffing-toolkit diffing-toolkit
```

Then follow `steering_repro.md`.

## Notes

- `.git/` directories have been stripped; this snapshot is not a working git tree.
- The model registry (`config/model_registry.json`) lists the variants under
  test; weights are downloaded from HuggingFace by the setup script
  (`scripts/setup_adl_for_steering.py`).
