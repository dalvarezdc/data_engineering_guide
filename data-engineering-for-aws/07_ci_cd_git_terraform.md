# CI/CD for Glue Ingestion: Git + Terraform

## Goal

Jobs should be version-controlled and deployed via Git and Terraform. This guide outlines the minimum viable CI/CD posture for many ingestion jobs.

## What goes in Git

- Glue job scripts (PySpark).
- Job configuration (per source/table): parameters, schedules, partitions.
- Infrastructure-as-code (Terraform): Glue jobs, IAM, connections, triggers, alarms.
- Documentation: dataset contracts, runbooks, ownership.

## Terraform: what to manage

Typical resources:
- Glue job definitions (script location, role, worker type/count, default args).
- IAM roles and policies for S3/logs/secrets.
- Triggers/schedules.
- Monitoring/alerts (where applicable).

## Environments

At minimum: `dev`, `test` (optional), `prod`.

Principles:
- Same code everywhere; only config changes.
- Promote artifacts/config through environments (no manual prod edits).
- Protect prod with reviews and approvals.

## Release workflow (simple, robust)

1. Feature branch for new/changed jobs.
2. CI checks: linting (if available), unit-ish tests (where possible), packaging.
3. Terraform plan in CI for the target env.
4. Review + approve.
5. Apply to deploy.

## Operational ownership

Define for each pipeline:
- Owner and on-call path.
- SLA/SLO (freshness, availability).
- Runbook link and escalation steps.

## Q&A

1. What belongs in Terraform vs runtime parameters?
2. How do you avoid configuration drift between dev and prod?
3. What does a safe rollout for a new ingestion job look like?
4. How do you handle secrets for source connectivity in CI/CD?
5. What do you do when a Terraform apply fails halfway through?
