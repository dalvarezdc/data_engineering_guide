# Data Engineering for AWS

This folder turns role requirements into practical, study-ready notes: concepts, implementation patterns, checklists, and interview-style Q&A.

## How to use

- If you’re building: start with ingestion + Glue fundamentals.
- If you’re interviewing: skim the Q&A sections and the checklists.
- If you’re operating: focus on monitoring, runbooks, data contracts, and quality.

## Curriculum map (role → guides)

0. Templates & checklists: `00_templates_checklists.md`
1. Lakehouse + Medallion: `01_lakehouse_medallion.md`
2. Bronze ingestion standards: `02_bronze_ingestion_patterns.md`
3. Loading methods (CDC/full/delta/snapshot): `03_cdc_and_incremental_loading.md`
4. Historical loads/backfills: `04_historical_backfills.md`
5. AWS Glue + PySpark fundamentals: `05_aws_glue_pyspark_fundamentals.md`
6. Glue performance optimization: `06_glue_performance_optimization.md`
7. Git + Terraform + CI/CD: `07_ci_cd_git_terraform.md`
8. Data contracts + governance: `08_governance_data_contracts.md`
9. Data quality (Soda-style checks): `09_data_quality_soda_and_checks.md`
10. Monitoring + operability + handover: `10_monitoring_observability_runbooks.md`
11. Delivery model (Agile/Jira/collab): `11_delivery_agile_jira_collaboration.md`
12. Consolidated Q&A bank: `QA_BANK.md`
13. Glossary (acronyms/terms): `GLOSSARY.md`

## What “good” looks like (quick rubric)

- Pipelines are repeatable and templated (same conventions across sources).
- Incremental logic is explicit and testable (no “mystery” where clauses).
- Backfills are safe (idempotent, partition-aware, controlled blast radius).
- Jobs are observable (metrics, logs, alerts, clear runbooks).
- Deployments are automated and versioned (Git + Terraform).
- Contracts and quality are enforced (schemas, SLAs, checks).
