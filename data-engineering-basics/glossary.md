# 📖 Data Engineering Glossary

## 🏗️ Core Infrastructure & Architecture

* **Data Engineering:** Building systems to collect, transform, and deliver data reliably.
* **ETL (Extract, Transform, Load):** Moving data from sources into a centralized destination system.
* **ELT (Extract, Load, Transform):** Loading raw data first, then transforming using target compute.
* **Data Pipeline:** Automated workflows that move data through various processing stages.
* **Data Lake:** Scalable repository storing raw data in its native format.
* **Data Warehouse:** Centralized, structured database optimized specifically for analytical business intelligence.
* **Data Lakehouse:** Architecture enabling warehouse-like features directly on low-cost object storage.
* **Lambda Architecture:** Balances low-latency views with comprehensive, accurate batch-processed results.
* **Kappa Architecture:** Simplifies design by treating all data as a continuous stream.
* **Medallion Architecture:** Incrementally improving data quality through Bronze, Silver, and Gold layers.

---

## 📊 Modeling & Storage

* **OLTP (Online Transactional Processing):** Databases optimized for high-volume, fast, short-lived atomic transactions.
* **OLAP (Online Analytical Processing):** Systems optimized for complex queries and multi-dimensional data analysis.
* **Star Schema:** Simplifies queries by connecting one fact table to dimensions.
* **Snowflake Schema:** Normalizes dimensions into multiple related tables to reduce redundancy.
* **Fact Table:** Contains quantitative metrics and foreign keys for analytical measurements.
* **Dimension Table:** Provides descriptive context and attributes for the facts recorded.
* **SCD (Slowly Changing Dimension):** Techniques to manage and track attribute changes over time.
* **SCD Type 1:** Overwrites existing data, maintaining no history of previous values.
* **SCD Type 2:** Creates new records for changes, preserving a full history.
* **Parquet:** Efficient columnar storage format designed for deep analytical queries.
* **Avro:** Row-based format featuring rich schema evolution for streaming data.

---

## ⚡ Processing & Compute

* **Batch Processing:** Executing high-volume data jobs at specific, scheduled time intervals.
* **Stream Processing:** Ingesting and analyzing data instantly as individual events occur.
* **Distributed Computing:** Spreading workloads across multiple nodes to increase processing speed.
* **Apache Spark:** High-speed engine for large-scale distributed data and machine learning.
* **RDD (Resilient Distributed Dataset):** Fault-tolerant, immutable collection of objects partitioned across a cluster.
* **Dataframe:** Distributed table with named columns, providing a higher-level Spark API.
* **MapReduce:** Legacy framework for processing massive datasets across distributed hardware clusters.
* **MPP (Massively Parallel Processing):** Coordinated processing of a single program by multiple independent CPUs.
* **Partitioning:** Physically dividing data into smaller chunks to improve query performance.
* **Shuffling:** The expensive process of redistributing data across nodes during joins.

---

## 🛠️ Orchestration & Governance

* **Workflow Orchestration:** Managing task dependencies, timing, and failures within a pipeline.
* **DAG (Directed Acyclic Graph):** Visual representation of tasks where data flows without loops.
* **Apache Airflow:** Open-source platform used to programmatically schedule and monitor workflows.
* **Data Governance:** Internal standards ensuring data is secure, private, and high quality.
* **Data Lineage:** The visual map showing data's journey from source to destination.
* **Data Catalog:** Centralized metadata inventory for discovering and managing data assets.
* **Schema Evolution:** Allowing data structures to change without breaking downstream applications.
* **Idempotency:** Property where running a job multiple times yields identical results.
* **Backfilling:** Re-processing historical data after logic changes or system failures.
* **Data Quality:** Automated checks ensuring data is accurate, timely, and correctly formatted.

---

## ☁️ Cloud & DevOps

* **IaC (Infrastructure as Code):** Managing cloud resources through machine-readable definition files (Terraform).
* **CI/CD (Continuous Integration/Continuous Deployment):** Automating code testing and deployment to production environments.
* **Docker:** Technology used to package code and dependencies into portable containers.
* **Kubernetes (K8s):** Open-source system for automating deployment and scaling of containerized apps.
* **Object Storage:** Flat hierarchy storage for unstructured data blobs like S3.
* **Serverless:** Execution model where cloud providers dynamically manage machine resource allocation.
* **Secret Management:** Securely handling sensitive credentials like passwords and API keys.
* **Observability:** Understanding system state through logs, metrics, and distributed tracing.
