# Experiment Tracking and Model Management

Track enough information to explain why one experiment differs from another and to reproduce the selected result. A chart without a dataset revision or split definition is incomplete evidence.

## What to record

| Record | Examples |
|---|---|
| Identity | Run ID, code commit, author, timestamp |
| Data | Dataset version, license, split, exclusions |
| Configuration | Hyperparameters, prompt, seed, model snapshot |
| Environment | Python/R version, dependency lock, device, precision |
| Results | Metrics, sample counts, uncertainty, failure examples |
| Artifacts | Model, processor, schema, evaluation report |
| Operations | Training duration, latency, memory, billed cost |

For generative systems, add retrieved evidence, tool versions, and execution traces where permitted. Redact private content before it reaches a tracking service.

## Tool choices

MLflow combines ML lifecycle tooling with support for generative AI tracing, evaluation, and prompt-related workflows. Its registry supports model versions, aliases, and tags. Use current registry workflows rather than designing a new process around old stage-transition examples.[^33][^34]

Weights & Biases provides a registry for versioned model and dataset artifacts. The original hub's “Model Registry: No” entry was incorrect for the current product. Evaluate deployment options, access controls, export, and plan requirements in your environment.[^35]

| Need | Selection criterion |
|---|---|
| Training comparison | Parameters, curves, artifacts and dataset lineage |
| Registry | Versioned artifacts, ownership and controlled promotion |
| Agent debugging | Linked traces of model, retrieval and tool calls |
| Sensitive projects | Hosting, redaction and retention controls |
| Long-term portability | Exportable metrics and artifact references |

## Minimal local experiment record

This standard-library example writes a record into the current directory. Values are fictional and demonstrate a schema, not a benchmark result.

```python
import json
from pathlib import Path

record = {
    "run_id": "example-001",
    "status": "illustrative",
    "dataset_revision": "synthetic-demo-v1",
    "split": "fixed-example-only",
    "config": {"model": "baseline", "seed": 42},
    "metrics": {"example_accuracy": 0.8},
    "notes": "Replace all example values with measured results.",
}
Path("experiment.json").write_text(json.dumps(record, indent=2), encoding="utf-8")
```

Once experiments outgrow this record, adopt a tracking system without changing the principle: evidence must remain tied to the exact configuration that produced it.

## Promotion and reproducibility

Keep experimentation separate from release promotion. Select a candidate based on an agreed evaluation, record the decision, and promote a specific immutable version. A mutable alias such as `champion` is a pointer; log the concrete version it resolved to for every deployment.

Record failures as well as successes. A sequence of selectively reported runs can hide instability. Repeated stochastic generations should retain trial count and variability rather than only the best output.

**Exercise:** Reproduce an experiment from another person's record. Add the missing fields and explain whether the remaining variation is caused by data, environment, or model behavior.

[Back to AI Essentials Hub](README.md)

## Sources

[^33]: MLflow. [MLflow for Agents and LLMs](https://mlflow.org/docs/latest/genai/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^34]: MLflow. [Model Registry Workflows](https://mlflow.org/docs/latest/ml/model-registry/workflow/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^35]: Weights & Biases. [Registry overview](https://docs.wandb.ai/models/registry). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
