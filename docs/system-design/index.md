---
title: System Design Questions
description: Core system design questions for data engineers covering real-time analytics, lakehouse architecture, and stream vs batch processing.
keywords: system design, data engineering, real-time analytics, lakehouse, stream processing, batch processing, architecture
---

## 1. How do you design a scalable real-time analytics platform?

A real-time analytics platform typically includes:
- **Data Ingestion**: Kafka / Kinesis
- **Stream Processing**: Apache Flink / Spark Streaming
- **Storage**: Hudi / Iceberg / Druid / ClickHouse
- **Serving Layer**: Presto / Athena / API with caching
- Ensure **horizontal scalability**, **exactly-once semantics**, and **low latency**.

---

## 2. What are the core tradeoffs in batch vs stream processing?

| Aspect              | Batch Processing            | Stream Processing             |
|---------------------|-----------------------------|-------------------------------|
| Latency             | High (minutes to hours)     | Low (seconds to milliseconds) |
| Data Freshness      | Delayed insights             | Real-time insights             |
| Tools               | Spark, dbt, Airflow          | Flink, Kafka Streams, Beam     |
| Use Cases           | Reports, ETL                 | Monitoring, anomaly detection  |

---

## 3. How do you design a data lakehouse?

Key components:
- **Storage**: S3/GCS + table formats like Apache Iceberg or Delta Lake
- **Metadata Layer**: Catalog (Glue, Hive, Unity Catalog)
- **Processing Engine**: Spark / Dask / Trino
- **Features**:
  - ACID transactions
  - Schema evolution
  - Time travel
- Ensure separation of storage and compute, and support for both BI + ML workloads.