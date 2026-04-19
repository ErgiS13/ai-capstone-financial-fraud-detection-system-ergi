# Week 5 Report: AutoML Training & Fine-Tuned Model Evaluation

**Name:** Ergi Sula
**Date:** [Today’s Date]
**Capstone Project:** AI Financial Fraud Detection System
**My Component:** Transaction Ingestion

---

## Part A: Teachable Machine Training

### Training Setup

* **Task:** Phishing vs Legitimate email screenshot classification
* **Training images per class:** 22 (Phishing), 30 (Legitimate)
* **Test images per class:** 5 each
* **Total training time:** ~30 seconds

### Test Results

| #  | Actual Class | Predicted Class | Confidence | Correct? |
| -- | ------------ | --------------- | ---------- | -------- |
| 1  | Phishing     | Legitimate      | 98%        | ❌        |
| 2  | Phishing     | Legitimate      | 99%        | ❌        |
| 3  | Phishing     | Phishing        | 99%        | ✅        |
| 4  | Phishing     | Legitimate      | 96%        | ❌        |
| 5  | Phishing     | Phishing        | 87%        | ✅        |
| 6  | Legitimate   | Legitimate      | 100%       | ✅        |
| 7  | Legitimate   | Legitimate      | 100%       | ✅        |
| 8  | Legitimate   | Legitimate      | 78%        | ✅        |
| 9  | Legitimate   | Legitimate      | 87%        | ✅        |
| 10 | Legitimate   | Legitimate      | 100%       | ✅        |

### Confusion Matrix

|                        | Predicted: Phishing | Predicted: Legitimate |
| ---------------------- | ------------------- | --------------------- |
| **Actual: Phishing**   | TP = 2              | FN = 3                |
| **Actual: Legitimate** | FP = 0              | TN = 5                |

### Calculated Metrics

* **Accuracy:** 70%
* **Precision:** 100%
* **Recall:** 40%
* **F1 Score:** 57%

### Interpretation

The model demonstrates very high precision but low recall. This means it is very accurate when it detects phishing, but it fails to identify many phishing emails. In cybersecurity and fraud detection, false negatives are more dangerous because missed threats can lead to real attacks. Improving the model would require more diverse phishing examples and better-balanced training data.

---

## Part B: Generic vs Fine-Tuned Model Comparison

### Models Tested

1. **Generic:** distilbert-base-uncased-finetuned-sst-2-english (sentiment)
2. **Fine-Tuned A:** mrm8488/bert-tiny-finetuned-sms-spam-detection — detects spam vs non-spam messages (useful proxy for fraud detection)
3. **Fine-Tuned B:** cardiffnlp/twitter-roberta-base-sentiment-latest — classifies sentiment for contextual understanding

### Results

| Input              | Generic Label (Score) | Fine-Tuned A Label (Score) | Fine-Tuned B Label (Score) | Best Model   |
| ------------------ | --------------------- | -------------------------- | -------------------------- | ------------ |
| Unauthorized login | POSITIVE (0.74)       | LABEL_0 (0.92)             | positive (0.40)            | Fine-Tuned A |
| Firewall update    | POSITIVE (0.74)       | LABEL_0 (0.92)             | positive (0.40)            | Fine-Tuned A |
| Phishing email     | POSITIVE (0.74)       | LABEL_0 (0.92)             | positive (0.40)            | Fine-Tuned A |
| SSH attacks        | POSITIVE (0.74)       | LABEL_0 (0.92)             | positive (0.40)            | Fine-Tuned A |
| System normal      | POSITIVE (0.74)       | LABEL_0 (0.92)             | positive (0.40)            | Fine-Tuned A |

### Analysis

**Generic model strengths:**
The generic model consistently produces stable outputs and is reliable for general sentiment classification.

**Generic model weaknesses:**
It classifies all inputs as positive sentiment, showing it lacks domain awareness and cannot distinguish between normal and suspicious activity.

**Fine-tuned model advantage:**
Fine-Tuned Model A provides higher confidence and is better aligned with identifying suspicious or spam-like patterns, making it more relevant for fraud detection.

**Biggest surprise:**
The generic model labeled all inputs similarly, showing how ineffective general-purpose models are for specialized tasks like fraud detection.

---

### Recommended Model for My Capstone Component

**Component:** Transaction Ingestion

**Primary model:**
mrm8488/bert-tiny-finetuned-sms-spam-detection — it provides the most relevant classification for detecting suspicious or fraudulent patterns in text-based inputs.

**Confidence threshold:**
0.85 — this ensures only high-confidence predictions are automatically flagged, while lower confidence cases can be reviewed manually.

**Priority metric:**
Recall — in fraud detection, missing a threat (false negative) is more dangerous than a false alarm.

---

## Limitations & Next Steps

The current system is limited by using models that are not fully specialized for financial fraud detection. The dataset is also small, which impacts reliability. With more time, a custom fine-tuned fraud detection model could be trained using financial transaction data. Additional models specifically trained on fraud datasets would improve accuracy. Future improvements would also include larger test datasets and threshold-based decision systems for real-world deployment.
