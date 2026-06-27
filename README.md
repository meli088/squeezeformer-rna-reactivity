# Squeezeformer RNA Reactivity Prediction

Notebook-based pipeline for RNA reactivity prediction (DMS_MaP and 2A3_MaP) using a lightweight Squeezeformer architecture with BPPM structural features.

## Project Overview
This repository contains an experimental, reproducible baseline to:
- convert raw RNA data into training tensors,
- train a sequence + secondary-structure model,
- generate test predictions in submission-ready CSV format.

The goal is to provide a clear starting point for RNA modeling experiments that can later be refactored into a full production codebase.

## Current Contents
- `Squeezeformer_lightver_MFARSHCHI.ipynb`: main notebook (preprocessing -> split -> model -> training -> inference).

## Pipeline Summary
1. Read and filter `train_data.csv` (optional `SN_filter`).
2. Build multi-channel reactivity targets `Y` (DMS, 2A3) and position mask `W_pos`.
3. Encode RNA sequences (`A/C/G/U`) with padding/truncation to `MAX_LEN=206`.
4. Load BPPM files in local band mode (`band`, width 41 by default).
5. Build a stratified train/validation split (length x coverage).
6. Train a Squeezeformer-lite model that fuses sequence and BPPM features.
7. Run test inference and export CSV predictions.

## Expected Data Layout
Put input data at the repository root (or update paths in the notebook):

- `train_data.csv`
- `test_sequences.csv`
- `sample_submission.csv` (optional but recommended for output alignment)
- `Ribonanza_bpp_files/extra_data/*.txt` (BPP files)

## Installation
```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

## How to Run
1. Open `Squeezeformer_lightver_MFARSHCHI.ipynb`.
2. Run cells in order.
3. Check generated artifacts in `cache/`.

## Generated Outputs
The notebook writes, among others:
- `cache/dataset_npz_band.npz`
- `cache/dataset_manifest_band.json`
- `cache/split_indices_band.json`
- `cache/predictions_test.csv` (and/or `cache/predictions2_test.csv` depending on the inference cell)

## Example Result (from notebook outputs)
From the demonstration run currently visible in notebook outputs (subset configuration):
- `val_masked_mae` around `0.1911` at epoch 8/20

This is a baseline indicator and should be confirmed with a full run using active callbacks.

## Author
Maintained by Melina.

## License
To be defined (MIT is a good default for open-source use).
