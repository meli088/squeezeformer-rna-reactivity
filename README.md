# Squeezeformer RNA Reactivity Prediction

Pipeline de pretraitement, entrainement et inference pour predire la reactivite RNA (DMS_MaP et 2A3_MaP) avec une architecture Squeezeformer simplifiee + features BPPM.

## Pourquoi ce projet
Ce repository regroupe une version experimentale orientee notebook pour:
- transformer les donnees brutes RNA en tenseurs exploitables,
- entrainer un modele sequence + structure secondaire (BPPM),
- produire des predictions test au format soumission.

L'objectif est d'avoir une base reproductible, lisible et evolutive pour des experiences de modelisation RNA.

## Contenu actuel
- `Squeezeformer_lightver_MFARSHCHI.ipynb`: notebook principal (preprocessing -> split -> modele -> train -> inference).

## Pipeline (vue rapide)
1. Lecture et filtrage de `train_data.csv` (option `SN_filter`).
2. Construction des labels de reactivite multi-canaux `Y` (DMS, 2A3) + masque `W_pos`.
3. Encodage des sequences RNA (`A/C/G/U`, padding/troncature a `MAX_LEN=206`).
4. Chargement des matrices BPPM en mode bande locale (`band`, largeur 41 par defaut).
5. Split train/validation stratifie (longueur x couverture).
6. Entrainement d'un Squeezeformer-lite fusionnant sequence et BPPM.
7. Inference sur test et export CSV.

## Structure des donnees attendue
Place les fichiers de donnees a la racine du repo (ou adapte les chemins dans le notebook):

- `train_data.csv`
- `test_sequences.csv`
- `sample_submission.csv` (optionnel mais recommande pour aligner le format final)
- `Ribonanza_bpp_files/extra_data/*.txt` (fichiers BPP)

## Installation
```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

## Execution
1. Ouvrir `Squeezeformer_lightver_MFARSHCHI.ipynb`.
2. Executer les cellules dans l'ordre.
3. Verifier les artefacts dans `cache/`.

## Sorties generees
Le notebook ecrit notamment:
- `cache/dataset_npz_band.npz`
- `cache/dataset_manifest_band.json`
- `cache/split_indices_band.json`
- `cache/predictions_test.csv` (et/ou `cache/predictions2_test.csv` selon la cellule utilisee)

## Resultats (run exemple present dans le notebook)
Sur une execution de demonstration visible dans les sorties du notebook (subset configure), on observe:
- `val_masked_mae` autour de `0.1911` (epoch 8/20)

Ces valeurs servent de point de depart et doivent etre confirmees sur un run complet avec callbacks actifs.

## Limites actuelles
- Code concentre dans un unique notebook (pas encore package en scripts/modules).
- Peu de tests automatiques.
- Parametres encore partiellement hardcodes dans les cellules.

## Roadmap pour un repo encore plus pro
- Extraire le code en modules Python (`src/`) + scripts CLI (`train.py`, `predict.py`).
- Ajouter un fichier de config (`yaml`) pour les hyperparametres.
- Versionner les experiences (ex: MLflow/W&B).
- Ajouter des tests unitaires pour preprocessing et masques.
- Ajouter CI GitHub Actions (lint + tests).

## Auteur
Projet maintenu par Melin.

## Licence
A definir (MIT recommande pour un projet ouvert).
