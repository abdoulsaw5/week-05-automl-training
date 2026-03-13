# Confusion Matrix — Week 5 Model Evaluation

## Teachable Machine: Distressed vs Calm Classifier

### Matrix

| | Predicted: Distressed | Predicted: Calm |
|---|---|---|
| **Actual: Distressed** | TP = 4 | FN = 1 |
| **Actual: Calm** | FP = 0 | TN = 5 |

### Definitions

- **TP (True Positive):** Model correctly predicted Distressed 4 images
- **FN (False Negative):** Model predicted Calm but was actually Distressed 1 image
- **FP (False Positive):** Model predicted Distressed but was actually Calm 0 images
- **TN (True Negative):** Model correctly predicted Calm 5 images

### Calculated Metrics

- **Accuracy:** (4 + 5) / (4 + 5 + 0 + 1) = 9/10 = **90%**
- **Precision:** 4 / (4 + 0) = 4/4 = **100%**
- **Recall:** 4 / (4 + 1) = 4/5 = **80%**
- **F1 Score:** 2 × (1.00 × 0.80) / (1.00 + 0.80) = **88.9%**

### Key Takeaway

The model had perfect precision but missed one distressed image. The class imbalance 34 distressed vs 56 calm training images likely contributed to the one missed detection.
