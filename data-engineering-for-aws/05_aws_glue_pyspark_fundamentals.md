# AWS Glue + PySpark Fundamentals (for Ingestion Jobs)

## What you should be able to do

- Build repeatable Glue ETL jobs in PySpark for ingesting tables to Bronze.
- Parameterize jobs so one template can handle many sources.
- Handle schema drift, nulls, data types, and partitioned writes.

## Glue building blocks (conceptual)

- **Job**: Spark application definition (script + configuration).
- **Triggers/Schedules**: when a job runs (time/event).
- **Connections**: network access and credentials for sources.
- **Catalog**: metadata for tables (optional but common).
- **IAM roles/policies**: access to S3, logs, secrets, source connectivity.

## PySpark ingestion patterns

### Read

Common considerations:
- Predicate pushdown when possible.
- Parallelism at the source (partitioning reads for large tables).
- Consistent snapshot reads (if DB supports it) to avoid partial extraction.

### Transform (minimal for Bronze)

Typical transformations:
- Column name normalization (avoid spaces/special chars).
- Type coercion (string → timestamp/decimal) with explicit error rules.
- Add standard metadata columns (`ingest_ts`, `batch_id`, etc.).

### Write (partitioned, rerun-safe)

Recommended:
- Partition by time or other natural slicing dimension.
- Use “replace partition” semantics for reruns of the same slice.
- Avoid small files (coalesce/repartition appropriately).

## Handling schema changes

Decide per dataset:
- **Fail fast** on unexpected schema changes (safer, more controlled).
- **Evolve schema** with explicit review gates (faster, riskier).

Rules to document:
- New columns allowed? Renames allowed? Type widening allowed?
- What is the alerting path when schema changes?

## Parameterization (job template approach)

A template job should accept:
- Source config (system, table, query/watermark)
- Target config (S3 path, partitions, database/table)
- Load config (type, range, lookback, dedup keys)
- Runtime config (DPU/worker sizing hints, max parallelism)

## Operational basics

- Ensure logs include: dataset id, run id, range, counts, timings.
- Emit simple metrics: rows read/written, duration, bytes, error counts.
- Persist run metadata to support operations and audits.

## Q&A

1. What do you keep in Glue job parameters vs hard-coded in the script?
2. How do you avoid creating millions of small files on S3 from Spark?
3. How do you handle schema drift safely in ingestion jobs?
4. What’s your default pattern for rerun-safe partition writes?
5. How do you debug a Glue job that is slow or intermittently failing?

