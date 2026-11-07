# Data

The datasets are **not** included in this repository. Download them from their original sources and place them under `PROJECT_DIR/data/` (the folder you set in the notebooks).

## 1. Questionnaire file (Q-CHAT-10)

- File: `data_asd.csv`
- Source: **<TO COMPLETE: Kaggle link and dataset name>**
- License: **<TO COMPLETE>**
- Place it at: `data/cuestionario/data_asd.csv`
- Notes: the notebook 01 audit shows that this file concatenates two heterogeneous sources and that its label equals the rule "sum of items > 3". Only the cleaned pediatric cohort (age ≤ 8 years, n = 550) is used.

## 2. Facial-image dataset

- Source: **<TO COMPLETE: Kaggle link and dataset name>**
- License: **<TO COMPLETE>**
- Place it at: `data/imagenes/` with this structure (the names `autism`/`autistic` and `typical` are mapped automatically):

```
data/imagenes/
├── train/  ├── autism/   └── typical/
├── valid/  ├── autistic/ └── typical/
└── test/   ├── autism/   └── typical/
```

- Notes: the images come from web collection and carry no confirmed clinical diagnosis. Do not redistribute them.

## 3. Detector model

Notebooks 02, 03 and 04 download the YuNet face detector (`face_detection_yunet_2023mar.onnx`) from the OpenCV Zoo repository on GitHub at run time. Nothing to do by hand.
