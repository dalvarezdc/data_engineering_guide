# Q&A Bank 

Use this as a rapid interview prep set or as a review checklist during delivery.

## Lakehouse / Medallion

1. What problems does the lakehouse solve compared to a plain data lake?
2. What are the responsibilities of Bronze vs Silver vs Gold?
3. What metadata do you add in Bronze and why?
4. What does idempotent ingestion mean and how do you implement it?
5. What does “raw but queryable” mean in Bronze?

## Loading methods (CDC/full/delta/snapshot)

1. When would you choose full load even if delta is possible?
2. How do you pick a watermark column and how do you handle late updates?
3. How do you handle deletes if you only have delta loads?
4. What keys/fields do you need from CDC to make it correct?
5. How do you prove you aren’t missing changes in incremental logic?

## Historical loads / backfills

1. How do you backfill years of data safely and cheaply?
2. How do you design backfills to be restartable?
3. How do you minimize impact to source systems during backfills?
4. How do you reconcile backfills with ongoing daily incrementals?
5. What’s your rollback plan after publishing bad historical partitions?

## Glue + PySpark

1. How do you parameterize one job template to ingest many tables?
2. How do you prevent small-file explosion on S3?
3. How do you handle schema drift: fail fast vs evolve?
4. What’s your approach for deduplication in Spark for incremental windows?
5. How do you debug a job that fails intermittently?

## Performance optimization

1. What’s your tuning workflow (measure → change → verify)?
2. How do DPU/worker sizing decisions relate to source vs compute bottlenecks?
3. How do partitions affect write performance and downstream query cost?
4. What are the most common causes of shuffle and how do you avoid them?
5. What metrics do you track to detect performance regressions?

## CI/CD (Git + Terraform)

1. What should be managed as code (Terraform) vs runtime config?
2. How do you structure environments and promotions (dev → prod)?
3. How do you handle secrets for source connectivity?
4. What’s your rollback approach for a bad deployment?
5. How do you prevent manual changes in prod (drift)?

## Governance / contracts / quality

1. What must be in a data contract for ingestion datasets?
2. Where do you enforce contracts (Bronze vs Silver) and why?
3. What are your minimum quality gates for Bronze?
4. How do you design alerts to avoid noise but catch real issues?
5. What happens operationally when a quality gate fails?
