# Delivery Model: Agile/Jira and Cross-Team Collaboration

## Purpose in this role

This delivery model supports aggressive migration targets, working with Jira, and partnering with data architects, data assurance, platform engineers, and source system owners.

## Typical squad interactions

- **Data Architect**: ingestion patterns, standards, target model expectations.
- **Data Assurance / Quality**: gates, contract enforcement, exceptions.
- **Platform Engineers**: tooling, environment, CI/CD, observability, access.
- **Source System Owners**: connectivity, extraction constraints, semantics.
- **Operations/Support**: handover, runbooks, on-call readiness.

## How to make migration predictable

- Standardize templates and reduce per-table customization.
- Define “Definition of Done” for each table:
  - ingested to Bronze,
  - monitored,
  - contract documented,
  - runbook created,
  - support handover completed.
- Track progress in Jira by dataset/table with clear acceptance criteria.

## Communication patterns that work

- Weekly source-owner sync for extraction constraints and change management.
- Migration dashboard: tables migrated, remaining, blockers, SLA compliance.
- Clear escalation path for blockers (permissions, performance, data quality).

## Q&A

1. What’s a good Definition of Done for migrating a table to Bronze?
2. How do you handle conflicting priorities between platform and ingestion squads?
3. How do you communicate schema changes and contract updates?
4. What Jira ticket structure works well for migrating many tables quickly?
5. How do you reduce coordination overhead while maintaining governance?
