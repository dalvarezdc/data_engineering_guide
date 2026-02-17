# Lakehouse & Medallion Architecture (Bronze/Silver/Gold)

## Purpose in this role

This role focuses on migrating source systems into a lakehouse “ingestion zone” (often the **Bronze** layer) with scalable ingestion pipelines, consistent patterns, and strong assurance.

## Core concepts

### Lakehouse (practical definition)

A lakehouse combines:
- **Low-cost object storage** (e.g., S3) as the system of record for data files.
- **Table formats / metadata** that make the lake behave like a warehouse (schema, partitions, ACID-ish semantics depending on format).
- **Compute engines** (Spark/Glue, Athena/Trino, etc.) to transform and query.

### Medallion layers

- **Bronze**: raw-ish ingestion, minimal transformation, “capture what happened”.
- **Silver**: cleaned, conformed, deduplicated, business-ready entities.
- **Gold**: curated aggregates / marts / KPI-ready datasets for specific consumers.

## Bronze layer design goals

- **Fidelity**: preserve source meaning (types, keys, timestamps, delete signals).
- **Traceability**: lineage back to source table + extraction time + batch id.
- **Idempotency**: re-running a load yields the same result.
- **Partitionability**: layout supports incremental writes and fast reads.

## Typical Bronze data shape

Recommended columns (add as standard in templates):
- `ingest_ts` (when the pipeline wrote the record)
- `source_system`, `source_schema`, `source_table`
- `load_type` (full/delta/cdc/snapshot)
- `batch_id` / `run_id`
- `record_hash` (optional; for change detection)
- `op` (optional; for CDC: I/U/D) and `op_ts` / `commit_ts`

## What should *not* happen in Bronze

- Heavy business transformations, joins across domains, or “fixing” data by guessing.
- Losing deletes (if the source can delete) without explicitly documenting the rule.
- Re-keying records without keeping original identifiers.

## Checklists

### Architecture checklist

- Bronze storage layout and partition scheme defined per source.
- Clear path for downstream Silver transformations (contracts, schemas).
- Data retention rules per layer (raw retention vs curated retention).

### Ingestion checklist

- Every load is replayable (rerun-safe).
- Every dataset has a “freshness” signal (latest partition / max timestamp).
- Every dataset has a minimal “shape contract” (required columns + types).

## Q&A (interview / review)

1. Why separate Bronze and Silver instead of ingesting directly into curated tables?
2. What metadata do you add in Bronze to make audit and debugging easy?
3. How do you handle source deletes in a Bronze ingestion pattern?
4. What does “idempotent ingestion” mean in practice? Give two strategies.
5. How do you choose partitions for Bronze datasets?
