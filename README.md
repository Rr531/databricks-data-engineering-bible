# databricks-data-engineering-bible
Complete Databricks &amp; PySpark interview preparation guide
# 📘 Databricks Data Engineering Bible
### Complete PySpark + Databricks Interview & Learning Guide

This repository is a **complete, end-to-end reference** for learning **Databricks and PySpark** from zero to interview-ready level.

If you understand and can explain everything in this document, you can confidently clear:
- Databricks interviews
- Spark interviews
- Azure Data Engineer interviews
- Senior Data Engineer technical rounds

---

## 🔹 1. What is Databricks?

Databricks is a **unified analytics platform** built on Apache Spark for:
- Big data processing
- Data engineering
- Streaming
- Machine learning
- Lakehouse architecture

### Why Databricks?
- Managed Spark (no cluster headaches)
- Delta Lake for ACID transactions
- Optimized runtime (Photon)
- Integrated MLflow
- Works with Azure, AWS, GCP

**Interview one-liner:**  
> Databricks is a cloud-native Spark platform that simplifies large-scale data engineering using the Lakehouse architecture.

---

## 🔹 2. Spark Fundamentals

Apache Spark is a **distributed, in-memory data processing engine**.

### Core Spark Components
- Driver
- Executor
- Cluster Manager
- DAG Scheduler
- Task Scheduler

### Spark Execution Flow

---

## 🔹 3. Spark Internals (Very Important)

### Driver Responsibilities
- Creates SparkSession
- Builds DAG
- Schedules jobs
- Tracks execution
- Collects results

If the **driver fails**, the application fails.

### Executors
- Execute tasks
- Store cached data
- Perform shuffles

### DAG
- Directed Acyclic Graph
- Divided into stages by shuffle boundaries

### Lineage
- Metadata describing transformations
- Used for fault tolerance
- Spark recomputes lost partitions using lineage

---

## 🔹 4. RDD vs DataFrame vs Dataset

| Feature | RDD | DataFrame | Dataset |
|------|----|-----------|---------|
| Level | Low | High | High |
| Optimization | ❌ | Catalyst | Catalyst |
| Speed | Slow | Fast | Fast |
| Use case | Rare | Preferred | Scala only |

**Golden rule:** Always use DataFrames unless you need low-level control.

---

## 🔹 5. Lazy Evaluation

Spark does **not execute transformations immediately**.

### Transformations
- map
- filter
- select
- groupBy

### Actions
- count
- collect
- show
- write

Execution starts **only when an action is triggered**.

---

## 🔹 6. Transformations: Narrow vs Wide

| Narrow | Wide |
|------|------|
| No shuffle | Shuffle |
| Faster | Slower |
| map, filter | join, groupBy |

---

## 🔹 7. map vs flatMap

- `map` → 1 input → 1 output
- `flatMap` → 1 input → many outputs (flattened)

---

## 🔹 8. reduceByKey vs groupByKey (CRITICAL)

| reduceByKey | groupByKey |
|-----------|------------|
| Aggregates before shuffle | Shuffles full data |
| Efficient | Expensive |
| Preferred | Avoid |

**Rule:** Always prefer `reduceByKey`.

---

## 🔹 9. Spark Joins

### Join Types
- inner
- left
- right
- full
- left semi
- left anti

### Semi vs Anti Join
- Semi → matching rows from left
- Anti → non-matching rows from left

---

## 🔹 10. Broadcast Join

Used when one table is small.

```python
from pyspark.sql.functions import broadcast
df_large.join(broadcast(df_small), "id")
🔹 11. Data Skew
What is Data Skew?

Uneven data distribution causing slow tasks.

How to Handle Skew

Broadcast joins

Salting keys

Repartition

Adaptive Query Execution (AQE)
12. Cache vs Persist vs Delta Cache
Cache	Persist	Delta Cache
Memory + disk	Custom level	SSD (Databricks)
Use caching only for reused datasets.
🔹 13. Accumulators & Broadcast Variables
Accumulator → write-only shared variable
Broadcast → read-only shared variable
🔹 14. Catalyst Optimizer
Catalyst converts:
Logical plan → Optimized logical plan → Physical plan
This is why DataFrames are faster than RDDs.
🔹 15. Checkpointing
Checkpointing:
Breaks long lineage
Improves fault tolerance
Used heavily in streaming
16. Delta Lake (Deep Dive)
Parquet vs Delta
Parquet	Delta
Immutable	ACID
No updates	UPDATE / DELETE
No history	Time Travel
Delta Features
ACID transactions
Schema enforcement
Schema evolution
Time travel
MERGE INTO
OPTIMIZE
VACUUM
🔹 17. Delta Lake ACID & Concurrency
Transaction log (_delta_log)
Optimistic concurrency control
Snapshot isolation for readers
18. MERGE INTO (Upserts)
MERGE INTO target t
USING source s
ON t.id = s.id
WHEN MATCHED THEN UPDATE
WHEN NOT MATCHED THEN INSERT
Used for:
Incremental loads
CDC
SCD Type 1 & 2
🔹 19. OPTIMIZE, ZORDER, VACUUM
OPTIMIZE table ZORDER BY (customer_id)
VACUUM table RETAIN 168 HOURS
OPTIMIZE → small file compaction
ZORDER → faster filtering
VACUUM → cleanup old files
🔹 20. Autoloader & Streaming
Autoloader uses Structured Streaming for scalable file ingestion.
spark.readStream.format("cloudFiles")
Supports:
Schema evolution
Checkpointing
Fault tolerance
🔹 21. Lakehouse Architecture
Lakehouse combines:
Data Lake (cheap storage)
Data Warehouse (ACID + performance)
Medallion Architecture
Bronze → Silver → Gold
Layer	Purpose
Bronze	Raw data
Silver	Cleaned data
Gold	Aggregated business data
🔹 22. ADF + Databricks Integration
Typical Pipeline
ADF → ADLS → Databricks → Delta → BI / ML
ADF Responsibilities
Orchestration
Scheduling
Monitoring
Databricks Responsibilities
Transformation
Optimization
Analytics
🔹 23. Performance Optimization Checklist
Use DataFrames
Avoid groupByKey
Broadcast small tables
Handle skew
Use Delta Lake
OPTIMIZE + ZORDER
Avoid UDFs
Monitor Spark UI
🔹 24. Cluster Deployment Modes
Client Mode	Cluster Mode
Driver local	Driver on cluster
Dev	Production
🔹 25. Interview Golden Answer
“I use Databricks with a Lakehouse architecture. Data is ingested via ADF into ADLS, processed using Spark and Delta Lake through Bronze–Silver–Gold layers, optimized using broadcast joins, AQE, ZORDER, and served to BI and ML workloads.”
