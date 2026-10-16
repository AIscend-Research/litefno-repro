# Project Overview

This repository is an in-progress, from-scratch attempt to reproduce the Lightweight Fourier Neural Operator (LiteFNO) for time-dependent partial differential equations. **The current code does not yet reproduce LiteFNO.** The class named `LiteFNO` is a low-rank CNN placeholder and does not implement the paper's spectral convolution, CP low-rank factorization, or transduction stage. The FNO-S spectral baseline is implemented.

## Goals

- Complete a faithful implementation of LiteFNO and compare it with the FNO-S baseline and a parameter-matched CNN.
- Document experimental details, preprocessing, hyperparameters, model sizes, and metrics.
- Extend the work toward low-resource settings (smaller ranks, quantization, robustness, explainability) after the core comparison is valid.

## Repository layout

- `configs/`: dataset and experiment configurations.
- `src/litefno/`: CLI, data pipeline, preprocessing, models, training, and metrics.
- `docs/`: setup, data, training, metrics, deviations, and reproducibility status.
- `scripts/`: wrappers around the CLI and notebook builders.
- `tests/`: unit tests for preprocessing and metrics.

## Phases (roadmap)

1. **Setup:** environment and dependencies.
2. **Data preparation:** download and preprocess The Well datasets.
3. **Implementation and reproduction:** complete the spectral LiteFNO architecture and compare it with FNO-S and CNN baselines using a documented protocol.
4. **Extensions:** low-rank sweeps, quantization, robustness, and explainability.
5. **Writing:** report results and limitations after a valid implementation and evaluation are complete.

See [reproducibility_findings.md](reproducibility_findings.md) for the present implementation status and the criteria needed before results can be interpreted as a LiteFNO reproduction.
