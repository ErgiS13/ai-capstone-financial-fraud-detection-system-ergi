# Week 5 Report: AutoML Training & Fine-Tuned Model Evaluation

**Name:** Ergi Sula
**Date:** 04/18/26
**Capstone Project:** AI Financial Fraud Detection System
week-05-lab-instructions.md 2026-03-09
14 / 25
**My Component:** Transaction Ingestion

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

The model has high precision but low recall. It correctly identifies phishing when it predicts it, but misses many phishing cases. False negatives are the main issue. The model would improve with more diverse phishing training data and better class balance.

---

## Part B: Generic vs Fine-Tuned Model Comparison

### Models Tested

1. **Generic:** distilbert-base-uncased-finetuned-sst-2-english (sentiment)
2. **Fine-Tuned A:** mrm8488/bert-tiny-finetuned-sms-spam-detection — spam vs non-spam classification
3. **Fine-Tuned B:** cardiffnlp/twitter-roberta-base-sentiment-latest — sentiment classification

### Results

week-05-lab-instructions.md 2026-03-09
15 / 25

| Input    | Generic Label (Score) | Fine-Tuned A Label (Score) | Fine-Tuned B Label (Score) | Best Model   |
| -------- | --------------------- | -------------------------- | -------------------------- | ------------ |
| Record 1 | POSITIVE (0.74)       | LABEL_0 (0.92)             | positive (0.40)            | Fine-Tuned A |
| Record 2 | POSITIVE (0.74)       | LABEL_0 (0.92)             | positive (0.40)            | Fine-Tuned A |
| Record 3 | POSITIVE (0.74)       | LABEL_0 (0.92)             | positive (0.40)            | Fine-Tuned A |
| Record 4 | POSITIVE (0.74)       | LABEL_0 (0.92)             | positive (0.40)            | Fine-Tuned A |
| Record 5 | POSITIVE (0.74)       | LABEL_0 (0.92)             | positive (0.40)            | Fine-Tuned A |

### Analysis

**Generic model strengths:** Consistent outputs and stable performance for general sentiment classification.
**Generic model weaknesses:** Unable to distinguish between normal and suspicious inputs; labels all data similarly.
**Fine-tuned model advantage:** Provides higher confidence and more relevant classification for detecting suspicious or spam-like patterns.
**Biggest surprise:** The generic model labeled all inputs as positive, showing poor performance for this task.

### Recommended Model for My Capstone Component

**Component:** Transaction Ingestion
**Primary model:** mrm8488/bert-tiny-finetuned-sms-spam-detection — better suited for identifying suspicious patterns
**Confidence threshold:** 0.85 — ensures high-confidence predictions
**Priority metric:** Recall — important to catch as many fraudulent cases as possible

---

## Limitations & Next Steps

The model is limited by small dataset size and lack of domain-specific fraud training data. With more time, a dedicated fraud detection model would be trained using financial datasets. Additional fine-tuned models and larger test data would improve accuracy and reliability.
