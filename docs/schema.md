# Data Schema (Standardized)

## Bucket Layout

```md
manifest.json
company_registry.json
ministry_lookup.csv
project_type_map.csv
daily/YYYY-MM-DD/
  tender_submissions.csv
  company_profiles.json
  payment_records.csv
  rag_docs.jsonl
  events.ndjson   # optional bonus source
  rag_brief.md    # optional context source
```

## Core Files

### `tender_submissions.csv`

- `submission_id`
- `submission_date`
- `company_ref` (may be missing)
- `ministry` (code/name inconsistent)
- `state` (code/name inconsistent)
- `project_type` (free text)
- `proposed_budget` (mixed formats)
- `proposed_duration_days` (may be unrealistic)
- `summary` (optional text)

### `company_profiles.json`

- `company_id`
- `company_name` (may drift)
- `registration_id` (stable company identity)
- `years_in_operation`
- `cash_reserve` (mixed formats)
- `has_iso_cert` (boolean-ish)
- `snapshot_date` (may be missing)

### `payment_records.csv`

- `payment_id`
- `company_ref`
- `payment_date` (can be late/out-of-order)
- `amount` (mixed formats)
- `payment_status`

### `events.ndjson`

Optional bonus replay source.

One JSON event per line.

Fields:

- `event_id`
- `event_type`
- `event_time`
- `entity_id`
- `source_date`
- `schema_version`
- `payload`

Delivery behavior:

- At-least-once (duplicate `event_id` possible)
- Out-of-order possible
- May be missing for some dates
- Some events may represent state transitions without a matching batch record

### `rag_docs.jsonl`

Retrieval corpus (one JSON document per line) for backend AI workflows.

Typical fields:

- `doc_id`
- `doc_type` (e.g., `submission_brief`, `submission_extract`, `company_brief`, `daily_brief`)
- `entity_id`
- `date_ref`
- `source_files`
- `source_ids`
- `text`
- `generated_by` (e.g., `template`, `template_variant`, `raw_extract`, `gemini`)
- `generated_at`

### `rag_brief.md`

Optional context source.

Daily narrative text derived from the same date batch, usable as additional retrieval context.

## Data Quality Conditions (Expected)

- Missing files
- Schema drift (column add/remove/rename)
- Dirty numeric/text values
- Duplicate records
- Late/out-of-order records
- Unsorted `manifest.json` date order
- Malformed rows (rare)
- Variable quality/missing retrieval artifacts
- Mixed retrieval doc styles (narrative + extract-like)
- Sparse metadata in some retrieval docs (e.g., fewer `source_ids`)

Your system is expected to continue processing with logging/flags where needed.
