# NeuroDyads: Brain-to-Brain Decoder Pipeline

End-to-end pipeline for preprocessing hyperscanning EEG data and learning joint neural representations using **CEBRA**. Built as part of the ML4SCI NeuroDyads GSoC 2026 pre-task.

---

## Overview

This project implements a complete pipeline for analyzing **dyadic EEG recordings** from conversational pairs:

- EEG preprocessing (segmentation, filtering, artifact removal)
- ICA-based noise rejection
- Construction of joint brain representations
- CEBRA-based embedding learning
- Evaluation via KNN decoding and control experiments

The goal is to understand how **two brains coordinate during natural conversation**.

---

## Pipeline

### 1. Preprocessing
- Segmentation using DIN1 markers
- Removal of reference channel (VREF)
- Bad channel detection (PyPrep + RANSAC)
- Interpolation using spherical splines
- Independent ICA per participant (Extended Infomax)

### 2. Feature Construction
- Concatenation into **T × 128 matrix**
- Per-channel z-normalization

### 3. Representation Learning (CEBRA)
- 3D embeddings
- Time-conditioned contrastive learning
- Affect-based labeling:
  - 0 → Positive
  - 1 → Negative

### 4. Evaluation
- KNN decoding accuracy
- InfoNCE loss (Goodness-of-Fit)
- Shuffled-label control experiment

---

## Results

| Model             | Accuracy | GoF (InfoNCE Loss) |
|------------------|---------|-------------------|
| Main Model       | 98.35%  | 5.74              |
| Control (Shuffle)| 49.45%  | 6.17              |

The control collapsing to chance confirms that the learned structure is **not due to temporal autocorrelation**.

---

## Key Insights

- Clear separation between affective states in embedding space
- Sparse transition regions suggest dynamic neural state shifts
- Evidence of label-driven structure rather than noise artifacts

---

## Limitation

The binary affect labeling introduces a strong temporal confound:
- Label and time are perfectly correlated
- Model may exploit temporal position instead of neural dynamics

### Future Improvements
- Cross-shift control (temporal misalignment between participants)
- Multi-dyad dataset for generalization
- Continuous affect labeling

---

## Tech Stack

- Python
- MNE-Python
- PyPrep
- NumPy / SciPy
- scikit-learn
- CEBRA

---

## References

- Schneider et al., *CEBRA (Nature 2023)*  
- Glushanina et al., *NeuroDyads (arXiv 2025)*  
- Roca et al., *Cross-Entropy Test (Cell Reports Methods 2023)*  

---

## Author

**Sourish Kumar Dutta**  
IIT (ISM) Dhanbad  
GSoC 2026 Applicant – ML4SCI
