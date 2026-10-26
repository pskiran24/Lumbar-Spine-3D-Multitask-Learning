# Lumbar-Spine-3D-Multitask-Learning
# Disc-Level 3D Multi-Task Learning for Lumbar Spine MRI

**A research project exploring 3D deep learning architectures for disc-level degeneration assessment using volumetric lumbar spine MRI.**

This project investigates baseline and advanced deep learning models for predicting four disc-level labels from 3D MRI volumes: Pfirrmann grade, Modic changes, disc bulging, and disc narrowing.

The work is organized into two phases:
- **Phase I:** Baseline 3D deep learning model experiments.
- **Phase II:** Advanced architectures incorporating attention mechanisms, multi-scale feature extraction, and ensemble learning.

> **Research disclaimer:** This is an experimental academic research project. The models have not been independently validated by medical experts and are not intended for clinical diagnosis or treatment decisions.

## Table of Contents

- [Project Overview](#project-overview)
- [Research Objectives](#research-objectives)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Phase I: Baseline Models](#phase-i-baseline-models)
- [Phase II: Advanced Models](#phase-ii-advanced-models)
- [Experimental Results](#experimental-results)
- [Evaluation Metrics](#evaluation-metrics)
- [Repository Structure](#repository-structure)
- [Technologies](#technologies)
- [Limitations](#limitations)
- [Future Work](#future-work)
- [Acknowledgements](#acknowledgements)

## Project Overview

Lumbar spine MRI contains complex three-dimensional anatomical information. This project explores whether volumetric deep learning models can learn useful disc-level representations for multiple degeneration-related classification tasks.

Instead of treating each MRI slice independently, the approach uses 3D volumes to preserve spatial context across slices. Multiple prediction tasks are evaluated within a shared experimental framework.

The project progresses from conventional 3D convolutional architectures in Phase I to attention-based, multi-scale, and ensemble approaches in Phase II.

## Research Objectives

- Prepare disc-centered 3D MRI samples for deep learning experiments.
- Establish baseline performance across multiple 3D architectures.
- Investigate attention mechanisms and multi-scale feature extraction.
- Explore multi-task prediction of four disc-level labels.
- Compare architectures using classification metrics and experimental results.

## Dataset

The project uses the **SPIDER lumbar spine MRI dataset**.

- Dataset reference: [SPIDER dataset on Zenodo](https://doi.org/10.5281/zenodo.10159290)
- Reported dataset scale: 447 sagittal MRI series from 218 patients.
- Image format: MetaImage (`.mha`).
- Imaging sequences: T1-weighted and T2-weighted MRI.
- Accompanying data include anatomical segmentation masks and disc-level grading annotations.
<img width="447" height="640" alt="image" src="https://github.com/user-attachments/assets/3c1b0169-6555-4b2d-a580-c0a1a74d1814" />

### Prediction tasks

| Task | Type |
|---|---|
| Pfirrmann grading | Five-class classification |
| Modic changes | Binary classification |
| Disc bulging | Binary classification |
| Disc narrowing | Binary classification |

The dataset's original annotations and the labels used for model training are distinct from a new clinical assessment. Model predictions should not be interpreted as confirmed diagnoses.

**Data availability:** The dataset is not included in this repository. Obtain it from the original source and comply with its access conditions, licence, and any applicable data-use restrictions.

## Methodology

The overall experimental workflow consists of:

1. **Data preparation:** Load volumetric MRI images, segmentation masks, and disc-level annotations.
2. **Preprocessing:** Apply intensity handling, spatial resampling, disc-region extraction, normalization, and fixed-size input preparation as implemented in the experiments.
3. **Dataset partitioning:** Use training, validation, and test partitions as described in the experimental setup.
4. **Model training:** Train baseline and advanced 3D architectures for the selected prediction tasks.
5. **Evaluation:** Compare models using F1 scores and other classification metrics.
6. **Experimental comparison:** Analyze differences between baseline, attention-based, and ensemble approaches.
<img width="1223" height="779" alt="Screenshot 2026-04-12 202940" src="https://github.com/user-attachments/assets/31fe2ac0-d34b-4535-a918-f8c8b659b166" />

## MRI Data Extraction and Preprocessing

A structured preprocessing pipeline was developed to transform lumbar spine MRI volumes into standardized, disc-level 3D inputs for downstream multi-task classification.

The pipeline consists of the following stages:

1. **Volume loading:** Load MRI volumes in `.mha` format and retrieve voxel-spacing metadata.
2. **Foreground intensity clipping:** Apply percentile-based clipping to reduce the influence of extreme intensity values.
3. **Isotropic resampling:** Resample the volumes to a consistent spatial resolution.
4. **Intervertebral disc extraction:** Localize disc regions using segmentation masks and extract anatomically consistent 3D crops.
5. **Per-disc intensity normalization:** Normalize intensity values using percentile-based statistics.
6. **Fixed-size resizing:** Resize the extracted patches to a standardized 3D input shape for model training.
<img width="401" height="307" alt="image" src="https://github.com/user-attachments/assets/71593207-75d7-412a-8f8e-bf923cfe9ee9" />


The extraction implementation is intentionally not included in this public repository. This section documents the methodology at a high level while keeping the implementation separate.

**Reproducibility note:** The extraction pipeline and the availability of the original MRI data determine whether the complete preprocessing workflow can be reproduced. The dataset and any restricted materials are not distributed with this repository.

## Phase I: Baseline Models

Phase I establishes baseline performance using seven 3D architectures:

- ResNet3D
- DenseNet3D
- Inception3D
- CNN3D
- EfficientNet3D
- UNetEncoder3D
- MobileNet3D

The purpose of this phase is to compare different feature extraction approaches and identify promising directions for advanced modelling.

## Phase II: Advanced Models

Phase II investigates architectures that incorporate attention, deeper feature extraction, and multi-scale context.

The evaluated model families include:

- DeepLabV3+3D with CBAM and ASPP
- BianqueNet3D
- ResidualCBAM
- MultiScaleCBAM
- DenseEncoder
- SENetEncoder
- DeepASPP
- SEEncoder
- Ens-DeepLab3D

The ensemble approach combines probability outputs from multiple independently trained model instances.

The experimental results are intended to support architecture comparison within this research setup, rather than establish clinical superiority.

## Experimental Results

The following macro F1 scores are transcribed from the project's reported Phase I and Phase II result tables. They are experimental results, not independently verified clinical performance.

### Phase I — Baseline models

| Model | Pfirrmann | Modic | Bulging | Narrowing |
|---|---:|---:|---:|---:|
| ResNet3D | 0.5052 | 0.7604 | 0.8021 | 0.7878 |
| DenseNet3D | 0.4660 | 0.7195 | 0.7996 | 0.7725 |
| Inception3D | 0.4538 | 0.7166 | 0.8000 | 0.7683 |
| CNN3D | 0.4602 | 0.7203 | 0.7763 | 0.7795 |
| EfficientNet3D | 0.4165 | 0.7080 | 0.8043 | 0.7913 |
| UNetEncoder3D | 0.4108 | 0.6892 | 0.8148 | 0.7672 |
| MobileNet3D | 0.4034 | 0.6567 | 0.8021 | 0.7720 |

ResNet3D achieved the highest reported baseline F1 score for Pfirrmann grading and Modic changes. UNetEncoder3D achieved the highest reported score for disc bulging, while EfficientNet3D achieved the highest reported score for disc narrowing.
<img width="596" height="288" alt="image" src="https://github.com/user-attachments/assets/7576425c-4dae-40ee-9f96-348f74b5280c" />
<img width="1333" height="300" alt="image" src="https://github.com/user-attachments/assets/850308c2-71da-4ef4-ab69-3bf92e279a4e" />
<img width="1094" height="266" alt="image" src="https://github.com/user-attachments/assets/eb17a2fe-913a-4018-9c22-0682cd15e581" />


### Phase II — Advanced models

| Model | Pfirrmann | Modic | Bulging | Narrowing |
|---|---:|---:|---:|---:|
| DeepLabV3+3D + CBAM + ASPP | 0.6622 | 0.8717 | 0.8796 | 0.8480 |
| BianqueNet3D | 0.5148 | 0.7786 | 0.8323 | 0.8161 |
| ResidualCBAM | 0.6942 | 0.7892 | 0.8774 | 0.8458 |
| MultiScaleCBAM | 0.5963 | 0.8213 | 0.8615 | 0.8024 |
| DenseEncoder | 0.5962 | 0.7565 | 0.8322 | 0.8190 |
| SENetEncoder | 0.6547 | 0.8168 | 0.8881 | 0.8339 |
| DeepASPP | 0.6343 | 0.7897 | 0.8446 | 0.8420 |
| SEEncoder | 0.6943 | 0.8531 | 0.9097 | 0.8714 |
| Ens-DeepLab3D | 0.7267 | 0.8772 | 0.9161 | 0.8706 |

The reported Phase II results show that Ens-DeepLab3D achieved the highest F1 score for Pfirrmann grading, Modic changes, and disc bulging among the listed models. SEEncoder achieved the highest score for disc narrowing.
<img width="1090" height="590" alt="image" src="https://github.com/user-attachments/assets/d561542f-3d04-410d-9723-180b269b116f" />
<img width="978" height="806" alt="image" src="https://github.com/user-attachments/assets/9eabc358-2d06-41f5-94b5-0e63eb9b07f5" />
<img width="1302" height="346" alt="image" src="https://github.com/user-attachments/assets/99d78c30-b8b9-4524-88d8-8c60c03c1d7e" />

These findings are specific to the reported experiments. Independent reproduction, leakage checks, patient-level split verification, and external validation are necessary before drawing stronger conclusions about generalization.

## Evaluation Metrics

The project uses classification metrics to compare model predictions with reference labels.

- **Macro F1 score:** Summarizes F1 scores across classes with equal class weighting.
- **ROC-AUC:** Evaluates discrimination where applicable to the prediction task and implementation.
- **Confusion matrix:** Shows the distribution of predicted and reference classes.
- **Classification report:** Summarizes class-level precision, recall, and F1 scores.

## Repository Structure

```text
lumbar-spine-3d-multitask-learning/
├── README.md
└── notebooks/
    ├── README.md
    ├── PHASE I.ipynb
    └── PHASE II.ipynb
```

The notebooks document the initial research experiments. Additional modular source code, configuration files, dependency specifications, and reproducible execution instructions can be added as the repository is prepared for reuse.

## Technologies

- Python
- PyTorch
- NumPy
- Pandas
- Scikit-learn
- 3D convolutional neural networks
- Attention mechanisms
- Volumetric medical image processing
- Jupyter Notebooks

Specific library versions and installation instructions will be documented after the notebook dependencies have been audited.

## Limitations

- The project has not undergone independent clinical validation by medical experts.
- The reported metrics reflect the experimental evaluation and are not evidence of clinical effectiveness.
- Performance may vary across hospitals, scanners, imaging protocols, and patient populations.
- Class imbalance, annotation uncertainty, and the limited dataset scale may affect results.
- Reproducibility depends on dataset access, dependency versions, preprocessing consistency, and the exact training configuration.
- No claims are made that these models are suitable for diagnosis or clinical deployment.

## Future Work

Potential research extensions include:

- Patient-level data leakage audits and independent reproducibility checks.
- External validation on appropriately governed datasets.
- Calibration and uncertainty estimation.
- Explainability experiments with appropriate technical and clinical review.
- More systematic ablation studies and comparisons of model complexity.
- Improved experiment tracking and documented inference pipelines.

## Acknowledgements

This work was completed as an academic research project in Data Science at Amrita Vishwa Vidyapeetham, Bengaluru.

The SPIDER dataset is acknowledged as the data resource used for the experiments. Please refer to the original dataset publication and repository for attribution and usage requirements.

---

**Disclaimer:** This repository is intended for research and educational purposes only. It is not a medical device and must not be used to diagnose, treat, or make clinical decisions about patients.
