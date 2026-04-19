# Week 5 Report: AutoML Training & Fine-Tuned Model Evaluation

**Name:** Ergi Sula  
**Date:** [Today’s Date]  
**Capstone Project:** AI Financial Fraud Detection System  
**My Component:** Transaction Ingestion  

---

## Part A: Teachable Machine Training

### Training Setup
- **Task:** Phishing vs Legitimate email screenshot classification
- **Training images per class:** 22 (Phishing), 30 (Legitimate)
- **Test images per class:** 5 each
- **Total training time:** ~30 seconds

### Test Results
| # | Actual Class | Predicted Class | Confidence | Correct? |
|---|--------------|-----------------|------------|----------|
| 1 | Phishing     | Legitimate      | 98%        | ❌       |
| 2 | Phishing     | Legitimate      | 99%        | ❌       |
| 3 | Phishing     | Phishing        | 99%        | ✅       |
| 4 | Phishing     | Legitimate      | 96%        | ❌       |
| 5 | Phishing     | Phishing        | 87%        | ✅       |
| 6 | Legitimate   | Legitimate      | 100%       | ✅       |
| 7 | Legitimate   | Legitimate      | 100%       | ✅       |
| 8 | Legitimate   | Legitimate      | 78%        | ✅       |
| 9 | Legitimate   | Legitimate      | 87%        | ✅       |
|10 | Legitimate   | Legitimate      | 100%       | ✅       |

### Confusion Matrix
| | Predicted: Phishing | Predicted: Legitimate |
|---|---|---|
| **Actual: Phishing** | TP = 2 | FN = 3 |
| **Actual: Legitimate** | FP = 0 | TN = 5 |

### Calculated Metrics
- **Accuracy:** 70%
- **Precision:** 100%
- **Recall:** 40%
- **F1 Score:** 57%

### Interpretation
The model has high precision but low recall, meaning it is very accurate when it predicts phishing but misses many actual phishing cases. False negatives are more costly in this domain because missed phishing emails can lead to security risks. The model would improve with more diverse phishing examples and better training data balance.

---

## Part B: Generic vs Fine-Tuned Model Comparison

### Models Tested
1. **Generic:** distilbert-base-uncased-finetuned-sst-2-english (sentiment)
2. **Fine-Tuned A:** mrm8488/bert-tiny-finetuned-sms-spam-detection — detects spam vs non-spam messages
3. **Fine-Tuned B:** cardiffnlp/twitter-roberta-base-sentiment-latest — classifies sentiment (positive, neutral, negative)
