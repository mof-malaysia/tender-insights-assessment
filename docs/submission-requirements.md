# Submission Requirements

## Must Have

- Public repo
- README with instructions and explanations
- One command to run ETL + AI outputs
- No need to commit output files, but must be reproducible
- Re-runnable and idempotent
- Deterministic scores/ranks
- AI uses retrieval and returns citations
- AI never computes/overwrites deterministic metrics

## Deterministic Output (required fields)

- submission_id
- evaluation_timestamp (or date)
- derived metrics/scores
- risk indicator
- rank

## AI Output (required fields)

- submission_id
- risk_score (copied from deterministic output)
- explanation
- citations
- status (ok or insufficient_evidence)
- generated_at

## README Must Include

- How to run
- Data assumptions/key rules
- State/idempotency approach
- AI retrieval/explanation approach
- Known limitations

## Validation

- Answer `docs/ai_test_queries.json`

## Done Checklist

- Fresh clone runs successfully
- Re-run gives stable/idempotent results
- Required fields exist in both outputs
