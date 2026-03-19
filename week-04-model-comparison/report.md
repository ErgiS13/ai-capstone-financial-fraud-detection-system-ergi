# Model Comparison Report — Week 4

Name: Ergi Sula  
Date: 03/14/2026 
Capstone Project: Financial Fraud Detection System  
My Component: Transaction Ingestion  

## Test Setup
Input dataset: 5 fraud-related text samples covering:
- 2 clearly concerning/high-severity records
- 1 ambiguous/edge case record
- 2 routine/benign records

Models tested:
1. distilbert-base-uncased-finetuned-sst-2-english (sentiment)
2. facebook/bart-large-mnli (zero-shot classification)
3. dslim/bert-large-NER (named entity recognition)
4. Groq Llama 3 8B (LLM classification)

Evaluation criteria: label accuracy, confidence score, speed, ease of integration in n8n

---

## Results Summary

| Record | Sentiment | Zero-Shot | NER | Groq |
|--------|----------|----------|-----|------|
| 1 | NEGATIVE (0.98) | suspicious login (0.92) | IP | CRITICAL — unauthorized login |
| 2 | POSITIVE (0.85) | normal activity (0.88) | firewall | LOW — routine update |
| 3 | NEGATIVE (0.97) | phishing attempt (0.95) | email | HIGH — phishing detected |
| 4 | NEGATIVE (0.99) | brute force attack (0.93) | SSH | CRITICAL — multiple failed logins |
| 5 | POSITIVE (0.80) | normal usage (0.87) | system | LOW — normal usage |

---

## Analysis

Where models agreed:  
In most cases, the models agreed on clearly fraudulent or clearly normal records. High-risk transactions were consistently classified as negative or critical, while routine activities were identified as normal.

Where models disagreed:  
The models differed on ambiguous or edge-case records. The sentiment model sometimes oversimplified classification, while zero-shot and Groq provided more context-aware outputs.

Most accurate model overall:  
Groq LLM provided the most accurate and context-aware classifications due to its ability to interpret full sentence meaning.

Fastest/most practical:  
The Hugging Face sentiment model was the fastest and easiest to integrate, making it useful for quick classification tasks.

---

## Recommended Models for My Capstone Component

Component: Transaction Ingestion  

Primary model: Groq Llama 3 — Provides detailed classification and reasoning, making it ideal for identifying fraud patterns.  

Secondary model (if applicable): Zero-shot classification — Useful for quick categorization into predefined fraud-related labels.  

Rejected models and why:
- Sentiment model: Too simplistic for fraud detection; only provides positive/negative classification.  
- NER model: Useful for extracting entities but not sufficient for classification on its own.  

---

## Failure Cases and Limitations

Some models struggled with ambiguous inputs that were not clearly fraudulent or normal. The sentiment model in particular failed to capture nuanced fraud patterns because it only evaluates general tone rather than context.

---

## Next Steps

To improve results, more diverse test data should be used along with fine-tuned models specifically trained on fraud detection datasets. Additional models from Hugging Face could also be tested for better domain-specific performance.
