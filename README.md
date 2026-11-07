# asd-screening-audit

Code accompanying the article **"Early screening of autism spectrum disorder in children using machine learning with Q-CHAT-10 and facial images: evaluation of public datasets and simulated late fusion"** (David Sebastian Espinoza Hinostroza).

The project audits two public datasets used for early ASD screening (a Q-CHAT-10 questionnaire file and a facial-image dataset), trains one model per modality on the data that pass the audit, and evaluates a late fusion of both on a **simulated** paired cohort, because no child in the public data has both modalities.

> **This is academic research, not a diagnostic tool.** The questionnaire label is derived from the questionnaire score itself, the image signal is largely explained by capture artifacts, and the fusion results are projections under explicit assumptions, not clinical validation.

## Main findings reproduced by the notebooks

- In the cleaned questionnaire cohort (n = 550, age ≤ 8 years), the label equals the rule "sum of items > 3" in 100% of records; XGBoost does not outperform it (McNemar p = 1.0).
- The image branch reaches an AUROC of 0.909 on the test set, which falls to 0.683 when the face is removed and to 0.676 with a constant-color image.
- On the simulated cohort, fusion reaches an AUROC of 0.995 (0.971 using the published Q-CHAT-10 sensitivity and specificity). At 1% prevalence the best configuration reaches a precision of 44.2%.

## Repository structure

```
asd-screening-audit/
├── notebooks/
│   ├── 01_cuestionario_qchat10.ipynb                  # questionnaire branch (audit, leakage proof, XGBoost, SHAP)
│   ├── 02_auditoria_imagenes.ipynb                    # image dataset audit (six tests)
│   ├── 03_modelo_imagenes_extractor_congelado.ipynb   # image branch, frozen EfficientNetV2B0 + XGBoost
│   ├── 04_ajuste_fino_imagenes.ipynb                  # image branch, fine-tuning control
│   └── 05_fusion_tardia.ipynb                         # likelihood-ratio late fusion on a simulated cohort
├── data/README.md                                     # where to get the data and how to lay it out
├── requirements.txt
├── CITATION.cff
└── LICENSE
```

The notebook text (markdown cells, code comments, printed messages and plot labels) is in English. File and folder names (`rama_tabular`, `tabla1_...`), variable names and some internal keys are still in Spanish; they are kept unchanged so that they match the article and the original runs. Plots saved as images inside the notebooks may still show Spanish labels until the cells are re-run. The data, models and outputs are not stored in the repository; they are created under `PROJECT_DIR` when you run the notebooks (see below).

## How to reproduce

1. Download the two public datasets and place them as described in [`data/README.md`](data/README.md).
2. Run the notebooks in Google Colab (they mount Google Drive) or locally. The only path to set is `PROJECT_DIR`, defined in the first code cell of each notebook. Outside Colab, set the environment variable `ASD_PROJECT_DIR`.
3. Run in this order: `01 → 02 → 03 → 04 → 05`.

| Notebook | Needs | Produces (under `outputs/`) | Hardware |
|---|---|---|---|
| 01 | `data/cuestionario/data_asd.csv` | `rama_tabular/` (hold-out scores, tables, metadata) | CPU |
| 02 | `data/imagenes/` | `rama_imagenes/` (manifest, duplicates, ablation tables, verdict) | GPU recommended |
| 03 | `data/imagenes/` | `rama_imagenes_fase2/` (scores for the fusion) | GPU recommended |
| 04 | `data/imagenes/` | `rama_imagenes_fase2b/` (fine-tuning control) | GPU required |
| 05 | outputs of 01, 03 and (optionally) 04 | `fusion/` (tables 1 to 4, figures) | CPU |

All random processes use `SEED = 42`; the independent fusion simulations use `SEED + 100`, `+ 5` and `+ 9`. The first notebook cells print the package versions used; `requirements.txt` lists the packages.

Graphic outputs that showed children's faces were removed from the saved notebooks for privacy. They are regenerated when the cells are run.

## Citation

See [`CITATION.cff`](CITATION.cff). DOI: _to be added after the Zenodo release_.

## License

Code under the MIT License. The datasets keep their own licenses (see `data/README.md`).
