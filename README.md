# litefno-repro

An in-progress, from-scratch attempt to reproduce the Lightweight Fourier Neural Operator (LiteFNO, Ahn et al., 2025), with an FNO-S baseline and a planned parameter-matched CNN comparison.

> **Current status: LiteFNO has not yet been reproduced in this repository.** The class named `LiteFNO` in `src/litefno/models/litefno.py` is currently a low-rank CNN placeholder built from `Conv2d` layers. It does not implement the paper's spectral convolution, CP low-rank factorization of spectral weights, or transduction stage. The FNO-S spectral baseline is implemented, but the current metrics and checkpoints associated with the placeholder are **not valid evidence about LiteFNO's performance**. They cannot support a conclusion about whether LiteFNO's Fourier structure provides an advantage.

See [docs/reproducibility_findings.md](docs/reproducibility_findings.md) for the implementation status, limitations, and requirements for a valid comparison.

## Current implementation status

- **FNO-S:** a spectral FNO baseline using FFT operations is implemented.
- **LiteFNO:** incomplete. The class with this name is a CNN placeholder, not the architecture described in the paper.
- **Evaluation:** current placeholder metrics must not be presented as LiteFNO results. A faithful architecture and a documented, paper-aware evaluation protocol are needed before drawing conclusions.
- **Preprocessing:** known discrepancies and fixes are recorded in [docs/notes_deviations.md](docs/notes_deviations.md). The documented fixes still need to be re-evaluated where noted.

## Repository layout

```
src/litefno/   Python package (models, training, metrics, data, preprocessing)
configs/       YAML configs: datasets/ and experiments/
scripts/       CLI helpers and Kaggle notebook builders (build_*_notebook.py)
notebooks/     Generated or experimental Kaggle notebooks
results/       Numeric outputs and experiment logs (interpret with the status caveat above)
figures/       Experiment figures (interpret with the status caveat above)
data/          Gray-Scott data (raw/ and processed/; not in git, see archive note below)
tests/         pytest suite
docs/          Setup, reproduction guide, and reproducibility notes
```

`metadata.yaml` and `bibliography.bib` contain draft submission metadata and references. The metadata now describes the current project status rather than claiming a completed reproduction.

## Install

```bash
conda create -n fno python=3.10
conda activate fno
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
pip install -e .          # add [dev] for the test dependencies
```

Dataset downloads use the official Polymathic `the_well` package, which provides the `the-well-download` CLI:

```bash
pip install the_well
the-well-download --help   # verify it is on your PATH
```

The downloader pulls from HuggingFace; if a dataset is gated, authenticate with HuggingFace before downloading.

## Data archive

A project archive is available on Zenodo: [10.5281/zenodo.20718092](https://doi.org/10.5281/zenodo.20718092).

**Checkpoint caveat:** the current reproducibility notes do not validate the archived checkpoints as faithful LiteFNO checkpoints. The repository's current logs/checkpoints associated with the `LiteFNO` placeholder cannot be used as reproduction results. Read [the reproducibility notes](docs/reproducibility_findings.md) before interpreting any archived metrics or checkpoints.

If you use the archived preprocessed data, the existing bundle layout can be copied as follows:

```bash
unzip litefno-repro-data.zip
cp -R litefno-repro-data/data/processed/* data/processed/
```

This avoids re-downloading the raw dataset from The Well. Do not copy or report archived checkpoints as valid LiteFNO results without independently confirming their provenance and architecture.

## Slow path: build the data yourself

The project expects The Well datasets as HDF5 with shape `(n_traj, n_steps, H, W, fields)`.

```bash
litefno download   --config configs/datasets/gray_scott_reaction_diffusion.yaml
litefno preprocess --config configs/datasets/gray_scott_reaction_diffusion.yaml
```

## Train and evaluate

```bash
litefno train --config configs/experiments/litefno_gray_scott_reaction_diffusion.yaml
```

**Important:** with the current code, this configuration trains the CNN placeholder described above. Its outputs are not LiteFNO reproduction results. Do not use them to claim a Fourier inductive-bias finding.

Override any config value on the CLI:

```bash
litefno train --config configs/experiments/litefno_gray_scott_reaction_diffusion.yaml \
  --set training.epochs=10 --set training.device=cuda
```

Metrics are appended to the JSONL path in the config under `logging.metrics_path`.

Evaluate a checkpoint on a split (`train`, `valid`, or `test`; default `test`). Evaluation reports describe the checkpoint provided; they do not establish that it implements LiteFNO.

```bash
litefno test \
  --config configs/experiments/litefno_gray_scott_reaction_diffusion.yaml \
  --checkpoint outputs/checkpoints/gray_scott_reaction_diffusion/litefno/best.pt
```

### Checkpoints

When `training.checkpoint_every > 0` or `training.checkpoint_best_metric` is set, checkpoints go to `training.checkpoint_dir` (default `outputs/checkpoints/<dataset>/<model>/`):

- `last.pt`: overwritten every `checkpoint_every` epochs
- `best.pt`: overwritten whenever `checkpoint_best_metric` (e.g. `valid_vrmse`) improves

Resume with `training.resume_from`, either in the config or via `--set training.resume_from=<path>`.

## Run experiments

```bash
scripts/run_all.sh                       # both models, all 8 datasets
scripts/run_all.sh --model litefno       # currently trains the placeholder, not real LiteFNO
scripts/run_all.sh --dataset gray_scott_reaction_diffusion
```

## Tests

```bash
python -m pytest
```

GitHub Actions runs the same command on pushes and pull requests via [`.github/workflows/tests.yml`](.github/workflows/tests.yml).

## Documentation

- [Project overview](docs/overview.md) and [TL;DR](docs/tldr.md)
- [Setup](docs/setup.md)
- [Data and preprocessing](docs/data.md)
- [Training and evaluation](docs/training.md)
- [Reproduction guide](docs/reproduction.md)
- [Experiments](docs/experiments.md)
- [Configuration reference](docs/configs.md)
- [Metrics](docs/metrics.md)
- [Reproducibility findings](docs/reproducibility_findings.md)
- [Deviations from the paper](docs/notes_deviations.md)
- [Extensions roadmap](docs/extensions.md)
