# AWS Glue Performance Optimization (DPU, Parallelism, Partitioning)

## Goal

Optimizing Glue jobs (DPU allocation, parallelization, partitioning) is core to reliable ingestion. This guide focuses on the high-impact levers for ingestion workloads.

## What to measure first

Before tuning, capture:
- Total runtime and stage breakdown (read vs transform vs write).
- Input rows/bytes and output rows/bytes.
- File counts and average file size written.
- Shuffle size (if applicable) and skew indicators.

## Common bottlenecks and fixes

### Too many small files (most common)

Symptoms:
- Slow reads downstream, slow writes, high overhead per task.

Fixes:
- Repartition/coalesce before write to target file size.
- Choose partitions wisely (don’t over-partition by high-cardinality keys).
- Batch writes per partition; avoid per-record writes.

### Source extraction is the bottleneck

Symptoms:
- Spark tasks idle; long read time; DB under strain.

Fixes:
- Partitioned reads (split by numeric range or hash).
- Limit concurrency to protect the source; schedule off-peak.
- Pushdown predicates (read only incremental slice).

### Shuffle-heavy transformations

Symptoms:
- Long shuffle stages; spill to disk; skew.

Fixes:
- Keep Bronze transforms minimal.
- Avoid wide joins/aggregations in ingestion jobs.
- If dedup is required, dedup within bounded windows and keys.

## DPU / worker sizing (practical guidance)

- Start with a baseline size that completes reliably.
- Scale up if:
  - tasks are CPU-bound (heavy transforms),
  - or I/O parallelism is limited by worker count.
- Scale down if:
  - job is mostly waiting on a slow source,
  - or you’re over-provisioned for small datasets.

Keep a record of “rows/sec and $/run” to make sizing decisions rational.

## Partitioning strategies

- Partition by a **time dimension** for incremental/backfill friendliness.
- Avoid partitions that create a huge number of tiny partitions.
- Choose partition column(s) that align with common query filters.

## A repeatable tuning workflow

1. Fix correctness first (idempotency + incremental logic).
2. Reduce small files (layout and write behavior).
3. Improve source read parallelism safely.
4. Only then tune DPUs/workers.

## Q&A

1. What are the top three causes of slow Glue ingestion jobs?
2. How do you decide partition columns for a Bronze dataset?
3. What’s the tradeoff between more partitions and file size?
4. How do you tune a job that is source-bound (DB is slow)?
5. What metrics do you capture for ongoing performance regression detection?
