# 🗄️ Storage Formats

## Parquet / Delta Lake / Apache Iceberg

---

## 1️⃣ Parquet

### What it is

Columnar file format optimized for analytics.

---

### How Parquet Stores Data

```
File
 ├─ Row Group
 │   ├─ Column A chunk
 │   ├─ Column B chunk
 │   └─ Column C chunk
```

Each column stored separately.

---

### Why Columnar Matters

* Read only needed columns
* Better compression
* Faster scans

Example:

```sql
SELECT user_id FROM events;
```

Reads only `user_id` column.

---

### Strengths

* Open standard
* Small file sizes
* High scan performance
* Supported everywhere

---

### Limitations

* No transactions
* No updates/deletes
* No versioning
* No concurrency control

Parquet alone = **files, not tables**

---

### When to Use Parquet Alone

* Append-only logs
* Raw ingestion layer
* Simple data lakes

---

---

## 2️⃣ Delta Lake

### What it is

Parquet + transaction log (`_delta_log`)

---

### Extra Layer

```
Table Folder
 ├─ part-0001.parquet
 ├─ part-0002.parquet
 └─ _delta_log/
      ├─ 00001.json
      ├─ 00002.json
```

Log tracks every change.

---

### Features

* ACID transactions
* MERGE / UPDATE / DELETE
* Time travel
* Schema enforcement
* Schema evolution
* Concurrency-safe writes

---

### Example

```sql
MERGE INTO users u
USING updates s
ON u.id = s.id
WHEN MATCHED THEN UPDATE
WHEN NOT MATCHED THEN INSERT;
```

---

### Why Delta Exists

Solve problems of:

* Concurrent writers
* Incremental pipelines
* CDC ingestion

---

### Typical Use Cases

* Silver / Gold layers
* Incremental models
* ML feature stores

---

---

## 3️⃣ Apache Iceberg

### What it is

Table format with snapshot-based metadata.

---

### Structure

```
Table
 ├─ Metadata files
 ├─ Manifest files
 └─ Data files (Parquet)
```

No centralized transaction log file.

---

### Features

* ACID transactions
* Time travel
* Schema evolution
* Hidden partitioning
* Row-level deletes
* Engine-agnostic

---

### Hidden Partitioning

You define:

```
PARTITION BY day(timestamp)
```

Engine handles physical layout.

No folder logic needed.

---

### Why Iceberg Exists

* Avoid vendor lock-in
* Work across Spark, Trino, Flink, Athena

---

---

## ⚔️ Delta vs Iceberg

| Feature             | Delta Lake | Iceberg |
| ------------------- | ---------- | ------- |
| ACID                | Yes        | Yes     |
| Time travel         | Yes        | Yes     |
| Schema evolution    | Yes        | Yes     |
| MERGE               | Yes        | Yes     |
| Engine agnostic     | Partial    | Strong  |
| Vendor neutrality   | Medium     | High    |
| Hidden partitioning | Partial    | Strong  |

---

## 🧠 Mental Model

Parquet = bricks
Delta/Iceberg = building rules

---

## 🏗️ Lakehouse Architecture

```
Raw (Parquet)
   ↓
Bronze (Delta/Iceberg)
   ↓
Silver (Delta/Iceberg)
   ↓
Gold (Delta/Iceberg)
```

---

## 🔥 Interview One-Liners

**Parquet**

> Columnar file format for analytics.

**Delta**

> Parquet with transaction log providing ACID and upserts.

**Iceberg**

> Table format with snapshot-based metadata and engine independence.

---

## 🎯 When to Choose What

**Use Parquet**

* Raw ingestion
* Append-only

**Use Delta**

* Spark-heavy stack
* CDC pipelines

**Use Iceberg**

* Multi-engine environment
* Vendor-neutral lakehouse

---

---


Below is a **clean glossary of common acronyms** related to **storage formats, lakehouse, and table formats**, with short, interview-friendly definitions.

---

# 📘 Storage Formats & Lakehouse — Acronym Glossary

---

### ACID

**Atomicity, Consistency, Isolation, Durability**
Guarantees reliable database-style transactions.

---

### CDC

**Change Data Capture**
Tracks inserts, updates, deletes from source systems.

---

### CTAS

**Create Table As Select**
Creates table from query result.

---

### DDL

**Data Definition Language**
SQL for creating/modifying tables.

---

### DML

**Data Manipulation Language**
SQL for inserting/updating/deleting data.

---

### ETL

**Extract, Transform, Load**
Transform before loading.

---

### ELT

**Extract, Load, Transform**
Transform after loading.

---

### IAM

**Identity and Access Management**
Controls permissions.

---

### KMS

**Key Management Service**
Manages encryption keys.

---

### OLAP

**Online Analytical Processing**
Analytics workloads.

---

### OLTP

**Online Transaction Processing**
Transactional workloads.

---

### SCD

**Slowly Changing Dimension**
Managing dimension changes.

---

### SLA

**Service Level Agreement**
Expected availability or freshness.

---

### SLO

**Service Level Objective**
Target reliability level.

---

### SSE

**Server-Side Encryption**
Encryption handled by storage service.

---

### TLS

**Transport Layer Security**
Encryption in transit.

---

### MVCC

**Multi-Version Concurrency Control**
Multiple snapshots for concurrency.

---

### RBAC

**Role-Based Access Control**
Permissions via roles.

---

### PII

**Personally Identifiable Information**
Sensitive personal data.

---

### WAL

**Write-Ahead Log**
Log written before data files.

---

### ZORDER

**Z-Order Clustering**
Multi-dimensional data layout optimization.

---

### GC

**Garbage Collection**
Removing unused data files.

---

---

# 📗 Delta / Iceberg Specific

---

### DML

**Data Manipulation Language**
UPDATE, DELETE, MERGE.

---

### DDL

**Data Definition Language**
CREATE TABLE, ALTER TABLE.

---

### OPTIMIZE

File compaction command.

---

### VACUUM

Remove obsolete files.

---

### MANIFEST

Metadata file listing data files.

---

### SNAPSHOT

Point-in-time view of table.

---

### METADATA FILE

File describing table structure.

---

---

# 📙 Storage / File Format

---

### ORC

**Optimized Row Columnar**
Columnar file format.

---

### AVRO

Row-based serialization format.

---

### HDFS

**Hadoop Distributed File System**
Distributed storage.

---

### S3

**Simple Storage Service**
AWS object storage.

---

### ADLS

**Azure Data Lake Storage**

---

### GCS

**Google Cloud Storage**

---

---

# 🧠 Mental Summary

> Parquet stores data efficiently. Delta and Iceberg add ACID, snapshots, and table semantics using logs or metadata.

