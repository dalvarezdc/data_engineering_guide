# Templates & Checklists (Copy/Paste Starters)

## 1) Dataset onboarding checklist (Bronze)

- Source confirmed: owner, credentials path, connectivity requirements.
- Load method selected: `full` / `snapshot` / `delta` / `cdc` (document why).
- Keys + delete semantics defined.
- Watermark defined (if delta) + lookback window defined.
- Target path/table naming confirmed + partition columns chosen.
- Quality gates selected (Bronze minimums).
- Monitoring signals selected (freshness, volume, duration, failures).
- Runbook drafted + handover plan agreed.
- CI/CD wired (Git + Terraform) and promotion path defined.

## 2) Definition of Done (per table)

A table is “migrated to Bronze” when:
- Data lands in the Bronze target with correct load semantics.
- Reruns are safe (idempotent by partition/window).
- Quality gates are implemented and passing.
- Monitoring/alerts exist for failure + freshness SLA.
- Contract is documented (schema + semantics + change rules).
- Runbook exists and support handover is completed.

## 3) Runbook template (minimal)

### Overview

- Dataset id:
- Purpose:
- Source:
- Target:
- Schedule:
- Owner:
- SLA (freshness):

### Dependencies

- Source connectivity:
- Credentials/secrets:
- Upstream/downstream:

### Parameters

- Load type:
- Watermark / window:
- Partitions:

### How to rerun safely

- Rerun scope:
- “Replace partition” / dedup notes:
- Expected outputs:

### Common failures

- Connectivity:
- Permissions:
- Schema changes:
- Data quality failures:

### Escalation

- Platform:
- Source owner:
- Data assurance:

## 4) Config shape example (YAML-like)

```yaml
dataset_id: corebanking.accounts
source:
  system: corebanking
  table: ACCOUNTS
  extract:
    type: delta   # full|snapshot|delta|cdc
    watermark_column: last_modified_ts
    lookback_minutes: 60
target:
  s3_path: s3://<bucket>/<env>/bronze/corebanking/accounts/
  partitions: [ingest_date]
  format: parquet
quality:
  required_columns: [account_id, last_modified_ts]
  max_null_rate:
    account_id: 0.0
observability:
  freshness_sla_minutes: 120
  volume_anomaly:
    warn_pct_change: 50
```

## 5) Glue job skeleton (pseudocode)

```python
# 1) parse args + load config
# 2) read incremental slice (delta/cdc/full/snapshot)
# 3) normalize schema + add metadata columns
# 4) write partitioned output (rerun-safe per window/partition)
# 5) validate + emit metrics + persist run metadata
```

