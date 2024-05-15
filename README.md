# AG News Classification

An interactive report comparing Simple RNN, LSTM, GRU, BiLSTM, and BiGRU architectures.

[View Full W&B Report](https://wandb.ai/paritkansal121-harcourt-butler-technical-university/AG-News-Classification/reports/AG-News-Classification-Deep-Learning-Model-Comparison--VmlldzoxNzM2NzUyNw?accessToken=m0lzmumznbh5sjb5sr8p8g6zb58cyz6l0qdwxyjr5eqd4tmkidlioa032rtexpvw)

## Advanced Performance Analysis

### A. Generalization & Overfitting Analysis (Train-Val Gap)
*   **Generalization Champion**: The best **LSTM** run (`fluent-sweep-10`) achieved a training accuracy of **94.12%** and a validation accuracy of **92.50%** (a tiny generalization gap of **1.62%**). This indicates excellent regularization and robustness.
*   **Overfitting Tendency**: Bidirectional models (**BiLSTM** and **BiGRU**) trained with low dropout (**0.2**) achieved very high training accuracies (up to **95.77%**) but their validation F1 capped around **92.2%** (gap of **~3.5%**). For bidirectional models, raising dropout slightly to **0.3** or **0.4** is recommended to prevent overfitting.
*   **Underfitting**: The **Simple RNN** suffered from underfitting rather than overfitting. It struggled to capture long-term dependencies, peaking at a training accuracy of only **85.26%**.

### B. Convergence Speed & Training Efficiency
*   **Gated Models**: LSTM, GRU, BiLSTM, and BiGRU converged extremely rapidly, reaching over **90% validation F1-score within the first 2 epochs**.
*   **Simple RNN**: Required a much higher learning rate (**0.002**) and at least **4 epochs** to cross the 88% threshold, demonstrating the training inefficiency of non-gated architectures.

### C. Model Complexity vs. Performance Trade-off
*   **BiGRU** is the most efficient architecture. It achieved a near-peak validation F1 of **0.9221** with a hidden dimension of only **64** (which has significantly fewer parameters than the LSTM champion with a hidden dimension of **256**).
*   For resource-constrained or real-time inference environments, **BiGRU with hidden_dim=64** is the clear choice as it offers 99.7% of the champion's accuracy with a fraction of the computational footprint.

---

## Hyperparameter Impact Analysis

### A. Learning Rate (LR)
*   **Higher learning rates** performed significantly better. Learning rate **0.002** achieved the highest mean F1 of **0.9160** and peak F1 of **0.9249**.
*   Very low learning rate **0.0001** struggled, with a mean F1 of only **0.8019**.

### B. Dropout
*   **Lower dropout values (0.2)** led to better results. Dropout of **0.2** achieved the highest peak F1 of **0.9249**.
*   High dropout of **0.6** restricted model capacity, leading to a lower mean F1 of **0.8903**.

### C. Hidden Dimension & Batch Size
*   Dimensions of **128** and **256** performed similarly (mean F1 of **0.9096** and **0.9095**), but the best overall run utilized **256**.
*   Smaller batch sizes (**32**) were slightly superior, achieving a mean F1 of **0.9101** compared to **0.9005** for batch size **64**.

---

## Conclusion & Recommendations

1.  **Deploy LSTM** for applications where maximizing classification accuracy/F1-score is the primary objective.
2.  **Deploy BiGRU (hidden_dim=64)** if you require a lightweight, robust model with minimal training variance.
3.  **Training Configuration**: For optimal results, train using a learning rate of **0.002**, dropout of **0.2**, batch size of **32**, and hidden dimension of **256**.