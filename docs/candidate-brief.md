# Data Engineer Assessment - Standardized Brief

## Objective

Build a local, re-runnable data system over daily synthetic procurement data that includes:

1. A deterministic ETL output (scoring/ranking)
2. A backend AI explanation output (grounded, cited)

Both are required.

## Data Policy

All data is synthetic and contains no real confidential or personal information.

## What You Must Deliver

### A) Deterministic Data Pipeline (Required)

Your pipeline must:

- Read `manifest.json` dynamically
- Process only new dates
- Handle dirty/missing data without crashing
- Produce deterministic risk metrics/rank output
- Be idempotent on re-run

### B) AI Explanation Engine (Required, Backend Only)

Build a backend explanation workflow (LangChain recommended, equivalents allowed):

- Generate explanations for the top 20 highest-risk submissions
- Ground explanations using retrievable evidence
- Return citations for each explanation
- Return `insufficient_evidence` when context is weak

> You can use any LLM provider but Gemini is recommended as their free tier is sufficient for testing.
> If you for some reason have hit limits please contact us to discuss.

Rules:

- No UI required
- AI must not compute or overwrite deterministic metrics/scores/ranks

## Data Layout

public s3 bucket link (read-only): `s3://procurement-synth-assessment-data`
Data is generated daily and partitioned by date. Your pipeline should process new data as it arrives without re-processing old data.
The main files you'll work with include:

```md
s3://procurement-synth-assessment-data/
  manifest.json
  company_registry.json
  ministry_lookup.csv
  project_type_map.csv
  daily/YYYY-MM-DD/
    tender_submissions.csv
    company_profiles.json
    payment_records.csv
    rag_docs.jsonl
    events.ndjson        # optional bonus source
    rag_brief.md         # optional context source
```

## Required Outputs

### 1) Deterministic Evaluation Output

Must include these concepts (column names flexible):

- `submission_id`
- `evaluation_timestamp` (or date)
- deterministic derived metrics/scores
- risk indicator
- ranked order

### 2) AI Explanation Output

Must include:

- `submission_id`
- deterministic `risk_score` (copied from pipeline output)
- `explanation`
- `citations` (e.g., `doc_id`, `submission_id`)
- `status` (`ok` or `insufficient_evidence`)
- `generated_at`

## Execution

Provide one reproducible local run path (examples):

- `python run_pipeline.py` (runs ETL + AI output)
- `docker compose up`

## Submission Requirements

Include a `README.md` that explains:

- How to run
- Data assumptions and key rules
- State/idempotency strategy
- AI retrieval/explanation strategy and guardrails
- Known limitations

For AI validation, answer the query set in:

- `docs/ai_test_queries.json`

## Optional Bonus

- Process `events.ndjson` as an additional event replay input
- Use `rag_brief.md` as additional retrieval context

## Scope Notes

This assessment is backend-focused. Clarity, correctness, and reproducibility matter more than framework complexity.
