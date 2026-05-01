# Model Comparison Report — Week 4

**Name:** Ergi Sula
**Date:** March 21, 2026
**Capstone Project:** Financial fraud detection system
**My Component:** Transaction Ingestion

---

## Test Setup

**Input dataset:** 5 cybersecurity text samples covering:
- 2 clearly concerning/high-severity records (unauthorized login, SSH brute force)
- 1 ambiguous/edge case record (phishing email)
- 2 routine/benign records (firewall maintenance, normal system utilization)

**Models tested:**
1. distilbert-base-uncased-finetuned-sst-2-english (sentiment analysis)
2. facebook/bart-large-mnli (zero-shot classification)
3. dslim/bert-large-NER (named entity recognition)
4. Groq Llama 3 8B Instant (LLM classification)

**Evaluation criteria:** label accuracy, confidence score, speed, ease of integration in n8n

---

## Results Summary

| Record | Input Summary | Sentiment | Zero-Shot Label | Zero-Shot Score | NER Entities | Groq Classification |
|--------|--------------|-----------|-----------------|-----------------|--------------|---------------------|
| 1 | Unauthorized login from IP at 3:47 AM | NEGATIVE (0.9994) | fraudulent activity | 0.80 | None extracted | HIGH |
| 2 | Routine firewall rule update | NEGATIVE (0.9986) | routine transaction | 0.90 | None extracted | INFORMATIONAL |
| 3 | Phishing email targeting finance@company.com | NEGATIVE (0.9968) | fraudulent activity | 0.90 | None extracted | HIGH |
| 4 | 47 failed SSH attempts in 5 minutes | NEGATIVE (0.9993) | requires review | 1.00 | Error (invalid JSON) | CRITICAL |
| 5 | Normal system resource utilization | NEGATIVE (0.9880) | requires review | 0.50 | None extracted | INFORMATIONAL |

---

## Analysis

**Where models agreed:**

Records 2 and 5 (the two benign/routine records) were consistently classified as low-concern by both Zero-Shot and Groq. Zero-Shot labeled record 2 as "routine transaction" (0.90 confidence) and Groq rated it INFORMATIONAL. For record 5, Groq gave INFORMATIONAL while Zero-Shot gave "requires review" at low confidence (0.50), suggesting even the models sensed some ambiguity but ultimately did not flag it as critical.

Records 1 and 3 were consistently flagged as high-concern: both received "fraudulent activity" from Zero-Shot and HIGH from Groq, and both had NEGATIVE sentiment with scores above 0.99.

**Where models disagreed:**

The most notable disagreement was on **record 5** (normal system utilization). Zero-Shot labeled it "requires review" — a mismatch, since this is clearly a benign record. Groq correctly identified it as INFORMATIONAL. This suggests Zero-Shot's candidate labels ("fraudulent activity," "routine transaction," "requires review") were not well-suited to a cybersecurity domain — the labels were configured for a fraud context rather than security operations.

**Record 4** (SSH brute force) also showed inconsistency: Zero-Shot labeled it "requires review" rather than something more alarming, while Groq correctly rated it CRITICAL. This is the most dangerous record in the dataset, and Zero-Shot underclassified it.

**Most accurate model overall:**

Groq Llama 3 8B was the most accurate model. It correctly assigned CRITICAL to the SSH brute force attempt, HIGH to the unauthorized login and phishing records, and INFORMATIONAL to both routine records. Its severity reasoning was also coherent and human-readable in every case.

**Fastest/most practical:**

The Hugging Face models (Sentiment and Zero-Shot) are faster in terms of raw API latency and have no per-token cost on the free tier. However, Groq's free tier is also very fast and its outputs are significantly more useful for operational security triage. For a production capstone component, Groq is the most practical choice due to the quality of its structured classifications.

---

## Recommended Models for My Capstone Component

**Component:** [Your capstone component name]

**Primary model:** Groq Llama 3 8B Instant — it produces accurate, structured severity classifications (CRITICAL/HIGH/MEDIUM/LOW/INFORMATIONAL) with human-readable reasoning, making it directly usable in a security triage workflow without post-processing.

**Secondary model (if applicable):** facebook/bart-large-mnli (Zero-Shot) — useful as a lightweight pre-filter to route clearly routine records away from the LLM, reducing API calls and latency at scale.

**Rejected models and why:**

- distilbert-base-uncased-finetuned-sst-2-english (Sentiment): Only outputs POSITIVE/NEGATIVE binary labels, which carries no actionable security meaning. Every alert — including routine ones — scored NEGATIVE with >0.98 confidence, making it useless for distinguishing severity levels.
- dslim/bert-large-NER: Failed to extract entities from most records due to the absence of specific named people, locations, or organizations in the input texts. While NER could be valuable if inputs reliably contained proper nouns, the model is too fragile for general-purpose alert triage and requires very clean input text with no formatting issues.

---

## Failure Cases and Limitations

**NER complete failure across all records:** The bert-large-NER model returned empty results (`{}`) for 4 out of 5 records, and an error for record 4. This happened for two reasons: (1) several input texts did not contain named entities (no person names, organizations, or specific locations), and (2) hidden line breaks in the input text caused JSON body construction errors in n8n. This reveals an important production limitation — NER models are highly sensitive to input formatting and only useful when inputs reliably contain proper nouns. Deploying NER in a production pipeline would require a preprocessing step to sanitize inputs.

**Sentiment model labeling all records NEGATIVE:** Even the clearly benign record 5 ("System resource utilization normal — no anomalies detected") received a NEGATIVE sentiment score of 0.988. This is because the model was fine-tuned on movie review sentiment (SST-2 dataset), not security logs. Technical/neutral language in security contexts does not map well onto positive/negative sentiment. This tells us that off-the-shelf sentiment models trained on consumer text are not reliable for cybersecurity classification without domain-specific fine-tuning.

**Zero-Shot label mismatch:** The candidate labels used ("fraudulent activity," "routine transaction," "requires review") were inherited from a fraud investigation domain rather than cybersecurity. This caused record 5 to be labeled "requires review" when it was clearly benign. In production, Zero-Shot labels must be carefully designed for the specific domain to avoid misleading classifications.

---

## Reflection

The most surprising finding was how uniformly the Sentiment model failed — assigning NEGATIVE to every single record including the most benign one. It was easy to assume that "negative sentiment" would correlate with "security threat," but the model simply doesn't understand security language. It treats words like "failed," "unauthorized," and "detected" as negative in the movie-review sense, but also treats neutral technical language as negative. Groq's LLM approach was far superior because it understood context and domain meaning, not just surface-level word sentiment. This made it clear that for any serious NLP task in a specialized domain like cybersecurity, general-purpose models need either fine-tuning or replacement with instruction-following LLMs that can be prompted with domain context.

---

## Next Steps

- Test with more records (20–30) to get statistically meaningful accuracy comparisons across models
- Try domain-specific Zero-Shot candidate labels ("critical threat," "routine activity," "possible anomaly") to see if classification improves
- Test a 5th model — unitary/toxic-bert — to see if toxicity scores correlate with alert severity
- Investigate fine-tuned cybersecurity classification models on Hugging Face (e.g., models trained on CVE or SIEM data)
- Add a preprocessing Code node in n8n to strip line breaks and special characters from input_text before it reaches any HTTP Request node, preventing the JSON formatting errors encountered throughout this lab
