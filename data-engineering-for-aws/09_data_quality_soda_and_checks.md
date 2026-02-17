# Data Quality in Pipelines (Soda-Style Checks)

## Purpose in this role

Quality checks across jobs (Soda-style checks are one example) help implement **consistent quality gates** that are actionable.

## What is Soda?

**Soda (Soda Core)** is an open-source data quality framework used to define and run checks (schema, freshness, validity, anomaly checks) and produce actionable results.

## Quality dimensions (use as a checklist)

- **Completeness**: required fields not null; expected row counts.
- **Uniqueness**: primary keys unique; no duplicate events.
- **Validity**: types, ranges, allowed values.
- **Consistency**: relationships across fields and tables.
- **Freshness**: data arrives on time; partitions advance.

## Minimum viable quality gates (Bronze)

Per run:
- Row count non-negative and within expected range (optionally via bounds).
- Required columns present.
- Key columns null rate below threshold.
- Watermark monotonicity (incremental loads).
- Duplicate rate for primary keys (if applicable).

Keep Bronze gates light: the job should catch obvious breakages, not perform heavy business validation.

## Quality gates (Silver/Gold)

In curated layers, add:
- Referential integrity checks (where feasible).
- Business rule checks (e.g., amounts >= 0, status in enum).
- Reconciliation checks (totals vs source controls).

## Making checks operationally useful

- Checks must produce:
  - clear failure reasons,
  - sample offending records (small),
  - and an owner/route for resolution.
- Avoid alert fatigue: separate warning vs fail thresholds.

## Q&A

1. What checks do you run in Bronze vs Silver?
2. How do you choose thresholds without constant false positives?
3. What do you do when quality fails: stop the pipeline or quarantine?
4. How do you surface “freshness” failures and who owns them?
5. What’s your strategy for sampling bad records safely?

## Glossary

See `GLOSSARY.md` for acronyms and terms used across this folder.
