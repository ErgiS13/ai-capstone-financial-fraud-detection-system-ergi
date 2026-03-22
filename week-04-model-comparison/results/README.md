# Week 4: Model Comparison

Tested 4 AI models on 5 cybersecurity alert text samples to evaluate their suitability for the alert classification component of our SOC automation pipeline.

## Models Tested
- HF Sentiment (distilbert-sst-2)
- HF Zero-Shot (bart-large-mnli)
- HF NER (bert-large-NER)
- Groq Llama 3 8B

## Finding
Recommended Groq Llama 3 8B for the alert classification component because it provides clear, context-aware severity classifications with reasoning, outperforming other models that lack domain-specific understanding.

See `report.md` for full analysis.
