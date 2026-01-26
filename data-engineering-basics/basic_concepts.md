# 📦 Pipelines & Reliability

## What is idempotency?

A process is idempotent if running it multiple times produces the **same final state**.

**Why it matters**

* Pipelines often retry.
* Jobs may be re-run manually.
* Prevents duplicated or corrupted data.

**Common techniques**

* Use primary keys / unique constraints
* MERGE / UPSERT instead of INSERT
* Deduplicate on write
* Overwrite partitions
* Deterministic file paths

**Example**

```sql
MERGE INTO users u
USING staging_users s
ON u.user_id = s.user_id
WHEN MATCHED THEN UPDATE
WHEN NOT MATCHED THEN INSERT;
```

---

## What is orchestration?

Controlling **when**, **how**, and **in what order** tasks run.

**Responsibilities**

* Scheduling
* Dependency management
* Retries
* Alerting
* Backfills

**Popular tools**

* Airflow
* Prefect
* Dagster
* AWS Step Functions

**Key idea**
Orchestration coordinates jobs — it does not perform heavy computation.

---

## What is a DAG?

Directed Acyclic Graph.

**Meaning**

* Directed → tasks have direction
* Acyclic → no circular dependencies
* Graph → nodes + edges

**Node**
Task

**Edge**
Dependency

**Example**

```
Extract → Clean → Transform → Load → Validate
```

---

## What is retry with backoff?

Retrying a failed operation with increasing wait time.

**Why**

* Temporary network issues
* API throttling
* Service hiccups

**Typical pattern**

```
1s → 5s → 30s → 2m → fail
```

**Often combined with**

* Max retry count
* Jitter (random delay)

---

## What is a Dead-Letter Queue (DLQ)?

Storage for records that repeatedly fail processing.

**Why**

* Prevents blocking pipeline
* Allows later investigation

**Common pattern**

```
Main Queue → Consumer → Fail → DLQ
```

**DLQ contains**

* Raw record
* Error message
* Timestamp

---

# ⚡ Performance

## What is partition pruning?

Query engine reads **only relevant partitions**.

**Example**

```
s3://events/date=2025-01-20/
s3://events/date=2025-01-21/
```

Query:

```sql
SELECT * FROM events WHERE date='2025-01-21';
```

Only one folder scanned.

**Benefit**

* Massive cost and time reduction.

---

## What is predicate pushdown?

Filters applied **inside storage engine**, not after loading.

**Bad**

```
Read all columns and rows
Then filter
```

**Good**

```
Filter while reading
```

**Works well with**

* Parquet
* ORC

---

## What is caching?

Keeping frequently accessed data in fast memory.

**Types**

* Query result cache
* In-memory dataframe cache
* Materialized views

**Tradeoff**

* Faster reads
* Uses memory

---

## What is shuffle?

Moving data across workers for grouping or joining.

**Triggered by**

* GROUP BY
* JOIN
* ORDER BY

**Why expensive**

* Network I/O
* Disk spill

**Optimization**

* Pre-partitioning
* Reduce data early
* Broadcast small tables

---

# ✅ Data Quality

## What is data quality?

Fitness of data for its intended use.

**Dimensions**

* Accuracy
* Completeness
* Consistency
* Timeliness
* Validity

---

## Common Data Quality Checks

* Null checks
* Duplicate checks
* Range checks
* Schema validation
* Referential integrity
* Freshness checks

---

## How do you detect duplicates?

```sql
SELECT id, COUNT(*)
FROM table
GROUP BY id
HAVING COUNT(*) > 1;
```

**Or**

* Hash rows
* Compare checksums

---

## How do you detect missing data?

**Null check**

```sql
SELECT COUNT(*) FROM table WHERE col IS NULL;
```

**Freshness**

```sql
MAX(event_time)
```

**Row count comparison**

* Today vs yesterday
* Source vs target

---

# 🧠 Mental Model (Memorize)

> Pipelines must be idempotent, orchestrated by DAGs, resilient through retries and DLQs, fast via pruning, pushdown and caching, and trustworthy through systematic data quality checks.


