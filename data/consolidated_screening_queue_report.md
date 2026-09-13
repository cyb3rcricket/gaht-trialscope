# Consolidated Human Screening Queue

This queue is generated deterministically from:

- `data/ai_triage.csv` — Gemini 3.7 Flash first-pass triage
- `data/sol_adversarial_audit.csv` — GPT-5.6 Sol independent adversarial audit
- `data/raw/enrichment/enriched_studies.json` — preserved ClinicalTrials.gov registry evidence
- `data/human_screening_decisions.csv` — explicit final human adjudications

The generator does **not** make new scientific screening decisions. It only reconciles existing evidence and decisions.

## Current screening state

- Candidate universe: **351**
- Final human adjudications: **123 excludes**
- Active proposed includes requiring human verification: **0**
  - Sol-promoted from Flash `human_review`: **0**
  - Flash/Sol agreement likely-includes: **0**
- Proposed excludes reserved for exclusion QC: **103**

## Include verification order

Review `data/likely_include_review_queue.csv` from top to bottom. The 17 Sol-promoted records come first because Flash originally expressed uncertainty; the 111 dual-model agreements follow.

For each study, confirm both protocol gates:

1. A transgender/gender-diverse population is explicitly part of the study population.
2. GAHT has an explicit research role as an intervention, exposure, comparison, monitoring target, pharmacologic variable, or subject of analysis.

If both are clearly met, record the human decision as `include`. If either fails, record `exclude`. If the preserved registry evidence is insufficient, set `needs_deeper_review` rather than forcing a decision.

## Batches



After all 128 proposed includes are human-verified, perform stratified QC on the 210 proposed excludes before locking the final included-study set.
