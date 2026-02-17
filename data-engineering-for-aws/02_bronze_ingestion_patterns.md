# Bronze Ingestion Patterns & Standards (Templates)

## Purpose in this role

This role benefits from “defined standards and templates” for Bronze ingestion Glue jobs. This document defines what a standard ingestion job should do and how to keep many sources consistent.

## The ingestion job contract (what every job must implement)

At minimum, each ingestion job should:
- Read from a defined source (DB, API, file drop) with a documented extraction strategy.
- Write to a standard Bronze table/path with consistent partitioning and metadata.
- Enforce schema handling rules (fail fast vs evolve, nullability).
- Emit operational signals (logs + metrics) and fail loudly on invariants.
- Validate minimal data quality gates (row counts, key null rates, freshness).

## Standard job structure (recommended)

1. **Config load** (source name, table, load type, watermark column, partitions, target path)
2. **Extract** (with snapshot time / consistent read if available)
3. **Normalize** (types, column naming, metadata columns)
4. **Write** (partitioned; atomic publish pattern)
5. **Validate** (counts, schema, watermark monotonicity, duplicates)
6. **Publish** (update catalog/metadata as needed)
7. **Observe** (emit metrics; store run metadata)

## Templates: what to standardize

### Naming conventions

- S3 path conventions (environment / domain / system / table).
- Dataset identifiers used in logging/monitoring.
- Standard metadata columns (see `01_lakehouse_medallion.md`).

### Job parameters

Define a consistent set across all jobs:
- `source_system`, `source_table`
- `load_type` (full/delta/cdc/snapshot)
- `start_ts`, `end_ts` (for backfills or deltas)
- `target_path`, `target_database`, `target_table`
- `partition_cols`
- `fail_on_schema_change` (boolean)
- `max_records_per_run` (optional throttling)

### Error handling & retries

- Treat “no data” as a first-class outcome (not always an error).
- Separate transient source errors from data errors.
- Persist run metadata (success/fail, timings, counts) for operations.

## Common ingestion patterns

### Pattern A: Append-only Bronze

Use when:
- Source is immutable append stream, or you can ingest change events.

Pros:
- Simple, preserves full history.

Cons:
- Downstream must interpret latest state.

### Pattern B: Latest-state Bronze snapshot

Use when:
- Source delivers periodic full snapshots and history isn’t required in Bronze.

Pros:
- Simplifies downstream “current state” queries.

Cons:
- Harder to debug changes without storing diffs or history.

### Pattern C: Merge-to-Bronze (careful)

Use when:
- Bronze is defined as “raw but current state” and you need upserts/deletes.

Pros:
- Downstream reads simpler.

Cons:
- Higher risk; needs table format semantics and careful concurrency handling.

## Checklists

- Source extraction method documented (including locking/consistency assumptions).
- Load method correct for source (CDC/full/delta/snapshot).
- Idempotency strategy documented and tested (rerun behavior).
- Partition scheme supports queries + backfills without rewriting huge volumes.
- Minimum quality gates run every time.

## Q&A

1. What belongs in a “standard Glue ingestion template”?
2. How do you make ingestion jobs idempotent?
3. When is it acceptable to “merge” in Bronze vs append?
4. How would you parameterize one template to support hundreds of tables?
5. What are your default quality gates for Bronze ingestion?
