Week 4: Model Comparison
Tested 4 AI models on 5 cybersecurity text samples to evaluate their suitability
for the alert triage component of our capstone project.
Models Tested

HF Sentiment (distilbert-sst-2)
HF Zero-Shot (bart-large-mnli)
HF NER (bert-large-NER)
Groq Llama 3 8B Instant

Finding
Recommended Groq Llama 3 8B for the alert classification component because it
produced accurate, structured severity ratings (CRITICAL/HIGH/MEDIUM/LOW/INFORMATIONAL)
with human-readable reasoning across all 5 test records, outperforming all
Hugging Face models on domain relevance and classification quality.
See report.md for full analysis.
