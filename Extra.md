# Data Infrastructure and Supporting Tools

Supporting infrastructure becomes useful when a specific bottleneck demands it. A learner does not need to install a message broker, cluster, orchestration service, and feature store before building a first model.

## Event streaming

Apache Kafka stores and transports streams of events. Consider it when several consumers need durable event access and replay. Define schemas, event identifiers, timestamps, consumer behavior, and retention before connecting it to training or inference.

Kafka 4.0 removed ZooKeeper mode and runs with KRaft. The original two-command ZooKeeper/server startup sequence should not be used as a modern setup recipe. Follow the quickstart for the exact Kafka release and test migrations on a separate cluster.[^40]

A business event can be delivered more than once at application boundaries. Use idempotent processing and understand offset and transaction behavior. Do not equate a broker configuration with end-to-end “exactly once” business effects.

## Distributed analytics

Spark supports distributed data processing, SQL/DataFrames, and Structured Streaming. Check the selected release's Java, Python, and deployment requirements together.[^41] Dask may fit Python-native partitioned computations; Polars and DuckDB may be sufficient for local analytics. Compare operational complexity and real workload measurements before adopting a cluster.

For a learning exercise, prefer explicit schemas over silent type inference on production-like files. Keep transformation logic, source versions, and output validation visible. Joins and shuffles often determine performance more than nominal cluster size.

## Orchestration and storage

| Need | Architecture to consider | Evidence before adoption |
|---|---|---|
| Repeat scheduled steps | Workflow scheduler | Dependencies, retries, backfills, ownership |
| Share training/inference features | Feature-management system | Freshness and offline/online consistency |
| Preserve model artifacts | Object storage plus registry | Immutability, access, retention |
| Search application knowledge | Database/search index | Recall, filters, deletion, cost |
| Trace a request | Application telemetry | Privacy-safe trace IDs and actionable signals |

These are architecture categories, not endorsements of a particular commercial product. Start with the simplest setup that satisfies the requirements.

## Visualization

Use Matplotlib for controlled static plots and Seaborn for statistical exploration. Consider an interactive plotting or BI tool when readers need filtering and drill-down. Always label units, denominators, sample sizes, and uncertainty. For model comparison, show subgroup results and cost alongside the headline score.

A polished chart cannot compensate for invalid splits. Store the underlying aggregate data and transformation so the figure can be reproduced.

## A scale-up rule

First reduce unnecessary work: select columns, filter early, avoid repeated scans, and use appropriate file formats. Next measure memory and runtime locally. Add parallel or distributed execution only when the measurement shows a need. Finally, test failure recovery and ongoing operating cost.

**Exercise:** Write a decision table comparing a local file-processing job, Dask, and Spark for your workload. Include data size, update frequency, team experience, and recovery requirements. Explain why the smallest viable option is sufficient or insufficient.

[Back to AI Essentials Hub](README.md)

## Sources

[^40]: Apache Software Foundation. [Apache Kafka 4.0.0 Release Announcement](https://kafka.apache.org/blog/2025/03/18/apache-kafka-4.0.0-release-announcement/). 2025-03-18. Reviewed 2026-09-12–2026-09-13.

[^41]: Apache Software Foundation. [Spark Overview](https://spark.apache.org/docs/latest/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
