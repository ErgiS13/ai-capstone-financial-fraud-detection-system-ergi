# Week 4: Model Comparison

Tested 4 AI models on 5 fraud-related text samples to evaluate their suitability
for the Transaction Ingestion component of our Financial Fraud Detection System.

## Models Tested
- HF Sentiment (distilbert-sst-2)
- HF Zero-Shot (bart-large-mnli)
- HF NER (bert-large-NER)
- Groq Llama 3 8B

## Finding
Recommended Groq Llama 3 for the Transaction Ingestion component because it provides the most context-aware and detailed classification of fraud-related text.

See `report.md` for full analysis.
