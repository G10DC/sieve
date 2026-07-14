---
name: sieve
description: Build data pipelines (ingest → validate → transform → persist) that are idempotent, schema-validated, and fail-as-value, with provenance and raw/curated separation. Use for any ETL, scraping-to-storage, or data-transform flow where fragility, silent failures, or duplicate re-runs are a risk. Never persist unvalidated data; never mutate raw inputs.
---

# sieve

Pipelines that don't break silently and don't duplicate on re-run. Five rules, in order. One above all:
**a pipeline you can't safely re-run is a bug.**

## Golden rules
1. **Validate before write.** Schema-check every record before persisting. Unvalidated data never reaches curated storage.
2. **Idempotent by design.** Same input + same run = same output, no duplicates. Key on a stable identifier; upsert, never blind-append.
3. **Fail-as-value.** A step that fails saves partials, logs the failure, and continues. The pipeline never crashes silently or loses what it already produced.
4. **Provenance always.** Every persisted dataset records source (URI/file), fetch timestamp, and transform hash. No orphan data.
5. **Raw is immutable.** Raw inputs are write-once, read-only. All transformation writes to a separate curated layer.
6. **Data quality, not just schema.** Validate completeness, uniqueness, and ranges too — not only structure. Quarantine bad records to a dead-letter store; never silently drop them.
7. **Fail loud, never silent.** Fail-as-value must be logged and alerted. A pipeline that "succeeds" with wrong or missing output is worse than a crash.
8. **Retry transients, halt permanents.** Backoff-retry flaky upstream (network/API); treat schema and logic errors as permanent — don't retry a broken transform into oblivion.

## Layout (enforced)
`raw/` (immutable ingested) → `staged/` (validated, typed) → `curated/` (transformed, query-ready). Never collapse the layers "for speed."

## When to use
- ETL / ELT, scraping → storage, batch transforms, anything that re-runs on a schedule.
- When a bad run must be safely re-runnable without manual cleanup.

## When NOT to use
- One-off ad-hoc analysis with no persistence (just use a notebook).
