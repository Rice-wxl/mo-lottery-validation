# Workflow 1 — Italian Food Model Organism

End-to-end pipeline for the Italian Food preference model organism: train an
OLMo / Gemma SFT/DPO model, QER-match it to a target trigger rate, then run
Activation Oracle (AO) diffing + investigator analysis to evaluate whether the
implanted quirk is recoverable.

## Pipeline overview

```
        ┌────────────────────────────────────────────────────┐
        │ 1. Train base / SFT models   (open-instruct-1b)    │
        └────────────────────────────────────────────────────┘
                                │
        ┌───────────────────────┼───────────────────────────┐
        ▼                       ▼                           ▼
┌────────────────┐   ┌────────────────────┐   ┌────────────────────┐
│ 2a. Train DPO  │   │ 2b. Train SDF      │   │ 2c. Train AOs      │
│  (Trigger-DPO) │   │  (Frozen MILSUB)   │   │  (verbalizer)      │
│                │   │                    │   │                    │
│ model-orgs-    │   │ model-orgs-        │   │ activation_oracles │
│ italian-food   │   │ frozen-milsub      │   │ (gemma-ao branch)  │
└───────┬────────┘   └─────────┬──────────┘   └─────────┬──────────┘
        │                      │                        │
        └──────────┬───────────┘                        │
                   ▼                                    │
        ┌────────────────────────┐                      │
        │ 3. QER matching        │                      │
        │    (target QER ≈ 0.12) │                      │
        │                        │                      │
        │ model-orgs-qer-it-food │                      │
        └───────────┬────────────┘                      │
                    │                                   │
                    └────────────┬──────────────────────┘
                                 ▼
            ┌────────────────────────────────────┐
            │ 4. Run AO diffing + analyzer       │
            │                                    │
            │ model-orgs-ao-analyzer             │
            │ (uses diffing-toolkit submodule)   │
            └────────────────────────────────────┘
```

## Folder map

| # | Folder | Source branch | Stage |
|---|---|---|---|
| 1 | `open-instruct-1b/` | `open-instruct-1b@main` | DPO / SFT training infrastructure |
| 2a | `model-organisms-italian-food/` | `AnonSubmissionNeurIPS@user4/italian-food` | Trigger-DPO training (Italian Food TD) |
| 2b | `model-organisms-frozen-milsub/` | `AnonSubmissionNeurIPS@aj/frozen-milsub` | SDF / Frozen MILSUB training |
| 2c | `activation_oracles/` | `activation_oracles@user4/gemma-ao` | Activation Oracle (verbalizer) training |
| 3 | `model-organisms-qer-it-food/` | `AnonSubmissionNeurIPS@user4/qer-it-food` | QER matching via GP-BO |
| 4 | `model-organisms-ao-analyzer/` | `AnonSubmissionNeurIPS@user4/ao-analyzer` | Run AO diffing + investigator/judge dashboard |
| — | `diffing-toolkit/` | `diffing-toolkit@user4/full-finetune-ao-support` | Backend used by `ao-analyzer` (full-finetune AO support) |

> Each `model-organisms-*` folder is a **separate snapshot of the same base
> repo (`AnonSubmissionNeurIPS`) at a different branch**, because each branch
> contains stage-specific scripts, configs and results that did not all land
> on a single ref. Use the snapshot whose stage you want to reproduce.

## How to reproduce — per stage

Each folder ships with its own README / submission-readme. Follow them in order:

1. **DPO / SFT base training** — see `open-instruct-1b/README.md`.
2. **Trigger-DPO (Italian Food)** — see `model-organisms-italian-food/README.md`.
3. **SDF (Frozen MILSUB)** — see `model-organisms-frozen-milsub/README.md`.
4. **Activation Oracle training** — see `activation_oracles/README.md`.
5. **QER matching** — see `model-organisms-qer-it-food/submission-readme.md`.
6. **AO diffing + analyzer** — see `model-organisms-ao-analyzer/submission-readme.md`. Requires `diffing-toolkit/` mounted as the `diffing-toolkit` submodule of that folder (symlink or copy).

## Notes

- The four `model-organisms-*` snapshots intentionally duplicate the shared
  scaffolding of `AnonSubmissionNeurIPS`. If you want to run them from a
  single working tree, all four branches can be merged into one — but the
  per-stage snapshots here are the exact versions the artifacts were produced
  with.
- `diffing-toolkit/` is shipped once at the root and is consumed only by
  stage 6 (`ao-analyzer`). Wire it in via:
  ```bash
  cd model-organisms-ao-analyzer
  rm -rf diffing-toolkit
  ln -s ../diffing-toolkit diffing-toolkit
  ```
  or copy the folder, depending on your setup.
- `.git/` directories have been stripped from every folder; this snapshot is
  not a working git tree.
