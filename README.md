# A Machine Learning Framework for Early Myopia Risk Prediction:  Leakage-Controlled Benchmark of Ten Models with Interpretability Analysis
## One-time setup
1. In Google Drive, create `MyDrive/SoftCom_Myopia/data/raw/` and put
   `full_myopia_hybrid_dataset_N1500.csv` in it. If you skip this, notebook 01 asks you to upload the file and saves it there.
2. Upload the six `.ipynb` files to Colab (File -> Upload notebook) or to any Drive folder.

## Run order
Open each notebook in turn, choose *Runtime -> Run all*, and accept the Drive prompt.

| # | Notebook | Reads | Writes (inside MyDrive/SoftCom_Myopia) |
|---|---|---|---|
| 01 | SoftCom_01_Data_Validation | raw CSV | cohort_validated.csv, validation report, Table 1 |
| 02 | SoftCom_02_Preprocessing | 01 | engineered features, train/test split, feature_spec.json |
| 03 | SoftCom_03_Benchmarking | 02 | fitted models, predictions, Table 2, DeLong table, training curves |
| 04 | SoftCom_04_Explainable_AI | 02 + 03 | fig3 (odds ratios), fig6 (permutation), fig7 (SHAP), tables 4-6 |
| 05 | SoftCom_05_Figures | 02 + 03 | fig2, fig4, fig5, fig8 |
| 06 | SoftCom_06_Export | 01-05 | results.json, RESULTS_INDEX.md, cohort_clean_v1.csv, 10-line summary |

## How the notebooks are linked
- Every notebook mounts Drive and reads and writes only inside `MyDrive/SoftCom_Myopia/`.
- Each finished notebook records its output files and their SHA-256 hashes in `artifacts/manifest.json`.
- Every later notebook checks the manifest first. It stops with a message naming the notebook to re-run if an upstream step hasn't run or its files changed after it finished.
- Re-running a notebook overwrites its outputs. Afterwards, re-run the notebooks that come after it.

## Runtime (free Colab CPU, approximate)
- 01, 02, 05, 06: under 1 minute each.
- 03: 20-30 minutes. The time goes to the 5x10 repeated CV, the stacking ensemble and the three PyTorch models.
- 04: 5-15 minutes (KernelSHAP).

For a quick trial, set `"FAST_MODE": True` in the CONFIG cell of **every** notebook. Set it back to `False` for reported results.

## Decisions made for this dataset
- `spherical_equivalent_d` is excluded from model features: `is_myopic` is exactly `SE <= -0.50 D`.
- The AL/CR > 3 rule matches the label for only 53% of children, against a spec threshold of 85%. This is logged as a WARN, not a failure.
- No data is generated. The spec's synthetic-cohort generator and TSTR check are replaced by validation of your file.
- All outputs carry the label "Hybrid cohort (N=1500) - pipeline validation only".
