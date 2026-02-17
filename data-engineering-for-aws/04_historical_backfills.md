# Historical Loading, Backfills, and Reprocessing

## Purpose in this role

Ingestion work often includes “historical loading mechanisms to bring legacy data into the Lakehouse” and aggressive migration targets. You need safe backfill patterns that don’t break production.

## Types of historical work

1. **Initial load**: first time ingesting a table (often full/snapshot).
2. **Backfill**: ingest older partitions/time ranges missing from Bronze.
3. **Reprocess**: re-run with new logic/bug fix to correct past data.

## Key principles

- **Idempotent writes**: each backfill run can be repeated without duplicates.
- **Partition-scoped**: operate on bounded partitions/time windows.
- **Observable**: track progress, counts, and failures per chunk.
- **Backpressure**: avoid overwhelming sources; throttle and schedule.

## Recommended backfill strategy (chunked windows)

1. Define a time/ID range slice (e.g., day/week or ID buckets).
2. Extract only that slice.
3. Write to a staging location.
4. Validate (counts, schema, duplicates, null rates).
5. Publish by replacing only the target slice partitions.

## Minimizing blast radius

- Never rewrite “everything” unless required; start with smallest slice.
- Use canary loads (one partition) before scaling out.
- Keep “before/after” snapshots for audit when reprocessing.

## Reconciling with ongoing incremental loads

Common approaches:
- Pause incremental for affected partitions, backfill, then resume from a known watermark.
- Allow both to run but write to distinct partitions and reconcile downstream.
- Use a “repair” job that merges corrected partitions.

## Checklists

- Backfill scope clearly defined (time range, tables, partitions).
- Throttling plan documented (parallelism, scheduling).
- Validation gates + rollback plan defined.
- Watermarks adjusted safely after backfill.

## Q&A

1. How do you backfill 3 years of data without breaking daily loads?
2. What validations do you run per backfill chunk?
3. How do you restart a backfill after partial failure?
4. How do you reconcile backfilled data with incremental CDC/delta logic?
5. What’s your rollback strategy if a backfill publishes bad partitions?
