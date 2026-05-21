Checkpoint 2 Capstone Audit Report
Date: 2026-05-19
Project: Financial Fraud Detection System

Audit Questions
1. Does data flow from Ingestion to AI Core without manual intervention?
YES. Component 1 writes records to Airtable with status=unprocessed. Component 2 automatically picks up all unprocessed records via Airtable Search filter. No manual data copying required.
2. Does AI Core produce output that Specialist can act on?
YES. Component 2 writes risk_score, anomaly_flags, and ai_explanation back to each transaction record and updates status to "processed". Component 3 filters on status="processed" and risk_score > 0.7 to find actionable records.
3. Are field names consistent across components?
PARTIALLY. Major mismatch discovered and fixed during Checkpoint 2: Component 3 was filtering for status="analyzed" but Component 2 was writing status="processed". Fixed by updating the filter. Remaining issue: anomaly_flags field format inconsistent (text labels vs semantic tags).
4. Is the schema stable and agreed upon?
PARTIALLY. Core fields (transaction_id, account_id, amount, merchant, location, risk_score, anomaly_flags, ai_explanation, status) are stable. Status value progression needs formal agreement: unprocessed → processed → case_created is functional but not documented.
5. Does the AI component produce meaningful output?
PARTIALLY. Risk scores are meaningful and accurate — wire transfers to high-risk locations correctly score 0.95-1.0, routine transactions score 0.1. Anomaly flags work but output text labels instead of semantic tags. ai_explanation produces relevant text but has reliability issues (~30% returning "Analysis unavailable").
6. Does the Specialist component fire automatically on AI output?
YES, with caveats. Component 3 runs on manual trigger and correctly picks up high-risk records. It does not auto-trigger when Component 2 completes — this is by design (manual trigger for demo). 4 cases successfully created from 35 transactions.
7. Is the dashboard showing real data?
PARTIALLY. Dashboard built and connected to Airtable. Not fully verified with live enriched data post-Checkpoint 2. Needs end-to-end test with analyst workflow.
8. What is the biggest integration risk going forward?
Groq prompt reliability. The ai_explanation and anomaly_flags quality depends entirely on Groq returning well-structured JSON. Parse failures cause silent degradation. Need retry logic and prompt hardening before final demo.
9. What worked better than expected?
Risk scoring accuracy. BART zero-shot classification combined with Groq scoring correctly differentiated high-risk transactions (wire transfers to Nigeria, Lagos, Cyprus) from routine ones (Starbucks, Netflix, Amazon) without hardcoded rules.
10. What is the critical path to final submission?

Fix Groq semantic flag output
Full end-to-end dashboard test
Standardize status field values
Add assigned_to logic
Final demo run with clean 35-record dataset


Status Change from Week 8
ComponentWeek 8 StatusCheckpoint 2 StatusIngestionWorkingWorking (unchanged)Anomaly DetectionBroken (null explanations, flat scores)Working (scores accurate, flags/explanation improving)Case ManagementBuilt, untestedWorking (4 cases created successfully)DashboardBuilt, untestedBuilt, partially testedEnd-to-end flowNot passingPASSING
Critical gaps from Week 8 addressed:

ai_explanation no longer universally null
Risk scores now differentiated (0.1 to 1.0 range)
Amount field no longer being wiped
Case creation working end-to-end
Status field mismatch identified and fixed
