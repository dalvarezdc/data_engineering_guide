# ❓ Practice Questions & Interview Prep

## 🏗️ Architecture & Concepts

1. **ETL vs. ELT:** What is the fundamental difference between these two patterns, and why has ELT become more popular with modern cloud data warehouses?
2. **Data Lakehouse:** How does a Lakehouse architecture combine the benefits of both a Data Lake and a Data Warehouse?
3. **Medallion Architecture:** Describe the purpose and typical data quality of the Bronze, Silver, and Gold layers.
4. **Lambda vs. Kappa:** When would you choose Kappa architecture over Lambda architecture?

## 📊 Modeling & Storage

5. **OLTP vs. OLAP:** If you were designing a system for a retail checkout process, would you use OLTP or OLAP? Why?
6. **Star vs. Snowflake Schema:** Which schema is generally easier for BI tools to query, and why?
7. **SCD Types:** Explain the difference between SCD Type 1 and Type 2. Which one would you use if you needed to track historical changes?
8. **Parquet vs. Avro:** Why is Parquet preferred for analytical queries while Avro is often preferred for streaming ingestion?

## ⚡ Processing & Compute

9. **Batch vs. Stream:** What are the trade-offs between batch and stream processing in terms of latency and cost?
10. **Partitioning & Shuffling:** What is "shuffling" in distributed computing, and how can partitioning help minimize it?
11. **Apache Spark:** What is the difference between an RDD and a Dataframe?

## 🛠️ Orchestration & Quality

12. **Idempotency:** Why is it critical that data pipelines are idempotent? Provide an example of a non-idempotent operation.
13. **DAG (Directed Acyclic Graph):** Why is the "Acyclic" part of a DAG important for workflow orchestration?
14. **Backfilling:** You've updated your transformation logic. How do you ensure historical data reflects these changes?

## ☁️ Cloud & DevOps

15. **IaC (Infrastructure as Code):** Why should data infrastructure be managed via tools like Terraform rather than manual configuration?
16. **Observability:** What are the three pillars of observability (Logs, Metrics, Tracing) and how do they apply to data pipelines?
