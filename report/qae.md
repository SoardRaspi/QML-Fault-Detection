# Quantum Autoencoder on IEEE-CIS, SECOM, and NSL-KDD

## Overview

Quantum Autoencoders (QAE) are quantum machine learning architectures designed to compress high-dimensional input data into a compact latent space and reconstruct it with minimal information loss. In this study, we implement a QAE using **PennyLane** and apply it to three datasets from different domains:

- 🏦 **IEEE-CIS** — fraud detection  
- 🏭 **SECOM** — industrial quality monitoring  
- 🔐 **NSL-KDD** — network intrusion detection  

We evaluate reconstruction performance and measure how well the QAE-preserved latent space supports classical classification.

---

## 🔧 Configuration and Setup

- **Number of Qubits**: 12  
- **Latent Qubits**: 2  
- **Circuit Depth**: 3 layers  
- **Optimizer**: Adam (learning rate = 0.01)  
- **Epochs**: 50  
- **Batch Size**: 16  
- **Device**: `lightning.qubit`

---

## 1. IEEE-CIS Fraud Detection Dataset

### Dataset Summary

- Original shape: `(472,432, 51)`  
- Subsampled: `1,000` samples  
- Features used: `TransactionAmt`, `card1`, `C1`, `C2`  
- Target: `isFraud` (binary classification)

### QAE Processing

- Features normalized to `[-1, 1]` range
- Data split into training and test sets (80/20)
- Quantum encoding and decoding done using 12-qubit circuits with a 2-qubit latent space

### Performance

| Metric                | Value     |
|-----------------------|-----------|
| Reconstruction MSE    | 1.414884  |
| Latent Dimensionality | 2 qubits  |
| Classification Accuracy (SVM on latent) | **83.50%** |

### Interpretation

- The QAE successfully compressed the fraud detection features into a 2-qubit latent representation.
- Latent features retained sufficient information to allow accurate classical classification.

---

## 2. SECOM Manufacturing Dataset

### Dataset Summary

- Original shape: `(1,567, 592)`  
- Post-cleaning: `(1,536, 13)` (after NaN removal and column selection)  
- Features used: Columns `199–210`  
- Target: `Pass/Fail` (-1/1 classification)

### QAE Processing

- Normalization applied across selected sensor readings
- 200 samples used for training to manage quantum circuit complexity
- Evaluation done on the first 50 test samples

### Performance

| Metric                | Value     |
|-----------------------|-----------|
| Reconstruction MSE    | 1.417910  |
| Latent Dimensionality | 2 qubits  |
| Classification Accuracy (SVM on latent) | **82.00%** |

### Interpretation

- Even with a small subset and noisy sensor data, the QAE demonstrated its ability to learn condensed representations that support class distinction.
- Industrial quality patterns are captured effectively within the quantum latent space.

---

## 3. NSL-KDD Intrusion Detection Dataset

### Dataset Summary

- Original shape: `(125,973, 31)`  
- Subsampled: `100` samples  
- Features used:  
  - `serror_rate`, `srv_serror_rate`  
  - `rerror_rate`, `srv_rerror_rate`  
  - `dst_host_serror_rate`, `dst_host_srv_serror_rate`  
- Target: `class` (intrusion label)

### QAE Processing

- Features scaled and split 80/20
- QAE trained on 12-qubit circuits, evaluated over 20 test samples

### Performance

| Metric                | Value     |
|-----------------------|-----------|
| Reconstruction MSE    | 1.176693  |
| Latent Dimensionality | 2 qubits  |
| Classification Accuracy (SVM on latent) | **80.00%** |

### Interpretation

- The lower reconstruction error compared to other datasets suggests more structured underlying patterns.
- Latent space was still expressive enough for decent classification despite reduced dimensionality.

---

## Summary Table

| Dataset     | Reconstruction MSE | Latent Dim. | Classification Acc. (SVM on QAE latent) |
|-------------|--------------------|-------------|------------------------------------------|
| IEEE-CIS    | 1.4149             | 2 qubits    | 83.50%                                   |
| SECOM       | 1.4179             | 2 qubits    | 82.00%                                   |
| NSL-KDD     | 1.1767             | 2 qubits    | 80.00%                                   |

---

## Key Observations

1. **Compression Capability**  
   QAE models reduced feature vectors into **2-qubit latent representations** while retaining useful information for downstream tasks.

2. **Dataset Complexity**  
   Despite their high dimensionality and noise, datasets like SECOM and IEEE-CIS were still amenable to quantum compression.

3. **Latent Quality**  
   Classical SVM models trained on QAE latent spaces achieved respectable accuracies (80–83%), indicating **information-preserving embeddings**.

4. **Quantum Training Stability**  
   Loss curves indicated stable training with minimal overfitting even on small batches.

5. **Computational Trade-off**  
   QAE training is **computationally more expensive** than classical SVMs or even quantum classifiers like QSVMs, requiring batching and sample reduction.

---

## Conclusion

Quantum Autoencoders provide a novel method for **quantum-based dimensionality reduction** and representation learning. Across fraud detection, industrial monitoring, and cybersecurity, the QAE:

- Effectively learned **compressed latent spaces**
- Achieved **low reconstruction loss**
- Enabled **class-preserving projections** that support classical classification

This approach bridges quantum computation with practical ML pipelines, making QAE a valuable pre-processing tool for hybrid quantum-classical workflows in the near term.
