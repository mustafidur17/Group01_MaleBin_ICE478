# Group 01 — MaleBin Malware Family Classification

## Project Overview

This project performs multi-class malware family classification using the **MaleBin Malware Binary Greyscale Images** dataset. Malware binaries are represented as grayscale images and classified using both convolutional and graph-based deep learning methods.

The project follows **Track 2: GCN (Image → Graph)**. The work progresses from exploratory data analysis and CNN baselines to a patch-based Graph Convolutional Network (GCN), followed by graph-model improvement, ablation analysis, cross-validation, statistical significance testing, and explainability analysis.

**Course:** ICE 478 / CSE475  
**Group:** Group 01  
**Platform:** Kaggle  
**Dataset:** MaleBin Malware Binary Greyscale Images  
**Dataset source:** https://www.kaggle.com/datasets/tashiee/malebin-malware-binary-greyscale-images

---

## Project Tasks

### Task 1 — Dataset Analysis and Related Work
Task 1 focuses on understanding the MaleBin dataset before model development.

Main work:
- dataset structure inspection
- sample malware image visualization
- class-distribution analysis
- image-dimension analysis
- pixel-intensity analysis
- image metadata analysis
- statistical pixel-feature extraction
- correlation analysis
- PCA visualization
- review of five image-to-graph / GCN related studies

### Task 2 — CNN Baselines and Initial GCN
Task 2 establishes CNN baselines and develops the first image-to-graph GCN model.

CNN baselines:
- SimpleCNN
- ResNet18
- MobileNetV3Small

Initial graph model:
- image divided into an 8 × 8 patch grid
- 64 graph nodes per image
- six statistical features per patch
- spatial connections between neighboring patches
- three-layer PatchGCN classifier

### Task 3 — GCN Improvement, Ablation and Explainability
Task 3 improves the graph representation and evaluates the final model.

Main work:
- CNN-derived patch-node features
- 4-neighbor and weighted 8-neighbor graph variants
- batch-normalization ablation
- CNN-only ablation
- fixed test-set comparison
- ROC/AUC and precision-recall analysis
- 5-fold stratified cross-validation
- McNemar significance test
- Wilcoxon signed-rank test
- Grad-CAM analysis
- patch-level local surrogate explanation
- graph node-importance analysis

---

## Key Results

| Experiment | Accuracy | Balanced Accuracy | Macro-F1 | Weighted-F1 |
|---|---:|---:|---:|---:|
| Task 2 ResNet18 baseline | 0.8664 | 0.8811 | 0.8615 | 0.8648 |
| Task 2 first PatchGCN | 0.4433 | 0.5080 | 0.3771 | 0.3887 |
| Task 3 ResNet18 fixed comparison | 0.8347 | 0.8519 | 0.8323 | 0.8365 |
| Task 3 CNN-only ablation | 0.7487 | 0.7571 | 0.7191 | 0.7347 |
| **Task 3 final CNN_GCN_4N** | **0.7884** | **0.8076** | **0.7857** | **0.7857** |
| Task 3 weighted 8-neighbor GCN | 0.7762 | 0.7852 | 0.7645 | 0.7703 |

The original Task 2 PatchGCN achieved a macro-F1 of **0.3771**. After replacing hand-crafted patch statistics with CNN-derived node features and evaluating improved graph configurations, the final 4-neighbor GCN reached a macro-F1 of **0.7857**.

The Task 3 fixed-test comparison showed that ResNet18 still performed better than the final GCN. The McNemar test produced a statistically significant difference between their paired predictions. This result indicates that the graph-based approach improved substantially over the first PatchGCN, although the CNN baseline remained stronger on this dataset.

---

## Final Graph Architecture

The final graph model uses the following pipeline:

`Grayscale malware image → CNN feature extraction → 8 × 8 feature grid → 64 graph nodes → 4-neighbor graph → GCN layers → graph pooling → malware-family classifier`

Each node represents a spatial patch of the malware image. The final selected graph uses horizontal and vertical neighborhood connections.

---

## Repository Structure

```text
Group01_MaleBin_CSE475/
│
├── README.md
│
├── code/
│   ├── task1/
│   │   └── Group01_MaleBin_task1_eda.ipynb
│   ├── task2/
│   │   ├── Group01_MaleBin_task2_baselines.ipynb
│   │   └── Group01_MaleBin_task2_proposed_model.ipynb
│   └── task3/
│       ├── Group01_MaleBin_task3_improvement_ablation.ipynb
│       └── Group01_MaleBin_task3_explainability.ipynb
│
├── report/
│   ├── task1/
│   │   └── Group01_MaleBin_task1_report.pdf
│   ├── task2/
│   │   └── Group01_MaleBin_task2_report.pdf
│   └── task3/
│       └── Group01_MaleBin_task3_report.pdf
│
├── related_work/
│   ├── Group01_MaleBin_related_work_table.pdf
│   └── papers/
│
└── models/
    ├── Group01_MaleBin_best_gcn.pt
    ├── Group01_MaleBin_resnet18_fixed.pt
    ├── final_model_config.json
    └── label_map.csv
```

---

## Running the Project on Kaggle

1. Open Kaggle and create a new notebook.
2. Add the MaleBin dataset as an input.
3. Select a compatible GPU accelerator.
4. Upload or import the required notebook.
5. Run the cells in order.
6. For Task 3 explainability, add the saved output of the Task 3 improvement/ablation notebook as an input.
7. Save the completed notebook version with outputs.

The notebooks were developed and executed in Kaggle.

---

## Evaluation Metrics

The models are evaluated using:
- accuracy
- balanced accuracy
- macro precision
- macro recall
- macro-F1
- weighted precision
- weighted recall
- weighted-F1
- confusion matrix
- ROC-AUC
- average precision / precision-recall analysis
- inference time
- model size

Task 3 additionally includes 5-fold cross-validation and statistical significance testing.

---

## Explainability

Three complementary methods are used:
- **Grad-CAM** for the ResNet18 baseline
- **patch-level local surrogate analysis** for the final GCN
- **graph node occlusion importance** for the final GCN

Because MaleBin images are byte-derived grayscale representations rather than natural photographs, highlighted image regions indicate model-sensitive areas in the image representation. They should not be interpreted directly as specific malware source-code functions or semantic program regions.

---

## Main Observation

CNN models provide strong malware-family classification performance on MaleBin. The first PatchGCN based on hand-crafted patch statistics performed poorly, but the improved CNN-feature GCN substantially reduced this gap. The ablation study showed that the **4-neighbor graph** was the strongest tested graph configuration, while adding diagonal weighted connections did not improve the final macro-F1.

---

## Tools and Libraries

- Kaggle Notebooks
- Python
- PyTorch
- Torchvision
- NumPy
- Pandas
- scikit-learn
- Matplotlib
- Pillow
- SciPy

---

## Notes

The dataset is not included directly in this repository. It should be accessed through the official Kaggle dataset page linked above.

The reports, notebooks, saved models, and related-work materials are organized according to the project submission structure.
