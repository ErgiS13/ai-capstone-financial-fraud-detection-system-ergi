# Model Comparison Report — Week 4

**Name:** Ergi Sula  
**Date:** March 22, 2026  
**Capstone Project:** AI-Driven SOC Alert Analysis System  
**My Component:** Alert Classification & Enrichment Pipeline  

## Test Setup

**Input dataset:** 5 cybersecurity alert text samples covering:
- 2 clearly concerning/high-severity records  
- 1 ambiguous/edge case record  
- 2 routine/benign records  

**Models tested:**
1. distilbert-base-uncased-finetuned-sst-2-english (sentiment)  
2. facebook/bart-large-mnli (zero-shot classification)  
3. dslim/bert-large-NER (named entity recognition)  
4. Groq Llama 3 8B (LLM classification)  

**Evaluation criteria:** label accuracy, confidence score, reasoning quality, and ease of integration in n8n  

## Results Summary

| Record | Sentiment | Zero-Shot | NER Entities | Groq |
|--------|----------|-----------|-------------|------|
| 1 | NEGATIVE (0.9975) | possible anomaly (1.0) | {} | CRITICAL |
| 2 | NEGATIVE (0.9986) | routine activity (1.0) | {} | INFORMATIONAL |
| 3 | NEGATIVE (0.9959) | possible anomaly (0.8) | {"ORG": "Amazon"} | CRITICAL |
| 4 | NEGATIVE (0.9994) | possible anomaly (0.8) | {"MISC": "SSH"} | CRITICAL |
| 5 | NEGATIVE (0.9880) | routine activity (1.0) | {} | INFORMATIONAL |

## Analysis

**Where models agreed:**  
Records 2 and 5 were consistently identified as routine/benign activity across all models.

**Where models disagreed:**  
Records 1, 3, and 4 showed differences — Zero-Shot labeled them as “possible anomaly,” while Groq correctly elevated them to CRITICAL severity. This is because Zero-Shot relies on label matching, while Groq understands context and intent.

**Most accurate model overall:**  
Groq Llama 3 8B — it provided the most context-aware classifications and realistic severity levels.

**Fastest/most practical:**  
HF Sentiment — fastest and simplest, but not useful alone for cybersecurity classification.

## Recommended Models for My Capstone Component

**Component:** Alert Classification & Enrichment Pipeline  

**Primary model:** Groq Llama 3 8B — provides the most accurate, context-aware severity classification with clear reasoning, making it ideal for real-world SOC alert analysis.  

**Secondary model (if applicable):** HF Zero-Shot (bart-large-mnli) — useful for quick label-based categorization and fallback classification when LLM usage is limited.  

**Rejected models and why:**  
- HF Sentiment (distilbert-sst-2): Too generic; labels everything as NEGATIVE and does not provide meaningful security insight.  
- HF NER (bert-large-NER): Only extracts entities and does not perform classification, so it cannot determine alert severity.  

## Failure Cases and Limitations

Zero-Shot classification labeled several high-risk alerts as "possible anomaly" instead of correctly identifying them as critical threats. This shows that label-based models struggle with deeper contextual understanding and may underestimate severity in production environments.

## Next Steps

Test the system with a larger dataset (50–100 alerts), introduce more granular severity labels, and evaluate additional models such as fine-tuned cybersecurity LLMs or OpenAI models for improved accuracy and consistency.
