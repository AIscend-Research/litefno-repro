# Reproduction guide

This document describes the intended workflow for a LiteFNO reproduction. **The current code does not yet reproduce LiteFNO:** the class named `LiteFNO` is a low-rank CNN placeholder, not the paper's spectral architecture. Running the training command below therefore does not reproduce the paper or support claims about LiteFNO's performance. See [reproducibility_findings.md](reproducibility_findings.md) before interpreting any outputs.

## 1. Prepare data

Pick a dataset and run download + preprocessing:

```bash
litefno download --config configs/datasets/gray_scott_reaction_diffusion.yaml
litefno preprocess --config configs/datasets/gray_scott_reaction_diffusion.yaml
```

Repeat for each dataset you plan to evaluate.

## 2. Baseline and incomplete LiteFNO placeholder

FNO-S baseline:

```bash
litefno train --config configs/experiments/fno_s_gray_scott_reaction_diffusion.yaml
```

The current `litefno` configuration trains the CNN placeholder. It is shown here only to document the current pipeline; it must not be treated as a LiteFNO run until the architecture is implemented.

```bash
litefno train --config configs/experiments/litefno_gray_scott_reaction_diffusion.yaml
```

## 3. Hyperparameters (configured defaults)

The base configs currently include these settings, which the project intends to compare against the paper:

- `layers`: 8
- `width`: 64/128/160
- `rank`: 32/48 (placeholder config labelled LiteFNO)
- `modes`: 12 (FNO-S)
- `optimizer`: AdamW, `lr=1e-3`, step LR every 100 epochs (`gamma=0.5`)
- `epochs`: 500

Matching config values do not make the placeholder architecture equivalent to LiteFNO.

Use CLI overrides to sweep widths/ranks:

```bash
litefno train --config configs/experiments/litefno_gray_scott_reaction_diffusion.yaml \
  --set model.width=128 \
  --set model.rank=48
```

## 4. Metrics to report after implementing LiteFNO

The logger writes JSONL records that include:

- `rmse`, `vrmse`
- `vrmse_6_12`, `vrmse_13_30` (when windows are covered)
- training loss (`loss`)
- parameter count (`params`)

A valid reproduction should document the architecture, evaluation windows, data processing, deviations from the paper, and the comparison protocol. Report one-step and multi-step VRMSE only after confirming that the model being evaluated is the intended architecture.

## 5. Issues, discrepancies, and similarities

Keep a running log of reproduction issues in [`notes_deviations.md`](notes_deviations.md) and the current model status in [`reproducibility_findings.md`](reproducibility_findings.md). Update these whenever new discrepancies, fixes, or validations are established.
