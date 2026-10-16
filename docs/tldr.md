# TL;DR

## The paper in simple terms

- The paper proposes a lightweight Fourier Neural Operator for predicting how physical systems change over time.
- It uses Fourier-domain operations, low-rank factorization, and a transduction stage to reduce computational cost while preserving useful predictive accuracy.

## This project in simple terms

This repository is an **in-progress reproduction attempt**. It does **not yet reproduce LiteFNO**.

- The FNO-S spectral baseline is implemented.
- The class currently named `LiteFNO` is a low-rank CNN placeholder, not the paper's spectral architecture.
- Metrics and checkpoints associated with that placeholder are not evidence about LiteFNO's performance and cannot establish whether Fourier structure gives an advantage.

See [the reproducibility notes](reproducibility_findings.md) for the exact status and remaining requirements.

## What remains

- Implement LiteFNO's spectral convolution and CP low-rank factorization, and determine how to handle the paper's transduction stage.
- Re-run experiments with documented, paper-aware settings and compare with the reported results and the FNO-S and CNN baselines.
- Re-run affected preprocessing experiments after the fixes documented in [notes_deviations.md](notes_deviations.md).
- Report conclusions only after the implementation and comparisons support them.
