# Dask Essentials

Dask schedules Python computations across partitions, cores, or machines. Use it when a workload benefits from this execution model; distribution adds coordination and data-transfer costs.

## Start with the workload

If data fits comfortably on one machine, compare pandas, Polars, or DuckDB first. Dask becomes relevant when partitioned processing, larger-than-memory data, or a cluster is necessary. Partitioning does not guarantee that every operation is memory-safe: joins, shuffles, and collecting a result can still exceed memory.

Dask Array works with chunked numerical arrays; DataFrame works with partitioned tables; Bag supports certain unstructured workflows; delayed tasks represent custom computations. Inspect task count, partition sizes, and skew before scaling workers.

## Choose a scheduler

The original guide's “threads are best for CPU-bound tasks” was too broad. In a conventional GIL-enabled Python runtime, threads help numerical operations that release the GIL; pure Python object processing may benefit from processes. Distributed scheduling can run locally or across machines and provides operational visibility. Benchmark your runtime and workload.[^8]

| Choice | Try it for | Watch for |
|---|---|---|
| Threads | Array operations, native numerical code | GIL-bound Python loops |
| Processes | Python-heavy tasks that can be serialized efficiently | Serialization and memory copies |
| Distributed | Coordinated local or cluster execution | Worker failures, network and storage throughput |

## Small self-contained example

Requires `dask[array]` and NumPy. The result is a scalar; the full array is not collected into the client.

```python
import dask.array as da

values = da.arange(1_000_000, chunks=100_000)
average = values.mean().compute(scheduler="threads")
assert average == 499999.5
print(average)
```

For real tables, start from partitioned Parquet and select only needed columns. A larger-than-memory source can produce a small aggregate; calling `compute()` on the entire table can undo that advantage.

## Operating practices

Make tasks large enough that useful computation dominates scheduling. Avoid millions of tiny tasks, repeated scans, and unnecessary transfers between workers. Use `persist()` only when retaining reusable partitions is worth the memory. Place workers close to the data.

When launching process-based workers from a standalone Python script, put startup logic behind `if __name__ == "__main__":`, particularly on Windows. Review the scheduler documentation for the exact client setup.[^8]

Measure wall time, peak worker memory, spill-to-disk behavior, bytes transferred, and failure recovery. A faster run with one cached dataset may not represent repeated production jobs.

**Exercise:** Compute the same aggregate with NumPy and Dask at several sizes. Explain the point at which scheduling helps or hurts. See [Extra tools](Extra.md) for Spark and streaming choices.

[Back to AI Essentials Hub](README.md)

## Sources

[^8]: Dask developers. [Scheduling](https://docs.dask.org/en/stable/scheduling.html). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
