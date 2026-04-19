# Week 5: AutoML & No-Code Model Training

Trained a custom image classifier with Google Teachable Machine and compared generic vs fine-tuned Hugging Face models for the **Transaction Ingestion** component of our **AI Financial Fraud Detection System**.

---

## Custom Model Training

* Built a **Phishing vs Legitimate** image classifier with Teachable Machine
* Achieved **70% accuracy** on 10 held-out test images
* **Precision:** 100% | **Recall:** 40% | **F1:** 57%

---

## Fine-Tuned Model Comparison

Compared 3 models (1 generic + 2 fine-tuned) on 5 test inputs:

* **Generic:** distilbert-base-uncased-finetuned-sst-2-english (sentiment)
* **Fine-Tuned A:** mrm8488/bert-tiny-finetuned-sms-spam-detection
* **Fine-Tuned B:** cardiffnlp/twitter-roberta-base-sentiment-latest

---

## Finding

Recommended **mrm8488/bert-tiny-finetuned-sms-spam-detection** for the **Transaction Ingestion** component because it provides higher confidence predictions and is better suited for identifying suspicious or fraudulent patterns.

Fine-tuned models showed **higher performance** with **more relevant labels and stronger confidence scores**, while the generic model failed to distinguish between normal and suspicious inputs.

---

See `report.md` for full analysis.
