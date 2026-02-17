# Glossary (Acronyms & Terms)

## Acronyms

- **ACID**: Atomicity, Consistency, Isolation, Durability — database-style transaction properties (some table formats provide ACID-like behavior on object storage).
- **API**: Application Programming Interface — a programmatic way to access a system’s data or functionality.
- **CDC**: Change Data Capture — techniques for capturing inserts/updates/deletes as a change stream.
- **CI**: Continuous Integration — automatically building/checking changes when code is pushed.
- **CI/CD**: Continuous Integration / Continuous Delivery (or Deployment) — automated build/test plus automated release/deploy workflows.
- **DPU**: Data Processing Unit — AWS Glue’s unit of compute capacity used for job sizing/billing.
- **ETL**: Extract, Transform, Load — pipeline pattern for moving and shaping data.
- **IAM**: Identity and Access Management — AWS service for roles/policies and access control.
- **ID**: Identifier — a value used to uniquely identify an entity (e.g., `account_id`).
- **KPI**: Key Performance Indicator — metric used to measure outcomes (often Gold-layer outputs).
- **S3**: Simple Storage Service — AWS object storage commonly used as a lakehouse storage layer.
- **SLA**: Service Level Agreement — promised service target (e.g., “data available within 2 hours”).
- **SLO**: Service Level Objective — measurable target that supports an SLA (e.g., freshness percentile target).
- **YAML**: YAML Ain’t Markup Language — a human-readable configuration file format.

## Terms

- **Backfill**: Loading historical data for past time ranges/partitions that were never ingested or need correction.
- **Bronze (layer)**: The raw-ish ingestion layer that prioritizes faithful capture, traceability, and rerun safety over business transformation.
- **CDC event**: A record in a change stream that represents an insert/update/delete (often includes ordering metadata like a commit timestamp or sequence).
- **Data contract**: An agreement describing a dataset’s schema, semantics, keys, freshness expectations, quality thresholds, and change-management rules.
- **Deduplication (dedup)**: Removing duplicates by defining uniqueness keys and selecting the “winner” record (e.g., latest timestamp/sequence).
- **Delta load (incremental load)**: Loading only changes since a watermark (e.g., `updated_at`) rather than reloading the full dataset.
- **Freshness**: How up-to-date a dataset is (commonly measured as “time since last successful publish” or “max event/partition timestamp”).
- **Full load**: Reloading the entire dataset each run.
- **Gold (layer)**: The curated, consumption-ready layer (aggregates, marts, KPI-ready outputs) optimized for specific consumer use cases.
- **Idempotency**: Property where re-running a job (with the same inputs/scope) produces the same end state without duplicating or corrupting data.
- **Ingestion (pipeline/job)**: The process of extracting data from a source and writing it into the lakehouse with consistent standards, metadata, and monitoring.
- **Ingestion zone**: The storage area where ingested datasets land (often aligned with the Bronze layer).
- **Lakehouse**: An architecture that combines object storage (e.g., S3) with warehouse-like capabilities via metadata/table formats and query engines.
- **Late-arriving data**: Changes/events that arrive after the expected window (e.g., updates with old timestamps), often requiring lookback windows.
- **Layering (Bronze/Silver/Gold)**: Separating concerns by stage: ingest (Bronze), clean/conform (Silver), and consume (Gold).
- **Lookback window**: A deliberate overlap in incremental extraction (re-reading the last N minutes/hours/days) to capture late-arriving changes; usually paired with dedup.
- **Medallion architecture**: A common lakehouse pattern using Bronze/Silver/Gold layers to structure data processing.
- **Monitoring**: Automated signals and alerts for pipeline health (failures, freshness, volume anomalies, latency, contract/quality failures).
- **Observability**: The ability to understand a pipeline’s internal state from external signals (logs, metrics, traces, run metadata).
- **Partitioning**: Organizing data by one or more columns (often time) to improve write/read performance and enable scoped backfills/reruns.
- **Rerun-safe write**: A write strategy that supports reprocessing a bounded scope (e.g., a partition/window) without leaving duplicates (often “replace partition” semantics).
- **Runbook**: Operational documentation describing what a pipeline does, how to rerun/backfill, common failures, and escalation paths.
- **Schema drift**: Changes in source schema over time (new/removed/renamed columns or type changes) that pipelines must handle explicitly.
- **Silver (layer)**: The cleaned, conformed layer where data is deduplicated, standardized, and modeled into reliable entities.
- **Small files problem**: When writes create too many tiny objects (e.g., on S3), harming performance and increasing overhead for downstream reads.
- **Snapshot load**: Loading a consistent point-in-time view (“as of time T”), often written as a partition by snapshot date/time.
- **Upsert**: Updating existing records and inserting new ones based on a key (often used when building “latest state” tables).
- **Watermark**: A cursor (timestamp/sequence) used to track incremental progress and define what “new” data to load next.

### Tools/frameworks

- **Soda / Soda Core**: An open-source, rules-based data quality framework used to define and run checks (e.g., schema, freshness, validity, and anomaly checks) and produce actionable results.
