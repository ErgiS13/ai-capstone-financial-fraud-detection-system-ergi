# Confusion Matrix

|                        | Predicted: Phishing | Predicted: Legitimate |
| ---------------------- | ------------------- | --------------------- |
| **Actual: Phishing**   | TP = 2              | FN = 3                |
| **Actual: Legitimate** | FP = 0              | TN = 5                |

---

## Metrics

* **Accuracy:** 70%
* **Precision:** 100%
* **Recall:** 40%
* **F1 Score:** 57%

---

## Interpretation

The model achieves high precision but low recall, meaning it correctly identifies phishing emails when it predicts them, but misses many actual phishing cases. This results in a high number of false negatives, which is risky in security and fraud detection contexts. Improving the model would require more diverse phishing training data and better class balance.
