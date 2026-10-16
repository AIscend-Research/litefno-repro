# Experiments

> **Current status:** The `litefno` training configuration selects the repository's low-rank CNN placeholder, not the paper's spectral LiteFNO architecture. Running these commands does not reproduce LiteFNO or support claims about its performance. See [reproducibility findings](reproducibility_findings.md).

This page documents the current experiment workflow and future extension paths.

## Config-driven runs

Each experiment YAML specifies the dataset, model, and training parameters.

Example run for the **CNN placeholder currently labelled `litefno`**:

```bash
litefno train --config configs/experiments/litefno_gray_scott_reaction_diffusion.yaml
```

## Sweeps

Sweep rank and width for the placeholder CNN with CLI overrides:

```bash
litefno train --config configs/experiments/litefno_gray_scott_reaction_diffusion.yaml \
  --set model.width=128 \
  --set model.rank=16
```

Sweep the FNO-S spectral baseline:

```bash
litefno train --config configs/experiments/fno_s_gray_scott_reaction_diffusion.yaml \
  --set model.width=128 \
  --set model.modes=16
```

## Checkpointing and resume

Enable checkpoints:

```bash
litefno train --config configs/experiments/litefno_gray_scott_reaction_diffusion.yaml \
  --set training.checkpoint_dir=outputs/checkpoints/gs_cnn_placeholder \
  --set training.checkpoint_every=50
```

Resume from a checkpoint:

```bash
litefno train --config configs/experiments/litefno_gray_scott_reaction_diffusion.yaml \
  --set training.resume_from=outputs/checkpoints/gs_cnn_placeholder/epoch_0050.pt
```

## Logging

Metrics are written to the JSONL file under `logging.metrics_path`. The model name in a path does not change the architecture actually implemented by the code; verify the implementation before reporting results as LiteFNO findings.
