# Governance & Data Contracts (for Ingestion Pipelines)

## Why this exists

Ingestion should comply with architectural and data assurance standards, including data contracts. Contracts make ingestion predictable: producers know what to send; consumers know what they’ll get.

## What is a data contract (practical)

A data contract is an agreement per dataset describing:
- Schema (columns, types, nullability)
- Keys (primary keys, uniqueness expectations)
- Semantics (what each field means, units, timezone)
- Freshness / delivery expectations (SLA)
- Quality thresholds (e.g., null rate, duplication rate)
- Change management (how schema changes are announced and rolled out)

## Contract enforcement points

Where to enforce:
- In ingestion jobs (Bronze gates: required columns, type coercion rules).
- In quality checks (Silver/Gold thresholds).
- In deployment (schema change requires review/approval).

## Change management

Define which changes are:
- Backward compatible (new nullable column)
- Potentially breaking (rename, type narrowing, key changes)
- Breaking (dropping columns, semantic changes)

Have a process:
- Propose → review → implement → rollout → deprecate old fields.

## Ownership and accountability

For each dataset, capture:
- Producer (source system owner)
- Consumer(s)
- Steward/owner within the platform team
- Support model (who triages incidents)

## Q&A

1. What fields must exist in a data contract for an ingestion dataset?
2. How do you handle a source team adding a column without warning?
3. What’s the difference between schema and semantics in a contract?
4. Where would you enforce contracts: Bronze, Silver, or both?
5. How do you roll out a breaking change without downtime?
