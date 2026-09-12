# AI Deployment Essentials

Deployment delivers a versioned model or workflow to its intended users. A successful response must be correct enough, timely, authorized, observable, and recoverable.

## Choose the serving shape

| Pattern | Fits | Typical concern |
|---|---|---|
| Batch job | Scheduled scoring, bulk extraction | Partial completion and reproducibility |
| API service | Interactive prediction or generation | Timeouts, concurrency and overload |
| Background queue | Long-running work | Idempotency, cancellation and retries |
| On-device inference | Offline or low-latency tasks | Memory, energy, update delivery |
| Managed endpoint | Teams outsourcing infrastructure | Quotas, region and provider dependencies |

For a small model, FastAPI or Flask can expose an application interface. Docker packages the application environment. Kubernetes is an option when workload scale and operational requirements justify a cluster; it is not a prerequisite for learning deployment.

FastAPI's deployment guide covers HTTPS, restarts, replication, and memory. Each worker process may load its own copy of a model, so increasing workers can exhaust RAM or accelerator memory.[^36]

## Generative-model serving

Ollama supports local model experimentation, and also supports cloud models. Selecting a cloud-backed model changes where inference runs.[^26] vLLM provides serving features including continuous batching, prefix caching, and quantization support. Model architecture, hardware, and feature compatibility must still be checked.[^27]

Separate application workers from a shared inference service when that architecture improves memory use and scaling. “OpenAI-compatible” describes an interface surface; test tool calling, streaming, structured output, token accounting, and error behavior explicitly.

## Release contract

Record code version, dependencies, model identifier and revision, preprocessing, prompt version, retrieval snapshot, schema, and evaluation set. A release may change without new weights: a prompt edit or indexing change can alter behavior.

Define input size limits, supported languages, timeouts, failure responses, and fallback behavior. Validate outputs before using them in business logic. Load model artifacts only from trusted, verified sources; serialized Python objects can carry executable behavior.

## Rollout sequence

1. Reproduce the approved evaluation in a clean environment.
2. Test the service interface, including malformed and oversized inputs.
3. Load-test at realistic concurrency and input/output lengths.
4. Deploy to staging with production-like configuration.
5. Release to a limited audience or small traffic share and inspect outcomes.
6. Expand only if predefined quality and reliability criteria hold.
7. Keep a tested rollback path for code, model, prompts, and index state.

These are recommended engineering gates. Their thresholds should reflect the actual application, not universal numbers.

## Failure handling

Use bounded retries with backoff for transient errors. Make state-changing requests idempotent so a retry does not create duplicate transactions. Distinguish an unavailable service from an uncertain answer. Expose health and readiness checks without disclosing credentials or private input.

Collect latency percentiles, queue depth, error rates, memory, resource utilization, cost, and task-quality samples. Inspect drift and changes in user behavior rather than monitoring uptime alone.

**Exercise:** Simulate a slow response, an unavailable model, and a duplicate request. Document the behavior and prove a previous release can be restored. Continue to [observability](Evaluation_and_Observability.md).

[Back to AI Essentials Hub](README.md)

## Sources

[^26]: Ollama. [Quickstart](https://docs.ollama.com/quickstart). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^27]: vLLM contributors. [vLLM documentation](https://docs.vllm.ai/en/latest/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^36]: FastAPI. [Deployments Concepts](https://fastapi.tiangolo.com/deployment/concepts/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
